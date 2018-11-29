---
title: cmder的segmentation fault错误修复
tags: cmder
category: 爬坑日常
thumbnail: /images/postsimages/cmder.png
abbrlink: 57887
date: 2018-11-29 10:54:10
---

### Segmentation fault

> 现场还原

+ 问题出现的原因是我在 cmder的命令行里执行了`cmder /register ALL`命令，本意是把cmder放到右键菜单里去的
但我没想到的是，各种不成功，提示cmder lanchun什么的，之后，我换在了windows自带的cmd中执行这个命令，成功了。
但是令人疑惑的是cmder的bash窗口就此开始抽疯，`cd` 命令可以使用 `ll`、`ls`之类的命令直接抛出`Segmentation fault`
错误。

> 实验过程&&猜想

+  百度搜了很久并没有找到解决方案，百度提到可能的原因：

1. git bash 版本过旧，有概率出现这个问题，但是升级之后并没有解决这个问题

+ 猜想可能的原因：

1. Cmder full这个版本是有BUG的，我们看Cmder的设置的时候，可以发现他的GUI其实是`ConEmu`
，然后又拿这个调用了git bash ，那我们直接拿ConEmu调用git bash会不会有问题呢，所以我们打开ConEmu执行
命令`cd git/bin` && `bash --login -i` 我们发现我们这时候进入bash 界面了，我们使用一下`ll`命令，哎，这次没有报错了。

2. 我们换Cmder mini 试一下，打开bash:bash窗口，卧槽，啥玩意，居然说系统找不到制定路径，好的，我们从设置里看一下，执行bash窗口之后
执行的哪个命令，可以看到是`cmd /c ""%ConEmuDir%\..\git-for-windows\bin\bash" --login -i"` ,我们打开安装目录，看一下，我去！
`ConEmuDir` 的上级根本没有`git-for-windows`目录，好吧，你赢了，而在full版本中是有的！！！ 我后来找到我git-bash的安装目录，复制整个文件夹
到cmder的`Vender` 目录，改名成`git-for-windows`,打开bash:bash窗口，哎，进来了，运行命令试试，好的，Surprise ! 这次没有报错。
问题成功解决。

> 结论 

#### 结论就是 Cmder full 的版本在Windows10下，可能有某种未知的Bug , 我们可以通过尝试使用给 Cmder mini 添加git-bash的办法，来代替它。