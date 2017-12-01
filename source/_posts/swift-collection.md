---
title: Swift：集合类
date: 2017-10-30 15:16:57
categories:
    - 学习
    - Swift
tags:
    - Swift
---
`Swift` 提供了三种主要的集合类型，所谓的数组、合集还有字典，用来储存值的集合。数组是有序的值的集合。合集是唯一值的无序集合。字典是无序的键值对集合。
<img style="margin: .auto," src="CollectionTypes_intro_2x.png", alt="CollectionTypes_intro_2x.png" title="CollectionTypes_介绍">

`Swift` 中的数组、合集和字典总是明确能储存的值的类型以及它们能储存的键。就是说你不会意外地插入一个错误类型的值到集合中去。它同样意味着你可以从集合当中取回确定类型的值。

> `Swift` 的数组、合集和字典是以泛型集合实现的。

### 集合的可变性
如果你创建一个*数组*、*合集*或者*一个字典*，并且赋值给一个变量，那么创建的集合就是**可变的**。这意味着你随后可以通过**添加、移除、或者改变集合中**的元素来改变（或者说异变）集合。如果你把数组、合集或者字典赋值给一个常量，则集合就成了**不可变的**，它的*大小*和*内容*都不能被改变。
> 集合不需要改变的情况下创建不可变集合是个不错的选择。这样做可以允许 Swift 编译器优化你创建的集合的性能。

### 数组
数组以有序的方式来储存相同类型的值。相同类型的值可以在数组的不同地方多次出现。
> `Swift` 的 `Array`类型被桥接到了基础框架的 `NSArray`类上。

#### 数组类型简写语法
`Swift` 数组的类型*完整写*法是` Array<Element>`， `Element`是数组允许存入的值的**类型**。你同样可以*简写数组的类型*为 `[Element]`。尽管两种格式功能上相同，我们**更推荐简写**并且涉及到数组类型的时候都会使用*简写*。
#### 创建一个空数组
```swift
var someInts = Array<Int>()//完整数组声明
var someInts = [Int]() //简写数组声明
print("someInts is of type[Int] with \(somInts.count) items.")
//prints "someInts is of type [Int] with 0 items."
```
注意 `someInts`变量的类型通过初始化器的类型推断为 `[Int]`。

**相反**，如果内容*已经*提供了*类型信息*， _比如说作为函数的实际参数或者已经分类了的变量或常量，你可以通过**空数组字面量来创建一个空数组**，它写作`[ ]`（一对空方括号）_ ：

```swift
someInts.append(2)
//someInts now contains 1 value of type Int
someInts = [] //数组的字面量复制
//someInts is now an emtpy array, but is still of type [Int]
```

#### 使用默认值创建数组
`Swift` 的 `Array`类型提供了初始化器来创建确定大小且元素都设定为相同默认值的数组。你可以传给初始化器对应类型的默认值（叫做 `repeating`）和新数组元素的数量（叫做 `count`）：

```swift
var threeDoubles = Array(repeating: 0.0, count: 3)
print(threeDoubles)
// threeDoubles is of type [Double], and equals [0.0, 0.0, 0.0]
```

#### 通过连接两个数组来创建数组
你可以通过把**两个兼容类型**的现存数组用加运算符`（+）`加在一起来创建一个新数组。新数组的类型将从你相加的数组里推断出来：

```swift
var anotherThreeDoubles = Array(repeating: 2.5, count: 3)
// anotherThreeDoubles is of type [Double], and equals [2.5, 2.5, 2.5]
 
var sixDoubles = threeDoubles + anotherThreeDoubles
// sixDoubles is inferred as [Double], and equals [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
```
#### 使用数组字面量创建数组
你同样可以使用*数组字面量*来初始化一个数组，它是一种以数组集合来写一个或者多个值的*简写*方式。数组字面量写做一系列的值，用逗号分隔，用方括号括起来：

