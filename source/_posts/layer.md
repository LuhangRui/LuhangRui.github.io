---
title: 'layer.js错误Uncaught TypeError: i is not a function'
tags: layui
category: 爬坑日常
thumbnail: /images/postsimages/layui.png
abbrlink: 32812
date: 2018-07-24 14:44:08
---

##### 最初是要写一个管理后台来着，项目中需要用到弹出层，但是没有前端配合，我一个小PHP需要去写这玩意，怎么办呢？查了一些资料，发现layer对我来说还行，文档写的也比较完全，学习成本不高，就下决心用这个了。但是现实总是会给你泼冷水，刚引入就报了一个莫名其妙的错误。

![i is not a function](https://kengdie.oss-cn-shanghai.aliyuncs.com/20180724113849694.png)

**呐，就是这个，我自己一个人在那里纳闷，卧槽，我啥也没做啊，我只是引入进来怎么还报错了呢？**

##### 我还心想着，这个layer.js也已经被很多人用了，不太可能是这个插件的问题吧。我就把引入的js文件一个一个的注释掉，开始排查，后来发现，单独引入layer，或者单独引入JQuery都是没有问题的，但是同时引入的话，就会报错，我也有点懵逼了，什么鬼，这两个js冲突了？后来突然想到，马丹，layer.js是依赖于JQ的。

 

后记：其实以上问题都是不仔细看别人的文档造成的。

来自layer.js官网的说明：

![来自官网的说明](https://kengdie.oss-cn-shanghai.aliyuncs.com/20180724114741117.png)
