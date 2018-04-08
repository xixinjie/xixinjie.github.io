---
layout: post
title:  "Swift基础之嵌套类型"
date:   2018-04-09 00:30:00 +0800
categories: Swift
---
###### 简介
可以在支持的类型内部定义嵌套的枚举、类或结构体。

#### 示例：在结构体中定义内嵌枚举
```swift
struct BlackjackCard {
    // 嵌套枚举
    enum Suit: Character {
        case Spades = "♠", Hearts = "♡", Diamonds = "♢", Clubs = "♣"
    }

    let suit: Suit
}
```

# 参考文档
- [The Swift Programming Language - Language Guide - Nested Types][swift-nested-types]

[swift-nested-types]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/NestedTypes.html