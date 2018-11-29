---
title: 使用vagrant一键部署本地php开发环境（二）制作自己的vagrant box
tags: vagrant
category: 厚积薄发
thumbnail: >-
  https://gss0.bdstatic.com/-4o3dSag_xI4khGkpoWK1HF6hhy/baike/w%3D268%3Bg%3D0/sign=b7c0b9d9fc36afc30e0c38638b228cf9/203fb80e7bec54e7825eee07b2389b504fc26a7d.jpg
abbrlink: 11255
date: 2018-09-14 16:00:43
---
在上篇的基础上 ，我们已经安装好了virtualbox和vagrant，没有安装的话，参照上篇

[使用vagrant一键部署本地php开发环境（一）](/2018/09/14/使用vagrant一键部署本地php开发环境（一）/)

1.从网易镜像或阿里等等镜像下载Centos7
http://mirrors.163.com/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1804.iso

2.打开virtualbox进行虚拟机安装 
------------------具体步骤自己百度，没什么难的。

3.虚拟机安装好之后
1）以root用户登陆
![登陆](/images/postsimages/20180914142203352.png)


登陆之后依次执行  `adduser vagrant`  和 `passwd vagrant`命令，创建vagrant用户密码也设置为vagrant。

为vagrant用户配置sudo免密权限：
```
chmod 0777 /etc/sudoers
vim /etc/sudoers
```

依次执行这两个命令
![设置免密](/images/postsimages/20180914143311492.png)


在root行下新增vagrant用户，参照图片设置。完事esc :wq保存退出

执行`chmod 0440 /etc/sudoers` 恢复默认权限

2）配置ssh

执行 yum install openssh-server 如果没安装的话安装一下，如图是已经安装过的。
![安装openssh](/images/postsimages/20180914142528988.png)


安装完毕之后执行  `vim /etc/ssh/sshd_config`
![配置ssh](/images/postsimages/20180914143918964.png)


打开监听和端口，并把允许root用户远程登陆打开。

4.下载官方公钥配置  vagrant ssh
1）. 下载官方公钥
```
wget https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant.pub
```

2）. 重命名authorized_keys，移动到.ssh文件下

`mv vagrant.pub .ssh/authorized_keys`

3）. 修改authorized_keys文件权限 除了属主vagrant以外，group和其他用户都不可写

`chmod go-w /home/vagrant/.ssh/authorized_keys` 

5.安装virtualbox增强工具，为共享目录做准备
1).点击菜单中的  设备 > 安装增强功能

2).切换到根目录创建cdrom目录

`cd / && mkdir cdrom && mount /dev/cdrom /cdrom`

3).切换到/cdrom并安装高级功能

`cd  /cdrom && ./VBoxLinuxAdditions.run`

4).安装完成关闭虚拟机

6.设置网络规则
网卡1按照如下设置，端口转发规则2222-》22
![设置端口映射](/images/postsimages/20180914145119497.png)


网卡2设置：
![设置网络](/images/postsimages/20180914145227965.png)


7.打包制作box
在本地主机的任意目录 执行
```
vagrant.exe package --base  centos(virtualbox中显示的虚拟机的名称)  --output  centos-lnmp.box(你给box起的名字，随意)，该操作会在当前目录下生成  centos-lnmp.box 
```
接下来你懂的。就又回到第一篇，如何使用box上了