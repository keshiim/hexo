---
title: Swift：闭包
date: 2017-11-10 15:51:44
categories:
    - 学习
    - Swift
tags:
    - Swift
---

### What is closure?
*闭包*是可以在你的代码中被传递和引用的*功能性独立*模块。Swift 中的闭包和 C  以及 Objective-C 中的 `blocks` 很像，还有其他语言中的匿名函数也类似。

闭包能够捕获和存储定义在其上下文中的任何常量和变量的引用，这也就是所谓的*闭合并包裹*那些常量和变量，因此被称为“闭包”，Swift 能够为你处理所有关于捕获的内存管理的操作。

在函数章节中有介绍的全局和内嵌函数，实际上是*特殊的闭包*。闭包符合如下*三种形式*中的一种：

* 全局函数是一个有名字但不会捕获任何值的闭包；
* 内嵌函数是一个有名字且能从其上层函数捕获值的闭包；
* 闭包表达式是一个轻量级语法所写的可以捕获其上下文中常量或变量值的没有名字的闭包。

Swift 的闭包表达式拥有简洁的风格，鼓励在常见场景中实现简洁，无累赘的语法。常见的优化包括：

* 利用上下文推断形式参数和返回值的类型；
* 单表达式的闭包可以隐式返回；
* 简写实际参数名；
* 尾随闭包语法。

### 闭包表达式
闭包表达式是一种在简短行内就能写完闭包的语法。闭包表达式为了缩减书写长度又不失易读明晰而提供了一系列的语法优化。下边的闭包表达式栗子通过使用几次迭代展示 `sorted(by:)`方法的精简来展示这些优化，每一次都让相同的功能性更加简明扼要。
#### Sorted 方法
Swift 的标准库提供了一个叫做 `sorted(by:)` 的方法，会根据你提供的排序闭包将已知类型的数组的值进行排序。一旦它排序完成， `sorted(by:)` 方法会返回与原数组类型大小完全相同的一个新数组，该数组的元素是已排序好的。原始数组不会被` sorted(by:)` 方法修改。

这是将被排序的初始数组：

```swift
let names = ["Chris","Alex","Ewa","Barry","Daniella"]
```

`sorted(by:)` 方法接收一个接收两个与数组内容相同类型的实际参数的闭包，然后返回一个 `Bool` 值来说明第一个值在排序后应该出现在第二个值的前边还是后边。如果第一个值应该出现在第二个值之前，排序闭包需要返回 `true` ，否则返回 `false`
这个栗子对一个 `String` 类型的数组进行排序，因此排序闭包需为一个 `(String, String) -> Bool` 的类型函数。

提供符合其类型需求的普通函数，并将它作为 `sorted(by:) `方法的形式参数传入：

```swift
func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}
var reversedNames = names.sorted(by: backward)
// reversedNames is equal to ["Ewa", "Daniella", "Chris", "Barry", "Alex"]
```
如果第一个字符串 `s1` 大于第二个字符串 `s2`， `backwards(_:,_:) `函数将返回 `true`，这意味着 `s1` 应该在排序数组中排在 `s2`  的前面。以此类推，实现倒叙排列。

总之，这样来写本质上只是一个单一表达函数`( a > b )`是非常啰嗦的。在这个栗子中，我们更倾向于使用闭包表达式在行内写一个简单的闭包。
#### 闭包表达式语法
闭包表达式语法有如下的一般形式：

```swift
{ (parameters) -> (return type) in
   statements
}
```
闭包表达式语法能够使用*常量形式参数、变量形式参数*和*输入输出形式*参数，但*不能*提供默认值。*可变形式参数*也能使用，但需要在形式参数列表的*最后面使用*。*元组*也可被用来作为形式参数和返回类型。

下面这个栗子展示一个之前` backward(_:_:) `函数的闭包表达版本：

```swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})
print(reversedNames)
// prints ["Ewa", "Daniella", "Chris", "Barry", "Alex"]
```

**需要注意的**是*行内闭包的形式*参数类型和返回类型的声明与 `backwards(_:_:)` 函数的声明相同。在这两个方式中，都书写成 `(s1: String, s2: String) -> Bool`。总之对于行内闭包表达式来说，形式参数类型和返回类型都应写在*花括号内*而不是*花括号外面*。

闭包的**函数整体部分**由关键字 `in` 导入，这个关键字表示闭包的*形式参数类型和返回类型*定义已经完成，并且闭包的函数体即将开始。

闭包的函数体特别短以至于能够只用一行来书写：

```swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2 } )
```
#### 从语境中推断类型
因排序闭包为实际参数来传递给函数，故 Swift 能推断它的形式参数类型和返回类型。 `sorted(by:)` 方法期望它的第二个形式参数是一个 `(String, String) -> Bool` 类型的函数。这意味着` (String, String)`和 `Bool` 类型不需要被写成闭包表达式定义中的一部分，因为所有的类型都能被推断，返回箭头 `(->)` 和围绕在形式参数名周围的括号也能被省略：

```swift
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 })
```
当把闭包作为行内闭包表达式传递给函数，形式参数类型和返回类型都可以被*推断出来*。所以说，当闭包被用作函数的实际参数时你都不需要用完整格式来书*写行内闭包*。

然而，如果你希望的话仍然可以明确类型，并且在读者阅读你的代码时如果它能避免可能存在的歧义的话还是值得的。在这个 `sorted(by:)` 方法的栗子中，闭包的目的很明确，即排序被替换。对读者来说可以放心的假设闭包可能会使用 `String` 值，因为它正帮一个字符串数组进行排序。
#### 从单表达式闭包隐式返回
单表达式闭包能够通过从它们的声明中删掉 `return` 关键字来隐式返回它们单个表达式的结果，前面的栗子可以写作：

```swift
reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
```
这里， `sorted(by:)` 方法的第二个实际参数的函数类型已经明确必须通过闭包返回一个 `Bool` 值。因为闭包的结构包涵返回 `Bool` 值的单一表达式 `(s1 > s2)`，因此没有歧义，并且 `return` 关键字能够被省略。
#### 简写的实际参数名
Swift 自动对行内闭包提供**简写实际参数名**，你也可以通过 `$0 , $1 , $2` 等名字来引用闭包的实际参数值。

如果你在闭包表达式中使用这些简写实际参数名，那么你可以在闭包的实际参数列表中忽略对其的定义，并且简写实际参数名的数字和类型将会从期望的函数类型中推断出来。 `in`  关键字也能被省略，因为闭包表达式完全由它的函数体组成：

```swift
reversedNames = names.sorted(by: { $0 > $1 } )
```
#### 运算符函数
实际上还有一种更简短的方式来撰写上述闭包表达式。Swift 的 `String` 类型定义了关于大于号`（ >）`的特定字符串实现，让其作为一个有两个 `String` 类型形式参数的函数并返回一个 `Bool` 类型的值。这正好与 ` sorted(by:)` 方法的第二个形式参数需要的函数相匹配。因此，你能简单地传递一个大于号，并且 Swift 将推断你想使用大于号特殊字符串函数实现：

```swift
reversedNames = names.sorted(by: >)
```
### 尾随闭包
如果你需要将一个很长的闭包表达式作为函数`最后一个实际参数`传递给函数，使用尾随闭包将增强函数的可读性。尾随闭包是一个被书写在函数形式参数的*括号外面*（后面）的闭包表达式：

