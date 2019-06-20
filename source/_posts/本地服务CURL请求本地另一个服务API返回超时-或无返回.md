---
title: 本地服务CURL请求本地另一个服务API返回超时/或无返回
tags: Curl
category: 爬坑日常
thumbnail: >-
  https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=2822188571,1792558379&fm=58&bpow=600&bpoh=600
abbrlink: 49088
date: 2018-07-23 14:27:06
---

##### 入职之后一直在忙，终于有时间整理一波最近踩到的坑。

> 起因：

项目是微服务架构，一个项目对外提供API，新的项目调用API获得数据。于是就在本地搭建了两个服务。配置了两个虚拟域名，指向两个项目，当然我本地是windows+nginx。意外就在这个时候发生了，我在新开发的项目中，调用另一个项目的API时，总是CURL超时，如果CURL不设置超时的话就会造成NGINX卡死。百思不得其解。

> 问题成因：

+ 后来了解到原来PHP+NGINX在windows下是不支持并发的？[参考文章在这里](http://ishwy.me/?cat=12)
这个我并不敢十分的肯定，我只是觉得能理解这个东西，我们看nginx的配置文件的话，会更清楚一些，到底发生了什么：

![nginx配置](https://kengdie.oss-cn-shanghai.aliyuncs.com/20180723183343520.png)

fastcgi_pass 都绑定了9000端口

所以两个服务就会有一个端口被占用，无法返回消息。

解决方案：



第一是要把服务绑定到其他没被占用的端口，比如9009
![参考配置](https://kengdie.oss-cn-shanghai.aliyuncs.com/20180723184314759.png)
然后切换到php-cgi所在的目录，再单独启动一个php-cgi进程，去监听这个个端口

```php
php-chi.exe -b 127.0.0.1:9009
```



好的，完美解决。