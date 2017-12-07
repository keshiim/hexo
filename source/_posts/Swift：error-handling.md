---
title: Swift：错误处理
date: 2017-12-07 15:46:16
categories:
    - 学习
    - Swift
tags:
    - Swift
---

**错误处理**是相应和接收来自你程序中错误条件的过程。Swift 给运行时可恢复错误的抛出、捕获、传递和操纵提供了一类支持。
有些函数和方法不能保证总能完全执行或者产生有用的输出。可选项用来表示不存在值，但是当函数错误，能够了解到什么导致了错误将会变得很有用处，这样你的代码就能根据错误来响应了。

举例来说，假设一个阅读和处理来自硬盘上文件数据的任务。这种情况下有很多种导致任务失败的方法，目录中文件不存在，文件没有读权限，或者文件没有以兼容格式编码。从这些错误中区分不同的状况将能够让程序解决和从这些错误中恢复，并且把不能解决的错误通知给用户。

### 表示和抛出错误
在 Swift 中，错误表示为遵循 `Error`协议类型的值。这个空的协议明确了一个类型可以用于错误处理。

Swift 枚举是典型的为一组相关错误条件建模的完美配适类型，关联值还允许错误错误通讯携带额外的信息。比如说，这是你可能会想到的游戏里自动售货机会遇到的错误条件：

```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
```
抛出一个错误允许你明确某些意外的事情发生了并且正常的执行流不能继续下去。你可以使用 `throw` 语句来抛出一个错误。比如说，下面的代码通过抛出一个错误来明确自动售货机需要五个额外的金币：

```swift
throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
```
### 处理错误
当一个错误被抛出，周围的某些代码必须为处理错误响应——比如说，为了纠正错误，尝试替代方案，或者把错误通知用户。

在 Swift 中有四种方式来处理错误。你可以将来自函数的错误传递给调用函数的代码中，使用 `do-catch` 语句来处理错误，把错误作为可选项的值，或者错误不会发生的断言。每一种方法都在下边的章节中有详细叙述。

当函数抛出一个错误，它就改变了你程序的流，所以能够快速定位错误就显得格外重要。要定位你代码中的这些位置，使用 `try` 关键字——或者 `try?` 或 `try!` 变体——放在调用函数、方法或者会抛出错误的初始化器代码之前。这些关键字在下面的章节中有详细的描述。

#### 使用抛出函数传递错误
为了明确一个函数或者方法可以抛出错误，你要在它的声明当中的形式参数后边写上 `throws`关键字。使用 `throws`标记的函数叫做**抛出函数**。如果它明确了一个返回类型，那么 `throws`关键字要在返回箭头 ( `->`)之前。

```swift
func canThrowErrors() throws -> String
 
func cannotThrowErrors() -> String
```
抛出函数可以把它内部抛出的错误传递到它被调用的生效范围之内。

> 注意：只有抛出函数可以传递错误。任何在非抛出函数中抛出的错误都必须在该函数内部处理。

*在抛出函数体内的任何地方，你都可以用 `throw`语句来抛出错误。*

在下边的栗子中， `VendingMachine`类拥有一个如果请求的物品不存在、卖光了或者比押金贵了就会抛出对应的 `VendingMachineError`错误的 `vend(itemNamed:)`方法：

```swift
struct Item {
    var price: Int
    var count: Int
}
class VendingMachine {
    var inventory = ["Candy Bar": Item(price: 12, count: 7),
                     "Chips": Item(price: 10, count: 4),
                     "Pretzels": Item(price: 7, count: 11)
    ]
    var coinsDeposited = 0
    func vend(itemNamed name: String) throws {
        guard let item = inventory[name] else {
            throw VendingMachineError.invalidSelection
        }
        guard item.count > 0 else {
            throw VendingMachineError.outOfStock
        }
        guard item.price <= coinsDeposited else {
            throw VendingMachineError.insufficientFunds(coinsNeeded: item.price - coinsDeposited)
        }
        coinsDeposited -= item.price
        var newItem = item
        newItem.count -= 1
        inventory[name] = newItem
        print("Dispensing \(name)")
    }
}
```
`vend(itemNamed:)`方法的实现使用了 `guard`语句来提前退出并抛出错误，如果购买零食的条件不符合的话。因为 `throw`语句立即传送程序控制，所以只有所有条件都达到，物品才会售出。

由于 `vend(itemNamed:)`方法传递它抛出的任何错误，所以你调用它的代码要么直接处理错误——使用 `do-catch`语句， `try?`或者 `try!`——*要么继续传递它们*。比如说，下边栗子中的 `buyFavoriteSnack(person:vendingMachine:)`同样是一个抛出函数，任何 `vend(itemNamed:)`方法抛出的函数都会向上传递给调用 `buyFavoriteSnack(person:vendingMachine:)`函数的地方。

```swift
let favoriteSnacks = [
    "Alice": "Chips",
    "Bob": "Licorice",
    "Eve": "Pretzels",
]
func buyFavoriteSnack(person: String, vendingMachine: VendingMachine) throws {
    let snackName = favoriteSnacks[person] ?? "Candy Bar"
    try vendingMachine.vend(itemNamed: snackName)
}
// Dispensing Chips
```
在这个栗子中， `buyFavoriteSnack(person:vendingMachine:)`函数查找给定人的最爱零食并且尝试通过调用 `vend(itemNamed:)`方法来购买它们。**由于 `vend(itemNamed:) `方法会抛出错误，调用的时候要在前边用 `try`关键字**。

