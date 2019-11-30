---
title: "Swift基础之方法"
date: 2018-04-09T00:30:59+08:00
draft: true
---

### 10.1 实例方法

###### 10.1.1 方法的局部参数名和外部参数名

###### 10.1.2 修改方法的外部参数名

###### 10.1.3 self属性

###### 10.1.4 在实例方法中修改值类型

###### 10.1.5 在可变方法中给self赋值

### 10.2 类型方法
声明class的类型方法时，用`class`关键字；声明struct和enum的类型方法时，使用`static`关键字。
调用类型方法时，使用`类型名.类型方法`格式。

```swift
class SomeClass {
    class func someFunction() {
        print("class的类型方法")
    }
}

struct SomeStruct {
    static func someFunction() {
        print("struct的类型方法")
    }
}

enum SomeEnum {
    static func someFunction() {
        print("enum的类型方法")
    }
}

SomeClass.someFunction()
SomeStruct.someFunction()
SomeEnum.someFunction()
```

# 参考文档
- [The Swift Programming Language - Language Guide - Methods][swift-methods]

[swift-methods]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Methods.html