---
title: linux操作利器alias用法
tags: linux
category: 爬坑日常
thumbnail: /images/postsimages/linux.jpg
abbrlink: 62801
date: 2019-04-04 15:33:35
---

### 写在前边

学习这件事，有时候并不一定很刻意，而是从生活，从经验中去积累，不知道什么时候就发生了。

> type 命令

 一般情况下，type命令被用于判断另外一个命令是否是内置命令，但是它实际上有更多的用法。

    1.判断一个名字当前是否是alias、keyword、function、builtin、file或者什么都不是；

    2.判断一个名字当前是否是alias、keyword、function、builtin、file或者什么都不是的另一种方法（适用于脚本编程）；

    3.显示一个名字的所有可能；

    4.查看一个命令的执行路径（如果它是外部命令的话）；

    5.强制搜索外部命令。
详细参见这位老哥的博客 [type命令使用](https://www.cnblogs.com/jxhd1/p/6699177.html)

> alias 命令

我们在服务器上查看日志的过程种，不可避免的要记住一大串的路径，比如说`/export/www/logs_backend/Java_service_logs/`,每次链接到服务器都要输一遍这个长长的路径才能进入日志目录，Linux就给我们提供了一个简单的方法来避免这种操作，就是`alias`,当然你也可以用软连接来简化这个过程，不过用`alias`会更为简单。

说这个之前我们要提一下`type`这个命令：

我们用到的是上边说的`type`命令的第一个功能，用来测试一下我们要自定义的别名有没有被占用

```bash
#如果没被占用
dell@DESKTOP-8U4HTOL MINGW64 /d/develop
$ type t
bash: type: t: not found
#如果被占用了
$ type ll
ll is aliased to `ls -l'
```


用法：alias [-p] [name[=value] ... ]    注意`=`和字符串之间不能包含空格

1.命令`alias`

直接使用命令`alias`可以查看当前登录环境下的所有命令别名

```bash
$ alias
alias ll='ls -l'
alias log='cd /d/develop/backend/storge/logs'
alias ls='ls -F --color=auto --show-control-chars'

```
2.设置别名 `alias 别名='完整命令'`

```bash
$ alias log='cd /d/develop/backend/storge/logs'
```

3.命令`alias + 命令`

这将显示这个别名命令的具体含义

```bash
$ alias log
alias log='cd /d/develop/backend/storge/logs'
```

4.给一组命令设置别名

```bash
dell@DESKTOP-8U4HTOL MINGW64 /d/develop
$ type t
bash: type: t: not found

dell@DESKTOP-8U4HTOL MINGW64 /d/develop
$ alias t='cd /d/study;mkdir test;touch 01.txt'

dell@DESKTOP-8U4HTOL MINGW64 /d/develop
$ t

dell@DESKTOP-8U4HTOL MINGW64 /d/study
$ ll
total 1
-rw-r--r-- 1 dell 197121 0 4月   4 16:03 01.txt

```

5.持久化别名

以上说的方法，都是临时性的，只在当前登录环境下有效，一旦退出登录就会失效，要想持久化，需要修改`/etc/bash.bashrc`


centos下是`/etc/bashrc`,ubuntu下为`/etc/bash.bashrc`

vi /etc/bashrc

在文件末尾添加alias log='cd /d/develop/backend/storge/logs'并保存退出

执行source /etc/bashrc  使配置生效

以上。
