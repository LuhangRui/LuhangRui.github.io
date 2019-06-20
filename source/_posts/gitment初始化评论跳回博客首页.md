---
title: gitment初始化评论跳回博客首页
tags: hexo
category: 爬坑日常
thumbnail: /images/postsimages/gitment.jpg
abbrlink: 47886
date: 2019-01-08 17:05:01
---

> 表现

众所周知，gitment评论系统需要初始化以创建对应的`issue`，可是我在点击login with github的时候，总是跳向博客首页！WTF！什么鬼？这样不程序啊？

![WTF](https://kengdie.oss-cn-shanghai.aliyuncs.com/what.jpg)

> 排查

1.F12查看login回调链接，`redirect_uri`参数没有什么问题啊，行，我们回头查看，github的文档，

![github文档](https://kengdie.oss-cn-shanghai.aliyuncs.com/redirect.png)

[github文档地址](https://developer.github.com/apps/building-oauth-apps/authorizing-oauth-apps/)

2.文档中提到，如果地址不匹配的话，就会重定向到你在OAuth Apps 中设置的`Authorization callback URL`，而这里我们配置的都是首页，所以会跳向首页就可以理解了，那也就是说这里的地址和回调地址里我们传的`redirect_uri`不匹配，那就检查这两个地址吧，对比之后果然发现了一些有意思的事情：

```shell 
redirect_uri      https://ergou.fun/posts/3834.html

Authorization callback URL  http://ergou.fun
```

一个是http，一个是https，当然匹配不上了啊！摔！修改一下，`Authorization callback URL`改为`https`，来来来测试一下，OK，完美解决。

> 后记

如果你仔细观察的时候，你会发现跳向博客首页的时候，地址栏链接变的很复杂，都是些什么东西呢，我们不妨解析一下

```
https://ergou.fun/?
error=redirect_uri_mismatch&
error_description=The+redirect_uri+MUST+match+the+registered+callback+URL+for+this+application.&error_uri=https://developer.github.com/apps/managing-oauth-apps/troubleshooting-authorization-request-errors/#redirect-uri-mismatch

//真是贴心啊，错误原因，和参考文档地址全都给了
```


