---
title: Swift：方法
date: 2017-11-22 17:44:49
categories:
    - 学习
    - Swift
tags:
    - Swift
---

**方法** 是关联了*特定类型*的 _函数_ 。类，结构体以及枚举都能定义**实例方法**，方法封装了给定类型特定的任务和功能。类，结构体和枚举同样可以定义**类型方法**，这是与类型本身关联的方法。类型方法与 Objective-C 中的类方法相似。

事实上在 _结构体和枚举中定义方法_ 是 Swift 语言与 C 语言和 Objective-C 的**主要区别**。在 Objective-C 中，类是唯一能定义方法的类型。但是在 Swift ，你可以选择无论类，结构体还是枚举，它们都拥有强大的灵活性来在你创建的类型中定义方法。

### 实例方法
实例方法 是属于 **特定** 类实例、结构体实例或者枚举实例的函数。他们为这些实例提供功能性，要么通过提供访问和修改实例属性的方法，要么通过提供与实例相关的功能。实例方法与函数的语法完全相同。

要写一个实例方法，你需要把它放在对应类的花括号之间。实例方法默认可以访问同类下所有其他实例方法和属性。**实例方法只能在类型的具体实例里被呼叫（`call`）**。它不能在独立于实例而被调用。

举例子：这里有个定义 `Counter`类的栗子，它可以用来计算动作发生的次数：

```swift
class Counter {
    var count = 0
    func increment() {
        count += 1
    }
    func increment(by amount: Int) {
        count += amount
    }
    func reset() {
        count = 0
    }
}
```
`Counter` 类定义了三个实例方法：

* `increment`每次给计数器增加 1；
* `increment(by: Int)`按照特定的整型数量来增加计数器；
* `reset`会把计数器重置为零。

`Counter`类同样声明了一个*变量属性* `count`来追踪当前计数器的**值**。

调用实例方法与属性一样都是用**点语法**：

```swift
let counter = Counter()
// the initial counter value is 0
counter.increment()
// the counter's value is now 1
counter.increment(by: 5)
// the counter's value is now 6
counter.reset()
// the counter's value is now 0
```
如同函数实际参数标签和形式参数名中描述的那样，同样的，对于方法的形式参数来说与函数也是神同步的，因为方法就是与类型关联的《函数》。

#### self 属性
每一个类的实例都隐含一个叫做 `self`的属性，它完完全全与实例本身相等。你可以使用 `self`属性来在当前实例当中调用它自身的方法。
在上边的例子中， `increment()`方法可以写成这样：

```swift
func increment() {
    self.count += 1
}
```
实际上，你不需要经常在代码中写 `self`。如果你没有显式地写出 `self`，Swift会在方法中使用已知属性或者方法的时候**假定你是调用了当前实例中的属性或者方法**。这个假定通过在 `Counter`的三个实例方法中使用 `count`（而不是 `self.count`）来做了**示范**。

对于这个规则的一个重要例外就是当一个实例方法的形式参数名与实例中某个属性拥有相同的名字的时候。在这种情况下，**形式参数名具有优先权**，并且调用属性的时候使用更加严谨的方式就很有必要了。你可以使用 `self`来**区分***形式参数名和属性名*。

这时， `self`就避免了叫做 `x` 的方法形式参数还是同样叫做 `x` 的实例属性这样的歧义。

```swift
struct Point {
    var x = 0.0, y = 0.0
    func isToTheRightOf(x: Double) -> Bool {
        return self.x > x
    }
}
let somePoint = Point(x: 4.0, y: 5.0)
if somePoint.isToTheRightOf(x: 1.0) {
    print("This point is to the right of the line where x == 1.0")
}
//Print "This point is to the right of the line where x == 1.0"
```
除去 `self` 前缀，Swift 将会假定两个 x 都是叫做`( x )`的方法**形式参数**。

#### 在实例方法中修改值类型
结构体和枚举是**值类型**。默认情况下，值类型属性不能被自身的实例方法修改。

