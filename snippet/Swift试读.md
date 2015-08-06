## Swift 试读
### 简单值
`let` 申明常量,用 `var` 来申明变量.
不用明确申明类型,申明的同时赋值的话,编译器会自动推断类型.
如果初始值没有提供足够的信息,需要在变量后面申明类型,用冒号分割.
```
let implicitInterger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70
```
值不会被隐私转换为其他类型,需要显示转换
```
let label = "the width is"
let width = 94
let widthLabel = label + String(width)
```

使用方括号 `[]` 来创见数组和字典,并使用下表或者建(key)来访问元素.
```
var shoppingList = ["catfish","water","tulips","blue paint"]
shoppingList[1] = "bottle of water"
```
```
var occupations = [
    "Malcolm": "Caption",
    "Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"
```
创见一个空数组或者字典
```
let emptyArray = [String]()
let emptyDictionary = [String: Float]()
```

### 控制流
可以一起使用 `if` 和 `let` 来处理值缺失的情况.这些值可由可选值来代表.一个可选值是一个具体的值
或者是 `nil` .
```
var optionalName: String? = "hello"
var greeting = "hello!"
if let name = optionalName {
    greeting = "hello,\(name)"
}

```
使用 `for-in` 来遍历字典
```
let interestingNumbers = [
    "Prime": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square": [1, 4, 9, 16, 25],
]
var largest = 0
for (kind, numbers) in interestingNumbers {
    for number in numbers {
        if number > largest {
            largest = number
        }
    }
}
print(largest)
```
在循环中使用..<来表示范围
```
var firstForLoop = 0
for i in 0..<4 {
    firstForLoop += i
}
print(firstForLoop)

var secondForLoop = 0
for var i = 0; i < 4; ++i {
    secondForLoop += i
}
print(secondForLoop)
```

### 函数和闭包
用 `func` 来申明一个函数,使用名字和参数来调整函数.使用 `->` 来制定函数返回值类型.
```
func greet(name: String,day: String) -> String{
  return "hello \(name),today is \(day)."
}
```
使用元组来让一个函数返回多个值.
```
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
    var min = scores[0]
    var max = scores[0]
    var sum = 0

    for score in scores {
        if score > max {
            max = score
        } else if score < min {
            min = score
        }
        sum += score
    }

    return (min, max, sum)
}
let statistics = calculateStatistics([5, 3, 100, 3, 9])
print(statistics.sum)
print(statistics.2)
```

函数是第一等类型,意味着函数可以作为另外一个函数的返回值.
```
func makeIncreamenter() -> (Int -> Int){
  func addOne(number: Int) -> Int{
    return 1 + number
  }
  return addOne
}
var increment = makeIncreamenter()
increment(7)
```
函数也可以作为参数传入另一个函数.
```
func hasAnyMatches(list: [Int], condition: Int -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(numbers, condition: lessThanTen)
```

函数实际上是一种特殊的闭包:它是一段能之后被掉取的代码.闭包中的代码能访问闭包所建作用域
中能得到的变量和函数.可以使用 `{}` 来创见 一个匿名闭包.使用 `in` 讲参数和返回值类型申明
与闭包函数体分离.
```
numbers.map({
    (number: Int) -> Int in
    let result = 3 * number
    return result
})
```