```swift
[value 1, value 2, value 3]
var shoppingList: [String] = ["Eggs", "Milk"]
```
`shoppingList`变量被声明为“字符串值的数组”写做 `[String]` 。由于这个特定的数组拥有特定的`String`值类型，它就只能储存 `String`值。这里， `shoppingList`被两个 `String`值`（ "Eggs"和 "Milk"）`初始化，写在字符串字面量里。

依托于 `Swift` 的*类型推断*，如果你用包含相同类型值的数组*字面量*初始化数组，就不需要写明数组的类型。`shoppingList`的初始化可以写得更短：

```swift
var shoppingList = ["Egg", "Milk"]
```
因为数组字面量中的值都是相同的类型，Swift 就能够推断 [String]是 shoppingList变量最合适的类型。
#### 访问和修改数组
你可以通过数组的方法和属性来修改数组，或者使用下标脚本语法。要得出数组中元素的*数量*，检查只读的 `count`属性：

```swift
print("The shopping list contains \(shoppingList.count) items.")
// prints "The shopping list contains 2 items."
```
使用布尔量 `isEmpty`属性来作为检查 count属性是否等于 `0`的快捷方式：

```swift
if shoppingList.isEmpty {
    print("The shopping list is empty.")
} else {
    print("The shopping list is not empty.")
}
// prints "The shopping list is not empty."
```
你可以通过 `append(_:)`方法给数组末尾添加新的元素：

```swift
shoppingList.append("Flour")
// shoppingList now contains 3 items, and someone is making pancakes
```
另外，可以使用加赋值运算符 `(+=)`来在数组末尾添加一个或者多个同类型元素：

```swift
shoppingList += ["Baking Powder"]
// shoppingList now contains 4 items
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
//// shoppingList now contains 7 items
```

通过*下标脚本语法*来从数组当中取回一个值，在紧跟数组名后的方括号内传入你想要取回的值的索引：

```swift
var fristItem = shoppingList[0]
```
你可以使用*下标脚本语法*来改变给定索引中已经存在的值：

```swift
shoppingList[0] = "Six eggs"
// the first item in the list is now equal to "Six eggs" rather than "Eggs"
```
你同样可以使用*下标脚本语法*来一次改变一个*范围*的值，就算*替换与范围长度不同*的值的合集也行。下面的栗子替换用 "Bananas"和 "Apples"替换 "Chocolate Spread", "Cheese", and "Butter"：

```swift
shoppingList[4...6] = ["Bananas", "Apples"]
// shoppingList now contains 6 items
```
调用 `insert(_:at:)`方法**插入**了一个新元素值为 `"Maple Syrup"`到 `shopping list` 的最前面，通过明确索引位置为 0 .
类似地，你可以使用` remove(at:)`方法来**移除**一个元素。这个方法移除特定索引的元素并且返回它（尽管你不需要的话可以无视返回的值）：

```swift
shoppingList.insert("Maple Syrup", at: 0)
let mapleSyrup = shoppingList.remove(at: 0)
```
#### 遍历一个数组
你可以用 `for-in`循环来遍历整个数组中值的合集

```swift
for item in shoppingList {
    print(item)
}
//Six eggs
//Milk
//Flour
//Baking Powder
//Bananas
//Apples
```
如果你需要每个*元素以及值的整数索引*，使用 *enumerated()*方法来遍历数组。 *enumerated()*方法返回数组中每一个*元素的元组*，包含了这个元素的**索引和值**。你可以分解元组为临时的*常量*或者*变量*作为遍历的一部分：

```swift
for (index, value) in shoppingList.enumerated() {
    print("Item \(index + 1): \(value)")
}
Item 2: Milk
Item 3: Flour
Item 4: Baking Powder
Item 5: Bananas
Item 6: Apples
```

### 合集
*合集*将同一类型且**不重复的值**、**无序地**储存在一个集合当中。当元素的顺序不那么重要的时候你就可以使用合集来代替数组，或者你需要确保元素不会重复的时候。
*注意：`Swift` 的 `Set`类型桥接到了基础框架的 `NSSet`类上。*

