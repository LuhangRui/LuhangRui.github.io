---
title: 使用vagrant一键部署本地php开发环境（一）
tags: vagrant
category: 厚积薄发
thumbnail: >-
  https://gss0.bdstatic.com/-4o3dSag_xI4khGkpoWK1HF6hhy/baike/w%3D268%3Bg%3D0/sign=b7c0b9d9fc36afc30e0c38638b228cf9/203fb80e7bec54e7825eee07b2389b504fc26a7d.jpg
abbrlink: 2021
date: 2018-09-14 15:46:31
---

#### 一：我们为什么需要用这玩意
我们在开发中经常会面临的问题：环境不一致，有人用Mac有人用Windos还有几个用linux的，而我们的服务器都是linux。
在我本地是可以的啊，我测了都，没有问题啊，然后看着上线之后的500错误懵比。It works on my pc .
#### 二：vagrant是什么东西
Vagrant是一个基于Ruby的工具，用于创建和部署虚拟化开发环境。它 使用Oracle的开源VirtualBox虚拟化系统，使用 Chef创建自动化虚拟环境。-------------------来自百度百科。

直白的说是一键生成特定虚拟机的工具。详细的我们下边会说。

#### 三：所需的软件：
1、virtualBox
可以到VirtualBox的官网进行下载： 
https://www.virtualbox.org/wiki/Downloads

2、vagrant
可以到vagrant官网下载 
https://www.vagrantup.com/downloads.html

这个下载特别慢，这里放出百度云的链接 

链接: https://pan.baidu.com/s/1Toy3SRRACOWa8g0ybUHD8Q 密码: puwm

3、vagrant box
    vagrant部署环境，需要一个box文件。如果在公司里面，可以从他们那里拷一个box文件安装。这样安装的环境与他们的开发环境是一致的。box文件也可以在网上下载。搜索：vagrantbox

http://www.vagrantbox.es/

（备注：这个地址实际上是失效的，很多box文件都找不到资源了，不过不要紧，因为我们可以自己做一个box，这个在下一篇文章我会讲如何定制一个自己的box，顺道放一个我制作好的基于centos7且安装好lnmp1.5的box）链接: Centos-lnmp.box密码: tssp

#### 四：工作流程
![工作流程](/images/postsimages/20180914115249464.png)

#### 五：vagrant的日常使用
下载以上vagrant和virtualbox之后，一路next安装，安装完成之后。

在任意位置新建一个文件夹，来管理你的box ，比如我们在D盘新建一个vagrant文件夹

1.把你下载的box文件扔进来，如图：

![](/images/postsimages/201809141313240.png)

2.我们在这个位置打开git-bash，或者用cmd切换到这个目录，我们以git-bash为例：

1）执行 
```
vagrant.exe  box add centos centos-lnmp.box
```
该命令是给box起名并添加到box列表

需要注意的就是在cmd命令窗口下可以不用.exe  直接执行
```
vagrant box add {name(你要起的名字随意)}  {url/file(本地文件地址或远程地址)}
```
![执行效果](/images/postsimages/20180914131643512.png)
2）执行命令
```
vagrant.exe init {centos(刚刚add操作时起的名字)}
```
![初始化虚拟机](/images/postsimages/20180914132351742.png)

就像这样，这个命令会初始化box并生成一个Vargrantfile的配置文件，在这个文件里我们可以 设置一些配置信息，比如共享主机目录到虚拟机目录，网络，虚拟机ip等信息。



打开配置文件
![修改配置文件](/images/postsimages/20180914132720938.png)


这些配置项默认都是注释掉的，我们需要找到这两行进行设置。其中共享目录的配置我们可以这样写，第一个参数为本地目录，第二个参数是虚拟机目录，/ 代表了虚拟机下的根目录。

通过共享目录设置，我们可以把我们的项目开发目录映射到虚拟机目录，通过虚拟机配置nginx，来让项目直接跑在虚拟机中。
```
config.vm.synced_folder "D:/data", "/vagrant_data"
```
3）部署环境

执行命令
```
vagrant.exe up
```
就可以一键部署虚拟机环境了

![启动虚拟机](/images/postsimages/20180914133548923.png)

4）虚拟机管理

> 执行命令 vagrant.exe ssh

vagrant.exe ssh
就能直接链接到虚拟机的系统了，目录已经挂载好了，接下来就是考验你linux操作能力的时候了，我们可以在这上边查看日志等等一系列事情。

备注:你也可以通过xshell来链接你的虚拟机，ip就是你配置的ip端口22，用户名密码均为vagrant



一般来说虚拟机启动之后就不需要管了。不过对于项目开发而言，你还需要做的一件事就是修改本地的hosts文件，让你请求的虚拟域名指向你的虚拟机ip。

#### 六：vagrant常用命令
+ vagrant init      # 初始化
+ vagrant up        # 启动虚拟机
+ vagrant halt      # 关闭虚拟机
+ vagrant reload    # 重启虚拟机
+ vagrant ssh       # SSH 至虚拟机
+ vagrant status    # 查看虚拟机运行状态
+ vagrant destroy   # 销毁当前虚拟机
+ vagrant box list    # 查看本地box列表
+ vagrant box add     # 添加box到列表
+ vagrant box remove  # 从box列表移除 
