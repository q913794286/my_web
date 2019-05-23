---
Title: Linux挂载img镜像文件及分区
Sort: 1
---
### cd到img所在文件目录

### 挂载：

```
$sudolosetup --show -f  xxx.img   #如果提示忙碌尝试loop1
$sudo kpartx -va /dev/loop0
$mont /dev/loop0p1 /mnt

```
### 卸载
```
kpart -d /dev/loop0
losetup -d /dev/loop0
```
### ROOTFS标记
给指定分区打上ROOTFS标记
```
e2label /dev/data "ROOTFS"
```