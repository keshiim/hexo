
---
title: Swift：字符串和字符
date: 2017-10-27 16:49:36
categories:
    - 学习
    - Swift
tags:
    - Swift
---

*字符串*是一系列的字符，比如说 `"hello, world"`或者 `"albatross"`。`Swift` 的字符串用 `String`类型来表示。 `String`的内容可以通过各种方法来访问到，包括作为 `Character`值的集合。

`Swift` 的 `String`  和 `Character`  类型提供了*一种快速的符合* `Unicode` 的方式操作你的代码。字符串的创建和修改语法非常轻量易读，使用与 `C` 类似的字符串字面量语法。字符串串联只需要使用 `+`运算符即可，字符串的可修改能力通过选择常量和变量来进行管理，就如同 `Swift` 语言中的其他值。你同样可以使用字符串来 _*插入*常量、变量、字面量以及表达式到更长的字符串当中_ ，这就是所谓的*字符串插值*。这样让创建自定义字符串值来显示、储存和打印值变得更加简单。

别看语法简单，`Swift` 的 `String`类型仍旧是快速和现代的字符串实现。每一个字符串都是由 `Unicode` 字符的*独立编码组成*，并且提供了多种 `Unicode` 表示下访问这些字符的支持。

> `Swift` 的 `String`类型桥接到了基础库中的 `NSString`类。`Foundation` 同时也扩展了所有 `NSString` 定义的方法给 `String` 。也就是说，如果你导入 `Foundation` ，就可以在 `String` 中访问所有的 `NSString`  方法，无需转换格式。

### 字符串字面量
你可以在你的代码中插入预先写好的 `String`值作为字符串`字面量`。字符串字面量是被双引号包裹的固定顺序文本字符（ "）。
使用字符串字面量作为常量或者变量的初始值：

```swift
let someString = "Some string literal value."
```
注意 `Swift` 会为 `someString`常量推断类型为 `String`，因为你用了字符串字面量值来初始化。

如果你需要很多行的字符串，使用多行字符串字面量。多行字符串字面量是用三个双引号引起来的一系列字符：

