---
title: CI框架对HTML输入的处理/CI框架引用ueditor时对提交内容的默认处理
tags: CI
category: 爬坑日常
thumbnail: 'http://codeigniter.org.cn/assets/images/ci-logo-big.png'
abbrlink: 23321
date: 2018-07-30 15:01:57
---

项目里近期用到了富文本编辑器，可是写入数据的时候总是写入，

```html
<p xss="removed">内容</p>
```
所有的样式都会被改写成这样，`xss="removed"`，造成前端展示的时候没有任何样式，排查的过程中一度以为是百度的富文本编辑器对输入的数据进行了处理，不过在进行排查的过程中，我用官方的demo提交到一个php文件做测试的时候发现，接受到的数据并没有问题。

所以想到框架中接受数据用的是CI框架封装的
```php
$this->input->post(null,true);
```
并不是原生的 `$_POST` 有可能是这里有问题，很容易就想到，是框架本身都输入的内容做了防XSS攻击的处理。
![对比图](/images/postsimages/20180730104321870.png)


以上。