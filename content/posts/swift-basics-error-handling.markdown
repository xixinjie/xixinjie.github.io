---
title: "Swift基础之错误处理"
date: 2018-04-09T00:30:59+08:00
draft: true
---

###### 关键词
`ErrorType` `throws` `throw` `try` `try?` `try!` `do-catch` `defer`

#### 定义错误
错误用符合`ErrorType`协议的类型的值来表示。该空协议表明该类型用于错误处理。一般通过新建一个遵从`ErrorType`协议的枚举类型来自定义错误类型，枚举中的每个值都代表不同的错误状态。且可以通过枚举的关联值为不同的错误状态提供额外的错误信息。

```swift
enum SomeError: ErrorType {
    case FirstKind
    case SecondKind
    case ThirdKind
}
```

#### 抛出错误
使用`throw`关键字来抛出错误。
注：定义一个方法时在其参数列表括号后写上`throws`来表示该方法可以抛出错误，然后就可以在该方法内部通过`throw`来抛出错误了。
```swift
throw SomeError.FirstKind
```

### 处理错误
Swift有四种处理错误的方式，分别为：
* 把函数抛出的错误传递给调用此函数的代码
* 用`do-catch`语句处理错误
* 将错误作为可选类型处理 `try?`
* 禁止错误传递，断言此错误根本不会发生 `try!`

###### 把函数抛出的错误传递给调用此函数的代码
如何定义一个可以抛出错误的函数？
在定义函数时把`throws`关键字写在参数列表的括号后面，即表明该函数可以抛出错误。
```swift
func canThrowErrors() throws -> String {
    // 在方法内部可以通过throw关键字来抛出错误
}

func canThrowErrors() throws {
     // 在方法内部可以通过throw关键字来抛出错误
}

func cannotThrowErrors() {

}
```

###### 使用`do-catch`语句来处理错误


###### 使用`try?`将错误作为可选类型处理
若执行`try?`所在的表达式时一个错误被抛出，则表达式的值就是`nil`。

###### 使用`try!`来断言此错误根本不会发生
若预估在调用可抛出错误的方法时不会抛出错误，可以使用`try!`来调用该方法。
注：若实际上抛出了错误，则会出现运行时错误（crash）。

### 用`defer`指定清理操作
```swift
// some scope
{
    // Get some resource

    defer {
        // Release resource
    }

    // Do things with the resource
    // Possibly return early if an error occurs
}    // Defferred code is executed at the end of the scope
```

### 代码示例
```swift
// BookShelf.playground
import UIKit

enum BookShelfError: ErrorType {
    case OutOfBooks
    case NoEnoughSpace(required: Int)
}

class BookShelf: CustomStringConvertible {
    private let maxCount = 1000
    private(set) var countOfBooksOnShelf: Int = 0    // 访问控制

    var description: String {
        return "书柜目前还有\(countOfBooksOnShelf)本书"
    }

    func purchaseBooks(count: Int) throws {
        guard countOfBooksOnShelf + count <= maxCount else{
            let required = countOfBooksOnShelf + count - maxCount
            throw BookShelfError.NoEnoughSpace(required: required)
        }
        countOfBooksOnShelf += count
    }

    func lendBooks(count: Int) throws {
        guard count <= countOfBooksOnShelf else {
            throw BookShelfError.OutOfBooks
        }

        countOfBooksOnShelf -= count
    }
}

func starterShelf() -> BookShelf {
    let myShelf = BookShelf()
    try! myShelf.purchaseBooks(100)

    return myShelf
}

let shelf = starterShelf()

do {
    try shelf.purchaseBooks(1500)
} catch BookShelfError.NoEnoughSpace(let required) {
    print("书柜满啦，新买的书放不下咯。还差\(requried)个空位，赶紧买个新书柜吧")
}

do {
    // should be OK
    try shelf.lendBooks(50)
    // should throw an error
    try shelf.lendBooks(200)
} catch BookShelfError.OutOfBooks {
    print("没有这么多本书可以借出。")
}
```


###### 参考资料
* Error Handling in Swift 2.0 https://www.bignerdranch.com/blog/error-handling-in-swift-2/


# 参考文档
- [The Swift Programming Language - Language Guide - Error Handling][swift-error-handling]

[swift-error-handling]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html