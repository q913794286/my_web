**openwrt的结构 与 编写 HelloWorld程序**

2014年08月04日 10:46:54

阅读数：15091

这次讲讲openwrt的结构.

1. 代码上来看有几个重要目录package, target, build_root, bin, dl....

\---build_dir/host目录是建立工具链时的临时目录

\---build_dir/toolchain-\<arch\>\*是对应硬件的工具链的目录

\---staging_dir/toolchain-\<arch\>\* 则是工具链的安装位置

\---target/linux/\<platform\>目录里面是各个平台(arch)的相关代码

\---target/linux/\<platform\>/config-3.10文件就是配置文件了

\---dl目录是'download'的缩写, 在编译前期，需要从网络下载的数据包都会放在这个目录下，这些软件包的一个特点就是，会自动安装在所编译的固件中，也就是我们make
menuconfig的时候，为固件配置的一些软件包。如果我们需要更改这些源码包，只需要将更改好的源码包打包成相同的名字放在这个目录下，然后开始编译即可。编译时，会将软件包解压到build_dir目录下。

\---而在build_dir/目录下进行解压，编译和打补丁等。

\---package目录里面包含了我们在配置文件里设定的所有编译好的软件包。默认情况下，会有默认选择的软件包。在openwrt中ipk就是一切,
我们可以使用

\$ ./scripts/feeds update来对软件包进行更新.

\$ ./scripts/feeds search nmap 查找软件包'nmap'

 Search results in feed ’packages’:   
 nmap       Network exploration and/or security auditing utility 

\$ ./scripts/feeds install nmap 安装'nmap'这个软件

\$ make package/symlinks  //估计意思是更新软件源之类的

\---bin目录下生成了很多bin文件，根据不同的平台来区分。另外bin/\<platform\>/package目录，里面有很多ipk后缀的文件，都是package目录下的源码在build_dir目录下编译后的生成的结果。

2. 新建自己的packages  
对于自己新建的package，而这个package又不需要随固件一起安装，换句话说，就是可以当做一个可选软件包的话。我们可以利用我们的SDK环境来单独编译，编译后会生成一个ipk的文件包。然后利用
opkg install xxx.ipk 来安装这个软件。

下面具体说下，如何编译一个helloword的软件包。  
（1）首先，编写helloworld程序  
编写helloworld.c  
/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*  
\* Helloworld.c  
\* The most simplistic C program ever written.  
\* An epileptic monkey on crack could write this code.  
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/  
\#include \<stdio.h\>  
\#include \<unistd.h\>  
int main(void)  
{  
     printf("Hell! O' world, why won't my code compile?\\n\\n");  
     return 0;  
}

编写Makefile文件  
\# build helloworld executable when user executes "make"  
helloworld: helloworld.o  
        \$(CC) \$(LDFLAGS) helloworld.o -o helloworld  
helloworld.o: helloworld.c  
        \$(CC) \$(CFLAGS) -c helloworld.c  
\# remove object files and executable when user executes "make clean"  
clean:  
        rm \*.o helloworld  
                                    
在这两个文件的目录下，执行make
应该可以生成helloworld的可执行文件。执行helloworld后，能够打印出“Hell!O' world,
why won't my code compile?”。 这一步，主要保证我们的源程序是可以正常编译的。

下面我们将其移植到OpenWRT上。  
（2）将OpenWrt-SDK-brcm47xx-for-Linux-x86_64-gcc-4.3.3+cs_uClibc-0.9.30.1.tar.bz2解压  
tar –xvf
OpenWrt-SDK-brcm47xx-for-Linux-x86_64-gcc-4.3.3+cs_uClibc-0.9.30.1.tar.bz2  
（3）进入SDK  
cd OpenWrt-SDK-brcm47xx-for-Linux-x86_64-gcc-4.3.3+cs_uClibc-0.9.30.1  
可以看到里面的目录结构跟我们之前source的目录结构基本相同，所需要编译的软件包，需要放置在package目录下  
（4）在package目录下创建helloworld目录  
cd package  
mkdir helloworld  
cd helloworld  
（5）创建src目录，拷贝 helloworld文件  
mkdir src  
cp /home/wrt/test/helloworld.c src  
cp /home/wrt/test/Makefile src  
（6）在helloworld目录下创建Makefile文件  
这个Makefile文件是给OpenWRT读的，而之前写的那个Makefile文件是针对helloworld给编译其读的。两个Makefile不在同一层目录下。