```swift
let quotation = """
The white rabbit put on his spectacles.  "where shall I begin.
please your Majesty?" he asked.

"Begin at the beginning," the King said gravely, and go on
till you come to the end; then stop."
"""
print(quotation)
```
如同上面展示的那样，由于多行用了三个双引号而不是一个，你可以在多行字面量中使用单个双引号 " 。要在多行字符串中包含 `"""` ，你必须用反斜杠`（ \ ）`转义至少其中一个双引号。举例来说：

```swift
let threeDoubleQuotes = """
Escaping the first quote \"""
Escaping all three quote \"\"\"
"""
print(threeDoubleQuotes)
```

在这个多行格式中，字符串字面量包含了双引号包括的所有行。字符串起始于三个双引号`（ """ ）`之后的第一行，结束于三个双引号`（ """ ）`之前的一行，也就是说**双引号**不会开始或结束带有**换行**。下面的两个字符串是一样的：

```swift
let singleLineString = "These are the same"
let multilineString = """
There are the same
"""
```

要让多行字符串字面量开始或结束带有换行，写一个空行作为第一行或者是最后一行。比如：

```swift
"""
 
This string starts with a line feed.
It also ends with a line feed.
 
"""
```

行字符串可以缩进以匹配周围的代码。双引号`（ """ ）`前的空格会告诉 `Swift` 其他行前应该有多少空白是需要忽略的。比如说，尽管下面函数中多行字符串字面量**缩进**了，但实际上字符串不会以任何空白开头。

```swift
func generateQuotation() -> String {
    let quotation = """
        The white rabbit put on his spectacles.  "where shall I begin.
        please your Majesty?" he asked.

        "Begin at the beginning," the King said gravely, and go on
        till you come to the end; then stop."
        """
    return quotation
}
print(generateQuotation())
print(quotation == generateQuotation()) //true
```
总而言之，如果你在某行的空格超过了**结束**的*双引号*`（ """ ）`，那么这些空格会被包含。也就是说以结束的`"""`左右标尺衡量。如图：
<img style="margin: auto;" src="multilineStringWhitespace.png" alt="多行尾双引号为准" title="多行尾双引号为准">

### 初始化一个空字符串
为了绑定一个更长的字符串，要在一开始创建一个空的 `String`值，要么赋值一个空的字符串字面量给变量，要么使用初始化器语法来初始化一个新的 `String`实例：

```swift
var emptyString = ""
var anotherEmptyString = String()
```
通过检查布尔量 isEmpty属性来确认一个 String值是否为空：

```swift
if emptyString.isEmpty {
    print("Nothing to see here")
}
```
### 字符串可变性
你可以通过把一个 `String`设置为**变量**（*这里指可被修改*），或者为**常量**（*不能被修改*）来指定它是否可以被修改（或者改变）：

```swift
var variableString = "Horse"
variableString += " and carriage"

let constantString = "Highlander"
constantString += "and another Highlander" //compile-time error
```
### 字符串是值类型
`Swift` 的 `String`类型是一种值类型。如果你创建了一个新的 `String`值， `String`值在传递给方法或者函数的时候会被**复制**过去，还有赋值给常量或者变量的时候也是一样。每一次赋值和传递，现存的 `String`值都会被复制一次，传递走的是**拷贝**而不是原本。

`Swift` 的默认拷贝 `String`行为保证了当一个方法或者函数传给你一个 `String`值，你就绝对拥有了这个 `String`值， _无需关心它从哪里来。_ 你可以确定你传走的这个字符串除了你自己就不会有别*人改变它*。

### 操作字符
你可以通过在 `for-in`循环里遍历 `characters`属性访问 `String` 中的每一个独立的 `Character`值：

```swift
for character in "Dog!🐶".characters {
    print(character)
}
// D
// o
// g
// !
// 🐶
```
另外，你可以通过提供 `Character`类型标注来从**单个字符**的字符串字面量创建一个独立的 `Character`常量或者变量：

```swift
let exclamationMark: Character = "!"
```
`String`值可以通过传入 `Character`值的字符串作为实际参数到它的初始化器来构造：

```swift
let catCharacters: [Character] = ["c", "a", "t", "!", "🐱"]
let catString = String(catCharacters)
print(catString);
//print "cat!🐱"
```

### 连接字符串和字符
`String`值能够被加起来（或者说连接），使用加运算符`（+）`来创建新的`String`值，你同样也可以使用加赋值符号`（+=）`在已经存在的 `String`值末尾追加一个 `String`值：

```swift
let string1 = "hello"
let string2 = " there"
var welcome = string1 + string2 // welcome now equals "hello there"

var instruction = "look over"
instruction += string2 // instruction now equals "look over there"
```
你使用 `String`类型的 `append()`方法来可以给一个 `String`变量的末尾追加 `Character`值：

```swift
let exclamationMark: Character = "!"
welcome.append(exclamationMark)
```
### 字符串插值
*字符串插值*是一种从混合**常量、变量、字面量和表达式的字符串字面量**构造新 `String`值的方法。每一个你插入到字符串字面量的元素都要被一对圆括号包裹，然后使用*反斜杠前缀*：

```swift
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier)) * 2.5"
// message is "3 times 2.5 is 7.5"
```
在上边的栗子当中， `multiplier`的值以 `\(multiplier)`的形式插入到了字符串字面量当中。当字符串插值需要被用来创建真的字符串的时候，这个占位符就会被 `multiplier`的**真实**值代替。

### Unicode
`Unicode` 是一种在不同书写系统中编码、表示和处理文本的国际标准。它允许你表示几乎标准化格式的任何语言中的任何字符，并且为外部源比如文本文档或者网页读写这些字符。如同这节中描述的那样，`Swift` 的 `String`和 `Character`类型是完全 *Unicode* 兼容的。

#### Unicode 标量
面板之下，`Swift` 的原生 `String` 类型建立于 *Unicode* 标量值之上。一个 *Unicode* 标量是一个为字符或者修饰符创建的独一无二的**21位数字，**比如 `LATIN SMALL LETTER A (" a")`的 `U+0061` ，或者` FRONT-FACING BABY CHICK  ( "🐥" )`的 `U+1F425` 。
> *Unicode* 标量码位位于 `U+0000`到 `U+D7FF`或者 `U+E000`到 `U+10FFFF`之间。*Unicode* 标量码位不包括从 `U+D800`到 `U+DFFF`的**16**位码元码位。

#### 字符串字面量中的特殊字符
字符串字面量能包含以下特殊字符：

* 转义特殊字符 `\0 (空字符)`， `\\ (反斜杠)`， `\t (水平制表符)`， `\n (换行符)`， `\r(回车符)`，` \" (双引号)` 以及 `\' (单引号)`；
* 任意的 *Unicode* 标量，写作 `\u{n}`，里边的 n是一个 `1-8` 个与合法 *Unicode* 码位相等的*16*进制数字。

下边的代码展示了这些特殊字符的四个栗子。 `wiseWords`常量包含了两个转义双引号字符。 `dollarSign`， `blackHeart`和 `sparklingHeart`常量展示了 *Unicode* 标量格式：

```swift
let wiseWords = "\"Imagination is more improtant than knowledge\" - Einstein"
// "Imagination is more important than knowledge" - Einstein
let dollarSign = "\u{24}" // $, Unicode scalar U+0024
let blackHeart = "\u{2665}" // ♥, Unicode scalar U+2665
let sparklingHeart = "\u{1F496}" // 💖, Unicode scalar U+1F496
```

#### 扩展字形集群
每一个 `Swift`的 `Character`类型实例都表示了单一的*扩展字形集群*。扩展字形集群是一个或者多个有序的 **Unicode** 标量（当组合起来时）产生的单个人类可读字符。
举个栗子。字母 `é`以单个 **Unicode** 标量 `é` `( LATIN SMALL LETTER E WITH ACUTE`, 或者 `U+00E9`)表示。总之，同样的字母**也可以用一对标量**——一个标准的字母 `e`` ( LATIN SMALL LETTER E`，或者说`  U+0065`)，以及 `COMBINING ACUTE ACCENT`标量`( U+0301)`表示。 `COMBINING ACUTE ACCENT`标量会以*图形方式应用*到它前边的**标量**上，当 **Unicode** 文本渲染系统渲染时，就会把 `e`转换为 `é`来输出。

```swift
let eAcute: Character = "\u{e9}" // é
let combinedEAcute: Character = "\u{65}\u{301}"
// eAcute is é, combinedEAcute is é
```
扩展字形集群是一种非常灵活的把各种复杂脚本字符作为单一 `Character`值来表示的方法。比如说韩文字母中的音节能被表示为**复合和分解**序列两种。这两种表示在 `Swift` 中都**完全合格于单一** `Character`值：

```swift
let precomposed: Character = "\u{d55c}" //한
let decomposeed: Character = "\u{1112}\u{1161}\u{11ab}" // ᄒ, ᅡ, ᆫ
// precomposed is 한, decomposed is 한
```
扩展字形集群允许**封闭标记**的标量 (比如 `COMBINING ENCLOSING CIRCLE`, 或者说 `U+20DD`) 作为单一 `Character`值来圈住其他 `Unicode` 标量：

```swift
let enclosedEAcute: Character = "\u{E9}\u{20DD}"// enclosedEAcute is é⃝
```
区域指示符号的 `Unicode` 标量可以成对组合来成为单一的 `Character`值，比如说这个 `REGIONAL INDICATOR SYMBOL LETTER` **U ( U+1F1FA)**和 `REGIONAL INDICATOR SYMBOL LETTER` **S  ( U+1F1F8)**：

```swift
let regionalIndicatorForUS: Character = "\u{1f1fa}\u{1f1f8}" // regionalIndicatorForUS is 🇺🇸
```

### 字符统计
要在字符串中取回 `Character`值的总数，使用字符串 `characters`属性中的的 `count`属性：

```swift
let unusualMenagerie = "Koala🐨, Snail 🐌, Penguin 🐧, Dromedary 🐪"
print("unusualMenagerie has \(unusualMenagerie.characters.count) characters") 
// prints "unusualMenagerie has 40 characters"
```
注意 `Swift` 为 `Character`值使用的**扩展字形集群***意味着字符串的创建和修改可能不会总是影响字符串的字符统计数*。
比如说，如果你使用四个字符的`cafe`来初始化一个新的字符串，然后追加一个 C*OMBINING ACUTE ACCENT*  `( U+0301)`到字符串的末尾，字符串的字符统计结果将仍旧是` 4`，但第四个字符是` é`而不是 `e`：

```swift
var word = "cafe"
print("the number of characters in \(word) is \(word.characters.count)")
//prints "the number of characters in cafe is 4"
word += "\u{301}" // COMBINING ACUTE ACCENT, U+301
print("the number of characters in \(word) is \(word.characters.count)")
//prints "the number of characters in café is 4"
```
**注意：**
> 扩展字形集群能够组合一个或者多个 `Unicode` 标量。这意味着不同的字符——以及相同字符的不同表示——能够获得不同大小的内存来储存。因此，`Swift` 中的字符并不会在字符串中获得相同的内存空间。所以说，字符串中字符的数量如果不遍历它的扩展字形集群边界的话，是不能被计算出来的。如果你在操作特殊的长字符串值，要注意 `characters`属性为了确定字符串中的字符要遍历整个字符串的 `Unicode` 标量。

> 通过 `characters`属性返回的字符统计并不会总是与包含相同字符的 `NSString`中 `length`属性相同。 `NSString`中的长度是基于在字符串的 `UTF-16` 表示中16位码元的数量来表示的，而不是字符串中 `Unicode` 扩展字形集群的数量。

### 访问和修改字符串
你可以通过下标脚本语法或者它自身的属性和方法来访问和修改字符串。

#### 字符串索引
每一个 `String`值都有相关的索引类型， `String.Index`，它相当于每个 `Character`在字符串中的位置。

如上文中提到的那样，不同的字符会获得不同的内存空间来储存，所以为了明确哪个 `Character` 在哪个特定的位置，你必须从 `String`的开头或结尾遍历每一个 `Unicode` 标量。因此，`Swift` 的字符串不能通过整数值索引。

使用 `startIndex`属性来访问 `String`中第一个 `Character`的位置。 `endIndex`属性就是 `String`中最后一个字符后的位置。所以说， `endIndex`属性并不是字符串下标脚本的合法实际参数。如果 `String`为空，则 `startIndex`与 `endIndex`相等。

使用 `index(before:)` 和 `index(after:)` 方法来访问给定索引的前后。要访问给定索引更远的索引，你可以使用 `index(_:offsetBy:) `方法而不是多次调用这两个方法。

你可以使用下标脚本语法来访问 `String`索引中的特定 `Character`。

```swift
let greeting = "Guten Tag!"
greeting[greeting.startIndex]
// G
greeting[greeting.index(before: greeting.endIndex)]
// !
greeting[greeting.index(after: greeting.startIndex)]
// u
let index = greeting.index(greeting.startIndex, offsetBy: 7)
greeting[index]
// a
```
尝试访问的 `Character`如果索引位置在字符串范围之外，就会触发运行时错误。

```swift
greeting[greeting.endIndex] //运行时报错
greeting.index(after: greeting.endIndex) //运行时报错
```
使用 `characters`属性的 `indices`属性来创建所有能够用来访问字符串中独立字符的索引范围 `Range`。

```swift
for index in greeting.characters.indices {
//    print("\(greeting[index])")
    print("\(greeting[index])", terminator: " ") //G u t e n   T a g !
}
```
**注意：**
> 你可以在任何遵循了 `Indexable` 协议的类型中使用 `startIndex` 和 `endIndex` 属性以及 `index(before:) ， index(after:) `和 `index(_:offsetBy:) `方法。这包括这里使用的 `String` ，还有集合类型比如 `Array` ， `Dictionary` 和 `Set`。

#### 插入和删除
要给字符串的特定索引位置插入字符，使用 `insert(_:at:)`方法，另外要冲入另一个**字符串**的内容到特定的索引，使用 `insert(contentsOf:at:) `方法。

```swift
var wellcome = "hello"
wellcome.insert("!", at: wellcome.endIndex) //"hello!"
wellcome.insert(contentsOf: " there".characters, at: wellcome.index(before: wellcome.endIndex)) 
//"hello there!"
```
要从字符串的特定索引位置**移除字符**，使用 `remove(at:)`方法，另外要移除一小段特定范围的**字符串**，使用 `removeSubrange(_:)` 方法

```swift
wellcome.remove(at: wellcome.index(before: wellcome.endIndex)) //"hello there"
wellcome.removeSubrange(wellcome.index(wellcome.endIndex, offsetBy: -6)..<wellcome.endIndex) //"hello"
```
**注意：**
> 你可以在任何遵循了 RangeReplaceableIndexable 协议的类型中使用 insert(_:at:) ， insert(contentsOf:at:) ， remove(at:) 方法。这包括了这里使用的 String ，同样还有集合类型比如 Array ， Dictionary 和 Set 。

### 字符串比较
`Swift` 提供了*三*种方法来比较文本值：*字符串*和*字符相等性*，*前缀相等性以及后缀相等性*。
#### 字符串和字符相等性
如同比较运算符中所描述的那样，**字符串和字符**相等使用“*等于*”运算符` ( ==)` 和“*不等*”运算符 `( !=)`进行检查：

```swift
let quotation1 = "We`re a lot alike, you and I."
let sameQuotation = "We`re a lot alike, you and I."
if quotation1 == sameQuotation {
    print("These two strings are considered equal")
}
```
两个 `String`值（或者两个 `Character`值）如果它们的扩展字形集群是**规范化相等**，则被认为是*相等*的。如果扩展字形集群拥有相同的**语言意义和外形**，我们就说它**规范化相等**，就算它们实际上是由*不同的 `Unicode` 标量组合而成*。

比如说， `LATIN SMALL LETTER E WITH ACUTE` *( U+00E9)*是规范化相等于 `LATIN SMALL LETTER`* E( U+0065)*加 `COMBINING ACUTE ACCENT` *( U+0301)*的。这两个扩展字形集群都是表示字符`é`的合法方式，所以它们被看做*规范化相等*：

```swift
// "Voulez-vous un café?" using LATIN SMALL LETTER E WITH ACUTE
let eAcuteQuestion = "Voulez-vous un caf\u{e9}?"
// "Voulez-vous un café?" using LATIN SMALL LETTER E and COMBINING ACUTE ACCENT
let combinedEAuteQuestion = "Voulez-vous un caf\u{65}\u{301}"
if eAcuteQuestion == combinedEAuteQuestion {
    print("These two strings are considered equal") // prints "These two strings are considered equal"
}
```
反而， `LATIN CAPITAL LETTER A` ( `U+0041`, 或者说 "A")在英语当中是不同于俄语的 `CYRILLIC CAPITAL LETTER A` ( U+0410,或者说 "А")的。字符看上去差不多，但是它们拥有不同的语言意义，所以不相等。

#### 前缀和后缀相等性
要检查一个字符串是否拥有特定的字符串前缀或者后缀，调用字符串的 `hasPrefix(_:)`和 `hasSuffix(_:)`方法，它们两个都会接受一个 `String` 类型的实际参数并且返回一个布尔量值。

举例，你可以使用 `hasPrefix(_:)`方法操作 `romeoAndJuliet`数组来计算第一场场景的数量：

```swift
let romeoAndJuliet = ["Act 1 Scene 1: Verona, A public place",
                      "Act 1 Scene 2: Capulet's mansion",
                      "Act 1 Scene 3: A room in Capulet's mansion",
                      "Act 1 Scene 4: A street outside Capulet's mansion",
                      "Act 1 Scene 5: The Great Hall in Capulet's mansion",
                      "Act 2 Scene 1: Outside Capulet's mansion",
                      "Act 2 Scene 2: Capulet's orchard",
                      "Act 2 Scene 3: Outside Friar Lawrence's cell",
                      "Act 2 Scene 4: A street in Verona",
                      "Act 2 Scene 5: Capulet's mansion",
                      "Act 2 Scene 6: Friar Lawrence's cell"]
var act1SceneCount = 0
for scene in romeoAndJuliet {
    if scene.hasPrefix("Act 1") {
        act1SceneCount += 1
    }
}
print("There are \(act1SceneCount) scenes in Act 1")// Prints "There are 5 scenes in Act 1"
```
同样的，使用 `hasSuffix(_:)`方法来计算后缀可等性。
> 如同字符串和字符相等性一节所描述的那样， `hasPrefix(_:)`和 `hasSuffix(_:)`方法只对字符串当中的每一个扩展字形集群之间进行了一个逐字符的规范化相等比较。

### 字符串的 Unicode 表示法
当一个 `Unicode` 字符串写入文本文档或者其他储存里边的时候，这个字符串的 `Unicode` 标量会被编码为*一个或者一系列* `Unicode` 定义的编码格式。每一种格式都把字符串编码成所谓**码元的小块**。这些包括 `UTF-8` 编码格式（它把字符串以8 码元编码），`UTF-16` 编码格式（它把字符串按照 16位 码元 编码），以及 `UTF-32` 编码格式（它把字符串以32位码元编码）。

`Swift` 提供了几种不同的方法来访问字符串的 `Unicode` 表示。你可以使用 `for-in`语句来遍历整个字符串，来访以 `Unicode` *扩展字形集群*的方式，访问单独的 `Character`值。

 或者，你也可以用以下三者之一的其他 `Unicode` 兼容表示法来访问 `String`值：

 * `UTF-8` 码元的集合（关联于字符串的 `utf8`  属性）
 * `UTF-16` 码元的集合（关联于字符串的 `utf16`  属性）
 * 21位 `Unicode` 标量值的集合，等同于字符串的 `UTF-32` 编码格式（关联于字符串的 `unicodeScalars` 属性）
 
 下边的每一个栗子都展示了接下来的字符串的不同表示方法，这个字符串由字符 `D ， o ， g ， ‼  ( DOUBLE EXCLAMATION MARK,` 或者说 `Unicode` 标量 `U+203C)`以及 `?` 字符`( DOG FACE , 或者说 Unicode 标量 U+1F436)`组成：
 
```swift
let dogString = "Dog‼🐶"
```
#### UTF-8 表示法
你可以通过遍历 `utf8`属性来访问一个 `String`的 `UTF-8` 表示法。这个属性的类型是 `String.UTF8View`，它是非负8位（ `UInt8`）值，在字符串的 `UTF-8` 表示法中每一个字节的内容：
 <img style="margin: auto," src="UTF8_2x.png", alt="UTF-8", title="UTF-8">
 
```swift
let dogString = "Dog‼🐶"
for codeUnit in dogString.utf8 {
    print("\(codeUnit)", terminator: " ")
}
print("")
// 68 111 103 226 128 188 240 159 144 182 
```
上文中的栗子，前三个十进制 `codeUnit`值 `( 68, 111, 103)`表示了字符 D , o , 和 g ，它们的 `UTF-8` 表示法与它们的 `ASCII` 表示法相同。接下来的三个十进制 `codeUnit`值 `( 226, 128, 188)`是 `DOUBLE EXCLAMATION MARK`字符的三字节 `UTF-8 `表示法。最后四个 `codeUnit`值 `( 240, 159, 144, 182)`是 `DOG FACE`字符的四字节 `UTF-8` 表示法。

#### UTF-16 表示法
你可以通过遍历 `utf16`属性来访问 `String`的 `UTF-16 `表示法。这个属性的类型是`String.UTF16View`，它是非负 `16位（ UInt16）`值，在字符串 `UTF-16` 表示法中每一个 16位 的内容：
<img style="margin: auto," src="UTF16_2x.png", alt="UTF16", title="UTF16_表示法">

```swift
for codeUnit in dogString.utf16 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// Prints "68 111 103 8252 55357 56374 "
```
再一次，前三个 `codeUnit`值 `( 68, 111, 103)`表示了字符 D , o , 和 g，它们的 `UTF-16` 码元与字符串 `UTF-8` 表示法中的值相同（因为这些 `Unicode` 标量表示 `ASCII` 字符）。

第四个 `codeUnit`值( `8252`)是与十六进制值 `203C`相等的十进制数字，它表示了 `DOUBLE EXCLAMATION MARK`字符的 `Unicode` 标量 `U+203C`。这个字符可以在 `UTF-16` 中表示为单个码元了。

第五和第六个 `codeUnit`值 ( `55357`和 `56374`)是 `UTF-16` 16位码元对表示的 DOG FACE字符。这些值是**高**16位码元值 *U+D83D*（十进制值为 `55357`）和**低**16位码元值 *U+DC36*（十进制值为 `56374`）。

#### Unicode 标量表示法
你可以通过遍历 `unicodeScalars`属性来访问 `String`值的 `Unicode` 标量表示法。这个属性的类型是 `UnicodeScalarView`，它是 `UnicodeScalar`类型值的合集。
每一个 `UnicodeScalar`都有值属性可以返回一个标量的21位值，用 *UInt32*值表示：
<img style="margin: auto," src="UnicodeScalar_2x.png" alt="UnicodeScalar_2x.png" title="UnicodeScalar_表示法">

```swift
for codeUint in dogString.unicodeScalars {
    print("\(codeUint.value)", terminator: " ")
}
print("")
// 68 111 103 8252 128054
```
前三个 `UnicodeScalar`值的 `value`属性 `( 68, 111, 103) `还是表示了字符 D, o, 和 g。

第四个 `codeUnit`值 ( `8252`)还是等于十六进制值 `203C`的十进制值，它表示了` DOUBLE EXCLAMATION MARK`字符的 `Unicode` 标量 `U+203C`。

第五个和最后一个 `UnicodeScalar`的 `value`属性， `128054`，是一个等于十六进制值 `1F436`的十进制数字，它表示了 DOG FACE字符的 `Unicode` 标量 `U+1F436`。

作为查询它们 `value`属性的替代方法，每一个 `UnicodeScalar`值同样可以用来构造新的 `String`值，比如说使用字符串插值：

```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar)")
}
//D
//o
//g
//‼
//🐶
```

> 摘自：[swift 官网](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/index.html#//apple_ref/doc/uid/TP40014097-CH3-ID0)
> 所有代码在Xcode9 Swift4 环境编译没问题，代码[戳这里 https://github.com/keshiim/LearnSwift4](https://github.com/keshiim/LearnSwift4)

