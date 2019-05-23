---
Title: 树莓派wifi
---
### 香蕉派M1+：
wif默认关闭，配置文件开启：

打开路径：

     nano /etc/modules   

写入： 
       
     ap6210

重启：
     
     service networking reset

### wifi链接配置：

打开路劲：


    nano /etc/network/interfaces
写入
```
auto wlan0
allow-hotplug wlan0
iface wlan0 inet dhcp
   wpa-ssid whkj
   wpa-psk whkj-yfb
   wpa-key-mgmt WPA-PSK
```
重启：

    service networking reset

查看wifi ESSID：
  
   iwlist wlan0 scan | grep ESSID

查看wifi是否开启成功：

    iwconfig

查看系统加载模块：

    lsmod
 