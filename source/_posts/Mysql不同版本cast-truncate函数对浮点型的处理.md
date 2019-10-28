---
title: Mysql不同版本cast/truncate函数对浮点型的处理
tags: mysql
category: 爬坑日常
thumbnail: /images/postsimages/mysql.jpg
abbrlink: 57345
date: 2019-10-28 13:35:48
---
### Mysql不同版本cast/truncate函数对浮点型的处理

我们不妨先来看一个现象

```mysql
select cast(1/5 as decimal(4,2));

```

我们先盲猜一下这条SQL的运行结果，如果按照我们的一贯经验，那应该是
`0.20`，毕竟我们`as decimal(4,2)`是保留两位小数的。但是实际上并不一定是。
在不同的mysql版本下执行了这条sql：
```mysql
select cast(1/5 as decimal(4,2)) as res,version() as v;
```
得到的结果如下：

`5.6.45`、`5.6.36`、`5.6.39`三个小版本中的表现，不论是`cast`还是`truncate`函数，对能整除的操作的保留两位小数
得到的结果都是无法保留末尾是`0`的小数的，末尾的`0`会自动被舍弃。

`5.7.22`、`8.0.15`这两个版本表现完全是另一种结果，执行以下SQL：

```mysql
select cast(1/5 as decimal(4,2)) as castRes,truncate(1/5,2) as truncateRes;
```

得到的结果都是`0.20`。

某次开发过程中遇到了这个Bug，不得其解，记录一下，以示后来人。不过我们也由此得出一个准则，数据格式的`format`处理还是
尽可能的交给程序去处理，而不是mysql。
