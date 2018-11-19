---
title: 两个比较好用的mysql函数
date: 2018-02-19 18:42:35
tags: mysql
---
##### 我们经常会面临要从数据库里判断时间，取出特定日期的查询。但是数据库里储存的都是unix时间戳，处理起来并不是特别友好。幸而MYSQL提供了几个处理时间戳的函数，可以帮助我们在查询的时候，就将时间戳格式化。用法举例如下：

#### 1.FROM_UNIXTIME()函数

> FROM_UNIXTIME(unix_timestamp,format)

* 参数unix_timestamp  时间戳 可以用数据库里的存储时间数据的字段

* 参数format　　要转化的格式  比如“”%Y-%m-%d“”  这样格式化之后的时间就是 2017-11-30

## 可以有的形式：##

+ %M 月名字(January～December) 
+ %W 星期名字(Sunday～Saturday) 
+ %D 有英语前缀的月份的日期(1st, 2nd, 3rd, 等等。） 
+ %Y 年, 数字, 4 位 
+ %y 年, 数字, 2 位 
+ %a 缩写的星期名字(Sun～Sat) 
+ %d 月份中的天数, 数字(00～31) 
+ %e 月份中的天数, 数字(0～31) 
+ %m 月, 数字(01～12) 
+ %c 月, 数字(1～12) 
+ %b 缩写的月份名字(Jan～Dec) 
+ %j 一年中的天数(001～366) 
+ %H 小时(00～23) 
+ %k 小时(0～23) 
+ %h 小时(01～12) 
+ %I 小时(01～12) 
+ %l 小时(1～12) 
+ %i 分钟, 数字(00～59) 
+ %r 时间,12 小时(hh:mm:ss [AP]M) 
+ %T 时间,24 小时(hh:mm:ss) 
+ %S 秒(00～59) 
+ %s 秒(00～59) 
+ %p AM或PM 
+ %w 一个星期中的天数(0=Sunday ～6=Saturday ） 
+ %U 星期(0～52), 这里星期天是星期的第一天 
+ %u 星期(0～52), 这里星期一是星期的第一天 
+ %% 一个文字% 

## 使用举例：##

```mysql
SELECT
username,
FROM_UNIXTIME(create_time, "%Y-%m-%d") AS dat
FROM
`wp_user`

GROUP BY 

dat
```
这样就能查出每天有哪些用户注册了。按天分组，你可以将数据导出后进行其他操作。

#### 2.UNIX_TIMESTAMP（）

> UNIX_TIMESTAMP(date)

* 其中date可以是一个DATE字符串，一个DATETIME字符串，一个TIMESTAMP或者一个当地时间的YYMMDD或YYYMMDD格式的数字

* 用这个函数可以帮助我们在时间戳中筛选出某些天的数据。

## 比如说： ##

```mysql
SELECT
username,
FROM_UNIXTIME(create_time, "%Y-%m-%d") AS dat
FROM
`wp_user`
WHERE
create_time >=UNIX_TIMESTAMP(''2017-11-29')
AND
create_time <UNIX_TIMESTAMP(''2017-11-30')
GROUP BY 
dat


```


* 这个查询可以让我们查出29号那一天的用户注册记录。

** 善用这两个MYSQL函数可以帮助我们提高处理数据的效率。**