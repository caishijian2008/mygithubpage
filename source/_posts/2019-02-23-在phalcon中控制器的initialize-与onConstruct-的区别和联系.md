---
title: 在phalcon中控制器的initialize()与onConstruct()的区别和联系
date: 2019-02-23 18:47:08
updated: 2019-02-23 18:47:08
tags: phalcon
categories: php
---

可参考：https://segmentfault.com/a/1190000008665717

执行顺序是先 onConstruct() 函数，后 initialize() 函数。
onConstruct() 是实例化对象的过程，相当于 new
initialize() 是初始化资源的过程。如加载DI中注册的所有服务
