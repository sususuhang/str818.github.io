---
layout: article
title: Java 语言基础
tags: Java

lang: zh-Hans
key: Java_Basic
pageview: true
toc: true
show_subscribe: false
---

### 标识符

数字、字母、下划线和美元符号($)组成，不能以数字开头，区分大小写。关键字不能作为标识符。

### 参数传递

Java 的参数是以值传递的形式传入方法中，而不是引用传递。

在将一个参数传入一个方法时，本质上是将对象的地址以值的方式传递到形参中。

### 代码块

1. 局部代码块
在方法中出现，限定变量生命周期，及早释放，提高内存利用率。
2. 构造代码块
在类中方法外出现，多个构造方法中相同的代码存放到一起，每次调用构造方法都执行，并且在构造方法前执行。
3. 静态代码块
在类中方法外出现，并加上 static 修饰。用于对类进行初始化，在加载的时候就执行，并且只执行一次。
4. 同步代码块

执行顺序：
1. 静态代码块：随着类加载而加载，且只执行一次。
2. 构造代码块：每创建一个对象就会执行一次，优先于构造方法执行。
3. 构造方法：没创建一个对象就会执行一次。