---
layout: post
title:  "Swift基础之属性"
date:   2018-04-09 00:30:00 +0800
categories: Swift
---

### 存储属性

##### 延迟存储属性
使用`lazy var`声明延迟存储属性。
> 因为这种属性的初始值可能在实例构造完成之后才会得到。而常量属性在构造过程完成之前必须要有初始值，因此无法声明成延迟属性。

* 延迟存储属性的两种声明方式
```swift
class SomeClass {
    // 使用一般初始化
    lazy var first = NSArray(objects: "1", "2")

    // 使用闭包初始化
    // 闭包形式一：无参闭包
    lazy var second: String = {
        return "Hello World!"
    }()

    // 闭包形式二：有参闭包
    lazy var third: String = {
        return $0 + $1
    }("Hello", "World!")
}
```

* 延迟存储属性的几种使用场景

1. 属性需要复杂计算，消耗大量CPU
```swift
class SomeClass {
    lazy var num: Int = {
        var sum = 0
        for i in 1...10000 {
            sum += i
        }
        return sum
    }()
}
```

2. 属性的值依赖于其他外部因素的值

3. 属性不一定会被用到

### 计算属性
用`var`来声明计算属性。类、结构体和枚举可以定义计算属性。计算属性不直接存储值，而是提供一个getter来间接获取该属性的值，或提供一个可选的setter来间接设置其他属性或变量的值。

* 只读计算属性
只有getter没有setter的计算属性就是只读计算属性。当计算属性只有getter时，可以省略其中的get和花括号。

```swift
struct Cuboid {
    var width = 0.0, height = 0.0, depth = 0.0

    var volume: Double {    // 只读计算属性，省略get{}
        return width * height * depth
    }

    var volume2: Double {    // 只读计算属性，未省略get{}
        get {
            return width * height * depth
        }
    }
}
```

* 可读写的计算属性
当为计算属性提供setter时，可以为其提供一个变量，也可以不提供而直接使用默认变量。

```swift
struct Point {
    var x = 0.0, y = 0.0
}

struct Size {
    var width = 0.0, height = 0.0
}

struct Rect {
    var origin = Point()
    var size = Size()

    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set(newCenter) {    // 此处为setter的新值提供了变量
            origin.x = newCenter.x - (size.width / 2)
            origin.y = newCenter.y - (size.height / 2)
        }
    }
}

struct AlternativeRect {
    var origin = Point()
    var size = Size()

    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set {    // 此处未给setter的新值提供变量，直接使用默认变量newValue
            origin.x = newValue.x - (size.width / 2)
            origin.y = newValue.y - (size.height / 2)
        }
    }
}
```

### 类型属性

类型属性用于定义某个类型所有实例共享的数据，无论创建了多少个该类型的实例，类型属性都只有唯一一份。
必须给存储型类型属性指定默认值。且存储型类型属性是延迟初始化的，即它们只有在第一次被访问时才被初始化。即使被多个线程同时访问，也只会进行一次初始化。且不需要对其显式使用lazy修饰符。

使用关键字`static`来定义类型属性。在为类定义计算型类型属性时，可以改用关键字`class`来支持子类对父类的实现进行重写。

```swift
struct SomeStructure {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 1
    }
}

enum SomeEnumeration {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 6 
    }
}

class SomeClass {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int { // 使用static声明的计算型类型属性
        return 10
    }

    class var overrideableComputedTypeProperty: Int { // 使用class声明的计算型类型属性
        return 20
    }
}
```

### 属性观察器
可以为属性添加`willSet`和`didSet`这两个属性观察器中的一个或两个。

```swift
class StepCounter {
    var totalSteps: Int = 0    // 未提供属性观察器
}

class StepCounter {
    var totalSteps: Int = 0 {    // 提供属性观察器并未其设置入口参数变量
        willSet(newTotalSteps) {    // 该变量代表新值
            pritn("About to set totalSteps to \(newTotalSteps)")
        }
        didSet(oldTotalSteps) {     // 该变量代表旧值
            if totalSteps > oldTotalSteps {
                print("Added \(totalSteps - oldTotalSteps) steps")
            }
        }
    }
}

class StepCounter {
    var totalSteps: Int = 0 {
        willSet {    // 未提供入口参数变量，直接使用默认变量newValue
            pritn("About to set totalSteps to \(newValue)")
        }
        didSet {    // 未提供入口参数变量，直接使用默认变量oldValue
            if totalSteps > oldValue {
                print("Added \(totalSteps - oldValue) steps")
            }
        }
    }
}
```

### 全局变量和局部变量

# 参考文档
- [The Swift Programming Language - Language Guide - Properties][swift-properties]

[swift-properties]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Properties.html