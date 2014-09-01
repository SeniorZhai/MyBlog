title: Swift笔记(一)
date: 2014-06-17 13:08:43
categories: iOS
tags: [iOS,Swift]
---
##Swift初识
WWDC2014年6月3日苹果开发者大会发布，2010年7约开始开发
- 基于C语言和Objective-C语言，使用现有的Cocoa和Cocoa Touch框架，无缝兼容C、Objective-C语言
- 兼具编译语言的高性能(Performance)和脚背语言的交互性(Interactive)
- 支持Playground，它允许程序实时预览，无需频繁创建和运行App
- 简洁、安全、容易、灵活、高效
##值
使用`let`来声明常量，使用`var`来声明变量。常量只能赋一次值。

值声明的类型必须和赋的值一致，声明时类型是可选的，声明的同时赋值，编译器会自动推断类型。也可以在声明时，指定类型。
```swift
var myVariable = 42
let myConstant = 42
let explicitDouble:Double = 70
```
值永远不会被隐式转换为其他类型，如果需要把一个值转换成其他类型，需要显示转换。
```swift
let label = "The width is"
let width = 94
let widthLabel = label + String(width)
```
有一种更简单的把值转换成字符串的方法：把值写到括号里，并且在括号之前加一个反斜杠。
```swift
let apples = 3
let appleSummary = "I have \(apples) apples."
```
使用方括号`[]`来创建数组和字典，并使用下标或者键(key)来访问元素。
```swift
var shoppingList = [catfish", "water", "tulips", "blue paint"]
shoppingList[1] = "bottle of water"

var occupations = [
	"Malcolm": "Captain",
    "Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"
```
创建一个空数组或者字典，可以使用初始化语法
```
let emptyArray = String[]()
let emptyDictionary = Dictionary<String,Float>()
```
如果类型信息可以被推断出来，你可以用[]和[:]来创建数组和空字典————就像声明变量或者给函数传参数的时候一样。
```swift
shoppingList = []
```

##控制流
使用`if`和`switch`来进行条件操作，使用`for-in`、`for`、`while`和`do-while`来进行循环，包括条件和循环变量的括号可以省略，但是语句体的大括号是必须的。

在`if`语句中，条件必须是一个布尔表达式

`switch`支持任意类型的数据及各种比较操作
```swift
let vegetable = "red pepper"
switch vegetable {
	let vegetableComment = "Add some raisins and make ants on a log."
case "cucumber", "watercress":
    let vegetableComment = "That would make a good tea sandwich."
case let x where x.hasSuffix("pepper"):
    let vegetableComment = "Is it a spicy \(x)?"
default:
    let vegetableComment = "Everything tastes good in soup."
}
```
运行`switch`中匹配到的子句之后，程序会退出`switch`语句，并不会继续向下运行，所以不需要在每个子句结尾写`break`。

`for-in`可以遍历字典
```swift
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
largest
```
使用`while`来重复运行一段代码直到不满足条件。循环条件可以在开头也可以在结尾。
```swift
var n = 2
while n < 100 {
    n = n * 2
}
n

var m = 2
do {
    m = m * 2
} while m < 100
m
```

你可以在循环中使用`..`来表示范围，也可以使用传统的写法，两者是等价的：
```swift
var firstForLoop = 0
for i in 0..3 {
    firstForLoop += i
}
firstForLoop

var secondForLoop = 0
for var i = 0; i < 3; ++i {
    secondForLoop += 1
}
secondForLoop
```
##函数和闭包
使用`func`来声明一个函数，使用名字和参数来调用函数，使用`->`来指定函数返回值
```swift
func greet(name:String,day:String) -> String{
	return "Hello \(name),today is \(day)."
}
greet("Bob","Tuesday")
```
使用一个元组来返回多个值
```swift
func getGasPrices() -> (Double,Double,Double){
	return (3.59,3.69,3.79)
}
getGasPrices()
```
函数可以带有可变个数的参数，这些参数在函数内表现为数组的形式：
```swift
func sumOf(number:Int...) -> Int {
	var sum = 0
	for number in numbers {
		sum += number
	}
	return sum
}
sumOf(42,597,12)
```
函数可以嵌套，被嵌套的函数可以访问外侧函数的变量
```swift
func returnFifteen() -> Int {
	var y = 10
	func add() {
		y += 5
	}
	add()
	return y
}
returnFifteen()
```
函数也可以作为另一个函数的返回值
func makeIncrementer() -> (Int-> Int){
	func addOne(number: Int) -> Int {
		return 1 + number
	}
	return addOne
}
var increment = makeIncrementer()
increment(7)
```
函数也可以当做参数传入另一个函数
```swift
func hasAnyMatches(list:Int[],condition:Int -> Bool) -> Bool{
	for item in list {
		if condition(item){
			return true
		}
	}
	return false;
}
func lessThanTen(number: Int) -> Bool {
	return number < 10
}
var numbers = [20,19,7,12]
hasAnyMatches(numbers,lessThanTen)
```
函数实际上是一种特殊的闭包，可以使用`{}`来创建一个匿名闭包，使用`in`来将参数和返回值类型声明与闭包函数体进行分离
```swift
numbers.map({
	(number:Int) -> Int in
	let result = 3 * number
	return result
})
```
如果一个闭包的类型已知，比如作为一个回调函数，可以忽略参数的类型和返回值，单个语句闭包会把它语句的值当作结果返回
```swift
number.map({number in 3 *number})
```
可以通过参数位置而不是参数名字来引用参数————这种方法在非常短的闭包中很有用，当一个闭包作为最后一个参数传给一个函数的时候，它可以直接跟在括号后面
```swift
sort([1,5,3,12,2]){$0 > $1}