#### Set 类型的哈希值
为了能让类型储存在合集当中，它必须是**可哈希的**——*就是说类型必须提供计算它自身哈希值的方法*。哈希值是`Int`值且所有的对比起来相等的对象都相同，比如 `a == b`，它遵循 `a.hashValue == b.hashValue。`

所有 `Swift` 的基础类型（比如 `String, Int, Double`, 和 `Bool`）*默认都是可哈希的*，并且可以用于合集或者字典的**键**。*没有关联值的*枚举成员值（如同枚举当中描述的那样）同样默认可哈希。

> 你可以使用你自己自定义的类型作为合集的值类型或者字典的键类型，只要让它们遵循 Swift 基础库的 Hashable协议即可。遵循 Hashable协议的类型必须提供可获取的叫做 hashValue的 Int属性。通过 hashValue属性返回的值不需要在同一个程序的不同的执行当中都相同，或者不同程序。

> 因为 Hashable协议遵循 Equatable，遵循的类型必须同时一个“等于”运算符 ( ==)的实现。 Equatable协议需要任何遵循 ==的实现都具有等价关系。就是说， ==的实现必须满足以下三个条件，其中 a, b, 和 c是任意值：

> * `a == a`  (自反性)
* `a == b` 意味着` b == a`  (对称性)
* `a == b && b == c` 意味着 `a == c ` (传递性)

#### 合集类型语法
`Swift` 的合集类型写做 `Set<Element>`，这里的 `Element`是合集要储存的类型。不同与数组，合集*没有等价的简写*。

#### 创建并初始化一个空合集
你可以使用初始化器语法来创建一个确定类型的空合集：

```swift
var letters = Set<Character>()
print("letters is of type Set<Character> with \(letters.count) items.")
// prints "letters is of type Set<Character> with 0 items."
```
另外，如果内容已经提供了类型信息，比如函数的实际参数或者已经分类的变量常量，你就可以**用空的数组字面量**来创建一个空合集：

```swift
letters.insert("a")
// letters now contains 1 value ('a') of type Charactor
print(letters)
letters = []
print(letters)
// letters is now an empty set, but is still of type Set<Character>
```
#### 使用数组字面量创建合集
你同样可以*使用数组字面量*来初始化一个合集，算是一种写一个或者多个合集值的快捷方式。

```swift
var favoriteGenres: Set<String> = ["Rock",  "Classical", "Hip hop"]
// favroiteGenres has been initialized with three initial items
```
`favoriteGenres`变量被声明为“ `String`值的合集”，写做 `Set<String>`。由于这个合集已经被明确值类型为 `String`，它只允许储存 `String`值。这时，合集 `favoriteGenres`用三个写在数组字面量中的 `String`值 `( "Rock", "Classical", 和 "Hip hop")`初始化。

由于 `Swift` 的类型推断，你不需要在使用包含*相同类型值的数组字面量初始化合集*的时候写合集的**类型**。 `favoriteGenres` 的初始化可以写的更短一些：

```swift
var favoriteGenres: Set = ["Rock",  "Classical", "Hip hop"]
//Swift 就可以推断 Set<String>是 favoriteGenres变量的正确类型
```
#### 访问和修改合集
`Swift`提供一些列API来访问和修改集合：`count、isEmpty、insert(_:)、remove(_:)、removeAll()、contains(_:)`
#### 遍历合集
你可以在 `for-in`循环里遍历合集的值。
`Swift` 的 `Set`类型是**无序的**。要以特定的顺序遍历合集的值，使用 `sorted()`方法，它把合集的元素作为使用 `<` 运算符排序了的数组返回

```swift
for genre in favoriteGenres {
    print(genre)
}
//Hip hop
//Rock
//Classical
for genre in favoriteGenres.sorted() {
    print(genre)
}
//Classical
//Hip hop
//Rock
```
### 执行合集操作
下边的示例描述了两个合集—— `a`和 `b`——在各种合集操作下的结果，用阴影部分表示。
<img style='margin: .auto' src='setVennDiagram_2x(1).png' alt='setVennDiagram_2x (1)' title='集合关系'>

