---
layout: article
title: Spring — 控制反转 IoC
tags: Spring

lang: zh-Hans
key: Spring_IOC
pageview: true
toc: true
show_subscribe: false
---

## 一、基本概念

### 1. IoC 是什么

IoC - Inversion of Control，即「控制反转」，不是什么技术，而是一种设计思想。

在 Java 开发中，IoC 意味着将你设计好的对象交给容器控制，而不是传统的在对象内部直接控制。

- **谁控制谁，控制什么：** 传统 Java SE 程序设计，我们直接在对象内部通过new进行创建对象，是程序主动去创建依赖对象；而 IoC 是有专门一个容器来创建这些对象，即由 IoC 容器来控制对象的创建；谁控制谁？当然是 IoC 容器控制了对象；控制什么？那就是主要控制了外部资源获取（不只是对象包括比如文件等）。

- **为何是反转，哪些方面反转了：** 有反转就有正转，传统应用程序是由我们自己在对象中主动控制去直接获取依赖对象，也就是正转；而反转则是由容器来帮忙创建及注入依赖对象；为何是反转？因为由容器帮我们查找及注入依赖对象，对象只是被动的接受依赖对象，所以是反转；哪些方面反转了？依赖对象的获取被反转了。

传统的程序设计如下图所示，都是主动去创建相关对象然后再组合起来。

<div align="center">  <img src="/img/spring_ioc_1.png" width="50%"/> </div><br>


当有了 IoC 容器后，在客户端类中不再主动去创建这些对象了。

<div align="center">  <img src="/img/spring_ioc_2.png" width="50%"/> </div><br>

### 2. IoC 能做什么

IoC 不是一种技术，只是一种思想，一个重要的面向对象编程的法则，它能指导我们如何设计出松耦合、更优良的程序。传统应用程序都是由我们在类内部主动创建依赖对象，从而导致类与类之间高耦合，难于测试；有了IoC容器后，把创建和查找依赖对象的控制权交给了容器，由容器进行注入组合对象，所以对象与对象之间是 松散耦合，这样也方便测试，利于功能复用，更重要的是使得程序的整个体系结构变得非常灵活。

其实 IoC 对编程带来的最大改变不是从代码上，而是从思想上，发生了“主从换位”的变化。应用程序原本是老大，要获取什么资源都是主动出击，但是在 IoC/DI 思想中，应用程序就变成被动的了，被动的等待 IoC 容器来创建并注入它所需要的资源了。

### 3. DI 是什么

DI — Dependency Injection，即「依赖注入」，组件之间依赖关系由容器在运行期决定，形象的说，即由容器动态的将某个依赖关系注入到组件之中。依赖注入的目的并非为软件系统带来更多功能，而是为了提升组件重用的频率，并为系统搭建一个灵活、可扩展的平台。通过依赖注入机制，我们只需要通过简单的配置，而无需任何代码就可指定目标需要的资源，完成自身的业务逻辑，而不需要关心具体的资源来自何处，由谁实现。

理解 DI 的关键是：“谁依赖谁，为什么需要依赖，谁注入谁，注入了什么”：

- 谁依赖于谁：当然是应用程序依赖于 IoC 容器；

- 为什么需要依赖：应用程序需要 IoC 容器来提供对象需要的外部资源；

- 谁注入谁：很明显是 IoC容器注入应用程序某个对象，应用程序依赖的对象；

- 注入了什么：就是注入某个对象所需要的外部资源（包括对象、资源、常量数据）。

IoC 和 DI 由什么关系呢？其实它们是同一个概念的不同角度描述，由于控制反转概念比较含糊（可能只是理解为容器控制对象这一个层面，很难让人想到谁来维护对象关系），所以 2004 年大师级人物 Martin Fowler 又给出了一个新的名字：“依赖注入”，相对 IoC 而言，“依赖注入”明确描述了“被注入对象依赖 IoC 容器配置依赖对象”。

## 二、源码相关参考

[零基础带你看Spring源码——IOC控制反转](https://zackku.com/spring-ioc/)

## 参考

- [谈谈对Spring IOC的理解](https://www.cnblogs.com/xdp-gacl/p/4249939.html)