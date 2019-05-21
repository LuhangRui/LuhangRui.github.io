---
title: wordpress插件开发流程梳理
tags: wordpress
abbrlink: 5344
date: 2019-05-21 16:32:49
category: 开发日常
thumbnail: /images/postsimages/wordpress.jpg
---

### 1.声明一个插件

> 首先我们必须明白，wordpress的插件可以是单文件，也可以是多文件，css/html都不是必须的，以下举例暂且在单文件模式下

比如我们要创建一个名为 `hellophp`的插件，那我们就需要在`wp-content/plugins`目录下新建`hellophp`文件夹和`hellophp.php`文件，但是这还是不能让系统识别这个插件。插件的主文件名要和目录一致。

在wordpress中，要让系统识别一个插件，首先要做的就是，声明一个DOCBLOCK(文档块)

示例：

`wp-content/plugins/hellophp/hellophp.php`

```php

<?php
/**
 * Plugin Name: hellophp
 * Plugin URI:  https://example.com/plugins/the-basics/
 * Description: Basic WordPress Plugin Header Comment
 * Version:     0.0.0
 * Author:      WordPress.org
 * Author URI:  https://author.example.com/
 * License:     GPL2
 * License URI: https://www.gnu.org/licenses/gpl-2.0.html
 * Text Domain: wporg
 * Domain Path: /languages
 */

 //完整的示例是这样，但是除了Plugin Name其他都不是必须的

```

### 2.初始化插件 

* wordpress-hook

    * register_activation_hook( __FILE__, 'pluginprefix_function_to_run' ); //启用插件时的钩子
    * register_deactivation_hook( __FILE__, 'pluginprefix_function_to_run' );//停用插件时的钩子

示例：

示例摘自官方文档，启动插件钩子最常见的用途之一是当插件注册自定义帖子类型时刷新WordPress永久链接。这摆脱了令人讨厌的404错误。

```php
//启用插件时的处理
function pluginprefix_setup_post_type() {
    // register the "book" custom post type
    register_post_type( 'book', ['public' => 'true'] );
}
add_action( 'init', 'pluginprefix_setup_post_type' );
 
function pluginprefix_install() {
    // trigger our function that registers the custom post type
    pluginprefix_setup_post_type();
 
    // clear the permalinks after the post type has been registered
    flush_rewrite_rules();
}
register_activation_hook( __FILE__, 'pluginprefix_install' );

//停用插件时的处理
function pluginprefix_deactivation() {
    // unregister the post type, so the rules are no longer in memory
    unregister_post_type( 'book' );
    // clear the permalinks to remove our post type's rules from the database
    flush_rewrite_rules();
}
register_deactivation_hook( __FILE__, 'pluginprefix_deactivation' );
```

如果您不熟悉注册自定义帖子类型，请不要担心 - 稍后将对此进行介绍。使用这个例子只是因为它很常见。

注：这部分看不懂也没关系，这两个钩子也并不是必须的，如果你要做一些必要的前置工作，比如说启用插件时，创建一个新的数据表，初始化一些变量之类的前置操作来保证插件的正常运行，可以使用这个钩子，如果没有这样的操作，这个钩子不用也可以。

### 3.插件的卸载

从站点卸载插件时，您的插件可能需要进行一些清理。

如果用户已停用插件，则会认为已卸载插件，然后单击WordPress管理中的删除链接。

卸载插件时，您需要清除插件和/或其他数据库实体（如表）的任何插件选项和/或设置。

卸载插件有两种方法：

*  系统的钩子函数
```php
register_uninstall_hook(__FILE__, 'pluginprefix_function_to_run');
```
* 自定义卸载脚本`uninstall.php`

你需要在你插件的根目录创建一个`uninstall.php`文件

示例：

示例脚本演示了一个删除自定义表的清理工作。

```php
// if uninstall.php is not called by WordPress, die
// 防止误操作直接访问该文件
if (!defined('WP_UNINSTALL_PLUGIN')) {
    die;
}
 
$option_name = 'wporg_option';
 
 //这两行是删除插件的一些附加配置，后边我会继续说这个
delete_option($option_name);
 
// for site options in Multisite
delete_site_option($option_name);
 
// drop a custom database table
global $wpdb;
$wpdb->query("DROP TABLE IF EXISTS {$wpdb->prefix}mytable");

```

这是wordpress插件开发流程梳理的第一篇，就先这样，后续会继续梳理。本文的示例和描述，大部分来自官方文档。