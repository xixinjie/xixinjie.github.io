---
layout: post
title:  "Swift基础之函数"
date:   2018-04-09 00:30:00 +0800
categories: Swift
---
### 函数定义与调用

### 函数参数与返回值

###### 无返回值函数

###### 多重返回值函数

###### 可选元组返回类型


### 函数参数名称

###### 无参函数
```swift
func sayHello() {
    print("Hello.")
}

sayHello()    // 输出"Hello"
```

###### 多参数函数
函数可以有一个或多个固定个数的参数。
注：第一个参数不需要外部参数名，其余的需要外部参数名。
```swift
func sayHello(name) {
    print("Hello \(name)")
}

func sayHello(name, message msg: String) {
    print("Hello \(name) \(msg)")
}

sayHello("Jay")
sayHello("Jay", message: "Welcome!")
```

###### 可变参数

###### 外部参数名与局部参数名

###### 默认参数值

###### 常量参数和变量参数

###### 输入输出参数

### 函数类型

###### 使用函数类型

###### 函数类型作为参数类型

###### 函数类型作为返回类型


### 嵌套函数
嵌套函数是指可以在一个函数内部定义另外一个函数。
```swift
func returnFifteen() -> Int {
    var y = 10

    func add() {
        y += 5
    }

    add()
    return y
}

var number = returnFifteen()    // 15
```

# 参考文档
- [The Swift Programming Language - Language Guide - Functions][swift-functions]

[swift-functions]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Functions.html