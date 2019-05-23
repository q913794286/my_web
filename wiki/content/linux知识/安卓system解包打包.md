---
Title: 安卓system解包打包
---
### 不具有压缩的ext4打包：
```
mkdir system 

sudo mount -o loop system.img system 

cp -rf xxx.xml system/etc
```
执行cp时候，提示设备上没有空间，这时候，可以
```
sudo apt-get install e2fsprogs

e2fsck -f system.img
resize2fs -p system.img 1000M
sudo mount -o loop system.img system

cp xxx 

sudo  umount system
resize2fs -M system.img
```
### 具有压缩打包的方式：
一、准备工作：
解压解打包工具，得到三个文件：make_ext4fs、mkuserimg.sh、simg2img，
把它们跟要修改的 .img.ext4(或.img)文件放置到同一个目录下

二、转换源文件为img格式( .img则略过)
使用./simg2img src des命令来转换system.img.ext4格式文件
终端输入：


./simg2img system.img system.img.ext4
 


等待一会就可以转换完毕


三、挂载镜像
新建一个目录，挂载此img到这个目录上使用
终端输入：

sudo mkdir sysmain

新建出一个名为sysmain的目录
继续输入：

mount -t ext4 -o loop system.img.ext4 sysmain
 


挂载成功后就可以在资源管理器中管理该img内的文件

四、修改镜像内容

五、重新打包
完成修改后就要打包，此时用到另外一个命令
首先在看看镜像挂载到目录后镜像分区的大小，例如是 512M
终端输入

chmod 777 ./mkuserimg.sh
./mkuserimg.sh -s sysmain systest.img.ext4 ext4 tmp 512M
 


随后就重新打包好了
#注意
下划线部分必须对应，如果是非M单位要转换成M

如果是打包成.img
终端输入

chmod 777 ./make_ext4fs
./make_ext4fs -l 512M -s -a system system_out.img ./system
 


#注意：
-l 512M"是分区大小,i9100的system分区是512M;
"-a system"，是指这个img用于Android，挂载点是/system
使用此参数后会自动根据private/android_filesystem_config.h里定义的权限给镜像中所有文件重新设置权限
如果刷机后发现有文件权限不对，可以修改android_filesystem_config.h添加权限重新编译make_ext4fs
也可以直接不使用 “-a system”参数，保持镜像中文件的默认权限。