---
Title: 编译自己的openwrt
---
### 编译自己的openwrt

前置条件

编译环境：Linuxmint 18.2 64-Bit

路由器型号：TP-LINK TL-WR720N v3， 改过硬件：16M Flash / 64M Memory
编译过程中会频繁下载东西，需要有网络良好的环境。整个编译过程可能会持续1到2个小时。
注意事项

请不要用root用户编译！
编译命令请在OpenWrt编译环境的根目录下运行，例如： ~/openwrt
编译环境所在的每一级目录的目录名称都不要有空格，
OpenWrt的目录所有者要改成root以外的用户。 (sudo chown -R user:username /openwrt/)
编译原因

官方有原版的WR720N固件：https://downloads.openwrt.org/

优点是方便，缺点是不能自己定制，因为我是改过 Flash 大小的，必须自己编译。

最主要的我想在路由器里运行ngrok，WR720N的CPU AR9331属于MIPS架构，GO直到GO1.6才支持MIPS64，GO1.8才支持MIPS。在此之前也有大神用C语言重写ngrok，具体可以看大神在github上的项目。虽然说GO1.8之后的版本开始支持MIPS架构，但是需要系统内核开启FPU(Float Point 语言Unit，浮点运算单元)才能使用。这次编译最主要的就是开启内核的FPU功能。

### 开始编译


以下是我对改过硬件的 TL-WR720N 编译适合的固件的步骤：

##### 1.获取最新源码

