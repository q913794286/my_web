---
Title: 1.Openwrt 编译前需做的事情
Description: .Openwrt 编译前需做的事情
Sort: 1
---
编译前：

[MT7688——添加WiFi驱动及IP设置](https://www.jianshu.com/p/0802394405b6)

1. make menuconfig

　　　这次openwrt升级后， 编译完刷上固件，openwrt会自动进入failsafe模式。怎么编译都不行。 后来发现， 新的固件里面选择了 Kernel Modules\Other modules\kmod-button-hotplug 模组。这个模组在启动的时候，触发了"f"或者“enter”按键，所以在启动的过程中就自动进入了failsafe模式。取消选择后，一切正常。

2. 不要整个LUCI， 只是要Luci rpc模块的话， Luci\Collection\Luci不要选择。 则整个web界面都不会被安装。只有rpc的功能会存在。

2. Openwrt默认不开启wifi，要开启的话， 修改这个文件： openwrt/trunk/package/kernel/mac80211/files/lib/wifi/mac80211.sh. 滚到文件最后， 注释掉 

 REMOVE THIS LINE TO ENABLE WIFI:
option disabled 1

 

3. openwrt默认开机启动ssh的方法

http://www.2cto.com/os/201304/204073.html

　　2.1 Openwrt下的路径：package/network/services/dropbear/files/dropbear.config
```
[openwrt@root files]$ vim dropbear.config 
　　 1config dropbear 
　　 2 option PasswordAuth 'on' 
　　 3 option RootPasswordAuth 'on' 
　　 4 option Port '22' 
　　 5 option Interface 'lan' 
　　 6# option BannerFile '/etc/banner' 
```

3）默认中文，修改主机名，添加并修改默认主题，设定时区

默认中文，添加并默认主题

修改feeds/luci/libs/web/root/etc/config

	option lang auto
改为

	option lang zh_cn
 并添加

    config internal languages
            option en 'English'
            option zh_cn 'chinese'


添加主题

1.首先打开trunk/feeds/luci/themes这个目录，你会发现里面有很多主题（除了base为基础包外）每一个文件夹就

是一个主题

2.我们得修改makefile文件，使其制定编译的时候能找到openwrtcn这个主题

找到路径为trunk/feeds/luci/contrib/package/luci下面的makefile文件双击打开

搜索OpenWrt.org这样很快就定位到添加主题的地方了，在下面空白处增加一句

效果如下


    $(eval $(call theme,base,Common base for all themes))
    $(eval $(call theme,openwrt,OpenWrt.org ))
    $(eval $(call theme,bootstrap,Bootstrap Theme))
    $(eval $(call theme,openwrtcn,openwrtcn Theme (default),,,1))
      
    $(eval $(call theme,freifunk-bno,Freifunk Berlin Nordost Theme,\
        Stefan Pirwitz ))
      
    $(eval $(call theme,freifunk-generic,Freifunk Generic Theme,\
        Manuel Munz ))
保存退出即可。





修改默认主题

修改feeds/luci/libs/web/root/etc/config

option mediaurlbase /luci-static/openwrt.org
可根据需要将openwrt.org修改为Bootstap、openwrtcn、freifunk-bno、freifunk-generic



修改主机名，设定时区
修改package/base-files/files/etc/config/system

    config system
    option conloglevel 8
    option cronloglevel 8
    option hostname Openwrt
    option timezone Asia/Shanghai
    option timezone CST-8
    config timeserver ntp
    list server 0.openwrt.pool.ntp.org
    list server 1.openwrt.pool.ntp.org
    list server 2.openwrt.pool.ntp.org
    list server 3.openwrt.pool.ntp.org
    option enable_server 0
    option hostname Openwrt 设定主机名
    option timezone Asia/Shanghai 时区设置为亚洲/上海
    option timezone CST-8 正8区
    list server 就是ntp服务器了。