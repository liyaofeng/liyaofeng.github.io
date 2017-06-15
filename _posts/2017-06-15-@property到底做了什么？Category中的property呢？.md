---
layout:     post
title:      "@property到底做了什么？Category中的property呢？"
subtitle:   ""
date:       2017-06-15 11:31:45 +0800
author:     "Liyf"
header-img: "img/161012_1.jpg"
catalog:    true
tags: 
    - iOS
---
---
#### @synthesize
    @synthesize xxx;
    //自动生成并实现 getter 和 setter 方法，但是不会生成以下划线开头 _xxx 成员变量，代码中使用 _xxx 报错

    @synthesize xxx = _xxx;
    //自动生成并实现 getter 和 setter 方法，并且生成 xxx 的指定成员变量 _xxx
>- 编译器默认添加的为 <code>@synthesize xxx = _xxx;</code>
- 如果添加了 <code>@synthesize xxx;</code> 不会再默认添加 <code>@synthesize xxx = _xxx;</code> 
- 如果手动实现了 getter 和 setter 方法，编译器不自动添加<code>@synthesize xxx = _xxx;</code>
- 如果没有手动添加并且阻止了编译器默认添加的<code>@synthesize xxx = _xxx;</code> **不能使用 _xxx 变量**

---
#### @dynamic
    @dynamic xxx;
    //告诉编译器不自动生成 getter 和 setter 方法，由开发者实现
>- <code>@dynamic xxx;</code> 和 <code>@synthesize xxx;</code> 不能同时存在
- 必须手动生成 getter 和 setter 方法

---
#### 声明属性是指定 getter 和 setter 方法
    @property (strong, nonatomic, getter=getMethod, setter=setMethod:) NSString *xxx;
    //修改默认的 getter 和 setter
>- 编译器同样会默认添加了<code>@synthesize xxx = _xxx;</code> 可不必去实现 <code>getMethod</code> 和 <code>setMethod:</code>

---
***
#### Category 中的 property
1. oc 中不允许类在确定后添加成员变量，所以类别中不允许添加成员变量
- 类别中可以添加成员属性，但是该属性要手动实现 getter 和 setter 方法

>- 因为类别中不允许添加成员变量，所以并不会自动添加 <code>@synthesize xxx = _xxx;</code>
- 因为编译器自动生成的 getter 和 setter 方法中使用了指定的成员变量，所以并不会自动实现 getter 和 setter 方法
- 类别中的成员属性没有自动实现 getter 和 setter 方法，通过 <code>.</code> 语法调用会出错

#### 在 Category 中添加 property 的正确姿势
- 手动实现 getter 和 setter 方法
- getter 和 setter 方法中操作其他已经定义的属性：


    @interface UIImageView (SICornerImageView)
    @property (assign, nonatomic) CGFloat height;
    @end

    @implementation UIImageView (SICornerImageView)
    - (CGFloat)height {
        return self.bounds.size.height;
    }
    - (void)setHeight:(CGFloat)height {
        CGRect rect = self.bounds;
        rect.size.height = height;
        self.bounds = rect;
    }
    @end
- 使用 Runtime 添加关联对象