#### 使用 Do-Catch 处理错误
使用 `do-catch`语句来通过运行一段代码处理错误。如果`do`分句中抛出了一个错误，它就会与 `catch`分句匹配，以确定其中之一可以处理错误。

这是 `do-catch`语句的通常使用姿势：

```swift
do {
    try expression
    statements
} catch pattern 1 {
    statements
} catch pattern 2 where condition {
    statements
}
```
在 `catch`后写一个*模式*来明确分句可以处理哪个错误。**如果一个 `catch`分句没有模式，这个分句就可以匹配所有错误并且绑定这个错误到本地常量 `error`上**。

`catch`分句没有处理 `do`分句可能抛出的所有错误。如果没有 `catch`分句能处理这个错误，那错误就会传递到*周围的生效范围当中*。总之，错误总得在周围某个范围内被处理。举例来说，接下来的代码处理了 `VendingMachineError`枚举里的所有三个错误，但其他所有错误得通过范围内其他代码处理：

```swift
var vendingMachine = VendingMachine()
vendingMachine.coinsDeposited = 8
do {
    try buyFavoriteSnack("Alice", vendingMachine: vendingMachine)
    // Enjoy delicious snack
} catch VendingMachineError.invalidSelection {
    print("Invalid Selection.")
} catch VendingMachineError.outOfStock {
    print("Out of Stock.")
} catch VendingMachineError.insufficientFunds(let coinsNeeded) {
    print("Insufficient funds. Please insert an additional \(coinsNeeded) coins.")
}
// prints "Insufficient funds. Please insert an additional 2 coins."
```
在上面的栗子当中，函数 `buyFavoriteSnack(person:vendingMachine:)`在 `try`表达式中被调用，因为它会抛出错误。如果抛出错误，执行会立即切换到 `catch`分句，它决定是否传递来继续。如果没有错误抛出， `do`语句中剩下的语句将会被执行。

#### 转换错误为可选项
使用 `try?`通过**将错误转换为可选项来处理一个错误**。如果一个错误在 `try?`表达式中抛出，则表达式的值为 `nil`。比如说下面的代码`x`和`y`拥有同样的值和行为：

```swift
func someThrowingFunction() throws -> Int {
    // ...
}
 
let x = try? someThrowingFunction()
 
let y: Int?
do {
    y = try someThrowingFunction()
} catch {
    y = nil
}
```
如果 `someThrowingFunction()`抛出一个错误， `x`和 `y`的值就是 `nil`。另一方面，`x`和`y`的值是函数返回的值。注意 `x`和 `y`是可选的无论 `someThrowingFunction()`返回什么类型，这里函数返回了一个整数，所以`x`和`y`是可选整数。

**当你想要在同一句里处理所有错误时**，使用 `try?`能让你的错误处理代码更加简洁。比如，下边的代码使用了一些方法来获取数据，或者在所有方式都失败后返回 `nil`。

```swift
func fetchData() -> Data? {
    if let data = try? fetchDataFromDisk() { return data }
    if let data = try? fetchDataFromServer() { return data }
    return nil
}
```
#### 取消错误传递
事实上有时你已经知道一个抛出错误或者方法不会在运行时抛出错误。在这种情况下，你可以在表达式前写 `try!`来**取消错误传递并且把调用放进不会有错误抛出的运行时断言当中**。如果错误真的抛出了，你会得到一个运行时错误。

比如说，下面的代码使用了 `loadImage(_:)`函数，它在给定路径下加载图像资源，如果图像不能被加载则抛出一个错误。在这种情况下，由于图像跟着应用走，运行时不会有错误抛出，所以取消错误传递是合适的。

```swift
let photo = try! loadImage("./Resources/John Appleseed.jpg")
```
### 指定清理操作
**使用 `defer`语句来在代码离开当前代码块前执行语句合集**。这个语句允许你在以任何方式离开当前代码块前执行必须要的清理工作——无论是因为抛出了错误还是因为 `return`或者 `break`这样的语句。比如，你可以使用 `defer`语句来保证文件描述符都关闭并且手动指定的内存到被释放。

`defer`语句延迟执行直到当前范围退出。这个语句由 `defer`关键字和需要稍后执行的语句组成。被延迟执行的语句可能不会包含任何会切换控制出语句的代码，比如 `break`或 `return`语句，或者通过抛出一个错误。**延迟的操作与其指定的顺序相反执行**——就是说，第一个 `defer`语句中的代码会在第二个中代码执行完毕后执行，以此类推。

```swift
func processFile(filename: String) throws {
    if exists(filename) {
        let file = open(filename)
        defer {
            close(file)
        }
        while let line = try? file.readline() {
            // Work with the file.
        }
        // close(file) is called here, at the end of the scope.
    }
}
```


> 摘自：[Swift 官网](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/index.html#//apple_ref/doc/uid/TP40014097-CH3-ID0)
> 所有代码在Xcode9 Swift4 环境编译没问题，代码[戳这里 https://github.com/keshiim/LearnSwift4](https://github.com/keshiim/LearnSwift4)


