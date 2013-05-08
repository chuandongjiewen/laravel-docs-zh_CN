# 向Laravel贡献

- [介绍](#introduction)
- [拉取请求](#pull-requests)
- [编码规范](#coding-guidelines)

<a name="introduction"></a>
## 介绍

Laravel是免费、开源的软件，这意味着任何人都可以参与到Laravele的开发。Laravel的源代码现在是托管在[Github](http://github.com)，利用Github可以很方便的fork和merge你贡献的东西。

<a name="pull-requests"></a>
## 拉取请求 (Pull Requests)

拉取请求分为特性请求和错误修复请求。在为一个新特性发起一个拉取请求之前，你应该先创建一个标题中含有`[Proposal]`的问题。proposal应该描述新的特性，就像描述一个已经实现的想法一样。proposal之后会被审核，proposal通过后，一个实现这个特性的拉取请求才可以被创建。没有按照这个指导发起的拉取请求将会被立即关闭。

针对错误修复的拉取请求无需先创建proposal问题即可发布，如果你认为你知道针对错误的修复方法，请留下评论详细描述下你的修复方法。

### 特性请求

如果你有一个想法，想让Laravel拥有某个特性，你可以在Github上创建一个标题中包含`[Request]`的问题，所有的问题将会由一个核心开发人员审阅。

<a name="coding-guidelines"></a>
## 编码规范

Laravel遵循[PSR-0](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md) and [PSR-1](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md)编码规范，除了这两个规范，下面是其它一些应该遵循的编码规范。

- 命名空间的声明应该和`<?php`同一行
- 类的开始括号`{`应该和类名在同一行
- 函数和控制结构的开始括号`{`应该在新的一行
- 所有接口的名称要有`Interface`后缀 （如`FooInterface`）
