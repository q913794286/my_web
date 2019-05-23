**OpenWRT开发自定义应用方法**

2014年08月16日 21:11:54

阅读数：9556

OpenWRT编译成功完成后，所有的产品都会放在编译根目录下的*bin/{TARGET}/，*例如*:*我所编译的产物都放在*./bin/ar74xx/*下，其中有一个

*packages*文件夹*:*

里面包含了我们在配置文件里设定的所有编译好的软件包。默认情况下，会有默认选择的软件包。需要主要的是，编译完成后，一定要将编译好的*bin*目录进行备份。

在编译根目录下会有一个*dl*的目录，这个目录其实是“*download”*的简写，在编译前期，需要从网络下载的数据包都会放在这个目录下，这些软件包的一个特点就是，会自动安装在所编译的固件中，也就是我们*make
menuconfig*的时候，为固件配置的一些软件包。如果我们需要更改这些源码包，只需要将更改好的源码包打包成相同的名字放在这个目录下，然后开始编译即可。编译时，会将软件包解压到*build_dir*目录下。

当然，你也可以自己在*dl*里面创建自己的软件包，然后更改相关的配置文件，让*openwrt*可以识别这个文件包。

新建和编译自己的*packages：*

对于自己新建的*package*，而这个*package*又不需要随固件一起安装，换句话说，就是可以当做一个可选软件包的话。我们可以利用我们的*SDK*环境来单独编译，编译后会生成一个*ipk*的文件包。然后利用*opkg
install
xxx.ipk *来安装这个软件，在package文件夹里面新建Helloworld，建立源码目录：

cd package

mkdir Helloworld ; cd Helloworld

*mkdir src*

*touch src/Makefile   /\* Helloworld 编译Makefile \*/*  


touch ./Makefile /\*建立顶层Makefile，这个*Makefile*文件是*OpenWRT*读的\*/

Helloworld.c程序：

1.  */\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\**

2.  *\* Helloworld.c*

3.  *\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/*

4.  \#include \<stdio.h\>

5.  \#include \<unistd.h\>

6.  int main(void)

7.  {

8.  printf("Hello world\\n");

9.  return 0;

10. }

编辑Helloworld的编译Makefile:

1.  \# build helloworld executable when user executes "make"

2.  helloworld: helloworld.o

3.  \$(CC) \$(LDFLAGS) helloworld.o -o helloworld

4.  helloworld.o: helloworld.c

5.  \$(CC) \$(CFLAGS) -c helloworld.c

6.  \# remove object files and executable when user executes "make clean"

7.  clean:

8.  rm \*.o helloworld

编写模块编译Makefile:

1.  Makefile文件模板内容如下：

2.  *\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#*

3.  *\# OpenWrt Makefile for helloworld program*

4.  *\#*

5.  *\#*

6.  *\# Most of the variables used here are defined in*

7.  *\# the include directives below. We just need to*

8.  *\# specify a basic description of the package,*

9.  *\# where to build our program, where to find*

10. *\# the source files, and where to install the*

11. *\# compiled program on the router.*

12. *\#*

13. *\# Be very careful of spacing in this file.*

14. *\# Indents should be tabs, not spaces, and*

15. *\# there should be no trailing whitespace in*

16. *\# lines that are not commented.*

17. *\#*

18. *\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#*

19. include \$(TOPDIR)/rules.mk

20. *\# Name and release number of this package*

21. PKG_NAME:=helloworld

22. PKG_RELEASE:=1

23. *\# This specifies the directory where we're going to build the program.*

24. *\# The root build directory, \$(BUILD_DIR), is by default the build_mipsel*

25. *\# directory in your OpenWrt SDK directory*

26. PKG_BUILD_DIR := \$(BUILD_DIR)/\$(PKG_NAME)

27. include \$(INCLUDE_DIR)/package.mk

28. *\# Specify package information for this program.*

29. *\# The variables defined here should be self explanatory.*

30. *\# If you are running Kamikaze, delete the DESCRIPTION*

31. *\# variable below and uncomment the Kamikaze define*

32. *\# directive for the description below*

33. define Package/helloworld

34. SECTION:=utils

35. CATEGORY:=Utilities

36. TITLE:=Helloworld -- prints a snarky message

37. endef

38. *\# Uncomment portion below for Kamikaze and delete DESCRIPTION variable
    above*

39. define Package/helloworld/description

40. If you can't figure out what this program does, you're probably

41. brain-dead and need immediate medical attention.

42. endef

43. *\# Specify what needs to be done to prepare for building the package.*

44. *\# In our case, we need to copy the source files to the build directory.*

45. *\# This is NOT the default. The default uses the PKG_SOURCE_URL and the*

46. *\# PKG_SOURCE which is not defined here to download the source from the
    web.*

47. *\# In order to just build a simple program that we have just written, it
    is*

48. *\# much easier to do it this way.*

49. define Build/Prepare

50. mkdir -p \$(PKG_BUILD_DIR)

51. \$(CP) ./src/\* \$(PKG_BUILD_DIR)/

52. endef

53. *\# We do not need to define Build/Configure or Build/Compile directives*

54. *\# The defaults are appropriate for compiling a simple program such as this
    one*

55. *\# Specify where and how to install the program. Since we only have one
    file,*

56. *\# the helloworld executable, install it by copying it to the /bin
    directory on*

57. *\# the router. The \$(1) variable represents the root directory on the
    router running*

58. *\# OpenWrt. The \$(INSTALL_DIR) variable contains a command to prepare the
    install*

59. *\# directory if it does not already exist. Likewise \$(INSTALL_BIN)
    contains the*

60. *\# command to copy the binary file from its current location (in our case
    the build*

61. *\# directory) to the install directory.*

62. define Package/helloworld/install

63. \$(INSTALL_DIR) \$(1)/bin

64. \$(INSTALL_BIN) \$(PKG_BUILD_DIR)/helloworld \$(1)/bin/

65. endef

66. *\# This line executes the necessary commands to compile our program.*

67. *\# The above define directives specify all the information needed, but
    this*

68. *\# line calls BuildPackage which in turn actually uses this information to*

69. *\# build a package.*

70. \$(eval \$(call BuildPackage,helloworld))

返回目录最顶层，执行make
menuconfig，在utils中选中刚加的模块名（这里是根据sections中定义的），保存.config。

执行make.

如果要单独编译模块：

makepackage/helloworld/compile

make package/helloworld/install

编译结果会放在*bin/{TARGET}/package*目录下:*helloworld\*.ipk*

*利用tftp将ipk文件上传到板子里面，用opkg install
命令加载模块，加载成功后执行helloworld，这时就是执行之前编写的helloworld程序*