```swift
func someFunctionThatTakesAClosure(closure: () -> Void) {
    //function body goes here
}
//here`s how you call this function without using a trailing closure
someFunctionThatTakesAClosure(closure: {
    //closure`s body goes here
})
//here`s how you call this function with a trailing closure instead
someFunctionThatTakesAClosure {
    //trailing closure`s body goes here
}
```
字符串排列闭包也可以作为一个尾随闭包被书写在 `sorted(by:)` 方法的括号外面：

```swift
reversedNames = names.sorted() {$0 > $1}
```
如果闭包表达式作为函数的唯一实际参数传入，而你又使用了尾随闭包的语法，那你就不需要在函数名后边写圆括号了：

```swift
reversedNames = names.sorted {$0 > $1}
```
当闭包很长以至于不能被写成一行时尾随闭包就显得非常有用了。举个栗子，Swift 的 `Array` 类型中有一个以闭包表达式为唯一的实际参数的 `map(_:)` 方法。数组中每一个元素调用一次该闭包，并且返回该元素所映射的值（有可能是其他类型）。具体的映射方式和返回值的类型有闭包来指定。

在给数组中每一个成员提供闭包时， `map(_:)`方法返回一个新的数组，*数组中包涵与原数组一一对应的映射值*。

使用一个带有尾随闭包的 `map(_:)` 方法将一个包含 `Int` 值的数组转换成一个包含 `String` 值的数组。这个数组 ` [16,58,510] `被转换成一个新的数组 `["OneSix","FiveEight","FiveOneZero"]` ：

```swift
let digitNames = [0: "zero", 1: "One", 2: "Two",  3: "Three", 4: "Four",
                  5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"]
let numbers = [16, 58, 510]
let strings = numbers.map { (number) -> String in
    var number = number
    var output = ""
    repeat {
        output = digitNames[number % 10]! + output
        number /= 10
    } while number > 0
    return output
}
print(strings)
// prints ["OneSix", "FiveEight", "FiveOnezero"]
```
`map(_:)` 方法在数组中为每一个元素调用了一次闭包表达式。你不需要指定闭包的输入形式参数 number 的类型，因为它能从数组中将被映射的值来做推断。

### 捕获值
一个闭包能够从上下文*捕获*已被定义的*常量和变量*。即使定义这些常量和变量的**原作用域已经不存在**，闭包仍能够在其函数体内*引用和修*改这些值。

在 Swift 中，一个能够捕获值的闭包最简单的模型是*内嵌函数*，即被书写在另一个函数的内部。一个内嵌函数能够捕获外部函数的实际参数并且能够捕获任何在外部函数的内部定义了的常量与变量。

这里有个命名为 `makeIncrement` 的函数栗子，其中包含了一个名叫 `incrementer` 一个内嵌函数。这个内嵌 `incrementer()`  函数能在它的上下文捕获两个值， `runningTotal`  和 `amount` 。在捕获这些值后，通过 `makeIncrement` 将 `incrementer`作为一个闭包返回，每一次调用 `incrementer` 时，将以 `amount`作为增量来增加 `runningTotal` ：

```swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementer() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementer
}
let incrementByTwo = makeIncrementer(forIncrement: 2)
print(incrementByTwo())
print(incrementByTwo())
print(incrementByTwo())
//prints 2
//       4
//       6
```
`incrementer()` 函数是没有任何形式参数， `runningTotal` 和 `amount` 不是来自于函数体的内部，而是通过捕获主函数的 `runningTotal` 和 `amount` **把它们内嵌在自身函数内部供使用**。当调用 `makeIncrementer`  结束时通过引用*捕获来确保不会消失*，并确保了在下次再次调用 `incrementer` 时， `runningTotal` 将继续增加。

如果你建立了第二个 `incrementer` ,它将会有一个新的、独立的 `runningTotal` 变量的引用：

```swift
let incrementBySeven = makeIncrementer(forIncrement: 7)
print(incrementBySeven())
//prints 7
```
再次调用原来增量器 ( `incrementByTwo` )  继续增加它自己的变量 `runningTotal` 的值，并且不会影响 `incrementBySeven` 捕获的变量 `runningTotal` 值（`8`）。
### 闭包是引用类型
在上面例子中， `incrementBySeven` 和 `incrementByTwo` 是常量，但是这些常量指向的闭包仍可以增加已捕获的变量 `runningTotal` 的值。这是因为*函数*和*闭包*都是**引用类型**。

无论你什么时候安赋值一个函数或者闭包给常量或者变量，你实际上都是将常量和变量设置为对函数和闭包的引用。这上面这个例子中，闭包选择 incrementByTwo 指向一个常量，而不是闭包它自身的内容。
这也意味着你赋值一个闭包到两个不同的常量或变量中，这两个常量或变量都将指向相同的闭包：

```swift
let alsoIncrementByTwo = incrementByTwo
print(alsoIncrementByTwo())
//prints 10
```
### 逃逸闭包
当闭包作为一个实际参数传递给一个函数的时候，我们就说这个闭包*逃逸*了，**因为它可以在函数返回之后被调用**。当你声明一个接受闭包作为形式参数的函数时，你可以在形式参数前写 `@escaping` 来明确闭包是允许逃逸的。

闭包可以逃逸的一种方法是**被储存在定义于函数外的变量里**。比如说，很多函数接收闭包实际参数来作为启动异步任务的回调。函数在启动任务后返回，但是闭包要直到任务完成——*闭包需要逃逸*，以便于稍后调用。举例来说：

```swift
var completionHandlers: [() -> Void] = []
func someFunctionWithEscapingClosure(completionHanlder: @escaping () -> Void) {
    completionHandlers.append(completionHanlder)
}
```
函数 `someFunctionWithEscapingClosure(_:)` 接收一个闭包作为实际参数并且添加它到声明在函数*外部的数组里*。如果你不标记函数的形式参数为 `@escaping` ，你就会遇到编译时错误。

让闭包 `@escaping` 意味着你必须在闭包中显式地引用 `self` ，比如说，下面的代码中，传给 `someFunctionWithEscapingClosure(_:) `的闭包是一个逃逸闭包，也就是说它需要显式地引用 `self` 。相反，传给 `someFunctionWithNonescapingClosure(_:)` 的闭包是非逃逸闭包，也就是说它可以隐式地引用 `self` 。

```swift
func someFunctionWithEscapingClosure(completionHanlder: @escaping () -> Void) {
    completionHandlers.append(completionHanlder)
}
func someFunctionWithNonescapingClosure(closure: () -> Void) {
    closure()
}
class SomeClass {
    var x = 11
    func doSomething() {
        someFunctionWithEscapingClosure { self.x = 100 }
        someFunctionWithNonescapingClosure { x = 200 }
    }
}
let instance = SomeClass()
instance.doSomething()
print(instance.x)
// Prints "200"
completionHandlers.first?()
print(instance.x)
// Prints "100"
```
### 自动闭包
*自动闭包*是一种**自动创建的用来把作为实际参数传递给函数的表达式打包的闭包**。**它不接受任何实际参数，并且当它被调用时，它会返回内部打包的表达式的值**。这个语法的好处在于*通过写普通表达式代替显式闭包而使你省略包围函数形式参数的括号。*

*调用*一个带有自动闭包的函数是很常见的，但实现这类函数就不那么常见了。比如说， `assert(condition:message:file:line:)` 函数为它的 `condition`  和 `message` 形式参数接收一个**自动闭包**；它的 `condition` 形式参数只有在调试构建是才评判，而且 `message` 形式参数只有在 `condition` 是 `false` 时才评判。

自动闭包允许你**延迟处理**，因此闭包内部的代码直到你调用它的时候才会运行。对于有副作用或者占用资源的代码来说很有用，因为它可以允许你控制代码何时才进行求值。下面的代码展示了*闭包如何延迟求值*。

```swift
var customersInLine = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
print(customersInLine.count)
// Prints "5"
 
