---
title: "Swift基础之继承"
date: 2018-04-09T00:30:59+08:00
draft: true
---

### 重写 Override

###### 重写方法

###### 重写属性
* 重写属性的setter和getter
* 重写属性观察器



###### 防止重写
通过在相应声明前添加`final`关键字防止重写。
* `final var`
* `final func`
* `final class func`
* `final subscript`
* `final class` 在类名前加final表示该类不能被继承

### 完整示例
```swift
class Vehicle {
    var currentSpeed = 0.0
    var description: String {
        return "traveling at \(currentSpeed) miles per hour"
    }

    func makeNoise() {
        // 什么也不做，因为车辆不一定有噪音
    }
}

class Bicycle: Vehicle {
    var hasBasket = false    // 在子类中添加子类特有的属性
}

class Tandem: Bicycle {
    var currentNumberOfPassengers = 0
}

class Train: Vehicle {
    override makeNoise() {    // 通过override关键字重写超类中的方法
        print("choo choo")
    }
}

class Car: Vehicle {
    var gear = 1
    override var description: String {    // 通过override关键字重写超类中的属性
        return super.description + " in gear \(gear)"
    }
}

class AutomaticCar: Car {
    override var currentSpeed: Double {    // 通过override关键字重写超类中的属性，为其添加属性观察器。当其值改变时，修改其他属性的值。
        didSet {
            gear = Int(current / 10.0) + 1
        }
    }
}

final class OldCar: Car {    // 通过final声明该类不可被继承
    var yearsUsed = 10
}
```

# 参考文档
- [The Swift Programming Language - Language Guide - Inheritance][swift-inheritance]

[swift-inheritance]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Inheritance.html