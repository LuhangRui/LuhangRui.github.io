---
title: 如何修改layer/layui中的confirm
tags: layui
category: 厚积薄发
thumbnail: /images/postsimages/layui.png
abbrlink: 45548
date: 2018-11-29 13:12:16
---

> 需求：

![原始图](https://kengdie.oss-cn-shanghai.aliyuncs.com/QQ01.png)改成![修改后](https://kengdie.oss-cn-shanghai.aliyuncs.com/QQ02.png)

> 背景：

这个confirm是layui中的layer弹出框，要想修改这个弹出框的内容岂不是要去修改源码？当我在源码里扒拉半天梳理好了逻辑之后，突然意识到，其实我本可以不必这么麻烦的，直接找到这个弹窗append元素进去就不就完了么？卧槽！
所以我在代码里加了一行注释 **浪费在这里的时间=2h**

> 10分钟可以搞定的解决方案：

```javascript
    var option = {1:'筛选项1',2:'筛选项2'};
    var optionString = '';
    for(let i in option) {
        optionString =optionString+'<option value="'+i+'">'+option[i]+"</option>";
    }
    //一定要给加入的元素加入id，提交的时候取值方便
    $(".layui-layer-content").append("<br/><select id='reinvest_type'>"+optionString+"</select>");


```

> 结语 

#### 尽可能的不要犯蠢吧，有些问题并不像看上去那么难。

当然修改源码也是可以的，而且还能做成一个通用方案，不过相当耗时。问题是没必要，整个项目就一处需要这样干，我们尽可能选择效率最高的那种方案。
