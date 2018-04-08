---
layout: post
title:  "Swift基础之协议"
date:   2018-04-09 00:30:00 +0800
categories: Swift
---
### 在协议中定义各种要求

##### 属性要求
协议可以要求采纳协议的类型提供特定名称和类型的实例属性或类型属性。
协议不指定属性是存储型属性还是计算型属性，它只指定属性的名称和类型，以及是只读的还是可读可写的。

协议中通常用`var`来声明变量属性，在属性的类型声明后加`{ set get}`来表示可读可写，加`{ get }`来表示只读。
若协议要求属性是可读可写的，那么该属性不能是常量属性或只读的计算型属性。
若协议要求属性是只读的，那么该属性不仅可以是只读的，如果代码需要的话也可以是可写的。

###### 类型属性要求
在协议中定义类型属性时，总是使用`static`关键字作为前缀。当类类型采纳协议时，除了使用`static`关键字，还可以使用`class`关键字。

```swift
protocol SomeProtocol {
    static var someTypeProperty: Int { get set }
}
```

###### 实例属性要求

```swift
protocol FullyNamed {
    var fullName: String { get }    // 只读属性要求
}

struct Person: FullyNamed {
    var fullName: String    // 将只读属性要求实现为存储属性
}

let john = Person(fullName: "John")

class Starship: FullyNamed {
    var prefix: String?
    var name: String
    init(name: String, prefix: String? = nil) {
        self.name = name
        self.prefix = prefix
    }

    var fullName: String {    // 将只读属性要求实现为只读计算属性
        return (prefix != nil ? prefix! + " " : "") + name
    }
}

var ncc1701 = Starship(name: "Enterprise", prefix: "USS")
```

##### 方法要求
协议可以要求采纳协议的类型实现指定的实例方法或类方法。
* 可以在协议中定义具有可变参数的方法
* 不支持为协议中的方法提供默认值

###### 类型方法要求
在协议中定义类型方法要求时，总是使用`static`关键字。而在遵从该协议的类中实现该方法时，可以用`static`（子类不可重写）或`class`（子类可重写）

```swift
protocol SomeProtocol {
    static func someTypeMethod()    // protocol中的类型方法要求必须使用static
}

struct SomeStruct: SomeProtocol {
    static func someTypeMethod() {
        // 实现部分
    }
}

enum SomeEnum: SomeProtocol {
    static func someTypeMethod() {
        // 实现部分
    }
}

class SomeClass: SomeProtocol {
    static func someTypeMethod() {    // 子类不可重写，此处static相当于final class
        // 实现部分
    }
}

class AnotherClass: SomeProtocol {
    class func someTypeMethod() {    // 子类可重写
        // 实现部分
    }
}
```

###### 实例方法要求

```swift
protocol SomeProtocol {
    func someInstanceMethod()    // 实例方法要求
}

class SomeClass: SomeProtocol {
    func someInstanceMethod() {
        // 实现部分
    }
}
```

###### Mutating方法要求
可以在协议中定义`mutating`方法，从而允许实现该方法的类型可以改变其实例。实现协议中的`mutating`方法时，若是类类型，则不用写`mutating`关键字，若是结构体或枚举类型，则需要写`mutating`关键字

```swift
protocol SomeProtocol {
    mutating func SomeMethod()
}

enum SomeEnum: SomeProtocol {
    mutating func SomeMethod() {
        // 实现部分
    }
}

struct SomeStruct: SomeProtocol {
    mutating func SomeMethod() {
        // 实现部分
    }
}

class SomeClass: SomeProtocol {
    func SomeMethod() {    // 此处省略了mutating关键字
        // 实现部分
    }
}
```

```swift
protocol Toggable {
    mutating func toggle()
}

enum OnOffSwitch: Toggable {
    case Off, On
    mutating func toggle() {
        switch self {
            case .Off:
                self = .On
            case .On:
                self = .Off
        }
    }
}

let lightSwitch = OnOffSwitch.Off
lightSwitch.toggle()    // lightSwitch的值现在被修改为.On
```

