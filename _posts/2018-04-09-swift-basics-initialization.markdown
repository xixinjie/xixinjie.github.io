---
layout: post
title:  "Swift基础之构造器"
date:   2018-04-09 00:30:00 +0800
categories: Swift
---
###### 关键词
`init`

### 存储属性的初始赋值

* 在声明属性的时候设置初始值。

```swift
struct Fahrenheit {
    var temperature = 32.0
}

var f = Fahrenheit()
f.temperature    // 32.0
```

* 在构造器中为属性设置初始值。

```swift
struct Fahrenheit {
    var temperature: Double
    init() {
        temperature = 32.0
    }
}

var f = Fahrenheit()
f.temperature    // 32.0
```

### 自定义构造过程

###### 构造参数

###### 参数的内部名称和外部名称
构造函数的参数也可以拥有一个在构造器内部使用的参数名字和一个在调用构造器时使用的外部参数名字。
若在定义构造器时没有提供参数的外部名字，swift会为构造器的每个参数自动生成一个和内部参数名相同的外部参数名。
若不希望为构造器的某个参数提供外部参数名，则可以使用下划线`_`来显式描述其外部参数名，以此重写自动生成的外部参数名。
```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {    // 显式外部名fromFahrenheit
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }

    init(kelvin: Double) {    // 隐式外部名，会自动生成与内部名同名的外部名kelvin
        temperatureInCelsius = kelvin - 273.15
    }

    init(_ celsius: Double) {    // 不带外部名
        temperatureInCelsius = celsius
    }
}

let bodyTemperature = Celsius(37.0)
```

###### 可选属性类型
如果自定义类型允许包含一个逻辑上为空的存储型属性，则需要将其定义为可选类型。该可选类型存储型属性将自动初始化为nil。

```swift
class SurveyQustion {
    var text: String
    var response: String?    // 可选型存储属性
    init(text: String) {
        self.text = text
    }

    func ask() {
        print(text)
    }
}

let cheeseQuestion = SurverQuestion(text: "Do you like cheese?")
cheeseQuestion.ask()
cheeseQuestion.response = "yes, I do like cheese."
```

###### 构造过程中修改常量属性
可以在构造过程中的任意时间点修改常量属性，只要在构造结束时是一个确定的值。一旦构造结束，其常量属性将不可修改。

```swift
class SurveyQustion {
    let text: String
    var response: String?
    init(text: String) {
        self.text = text    // 在构造器中修改常量的值
    }

    func ask() {
        print(text)
    }
}

let cheeseQuestion = SurverQuestion(text: "Do you like cheese?")
cheeseQuestion.ask()
cheeseQuestion.response = "yes, I do like cheese."
```

### 默认构造器

###### 不含参的默认构造器
如果结构体或类的所有属性都有默认值，且没有自定义的构造器，那么swift会给这些类或结构体提供一个默认构造器。这个构造器将简单地创建一个所有属性都设置为默认值的对象实例。

```swift
class ShoppingListItem {
    var name: String?    // 默认值为nil
    var quantity = 1
    var purchased = false
}

var item = ShoppingListItem()
```

###### 结构体的逐一成员构造器：含所有参数的默认构造器
对结构体而言，若没有自定义构造器，除了不含参的默认构造器外，结构体还会自动获得一个逐一成员构造器，即使结构体的存储属性没有默认值。

```swift
struct Size {
    var width = 0.0, height = 0.0
}

let twoByTwo = Size(width: 2.0, height: 2.0)
```

### 值类型的构造器代理
构造器可以通过调用其他构造器来完成实例的部分构造过程，这一过程成为构造器代理。可以减少多个构造器之间的代码重复。

```swift
// 定义
struct Size {
    var width = 0.0, height = 0.0
}

struct Point {
    var x = 0.0, y = 0.0
}

struct Rect {
    var origin = Point()
    var size = Size()

    // 构造器1：使用被初始化为默认值的属性来初始化
    init() {
    }

    // 构造器2：使用指定的origin和size来初始化
    init(origin: Point, size: Size) {
        self.origin = origin
        self.size = size
    }

    // 构造器3：使用指定的center和size来初始化。该构造器内部调用了构造器2，可以看做构造器2的代理
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)    // 调用其他构造器
    }
}

// 调用
let basicRect = Rect()
let originRect = Rect(origin: Point(x: 2.0, y: 2.0), size: Size(width: 5.0, height: 5.0))
let centerRect = Rect(center: Point(x: 4.0, y: 4.0), size: Size(width: 3.0, height: 3.0))
```

### 类的继承和构造过程

###### 指定构造器和便利构造器

###### 类的构造器代理规则
指定构造器必须是向上代理，便利构造器必须是横向代理
* 指定构造器必须调用其直接父类的指定构造器
* 便利构造器必须调用同一类中定义的其他构造器
* 便利构造器必须最终导致一个指定构造器被调用

###### 两段式构造过程

###### 构造器的继承和重写

```swift
class Vehicle {
    var numberOfWheels = 0
    var description: String {
        return "\(numberOfWheels wheel(s))"
    }
}

class Bicycle: Vehicle {
    override init() {    // 重写父类中的默认构造器
        super.init()
        numberOfWheels = 2
    }
}
```

###### 构造器的自动继承