##对象和类
使用`class`和类名来创建一个类，类中属性的声明和常量、变量声明一样，唯一区别的是上下文是类，同样方法和函数声明也一样
```swift
class Shape{
	var numberOfSides = 0
	func simpleDescriotion() -> String{
		return "A shape with \(numberOfSides) sides."
	}
}
```
创建一个类的实例，在类名后加上括号，使用点语法来访问实例的属性和方法
```swift
var shape = Shape()
shape.numberOfSides = 7
var shapeDescription = shape.simpleDescription()
```
使用`init`来创建一个构造器
```swift
class NamedShape{
	vae numberOfSides:Int = 0
	var name:String

	init(name:String) {
		self.name = name
	}

	func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```
如果你需要在删除对象之前进行一些清理工作，使用`deinit`创建一个析构函数。

子类的定义方法是在类名后加上父类的名字，用冒号分割，创建类的时候并不需要一个标准的根类，可以忽略父类

子类如果重写父类的方法的话，需要用`override`标记————如果没有添加`override`就重写父类方法的话编译器会报错，编译器同样会检测`override`标记的方法是否确实在父类中。
```swift
class Square:NamsedShape {
	var sideLength:Double

	init(sideLength:Double,name:String){
		self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 4
	}
	func area() ->  Double {
        return sideLength * sideLength
    }

    override func simpleDescription() -> String {
        return "A square with sides of length \(sideLength)."
    }
}
let test = Square(sideLength: 5.2, name: "my test square")
test.area()
test.simpleDescription()
```
属性可以有 getter 和 setter 。
```swift
class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 3
    }

    var perimeter: Double {
    get {
        return 3.0 * sideLength
    }
    set {
        sideLength = newValue / 3.0
    }
    }

    override func simpleDescription() -> String {
        return "An equilateral triagle with sides of length \(sideLength)."
    }
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")
triangle.perimeter
triangle.perimeter = 9.9
triangle.sideLength
```
在`perimeter`的setter中，新值的名字是`newValue`。也可以再set之后显示的设置一个名字

不需要计算属性，但是仍然需要在设置一个新值之前或者之后运行代码，使用`willSet`和`didSet`
```swift
// 确保三角形的边长总是和正方形的边长相同
class TriangleAndSquare {
	var triangle : EquilateralTriangle {
	willSet{
		square.sideLength = newValue.sideLength
	}
	}
	var square:Square {
	willSet {
		triangle.sideLength = newValue.sideLength
	}
	}
	init(size:Double,name:String){
		square = Square(sideLength:size,name:name)
		triangle = EquilateralTriangle(sideLength:size,name:name)
	}
}
var triangeleAndSquare = TriangleAndSquare(size:10,name:"another test shape")
triangleAndSquare.square.sideLength
triangleAndSquare.triangle.sideLenth
triangleAndSquare.square = Square(sideLength:50,name:"larger square")
triangleAndSquare.triangle.sideLength
```
类中的方法和一般的函数有一个重要的区别，函数的参数名只在函数内部使用，但是方法的参数名需要在调用的时候显示说明(除了第一个参数)。默认情况下，方法的参数名和它在方法内部的名字一样，不过也可以定义第二个名字，这个名字被用在方法内部
```swift
class Counter {
	var count:Int = 0
	func incrementBy(amout:Int,numberOfTimers times:Int){
		count += amount * times
	}
}
var counter = Counter()
counter.incrementyBy(2,numberOfTimes:8)
```
出来变量的可选值时，可以再操作（比如方法、属性和子脚本）之前加`?`，如果`?`之前的值是`nil`,`?`后面的东西都会被忽略，并且整个表达式返回`nil`，否则，`?`之后的东西都会被运行，这两种情况下，整个表示式的值也是一个可选值
```swift
let optionalSquare:Square ?= Square(sideLength:2.5,name:"optional square")
let sideLength = optionalSquare?.sideLength
```

