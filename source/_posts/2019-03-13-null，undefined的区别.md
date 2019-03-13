---
title: null，undefined的区别
date: 2019-03-13 20:36:50
updated: 2019-03-13 20:36:50
tags: JavaScript
categories: JavaScript
---


null是一个表示“空”的对象，转为数值时为0；
undefined是一个表示"此处无定义"的原始值，转为数值时为NaN。
如果 JavaScript 预期某个位置应该是布尔值，会将该位置上现有的值自动转为布尔值。
转换规则是除了下面六个值被转为false，其他值都视为true：
undefined
null
false
0
NaN
""或''（空字符串）

来源于：http://teadocs.cn/docs/javascript/types/null-undefined-boolean.html
