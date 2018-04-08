---
layout: post
title:  "Swift基础之流程控制"
date:   2018-04-09 00:30:00 +0800
categories: Swift
---
###### 关键词
`for-in`    `while`    `guard-else`

### `for-in`循环
可以用`for-in`循环遍历一个集合里面的所有元素，如由数字表示的区间、数组中的元素、字符串中的字符

* 遍历数字区间

```swift
for index in 1...5 {
    print("index: \(index)")
}

for index in 1..<5 {
    print("index: \(index)")
}
```

* 遍历数组

```swift
let names = ["Alex", "Jay", "Bob"]
for name in names {
    print("name: \(name)")
}
```

* 遍历字典

```swift
let students = ["name": "Jay", "gender": "male", "age": "30"]
for (key, value) in students {
    print("\(key): \(value)")
}
```

### While 循环

### 条件语句 if

### 控制转移语句 switch

###### 匹配区间
case中的模式可以为一个区间

###### 匹配元组

###### where
case中的模式可以使用where来添加额外的限制条件

###### 必须显式贯穿 fallthrough

###### 值绑定
可以将case中的模式匹配到的值绑定到一个临时的常量或变量，从而可以在case分支内使用。

### 提前退出`guard-else`
guard后的条件满足时，则执行guard后的代码。条件不满足时，执行else中的代码。
在else中可以通过以下方式之一来提前退出：
* return
* return someValue
* break
* continue
* throw
* 调用一个不返回的方法或函数

```swift
func someFunction(a: Int) {
    guard a > 0 else {
        return
    }

    print(a + 1)
}
```

```swift
func testGuard(a: Int) -> Int {
    guard a >= 0 else {
        return 0
    }

    return a + 1
}
```

```swift
let names = ["Alex", "Abra", "Bob", "Cat"]

for name in names {
    guard name.hasPrefix("A") else {
        break
    }

    print("A-prefixed names: \(name)")
}

// 打印所有以"A"开头的名字
for name in names {
    guard name.hasPrefix("A") else {
        continue
    }

    print("Name: \(name)")
}
```

```swift
enum LoginError: ErrorType {
    case InvalidUserName, InvalidPassword
}

func login(userName: String, password: String) throws {
    guard userName.characters.count > 0 else {
        throw LoginError.InvalidUserName
    }
    
    guard password.characters.count > 0 else {
        throw LoginError.InvalidPassword
    }
    
    print("Start to Login ...")
}

do {
    try login("a", password: "")
} catch LoginError.InvalidUserName {
    print("invalid username")
} catch LoginError.InvalidPassword {
    print("invalid password")
}
```
### 检测 API 可用性

# 参考文档
- [The Swift Programming Language - Language Guide - Control Flow][swift-control-flow]

[swift-control-flow]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html