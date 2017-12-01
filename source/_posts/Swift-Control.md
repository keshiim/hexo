---
title: Swift：控制流
date: 2017-11-09 17:01:14
categories:
    - 学习
    - Swift
tags:
    - Swift
---
`Swift` 提供所有多样化的控制流语句。包括 `while` 循环来多次执行任务； `if ， guard 和 switch `语句来基于特定的条件执行不同的代码分支；还有比如 `break` 和 `continue` 语句来传递执行流到你代码的另一个点上。

`Swift` 同样添加了 `for-in `循环，它让你更简便地遍历*数组、字典、范围和其他序列*。

`Swift` 的 `switch` 语句同样比 `C` 中的对应语句多了不少新功能。比如说 `Swift` 中的 `switch` 语句不再“**贯穿**”到下一个情况当中，这就避免了 `C` 中常见的 `break` 语句丢失问题。情况可以匹配**多种模式**，包括**间隔匹配**，**元组**和**特定的类型**。 `switch` 中匹配的值还能绑定到*临时的常量和变量*上供情况中代码使用，并且可以为每一个情况写 `where` 分句表达式来应用复杂条件匹配。
### For-in 循环
使用 `for-in` 循环来遍历序列，比如一个范围的*数字*，*数组中的元素*或者*字符串中的字符*。

```swift
//这个例子使用 for-in 循环来遍历数组中的元素：
let names = ["Anna", "Alex", "Brian", "Jack"]
for name in names {
    print("Hello, \(name)!")
}
// Hello, Anna!
// Hello, Alex!
// Hello, Brian!
// Hello, Jack!
```
The same as Dictionary，访问它的键值对。当字典遍历时，每一个元素都返回一个 `(key, value)` 元组，你可以在` for-in` 循环体中使用显式命名常量来分解 `(key, value)` 元组成员，当然不想要的值使用` _ `来舍弃值或键。

```swift
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
}
// ants have 6 legs
// cats have 4 legs
// spiders have 8 legs
```
`for-in` 循环同样能遍历数字区间。这个栗子打印了乘五表格的前几行：

```swift
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```
在上面的栗子当中， `index` 是一个常量，它的值在每次遍历循环开始的时候被自动地设置。因此，它不需要在使用之前声明。它隐式地在循环的声明中声明了，不需要再用 `let` 声明关键字。
如果你不需要序列的每一个值，你可以使用 *下划线* ( `_` )来取代遍历名以**忽略值**。

使用半开区间运算符`（ ..< ）`来包含最小值但不包含最大值

```swift
let minutes = 60
for tickMark in 0..<minutes {
    // render the tick mark each minute (60 times)
}
```
有些用户可能想要在他们的UI上少来点 分钟标记。比如说每 5 分钟一个标记吧。使用 `stride(from:to:by:)` 函数来跳过不想要的标记。

```swift
let minuteInterval = 5
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
    // render the tick mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 55)
}
```
闭区间也同样适用，使用 `stride(from:through:by:)` 即可：

```swift
let hours = 12
let hourInterval = 3
for tickMark in stride(from: 3, through: hours, by: hourInterval) {
    print(tickMark, terminator: " > ")
}
//3 > 6 > 9 > 12 >
```
### While 循环
`while` 循环执行一个合集的语句指导条件变成 `false` 。这种循环最好在第一次循环之后还有未知数量的遍历时使用。`Swift` 提供了两种 `while` 循环：

* `while` _在每次循环开始的时候计算它自己的条件；_
* `repeat-while` _在每次循环结束的时候计算它自己的条件。_

#### While
`while` 循环通过判断单一的条件开始。如果条件为 `true` ，语句的合集就会重复执行直到条件变为 `false` 。

```swift
//while 循环的通用格式：
while condition {
    statements
}
```
玩一个玩蛇与梯子（也叫滑梯与梯子）的简单栗子：
<: style='margin: .auto' src='snakesAndLadders_2x.png' alt='snakesAndLadders_2x' title='滑梯与梯子'>

