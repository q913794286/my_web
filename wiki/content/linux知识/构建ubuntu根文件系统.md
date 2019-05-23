## 构建ubuntu根文件系统

1. 下载Ubuntu Core

Ubuntu Core是Ubuntu的一个精简版本，只包含Ubuntu根文件系统的核心部分，默认没有图形界面等等。在Ubuntu主机中创建目录并下载Ubuntu 16.04.02的Core根文件系统。

mkdir ~/rootfs && cd ~/rootfs

wget http://cdimage.ubuntu.com/ubuntu-base/releases/16.04/release/ubuntu-base-16.04.2-base-arm64.tar.gz

2. 解压Ubuntu Core

sudo tar –xpf ubuntu-base-16.04.2-base-arm64.tar.gz

3. 安装qemu

安装qemu用于在主机端模拟执行基于arm架构的代码，把模拟器拷贝到Core根文件系统目录中。如果是32位armhf根文件系统版本，拷贝qemu-arm-static，此处是64位arm64版本，拷贝qemu-aarch64-static。

sudo apt-get install qemu-user-static

sudo cp -a /usr/bin/qemu-aarch64-static usr/bin/

4. 安装linux内核模块

编译linux内核，在内核源码目录output/lib/中拷贝modules目录中的所有内容到Core根文件系统/lib目录。

5. 切换根文件系统

切换到Core根文件系统，进行更新设置。

sudo chroot ../rootfs

此时处于arm模拟器Core根文件系统中。

6. 设置root密码

passwd root

#### 7. 添加ubuntu管理员账号

可以添加一个ubuntu的管理员账号并修改密码。

useradd –G sudo –m –s /bin/bash ubuntu

passwd ubuntu

8. 设置主机名

可以为目标板设置一个主机名。

echo Ubuntu > /etc/hostname

echo "127.0.0.1   localhost.localdomain localhost" > /etc/hosts

echo "127.0.0.1    Ubuntu" >> /etc/hosts

9. 设置DNS解析器配置文件

echo "nameserver 114.114.114.114" > /etc/resolv.conf

echo "nameserver 80.80.81.81" > /etc/resolv.conf

10. 设置串口登录

ln –s /lib/systemd/system/serial-getty\@.service/etc/systemd/system/getty.target.wants/serial-getty@ttyS0.service

Ubuntu 16.04.02采用了systemd的init初始化系统，用于提高系统的启动速度。在执行getty.targe时，systemd会自动在/etc/systemd/system/getty.target.wants查找相关的targe执行，即实际执行/lib/systemd/system/serial-getty@.service这个串口终端服务。

11. 从服务器获取最新的包列表

apt-get update

12. 安装网络工具包

apt-get install ifupdown net-tools

13. 安装udev设备管理器

apt-get install udev

#### 15. 安装其他的软件包

可以用apt-get安装其他适用于目标板的软件包，如vim，ssh,sudo,nano等等。

16. 安装initramfs-tools工具

apt-get install initramfs-tools

17. 生成ramdisk归档文件

mkinitramfs -o /boot/initrd.img 3.10.65

其中3.10.65为内核版本，可以把生成的initrd.img拷贝到linux源码目录，重命名并替换掉rootfs.cpio.gz，重新编译linux内核，并生成新boot.img。

可以通过以下命令解压img文档，从而实现修改。

mv initrd.img initrd.img.gz

gunzip initrd.img.gz

cpio -idmv < initrd.img

18. 退出构建Core根文件系统

设置好Core根文件系统后，用exit命令退出chroot。

19. 打包做好的Core根文件系统

sudo tar –czvf ../ubuntu.tar.gz .

在根文件系统上一目录生成ubuntu.tar.gz的文档。

20. Core根文件系统更新到sd卡

插入sd卡，清空rootfs目录，把ubuntu.tar.gz拷贝到sd卡rootfs目录，并解压。

tar –xzvf ubuntu.tar.gz

21. ubutun启动
#### 22. 附录

本章所述构建好的Ubuntu Core根文件系统。

<http://pan.baidu.com/s/1i4Hfrvv>

https://blog.csdn.net/huang20083200056/article/details/77429567