总之，如果你需要在*特定的方法中修改结构体或者枚举的属性*，你可以选择将这个方法*异变*。然后这个方法就可以在方法中异变（嗯，改变）它的属性了，并且任何改变在方法结束的时候都会写入到原始的结构体中。方法同样可以指定一个全新的实例给它隐含的 `self`属性，并且这个新的实例将会在方法结束的时候替换掉现存的这个实例。
你可以选择在 `func`关键字前放一个 `mutating`关键字来使用这个行为：

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
var somePoint = Point(x: 1.0, y: 1.0) // not let somePoint
somePoint.moveBy(x: 2.0, y: 3.0)
print("The point is now at (\(somePoint.x), \(somePoint.y))")
// prints "The point is now at (3.0, 4.0)"
```
上文中的 `Point` 结构体定义了一个**异变**方法 `moveBy(x:y:)`，它以特定的数值移动一个 `Point`实例。相比于返回一个新的点，这个方法实际上修改了调用它的点。 被添加到定义中的 `mutating`关键字允许它修改自身的属性。

注意，如同 **常量**结构体实例的存储属性 里描述的那样，你不能在**常量结构体类型里调用异变方法**，因为自身属性不能被改变，就算它们是变量属性：

下面代码中调用会编译报错：

```swift
let fixedPoint = Point(x: 3.0, y: 3.0)
fixedPoint.moveBy(x: 2.0, y: 3.0)
// this will report an error
```
#### 在变异方法里指定自身
变异方法可以指定整个**实例给隐含的`self`属性**。上文中那个`Point`的栗子可以用下边的代码替代：

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```
这次的异变方法 `moveBy(x:y:)`创建了一个 `x`和 `y`设置在目的坐标的全新的结构体。调用这个版本的方法和的结果会和之前那个完全一样。

**枚举**的*异变方法*可以设置隐含的 `self`属性为相同枚举里的不同成员：

```swift
enum TriStateSwitch {
    case off, low, high
    mutating func next () {
        switch self {
        case .low:
            self = .high
        case .off:
            self = .low
        case .high:
            self = .off
        }
    }
}
var ovenLight = TriStateSwitch.low
ovenLight.next()
// ovenLight is now equal to .high
ovenLight.next()
// ovenLight is now equal to .off
```
这个栗子定义了一个三种开关状态的枚举。每次调用 `next()`方法时，这个开关就会在三种不同的电力状态（ `Off` , `low`和 `high`）下切换。
### 类方法
如上文描述的那样，*实例方法*是特定类型实例中调用的方法。你同样可以定义在类型本身调用的方法。这类方法被称作**类型方法**。你可以通过在 `func`关键字之前使用 `static`关键字来明确一个类型方法。
类同样可以使用 `class`关键字来允许子类重写父类对类型方法的实现。

> 注意：
> 在 Objective-C 中，你只能在 Objective-C 的类中定义**类级别**的方法。但是在 Swift 里，你可以在所有的类里定义类级别的方法，**还有结构体和枚举**。每一个类方法都能够对它自身的类范围显式生效。

类型方法和实例方法一样**使用点语法调用**。不过，你得在类上调用类型方法，而不是这个类的实例。接下来是一个在 `SomeClass`类里调用类型方法的栗子：

```swift
class SomeClass {
    class func someTypeMethod() {
        print("调用了someclass")
    }
}
SomeClass.someTypeMethod()
//Prints "用了someclass"
```
在类型方法的函数体中，**隐含**的 `self`属性指向了类本身而不是这个类的实例。对于结构体和枚举，这意味着你可以使用 `self`来 _消除类型属性和类型方法形式参数之间的歧义_ ，用法和实例属性与实例方法形式参数之间的用法完全相同。

一般来说，你在类型方法函数体内书写的任何非完全标准的方法和属性名 都将会指向另一个类级别的*方法和属性*。一个类型方法可以使用方法名调用另一个类型方法， _并不需要使用类型名字作为前缀_ 。同样的，结构体和枚举中的类型方法也可以通过直接使用类型属性名而不需要写类型名称前缀来访问类型属性。

下边的栗子定义了一个叫做 `LevelTracker`的结构体，它通过不同的等级或者阶层来追踪玩家的游戏进度。这是一个单人游戏，但是可以在一个设备上储存多个玩家的信息。

