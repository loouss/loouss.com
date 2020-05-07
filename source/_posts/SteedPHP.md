---
title: SteedPHP
date: 2020-05-07 23:26:37
tags:
---


## 生命周期

生命周期依赖于swoole 生命周期

## 服务容器

对象无状态原则，以单例模式存储对象。

##### 绑定对象

- 单例绑定
- 绑定接口到实现
- 绑定到匿名函数

##### 解析对象

- 单例直接解析一次



#### 容器服务实现 PSR-11 接口

 如果无法解析给定的标识符，则将会引发异常。

>  未绑定标识符时，会抛出 ***Psr\Container\NotFoundExceptionInterface*** 异常。
>
>  如果标识符已绑定但无法解析，会抛出 ***Psr\Container\ContainerExceptionInterface*** 异常。