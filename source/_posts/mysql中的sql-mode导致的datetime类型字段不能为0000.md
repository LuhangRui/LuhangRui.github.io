---
title: mysql中的sql_mode导致的datetime类型字段不能为0000
tags: mysql
thumbnail: /images/postsimages/mysql.jpg
category: 爬坑日常
abbrlink: 57518
date: 2019-05-14 11:39:07
---

> 问题描述：

在执行建表语句的时候，出现`invalid default datetime value '0000-00-00 00:00:00'`,从字面意思看，就是**不合法的默认值'0000-00-00 00:00:00'**，但是为什么呢？，datetime类型应该是允许这样的值出现。

> 排查：
这个时候我们需要执行

```mysql
select @sql_mode;
```

你会发现值是这样的：

```mysql

@@sql_mode = 'ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'

```

各项数值含义：

ONLY_FULL_GROUP_BY

对于GROUP BY聚合操作，如果在SELECT中的列，没有在GROUP BY中出现，那么将认为这个SQL是不合法的，因为列不在GROUP BY从句中

STRICT_TRANS_TABLES

在该模式下，如果一个值不能插入到一个事务表中，则中断当前的操作，对非事务表不做任何限制

NO_ZERO_IN_DATE

在严格模式，不接受月或日部分为0的日期。如果使用IGNORE选项，我们为类似的日期插入'0000-00-00'。在非严格模式，可以接受该日期，但会生成警告。

NO_ZERO_DATE

在严格模式，不要将 '0000-00-00'做为合法日期。你仍然可以用IGNORE选项插入零日期。在非严格模式，可以接受该日期，但会生成警告

ERROR_FOR_DIVISION_BY_ZERO

在严格模式，在INSERT或UPDATE过程中，如果被零除(或MOD(X，0))，则产生错误(否则为警告)。如果未给出该模式，被零除时MySQL返回NULL。如果用到INSERT IGNORE或UPDATE IGNORE中，MySQL生成被零除警告，但操作结果为NULL。

NO_AUTO_CREATE_USER

防止GRANT自动创建新用户，除非还指定了密码。

NO_ENGINE_SUBSTITUTION

如果需要的存储引擎被禁用或未编译，那么抛出错误。不设置此值时，用默认的存储引擎替代，并抛出一个异常。


> 解决方案：

经过排查，问题已经很明显了，所以我们怎么做呢?

答案就是去掉`NO_ZERO_IN_DATE`&&`NO_ZERO_DATE`。

```mysql
#1.先执行这句更改SQL模式
set @@sql_mode = 'NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
#2.在执行要执行的建表语句
create table .......

```

以上。