##枚举和结构体
使用`enum`来创建一个枚举，枚举可以包含方法
```swift
enum Rank:Int {
	case Ace = 1
	case Two,Three,Four,Five,Six,Seven,Eight,Nine,Ten
	case Jack,Queen,King
	func simpleDesription() -> String {
		switch self{
		case .Ace:
			return "ace"
		case .Jack:
			return "Jack"
		case .Queen:
			return "Queen"
		case .King:
			return "King"
		default:
			return String(self.toRaw())
		}
	}
}
let ace = Rank.Ace
let aceRawValue = ace.toRaw()
```
上面的枚举原始值的类型是`Int`，所以需要设置当一个原始值，剩下的原始值会按照顺序赋值。也可以使用字符串或者浮点数作为枚举的原始值。

使用`toRaw`和`fromRaw`函数来在原始值和枚举值之间进行转换
```swift
if let convertedRank = Rank.froRaw(3) {
	let threeDescription = convertedRank.simpleDescription()
}
```
枚举的成员是实际值，并不是原始值的另一种表达式。如果原始值没有意义，不重要设置
```swift
enum Suit {
	case Spades,Hearts,Diamonds,Clubs
	func simpleDescription() -> String {
		switch self {
		case .Spades:
			return "spades"
		case .Hearts:
			return "hearts"
		case .Diamonds:
			return "diamonds"
		case .Clubs
			return "clubs"
		}
	}
}
let hearts = Suit.Hearts
let heartsDescription = hearts.simpleDescription()
```
使用`struct`来创建一个结构体，结构体和类有很多相同的地方，比如方法和构造器，他们之间最大的区别就是，结构体是值传递，类是引用传递
```swift
struct Card{
	var rank:Rank
	var suit:Suit
	func simpleDescription() -> String {
		return "The \(rank.simpleDescription()) of \(suit.simpleDescription)"
	}
}
let threeOfSpades = Card(rank: .Three,suit: .Spades)
let threeOfSpadesDescription = threeOfSpades.simleDescription()
```
##协议和扩展
使用`protocol`来声明一个协议
```swift
protocol ExampleProtocol {
	var simpleDescription: String{get}
	mutating func adjust()
}
```
类、枚举和结构体都可以实现协议
```swift
class SimpleClass:ExampleProtocol {
	var simpleDescription:String = "A very simple class"
	var anotherProperty: Int = 69105
	func adjust(){
		simpleDescription += "Noew 100% adjusted."
	}
}
var a = SimpleClass()
a.adjust()
let aDescription = a.simpleDescription

struct SimpleStructire:ExampleProtocol{
	var simpleDescription:String = "A simple structure"
	mutating func adjust(){
		simpleDescription += "(adjust)"
	}
}
var b = SimpleStructire()
b.adjust()
let bDescruption = b.simpleDescription
```
声明的时候`mutating`关键字用来标记一个会修改结构体的方法。

使用`extension`来为现有的类型添加功能，比如新的方法和参数。
```swift
extension Int: ExampleProtocol{
	var simpleDescription:String{
		return "The number \(self)"
	}
	mutating func adjust(){
		self += 42
	}
}
7.simpleDescription
```
你可以像使用其他命名类型一样使用协议名——例如，创建一个有不同类型但是都实现一个协议的对象集合。当你处理类型是协议的值时，协议外定义的方法不可用。
```swift
let protocolValue: ExampleProtocol = a
protocolValue.simpleDescription
// protocolValue.anotherProperty  // Uncomment to see the error
```

##泛型
在尖括号里写一个名字来创建一个泛型函数或者类型
```swift
func repeat<ItemType>(item: ItemType,times:Int) -> ItemType[]{
	var result = ItemType[]()
	for i in 0..times{
		result += item
	}
	return result
}
repeat("knock",4)
```
也可以创建泛型类、枚举和结构体
```swift
enum OptionalValue<T> {
	case None
	case Some(T)
}
var possibleInteger: OptionalValue<Int> = .None
possibleInteger = .Some(100)
```
在类型名后面使用`where`来指定类型的需求，比如，限定类型实现某一个协议，限定两个类型是相同的，或者限定某个类必须有一个特定的父类
```swift
func anyCommonElements <T,U where T:Sequence,U:Sequence,T.GeneratorType.Element:Equatable,T.GeneratorType.Element == U.GeneratorType.Element> (1hs: T, rhs: U) -> Bool {
	or lhsItem in lhs {
        for rhsItem in rhs {
            if lhsItem == rhsItem {
                return true
            }
        }
    }
    return false
}
anyCommonElements([1, 2, 3], [3])
```
简单起见，可以忽略where，只在冒号后面写协议或者类名。<T: Equatable>和<T where T: Equatable>是等价的。