---
Title: Homeassistant
Description: 家庭助理
Sort: 1
---
---

---

### 通用

Home Assistant 运行在 Python 3.5.3 及以上 环境下，一般来说，符合 Python 运行条件的系统皆可安装 Home Assistant。

---
### 通用安装

pip3 install homeassistant
hass --open-ui
注意
使用此方法应确保所需依赖已安装。
---
### Python 虚拟环境安装

Home Assistant 官方推荐使用 Python 虚拟环境安装 Home Assistant 以避免影响生产环境。

安装Python3虚拟环境：   apt-get install python3-venv

创建虚拟环境： python3 -m venv homeassistant

打开虚拟环境： cd homeassistant

激活虚拟环境： source bin/activate

安装 Home Assistant： python3 -m pip install --upgrade homeassistant

启动 Home Assistant 并打开网页： hass --open-ui

---
### 开发版安装

pip3 install --upgrade git+git://github.com/home-assistant/home-assistant.git@dev
hass --open-ui

---
## 设备接入

* 注： 只要是Wifi的设备，都将通过 xiaomi_miio接入，Zigbee 设备则统一是 xiaomi_aqara。

---
### Zigbee 设备（网关类设备）

本部分教程仅适用于『绿米网关局域网通信协议』 1.x 版本的环境，不保证未来『局域网通信协议』升级后的适用性。

以小米多功能网关（第二代）为代表的网关类设备是大部分『米家』及 『绿米（Aqara）』 Zigbee 设备的联动基础，也是整个米家智能家居系统的核心。除多功能网关外，空调伴侣和 Aqara 监控摄像头都具备网关功能。

要将网关接入 HA，我们需要先打开网关的通信协议，并获取通讯密码。

打开米家 app，连接设置多功能网关，点击进入网关页面，点击右上角「…」，进入「设置」。点击第二行「关于」，狂点空白处，便会跳出「局域网通信协议」以及「网关信息」。进入「局域网通信协议」，打开开关，记录下密码，这就是之后待填的「key」。回到上级页面，进入「网关信息」，记录下 mac 地址，这就是之后待填的「sid 或 mac」。

HA 0.50.0 及之后版本已经添加对米家平台的官方支持，我们只需要直接配置即可。如果之前有复制 custom_componets 文件夹的同学，升级后请删除该文件夹。 打开 configuration.yaml 文件，添加以下配置：

此设置适用于 HA 0.54.0版及之后
```
xiaomi_aqara:
  gateways:
      - mac: MAC 地址 （地址不带 "-" 或 ":" ，行首的「-」必须保留）
        key: 通讯密码
```
如果你有多个网关，则按以下格式设定：
```
xiaomi_aqara:
  gateways:
    - mac: xxxxxxxxxxxx
      key: xxxxxxxxxxxxxxxx
    - mac: xxxxxxxxxxxx
      key: xxxxxxxxxxxxxxxx
```
重启后，HA 主界面将会自动识别网关及捆绑的所有设备。

---

### Yeelight 灯具

Yeelight 目前已经从小米独立出来，运作良好，原生支持 Google Assistant 等平台，应该说是小米生态链下『走出去』的先锋。Yeelight 在 HA 中支持色温、色彩、亮度等控制，基本实现全品支持。

配置前请先在 Yeelight App 中打开『局域网控制 (LAN Control)』，服务器选择中国大陆、新加坡皆可，注意如果所选服务器与米家 App 中不同，则设备无法接入米家 App，但不影响在 Yeelight App 及 HA 中的控制。（Yeelight App 3.0 之前版本中，『局域网控制』为『极客模式』）

之后重启 HA，重启前请打开灯具，稍等几分钟，确认一下设备是否已经被 HA 自动识别添加。如果是的话，皆大欢喜，什么都无需再做了；如果没有，请打开configuration.yaml文件，在合适位置添加如下字段：
```
light:
  - platform: yeelight
    devices:
      192.168.1.25:              #Yeelight灯具ip
        name: Living Room      #昵称
        transition: 1000
        use_music_mode: True #音乐随动模式，默认关闭
```

---

### WiFi 设备& Token 获取

所有的小米 WiFi 设备都必须先取得设备的 token 方可接入 HA，以下简要介绍获取 token 的方法之一，更多方法请参考 Home Assistant 中文文档 。

首先在安装有 Node.js 的电脑上安装 miio 库

    sudo npm install miio
之后，重置待连小米设备的网络，使其产生 WIFI 热点，将电脑连接至该热点上，之后输入：

    miio --discover
    
即获取设备的 token，请集中保存。获取 token 后，如需绑定米家 App，请直接在 App 中添加设备，切勿继续重置设备，否则 token 将重新生成，原有 token 将失效。

---

### 米家扫地机器人

