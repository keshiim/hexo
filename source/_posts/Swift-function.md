---
title: Swift：函数
date: 2017-11-10 13:24:20
categories:
    - 学习
    - Swift
tags:
    - Swift
---
**函数**是一个独立的代码块，用来执行特定的任务。通过给函数一个名字来定义它的功能，并且在需要的时候，通过这个名字来“调用”函数执行它的任务。

Swift 统一的函数语法十分灵活，可以表达从简单的无形式参数的 C 风格函数到复杂的每一个形式参数都带有局部和外部形式参数名的 Objective-C 风格方法的任何内容。

* 形式参数能提供一个*默认的值*来简化函数的调用
* 也可以被当作*输入输出形式参数被传递*
* 它在函数执行完成时修改传递来的变量。

Swift中函数也是一等公民，每个函数都有类型，由函数的形式参数类型和返回类型组成。可做用作其他函数的*参数*或*返回值*，同时也可以写在其他函数内部来在*内嵌范围封装有用的功能*。

### 定义和调用函数
当你定义了一个函数的时候，你可以选择定义一个或者多个命名的分类的值作为*函数的输入*（*所谓的形式参数*），并且/或者定义函数完成后将要传回作为*输出的值的类型*（*所谓它的返回类型*）。

每一个函数都有一个*函数名*，它描述函数执行的任务。要使用一个函数，你可以通过“调用”函数的名字并且传入一个符合*函数形式参数类型*的输入值（*所谓实际参数*）来调用这个函数。——给函数提供的实际参数的顺序必须符合函数的*形式参数列表顺序*。

下边栗子🌰中的函数叫做 `greet(person:)` ，跟它的功能一致——它接收一个人的名字作为输入然后返回对这个人的问候。要完成它，你需要定义一个输入形式参数——一个叫做 person 的 `String`类型值——并且返回一个 `String` 类型，它将会包含对这个人的问候：

```swift
func greet(person: String) -> String {
    let greeting = "Hello, " + person + "!"
    return greeting
}
print(greet(person: "zheng"))
```
这些信息都被包含在了函数的定义中，它使用一个 `func`的关键字前缀。你可以用一个返回箭头 `->` (一个连字符后面跟一个向右的尖括号)来明确函数*返回的类型*。
你可以通过在 `person` 标签后边给函数 `greet(person:)`传入一个用圆括号包裹的 `String`实际参数值来调用它，例如 `greet(person: "Anna")`。

为了简化这个函数的主体，我们将创建信息和返回语句组合到一行：

```swift
func greetAgain(person: String) -> String {
    return "Hello again, " + person + "!"
}
print(greetAgain(person: "Anna"))
// Prints "Hello again, Anna!"
```
### 函数的形式参数和返回值
在 Swift 中，函数的形式参数和返回值非常灵活。你可以定义从一个简单的只有一个未命名形式参数的工具函数到那种具有形象的参数名称和不同的形式参数选项的复杂函数之间的任何函数。
#### 无形式参数的函数
函数没有要求必须输入一个参数，这里有一个没有输入形式参数的函数，无论何时它被调用永远会返回相同的 `String`信息：

```swift
func sayHelloWorld() -> String {
    return "Hello, world"
}
print(sayHelloWorld())
```
函数的定义仍然需要在名字后边加一*个圆括号*，即使它不接受形式参数也得这样做。当函数被调用的时候也要在函数的名字后边*加一个空的圆括号*。
#### 多形式参数的函数
函数可以输入多个形式参数，可以写在函数后边的圆括号内，用**逗号分隔**。

这个函数以一个人的名字以及是否被问候过为输入，并返回对这个人的相应的问候：

