---
Title: nmcli 命令行方式连接wifi及删除连接操作
---
    在linux下除了图形方式连接wifi，还可以使用命令行方式连接，这种方式方便没有图形界面的linux、无显示器、裁剪系统等嵌入式设备。

#### 获取nmcli方法
1. 如果在你的设备可以访问互联网的情况下
2. ```sudo apt-get install network-manager```
3. 通过上述命令直接安装```network-manager```，包含了nmcli 命令

4. 如果在你的设备无法访问互联网的情况下，你可以下载一份源码，通过目标板编译或者交叉编译的方式编译出```network-manager```工具，这里不多做描述。

#### nmcli扫描并查看wifi情况
1.使用方法：
```
nmcli d wifi connect password iface
``` 
    
   例如：连接KFC_free
    
   通过ifconfig 获取网卡描述，一般默认wlan0
    
   nmcli d wifi connect "KFC_free" password "12345678" wlan0

   连接成功后通过ifconfig 查看一下是否获得了ip
    

2.如果出现下面的情况

```
Error: Failed to add/activate new connection: (32) Not authorized to control networking. 
```
尝试切换root用户尝试。

3.连接成功后，每次开机默认都会去连接wifi，此时去切换别的wifi会失败，可提前断开连接

```nmcli dev dis wlan0```

或者

`nmcli con del KFC_free`

断开连接后，再连接别的wifi就正常了。

彻底删除wifi连接的方法

`nmcli c`

这个命令可以获取到当前设备所有连接过多的历史连接及对于UUID号码

通过

`nmcli c del 72ffd5f4-71f8-0001-b434-6122908cfd4e`

del 后边是UUID号码