touch Makefile  
vim Makefile

Makefile文件模板内容如下：

1.  \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

2.  \# OpenWrt Makefile for HelloWorld program

3.  \#

4.  \#

5.  \# Most of the variables used here are defined in

6.  \# the include directives below. We just need to

7.  \# specify a basic description of the package,

8.  \# where to build our program, where to find

9.  \# the source files, and where to install the

10. \# compiled program on the router.

11. \#

12. \# Be very careful of spacing in this file.

13. \# Indents should be tabs, not spaces, and

14. \# there should be no trailing whitespace in

15. \# lines that are not commented.

16. \#

17. \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

18. include \$(TOPDIR)/rules.mk

19. \# Name and release number of this package

20. PKG_NAME:=HelloWorld

21. PKG_RELEASE:=1

22. \# This specifies the directory where we're going to build the program.

23. \# The root build directory, \$(BUILD_DIR), is by default the build_mipsel

24. \# directory in your OpenWrt SDK directory

25. PKG_BUILD_DIR := \$(BUILD_DIR)/\$(PKG_NAME)

26. include \$(INCLUDE_DIR)/package.mk

27. \# Specify package information for this program.

28. \# The variables defined here should be self explanatory.

29. \# If you are running Kamikaze, delete the DESCRIPTION

30. \# variable below and uncomment the Kamikaze define

31. \# directive for the description below

32. define Package/HelloWorld

33. SECTION:=utils

34. CATEGORY:=Utilities

35. TITLE:=HelloWorld -- prints a snarky message

36. endef

37. \# Uncomment portion below for Kamikaze and delete DESCRIPTION variable
    above

38. define Package/HelloWorld/description

39. If you can't figure out what this program does, you're probably brain-dead
    and need immediate medical attention.

40. endef

41. \# Specify what needs to be done to prepare for building the package.

42. \# In our case, we need to copy the source files to the build directory.

43. \# This is NOT the default. The default uses the PKG_SOURCE_URL and the

44. \# PKG_SOURCE which is not defined here to download the source from the web.

45. \# In order to just build a simple program that we have just written, it is

46. \# much easier to do it this way.

47. define Build/Prepare

48. mkdir -p \$(PKG_BUILD_DIR)

49. \$(CP) ./src*/\* \$(PKG_BUILD_DIR)/*

50. *endef*

51. *\# We do not need to define Build/Configure or Build/Compile directives*

52. *\# The defaults are appropriate for compiling a simple program such as this
    one*

53. *\# Specify where and how to install the program. Since we only have one
    file,*

54. *\# the HelloWorld executable, install it by copying it to the /bin
    directory on*

55. *\# the router. The \$(1) variable represents the root directory on the
    router running*

56. *\# OpenWrt. The \$(INSTALL_DIR) variable contains a command to prepare the
    install*

57. *\# directory if it does not already exist. Likewise \$(INSTALL_BIN)
    contains the*

58. *\# command to copy the binary file from its current location (in our case
    the build*

59. *\# directory) to the install directory.*

60. *define Package/HelloWorld/install*

61. *\$(INSTALL_DIR) \$(1)/bin*

62. *\$(INSTALL_BIN) \$(PKG_BUILD_DIR)/HelloWorld \$(1)/bin/*

63. *endef*

64. *\# This line executes the necessary commands to compile our program.*

65. *\# The above define directives specify all the information needed, but
    this*

66. *\# line calls BuildPackage which in turn actually uses this information to*

67. *\# build a package.*

68. *\$(eval \$(call BuildPackage,HelloWorld))*

（7）返回到SDK的根目录  
执行make进行编译  
编译过程会在build_dir目录下完成  
编译结果会放在 bin/[yourtarget]/package目录下helloworld_1_bcm47xx.ipk  
（8）上传helloworld_1_bcm47xx.ipk  
上传helloworld_1_bcm47xx.ipk至路由器  
执行\# opkg install helloworld_1_bcm47xx.ipk  
然后输入hello然后按Tab键，发现openwrt中已经有helloworld可执行命令。  
执行 helloworld命令来查看程序的效果。  
Hell! O' world, why won't my code compile?
