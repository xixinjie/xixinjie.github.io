---
layout: post
title:  "Swift基础之可选链式调用"
date:   2018-04-09 00:30:00 +0800
categories: Swift
---
### 使用可选链式调用代替强制展开

```swift
class Residence {
	var numberOfRooms = 1
}

class Person {
	var residence: Residence?	// 默认初始化为nil
}

let john = Person()

// 强制展开
let roomCount = john.residence!.numberOfRooms

// 可选链式调用
let roomCount = john.residence?.numberOfRooms
```

### 为可选链式调用定义模型类
```swift
class Room {
    let name: String
    init(name: String) {
        self.name = name
    }
}

class Address {
    var buildingName: String?
    var buildingNumber: String?
    var street: String?

    func buildingIdentifier() -> String? {
        if buildingName != nil {
            return buildingName
        } else if buildNumber != nil && street != nil {
            return "\(buildingNumber) \(street)"
        } else {
            return nil
        }
    }
}

class Residence {
    var rooms = [Room]()
    var numberOfRooms: Int {
        return rooms.count
     }
    var address: Address?

    func printNumberOfRooms() {
        print("The number of rooms is \(numberOfRooms)")
    }

    subscript(i: Int) -> Room {
        get {
            return rooms[i]
        }
        set {
            rooms[i] = newValue
        }
    }
}

class Person {
    var residence: Residence?
}
```

### 通过可选链式调用访问属性
可以通过可选链式调用`可选值?.属性`获取可选值的属性，也可为其设置新值。

* 获取属性的值
通过`可选值?.属性`访问该属性时，若可选值为nil则返回nil，若可选值非nil则返回与该属性同类型的可选值。

```swift
let john = Person()
if let roomCount = john.residence?.numberOfRooms {
   print("John's residence has \(roomCount) rooms")
} else {
    print("Unable to retrieve the number of rooms")
}
// 输出"Unable to retrieve the number of rooms"
```

* 设置属性的值
通过`可选值?.属性 = 新值`设置值时，若可选值不为nil则设置成功，若可选值为nil，则等号右边的不会执行。

```swift
func createAddress() -> Address {
    print("Function was called.")  // 打印该句只是用来判断该函数是否被执行

    let address = Address()
    address.buildingNumber = "21"
    address.street = "Nanjing Road"

    return address
}

let john = Person()    // john.residence默认被初始化为nil

// 赋值但不判断否成功
john.residence?.address = createAddress()    // 为可选链中的属性赋值时，因可选链为nil导致属性访问失败，最终导致右边的表达式不会被执行

// 赋值并判断是否成功
if (john.residence?.address = createAddress()) != nil {
    print("It was possible to set the address")
} else {
    print("It was not possible to set the address")
}
```

### 通过可选链式调用调用方法
通过`可选值?.方法名()`来调用方法
```swift
let john = Person()

// 调用方法但不判断是否成功
john.residence?.printNumberOfRooms()

// 调用方法并判断是否成功
if john.residence?.printNumberOfRooms() != nil { // 因residence默认被初始化为nil，因此调用失败，返回nil
    print("It was possible to print the number of rooms")
} else {
    print("It was not possible to print the number of rooms")
}
// 输出"It was not possible to print the number of rooms"
```

### 通过可选链式调用访问下标
通过`可选值?[下标]`来访问

```swift
let john = Person()
if let firstRoomName = john.residence?[0].name {
    print("The first room name is \(firstRoomName)")
} else {
    print("Unable to retrieve the first room name")
}

john.residence?[0].name = "Bedroom"
```

```swift
// 通过可选链式调用访问Dictionary的下标
var testScores = ["Dave": [86, 82, 84], "Bev": [79, 94, 81]]
testScores["Dave"]?[0] = 91
testScores["Bev"]?[0]++
testScores["Brian"]?[0] = 72    // 因testScores中不存在键名"Brian"，因此调用失败。
// 此时testScores的值为: ["Dave": [91, 82, 84], "Bev": [80, 94, 81]]
```

### 连接多层可选链式调用
可以使用多个`?`来进行多层可选链式调用。
* 若访问的值不是可选的，可选链式调用将会返回可选值
* 若访问的值就是可选的，可选链式调用不会使返回值变得"更可选"，即只包了一层可选

```swift
let john = Person()
if let street = john.residence?.address?.street {
    print("John's street name is \(street)")
} else {
    print("Unable to retrieve the street name")
}
```

### 在方法的可选返回值上进行可选链式调用
若方法返回可选值，则可以通过`方法名()?.属性`或`方法名()?.方法名()`进行可选链式调用
```swift
let john = Person()
if let beginWithThe = john.residence?.address?.buildingIdentifier()?.hasPrefix("The") {
    if beginWithThe {
        print("John's building identifier begins with \"The\".")
    } else {
        print("John's building identifier does not begin with \"The\".")
    }
}
```

# 参考文档
- [The Swift Programming Language - Language Guide - Optional Chaining][swift-optional-chaining]

[swift-optional-chaining]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/OptionalChaining.html