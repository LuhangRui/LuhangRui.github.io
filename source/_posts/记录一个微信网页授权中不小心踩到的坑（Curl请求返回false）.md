---
title: 记录一个微信网页授权中不小心踩到的坑（Curl请求返回false）
tags: 微信开发
category: 爬坑日常
thumbnail: >-
  https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=436013150,3405885141&fm=26&gp=0.jpg
abbrlink: 40635
date: 2018-03-29 09:47:40
---

* 这个问题是file_get_contents不能获取https的内容引起的。这样的情况下，我们一般会采用curl拓展来模拟请求。

##### 代码demo（当然这是错误的示范）：
```php
function get_url_content($url){
    $ch=curl_init();
    curl_setopt($ch,CURLOPT_URL,$url);
    curl_setopt($ch,CURLOPT_SSL_VERIFYPEER,false);
    curl_setopt($ch,CURLOPT_SSL_VERIFYHOST,false);
    curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
    $result=curl_exec($ch);
    return $result;
}

```
##### 但是这个代码中有些问题，假如我们想访问https的内容的话，我们必须开启这两项验证
```php
CURLOPT_SSL_VERIFYPEER
CURLOPT_SSL_VERIFYHOST
```

```php
function get_url_content($url){
    $ch=curl_init();
    curl_setopt($ch,CURLOPT_URL,$url);
    curl_setopt($ch,CURLOPT_SSL_VERIFYPEER,true);
    curl_setopt($ch,CURLOPT_SSL_VERIFYHOST,true);
    curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
    $result=curl_exec($ch);
    return $result;
}
```

* 但是在获取用户信息的时候,Curl请求总是返回false，检查参数，什么都没错，为什么会返回false呢，最后实在想不通。

![百度哇](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522296265531&di=96cd2e42640c8e5f6c3574b3eeed7904&imgtype=jpg&src=http%3A%2F%2Fimg0.imgtn.bdimg.com%2Fit%2Fu%3D2917263305%2C2408849530%26fm%3D214%26gp%3D0.jpg)

so 看到了这篇文章  

#### [php curl返回false填坑记-curl调用微信创建自定义菜单返回false](https://blog.csdn.net/marswill/article/details/71123253)
```php
$url = ' https://api.weixin.qq.com/cgi-bin/menu/create?access_token='.$accessToken;
```
回头去检查我的代码，哎哟卧槽，还真是。就像那篇文章的大哥说的那样

### 总结：使用curl来请求数据时curl的url地址中的任何地方不能有空格存在，不然会返回一个你琢磨不透的false