let customerProvider = { customersInLine.remove(at: 0) }
print(customersInLine.count)
// Prints "5"
 
print("Now serving \(customerProvider())!")
// Prints "Now serving Chris!"
print(customersInLine.count)
// Prints "4"
```
尽管 `customersInLine` 数组的第一个元素以闭包的一部分被移除了，但任务并没有执行直到闭包被实际调用。如果闭包永远不被调用，那么闭包里边的表达式就永远不会求值。注意 `customerProvider` 的类型不是 `String` 而是  `() -> String` ——一个不接受实际参数并且返回一个字符串的函数。

当你传一个闭包作为实际参数到函数的时候，你会得到与延迟处理相同的行为。

```swift
// customersInLine is ["Alex", "Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: { customersInLine.remove(at: 0) } )
// Prints "Now serving Alex!"
```

上边的函数 `serve(customer:)` 接收一个**明确的**返回下一个**客户名称的闭包**。下边的另一个版本的 `serve(customer:)` 执行相同的任务但是*不使用明确的闭包*而是通过 `@autoclosure` 标志标记它的形式参数**使用了自动闭包**。现在你可以调用函数就像它接收了一个 `String` 实`际参数`而不是`闭包`。实际参数自动地转换为闭包，因为 `customerProvider` 形式参数的类型被标记为 `@autoclosure` 标记。

```swift
// customersInLine is ["Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: @autoclosure () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: customersInLine.remove(at: 0))
// Prints "Now serving Ewa!"
```
如果你想要自动闭包允许逃逸，就同时使用 `@autoclosure` 和 `@escaping` 标志。

```swift
// customersInLine is ["Barry", "Daniella"]
var customerProviders: [() -> String] = []
func collectCustomerProviders(_ customerProvider: @autoclosure @escaping () -> String) {
    customerProviders.append(customerProvider)
}
collectCustomerProviders(customersInLine.remove(at: 0)) //体现自动闭包的特性
collectCustomerProviders(customersInLine.remove(at: 0))
 
print("Collected \(customerProviders.count) closures.")
// Prints "Collected 2 closures."
for customerProvider in customerProviders {
    print("Now serving \(customerProvider())!")
}
// Prints "Now serving Barry!"
// Prints "Now serving Daniella!"
// customersInLine is []空了
```
上边的代码中，不是调用传入后作为 `customerProvider` 实际参数的闭包， `collectCustomerProviders(_:)` 函数把闭包追加到了 `customerProviders` 数组的末尾。数组声明在函数的生效范围之外，**也就是说数组里的闭包有可能在函数返回之后执行**(*体现逃逸闭包特性*)。结果， `customerProvider` 实际参数的值必须能够逃逸出函数的生效范围。

> 摘自：[swift 官网](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/index.html#//apple_ref/doc/uid/TP40014097-CH3-ID0)
> 所有代码在Xcode9 Swift4 环境编译没问题，代码[戳这里 https://github.com/keshiim/LearnSwift4](https://github.com/keshiim/LearnSwift4)