如何获取源码可查看：[https://dev.openwrt.org/wiki/GetSource](https://dev.openwrt.org/wiki/GetSource)
```
git clone git://git.openwrt.org/openwrt.git
```
也可以获取特定分支的源码，例如我们获取 15.05 branch (Chaos Calmer) 这个分支 。以下步骤都使用的这份源码
```
git clone git://git.openwrt.org/15.05/openwrt.git
```
#### 2.修改源码，使其适用于 16M Flash。

参考的这篇文章： [ http://wiki.openwrt.org/toh/tp-link/tl-wr703n#mb_flash_mod](http://wiki.openwrt.org/toh/tp-link/tl-wr703n#mb_flash_mod)

进入源码目录 
```
cd openwrt
```
打开 target/linux/ar71xx/image/Makefile 这个文件。将如下内容
```
define Device/tl-wr720n-v3
    $(Device/tplink-4mlzma)
    BOARDNAME := TL-WR703N
    DEVICE_PROFILE := TLWR720
    TPLINK_HWID := 0x07030101
    CONSOLE := ttyATH0,115200
endef
```
改为：
```
define Device/tl-wr720n-v3
    $(Device/tplink-16mlzma)
    BOARDNAME := TL-WR703N
    DEVICE_PROFILE := TLWR720
    TPLINK_HWID := 0x07030101
    CONSOLE := ttyATH0,115200
endef
```
打开 tools/firmware-utils/src/mktplinkfw.c 这个文件。将如下内容：
```
.id             = "TL-WR720Nv3",
.hw_id          = HWID_TL_WR720N_V3,
.hw_rev         = 1,
.layout_id      = "4Mlzma",
```
改为：
```
.id             = "TL-WR720Nv3",
.hw_id          = HWID_TL_WR720N_V3,
.hw_rev         = 1,
.layout_id      = "16Mlzma",
```
3.下载和安装所有可用的 Feeds（可选）

feeds其实就是外围的软件源，如luci就是其中之一，不是OpenWrt核心开发组的。还是执行这一步比较好。feeds的说明参考这篇文章：http://wiki.openwrt.org/doc/devel/feeds。
```
./scripts/feeds update -a
./scripts/feeds install -a
```
4.检查依赖软件

运行下面的命令（3选1!），让OpenWrt编译系统检查编译环境中缺失的软件包:
```
make menuconfig         # 总体配置，推荐使用此命令
make defconfig                  # 默认配置
make prereq                         #
```
#### 如果以上3个命令都运行了,编译会出错!
根据提示安装对应的软件。如果以前没装过build-essential这个包的话，这次也要安装这个包。

5.总体配置
```
make menuconfig
```
输入以上命令之后，会进入一个命令行图形界面，界面最上面是用法。

<*> 该代码将被放入固件中 (on the SqashFS partition)
< > 该代码将不会被编译
<M> 该代码将被交叉编译，生成的ipk软件包将被放在 /buildsystem/bla/bla/bla, 但该软件包不会放入固件中
以下是我的一些配置。

选择 CPU 型号
Target System 选为 Atheros ARM7xxx/ARM9xxx

选择路由器型号
Target Profile 选为 TP-LINK TP-WR720N

启用 Web 管理界面 LuCI
```
LuCI → Collections → 选中 luci
```
添加 LuCI 中文支持
```
LuCI → Modules → Translations → 选中 Chinese (zh-cn)
```
添加支持USB自动mount功能
添加USB相关支持
```
Kernel modules —> USB Support —> <*> kmod-usb-core.
Kernel modules —> USB Support —> <*> kmod-usb-ohci.
Kernel modules —> USB Support —> <*> kmod-usb-uhci.
Kernel modules —> USB Support —> <*> kmod-usb-storage.
Kernel modules —> USB Support —> <*> kmod-usb-storage-extras.
Kernel modules —> USB Support —> <*> kmod-usb2.
```
添加USB挂载
```
Base system —> <*>block-mount
```
添加自动挂载工具
```
Utilities —> Filesystem —> <*> badblocks
```
添加文件系统支持
```
Kernel modules —> Filesystems —> <*> kmod-fs-ext4 (移动硬盘EXT4格式选择)
Kernel modules —> Filesystems —> <*> kmod-fs-vfat(FAT16 / FAT32 格式 选择)
Kernel modules —> Filesystems —> <*>kmod-NTFS(NTFS选择，具体是什么忘记了，总之有NTFS字样的)
添加UTF8编码,CP437编码，ISO8859-1编码
Kernel modules —> Native Language Support —> <> kmod-nls-utf8
Utilities ---> disc ---> <> fdisk.................................... manipulate disk partition table
Utilities ---> <*> usbutils................................... USB devices listing utilities
```
6.内核配置
```
make kernel_menuconfig
```
进入文本行图形界面后，开启FPU。
```
Kernel type → 选中 MIPS FPU Emulator
```
7.编译
```
make
```
更多编译选项参考文章最后面的链接。整个编译过程会比较久，这段时间可以去做点别的事情，比如我打完make命令之后就去洗澡睡觉了。囧......

编译好了之后可在 bin/ar71xx 目录找到生成的固件：
```
openwrt-ar71xx-generic-tl-wr703n-v1-squashfs-factory.bin
openwrt-ar71xx-generic-tl-wr703n-v1-squashfs-sysupgrade.bin
```
刷机

（待定，这一步我没有做。我买路由器的时候淘宝商家已经刷了明月永在的固件和不死uboot，我是直接刷openwrt-ar71xx-generic-tl-wr703n-v1-squashfs-sysupgrade.bin的。而且也没有出现过刷死的情况。）

我所使用的 U-Boot 是这个：https://github.com/pepe2k/u-boot_mod。
步骤如下：

将电脑 IP 设置为 192.168.1.2, 子网掩码设置为 255.255.255.0；
断开路由器的电源，用网线连接好电脑和路由器；
按住路由器的 Reset 按钮，插上电源，待路由器灯闪四次后放开，放开后，路由器灯会快闪一次，代表进入了恢复模式；
用电脑浏览器打开 http://192.168.1.1/index.html, 打开后，上传固件；
上传完固件后，等一段时间，待机器自动重启此时可将电脑 IP 设置为自动获取。
配置系统

这里可以参考我的另外一篇笔记。

错误（后续）处理

关于自己编译的系统不能安装官方软件源的软件。
安装不了的软件大多是跟内核相关的，一般都会带有kmod字样，如kmod-fs-ext4。这是因为官方软件源的软件编译时用的内核和我们自己的不一样，就算opwnwrt正式版的已经发布，但是github上的代码还是会更新，包括内核版本。比如官方的Chaos Calmer内核是3.18.21，我从github上下的内核时3.18.45。这时候在安装命令之后加上 --force-depends即可
```
opkg install kmod-fs-ext4 --force-depends
```
路由器指示灯一直在闪烁
这个是因为在编译固件时没有选择led灯的配置项，系统就采用默认值。可以用以下的命令设置LED灯。以下命令最好加入到开机启动项中。

LED灯长亮：
```
echo "1" > /sys/devices/platform/leds-gpio/leds/tp-link:blue:system/brightness
LED灯熄灭：

echo "0" > /sys/devices/platform/leds-gpio/leds/tp-link:blue:system/brightness
LED灯闪烁：

cd /sys/devices/platform/leds-gpio/leds/tp-link:blue:system/
echo timer > trigger
echo 100 > delay_on
echo 100 > delay_off
```
注意：延时时间（delay_on）要小于255。

参考文章：

编译自己的 OpenWrt 固件
OpenWrt编译系统 – 安装
OpenWrt Buildroot – 使用说明
Ubuntu下交叉编译kcptun go语言源码 for openwrt
System configuration
