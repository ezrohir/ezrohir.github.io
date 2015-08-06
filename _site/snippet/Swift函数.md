## 函数
在 `Swift` 中,每个函数都有一种类型,包括函数的参数值类型和返回值类型.这养能把函数当别的函数
的参数,也可以从其他函数中返回函数.

最基本的函数定义
```
func sayHello(personName: String) -> String {
    let greeting = "Hello, " + personName + "!"
    return greeting
}
```

多重输入参数
```
func halfOpenRangeLength(start: Int, end: Int) -> Int {
    return end - start
}
print(halfOpenRangeLength(1, 10))
```
无参函数
```
func sayHelloWorld() -> String {
    return "hello, world"
}
print(sayHelloWorld())
```
无返回值函数
```
func sayGoodbye(personName: String) {
    print("Goodbye, \(personName)!")
}
```
多重返回值函数
```
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```
默认参数值
```
func someFunction(parameterWithDefault: Int = 12) {
    // function body goes here
    // if no arguments are passed to the function call,
    // value of parameterWithDefault is 42
}
```
可变参数
```
func arithmeticMean(numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
```
### 函数类型
#### 基本定义
每个函数都有种特定的函数类型,由函数的参数类型和返回类型组成.
```
func addTwoInts(a: Int, _ b: Int) -> Int {
    return a + b
}
func multiplyTwoInts(a: Int, _ b: Int) -> Int {
    return a * b
}

func printHelloWorld() {
    print("hello, world")
}
```
前俩函数的函数类型是 `(Int,Int)->Int`,后一个函数的类型是 `()->void`.
使用函数类型,可以定义一个类型为函数的常量或者变量,并将函数赋值给它:
```
var mathFunction: (Int,Int) -> Int = addTwoInts
```
已上定义了一个叫 `mathFunction` 的变量,类型是一个有两个 `Int` 型的参数并返回一个 `Int` 型
的值的函数,并让新变量指向 `addTwoInts` 函数.

#### 函数类型作为参数类型
可以用 `(Int,Int) -> Int ` 这样的函数类型作为另外一个函数的参数类型.
```
func printMathResult(mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}
printMathResult(addTwoInts, 3, 5)
```
这个例子定义了 `printMathResult(_:_:_:)` 函数,它有三个参数: 第一个参数是 `mathFunction`,
类型是 `(Int,Int)->Int`,第二个和第三个参数叫 `a` `b` ,他们都是 `Int` 类型.

#### 函数类型作为返回类型
```
func stepForward(input: Int) -> Int {
    return input + 1
}
func stepBackward(input: Int) -> Int {
    return input - 1
}
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
    return backwards ? stepBackward : stepForward
}
```
#### 嵌套函数
已上所有函数都是全局函数,它们定义在全局中.也可以把函数定义在别的函数体中,称为嵌套函数.
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backwards ? stepBackward : stepForward
}
var currentValue = -4
let moveNearerToZero = chooseStepFunction(currentValue > 0)
// moveNearerToZero now refers to the nested stepForward() function
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
