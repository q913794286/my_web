---
---
1 可以直接通过工具连接，armbian 系统镜像默认安装了wifi 配置工具
输入命令 sudo nmtui 来配置


2 通过修改/etc/network/interfaces 文件，将其中以下内容注释取消掉
allow-hotplug wlan0
iface wlan0 inet dhcp
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
然后再在 /etc/wpa_supllicant/目录下新建一个 wpa_supplicant.conf 文件输入下面内容
ctrl_interface=DIR=/var/run/wpa_supplicant
update_config=1
network={
ssid="yizhan-SHEBEI"
psk="shebeizhineng"
key_mgmt=WPA-PSK
}
保存后重启下主机