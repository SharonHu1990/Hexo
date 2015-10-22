title: Swift Cheat Sheet [1] — Basic Types
toc: true
date: 2015-10-21 10:28:31
tags: [iOS, Swift, 知识小集]
categories: [iOS]
---
# 常量和变量
## Varibales

    var myInt = 1 //inexplicit type
    var myExplicitInt : Int = 1 // explicit type
    var x = 1, y = 2, z = 3 //declare multiple integers
    myExplicitInt = 3 // set to another integer value
    
## Constants

    let myInt = 1
    myInt = 2 //compile-time error !!!
    
## 常量和变量的命名

    let π = 3.14159
    let 你好 = "你好世界"
    let 🐶🐮 = "dogcow" //可以用任何字符作为常量或变量名，包括Unicode字符
    
    
<!-- more -->
    
# 可选类型
可选类型，暗示常量或者变量可以没有值。


    let possibleNumber = "123"
    let convertedNumber = Int(possibleNumber)
    // convertedNumber 被推测为类型 "Int?"， 或者类型 "optional Int"
    
## nil
可以给可选变量赋值为nil来表示它没有值.

    var serverResponseCode: Int? = 404
    // serverResponseCode 包含一个可选的 Int 值 404
    serverResponseCode = nil
    // serverResponseCode 现在不包含值

如果你声明一个可选常量或者变量但是没有赋值，它们会自动被设置为nil

    var surveyAnswer: String?
    // surveyAnswer 被自动设置为 nil
    
注意：

> - Swift 的nil和 Objective-C 中的nil并不一样。在 Objective-C 中，nil是一个指向不存在对象的指针。
- 在Swift 中，nil不是指针——它是一个确定的值，用来表示值缺失。
- 任何类型的可选状态都可以被设置为nil，不只是对象类型。
- nil不能用于非可选的常量和变量。如果你的代码中有常量或者变量需要处理值缺失的情况，请把它们声明成对应的可选类型。


##  if 语句以及可选值的强制解析（forced unwrapping）
使用if语句和nil比较来判断一个可选值是否包含值
当你确定可选类型确实包含值之后，你可以在可选的名字后面加一个感叹号（!）来获取值

    var convertedNumber : Int? = 10
    if convertedNumber != nil{
        print("convertedNumber has an integer value of \(convertedNumber!)")
    }
    // 输出 "convertedNumber has an integer value of 10"

## 可选绑定（option binding）
使用可选绑定（optional binding）来判断可选类型是否包含值，如果包含就把值赋给一个临时常量或者变量。可选绑定可以用在if和while语句中，这条语句不仅可以用来判断可选类型中是否有值，同时可以将可选类型中的值赋给一个常量或者变量


### 一个示例解析
- 示例：

    let possibleNumber = "123"
    if let actualNumber = Int(possibleNumber){
        print("\'\(possibleNumber)\' has an integer value of \(actualNumber)")
    }else{
        print("\'\(possibleNumber)\' could not be convered to an integer")
    }
    
- 解释这个示例：
如果Int(possibleNumber)返回的可选Int包含一个值，创建一个叫做actualNumber的新常量并将可选包含的值赋给它。
如果转换成功，actualNumber常量可以在if语句的第一个分支中使用。它已经被可选类型包含的值初始化过，所以不需要再使用!后缀来获取它的值。在这个例子中，actualNumber只被用来输出转换结果。


### 包含多个可选绑定在条件判断语句中

    if let firstNumber = Int("4"), secondNumber = Int("42") where firstNumber < secondNumber
    {
        print("\(firstNumber) < \(secondNumber)")
    }
    // prints "4 < 42"
    
## 隐式解析可选类型（implicitly unwrapped optionals）
在Swift构造的过程中，当可选类型第一次赋值之后，就可以确定之后一直有值。这种情况下，可选类型的可选状态被定义为隐式解析可选类型。把可选类型后边的问号改为叹号。

    let possibleString: String? = "An optional string."
    let forcedString: String = possibleString! // 需要惊叹号来获取值
    
    let assumedString: String! = "An implicitly unwrapped optional string."
    let implicitString: String = assumedString  // 不需要感叹号

# 分号

    //Swift不强制要求在语句结尾处使用分号，当然，也可以按照自己的习惯添加
    //当在同一行内写多条独立的语句时，必须要用分号！
    let cat = "🐱";print(cat)

# 整数
## 整数范围
使用min和max属性获取整数的最小值和最大值

    let minValue = UInt8.min // minValue 为 0，是 UInt8 类型
    let maxValue = UInt8.max  // maxValue 为 255，是 UInt8 类型
    
## Int
- 在32位平台上，Int和Int32长度相同。
- 在64位平台上，Int和Int64长度相同。
- Int足够用了。

## UInt
- 在32位平台上，UInt和UInt32长度相同。
- 在64位平台上，UInt和UInt64长度相同。
- 尽量不要使用UInt

# 浮点数
- Double表示64位浮点数，至少15位小数点。当你需要存储很大或者很高精度的浮点数时请使用此类型。
- Float表示32位浮点数，至少6位小数点。精度要求不高的话可以使用此类型。


