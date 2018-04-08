---
layout: post
title:  "Swift基础之析构器"
date:   2018-04-09 00:30:00 +0800
categories: Swift
---
###### 关键词
`deinit`

### 析构过程原理
析构器只适用于类类型。当一个类的实例被释放之前，析构器会被立即调用。
* 析构器不是必需的，若在类释放前没有特殊清理操作要做，可以不定义析构器。
* 每个类最多只能有一个析构器，且析构器不带任何参数。
* 析构器是在类实例释放发生前被自动调用的，不能人为主动调用析构器。
* 在继承体系中，子类会继承父类的析构器。若子类不提供自己的析构器，则在子类实例释放前会自动调用父类的析构器。若子类提供自己的析构器，则在子类析构器的最后，会自动调用父类的析构器。

```swift
class SomeClass {
    deinit {

    }
}
```

# 参考文档
- [The Swift Programming Language - Language Guide - Deinitialization][swift-deinitialization]

[swift-deinitialization]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Deinitialization.html