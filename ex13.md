# 练习 13：文档：Google

> 原文：[Exercise 13. Documentation: Google](https://archive.fo/8kvYG)

> 译者：[飞龙](https://github.com/wizardforcel)

> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)

> 自豪地采用[谷歌翻译](https://translate.google.cn/)

## 文档搜索简介

现在你知道了如何使用 Linux 在线文档，我会告诉你：“Linux 在线文档是好的，但它还不够。”这意味着如果你已经熟悉了某个特定程序的工作原理，那么手册页很有用，但是当你没有时它们就没有帮助。

为了让自己起步，你需要阅读一本书，或者找到一个允许你开始的小秘籍，这被称为“如何做”。例如，要开始使用 Apache Web 服务器，你可能需要使用“如何使用 Apache”。没关系，这就是谷歌的意义，但现在我会给你一个大警告：

> 不要盲目遵循任何“如何做”，永远不要！

使用 Google 的正确方法是：

+   找到一个“如何做”。
+   遵循它，但阅读，或至少浏览所有手册页，来了解你不了解的程序。另外，请阅读“如何做”中所有未知选项。这是非常重要的。

## 实用资源的列表

有时最好是搜索特定网站，而不是盲目地将内容输入 Google。这是有用资源的列表：

+   <http://en.wikipedia.org> 是非常有价值的，当你获取某些主题的初始信息的时候。其链接部分更是无价之宝。
+   <http://stackexchange.com/> 这是非常有用的网站，用于查找使用示例和用例的信息。StackExchange 网络包括几个资源，其中最有用的是 <http://serverfault.com/> 和 <http://unix.stackexchange.com/>。 当你编写 bash 脚本时，<http://stackoverflow.com/> 是一个非常有用的资源。
+   <http://www.cyberciti.biz/> 包含许多有用的“如何做”和例子。
+   许多程序的主页提供了良好的，有时是优秀的文档。例如 Apache 和 ngnix，分别为：<http://httpd.apache.org/docs/>， <http://nginx.org/en/docs/>。
+   <http://tldp.org/> 是 Linux 文档项目，包含许多不同主题的深入指南。

## 搜索小提示

Google 有一种查询语言，可以让你执行强大的查询。这是这种语言的主要命令：

+   `(screen|tmux) how to` - 同时搜索`screen`和`tmux`的“如何做”。记得 shell 参数的扩展嘛？这是相似的。
+   `site:serverfault.com query` - 仅在这个网站上搜索。你可以使用`(site:serverfault.com | site:stackexchange.com)`，一次性搜索多个站点。
+   `"..."` - 仅显示包含此查询的那些页面。
+   `-query` - 从搜索结果中排除某些内容。
