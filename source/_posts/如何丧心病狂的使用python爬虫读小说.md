---
title: 如何丧心病狂的使用python爬虫读小说
tags: python
categroy: 厚积薄发
thumbnail: /images/postsimages/20181213.jpg
abbrlink: 51984
date: 2018-12-13 09:37:03
---

> 写在前边

其实一直想入门python很久了，慕课网啊，菜鸟教程啊python的基础的知识被我翻了很多遍了，但是一直没有什么实践。刚好，这两天被别人一直安利一本小说《我可能修的是假仙》，还在连载中的，我等屌丝，打钱是不可能打钱的，只好先去网上找一下资源了，基本笔趣阁啊，什么的提供很多在线的资源给我们。好吧，就看这个就行了，可是看也看得不爽啊，，浏览器上下部分都被什么 **美女荷官在线发牌**，**一夜不射提升半小时**之类你懂的画面遮盖了，还经常误触，如果是在电脑上看，我们可以用ADBLOCK之类的广告插件屏蔽，可是手机浏览器貌似没有插件啊，那怎么办呢？我可是程序员啊，程序员怎么能向这种问题低头呢？

> 解决方案

我们把在线网页上的章节名和章节内容都保存下来，造一个离线的版本不就没这个问题了么？

那怎么保存呢，这就需要我们的主角出场了，铛铛铛，**python scrapy爬虫框架**

> 关于scrapy

向大家推荐 一个好玩的有趣的牛逼的网站**[scrapy中文教程](http://www.scrapyd.cn/doc)**

这个作者写的很有趣，摘录一下：

本scrapy文档，主要是给诸君介绍一下神马是scrapy，scrapy能干神马，提提大伙的学习热情！scrapy是一个网页爬虫框架，神马叫做爬虫，如果没听说过，那就：内事不知问度娘，外事不决问谷歌，百度或谷歌一下吧！……（这里的省略号代表scrapy很牛逼，基本神马都能爬，包括你喜欢的苍老师……这里就不翻译了）

> 爬虫代码

```python
import scrapy

class firstdemo(scrapy.Spider):

    # 爬虫名称
    name = 'firstdemo'
    # 第一页
    start_urls= ['http://m.biquku.la/16/16889/578155.html']
    def parse(self,response):
        filename = '我可能修的是假仙.txt'
        # 章节名
        title = response.css('.zhong::text').extract_first()
        # 章节内容
        content = response.xpath("string(//article[@id='nr'])").extract()[0].replace('\n','').replace('\xa0','')
        self.log(title)
        with open(filename,"a+",encoding='utf-8') as f:
           f.write(title)
        #    添加章节目录
           f.write('\n')
        #    添加换行（\n）是为了让txt阅读器识别章节目录
           f.write(content)
           f.write('\n')
           f.close
        next_page = response.css('.nr_page a::attr(href)').extract()[2]
        if next_page is not None:
            next_page = 'http://m.biquku.la'+next_page
            yield scrapy.Request(next_page,callback=self.parse)
        else:
            self.log('已到最终章节')
```

没想到吧，代码就这么多，具体的教程可以参见向大家推荐的那个网站。最后我们执行`scrapy crawl firstdemo`就开始爬取了。

![运行图](/images/postsimages/20181213100803.png)

> 最后

最后？哪里有什么最后？都下载下来了，还不抓紧去看一下我们的战斗成果？

当然还是要提醒诸位，学习为主，不要玩物丧志。