---
title: laye快速点击会触发多次回调
tags: layer
category: 爬坑日常
thumbnail: /images/postsimages/layui.png
abbrlink: 23580
date: 2018-12-14 14:16:53
---

> 场景还原

测试同学反馈点击了一次操作，为什么会有两条操作记录？

我：？？？？

> 排查思路

查看日志，看一下是不是发了两次请求，果不其然啊：

![日志截图](/images/postsimages/20181214142322.png)

并发了，同一时间发送了两次请求，出现了脏写。

> 原因  

系统的confirm是线程阻塞的，而layer.confirm是非阻塞的，这一点在官方的API文档中有提到。

> 解决方案

```javascript
var lock = false;
layer.confirm('is not?',{btn:['确定','取消']},function(index){
    if(!lock) {
        //加锁防止多次回调
        lock =true;
        $.ajax({
            // .....
        })
    }
})

```




