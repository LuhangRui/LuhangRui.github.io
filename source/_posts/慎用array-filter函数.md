---
title: 慎用array_filter函数
date: 2018-09-26 16:22:21
tags: 数组筛选
category: 爬坑日常
---

### array_filter
(PHP 4 >= 4.0.6, PHP 5, PHP 7)

array_filter — 用回调函数过滤数组中的单元

> 说明

array array_filter ( array $array [, callable $callback [, int $flag = 0 ]] )


依次将 array 数组中的每个值传递到 callback 函数。如果 callback 函数返回 true，则 array 数组的当前值会被包含在返回的结果数组中。数组的键名保留不变。

> 参数
 

+ array

     要循环的数组

+ callback

     使用的回调函数

如果没有提供 callback 函数， 将删除 array 中所有等值为 FALSE 的条目。更多信息见转换为布尔值。

+ flag

    决定callback接收的参数形式:
    
    ARRAY_FILTER_USE_KEY - callback接受键名作为的唯一参数
    ARRAY_FILTER_USE_BOTH - callback同时接受键名和键值
> 返回值

返回过滤后的数组。

 

array_filter其实是一个相当好用的函数，常用的场景包括，表单多条件筛选，可以直接用此函数过滤掉没有值的筛选项。

**但是有一个问题，必须要重视：**

#### array_filter会过滤掉任何值等于FALSE的值，也就是说  0值，空字符串，null，都会被过滤当你的筛选项里有值等于0时，问题就会暴露出来，在我们的项目里，在调接口时做了过滤，没想到有一个默认的状态等于0的参数被我过滤掉了，就造成了线上数据的失常，也算是一个比较低级的错误了。此文谨记。