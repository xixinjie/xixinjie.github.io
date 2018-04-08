---
layout: post
title:  "Swift基础之运算符"
date:   2018-04-09 00:30:00 +0800
categories: Swift
---
###### 关键词
`+=`    `==`    `!=`    `>`    `<`    `>=`    `<=`    `===`    `!==`    `a ? b : c`    `a ?? b`    `a...b`    `a..<b`

### 赋值运算符
* `=` 如`a = b` 表示将b的值赋给a

### 算术运算符
* 加法 `+`
* 减法 `-`
* 乘法 `*`
* 除法 `/`
* 求余 `%`
* 自增 `++`，前自增先返回原值再自增，后自增先自增然后返回加一后的新值
* 自减 `--`，前自减先返回原值再自减，后自减先自减然后返回减一后的新值
* 负号 `-`
* 正号 `+`，不做任何改变地返回原值

```swift
2 + 3    // 5
5 - 3    // 2
2 * 3    // 6
10.0 / 2.5    // 4.0
9 % 4    // 1
9 % -4    // 1
-9 % 4    // -1
8 % 2.5    // 0.5

"Hello" + " World"    // "Hello World"

let three = 3
let minusThree = -three    // -3
let plusThree = -minusThree    // +3

let minusSix = -6
let alsoMinusSix = +minusSix    // -6
```

### 组合运算符
* `+=` 如`a += 2`等价于`a = a + 2`

### 比较运算符
* 等于 `==`
* 不等于 `!=`
* 大于 `>`
* 小于 `<`
* 大于等于 `>=`
* 小于等于 `<=`
* 恒等 `===` 判断两个对象是否引用同一个对象实例。
* 不恒等 `!==` 

### 条件运算符
`a ? b : c`为条件运算符，若条件a成立则返回b，若条件a不成立则返回c

```swift
let contentHeight = 40
let hasHeader = true

let rowHeight = contentHeight + (hasHeader ? 50 : 20)    // 90
```

### 空合运算符
`a ?? b`为空合运算符，若可选值a为nil则返回默认值b，若非nil则返回a!。其中：
* a 必须为一个可选类型
* b 必须与 a 存储值的类型保持一致
注：空合运算符其实是条件运算符的简化。`a ?? b` 等价于 `(a != nil) ? a! : b`

```swift
let defaultColorName = "red"
var userDefinedColorName: String?    // 默认值为nil

var colorNameToUse = userDefinedColorName ?? defaultColorName    //    "red"，因为??前的可选值为nil，所以取后者

userDefinedColorName = "Green"
colorNameToUse = userDefinedColorName ?? defaultColorName    //    "Green"，因为??前的可选值不为nil，所以取解包后的值
```

### 区间运算符
`...`为闭区间运算符，如`a...b`表示[a, b]
`..<`为半开区间运算符，如`a..<b`表示[a, b)

```swift
for index in 1...5 {
    print("index: \(index)")
}

let nams = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..<count {
    print("第 \(i + 1) 个人叫 \(names[i])")
}
```

### 逻辑运算符
* 逻辑与 `a && b` 若布尔值a和b同时成立则返回true，否则返回false
* 逻辑或 `a || b` 表示布尔值a或b只要其中一个为true就返回true，若两者均为false则返回false
* 逻辑非 `!a` 表示对布尔值a取反，若a为true则返回false，若a为false则返回true

# 参考文档
- [The Swift Programming Language - Language Guide - Basic Operators][swift-basic-operators]
- [The Swift Programming Language - Language Guide - Advanced Operators][swift-advanced-operators]

[swift-basic-operators]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/BasicOperators.html

[swift-advanced-operators]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html