当游戏第一次开始的时候所有的的游戏等级（除了第一级）都是锁定的。每当一个玩家完成一个等级，那这个等级就对设备上的所有玩家解锁。 `LevelTracker`结构体使用类型属性和方法来追踪解锁的游戏等级。它同样追踪每一个独立玩家的当前等级。

```swift
struct LevelTracker {
    static var hightestUnlockedLevel = 1
    var currentLevel = 1
    
    static func unlock(_ level: Int) {
        if level > hightestUnlockedLevel { hightestUnlockedLevel = level }
    }
    static func isUnlocked(_ level: Int) -> Bool {
        return level <= hightestUnlockedLevel
    }
    @discardableResult
    mutating func advance(to level: Int) -> Bool {
        if LevelTracker.isUnlocked(level) {
            currentLevel = level
            return true
        } else {
            return false
        }
    }
}
```
`LevelTracker`结构体持续追踪任意玩家解锁的最高等级。这个值被储存在叫做 `highestUnlockedLevel`的类型属性里边。

`LevelTracker`同时还定义了两个类型函数来操作 `highestUnlockedLevel`属性。第一个类型函数叫做 `unlock(_:)`，它在新等级解锁的时候更新 `highestUnlockedLevel`。第二个是叫做 `isUnlocked(_:)`的便捷类型方法，如果特定等级已经解锁，则返回 `true`。（注意这些类型方法可以访问 `highestUnlockedLevel`类型属性而并不需要写作 `LevelTracker.highestUnlockedLevel`。）

除了其自身的类型属性和类型方法， `LevelTracker`还追踪每一个玩家在游戏中的进度。它使用一个实例属性 `currentLevel`来追踪当前游戏中的游戏等级。

为了帮助管理 `currentLevel`属性， `LevelTracker`定义了一个叫做 `advance(to:)`的实例方法。在更新 `currentLevel`之前，这个方法会检查请求的新等级是否解锁。 `advance(to:)`方法返回一个布尔值来明确它是否真的可以设置 `currentLevel`。由于调用时**忽略** `advance(to:)` 的返回值并不是什么大不了的问题，这个函数用 `@discardableResult` **标注**。

看下边的栗子， `LevelTracker`结构体与 `Player`类共同使用来追踪和更新每一个玩家的进度：

```swift
class Player {
    var tracker = LevelTracker()
    let playerName: String
    func completedLevel(level: Int) {
        LevelTracker.unlockLevel(level + 1)
        tracker.advanceToLevel(level + 1)
    }
    init(name: String) {
        playerName = name
    }
}
```
`Player`类创建了一个新的 `LevelTracker`实例 来追踪玩家的进度。它同事提供了一个叫做 `complete(level:)`的方法，这个方法在玩家完成一个特定等级的时候会被调用。这个方法会为所有的玩家解锁下一个等级并且更新玩家的进度到下一个等级。（ `advance(to:)`返回的布尔值被忽略掉了，因为等级已经在先前调用 `LevelTracker.unlock(_:)`时已知解锁。）
你可以通过为 `Player`类创建一个实例来新建一个玩家，然后看看当玩家达成等级`1`时会发生什么：

```swift
var player = Player(name: "Argyrios")
player.complete(level: 1)
print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")
// Prints "highest unlocked level is now 2"
var player = Player(name: "Argyrios")
player.complete(level: 1)
print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")
// Prints "highest unlocked level is now 2"
```
如果你创建了第二个玩家，当你让他尝试进入尚未被任何玩家在游戏中解锁的等级时， 设置玩家当前等级的尝试将会失败：

```swift
player = Player(name: "Beto")
if player.tracker.advance(to: 6) {
    print("player is now on level 6")
} else {
    print("level 6 has not yet been unlocked")
}
// Prints "level 6 has not yet been unlocked"
```

> 摘自：[swift 官网](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/index.html#//apple_ref/doc/uid/TP40014097-CH3-ID0)
> 所有代码在Xcode9 Swift4 环境编译没问题，代码[戳这里 https://github.com/keshiim/LearnSwift4](https://github.com/keshiim/LearnSwift4)

