---
title: 'Sublime text3中文版 无法安装插件There are no packages available for installation问题的解决'
date: 2018-08-01 15:16:11
tags: Sublime text3
category: 爬坑日常
thumbnail: http://www.mt30.com/Soft/UploadSoftPic/201711/2017110909245152.png
---

说起来差点没被气死，我当时的情况已经是要疯了，连他们的域名都ping不通，我还想着，咋地，要倒闭了？

首选项-》插件设置-》Package Control-》默认

里边的这个配置项  https://sublime.wbond.net/channel.json 这个地址我在浏览器都无法访问，ping这个域名都超时（后来被证明可能是网速的原因，有时候能访问得到有时候访问不到）。

可是我不明白的是，官方的这个配置为什么不能直接用，还需要用户自行进行配置，感觉有毒啊。

好了，不废话了，直接说解决方案：

1.下载  https://packagecontrol.io/channel_v3.json 随便放在什么目录里，php程序员的话，我觉得我们可以这么搞:

```php
file_put_contents('channel_v3.json',file_get_contents("https://packagecontrol.io/channel_v3.json"));
```


在sublime里直接ctrol+b运行，直接下载了，是不是很棒？哈哈哈

2.打开这个文件，修改"schema_version": " 3.0.0" 为 "schema_version":" 2.0"

3.路径：首选项-》插件设置-》Package Control-》默认

修改此配置
```JSON
"channels": [
		"https://sublime.wbond.net/channel.json"
	],
```
改为：
```JSON
"channels": [
		"D:/Program File/phpstudy/PHPTutorial/WWW/test/channel_v3.json"
	],
```


好了，大功告成。至于这么做的原理，参见这位老哥的文章，不过这个老哥好像发了一篇博文之后再没有更新了，本来也想直接转载的，但是怕是等不到作者回我了，还是自己写一遍吧。

> 文章地址在这里

[参考文章](https://blog.csdn.net/qiangweiyan/article/details/79117641)