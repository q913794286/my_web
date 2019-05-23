---
Title: homeserver教程
---
# APP
### 注册 
      大白话了自己琢磨
      
      
### 登录
* 1.主页有个房子图标 -> 修改默认房间名 -> 回到主页->点击设置图标 —> 添加房间 -> 
    
     回到主页 -> 点击房间名的 + 号 -> 点击设备修改名字并添加到房间
  

* 2.回到主页 -> 点击最顶上的添加 -> 再点击顶上的添加 -> 点击小米设备 -> 
     
     小米网关(ssid：mac — key: 密码) -> 重启homeserver
     
      例如：ssid：7811DCE1355b  
          
          key：imqnkgsarz3bucqk 
      
     小米wifi设备（ssid：IP  -  key：token）
     
         ssid: 192.168.31.11
         
         key: 1dbcbff9e79fa2194b1c238eac8607c2

* 3.回到主页 -> 点击房间名的添加 -> 继续添加设备

* 4.音响配网：主页下方，我的 -> 网络配置 ->扫描 ->找到音响 -> 点击配置wifi ->添加到房间



# server

* 1.ssh登录 - > cd /var/ha/ - > nano broadlink -> 修改各种 按键码。