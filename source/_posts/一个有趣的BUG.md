---
title: 一个有趣的BUG
tags: fun
category: 有点意思
thumbnail: /images/postsimages/20181217.jpg
abbrlink: 25776
date: 2018-12-17 21:11:25
---

> 一个很有意思的Bug

某天测试同学再次向我反馈，你这个删除按钮虽然置灰了，但是还是可以点击啊？

我：？？？？（黑人问号）

卧槽？不可能啊，按钮都disabled了，怎么还可以点击？还能触发click事件？开玩笑的吧？，匆忙应付了测试同学开始复现这个Bug.

> 复现

重新写了个页面demo，开始测试，卧槽？复现不了啊，这尼玛。。。。？

![摸不着头脑](/images/postsimages/2018121701.jpg)

> 叮！事情の真相

没办法复现很烦啊，什么鬼？遂去原页面检查，然后发现了这样一段代码：

```html
<div class="btn-cancel">
    <button disabled>删除</button>
</div>
<script>
$(".btn-cancel").click(function(){
    //业务逻辑....
})

</script>

```

**卧槽，点击事件居然是绑定在div上的，由于button在div内部并且是disabled，button处于不可点击的状态，所以测试同学点击到的其实是div!!!**

真的是十分的有意思了。Funny!