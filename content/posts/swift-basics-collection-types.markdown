---
title: "Swift基础之集合类型"
date: 2018-04-09T00:30:59+08:00
draft: true
---

### 简介
Swift中提供Array、Set、Dictionary三种基本集合类型来存储集合数据。

### 集合的可变性
使用let定义时，集合不可变。
使用var定义时，集合可变。


### Array 数组

#### 创建数组

###### 通过字面值创建数组
```swift
    var shoppingList = ["Eggs", "Milk"]    // 推断该数组类型为[String]
```

###### 通过构造函数创建数组
```swift
    var threeDoubles = [Double](count: 3, repeatedValue: "Hello")    // 推断数组类型为[String], 初始容量为3， 默认值为"Hello"
```

###### 通过+运算符合并两个数组
```swift
    var threeDoubles = [0.0, 0.0, 0.0]
    var anotherThreeDoubles = [2.5, 2.5, 2.5]
    var sixDoubles = threeDoubles + anotherThreeDoubles    // [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
```

#### 访问数组

###### 通过下标访问
```swift
var data = [1, 2, 3, 4, 5]
data[0]
```

###### 通过数组的方法或属性访问
```swift
var data = [1, 2, 3, 4, 5]
data.isEmpty    // false
```

#### 修改数组
###### 清空数组
```swift
var data = [1, 2, 3, 4, 5]
data = []    // 清空数组
```

#### 遍历数组
##### 使用for-in遍历数组
```swift
    let data = [1, 2, 3, 4, 5]
    for item in data {
        print(item)
    }
```

###### 使用数组的enumerate()方法来遍历数组元素的索引和值
```swift
    for (index, value) in shopplingList.enumerate() {
        print("Item \(String(index + 1)): \(value)")
    }
```

#### Array与NSArray

### 字典 Dictionary

#### 创建字典

```swift
// 直接声明并创建
var namesOfIntegers = [Int: String]()

// 先声明再创建
var namesOfIntegers: [Int: String]
namesOfIntegers = [:]    // 创建了空字典

// 用字面量创建
var namesOfIntegers: [Int: String] = [0: "Zero", 1: "One", 2: "Two"]    // 显式声明字典类型
var namesOfIntegers = [0: "Zero", 1: "One", 2: "Two"]    // 隐式推断字典类型为[Int: String]
```

#### 访问字典
###### 数据项的个数
```swift
var aDictionary = ["name": "xiaoming", "gender": "male", "age": "15"]    // 推断字典类型为[String: String]
aDictionary.count // 3
```

###### 判断字典是否为空
> 其实就是判断字典数据项count是否为0

```swift
var aDictionary = ["name": "xiaoming", "gender": "male", "age": "15"]    // 推断字典类型为[String: String]
aDictionary.isEmpty    // false
```

###### 获取字典的某个键对应的值
可以通过下标方式获取字典对应的值，若下标对应的键值对存在则返回与其值同类型的可选值，若不存在则返回nil

```swift
var namesOfIntegers = [0: "Zero", 1: "One", 2: "Two"]
if let one = namesOfIntegers[1] {    // 通过下标访问返回的是与字典数据项的值同类型的一个可选值
    print("The name of 1 is \(one)")
}
```

#### 修改字典

###### 清空已有字典
```swift
var aDictionary = ["name": "xiaoming", "gender": "male", "age": "15"]    // 推断字典类型为[String: String]
aDictionary = [:]    // 清空字典
```

###### 删除一个键值对
有两种方法来删除一个键值对：
* 通过下标语法给字典的某个键的对应值赋值为`nil`来移除一个键值对
* 通过字典的`removeValueForKey(_: String)`来移除一个键值对。若对应键存在则返回其值，若不存在则返回`nil`

```swift
var airports: [String: String] = ["YYZ": "Toronto Pearon", "DUB": "Dublin"]

// 通过下标方式来删除一个键值对
airports["YYZ"] = nil

// 通过字典的方法来删除一个键值对
airports.removeValueForKey("DUB")

// 通过字典的方法来输出一个键值对，并判断是否成功
if let removedValue = airport.removeValueForKey("NYK") {
    print("The removed airport name is \(removedValue)")
} else {
    print("The airports does not contain a value for NYK")
}
```

###### 新增一个键值对

* 通过字典的方法`updateValue(_)`

```swift
var airports: [String: String] = ["YYZ": "Toronto Pearon", "DUB": "Dublin"]
airports["NYK"] = "XXXXX"

airports.updateValue("Dublin Airport" forKey: "DUB")

if let oldValue = airports.updateValue("Dublin Airport" forKey: "DUB") {
    print("The old value for DUB is \(oldValue)")
} else {
    print("")
}
```

###### 修改一个已有的键值对

```swift
var airports: [String: String] = ["YYZ": "Toronto Pearon", "DUB": "Dublin"]

// 通过下标方式修改
airports["DUB"] = "Dublin Airport"

// 通过字典的方法修改
if let oldValue = airports.updateValue("New Dublin Airport", forKey: "DUB") {
    print("Old value is \(oldValue)")
} else {
    print("The value does not exist before modifying the dictionary")
}
```

#### 遍历字典

###### 通过元组来遍历字典的一个键值对
```swift
var aDictionary = ["name": "xiaoming", "gender": "male", "age": "15"]
for (key, value) in aDictionary {
    print("Key: \(key), Value: \(value)")
}
```

###### 通过keys属性遍历字典的键
```swift
var aDictionary = ["name": "xiaoming", "gender": "male", "age": "15"]
for key in aDictionary.keys {
    print("Key: \(key)")
}
```
###### 通过values属性遍历字典的值

```swift
var aDictionary = ["name": "xiaoming", "gender": "male", "age": "15"]
for value in aDictionary.values {
    print("Value: \(value)")
}
```

#### Dictionary与NSDictionary

# 参考文档
- [The Swift Programming Language - Language Guide - Collection Types][swift-collections-types]

[swift-collections-types]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html