HA 原生支持米家和石头扫地机器人(2nd)，使用专门的类—— vacuum。

按照上方教程获取扫地机的 token，之后在 configuration.yaml 中填入以下配置：

```
vacuum:
   - platform: xiaomi_miio
      name: '***'                    #昵称
      host: 192.168.1.2            #ip
      token:     #token

```
---

### 空气净化器

HA 目前原生支持 2 代空净，暂不支持 Pro 版。

按照上方教程获取空气净化器的 token，之后在 configuration.yaml 中填入以下配置：

```
fan:
  - platform: xiaomi_miio
    name: Xiaomi Air Purifier 2
    host: 192.168.130.66
    token: YOUR_TOKEN
```

---

### 空调伴侣

米家和 Aqara 均发布了自己品牌的空调伴侣，除外观差异外，内核并无不同。小米已于近日固件更新中开放空调伴侣的『局域网通信协议』，空调控制和网关功能请分别使用各自插件接入。另，目前插件仅支持部分空调品牌的部分预设模式，详见插件说明页，未支持的型号请自行抓包空调码。

Home Assistant 中空调伴侣以自定义组件的方式加入，此组件为爱好者 Mac_zhou 制作，欢迎大家前往 项目地址 点赞。

插件使用前需获取设备的 token：进入『米家』应用，点击空调伴侣，选择右上角『•••』—— 『关于』—— 狂点空白区域 —— 网关信息 “token=xxxxxxx”即是 。

将 Github 中的对应文件放入文件夹，层级与 Github 中展示一致，之后在配置文件中增加以下配置：

```
climate:
    - platform: mi_acpartner
      name: mi_acpartner
      host: 10.0.0.234 #ip地址
      token: ****** #token
      target_sensor: sensor.temperature_158d00015aefc4 #温度传感器 ID
      target_temp: 26 #目标温度
      
```

组件由国人制作， README 文件以中文书写，这里我就不再作变量说明了。

---

### WiFi 插座及智能插线板

0.56.0 的更新带来了插座的支持，接入前先获取设备的 token，具体方法见前文。之后，在 configuraiton.yaml 添加如下设置：
```
switch:
  - platform: xiaomi_miio
    name: Original Xiaomi Mi Smart WiFi Socket
    host: 192.168.130.59
    token: YOUR_TOKEN
```
这英文简单得不能再简单了，我就不做变量说明了。

---


### 净水器

小米净水器插件由 bit3725 制作，欢迎前往 项目 点赞~

使用方法：从 Github 下载 mi_water_purifier.py，放入 custom_components/sensor/ 文件夹内（文件层级项目本身已经很清晰地给出了），在 configuration.yaml 添加如下设置：

```
sensor:
  - platform: mi_water_purifier
    host: YOUR_SENSOR_IP
    token: YOUR_SENSOR_TOKEN
    name: YOUT_SENSOR_NAME
```
接入后自动会生成几项相关的传感器数值，如果需要集中查看，请使用群组：

```
group:
  - xiaomi_water_purifier:
    name: Xiaomi Water Purifier
    icon: mdi:water
    entities:
      - sensor.tap_water
      - sensor.filtered_water
      - sensor.pp_cotton_filter
      - sensor.front_active_carbon_filter
      - sensor.ro_filter
      - sensor.rear_active_carbon_filter

```
---

### PM 2.5 监测仪

PM 2.5 监测仪插件由 bit3725 制作，欢迎前往 项目 点赞~

使用方法：从 Github 下载 mi_air_quality_monitor.py，放入 custom_components/sensor/ 文件夹内（文件层级项目本身已经很清晰地给出了），在 configuration.yaml 添加如下设置：

```
sensor:
  - platform: mi_air_quality_monitor
    host: YOUR_SENSOR_IP
    token: YOUR_SENSOR_TOKEN
    name: YOUT_SENSOR_NAME
```

---

### 博联万能遥控器broadlink pro+
执行：
```
pip3 install broadlink
```
将模块更新到最新。
在 configuration.yaml 添加如下设置：
```
switch:
  - platform: broadlink
    host: 你的broadlink IP
    mac: '你的broadlink MAC'
```
有关小米设备接入的完整攻略，请参考 [Home Assistant](https://home-assistant.cc/component/xiaomi/) 中文文档小米设备章节。

### 自动化配置，脚本配置，设备分组等请查看[更多](https://www.hachina.io/)


### token获取：


方法一、安装 miio  

```
  pip3 install python-miio
```  
  发现小米设备： 
```  
  mirobo discover
```  


方法二：

安装4.0版本以下的米家app：例如：米家3.8

用手机登录米家账号，配置完成后，使用米家3.8版本app在虚拟机登录

然后进入系统文件 /data/data/com.xiaomi.smarthome/databass/miio.db

将miio.db打开即可得到token等所有设备信息。