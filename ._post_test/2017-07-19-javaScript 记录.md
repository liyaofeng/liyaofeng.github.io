---
layout:     post
title:      "javaScript 记录"
subtitle:   ""
date:       2017-07-19 14:05:19 +0800
author:     "Liyf"
header-img: "img/"
catalog:    true
tags: 
    - 
---
字符串 属于 数组，数组 属于 对象，字符串和数组的属性为‘0，1，2...’

字符串、数组、对象都可以用‘[下标]’方式获取值，但只有对象可以通过‘.’获取属性，字符串、数组不能通过‘.0’方式获取

for in 遍历对象的属性，通过属性获取属性值

对象的属性只能是字符串，Map 数据类型的属性可以是任意类型，但是 Map 只能通过 get('属性') 获取属性值

Array、Map、Set 属于 iterable 类型

interable 类型可以使用 for of 遍历值，forEach() 遍历接受一个函数

---
函数可以传入任意个参数，通过默认的 arguments 获取，arguments 类似数组，但不是数组

es6 引入 rest 参数，function f(a, ...rest)，rest 前面有3点，内容为除了声明的参数外的所有传入的参数，和 arguments 类似，但是不包含已声明的参数

变量提升：声明的变量会提升到函数的开始，

