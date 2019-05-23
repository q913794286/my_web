---
Title: pythom3.4升级python3.6
---
### Linux下python卸载与安装
96  箱子君 关注
2018.01.22 11:45* 字数 167 阅读 10603评论 0喜欢 0
卸载：
1、卸载python3.4
sudo apt-get remove python3.4

2、卸载python3.4及其依赖
sudo apt-get remove --auto-remove python3.4

3、清除python3.4
sudo apt-get purge python3.4
or
sudo apt-get purge --auto-remove python3.4

安装：
下载python
wget https://www.python.org/ftp/python/2.7.9/Python-2.7.9.tgz
wget -p /usr/lib https://www.python.org/ftp/python/2.7.9/Python-2.7.9.tgz(下载文件到指定目录)

解压、编译安装
tar -zxvf Python-2.7.9.tgz
cd Python-2.7.9
./configure --prefix=/usr/local/python-2.7.9
make
make install

系统自带了python版本，我们需要为新安装的版本添加一个软链
ln -s /usr/local/python-2.7.9/bin/python /usr/bin/python2.7.9


### Debian 8下Python 3.6的部署

最近在Debian 8下安装最新的Python 3.6时遇到了一些问题，在这里记录下安装过程。

使用的VPS商家提供的最小化Debian 8镜像。

1.前期工作

第一步先升级系统
```
apt-get update
```
然后安装screen，避免SSH连接断开造成安装中断
```
apt-get install screen
```
之后的操作都在screen下进行，screen的简单使用命令

“screen” 开启新screen实例

“screen -r” 重新attach到上次开启的实例

“ctrl-a、ctrl-d” 从当前screen实例detach

使用dpkg -l可以查询可用包，比如
```
dpkg -l python*
```
查到只有python3.4

2.安装依赖包
```
apt-get install build-essential -y
apt-get install libncurses5-dev libncursesw5-dev libreadline6-dev -y
apt-get install libdb5.3-dev libgdbm-dev libsqlite3-dev libssl-dev -y
apt-get install libbz2-dev libexpat1-dev liblzma-dev zlib1g-dev -y
apt-get install ca-certificates -y
```
这样安装好后，openssl的so库在/usr/lib/x86_64-linux-gnu下，而Python的setup.py安装脚本固定是在/usr/local/lib下寻找so库，会造成编译ssh相关模块时出错，因此需要创建软连接
```
ln /usr/lib/x86_64-linux-gnu/libssl.so.1.0.0 /usr/local/libssl.so
ln /usr/lib/x86_64-linux-gnu/libcrypto.so.1.0.0 /usr/local/libcrypto.so
```
3.下载Python源代码编译安装
```
wget https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tar.xz
tar -Jxvf Python-3.6.4.tar.xz
cd Python-3.6.4
./configure
make & make install
```

好了，现在通过python3 -V来查看当前版本，3.6.4成功安装，然后通过pip3可以安装软件。