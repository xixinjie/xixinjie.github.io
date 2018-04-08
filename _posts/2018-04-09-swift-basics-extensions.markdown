---
layout: post
title:  "Swift基础之扩展"
date:   2018-04-09 00:30:00 +0800
categories: Swift
---
###### 概述
扩展就是为一个已有的类class、结构体struct、枚举enum或者协议protocol这四种类型添加新功能。可以为其添加以下属性或方法：
* 添加计算型实例属性和计算型类型属性 (不能通过扩展为其添加存储型属性)
* 添加实例方法和类型方法
* 添加新的构造器
* 添加下标
* 添加新的嵌套类型
* 使一个已有类型符合某个协议

### 语法
根据扩展的功能是否遵从于某个协议，扩展语法归结为以下两种类型：

###### 扩展一般的新功能
```swift
extension SomeType {
    // 为SomeType添加的新功能写在这里
}
```

###### 扩展的功能来自于某个协议
```swift
extension SomeType: SomeProtocol, AnotherProtocol {
    // 协议实现写在这里
}
```

### 具体扩展示例

###### 1. 在扩展中添加计算型实例属性或类型属性
> 扩展不可以添加存储型属性，也不可以为已有属性添加属性观察器

```swift
struct Document {
    var name: String
    var size: Int
}

extension Document {
    var sizeInMB: Int {
        return size / 1024 / 1024
    }
}
```

###### 2. 在扩展中添加构造器


###### 3. 在扩展中添加实例方法或类型方法

###### 4. 在扩展中添加下标

###### 5. 在扩展中添加嵌套类型：如嵌套枚举

# 参考文档
- [The Swift Programming Language - Language Guide - Extensions][swift-extensions]

[swift-extensions]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Extensions.html