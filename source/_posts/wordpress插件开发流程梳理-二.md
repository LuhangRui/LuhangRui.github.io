---
title: wordpress插件开发流程梳理(二)插件最佳实践
tags: wordpress
category: 厚积薄发
thumbnail: /images/postsimages/wordpress.jpg
abbrlink: 45909
date: 2019-05-22 10:55:41
---

### 开发插件的最佳实践

## 避免命名冲突
当您的插件对变量，函数或类使用相同的名称作为另一个插件时，会发生命名冲突。

幸运的是，您可以使用以下方法避免命名冲突。

## 程序性
默认情况下，所有变量，函数和类都在全局命名空间中定义，这意味着您的插件可以覆盖由另一个插件设置的变量，函数和类，反之亦然。在函数或类中定义的变量不受此影响。

## 前缀一切  
所有变量，函数和类都应以唯一标识符为前缀。前缀可防止其他插件覆盖您的变量并意外调用您的函数和类。它也会阻止你做同样的事情。

## 检查现有实现  
PHP提供了许多函数来验证变量，函数，类和常量的存在。如果实体存在，所有这些都将返回true。

- 变量：  isset()  （包括数组，对象等）
- 函数：  function_exists()
- 类：  class_exists()
- 常量：defined()

## OOP面向对象

解决命名冲突问题的一种更简单的方法是使用类来获取插件的代码。

您仍然需要检查是否已经使用了您想要的类的名称，但其余的将由PHP处理。

示例：

```php
<?php
if ( !class_exists( 'WPOrg_Plugin' ) ) {
    class WPOrg_Plugin
    {
        public static function init() {
            register_setting( 'wporg_settings', 'wporg_option_foo' );
        }
 
        public static function get_foo() {
            return get_option( 'wporg_option_foo' );
        }
    }
 
    WPOrg_Plugin::init();
    WPOrg_Plugin::get_foo();
}
```
## 目录结构：

```
/plugin-name
     plugin-name.php //插件主文件，必须和文件夹名保持一致
     uninstall.php //卸载插件时自动运行的脚本
     /languages //国际化语言包
     /includes //其他功能脚本
     /admin
          /js
          /css
          /images
     /public
          /js
          /css
          /images
```
## 条件加载

将管理代码与公共代码分开是有帮助的。使用条件is_admin()。

示例：
```php
<?php
if ( is_admin() ) {
    // we are in admin mode
    require_once( dirname( __FILE__ ) . '/admin/plugin-name-admin.php' );
}
```

## 文件结构参考

+ [单个插件文件，包含函数](https://github.com/GaryJones/move-floating-social-bar-in-genesis/blob/master/move-floating-social-bar-in-genesis.php)
+ [单个插件文件，包含类，实例化对象和可选功能](https://github.com/norcross/wp-comment-notes/blob/master/wp-comment-notes.php)
+ [主插件文件，然后是一个或多个类文件](https://github.com/DevinVinson/WordPress-Plugin-Boilerplate)

## 参考模板

对于您编写的每个新插件，您可能希望从样板开始，而不是从头开始。使用样板的一个优点是在您自己的插件之间保持一致。如果使用他们熟悉的样板，使用模板也可以让其他人更容易为您的代码做出贡献。

这些也可作为不同但可比较的架构的进一步示例。

+ [WordPress插件Boilerplate](https://github.com/DevinVinson/WordPress-Plugin-Boilerplate)
