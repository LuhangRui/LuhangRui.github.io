---
title: wordpress插件开发从创建一个新的菜单开始
tags: wordpress
category: 厚积薄发
thumbnail: /images/postsimages/wordpress.jpg
abbrlink: 18296
date: 2019-06-10 14:14:57
---

### 创建插件的目的

1.我们为什么要创建一个插件？
* IT界有一个知名的论调叫做**不要造重复的轮子**，如果有可能的话，你应该尽可能的从现有的网络资源上选择一个已有的插件来使用，而不是重新创造一个。它耗费的精力很可能是没有相应价值的。所以在创建一个插件之前，你应该先去wordpress的[插件仓库](https://wordpress.org/plugins/)搜索关键词，看一下是不是已经有了满足需求的插件。

2.你希望你的插件具有什么功能？
* 在开始开发之前，想好这个问题，能帮你省去很多麻烦。比如，你的插件是提供给前台页面使用的还是在后台页面使用的？是后台的独立模块么？它应该有哪几个页面，页面上应该有哪些功能？怎么去设计这个页面的布局？这些问题，都需要有一个清晰的规划。

### 前置工作

尽管我们在[梳理开发流程](/posts/5344.html)中已经提到了如何创建一个插件，但是我们还是要再说一次，以防有些读者没有看到。

1.打开WordPress安装目录下的`wp-content`目录。

2.打开`plugins`目录。

3.创建一个新目录并在插件后命名（例如`plugin-name`）。

4.打开新插件的目录。

5.创建一个新的PHP文件（例如，在插件后命名此文件也很好`plugin-name.php`）。

### 开始开发

就像下面的示例一样，你必须在你创建的主插件文件的**开头**加上一段`doc`注释来告诉wordpress这是个插件，当然也可以加上作者，邮箱等信息，下面只是简单示例，详细可以在我们梳理流程的那边博文/或者官网中看到。

```php
//wp-content/plugin-name/plugin-name.php
<?php
/**
 * Plugin Name: 插件名称
 */
function do_something_else()
{
    //.....你的代码
}
```

还记得我们在第一篇梳理中提到的三个基础插件钩子么？

1.register_activation_hook //启用插件时触发的钩子

2.register_deactivation_hook//禁用插件时触发的钩子

3.register_uninstall_hook//删除插件时触发的钩子

我们可以通过这三个钩子函数来做一下一些前置/后置的处理，比如说插件被启用时创建一个自定义数据表，初始化一些配置，禁用时恢复初始化设置，删除时删除自定义的数据表。

这里我们先不展开来讲我们在讲`OptionApi`的时候再讲这个。

准备工作都已经做好了，我们现在开始正式的开发。我们假设说我们要做的是一个额外的内容管理插件。那我们现在想要在后台创建一个`定制内容管理`菜单，该怎么做呢？

wordpress向我们提供了一个`add_menu_page`的函数：

```php
/**
    //我们先看一下函数的参数
    add_menu_page(
        string $page_title, //页面标题
        string $menu_title, //菜单名称
        string $capability, //权限级别
        string $menu_slug,  //菜单标识 唯一
        callable $function = '', //回调函数 其实就是点击这个菜单后触发的函数 我们可以返回一个页面
        string $icon_url = '', //图标，可以为空
        int $position = null //位置  决定了菜单应该插入在第几个
    );
**/
```
那我们应该怎么使用呢？

> for-example :

你的`wp-content/plugin-name/plugin-name.php`文件，看起来应该像这样：
```php
<?php
/*
    Plugin Name: 定制内容管理
    Plugin URI: http://ergou.fun
    Description: 内容管理模块（自定义内容非posts）
    Version: 1.0
    Author: ergou
    Author URI: http://ergou.fun

    Copyright 2019  ergou  (email : 531432012@qq.com)

    This program is free software; you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation; either version 2 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program; if not, write to the Free Software
    Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301  USA
*/

/** 第1步：创建自定义菜单的函数**/
function ergou_cms_plugin_menu()
{
    add_menu_page('定制内容管理', '内容管理', 'manage_options', 'ergou-cms-manager', 'ergou_cms_plugin_options', '', 7);

}

/** 第2步：将函数注册到钩子中 */
add_action('admin_menu', 'ergou_cms_plugin_menu');

/** 第3步：定义选项被点击时打开的页面 */
function ergou_cms_plugin_options()
{
    if (!current_user_can('manage_options')) {
        wp_die(__('You do not have sufficient permissions to access this page.'));
    }
    //include_once(plugin_dir_path(__FILE__) . 'detail/index.php');

    //也可以直接返回HTML，不过我建议是额外放一个文件，这样以后维护起来好处理

    //你可以直接 echo "hello world"
    echo "Hello World";
    wp_die();
}


```

我们现在回到我们的后台管理页面点击`插件管理`你会发现多了一个，定制内容管理的菜单，点击启用。菜单就会增加在左侧顶级菜单里。点击`定制内容管理`菜单，页面输出了`"Hello World"`。至此，我们算是完成了第一步。

本篇内容就是这些。That's all .Thank you .
