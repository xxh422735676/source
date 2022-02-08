---
title: 【c++语法基础】decltype关键字
author: MoonXu
description: 懒癌犯了，摘要同标题~
comments: true
sticky: '0'
tags:
  - cpp语法
categories:
  - cpp
  - cpp语法
password: ''
abbrlink: 40002
date: 2022-02-08 18:23:25
---

### decltype关键字

==decltype==被称作类型说明符，作用是选择并返回操作数的数据类型。

```cpp
// sum类型就是函数f返回的类型
decltype(f()) sum = x;
```

解决难以拼写的类型名，有以下两个方案：

1. 使用[类型别名]()技术
2. 使用[auto]()和decltype

### 工作原理

==decltype==不会计算表达式的值，编译器分析表达式并得到它的类型。

函数调用也算一种表达式，因此不必担心在使用deltype时执行了函数。

### decltype+变量

根据[表达式的定义]()，单独使用一个变量，相当于一个最简单的表达式。

