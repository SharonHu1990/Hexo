title: Swift Cheat Sheet [3] - Functions
toc: true
date: 2015-11-03 19:42:53
tags: [Swift]
categories: [iOS]
---

# 无参函数

    func sayHelloWorld() -> String{
        return "hello, world"
    }
    print(sayHelloWorld())

<!--more-->


# 多参函数

    func sayHello(personName: String) -> String{
        return "hello, \(personName)"
    }
    
    func sayHelloAgain(personName: String) -> String{
        return "hello again, \(personName)"
    }
    
    func sayHello(personName: String, alreadyGreeted: Bool) -> String {
        if alreadyGreeted{
            return sayHelloAgain(personName)
        }else{
            return sayHello(personName)
        }
    }
    
    print(sayHello("Sharon", alreadyGreeted: true))


# 无返回值函数

    func sayGoodBye(personName:String){
        print("Goodbye, \(personName)")
    }
    sayGoodBye("Sharon")


## 函数的返回值可以被忽略

    func printAndCount(stringToPrint: String) -> Int {
        print(stringToPrint)
        return stringToPrint.characters.count
    }
    
    //函数的返回值可以被忽略
    func printWithoutCounting(stringToPrint : String) {
        printAndCount(stringToPrint)
    }
    
    printAndCount("hello, world")
    printWithoutCounting("hello, world")


# 多重返回值函数(使用元组类型）

    func minMax(array: [Int]) -> (min: Int, max: Int){
        var currentMin = array[0]
        var currentMax = array[0]
        for value in array[1..<array.count]{
            if value < currentMin {
                currentMin = value
            }else if value > currentMax {
                currentMax = value
            }
        }
        return (currentMin, currentMax)
    }
    
    let bounds = minMax([8, -6, 2, 109, 3, 71])
    print("min is \(bounds.min) and max is \(bounds.max)")
    //prints "min is -6 and max is 109"


## 可选元组返回类型

    func minMax2(array: [Int]) -> (min: Int, max: Int)? {
        if array.isEmpty {return nil}
        var currentMin = array[0]
        var currentMax = array[0]
        for value in array[1..<array.count] {
            if value < currentMin {
                currentMin = value
            }else if value > currentMax {
                currentMax = value
            }
        }
        return (currentMin, currentMax)
    }
    
    if let bounds = minMax2([8, -6, 2, 109, 3, 71]) {
        print("min is \(bounds.min) and max is \(bounds.max)")
    }//prints "min is -6 and max is 109"



# 指定外部参数名
你可以在局部参数名前指定外部参数名，局部参数名和外部参数名之间以空格分隔：

    func sayHello(to person:String, and anotherPerson:String) -> String{
        return ("hello \(person) and \(anotherPerson)")
    }
    print(sayHello(to: "Sharon", and: "XiaoYang"))


# 忽略外部参数名
如果你不想为第二个以及后续的参数设置外部参数名，用一个下划线代替一个明确的参数名
**注意**：第一个参数默认忽略其外部参数名称，所以显示地写下划线是多余的

    func someFunction(firstParameterName: Int, _ secondParameterName: Int) {
        // function body goes here
        // firstParameterName and secondParameterName refer to
        // the argument values for the first and second parameters
    }
    someFunction(1, 2);


# 默认参数值
当默认参数值被定义后，调用这个函数可以忽略这个参数

    func someFunction(parameterWithDefault: Int = 12) {
        // function body goes here
        // if no arguments are passed to the function call,
        // value of parameterWithDefault is 12
    }
    someFunction(6)//parameterWithDefault is 6
    someFunction()//parameterWithDefault is 12


# 可变参数
在函数类型后面加...来表示可变参数
**注意：一个函数最多只能有一个可变参数**
下面的函数用来计算一组任意长度数字的`算术平均值`

    func arithmeticMean(numbers: Double...) -> Double {
        var total : Double = 0
        for number in numbers {
            total += number
        }
        return total / Double(numbers.count)
    }
    
    arithmeticMean(1,2,3,4,5)//prints 3
    arithmeticMean(4,5,6,7,8)//prints 6