下边是游戏的规则：
* 棋盘拥有 25 个方格，目标就是到达或者超过第 25 号方格；
* 每一次，你扔一个六面色子，安装方格的数字移动，依据水平的线路，如图安装上边虚线箭头标注的路线；
* 如果你停留在了梯子的下边，你就可以顺着梯子爬上去；
* 如果你停留在了蛇的头上，你就要顺着蛇滑下来。

游戏棋盘用 `Int` 值的数组来表示。它的大小基于一个叫做 `finalSquare` 的常量，它被用来初始化数组同样用来检测稍后的胜利条件。棋盘使用 26 个零 `Int` 值初始化，而不是 25 个（从 0  到 25 ）：

```swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
```
随后设置为拥有更多特定的值比如蛇和梯子。有梯子的方格有一个正数来让你移动到棋盘的上方，因此有蛇的方格有一个负数来让你从棋盘上倒退：

```swift
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
```
玩家从“零格”开始，它正好是棋盘的左下角。第一次扔色子总会让玩家上到棋盘上去：

```swift
var square = 0
var diceRoll = 0
while square < finalSquare {
    //roll the dice
    diceRoll += 1
    if diceRoll == 7 {
        diceRoll = 1
    }
    //move by the rolled amount
    square += diceRoll
    if square < board.count {
        // if we`re still on the board, move up or down for a snake or a ladder
        square += board[square];
    }
    print("now: \(square)", terminator: "> ")
}
print("Game over!")
//print now: 1> now: 11> now: 4> now: 8> now: 13> now: 8> now: 18> now: 20> now: 23> now: 27> Game over!
```
当前的 `while` 循环执行结束，并且循环条件已经检查来看循环是否应该再次执行。如果玩家已经移到或者超出了第 25 号方格，循环评定为 `false` ，游戏就结束了。

`while` 循环在这个情况当中合适是因为开始 `while` 循环之后游戏的长度并不确定。循环会一直执行下去直到特定的条件不满足。
#### Repeat-While
`while` 循环的另一种形式，就是所谓的 `repeat-while` 循环，在判断循环条件之前会执行一次循环代码块。然后会继续重复循环直到条件为 `false` 。

`repeat-while` 循环的通用形式：

```swift
repeat {
    statements
} while condiation
```
再次回顾蛇与梯子的栗子，使用 `repeat-while` 循环而不是 `while` 循环。 `finalSquare , board , square , 和 diceRoll `的值初始化的方式与 `while` 循环完全相同：

```swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
var square = 0
var diceRoll = 0
```
在这个版本的游戏中，第一次循环中的动作是用来检查梯子或者蛇的。没有梯子能直接把玩家带到25格，因此不可能通过梯子赢得游戏。也就是说，在循环一开始就检查蛇还是梯子是安全的。
游戏一开始，玩家在“零格”。 `board[0]` 总是等于 `0` 的，并且没有效果：

```swift
square = 0
diceRoll = 0
repeat {
    // move up or down for a snake or ladder
    square += board[square]
    // roll the dice
    diceRoll += 1
    if diceRoll = 7 {
        diceRoll = 1
    }
    // move by the rolled amount
    square += diceRoll
    print("now: \(square)", terminator: "> ")
} while square < finalSquare
//now: 1> now: 3> now: 14> now: 8> now: 13> now: 19> now: 9> now: 20> now: 23> now: 27> Game over!
```
在检查是蛇还是梯子的代码之后，就是要色子了，玩家按照 `diceRoll` 数量的格数前进。当前循环执行结束。
循环条件 `( while square < finalSquare)`  与之前的相同，但是这次它会在第一次循环结束之后才会被判定。 `repeat-while` 循环的结构要比前边栗子里的 `while` 循环更适合这个游戏。在上边的 `repeat-while` 循环中， `square += board[square]` 总是会在循环的 `while` 条件确定 `square` 仍在棋盘上之后立即执行。这个行为就去掉了早期游戏版本中对数组边界检查的需要。
### 条件语句
很多时候根据特定的条件来执行不同的代码是很有用的。你可能想要在错误发生时运行额外的代码，或者当值变得太高或者太低的时候显示一条信息。要达成这个目的，你可以让你的那部分代码**有条件地**执行。

`Swift` 提供了两种方法来给你的代码添加条件分支，就是所谓的 `if` 语句和 `switch` 语句。总的来说，你可以使用 `if` *语句来判定简单的条件*，比如少量的可能性。 `switch` *语句则适合更复杂的条件*，比如多个可能的组合，并且在模式匹配的情况下更加有用，可以帮你选择一段合适的代码分支来执行。

#### If
最简单的形式中， `if` 语句有着一个单一的 `if` 条件。它只会在条件为 `true` 的情况下才会执行语句的集合：

```swift
var temperatureInFahrenheit = 30
if temperatureInFahrenheit <= 32 {
    print("It`s very cold. Consider wearing a scarf.")
} else {
    print("It`s not that cold. Wear a t-shirt.")
}
//prints "It's not that cold. Wear a t-shirt."
```
先前的栗子检测了温度是否小于等于 32 华氏温度（水的冰点）。如果是，就打印一个信息。否则，没有信息打印，并且执行 `if` 语句的大括号后边的代码，如果温度大于32华氏温度，会执行*`else` 分句*。

#### Switch
`switch` 语句会将一个值与多个可能的**模式匹配**。然后基于第一个成功匹配的模式来执行合适的代码块。 `switch` 语句代替 `if` 语句提供了对多个潜在状态的响应。

在其自身最简单的格式中， `switch` 语句把一个值与一个或多个相同类型的值比较：

```swift
switch some value to consider {
case value 1:
    respond to value 1
case value 2,
value 3:
    respond to value 2 or 3
default:
    otherwise, do something else
}
```
##### 没有隐式贯穿
相比 `C` 和 `Objective-C `里的 `switch` 语句来说，`Swift` 里的 `switch` *语句不会默认从每个情况的末尾贯穿到下一个情况里*。相反，整个 `switch` 语句会在匹配到第一个 `switch` 情况执行完毕之后退出，不再需要显式的 `break` 语句。这使得 `switch` 语句比 `C` 的更安全和易用，并且避免了意外地执行多个 `switch` 情况。
> 尽管 `break` 在 Swift 里不是必须的，你仍然可以使用 `break` 语句来匹配和忽略特定的情况，或者在某个情况执行完成之前就打断它。

每一个情况的函数体**必须**包含至少*一个可执行的语句*。下面的代码就是不正确的，因为第一个情况是空的：

```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a":
case "A":
    print("The letter A")