###### 构造方法要求
可以在协议中定义构造器方法，要求采纳协议的类型实现该构造器。
* 在采纳协议的类中实现协议所要求的的构造器时，必须为其添加`required`关键字。特殊地，若类已经被标记为`final`，则可以省略`required`关键字。

```swift
protocol SomeProtocol {
    init(someParameter: Int)
}

class SomeClass: SomeProtocol {
    required init(someParameter: Int) {    // 因为该构造器遵从协议，所以要加required
        // 实现部分
    }
}

final class SomeFinalClass: SomeProtocol {
    init(someParameter: Int) {    // 虽然该构造器遵从协议，但是该类为final，所以可以省略required
        // 实现部分
    }
}
```

* 若子类重写了父类中的指定构造器，且该构造器满足了某个协议的要求，则该构造器的实现中需要同时添加`required`和`override`这两个关键字。

```swift
protocol SomeProtocol {
    init()
}

class SomeSuperClass {
    init()
}

class SomeSubClass: SomeSuperClass, SomeProtocol {
    required override init() {    // 因为采纳协议所以要添加required，因为重写父类的init方法，所以要添加override
        // 实现部分
    }
}
```

##### 关联类型要求
在协议中可以通过`typealias 类型别名`定义一个关联类型要求，然后可以使用该类型别名做为协议中属性要求的类型，方法要求的参数类型或返回类型等。遵从该协议的类型要在其实现中通过`typealias 类型别名= 具体类型名`来实现这个关联类型要求。

有时可以从遵从协议的类型的实现中推断出关联类型要求的具体类型，这种情况下可以省略`typealias 类型别名 = 具体类型名`。

```swift
protocol Parsable {
    typealias ModelType    // 关联类型要求
    static func parse(json: JSON) -> ModelType    // 在其他要求中使用关联类型别名
}

struct Comment: Parsable {
    // Comment 自己的实现部分
    var username: String
    var comment: String
    var dateString: String

    // Parsable协议的实现部分
    typealias ModelType = Comment
    static func parse(json: JSON) -> Comment {
        let username = json["username"].stringValue
        let comment = json["comment"].stringValue
        let dateString = json["date"].stringValue

        return Comment(username: username, comment: comment, dateString: dateString)
    }    
}
```


### 协议的扩展

###### 通过扩展为协议提供默认实现
* 通过扩展可以为协议中的属性、方法或下标提供默认实现。
* 若协议提供了默认实现，采纳协议的类可以直接调用其默认实现，也可以通过在类内提供自己的实现来覆盖默认实现

```swift
// 定义协议
protocol PrettyTextRepresentable {
    var prettyTextualDescription: String { get }    // 协议属性要求
}

// 扩展协议：提供默认实现
extension PrettyTextRepresentable {
    var prettyTextualDescription: String {    // 协议属性要求的默认实现
        return "Some default string"
    }
}

// 采纳协议：直接使用默认实现的值
class SomeClass: PrettyTextRepresentable {
    func someFunction() {
        print("默认实现的值: \(prettyTextualDescription)")    // 直接在采纳协议的类中调用协议的默认实现
    }
}

// 采纳协议：使用覆盖默认实现的值
class AnotherClass: PrettyTextRepresentable {
    var prettyTextualDescription: String {    // 在类中提供对协议属性要求的特有实现，覆盖协议默认实现
        return "覆盖协议默认实现的值"
    }

    func someFunction() {
        print("自己实现的值: \(prettyTextualDescription)")    // 调用类自己对协议中属性要求的实现
    }
}
```

###### 为协议扩展添加限制条件
在扩展协议时，可以指定一些限制条件，使只有采纳协议的类型满足这些限制条件时，才能获得协议扩展提供的默认实现。

```swift

```


### 协议的继承
协议能够继承一个或多个其他协议，可以在继承的协议的基础上增加新的要求。

```swift
protocol InheritingProtocol: SomeProtocol, AnotherProtocol {

}
```

###### 类类型专属协议
可以通过在协议的继承列表中第一个位置放`class`关键字，来限制协议只能被类类型采纳，而不能被枚举或结构体类型采纳。