```swift
func greetAgain(person: String) -> String {
    return "Hello again, \(person)!"
}
func greet(person: String, alreadyGreeted: Bool) -> String {
    if alreadyGreeted {
        return greetAgain(person: person)
    } else {
        return greet(person: person)
    }
}
print(greet(person: "Tim", alreadyGreeted: true))
// Prints "Hello again, Tim!"
```
通过在圆括号中传入带有 `person` 标签的 `String`实际参数值和带有 `alreadyGreeted` 标签的 `Bool`实际参数值来调用 `greet(person:alreadyGreeted:)`这个函数，实际参数之间用*逗号分隔*。注意这个函数与之前展示的函数` greet(person:) `是明显不同的。尽管两个函数都叫做 greet ， `greet(person:alreadyGreeted:)` 接收两个实际参数但 `greet(person:) `函数只接收一个。
#### 无返回值的函数
函数定义中没有要求必须有一个返回类型。下面是另一个版本的 `greet(person:)`函数，它将自己的 `String`值打印了出来而不是返回它：

```swift
func greet(person: String) {
    print("Hello, \(person)!")
}
greet(person: "Dave")
```
正因为它不需要返回值，函数在定义的时候就没有包含返回箭头`（->）`或者返回类型。
> 注意：严格来讲，函数 `greet(person:)`还是有一个返回值的，尽管没有定义返回值。没有定义返回类型的函数实际上会返回一个特殊的类型 `Void`。它其实是一个空的元组，作用相当于没有元素的元组，可以写作 `()`

当函数被调用时，函数的返回值可以被忽略：

```swift
 let _ = printAndCount(string: string)
```
#### 多返回值的函数
为了让函数返回多个值作为一个复合的返回值，你可以使用元组类型作为返回类型。

下面的栗子定义了一个叫做` minMax(array:)`的函数，它可以找到类型为 `Int`的数组中最大数字和最小数字。

```swift
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
print(minMax(array: [1, 2, -4, 3, 74]))
// prints (min: -4, max: 74)
```
函数 `minMax(array:)`返回了一个包含两个 `Int`值的元组。这两个值被 `min`和 `max` 标记，这样当查询函数返回值的时候就可以通过名字访问了。
#### 可选元组返回类型
如果元组在函数的返回类型中有可能“没有值”，你可以用一个*可选元组返回类型*来说明整个元组的可能是 `nil` 。书法是在可选元组类型的圆括号后边添加一个问号`（?）`例如 `(Int, Int)? ` 或者 `(String, Int, Bool)?` 。

上面的函数` minMax(array:)`返回了一个包含两个 `Int`值的元组。总之，函数不会对传入的数组进行安全性检查。如果 `array`的实际参数包含了一个空的数组，上面定义的函数 `minMax(array:)`在尝试调用 `array[0]`的时候就会触发一个运行时错误。

为了安全的处理这种“空数组”的情景，就需要把` minMax(array:)`函数的返回类型写做**可选元组**，当数组是空的时候返回一个 `nil`值：

```swift
func minMax2(array: [Int]) -> (min: Int, max: Int)? {
    guard !array.isEmpty else {
        return nil
    }
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
print(minMax2(array: [1, -3, 1, 2, 67]) ?? "") //prints (min: -3, max: 67)
print(minMax2(array: []) ?? "")                //prints ""
```
### 函数实际参数标签和形式参数名
每一个函数的形式参数都包含实际参数标签和形式参数名。*实际参数标签用在调用函数的时候*；在调用函数的时候每一个实际参数前边都要写实际参数标签。*形式参数名用在函数的实现当中*。**默认情况下，形式参数使用它们的形式参数名作为实际参数标签**。

```swift
func someFunction(firstParameterName: Int, secondParameterName: Int) {
    // In the function body, firstParameterName and secondParameterName
    // refer to the argument values for the first and second parameters.
}
someFunction(firstParameterName: 1, secondParameterName: 2)
```
左右的*形式参数*必须有**唯一**的名字。尽管有可能多个形式参数拥有相同的实际参数标签，唯一的实际参数标签有助于让你的代码更加易读。
#### 指定实际参数标签
在提供形式参数名之前写实际参数标签，用空格分隔：