default:
    print("Not the letter A")
}
// this will report a compile-time error
```
与 C 中的 `switch` 语句不同，这个 `switch` 语句没有同时匹配 ”a” 和 ”A” 。相反它会导致一个编译时错误 `case “a”:`**没有包含任何可执行语句** 。这可以避免意外地从一个情况贯穿到另一个情况中，并且让代码更加安全和易读。

在一个 `switch` 情况中匹配多个值可以用逗号分隔，并且可以写成多行，如果列表太长的话：

```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a", "A":
    print("The letter A")
default:
    print("Not the letter A")
}
// Prints "The letter A"
```
##### 区间匹配
`switch`情况的值可以在一个区间中匹配。这个栗子使用了数字区间来为语言中的数字区间进行转换：

```swift
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
```
在上面的栗子中， `approximateCount`  在 `switch` 语句中进行评定。每个 `case` 都与数字或者区间进行对比。由于 `approximateCount` 的值在12和100之间， `naturalCount` 被赋值 `“dozens of” `，并且执行结果传递出了 `switch` 语句。
##### 元组
你可以使用元组来在一个 `switch` 语句中测试多个值。每个元组中的元素都可以与**不同**的*值*或者*区间*进行匹配。另外，使用下划线`（_）`来表明匹配**所有可能的值**。

```swift
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
    print("(\(somePoint.0), \(somePoint.1)) is outside the box")
}
// prints "(1, 1) is inside the box"
```
`switch` 语句决定坐标是否在原点 `(0,0)` ；在红色的 x 坐标轴；在橘黄色的 y坐标轴；在蓝色的4乘4以原点为中心的方格里；或者在方格外边。

<img style='margin: .auto' src='coordinateGraphSimple_2x.png' alt='coordinateGraphSimple_2x' title='表示区间(1)'>
##### 值绑定
`switch` 情况可以将匹配到的**值***临时绑定*为一个*常量或者变量*，来给情况的函数体使用。这就是所谓的*值绑定*，因为值是在情况的函数体里“绑定”到临时的常量或者变量的。

下边的栗子接收一个* (x,y)* 坐标，使用 *(Int,Int)* 元组类型并且在下边的图片里显示：

```swift
let anotherPoint = (2, 0)
switch anotherPoint {
case (let x, 0):
    print("on the x-axis with an x value of \(x)")
case (0, let y):
    print("on the y-axis with a y value of \(y)")
case let (x, y):
    print("somewhere else at (\(x), \(y))")
}
// print:on the x-axis with an x value of 2
```
<img style='margin: .auto' src='coordinateGraphMedium_2x.png' alt='coordinateGraphMedium_2x' title='表示区间（2）'>

`switch` 语句决定坐标是否在在红色的x坐标轴，在橘黄色的y坐标轴；还是其他地方；或不在坐标轴上。

三个 `switch` 情况都使用了常量占位符 x 和 y ，它会从临时 `anotherPoint` 获取一个或者两个元组值。第一个情况， `case(let x, 0) `，匹配任何 y 的值是 0 并且赋值坐标的x到临时常量 x 里。类似地，第二个情况， `case(0,let y)` ，匹配让后 x 值是 0 并且把 y 的值赋值给临时常量 y 。

**在临时常量被声明后，它们就可以在情况的代码块里使用**。这里，它们用来输出点的分类。

注意这个 switch 语句没有任何的 `default` 情况。最后的情况， `case let (x,y)` ，声明了一个带有*两个占位符常量的元组*，它可以匹配所有的值。结果，它匹配了所有剩下的值，然后就不需要 `default` 情况来让 `switch` 语句穷尽了。
##### Where
`switch` 情况可以使用 `where` 分句来检查额外的情况。
下边的栗子划分 `(x,y)` 坐标到下边的图例中：

```swift
let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
case let (x, y) where x == y:
    print("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
    print("(\(x), \(y)) is on the line x == -y")
case let (x, y):
    print("(\(x), \(y)) is just some arbitrary point")
}
// prints "(1, -1) is on the line x == -y"
```
<img style='margin: .auto' src='coordinateGraphComplex_2x.png' alt='coordinateGraphComplex_2x' title='表示区间（3）'>

三个 `switch` 情况声明了占位符常量 x 和 y ，它从 `yetAnotherPoint` 临时接收两个元组值。这个常量使用 `where` 分句，来创建动态过滤。 `switch` 情况只有 `where` 分句情况评定等于 `true` 时才会匹配这个值。

和前边的栗子一样，最后的情况匹配了余下所有可能的值，所以不需要 `default` 情况这个 `switch` 也是全面的。
###### 复合情况
多个 `switch` 共享同一个函数体的多个情况可以在 `case` 后写多个模式来复合，在每个模式之间用逗号分隔。如果任何一个模式匹配了，那么这个情况都会被认为是匹配的。如果模式太长，可以把它们写成多行，比如说：

```swift
let someCharacter: Character = "e"
switch someCharacter {
case "a", "e", "i", "o", "u":
    print("\(someCharacter) is a vowel")
case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
     "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
    print("\(someCharacter) is a consonant")
default:
    print("\(someCharacter) is not a vowel or a consonant")
}
// Prints "e is a vowel"
```
*复合情况*同样可以包含**值绑定**。所有复合情况的模式都*必须包含相同的值绑定集合*，并且复合情况中的每一个绑定都得有*相同的类型格式*。这才能确保无论复合情况的那部分匹配了，接下来的函数体中的代码都能访问到绑定的值并且值的类型也都相同。

```swift
let stillAnotherPoint = (9, 0)
switch stillAnotherPoint {
case (let distance, 0), (0, let distance):
    print("On an axis, \(distance) from the origin")
default:
    print("Not on an axis")
}
// Prints "On an axis, 9 from the origin"
```
上边的 `case` 拥有两个模式：  `(let distance, 0) ` 匹配 x 轴的点以及 `(0, let distance)` 匹配 y 轴的点。两个模式都包含一个 `distance` 的绑定并且 `distance` 在两个模式中都是整形——也就是说这个 `case` 函数体的代码一定可以访问 `distance` 的值。
### 控制转移语句
控制转移语句在你代码执行期间改变代码的执行顺序，通过从一段代码转移控制到另一段。Swift 拥有五种控制转移语句：

* continue
* break
* fallthrough
* return
* throw

`continue` , `break` , 和 `fallthrough`  语句在下边有详细描述。 `return` 语句在函数中描述，还有 `throw` 语句在使用抛出函数传递错误中描述。
#### Continue
`continue` 语句告诉循环停止正在做的事情并且再次从头开始循环的下一次遍历。它是说“我不再继续当前的循环遍历了”而不是离开整个的循环。
举个栗子🌰：

```swift
let puzzleInput = "great minds think alike"
var puzzleOutput = ""
for character in puzzleInput.characters {
    switch character {
    case "a", "e", "i", "o", "u", " ":
        continue
    default:
        puzzleOutput.append(character)
    }
}
print(puzzleOutput)
// prints "grtmndsthnklk"
```
上面的代码在匹配到元音或者空格的时候调用了 `continue` 关键字，导致遍历的当前循环立即结束并直接跳到了下一次遍历的开始。这个行为使得 `switch` 代码块匹配（和忽略）只有元音和空格的字符，而不是请求匹配每一个要打印的字符。

#### Break
`break` 语句会立即结束整个控制流语句。当你想要提前结束 `switch` 或者循环语句或者其他情况时可以在 `switch` 语句或者循环语句中使用 `break` 语句。
##### 循环语句中的 Break
当在循环语句中使用时， `break` 会立即结束循环的执行，并且转移控制到循环结束花括号（ } ）后的第一行代码上。当前遍历循环里的其他代码都不会被执行，并且余下的遍历循环也不会开始了。

##### Switch 语句里的 Break
当在`switch`语句里使用时， `break` 导致 `switch` 语句立即结束它的执行，并且转移控制到 `switch` 语句结束花括号（ } ）之后的第一行代码上。

这可以用来在一个 `switch` 语句中匹配和忽略一个或者多个情况。因为 Swift 的 `switch` 语句是**穷尽且不允许空情况的**，所以有时候有必要故意匹配和忽略一个匹配到的情况以让你的意图更加明确。要这样做的话你可以通过把 `break` 语句作为情况的整个函数体来忽略某个情况。当这个情况通过 `switch` 语句匹配到了，情况中的 `break` 语句会立即结束 `switch` 语句的执行。


```swift
let numberSymbol: Character = "三"  // Simplified Chinese for the number 3
var possibleIntegerValue: Int?
switch numberSymbol {
case "1", "١", "一", "๑":
    possibleIntegerValue = 1
case "2", "٢", "二", "๒":
    possibleIntegerValue = 2
case "3", "٣", "三", "๓":
    possibleIntegerValue = 3
case "4", "٤", "四", "๔":
    possibleIntegerValue = 4
default:
    break
}
if let integerValue = possibleIntegerValue {
    print("The integer value of \(numberSymbol) is \(integerValue).")
} else {
    print("An integer value could not be found for \(numberSymbol).")
}
// prints "The integer value of 三 is 3."
```
在上边的例子中，列举所有可能的 `Character` 值是不实际的，所以`default`情况就提供了一个匹配所有没有匹配到的字符的功能。这个 `default` 情况不需要执行任何动作，所以因此就写了一个 `break` 语句作为函数体。一旦 `default` 情况匹配到了， `break` 语句结束 `switch` 语句的执行，然后代码从 `if let` 语句继续执行。
##### Fallthrough
`swift`贯穿。
如果你确实需要 `C` 风格的贯穿行为，你可以选择在每个情况末尾使用 `fallthrough` 关键字。下面的栗子使用了 `fallthrough` 来创建一个数字的文字描述：

```swift
let integerToDescribe = 5
var description = "The number \(integerToDescribe) is"
switch integerToDescribe {
case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " a prime number, and also"
    fallthrough
default:
    description += " an integer."
}
print(description)
// prints "The number 5 is a prime number, and also an integer."
```
这个栗子声明了一个新的 `String` 变量叫做 `description` 并且赋值给它一个初始值。然后函数使用一个 `switch` 语句来判断 `integerToDescribe` 。如果 `integerToDescribe` 是一个列表中的质数，函数就在 `description` 的末尾追加文字，来标记这个数字是质数。然后它使用 `fallthrough` 关键字来“贯穿到” `default` 情况。 `default` 情况添加额外的文字到描述的末尾，接着 `switch` 语句结束。
##### 给语句打标签
你可以内嵌循环和条件语句到其他循环和条件语句当中以在 Swift 语言中创建一个复杂的控制流结构。总之，循环和条件语句都可以使用 `break` 语句来提前结束它们的执行。因此，显式地标记那个循环或者条件语句是你想用 `break` 语句结束的就很有必要。同样的，**如果你有多个内嵌循环，显式地标记你想让 `continue` 语句生效的是哪个循环就很有必要了。**

要达到这些目的，你可以用**语句标签**来给循环语句或者条件语句做*标记*。在一个条件语句中，你可以使用一个语句标签配合 `break` 语句来结束被标记的语句。在循环语句中，你可以使用语句标签来配合 `break` 或者 `continue` 语句来结束或者继续执行被标记的语句。

*通过把标签作为关键字放到语句开头来用标签标记一段语句*，后跟**冒号**。这里是一个对 `while` 循环使用标签的栗子，这个原则对所有的循环和 `switch` 语句来说都相同：

```swift
(label name): while condition {
    statements
}
```

下边的栗子为你之前章节看过的*蛇与梯子游戏*做了修改，在 `while` 循环中使用了标签来配合 `break` 和 `continue` 语句。这次，这个游戏有一个额外的规则：

* 要赢得游戏，你必须精确地落在第25格上。
如果特定的点数带你超过了第25格，你必须再次掷色子直到你恰好得到了落到第25格的点数。

游戏棋盘与之前的一样：
<img style='margin: .auto' src='snakesAndLadders_2x(1).png' alt='snakesAndLadders_2x(1)' title='蛇和梯子游戏示意图'>
`finalSquare ， board ， square 和 diceRoll` 的值也用和之前一样的方式来初始化：

```swift
let finalSquare = 25
var board = [Int](count: finalSquare + 1, repeatedValue: 0)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
var square = 0
var diceRoll = 0
```
这个版本的游戏使用了一个 `while` 循环和一个 `switch` 语句来实现游戏的逻辑。 `while` 循环有一个叫做 `gameLoop` 的标签，来表明它是蛇与梯子游戏的主题循环。

`while` 循环条件是 `while square != finaleSquare` ，用来反映你必须精确地落在第25格上：

```swift
gameLoop: while square != finalSquare {
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    switch square + diceRoll {
    case finalSquare:
        // diceRoll will move us to the final square, so the game is over
        break gameLoop
    case let newSquare where newSquare > finalSquare:
        // diceRoll will move us beyond the final square, so roll again
        continue gameLoop
    default:
        // this is a valid move, so find out its effect
        square += diceRoll
        square += board[square]
    }
}
print("Game over!")
```
每次循环，都会扔色子。使用一个 `switch` 语句来考虑移动的结果而不是立即移动玩家，然后如果移动允许的话就工作：

* 如果扔的色子将把玩家移动到最后的方格，游戏就结束。 `break` `gameLoop` 语句转移控制到 `while` 循环外的第一行代码上，它会结束游戏。

* 如果扔的色子点数将会把玩家移动超过最终的方格，那么移动就是不合法的，玩家就需要再次扔色子。 `continue gameLoop` 语句就会结束当前的 `while` 循环遍历并且开始下一次循环的遍历。

* 在其他所有的情况中，色子是合法的。玩家根据 `diceRoll` 的方格数前进，并且游戏的逻辑会检查蛇和梯子。然后循环结束，控制返回到 `while` 条件来决定是否要再次循环。

### 提前退出
`guard` 语句，类似于 `if` 语句，基于布尔值表达式来执行语句。使用 `guard` 语句来要求一个条件必须是真才能执行 `guard` 之后的语句。与 `if` 语句不同， `guard` 语句**总是有一个 `else` 分句**—— `else` 分句里的代码会在条件不为真的时候执行。

```swift
func greet(_ person: [String: String]) {
    guard let name = person["name"] else {
        return
    }
    
    print("Hello \(name)!")
    
    guard let location = person["location"] else {
        print("I hope the weather is nice near you.")
        return
    }
    
    print("I hope the weather is nice in \(location).")
}
greet(["name": "John"])
// prints "Hello John!"
// prints "I hope the weather is nice near you."
greet(["name": "Jane", "location": "Cupertino"])
// prints "Hello Jane!"
// prints "I hope the weather is nice in Cupertino."
```
如果 `guard` 语句的条件被满足，代码会继续执行直到 `guard` 语句后的花括号。任何在条件中使用可选项绑定而赋值的变量或者常量在 `guard` 所在的代码块中*随后的代码里都是***可用的**。

如果这个条件没有被满足，那么在 `else` 分支里的代码就会被执行。这个分支必须转移控制结束 `guard` 所在的代码块。要这么做可以使用控制转移语句比如 `return ， break ， continue 或者 throw` ，或者它可以调用一个不带有返回值的函数或者方法，比如 `fatalError()` 。

### 检查API的可用性
Swift 拥有内置的对 API 可用性的检查功能，它能够确保你不会悲剧地使用了对部属目标不可用的 API。

编译器在 SDK 中使用可用性信息来确保在你项目中明确的 API 都是可用的。如果你尝试使用一个不可用的 API 的话，Swift 会在编译时报告一个错误。

你可以在 `if` 或者 `guard` 语句中使用一个可用性条件来有条件地执行代码，基于在运行时你想用的哪个 API 是可用的。当验证在代码块中的 API 可用性时，编译器使用来自可用性条件里的信息来检查。

```swift
if #available(iOS 10, macOS 10.12, *) {
    // Use iOS 10 APIs on iOS, and use macOS 10.12 APIs on macOS
} else {
    // Fall back to earlier iOS and macOS APIs
}
```
上边的可用性条件确定了在 iOS 平台， `if` 函数体只在 *iOS 10* 及以上版本才会执行；对于 macOS 平台，在只有在 `macOS 10.12` 及以上版本才会运行。最后一个实际参数， `*` ，它需求并表明在其他所有平台， `if` 函数体执行你在目标里明确的最小部属。

> 摘自：[swift 官网](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/index.html#//apple_ref/doc/uid/TP40014097-CH3-ID0)
> 所有代码在Xcode9 Swift4 环境编译没问题，代码[戳这里 https://github.com/keshiim/LearnSwift4](https://github.com/keshiim/LearnSwift4)