# String
## 操作符+

    var myString = "a"
    let myImmutableString = "c"
    myString += "b" // ab
    myString = myString + myImmutableString //abc
    myImmutableString += "d" //compile-time error!!!
    
## 字符串插值\\(value)

    let count = 7
    let message = "There are \(count) days in a week"
    
# Bool值在if语句中的应用

    let turnipsAreDelicious = false
    if turnipsAreDelicious {
        print("Mmm, tasty turnips!")
    }else {
        print("Eww, turnips are horrible.")
    }
    
# 元组
元组（tuples）把多个值组合成一个复合值。元组内的值可以是**任意类型**，并不要求是相同类型。

## 创建一个元组

    let http404Error = (404, "Not Found")
    //let http404Error = (404, "Not Found")
    
## 分解元组内容

    let http404Error = (404, "Not Found")
    let (statusCode, statusMessage) = http404Error
    print(("The status code is \(statusCode)"))
    // 输出 "The status code is 404"
    print("The status message is \(statusMessage)")
    // 输出 "The status message is Not Found"
    
## 用下划线_忽略一部分元组值
如果你只需要一部分元组值，分解的时候可以把要忽略的部分用下划线（_）标记：
    
    let (justTheStatusCode, _) = http404Error
    print("The status code is \(justTheStatusCode)")
    // 输出 "The status code is 404"
    
## 访问元组的单个元素

    print("The status code is \(http404Error.0)")
    // 输出 "The status code is 404"
    print("The status message is \(http404Error.1)")
    // 输出 "The status message is Not Found"
    
## 给元组的单个元素命名
    
    let http200Status = (statusCode: 200, description: "OK")
    
## 通过名字访问元组元素

    print("The status code is \(http200Status.statusCode)")
    // 输出 "The status code is 200"
    print("The status description is \(http200Status.description)")
    // 输出 "The status message is OK"

# 类型别名

    typealias AudioSample = UInt16
    //使用typealias关键字来定义类型别名
    var maxAmplitudeFound = AudioSample.min
    //maxAmplitudeFound 现在是 0
    



# 类型转换

## 整数和浮点数

### 整数 to 浮点数

    let three = 3
    let pointOneFourOneFiveNine = 0.14159
    let pi = Double(three) + pointOneFourOneFiveNine
    // pi 等于 3.14159，所以被推测为 Double 类型
    
### 浮点数 to 整数

    let integerPi = Int(pi)
    // integerPi 等于 3，所以被推测为 Int 类型

## 整数和字符串
#### Int to String

    let label = "The width is"
    let width = 94
    let widthLabel = label + String(width)// The width is 94
    
### String to Int

code1:

    var myString = "7" //7
    var possibleInt = Int(myString) //7
    print(possibleInt) //"Optional(7)\n"
    
code2:

    var myString1 = "banana" // "banana"
    var possibleInt1 = Int(myString1) //nil
    print(possibleInt1) // "nil\n"
    
    
# Printing

    let name = "Swift"
    println("Hello")
    pringln("My name is \(name)")
    print("See you")
    print(later)
    /*
        Hello
        My name is Swift
        See you later
    */
    
    
# Logical Operators

    var happy = true
    var sad = !happy//logical NOT,sad = false
    var everyoneHappy = happy && sad//logical AND, everyoneHappy = false
    var someoneHappy = happy || sad //logical OR, someoneHappy = true


# Functions

    func iAdd(a:Int,b:Int,c:Int) -> Int{
        return a + b + c
    }
    iAdd(1, b: 2, c: 3)//return 6
    
    
    func eitherSide(n:Int)-> (nMinusOne:Int, nPlusOne:Int){
        return(n-1, n+1)
    }
    eitherSide(5)//(.0 4, .1 6)
    

# Array

## 空数组

    // Creates an empty array.
    let emptyArray = [String]() // []
    
## 索引

    var ratingList = ["Poor", "Fine", "Good", "Excellent"]
    ratingList[1] = "k"
    ratingList // return ["Poor", "OK", "Good", "Excellent"]
    
## 拼接数组

    var colors = ["red", "blue"] //["red", "blue"]
    var moreColors: [String] = ["orange", "purple"] //["orange", "purple"]
    colors.append("green") //["red", "blue", "green"]
    colors += ["yellow"] //["red", "blue", "green", "yellow"]
    colors += moreColors //["red", "blue", "green", "yellow", "orange", "purple"]
    
## 添加和删除元素

    var days = ["mon", "thu"] 
    var firstDay = days[0] // mon
    days.insert("tue", atIndex: 1) // [mon, tue, thu]
    days[2] = "wed"  // [mon, tue, wed]
    days.removeAtIndex(0)  //[tue, wed]
    
# Dictionary

    var days = ["mon": "monday", "tue": "tuseday"]
    days["tue"] = "tuesday" // change the value for key "tue"
    days["wed"] = "wednesday" // add a new key/value pair
    
    var moreDays: Dictionary = ["thu": "thursday", "fri": "friday"]
    moreDays["thu"] = nil // remove thu from the dictionary
    moreDays.removeValueForKey("fri") // remove fri from the dictionary



    


