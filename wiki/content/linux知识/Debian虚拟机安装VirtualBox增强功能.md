# Debian虚拟机安装VirtualBox增强功能

**Debian虚拟机安装VirtualBox增强功能的具体步骤如下：**

#### 1 前期准备

打开Debian虚拟机并登陆，依次点击VirtualBox的“设备->安装增强功能”。 
这时我们可以在Debian的桌面上看到一个光盘图标，例如我的计算机上图标的名称是： 
VBox_GAs_5.2.6

在VBox_GAs_5.2.6图标上右键选择“挂载卷”，之后我们就可以在“/media/cdrom0”路径下看到VBox_GAs_5.2.6中的内容了。

切换到/media/cdrom0路径下：

```
cd /media/cdrom0
```

#### 2 安装内核头文件（root用户下执行）

**注：如果不执行这一步，直接执行下一步（第3步）可能会出现如下报错：** 
his system is currently not set up to build kernel modules. 
Please install the gcc make perl packages from your distribution. 
Please install the Linux kernel “header” files matching the current kernel 
for adding new hardware support to the system. 
The distribution packages containing the headers are probably: 
报错的原因是没有安装内核头文件，因此，我们首先安装内核头文件。

获取系统内核版本信息：

```
uname -r
```

例如在我的计算机上上述命令的执行结果是：

```
4.9.0-4-amd64
```

下一步命令我们需要使用这个参数。

安装内核头文件，命令：

```
apt-get install build-essential linux-headers-内核版本号
```

例如在我的计算机上需要执行的命令就是：

```
apt-get install build-essential linux-headers-4.9.0-4-amd64
```

#### 3 安装VBox增强功能（root用户下执行）

进入/media/cdrom0路径：

```
cd /media/cdrom0
```

开始安装：

```
sh ./VBoxLinuxAdditions.run
```

#### 4 重启

#### 5 增加共享文件查看权限

`exo-open --launch TerminalEmulator`