```swift
func someFunction(argumentLabel parameterName: Int) {
    // In the function body, parameterName refers to the argument value
    // for that parameter.
}
someFunction(argumentLabel: xx)
```
#### 省略实际参数标签
如果对于函数的形式参数不想使用实际参数标签的话，可以利用下划线（ _ ）来为这个形式参数代替显式的实际参数标签。

```swift
func someFunction(_ firstParameterName: Int, secondParameterName: Int) {
    // In the function body, firstParameterName and secondParameterName
    // refer to the argument values for the first and second parameters.
}
someFunction(1, secondParameterName: 2)
```
#### 默认形式参数值
你可以通过在形式参数类型后给形式参数赋一个值来给函数的任意形式参数定义一个**默认值**。如果定义了默认值，你就可以在调用函数时候*省略这个形式参数*。

```swift
func someFunction(parameterWithDefault: Int = 12) {
    // In the function body, if no arguments are passed to the function
    // call, the value of parameterWithDefault is 12.
}
someFunction(parameterWithDefault: 6) // parameterWithDefault is 6
someFunction()                        // parameterWithDefault is 12
```
把不带有默认值的形式参数放到函数的形式参数列表中带有默认值的形式参数前边，不带有默认值的形式参数通常对函数有着重要的意义——把它们写在前边可以便于让人们看出来无论是否省略带默认值的形式参数，调用的都是同一个函数。
#### 可变形式参数
一个可变形式参数可以接受**零或者多个**特定类型的值。当调用函数的时候你可以利用可变形式参数来声明形式参数可以被传入值的数量*是可变的*。可以通过在形式参数的类型名称后边插入三个点符号（ ...）来书写可变形式参数。传入到可变参数中的值在函数的主体中被当作是**对应类型的数组**。

举个栗子，一个可变参数的名字是 `numbers`类型是 `Double...`在函数的主体中它会被当作名字是 `numbers` 类型是 `[Double]`的常量数组。

```swift
func arithmeticMean(numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5)
// returns 3.0, which is the arithmetic mean of these five numbers
arithmeticMean(3, 8.25, 18.75)
// returns 10.0, which is the arithmetic mean of these three numbers
```
> 注意：一个函数最多只能有一个可变形式参数。

#### 输入输出形式参数
你想函数能够修改一个形式参数的值，而且你想这些改变在函数结束之后*依然生效*，那么就需要将形式参数定义为*输入输出形式参数*。
在形式参数定义开始的时候在前边添加一个 `inout`关键字可以定义一个*输入输出形式参数*。输入输出形式参数有一个能输入给函数的值，函数能对其进行修改，还能输出到函数外边替换原来的值。

调用时，你只能把**变量**作为输入输出形式参数的实际参数。你不能用常量或者字面量作为实际参数，因为常量和字面量不能修改。在将变量作为实际参数传递给输入输出形式参数的时候，直接在它前边添加一个`(&) `*符号来明确可以被函数修改*。

