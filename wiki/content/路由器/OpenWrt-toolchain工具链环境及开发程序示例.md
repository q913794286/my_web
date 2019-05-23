---
Title: OpenWrt toolchain工具链环境及开发程序示例
---
### OpenWrt toolchain工具链环境及开发程序示例

#### 一、交叉编译
1. 建立交叉编译环境

编译环境手工也可以建立，不过要涉及到很多开发包的安装和编译，比较繁杂琐碎，因此，我们偷个懒，直接使用OpenWrt源码编译环境中的toolchain工具链。
通常我们对openwrt进行编译之后，在buildroot目录下会有一个名叫staging_dir的目录，针对当前平台的toolchain都在这个目录下，请务必及时做好备份保存。

1.1增加toolchain的目录到PATH目录中
```
PATH=$PATH:/home/appdevp/openWrt/trunk/staging_dir/toolchain-i386_gcc-4.6-linaro_uClibc-0.9.33.2/bin/
```
1.2 增加staging_dir的目录到toolchain PATH
```
export STAGING_DIR=/home/appdevp/openWrt/trunk/staging_dir/
```
2. 编译

2.1 configure
```
./configure–target=i486-openwrt-linux-uclibc
```
2.2 make
```

make CC=i486-openwrt-linux-uclibc-gcc LD=i486-openwrt-linux-uclibc-ld
```
#### 二、编译ipk包
1. 编译SDK
在buildroot目录下make menuconfig，然后选中SDK进行编译
编译完成后，对应生成的SDK会出现类似这样的目录：“openWrt/trunk/bin/x86”，进入SDK后，打印当前工作路径如下：
```
“/openWrt/trunk/bin/x86/OpenWrt-SDK-x86-for-redhat-x86_64-gcc-4.6-linaro_uClibc-0.9.33.2”
```
2. 创建工程
在sdk的package目录下创建我们的工程“helloworld”:
新建目录中包含src目录，这个目录就是我们的源代码所在地，另外一个非常重要的文件Makefile，这个Makefile的组成与GNU的有所不同，有点类似于制作rpm包时的spec文件。到后面会有更详细的介绍。以下是具体文件的位置：
其中helloworld.c的内容如下：

Src目录下的Makefile文件内容如下：
最后是helloworld目录下的Makefile的内容：
```
##############################################

# OpenWrtMakefile for helloworld program

#

#

# Most ofthe variables used here are defined in

# theinclude directives below. We just need to

# specifya basic description of the package,

# whereto build our program, where to find

# thesource files, and where to install the

#compiled program on the router.

#

# Be verycareful of spacing in this file.

# Indentsshould be tabs, not spaces, and

# thereshould be no trailing whitespace in

# linesthat are not commented.

#

##############################################

include$(TOPDIR)/rules.mk

# Nameand release number of this package

PKG_NAME:=helloworld

PKG_RELEASE:=1

# Thisspecifies the directory where we’re going to build the program.

# Theroot build directory, $(BUILD_DIR), is by default the build_mipsel

#directory in your OpenWrt SDK directory

PKG_BUILD_DIR:= $(BUILD_DIR)/$(PKG_NAME)

include$(INCLUDE_DIR)/package.mk

# Specifypackage information for this program.

# Thevariables defined here should be self explanatory.

# If youare running Kamikaze, delete the DESCRIPTION

#variable below and uncomment the Kamikaze define

# directivefor the description below

definePackage/helloworld

SECTION:=utils

CATEGORY:=Utilities

TITLE:=Helloworld– prints a snarky message

endef

# Specifywhat needs to be done to prepare for building the package.

# In ourcase, we need to copy the source files to the build directory.

# This isNOT the default. The default uses thePKG_SOURCE_URL and the

#PKG_SOURCE which is not defined here to download the source from the web.

# Inorder to just build a simple program that we have just written, it is

# mucheasier to do it this way.

defineBuild/Prepare

mkdir -p $(PKG_BUILD_DIR)

$(CP) ./src/* $(PKG_BUILD_DIR)/

endef

# We donot need to define Build/Configure or Build/Compile directives

# Thedefaults are appropriate for compiling a simple program such as this one

# Specifywhere and how to install the program. Since we only have one file,

# thehelloworld executable, install it by copying it to the /bin directory on

# therouter. The $(1) variable represents the root directory on the router running

#OpenWrt. The $(INSTALL_DIR) variable contains a command to prepare the install

#directory if it does not already exist. Likewise $(INSTALL_BIN) contains the

# commandto copy the binary file from its current location (in our case the build

#directory) to the install directory.

definePackage/helloworld/install

$(INSTALL_DIR) $(1)/bin

$(INSTALL_BIN) $(PKG_BUILD_DIR)/helloworld$(1)/bin/

endef

# Thisline executes the necessary commands to compile our program.

# Theabove define directives specify all the information needed, but this

# linecalls BuildPackage which in turn actually uses this information to

# build apackage.

$(eval $(call BuildPackage,helloworld))
```

这个Makefile的语法规则还是参考官网吧：

[http://wiki.openwrt.org/doc/devel/packages](http://wiki.openwrt.org/doc/devel/packages)

3. 编译

将当前目录返回到SDK：执行make进行编译。如果一切顺利，最后的结果会保存在SDK/bin/x86/packages目录下，名称为helloword_1_x86.ipk。

4. 安装最新编译的包

通过scp将该包拷贝到目的机器上，通过opkg包管理工具进行安装：
```
opkg install helloworld_1_x86.ipk
```
一切顺利，包安装成功，在当前路径下执行helloworld查看程序执行结果。
恭喜你，你已经实现在OpenWrt平台中开发程序的目标了！