```swift
class Food {
    var name: String
    init(name: String) {    // 指定构造器
        self.name = name
    }

    convenience init() {    // 便利构造器
        self.init(name: "[Unnamed]")
    }
}

let namedMeat = Food(name: "Bacon")    // 使用指定构造器初始化

class RecipeIngredient: Food {
    var quantity: Int
    init(name: String, quantity: Int) {
        self.quantity = quantity
        super.init(name: name)    // 子类的指定构造器代理父类的指定构造器
    }

    override convenience init(name: String) {    // 子类的便利构造器重写了父类的指定构造器(同名同参)，所以要加convenience
        self.init(name: name, quantity: 1)    // 便利构造器横向代理类内的指定构造器
    }
}

let oneMysteryItem = RecipeIngredient()    // 使用子类从父类中自动继承得到的指定构造器初始化
let oneBacon = RecipeIngredient(name: "Bacon")    // 使用子类的便利构造器实例化
let sixEggs = RecipeIngredient(name: "Eggs", quantity: 6)    // 使用子类的指定构造器实例化

class ShoppingListItem: RecipeIngredient {
    var purchased = false
    var description: String {
        var output = "\(quantity) x \(name)"
        output += purchased ? " yes" : " no"
        return output
    }
}

var breakfastList = [
    ShoppingListItem(),    // 使用继承自父类的指定构造器实例化
    ShoppingListItem(name: "Bacon"),    // 使用继承自父类的便利构造器实例化
    ShoppingListItem(name: "Eggs", quantity: 6)    // 使用继承自父类的指定构造器实例化
]
```

### 可失败构造器

###### 枚举的可失败构造器

```swift
enum TemperatureUnit {
    case Kelvin, Celius, Fahrenheit
    init?(symbol: Character) {
        switch symbol {
            case "K":
                self = .Kelvin
            case "C":
                self = .Celius,
            case "F":
                self = .Fahrenheit
            default:
                return nil
        }
    }
}

let fahrenheitUnit = TemperatureUnit(symbol: "F")    // 构造成功，返回Optional .Fahrenheit
let unknownUnit = TemperatureUnit(symbol: "X")    // 构造失败，返回nil
```

###### 带原始值的枚举类型的可失败构造器
带原始值的枚举类型会自带一个可失败构造器`init?(rawValue:)`，若构造时rawValue可以和某个枚举成员的原始值匹配，则构造成功并返回可选的相应枚举成员，否则构造失败并返回nil。

```swift
enum TemperatureUnit: Character {
    case Kelvin = "K", Celsius = "C", Fehrenheit = "F"
}

let fahrenheitUnit = TemperatureUnit(rawValue: "F")    // 构造成功，返回Optional .Fahrenheit
let unknownUnit = TemperatureUnit(rawValue: "X")    // 构造失败，返回nil
```

###### 类的可失败构造器
值类型（枚举、结构体）的可失败构造器可以在构造过程的任意时间点触发构造失败，而类的可失败构造器只能在类的所有存储型属性被初始化后，以及构造器代理完成之后，才能触发构造失败。

```swift
class Product {
    let name: String!
    init?(name: String) {
        self.name = name
        if name.isEmpty {
            return nil
        }
    }
}
```


###### 构造失败的传递

```swift
class Product {
    let name: String!
    init?(name: String) {
        self.name = name
        if name.isEmpty {
            return nil
        }
    }
}

class CartItem: Product {
    let quantity: Int!
    init?(name: String, quantity: Int) {
        self.quantity = quantity
        super.init(name: name)    // 子类的可失败构造器向上代理父类的可失败构造器
        if quantity < 1 {
            return nil
        }
    }
}

if let twoSocks = CartItem(name: "sock", quantity: 2) {    // 构造成功返回可选值
    print("Item: \(twoSocks.name), quantity: \(twoSocks.quantity)")
}

if let zeroShirt = CartItem(name: "shirt", quatity: 0) {    // 构造失败返回nil，子类quantity不能小于1
}

if let oneUnnamed = CartItem(name: "", quantity: 1) {    // 构造失败返回nil，父类name不能为空""
}
```

###### 重写一个可失败构造器
* 可以在子类中重写父类的可失败构造器为可失败构造器
* 可以在子类中重写父类的可失败构造器为非可失败构造器

```swift
class Document {
    var name: String?
    init() {
    }

    init?(name: String) {
        self.name = name
        if name.isEmpty {
            return nil
        }
    }
}

class AutomaticallyNamedDocument: Document {
    override init() {
        super.init()
        self.name = "[Untitled]"
    }

    // 重写父类的可失败构造器为子类的非可失败构造器
    override init(name: String) {
        super.init(name: name)
        if name.isEmpty {
            self.name = "[Untitled]"
        } else {
            self.name = name
        }
    }
}

class UntitiledDocument: Document {
    // 通过强制解包方式在子类的非可失败构造器中调用父类的可失败构造器。
    override init() {
        super.init(nanme: "[Untitiled]")! 
    }
}
```

###### 可失败构造器 `init!`

### 必要构造器
* 在类的构造器前加`required`关键字表示该类的所有子类都必须实现该构造器。
* 在子类重写父类的必要构造器时，必须在子类的构造器前也添加`required`关键字。此处`required`包含`override`的意思，不用再添加`override`关键字。

```swift
class SomeClass {
    required init() {
    }
}

class SomeSubClass: SomeClass {
    required init() {    // 子类必须重写父类中含required关键字的构造器
    }
}
```

### 通过闭包或函数设置属性的默认值

# 参考文档
- [The Swift Programming Language - Language Guide - Initialization][swift-initialization]

[swift-initialization]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html