* 使用 `intersection(_:)`方法来创建一个只包含两个合集共有值的新合集；
* 使用 `symmetricDifference(_:)`方法来创建一个只包含两个合集各自有的非共有值的新合集；
* 使用 `union(_:)`方法来创建一个包含两个合集所有值的新合集；
* 使用 `subtracting(_:)`方法来创建一个两个合集当中不包含某个合集值的新合集。

下面的示例描述了三个合集—— `a`， `b`和 `c`——用重叠区域代表合集之间值共享。合集 `a`是合集 `b`的超集，因为 `a`包含 `b`的所有元素。相反地，合集 `b`是合集 a`的`子集，因为 `b`的所有元素被 `a`包含。合集 `b`和合集 `c`是不相交的，因为他们的元素没有相同的。

<img style='margin: .auto' src='setEulerDiagram_2x(2).png' alt='setEulerDiagram_2x(2)' title='集合关系'>

* 使用“相等”运算符 `( == )`来判断两个合集是否包含有相同的值；
* 使用 `isSubset(of:)` 方法来确定一个合集的所有值是被某合集包含；
* 使用 `isSuperset(of:)`方法来确定一个合集是否包含某个合集的所有值；
* 使用 `isStrictSubset(of:)` 或者 `isStrictSuperset(of:)`方法来确定是个合集是否为某一个合集的*子集或者超集*，**但并不相等**；
* 使用 `isDisjoint(with:)`方法来判断两个合集是否拥有完全不同的值。

```swift
let houseAnimals: Set = ["🐶", "🐱"]
let farmAnimals: Set = ["🐮", "🐔", "🐑", "🐶", "🐱"]
let cityAnimals: Set = ["🐦", "🐭"]
print(houseAnimals.isSubset(of: farmAnimals))   //true
print(farmAnimals.isSuperset(of: houseAnimals)) //true
print(farmAnimals.isDisjoint(with: cityAnimals))//true
```
### 字典
**字典**储存无序的互相关联的同一类型的*键*和同一类型的*值*的集合。每一个值都与唯一的键相关联，它就好像这个值的身份标记一样。不同于数组中的元素，字典中的元素没有特定的顺序。当你需要查找基于特定标记的值的时候使用字典，很类似现实生活中字典用来查找特定字的定义。
#### 字典类型简写语法
`Swift` 的字典类型写全了是这样的： `Dictionary<Key, Value>`，其中的 `Key`是用来作为字典键的值类型， `Value`就是字典为这些键储存的值的类型。
> 字典的 `Key`类型必须遵循 `Hashable`协议，就像合集的值类型

简写形式：`[Key: Value]`
#### 创建一个空字典
就像数组，你可以用初始化器语法来创建一个空 `Dictionary`：

```swift
var nameOfIntegers = [Int: String]()
```
如果内容已经提供了信息，你就可以用字典字面量创建空字典了，它写做 [:]（在一对方括号里写一个冒号）：

```swift
nameOfIntegers[16] = "sixteen"
// namesOfIntegers now contains 1 key-value pair
nameOfIntegers = [:]
// namesOfIntegers is once again an empty dictionary of type [Int: String]
```
#### 用字典字面量创建字典
你同样可以使用字典字面量来初始化一个字典，它与数组字面量看起来差不多。字典字面量是写一个或者多个键值对为 `Dictionary`集合的快捷方式。

键值对由一个键和一个值组合而成，每个键值对里的键和值用冒号分隔。键值对写做一个列表，用逗号分隔，并且最终用方括号把它们括起来：

`[key 1: value 1, key 2: value 2, key 3: value 3]`

```swift
//airports字典被声明为 [String: String]类型，它意思是“一个键和值都是 String的 Dictionary”。
var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```
与数组一样，如果你用*一致类型的字典字面量初始化字典*，就不需要写出字典的类型了。 `airports`的初始化就能写的更短：