```swift
protocol SomeClassOnlyProtocol: class, SomeInheritedProtocol {

}
```


### 使用协议

###### 在定义类型时使该类型遵从协议

```swift
protocol SomeProtocol {
    // 定义协议要求
}

class SomeClass: SomeProtocol {
    // 实现协议要求
}
```

###### 通过扩展使已有类型遵从协议
可以通过扩展使已有类型采纳并符合某协议。

```swift
protocol TextRepresentable {
    var textualDescription: String { get }
}

extension Dice: TextRepresentable {
    var textualDescription: String {
        return "A \(sides)-sides dice"
    }
}
```

###### 将协议作为一种类型
尽管协议本身并未实现任何功能，但可被当做一个类型来使用
* 作为函数、方法或构造器中的参数类型或返回值类型
* 作为常量、变量或属性的类型
* 作为数组、字典或其他容器中的元素类型


### 完整示例
```swift
protocol RandomNumberGenerator {
    func random() -> Double
}

class LinearCongruentialGenerator: RandomNumberGenerator {
    var lastRandom = 42.0
    let m = 139968.0
    let a = 3877.0
    let c = 29573.0
    func random() -> Double {
        lastRandom = ((lastRandom * a + c) % m)
        return lastRandom / m
    }
}

let generator = LinearCongruentialGenerator()
print("Here's a random number: \(generator.random())")
print("And another one: \(generator.random())")

class Dice {
    let sides: Int
    let generator: RandomNumberGenerator    // 协议作为属性的类型。任何采纳了RandomNumberGenerator协议的类型的实例都可以赋值给该属性

    init(sides: Int, generator: RandomNumberGenerator) {    // 协议作为构造器的参数类型
        self.sides = sides
        self.generator = generator
    }

    func roll() {
        return Int(generator.random() * Double(sides)) + 1
    }
}

var d6 = Dice(sides: 6, generator: LinearCongruentialGenerator())
for _ in 1...5 {
    print("Random dice roll is \(d6.roll())")
}

protocol DiceGame {
    var dice: Dice { get }
    func play()
}

protocol DiceGameDelegate {
    func gameDidStart(game: DiceGame)
    func game(game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int)
    func gameDidEnd(game: DiceGame)
}

class SnakesAndLadders: DiceGame {
    let finalSquare = 25
    let dice = Dice(sides: 6, generator: LinearCongruentialGenerator())
    var square = 0
    var board: [Int]

    init() {
        board = [Int](count: finalSquare + 1, repeatedValue: 0)
        board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
        board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
    }

    var delegate: DiceGameDelegate?    // 定义用作代理的属性
    func play() {
        square = 0
        delegate?.gameDidStart(self)    // 调用代理方法
        gameLoop: while square != finalSquare {
            let diceRoll = dice.roll()
            delegate?.game(self, didStartNewTurnWithDiceRoll: diceRoll)    // 调用代理方法
            switch square + diceRoll {
            case finalSquare:
                break gameLoop
            case let newSquare where newSquare > finalSquare:
                continue gameLoop
            default:
                square += diceRoll
                square += board[square]
            }
        }
        delegate?.gameDidEnd(self)    // 调用代理方法
    }
}

class DiceGameTracker: DiceGameDelegate {
    var numberOfTurns = 0

    func gameDidStart(game: DiceGame) {
        numberOfTurns = 0
        if game is SnakesAndLadders {
            print("Started a new game of Snakes and Ladders")
        }
        print("The game is using \(game.dice.sides)-sides dice")
    }

    func game(game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int) {
        ++numberOfTurns
        print("Rolled a \(diceRoll)")
    }

    func gameDidEnd(game: DiceGame) {
        print("The game lasted for \(numberOfTurns) turns")
    }
}

let tracker = DiceGameTracker()
let game = SnakesAndLadders()
game.delegate = tracker
game.play()
```

# 参考文档
- [The Swift Programming Language - Language Guide - Protocols][swift-protocols]

[swift-protocols]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html