这里有一个 `swapTwoInts(_:_:)`函数，它有两个输入输出整数形式参数 a和 b：

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```
你可以通过两个`Int`类型的变量来调用函数 `swapTwoInts(_:_:)`去调换它们两个的值，需要注意的是 `someInt`的值和 `anotherInt`的值在传入函数 `swapTwoInts(_:_:)`时都添加了'*和*'符号。

```swift
var someInt = 3
var anotherInt = 107
swap(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// prints "someInt is now 107, and anotherInt is now 3"
```
> 输入输出形式参数与函数的返回值不同。上边的 `swapTwoInts`没有定义返回类型和返回值，但它仍然能修改 `someInt`和 `anotherInt`的值。输入输出形式参数是函数能影响到函数范围外的另一种替代方式。

### 函数类型
每一个函数都有一个特定的**函数类型**，它由*形式参数类型，返回类型*组成。
栗子🌰：

```swift
func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}
func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}
```
上边的栗子定义了两个简单的数学函数 `addTwoInts`和 `multiplyTwoInts` 。这两个函数每个传入两个 `Int`值，返回一个 `Int`值，就是函数经过数学运算得出的结果。

这两个函数的类型都是 `(Int, Int) -> Int` 。也读作：
*“有两个形式参数的函数类型，它们都是 Int类型，并且返回一个 Int类型的值。”*

下边的另外一个栗子，一个没有形式参数和返回值的函数。

```swift
func printHelloWorld() {
    print("hello, world")
func printHelloWorld() {
    print("hello, world")
}
```
这个函数的类型是 `() -> Void`，或者 *“一个没有形式参数的函数，返回 `Void`。”*
#### 使用函数类型
你可以像使用 Swift 中的其他类型一样使用函数类型。例如，你可以给一个常量或变量定义一个函数类型，并且为变量指定一个相应的函数。

```swift
var mathFunction: (Int, Int) -> Int = addTwoInts
```
读作：*“定义一个叫做 `mathFunction`的变量，它的类型是‘一个能接受两个 `Int`值的函数，并返回一个 `Int`值。’将这个新的变量指向 `addTwoInts`函数。”*

可以利用名字 `mathFunction`来调用指定的函数。

```swift
print("Result: \(mathFunction(2, 3))")
// prints "Result: 5"
```
同样也适用于`multiplyTwoInts(_:_:)`
#### 函数类型作为形式参数类型
利用使用一个函数的类型例如 `(Int, Int) -> Int`作为其他函数的*> Int`*。这允许你预留函数的部分实现从而让函数的调用者在调用函数的时候提供。

下面的栗子打印出了上文中数学函数执行后的结果：

```swift
func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}
printMathResult(addTwoInts, 3, 5)
// Prints "Result: 8"
```
函数 `printMathResult(_:_:_:)`的作用就是当调用一个相应类型的数学函数的时候打印出结果。它并不关心函数在实现过程中究竟做了些什么，它只关心函数是不是正确的类型。这使得函数 `printMathResult(_:_:_:)`以一种类型安全的方式把自身的功能传递给调用者。

#### 函数类型作为返回类型
利用函数的类型作为另一个函数的返回类型。写法是在函数的返回箭头（ ->）后立即写一个完整的*函数类型*。
下边的栗子定义了两个简单函数叫做 `stepForward(_:)`和 `stepBackward(_:)`。函数 `stepForward(_:)`返回一个大于输入值的值，而 `stepBackward(_:)`返回一个小于输入值的值。这两个函数的类型都是 `(Int) -> Int`：

```swift
func stepForward(_ input: Int) -> Int {
    return input + 1
}
func stepBackward(_ input: Int) -> Int {
    return input - 1
}
```
这里有一个函数 `chooseStepFunction(backward:)`，它的返回类型是“一个函数的类型 `(Int) -> Int`”。函数 `chooseStepFunction(backward:)`返回了 `stepForward(_:)`函数或者一个基于叫做 `backwards` 的布尔量形式参数的函数 `stepBackward(_:)`：

```swift
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
    return backwards ? stepBackward : stepForward
}
var currentValue = 3
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the stepBackward() function
```
### 内嵌函数
到目前为止，你在本章中遇到的所有函数都是全局函数，都是在全局的范围内进行定义的。你也可以在函数的内部定义另外一个函数。这就是内嵌函数。

内嵌函数在默认情况下在外部是被隐藏起来的，但却仍然可以通过包裹它们的函数来调用它们。包裹的函数也可以返回它内部的一个内嵌函数来在另外的范围里使用。

你可以重写上边的栗子 `chooseStepFunction(backward:)`来使用和返回内嵌函数：

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1}
    func stepBackward(input: Int) -> Int { return input - 1}
    return backward ? stepBackward : stepForward
}
currentValue = -4
moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
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
```

> 摘自：[swift 官网](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/index.html#//apple_ref/doc/uid/TP40014097-CH3-ID0)
> 所有代码在Xcode9 Swift4 环境编译没问题，代码[戳这里 https://github.com/keshiim/LearnSwift4](https://github.com/keshiim/LearnSwift4)


