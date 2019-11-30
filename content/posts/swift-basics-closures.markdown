---
title: "Swift基础之闭包"
date: 2018-04-09T00:30:59+08:00
draft: true
---

# 简介
闭包是引用类型。

# 闭包表达式
###### 完整格式
```swift
{ (parameters) -> returnType in
    statements
}
```
###### 简化
```swift
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
let reversedNames = names.sort({ (s1: String, s2: String) -> Bool in
    return s1 > s2
})
```

###### 简化：省略闭包中各参数的类型和返回类型。根据上下文推断类型，因此可以省略闭包中参数列表中的类型和返回类型
```swift
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
let reversedNames = names.sort({ s1, s2 in
    return s1 > s2
})
```

###### 简化：省略return关键字。单表达式闭包隐式返回，即省略return关键字
当闭包中的执行体部分只有一个表达式时，可以通过省略`return`关键字来隐式返回单行表达式的结果。
```swift
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
let reversedNames = names.sort({ s1, s2 in
   s1 > s2    // 此闭包体省略了return关键字，因为其只有一个表达式
})
```

###### 简化：省略参数列表和`in`关键字，直接通过`$0, $1, $2`等来顺序调用闭包的参数。

```swift
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
let reversedNames = names.sort({
   $0 > $1    // 此闭包体省略了闭包参数列表，直接使用$0和$1分别表示第一、第二个输入参数。
})
```

# 尾随闭包 Trailing Closure
若需要将一个很长的闭包表达式作为最后一个参数传递给函数，可以使用尾随闭包来增强函数的可读性。尾随闭包是一个书写在函数括号之后的闭包表达式，函数支持将其作为最后一个参数调用。
```swift
// 定义一个函数，该函数中的输入参数closure类型为() -> Void
func someFunctionThatTakesAClosure(closure: () -> Void) {
    // 函数体部分
}

// 函数调用：不使用尾随闭包形式，而是参数常规的函数调用形式
someFunctionThatTakesAClosure({
    // 闭包主体
})

// 函数调用：使用尾随闭包形式
someFunctionThatTakesAClosure() {
    // 闭包主体
}

// 函数调用：使用尾随闭包形式并省略括号。条件：函数只需要闭包表达式一个参数时。
someFunctionThatTakesAClosure {
    // 闭包主体
}
```

# 参考文档
- [The Swift Programming Language - Language Guide - Closures][swift-closures]

[swift-closures]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html