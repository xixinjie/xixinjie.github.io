---
layout: post
title:  "Swift基础之泛型"
date:   2018-04-09 00:30:00 +0800
categories: Swift
---
在Swift中，可以定义泛型函数和泛型类型。

### 泛型函数
泛型函数适用于任何类型。

```swift
// 注：系统有用于交换两个变量的值的内置方法swap。此处单独定义一个用于说明泛型。

// 交换两个Int值
func swapTwoInt(inout a: Int, inout _ b: Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}

// 交换类型相同的两个变量的值
func swapTwoValues<T>(inout a: T, inout _ b: T) {
    let temporaryA = a
    a = b
    b = temporaryA
}

// 测试泛型函数
var someInt = 3
var anotherInt = 107
swapTwoValues(&someInt, &anotherInt)    // a由3换为107，b由107换为3

var someString = "hello"
var anotherString = "world"
swapTwoValue(&someString, &anotherString)
```


### 类型参数

###### 类型参数的用途
在泛型中，类型参数只是一个占位符，可以用在泛型函数或泛型类型的定义中代指某种类型，用做泛型函数的参数类型、返回类型，用作泛型类型中属性的类型等。在调用时，类型参数会被实际类型所替换。


###### 类型参数的命名
使用大写字母开头的驼峰式命名法为类型参数命名，以表明它们是占位类型而不是一个值。可以使用一个描述性的名字（如Key，Value，Element），也可以使用单一字母来命名（如T, U, V）。

###### 类型参数的格式
在泛型中，可以提供一个或多个类型参数，如 `<T>` 或 `<E, T>` 
```swift
<类型参数名>
<类型参数名1, 类型参数名2>
```

###### 类型参数的约束
类型约束可以指定一个类型参数必须继承自指定类，或者符合一个特定的协议或协议组合。如Dictionary中键类型必须符合`Hashable`协议。
可以在一个类型参数名后面放置一个类名或者协议名，通过冒号分隔，从而定义类型约束。

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    // 这里是泛型函数的函数体部分
}
```



###### Where 子句
可以在参数列表中通过`where`子句为关联类型定义约束。一个`where`子句能够使一个关联类型符合某个特定的协议，以及某个特定的类型参数和关联类型必须类型相同。
可以通过将`where`关键字紧跟在类型参数列表后面来定义`where`子句，`where`子句后跟一个或多个针对关联类型的约束，以及一个或多个类型参数和关联类型间的相等关系。

```swift
func allItemsMatch<C1: Container, C2: Container where C1.ItemType == C2.ItemType, C1.ItemType: Equatable>(someContainer: C1, _ anotherContainer: C2) -> Bool {
    // 检查两个容器含有相同数量的元素
    if someContainer.count != anotherContainer.count {
        return false
    }

    // 检查每一对元素是否相等
    for i in 0..<someContainer.count {
        if someContainer[i] != anotherContainer[i] {
            return false
        }
    }

    // 所有元素都匹配，返回true
    return true
}

var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")

var arrayOfStrings = ["uno", "dos", "tres"]

if allItemsMatch(stackOfStrings, arrayOfStrings) {
    print("All items match.")
} else {
    print("Not all items match.")
}
// "All items match"
```


### 泛型类型
除了泛型函数，Swift还允许定义泛型类型。

###### 定义一个泛型类型

```swift
// 定义一个Int型的栈
struct IntStack {
    var items = [Int]()
    mutating func push(item: Int) {
        items.append(item)
    }

    mutating func pop() -> Int {
        return items.removeLast()
    }
}

// 定义一个泛型栈
struct Stack<Element> {
    var items = [Element]()
    mutating func push(items: Element) {
        item.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}

// 测试泛型栈
var stackOfString = Stack<String>()
stackOfString.push("uno")
stackOfString.push("dos")
stackOfString.push("tres")
stackOfString.push("cuatro")

let fromTheTop = stackOfString.pop()
```

###### 扩展一个泛型类型
扩展一个泛型类型时，可以直接使用原始类型定义中声明的类型参数。

```swift
extension Stack {
    var topItem: Element? {    // 返回栈顶元素，但不删除
        return item.isEmpty ? nil : items[items.count - 1]
    }
}

// 测试对泛型类型的扩展
if let topItem = stackOfString.topItem {
    print("The top item on the stack is \(topItem)")
}
```



### 关联类型
可以通过`typealias`关键字来指定关联类型。

```swift
protocol Container {
    typealias ItemType
    mutating func append(item: ItemType)
    var count: Int { get }
    subscript(i: Int) -> ItemType { get }
}

struct Stack<Element>: Container {
    // Stack<Element>的原始实现部分
    var items = [Element]()
    mutating func push(item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }

    // Container协议的实现部分

    // typealias ItemType = Element  // 该句可以省略，因为可以根据下面方法的参数推断出ItemType类型为Element
    mutating func append(item: Element) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Element {
        return items[i]
    }
}
```

###### 通过扩展一个存在的类型来指定关联类型


### 实例

```swift
func findStringIndex(array: [String], _ valueToFind: String) -> Int? {
    for (index, value) in array.enumerate() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}

let strings = ["cat", "dog", "llama", "parakeet", "terrapin"]
if let foundIndex = findStringIndex(strings, "llama") {
    print("The index of llama is \(foundIndex)")
}
// "The index of llama is 2

// 定义包含约束的泛型函数
func findIndex<T: Equatable>(array: [T], _ valueToFind: T) -> Int? {
    for (index, value) in array.enumerate() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}

let doubleIndex = findIndex([3.14159, 0.1, 0.25], 9.3)    // nil
let stringIndex = findIndex(["Mike", "Malcolm", "Andrea"], "Andrea")    // 返回Int?，其值为2
```

# 参考文档
- [The Swift Programming Language - Language Guide - Generics][swift-generics]

[swift-generics]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Generics.html