# 常量参数和变量参数
通过在参数名前加关键字`var`来定义变量参数：

    func alignRight(var string : String, totalLength: Int, pad: Character) -> String {
        let amountToPad = totalLength - string.characters.count
        if amountToPad < 1 {
            return string
        }
        let padString = String(pad)
        for _ in 1...amountToPad {
            string = padString + string
        }
        return string
    }
    let originalString = "hello"
    let paddedString = alignRight(originalString, totalLength: 10, pad: "-")
    //paddedString is eaual to "-----hello"
    //originalString is still equal to "hello"


# 输入输出参数(inout关键字)
变量参数仅仅能在函数体内被更改。输入输出参数对参数值得修改，在函数体调用完成后依然存在
定义一个输入输出参数时，在参数定义前加 `inout` 关键字。只能传递变量给输入输出参数。
输入输出参数不能有默认值，而且可变参数不能用 inout 标记。如果你用 `inout` 标记一个参数，这个参数不能被 `var` 或者 `let` 标记。

    func swapTwoInts(inout a: Int, inout _ b: Int) {
        let temporary = a
        a = b
        b = temporary
    }
    
    //调用
    var someInt = 3
    var anotherInt = 204
    swapTwoInts(&someInt, &anotherInt)
    //Swift 本来就有这个函数： swap(&someInt, &anotherInt)


# 函数类型的使用
函数类型由它的参数类型和返回值类型组成
在Swift中，使用函数类型就像使用其他类型一样。例如，你可以定义一个类型为函数的常量或者变量，并将适当的函数赋值给它

    func addTwoInts (someInt : Int, anotherInt: Int) -> Int {
        return someInt + anotherInt
    }
    
    func multiplyTwoInts (someInt: Int, anotherInt: Int) -> Int {
        return someInt * anotherInt
    }
    var mathFunction1:(Int, Int) -> Int = addTwoInts
    var mathFunction2:(Int, Int) -> Int = multiplyTwoInts
    print("Result:\(mathFunction1(2,3))")//prints "Result:5"
    print("Result:\(mathFunction2(2,3))")//prints "Result:6"


## 函数类型作为参数类型

    func printMathResult(mathFunction : (Int, Int) -> Int, _ a: Int, _ b :Int) {
        print("Result: \(mathFunction(a, b))")
    }
    
    printMathResult(addTwoInts, 2, 3)//prints "Result:5"


# 函数类型作为返回类型

箭头后面写上一个完整的函数类型即可

    func stepForward(input: Int) -> Int {
        return input + 1
    }
    func stepBackward(input: Int) -> Int {
        return input - 1
    }
    
    //定义一个函数类型返回值的函数
    func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
        return backwards ? stepBackward : stepForward
    }
    
    var currentValue = 3
    let moveNearerToZero = chooseStepFunction(currentValue > 0)//计算出从 currentValue 逐渐接近到0是需要向正数走还是向负数走
    
    print("Counting to zero")
    while currentValue != 0 {
        print("\(currentValue)...")
        currentValue = moveNearerToZero(currentValue)
    }
    
    print("zero!")
    //3...
    //2...
    //1...
    //zero!


# 嵌套函数
将函数定义在别的函数体中。默认情况下，嵌套函数对外界是不可见的，但是，它可以被它的外围函数调用

    func chooseStepFunction1(backward: Bool) -> (Int) -> Int {
        func stepForward(input: Int) -> Int {
            return input + 1
        }
        func stepBackground(input: Int) ->Int{
            return input - 1
        }
        return backward ? stepBackground : stepForward
    }
    
    var currentValue1 = -4
    let moveNearerToZero1 = chooseStepFunction1(currentValue > 0)
    while currentValue1 != 0 {
        print("\(currentValue1)")
        currentValue1 = moveNearerToZero1(currentValue1)
    }
    print("zero!")
    // -4...
    // -3...
    // -2...
    // -1...
    // zero!









