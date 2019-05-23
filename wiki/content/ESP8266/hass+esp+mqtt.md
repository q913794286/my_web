---
Title: Hass+esp+mqtt
---
ESP8226  espeasy固件地址：[https://github.com/letscontrolit/ESPEasy/releases](https://github.com/letscontrolit/ESPEasy/releases)
配置教程：[https://bbs.hassbian.com/forum.php?mod=viewthread&tid=651&highlight=门磁](https://bbs.hassbian.com/forum.php?mod=viewthread&tid=651&highlight=门磁)

linux  MQTT的安装


#### 安装MQTT服务
#### 安装3个MAKE所需的工具
#### 编译找不到openssl/ssl.h
    sudo apt-get install libssl-dev
#### 编译过程找不到ares.h
    sudo apt-get install libc-ares-dev
#### 编译过程找不到uuid/uuid.h
    sudo apt-get install uuid-dev
#### 安装
    sudo apt-get install mosquitto
### 开启服务
    sudo systemctl start mosquitto
#### 查看服务状态
sudo systemctl status mosquitto

### hass 若无法安装hbMqtt则手动安装
    pip3 install -i https://pypi.douban.com/simple/ hbmqtt
继电器修改配置文件：    
configuration.yaml     
```
mqtt:
  broker: 127.0.0.1
  port: 1883
#  username: 
#  password: 
  
switch:
#  - platform: broadlink
#    host: 192.168.1.149
#    mac: '78:0F:77:5A:2F:EE'

  - platform: mqtt
    name: "switch"
    state_topic: "/ESP_Easy/switch/Switch01"
    command_topic: "/ESP_Easy/gpio/13"
    payload_on: "0"
    payload_off: "1"
    qos: 1
    retain: true
```