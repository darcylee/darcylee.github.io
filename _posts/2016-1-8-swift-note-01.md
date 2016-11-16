---
layout:     post
title:      "swift－Optional类型"
subtitle:   "swift学习笔记01, Optional类型"
date:       2016-1-8 15:19:30 +0800
header-img: "img/post-bg-default.jpg"
author:     darcylee
catalog:    true
tags:       [swift, 学习笔记]

---
使用可选类型（optionals）来处理值可能缺失的情况。可选类型表示：

* 有值，等于 x

或者

* 没有值

> C 和 Objective-C 中并没有可选类型这个概念。最接近的是 Objective-C 中的一个特性，一个方法要不返回一个对象要不返回nil，nil表示“缺少一个合法的对象”。然而，这只对对象起作用——对于结构体，基本的 C 类型或者枚举类型不起作用。对于这些类型，Objective-C 方法一般会返回一个特殊值（比如NSNotFound）来暗示值缺失。这种方法假设方法的调用者知道并记得对特殊值进行判断。然而，Swift 的可选类型可以让你暗示_任意类型_的值缺失，并不需要一个特殊值。

一个例子。Swift 的String类型有一种构造器，作用是将一个String值转换成一个Int值。然而，并不是所有的字符串都可以转换成一个整数。字符串"123"可以被转换成数字123，但是字符串"hello, world"不行。

下面的例子使用这种构造器来尝试将一个String转换成Int：

```swift
let possibleNumber = "123"
let convertedNumber = Int(possibleNumber)
// convertedNumber 被推测为类型 "Int?"， 或者类型 "optional Int"
```

因为该构造器可能会失败，所以它返回一个可选类型（optional）Int，而不是一个Int。一个可选的Int被写作Int?而不是Int。问号暗示包含的值是可选类型，也就是说可能包含Int值也可能不包含值。（不能包含其他任何值比如Bool值或者String值。只能是Int或者什么都没有。）

nil

你可以给可选变量赋值为nil来表示它没有值：

```swift
var serverResponseCode: Int? = 404
// serverResponseCode 包含一个可选的 Int 值 404
serverResponseCode = nil
// serverResponseCode 现在不包含值
```

> nil不能用于非可选的常量和变量。如果你的代码中有常量或者变量需要处理值缺失的情况，请把它们声明成对应的可选类型。

如果你声明一个可选常量或者变量但是没有赋值，它们会自动被设置为nil：

```swift
var surveyAnswer: String?
// surveyAnswer 被自动设置为 nil
```

> 注意：
Swift 的nil和 Objective-C 中的nil并不一样。在 Objective-C 中，nil是一个指向不存在对象的指针。在 Swift 中，nil不是指针——它是一个确定的值，用来表示值缺失。任何类型的可选状态都可以被设置为nil，不只是对象类型。

可以使用if语句和nil比较来判断一个可选值是否包含值

```swift
var optionalVal: String?

if optionalVal != nil {
    print("optionalVal contains some string")
} else {
    print("optionalVal is nothing!")
}
```

当你确定可选类型确实包含值之后，你可以在可选的名字后面加一个感叹号（!）来获取值。这被称为可选值的强制解析（forced unwrapping）：

```swift
var optionalVal: String?
optionalVal ＝ “123s” //如果没有值，则下面的if判断会出现编译错误
if let actValue = Int(optionalVal!) {
    print("\(optionalVal) has an integer value \(actValue)")
} else  {
    print("\(optionalVal) could not be converted to an integer")
}
```

还没明白，这样的一个可选属性的设置，在实际编程里面，主要用在什么样的情况下。

顺道提下swift里面对可选类型判断的运算符：

**空合运算符 (Nil Coalescing Operator)**

空合运算符(a ?? b)将对可选类型a进行空判断，如果a包含一个值就进行解封，否则就返回一个默认值b.这个运算符有两个条件:

* 表达式a必须是Optional类型
* 默认值b的类型必须要和a存储值的类型保持一致

空合运算符等同于下面的表达式：

```swift
a != nil ? a! : b
```

上述代码使用了三目运算符。当可选类型a的值不为空时，进行强制解封(a!)访问a中值，反之当a中值为空时，返回默认值b。无疑空合运算符(??)提供了一种更为优雅的方式去封装条件判断和解封两种行为，显得简洁以及更具可读性。

再一个例子

```swift
let defaultName = "xiaoli"
var userDefinedName: String?;   //默认值为 nil

var NameToUse = userDefinedName ?? defaultName
// userDefinedName 的值为空，所以 NameToUse 的值为 "xiaoli"

userDefinedName = "zhangsan";
var NameToUse1 = userDefinedName ?? defaultName
// userDefinedName 有值，则 NameToUse 的值为 ”zhangsan”
```
