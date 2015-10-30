title: Swift Cheat Sheet [2] - Control Flow
toc: true
date: 2015-10-22 10:28:13
tags:
categories:
---
# If 条件语句

## if else
    let number = 100
    if number < 10 {
        print("The number is small")
    } else if number > 100 {
        print("The number is pretty big")
    } else {
        print("The number is between 10 and 100")
    }

<!-- more -->
    
    
## if else + for in

    let individualScores = [75, 43, 103, 87, 12]
    var teamScore = 0
    for score in individualScores {
        if score > 50 {
            teamScore += 3
        } else {
            teamScore += 1
        }
    }
    print(teamScore)
    
    
## 使用可选绑定

    var optionalName: String?
    var greeting = "Hello!"
    if let name = optionalName {
        greeting = "Hello, \(name)"
    }else
    {
        print("optionalName is nil")
    }
    
## 在if条件判断语句中使用where关键字

    var optionalHello: String? = "Hello"
    if let hello = optionalHello where hello.hasPrefix("H"), let name = optionalName {
        greeting = "\(hello), \(name)"
        print("greeting:\(greeting)")
    }else
    {
        //跳到这里，因为hello还没有被复制，它没有“H”前缀，仅当where模式匹配成功，if条件语句才执行。
    }
    
# Switch

## 一条case分支可匹配多个模式

    let vegetable = "red pepper"
    switch vegetable {
        case "celery":
            let vegetableComment = "Add some raisins and make ants on a log."
        case "cucumber", "watercress":
            let vegetableComment = "That would make a good tea sandwich."
        case let x where x.hasSuffix("pepper"):
            let vegetableComment = "Is it a spicy \(x)?"
        default://必须有default分支
            let vegetableComment = "Everything tastes good in soup."
    }
    
## 不存在隐式的贯穿
下面的代码会有编译错误！

    let anotherCharacter: Character = "a"
    switch anotherCharacter {
    case "a":
    case "A":
        print("The letter A")
    default:
        print("Not the letter A")
    }
    // this will report a compile-time error
    
## 区间匹配

使用闭区间操作符`..`或开区间操作符`..<`

    let approximateCount = 62
    let countedThings = "moons orbiting Saturn"
    var naturalCount: String
    switch approximateCount {
    case 0:
        naturalCount = "no"
    case 1..<5:
        naturalCount = "a few"
    case 5..<12:
        naturalCount = "several"
    case 12..<100:
        naturalCount = "dozens of"
    case 100..<1000:
        naturalCount = "hundreds of"
    default:
        naturalCount = "many"
    }
    print("There are \(naturalCount) \(countedThings).")
    // 输出 "There are dozens of moons orbiting Saturn."
    
## 使用元组

    let somePoint = (1, 1)
    switch somePoint {
    case (0, 0):
        print("(0, 0) is at the origin")
    case (_, 0):
        print("(\(somePoint.0), 0) is on the x-axis")
    case (0, _):
        print("(0, \(somePoint.1)) is on the y-axis")
    case (-2...2, -2...2):
        print("(\(somePoint.0), \(somePoint.1)) is inside the box")
    default:
        print("(\(somePoint.0), \(somePoint.1)) is outside of the box")
    }
    // 输出 "(1, 1) is inside the box"
    
> 如果存在多个匹配，那么只会执行第一个被匹配到的 case 分支。剩下的能够匹配 case 分支都会被忽视掉.

## case分支中使用值绑定

    let anotherPoint = (2, 0)
    switch anotherPoint {
    case (let x, 0):
        print("on the x-axis with an x value of \(x)")
    case (0, let y):
        print("on the y-axis with a y value of \(y)")
    case let (x, y):
        print("somewhere else at (\(x), \(y))")
    }
    // 输出 "on the x-axis with an x value of 2"
    
## 使用where 模式匹配

    let yetAnotherPoint = (1, -1)
    switch yetAnotherPoint {
    case let (x, y) where x == y:
        print("(\(x), \(y)) is on the line x == y")
    case let (x, y) where x == -y:
        print("(\(x), \(y)) is on the line x == -y")
    case let (x, y):
        print("(\(x), \(y)) is just some arbitrary point")
    }
    // 输出 "(1, -1) is on the line x == -y"
    
    
# For循环

#使用For-In
    var firstForLoop = 0
    for i in 0..<4 {
        firstForLoop += i
    }
    print(firstForLoop)
    
    
    var secondForLoop = 0
    for _ in 0...4 {
        secondForLoop += 1
    }
    print(secondForLoop)
    
## 使用下划线_替代循环变量名
如果你不需要知道区间内每一项的值，你可以使用下划线（_）替代变量名来忽略对值的访问：
code:

    let base = 3
    let power = 10
    var answer = 1
    for _ in 1...power {
        answer *= base
    }
    print("\(base) to the power of \(power) is \(answer)")
    // 输出 "3 to the power of 10 is 59049"
    
code2:

    var secondForLoop = 0
    for _ in 0...4 {
        secondForLoop += 1
    }
    print(secondForLoop)
    //输出5
    
## 遍历数组元素

    let names = ["Anna", "Alex", "Brian", "Jack"]
    for name in names {
        print("Hello, \(name)!")
    }
    // Hello, Anna!
    // Hello, Alex!
    // Hello, Brian!
    // Hello, Jack!
    
## 遍历字典的键值对

字典元素的遍历顺序和插入顺序可能不同

    let numberOfLegs = ["spider": 8, "ant": 6, "cat":4]
    for (animalName, legCount) in numberOfLegs{
        print("\(animalName)s have \(legCount) legs")
    }
    // ants have 6 legs
    // cats have 4 legs
    // spiders have 8 legs
    
## 使用条件判断和递增方法的标准 C 样式for循环

    for var index = 0; index < 3; ++index {
        print("index is \(index)")
    }
    // index is 0
    // index is 1
    // index is 2
    
# While循环

## while

    var count = 1
    while count < 3 {
        println("count is \(count)")
        ++count
    }
    // count is 1
    // count is 2
    
    count = 1
    while count < 1 {
        println("count is \(count)")
        ++count
    }

## repeat-while

    var count = 1
    repeat {
        print("count is \(count)")
        ++count
    } while count < 3
    // count is 1
    // count is 2
    
    count = 1
    repeat {
        print("count is \(count)")
        ++count
    } while count < 1
    // count is 1
    
    



    