```swift
//由于字面量中所有的键都有相同的类型，同时所有的值也是相同的类型，
//Swift 可以推断 [String: String]就是 airports字典的正确类型。
var airports = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```
#### 访问和修改字典
你可以通过字典自身的方法和属性来访问和修改它，或者通过使用下标脚本语法。

如同数组:

* 你可以使用 `count`只读属性来找出 Dictionary拥有多少元素

```swift
print("The airports dictionary contains \(airports.count) items.")
```
* 用布尔量 `isEmpty`属性作为检查 `count`属性是否等于 `0`的快捷方式

```swift
if airports.isEmpty {
    print("The airports dictionary is empty.")
} else {
    print("The airports dictionary is not empty.")
}
// prints "The airports dictionary is not empty."
```
* 你可以用下标脚本给字典添加新元素。使用正确类型的新键作为下标脚本的索引，然后赋值一个正确类型的值

```swift
airports["LHR"] = "London"
// the airports dictionary now contains 3 items
```
* 你同样可以使用下标脚本语法来改变特定键关联的值：

```swift
airports["LHR"] = "London Heathrow"
// the value for "LHR" has been changed to "London Heathrow"
```
* `updateValue(_:forKey:)`。不同于下标脚本， `updateValue(_:forKey:)`方法在执行更新之后返回旧的值。这允许你检查更新是否成功。

`updateValue(_:forKey:)`方法返回一个字典值类型的可选项值。比如对于储存 `String`值的字典来说，方法会返回 `String?`类型的值，或者说“可选的 `String`”。这个可选项包含了键的旧值如果更新前存在的话，否则就是 `nil`：

```swift
if let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {
    print("The old value for DUB was \(oldValue).")
}
// prints "The old value for DUB was Dublin."
```
* 你可以使用下标脚本语法给一个键赋值 `nil`来从字典当中移除一个键值对：

```swift
airports["APL"] = "Apple International"
// "Apple International" is not the real airport for APL, so delete it
airports["APL"] = nil
// APL has now been removed from the dictionary
```
* 另外，使用 `removeValue(forKey:)`来从字典里移除键值对。这个方法移除键值对如果他们存在的话，并且返回移除的值，如果值不存在则返回 `nil`：

```swift
if let removedValue = airports.removeValue(forKey: "DUB") {
    print("The removed airport's name is \(removedValue).")
} else {
    print("The airports dictionary does not contain a value for DUB.")
}
// Prints "The removed airport's name is Dublin Airport."
```
#### 遍历字典
你可以用 `for-in`循环来遍历字典的键值对。字典中的每一个元素返回为` (key, value)`元组，你可以解开元组成员到临时的常量或者变量作为遍历的一部分：

```swift
for (airportCode, airportName) in airports {
    print("\(airportCode): \(airportName)")
}
// YYZ: Toronto Pearson
// LHR: London Heathrow
```
你同样可以通过访问字典的 `keys`和 `values`属性来取回可遍历的字典的键或值的集合：

```swift
for airportCode in airports.keys {
    print("Airport code: \(airportCode)")
}
// Airport code: YYZ
// Airport code: LHR
 
for airportName in airports.values {
    print("Airport name: \(airportName)")
}
// Airport name: Toronto Pearson
// Airport name: London Heathrow
```
如果你需要和接收 `Array`实例的 `API` 一起使用字典的键或值，就用 `keys`或 `values`属性来初始化一个新数组：

```swift
let airportCodes = [String](airports.keys)
// airportCodes is ["YYZ", "LHR"]
let airportNames = [String](airports.values)
// airportNames is ["Toronto Pearson", "London Heathrow"]
```
`Swift` 的 `Dictionary`类型是无序的。要以特定的顺序遍历字典的键或值，使用键或值的 `sorted()`方法。

> 摘自：[swift 官网](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/index.html#//apple_ref/doc/uid/TP40014097-CH3-ID0)
> 所有代码在Xcode9 Swift4 环境编译没问题，代码[戳这里 https://github.com/keshiim/LearnSwift4](https://github.com/keshiim/LearnSwift4)

