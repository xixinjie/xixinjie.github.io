---
layout: post
title:  "Swift基础之字符串与字符"
date:   2018-04-09 00:30:00 +0800
categories: Swift
---
### 初始化空字符串
可以通过字面量`""`或构造器`String()`创建空字符串，然后可以通过String实例的isEmpty属性判断字符串是否为空
```swift
let emptyStrinig = ""
let anotherEmptyString = String()

if emptyString.isEmpty {    // true
}

if anotherEmptyString.isEmpty {    // true
}
```

### 字符串的可变性
通过`let`声明的字符串变量不可以修改，通过`var`声明的字符串变量可以修改。
```swift
let constantString = "Hello"    // 不可修改
var variableString = "Hello"    // 可以修改
variableString += “ World!"    // 此时variableString的值变为"Hello World!"
```

### 字符串是值类型


### 遍历字符串中的每个字符
```swift
let constantString = "Hello"
for character in constantString.characters {
    print("\(character)")
}
// H
// e
// l
// l
// o
```

### 使用字符数组来初始化字符串
```swift
let characters: [Character] = ["H", "e', "l", "l", "o"]
let constantString = String(characters)    // "Hello"
```

### 拼接字符串
```swift
var firstString = "Hello"
var secondString = " World"

// 使用 + 连接两个字符串    
var thirdString = firstString + secondString    // "Hello World"

// 使用 += 来连接字符串
firstString += " Our "

// 使用append方法将一个字符添加到字符串尾部
let character: Character = "A"
let someString = "Hello"

someString.append(character)    // "HelloA"
```

### 字符串插值（拼接字符串）
字符串插值用于构建新字符串，可以在其中包含常量、变量、字面量和表达式。格式为：`\(表达式)`
```swift
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"    // "3 times 2.5 is 7.5"
```

### 计算字符串中字符的个数
```swift
let someString = "Hello"
let count = someString.characters.count    // 5
```

### 字符串索引
* `startIndex`属性可以获取字符串的第一个字符的索引
* `endIndex`属性可以获取字符串的最后一个字符的下一个位置的索引
* 通过调用`String.Index`的`predecessor()`方法可以获取当前Index的前一个索引
* 通过调用`String.Index`的`successor()`方法可以获取当前索引的后一个索引
* 通过`advancedBy(_:)`方法可以获取指定位置上的索引。

```swift
//    通过索引方式遍历字符串中的所有字符
let someString = "Hello"
for index in someString.characters.indices {
    print(\(someString[index]))
}
```

### 插入与删除

### 比较字符串
###### 相等/不相等
使用`==`比较相等，使用`!=`比较不相等。
```swift
let stringA = "Hello"
let stringB = "Hello"
let stringC = "World"

stringA == stringB    // true
stringA == stringC    // false
```

###### 前缀/后缀相等
使用`hasPrefix(_:)`判断两个字符串是否有相同前缀，使用`hasSuffix(_:)`判断两个字符串是否有相同后缀
```swift
// 判断人名中姓李的人数
let names = [
    "张三",
    "李四",
    "王五",
    "赵六",
    "李明"
]
let count = 0
for name in names {
    if name.hasPrefix("李") {
        count += 1
    }
}
print("姓李的共： \(count)人")
```

### 字符串的Unicode表示
使用utf8属性
使用`utf16`属性来访问字符串的UTF-16表示
```swift
let dogString = "Dog!"
for codeUnit in dogString.utf8 {
    print("\(codeUnit) ", terminator: "")
}
```

# 参考文档
- [The Swift Programming Language - Language Guide - Strings and Characters][swift-strings-and-characters]

[swift-strings-and-characters]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html