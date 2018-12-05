---
title: laydate V5.0.8动态设置min值'
tags: laydate
category: 爬坑日常
thumbnail: /images/postsimages/20181205.jpg
abbrlink: 55229
date: 2018-12-05 13:47:18
---
### laydate通过设置min,max值来对用户输入的时间做约束

> laydate v1.0版本

```javascript

//日期插件
    var start={
        elem:"#start",
        format:"YYYY-MM-DD hh:mm:ss",
        min:"2013-08-18 00:00:00",
        max:"2099-06-16 23:59:59",
        istime:true,
        istoday:true,
        choose:function(a){
            end.min=a;
            end.start=a
        }
    };
    var end={
        elem:"#end",
        format:"YYYY-MM-DD hh:mm:ss",
        min:"2013-08-18 00:00:00",
        max:"2099-06-16 23:59:59",
        istime:true,
        istoday:true,
        choose:function(a){
            start.max=a
        }
    };
    laydate(start);
    laydate(end);
//只要在回调函数choose中赋值就行了

```



> laydate v5.0版本

```javascript
//日期插件
        var start=laydate.render({
            elem:"#start_time",
            type:"datetime",
            format:"yyyy-MM-dd HH:mm:ss",
            min:"2013-08-18",
            max:"2099-06-16",
            done:function(value,date){
                end.config.min={
                    year: date.year,
                    month: date.month-1,
                    date: date.date,
                    hours: date.hours,
                    minutes: date.minutes,
                    seconds: date.seconds
                };
            }
        });
        var end =laydate.render({
            elem:"#end_time",
            type:"datetime",
            format:"yyyy-MM-dd HH:mm:ss",
            min:"2013-08-18",
            max:"2099-06-16",
            done:function(a){
                // start.max=a
            }
        });


```

在v5.0版本中，min值变成了一个对象，并不会重新渲染，所以直接对min设置值是没有用的，得单独对每个min的子元素赋值

**特别要注意的是month要减1**

虽然我也并不清楚为什么要这么做，但是It works. 我已经去layui的社区问了，希望能有人帮我解惑。    
    

