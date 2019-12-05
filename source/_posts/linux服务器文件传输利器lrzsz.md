---
title: linux服务器文件传输利器lrzsz
tags: linux
category: 爬坑日常
thumbnail: /images/postsimages/linux.jpg
abbrlink: 26716
date: 2019-12-05 15:37:45
---

#### 场景 

&emsp;&emsp;服务端开发人员经常会遇到的一个场景就是将本地的文件传到服务器上，或者把服务器上的文件下载到本地，那这个时候大家一般会用ftp服务，来完成这件事。但是如果服务器上并没有安装ftp服务的时候，这个操作就显的很繁琐。那有没有轻量的，方便又好用的工具呢？这就要说到我们今天要给诸位介绍的这个小工具了`lrzsz`。

#### 关于`lrzsz`

&emsp;&emsp;lrzsz是一个unix通信套件提供的X，Y，和ZModem文件传输协议,可以用在windows与linux 系统之间的文件传输，体积小速度快。

#### 安装`lrzsz`

&emsp;&emsp;安装可以用源码安装也可以以所在平台的软件管理工具下载。

1.以centos为例：
```bash
yum -y install lrzsz
```
2.源码安装：
```bash
# 下载安装包
wget http://down1.chinaunix.net/distfiles/lrzsz-0.12.20.tar.gz
tar -zxvf lrzsz-0.12.20.tar.gz
cd lrzsz-0.12.20
# 编译
./configure –prefix=/usr/local/lrzsz
make
make install
# 把命令加入$PATH
ln -s /usr/local/lrzsz/bin/lrz /usr/bin/rz
ln -s /usr/local/lrzsz/bin/lsz /usr/bin/sz

```
#### `lrzsz`使用

1.sz: 将选定的文件发送(send)到本地机器。
example:
```bash
sz /home/wwwlog/nginx.error.log
```
2.rz: 运行该命令会弹出 一个文件选择窗口, 从本地选择文件上传到服务器(receive)。

```bash
#rz命令不使用参数即可，会弹出系统自带文件选择框
rz
```
#### xshell or SecureCRT

&emsp;&emsp;在xshell和SecureCRT中，执行`sz`命令的表现略有不同，xshell会弹出保存位置选项，而CRT采用的是默认位置，这是个配置项。可以依次打开菜单`Options -> session options -> X/Y/Zmodem`进行设置。

#### 结语

&emsp;&emsp;贼拉好用，隔壁后端大哥都感动哭了。
