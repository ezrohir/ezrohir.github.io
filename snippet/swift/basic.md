##　概述
+ 使用 `let` 来申明常量 `var` 申明变量.
+ 使用 `\()` 把值转换成字符串.
  ```
  let apples =3
  let appleSummary = "i have \(apples) apples"
  ```
+ 关于数组,字典
  ```
  使用 `[]` 来创建数组和字典
  var shoppingList = ["catfish", "water", "tulips"]

  var occupations = [
    "Malcolm": "Captian",
    "kaylee": "Mechanic",
  ]

  // 创建空数组,字典 []  [:]
  let emptyArray = [String]()
  let emptyDictionary = [String: Float]()
  ```

+ 可以在循环中使用 `..<` 来表示范围,但不包换上界,如果想包含的话需要使用 '...'
  ```
  var firstForLoop = 0
  for i in 0..<4{
      firstForLoop += i
  }
  ```

+ 函数和闭包
  ```
  使用 `func` 来申明ige函数
  fun greet(name: String, day: String) -> String{
      return "Hello \(name), today is \(day)."
  }
  greet("Bob",day: "Tuesday")
  ```


## 字符串
+ 与 `Swift` 中其他值一样,能否更改字符串的值,取决于其被定义为常量还是变量.
+ 初始化空字符串
  ```
  var emptyString = ""                 // 空字符串面量
  var anotherEmptyString = String()    // 初始化方法
  // 两个字符串均为空并等价
  ```
+ 字符串可变性
  ```
  var variableString = "Horse"
  variableString += "and carriage"
  ```
+ 字符串是值类型,当其进行常量,变量赋值操作,或者在函数/方法中传递时,会进行值拷贝.
+ 字符串/字符可以用等于与操作符 `(==)` 比较文本值

## 属性
Swift 中的属性没有对应的实例额变量,属性的后端存储也无法直接访问.

##　函数
```
// 标准函数
func sayHello(personName:String) -> String {
    let greeting = "hello," + personName + "!"
    return greeting;
}
// 无参函数
func sayHello() -> String{
    return ...
}
// 多参数函数
func sayHello(personName: String, alreadyGreeted: Bool) -> String{
    return ...
}
// 无返回函数
func sayHello(personName:String) -> String{

}
// 多重返回值函数
func minMax(array: [Int]) -> (min: Int, max: Int)
{}
// 函数参数名
func someFunction(firstParameterName: Int, secondParameterName: Int) {

}
someFunction(1, secondParameterName: 2)
```
+ 函数参数默认是常量.视图在函数体中更改参数会导致编译错误.通过在参数名前加关键字 'var' 来定义变量参数.

## 类与结构体
### 恒等运算符
如果能判定两个常量或者变量是否应用同一个类实例将会很有帮助.为此, Swift 内建了两个恒等运算符
+ 等价与 (`===`)
+ 不等价与 (`!==`)

## 构造过程
+ Swift 的构造器无需返回值,它们的主要任务是保证新实例在第一次使用前完成正确的初始化.
+ 可选属性类型.可选类型的实行将自动初始化为 `nil`
  ```
  class SurveyQuestion{
      var text: String
      var responese: String?
      init (text: String){
          self.text = text;
      }
      func ask(){}
  }
  ```
+ 构造过程中常量属性的修改.可以在构造过程中的任意时间点修改常量属性的值,只要在构造过程结束时是一个确定的值.一旦常量属性被赋值,它将永远不可更改.对于类的实例来说,它的常量属性只能在定义它的类的构造过程中修改;不能在子类中修改.
+ 默认构造器.如果结构体或者类的所有属性都有默认值,同时没有自定义的构造器,那么 Swift 会给这些结构体或者类提供一个默认构造器.这个默认构造器将简单的创见一个所有属性值都设置为默认值的实例.

## 值类型
在 Swift 中,所有的基本类型: 整数(Integer) 浮点数(floating-point) 布尔值(Boolean) 字符串(String)
数组(array) 和字典(dictionary) 都是值类型,底部都是已结构体的形式所实现.
