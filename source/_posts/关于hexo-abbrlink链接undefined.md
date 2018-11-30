---
title: 关于hexo-abbrlink链接undefined
tags: abbrlink
category: 爬坑日常
thumbnail: 'http://img0.imgtn.bdimg.com/it/u=2314092782,3784320398&fm=26&gp=0.jpg'
abbrlink: 733
date: 2018-11-30 10:01:44
---

> 关于hexo-abbrlink

`hexo-abbrlink`是一个hexo博客链接永久化的解决方案

+ 支持使用不同的算法和进制对文章链接进行转换

| 算法 |  进制 | 生成链接|
|---|---|---|
|crc16|	hex|	https://post.zz173.com/posts/3ab2.html  |
|crc16|	dec|	https://post.zz173.com/posts/12345.html  |
|crc32|	hex|	https://post.zz173.com/posts/9a8b6c4d.html  |
|crc32|	dec|	https://post.zz173.com/posts/1690090958.html  |

> 安装

```
npm install hexo-abbrlink --save
```

> 使用

打开config.yml，修改permalink中类似这样

```
permalink: posts/:abbrlink.html
```
具体说明参照作者的github说明


[项目地址在这里](https://github.com/Rozbo/hexo-abbrlink)

> 可能出现的问题：

终于回到标题上来，有的同学说，这个配置完成之后，文章的链接都变成了undefined,新的文章没问题，老的文章就不行了。这个问题其实我们仔细想一下就能明白，我们首先要执行`hexo clean` 清楚掉以前生成的文章缓存，然后`hexo g`重新渲染就ok了。