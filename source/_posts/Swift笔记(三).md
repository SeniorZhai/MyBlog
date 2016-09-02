title: Swift笔记(三)
date: 2014-07-10 13:11:08
categories: iOS
tags: [iOS,Swift]
---
## 字符串和字符（String and Character）
Swift的String类型和Foundation的NSString类进行了无缝桥接，所有NSString API都可以调用Swift的String类型的值

### 字符串字面量(String Literals)
字符串字面量是由双引号("")包裹的具有固定顺序的文本字符集
字符串字面量可以包含以下特殊字符：
- 转义字符\0(空字符)、\\(反斜线)、\t(水平制表符)、\n(换行符)、\r(回车符)、\"(双引号)、\'(单引号)
- 单字节 Unicode 标量，写成\xnn，其中nn为两位十六进制数
- 双字节 Unicode 标量，写成\unnnn，其中nnnn为四位十六进制数
- 四字节 Unicode 标量，写成\Unnnnnnnn，其中nnnnnnnn为八位十六进制数
### 初始化空字符串
```swift
var emptyString = ""
var antherEmptyString = String()
if emptyString.isEmpty {
    //
}
```
### 字符串可变性(String Mutability)
可以通过分配一个变量来对字符串进行修改，或者分配一个常量保证其不被修改
### 字符串是值类型
String类型进行常量、变量赋值操作或在函数\方法中传递，会进行拷贝，是值传递
### 字符
Swift的String类型表示特定序列的character类型值的集合
```swift
for character in "Hello world"}
    println(character)
}
```
可以表明`Character`类型来创建字符常量或者变量
```Swift
let a:Character = "1"
```
### 计算字符数量
通过调用全局的`countElement`函数，并将字符串作为参数传递，可以获取字符串的字符数量
```
let str = "I have a word"
pritln(\(coutElemnts(str)));
```
### 连接字符串和字符
使用`+`相加就可以连接字符串
使用`+=`可以讲一个字符串加上另一个字符串存到原有的字符串上
### 字符串插值
字符串插值是一种构建新字符串的方式，可以在其中包含常量、变量、字面量和表达式
```swift
\(Double(3) * 2.5) // "7.5"
```
### 比较字符串
Swift 提供了三种方式来比较字符串的值：字符串相等、前缀相等和后缀相等。
#### 字符串相等
```swift
let str = "一样样的"
str == "一样样的" // true
#### 前缀/后缀相等
调用字符串的`hasPrefix`/`hasSuffix`方法来检查字符串是否拥有特定前缀/后缀。
```swift
"I am a boy".hasPrefix("I am"); // true
```
### 大写和小写字符串
通过字符串的`uppercaseString`和`lowercaseString`属性来方位大写/小写版本的字符串
### Unicode
Unicode是一个国际标准，用于文本的编码和表示。
每一个字符都可以被Unicode解释成一个或多个unicode标量。字符的unicode标量是一个唯一的21位数字，例如`U+0061`表示小写的拉丁字母`a`
当Unicode字符串被写进文本文件或其他存储结构当中，这些unicode标量将会按照Unicode定义的集中格式之一进行编码，其中包括`UTF-8`（以8位代码单元进行编码）和`UTF-16`（以16位代码单元进行编码）
#### 字符串的Unicode表示
Swift提供了几种不同的方法来访问字符串的Unicode
- UTF-8 代码单元集合 (利用字符串的utf8属性进行访问)
- UTF-16 代码单元集合 (利用字符串的utf16属性进行访问)
- 21位的 Unicode 标量值集合 (利用字符串的unicodeScalars属性进行访问)
utfb属性其为`UTF8View`类型的属性，是无符号8位(UInt)值的集合，同理utf16属性是`UTF16View`类型的属性
```swift
let dogString "Dog!"
for codeUnit in dogString.utf8 {
    pritln("\(codeUnit)");
}
for code16Unit in dogString.utf16 {
    println("\(code16Unit)");
}
```
#### Unicode标量
unocodeScalars属性为`UnicodeScalarView`类型的属性，是`UnicodeScalar`的集合,`UnicodeScalar`是21位的 Unicode 代码点。
```swift
for scale in dogString.unicodeScalars {
    println("\(scale.value)")
}
```
集合类型(Collection Types)
## 数组
数组使用有序列表存储同一类型的多个值，相同的值可以多次出现在一个数组的不同位置中
在Swift中。数据值在被存储进入某个数组之前类型必须明确，方法是通过显示的类型标注或类型推断，Swift的数组是类型安全的，并且它包含的类型必须明确，这点和NSArray和NSMutableArray很不同。
### 数组的简单语法
数组遵循`Array<SomeType>`这样的形式，其中`SomeType`是这个数组中唯一允许存在的数据类型。也可以使用像`SomeType[]`这样的简单语法。
### 数组构造语句
形如`[value1,value2,value3]`
```Swift
var names:String[] = ["joy","jack"]
```
变量被声明为`字符串类型的数组`
由于Swift的类型推断机制，也可以这样写
```swift
var names = ["jay","jack"]
```
### 访问和修改数组
可以通过数组的方法和属性来访问和修改数组，或者下表语法。还可以使用数组的只读属性`count`来获取数组中的数据数量，使用`isEmpty`属性可以检测数组是否为空，使用`append`方法在数组后面添加新的数据项，也可以使用(+=)添加单个数据项或者拥有相同数据类型的数组
也可以通过索引获取数组项
```
var names = ["joy","jack"]
names.count // 2
names.isEmpty // false
names.append("alisa")
names+="Demi"
names+=["Carry","Carry"]
names[0] //"joy"
names[2...3] // ["alisa","Demi"]
```
调用`insert(atIndex:)`可以再在指定位置插入数据，`removeAtIndex`方法可以移除数组中的某一项，`removeLast`方法可以移除最后一项
### 数组的遍历
使用`for-in`循环来遍历所有数组中的数据项
```
for item in shoppingList{
   println(item) 
}
```
也可以使用全局`enumerate`函数来进行数组遍历
```swift
for (index,value) in enumerate(shoppingList) {
    println("Item \(index+1) : \(value)")
}
```
### 创建或构造一个数组
```swift
var someInts = Int[]()
var threeDoubles = Double[](count:3,repeatedValue:0.0) // 指定大小，初始值
var anotherThreeDoubles = Array(count:3,repeatedValue:2.5) // 类型推到
```

## 字典
字典是一种存储多个相同类型的值的容器，没个值都关联唯一的键，键作为字典中的这个值数据的标识符。字典的数据项没有具体的顺序，需要通过键访问数据。
与Objective-C中的`NSDictionary`和`NSMutableDictionary`类可以使用任何类型的对象做键和值不同，Swift在某个特定字典中可以存储的键和值必须提前定义，方法是通过显式标注或者类型推断
Swift的字典可以使用`Dictionary<KeyType,ValueType>`定义，其中`keyType`是键的数据类型，`ValueType`是值的数据类型。`keyType`的唯一限制是可哈希的，这样可以保证它的唯一性，所有Swift的基本类型(String,Int,Double和Bool)都是可哈希的，未关联的枚举成员也是可哈希的。
### 字典字面量
`[key1:value1,key2:value2,key3:value3]`可以创建字典
```swift
var airports:Dictionary<String,String> = ["TYO":"Tokyo","DUB":"Dublin"]
var airports = ["TYO":"Tokyo","DUB":"Dubin"]
```
### 读取和修改字典
使用下标语法或者字典的方法属性可以读取字典，只读属性`count`来获取字典的数据项的数量
也可以使用下标法添加新的数据项
`updateValue(forkey:)`方法可以设置或更新特定键对应的值，根据键值是否存在判断。该函数会返回包含一个字典值类型的可选值
```
airports["LHR"] = "London Heathrow" // 添加数据项
airports["LHR"] = "London" // 修改
if let oldValue = airports.updateValue("Dublin Internation",forKey:"DUB"){
    oldValue // 返回的是原值
}
```
使用下标法也可以访问对应键的值，如果不存在，返回`nil`，通过下标法设置某键的值为`nil`，也可以删除数据项，也可以使用`removeValueForKey`方法移除
```swift
if let removedValue = airports.removeValueForKey("DUB"){
    // removedValue为被移除的值，不存在的话返回nil
}
```
### 字典遍历
使用`for-in`语法便可遍历，每一个字典的数据项都由`(key,value)`元组形式返回
```swift
for (airportCode,airportName) in airport {
    prinln("\(airportCode):\(airportName)")
}
```
也可以访问它的`keys`和`values`属性检索一个字典的键或者值
```swift
for airportCode in airports.keys {
    println("Airport code: \(airportCode)")
}
let airportNames = Array(airports.values)
```
### 创建字典
创建空字典
```swift
var nameOfIntegers = Dictionary<Int,String>()
var nameOfIntegers = [:]
```

## 集合的可变性
如果数组或字典设置为变量，那么它的数据项是可变的，设置为常量，那么它的大小是不可变的，数据在首次设定之后便不能改变。不同的是，数组的大小不能改变，但是可以改变它的值。
控制流
## For循环
`for`循环用作按照指定的次数多次执行一系列语句。Swift提供了两种`for`循环形式：
- `for-in`用来遍历一个区间(range)，序列(sequence),集合(collection)，系列(progression)里面多有的元素
- for条件递增(`for-condition-increment`)语句，用来重复执行一系列语句知道达到特定条件，一般通过在每次循环完成后增加计数器的值来实现。
### For-In
```swift
// index不需要声明
for index in 1...5{
    println("\(index)");
}
```
如果不需要知道区间每一项的值，可以使用下划线(`_`)替代变量名忽略对值的访问
```
for _ in 1...9{
    //
}
```
### For条件递增（for-condition-increment）
```swift
for var index = 0; index < 3;++index{
    //
}
```
下面是一般情况下这种循环方式的格式：
>for `initialization`; `condition`; `increment` {
    statements
}


## While循环
`while`循环运行一系列语句直到条件变成`false`。这类循环适合使用在第一次迭代前迭代次数未知的情况下。`Swift` 提供两种`while`循环形式：
- while循环，每次在循环开始时计算条件是否符合；
- do-while循环，每次在循环结束时计算条件是否符合。
一般格式如下：
```swift
while condition{
    statements
}
do {
    statements
} while condition
```

## 条件语句
Swift提供两种类型的条件语句：`if`语句和`switch`语句。通常，当条件较为简单且可能的情况很少时，使用`if`语句。而`switch`语句更适用于条件较复杂、可能情况较多且需要用到模式匹配（pattern-matching）的情境。
### If
if语句最简单的形式就是只包含一个条件，当且仅当该条件为true时，才执行相关代码：
```swift
if true {
    //
}
```
### Switch
switch语句会尝试把某个值与若干个模式（pattern）进行匹配。根据第一个匹配成功的模式，switch语句会执行对应的代码。
```swift
witch some value to consider {
    case value 1:
        respond to value 1
    case value 2,
    value 3:
        respond to value 2 or 3
    default:
        otherwise, do something else
}
```
swift不存在隐藏的贯穿，即不需要break，执行完也会推出，同时swift还支持区间匹配
```swift
switch count{
    case 0:
        //
    case 1...3:
        //
}
```
元组匹配
```swift
var count = (1,3)
switch count{
    case (0,0):
        //
}
```
#### 值绑定
case分支的模式允许将匹配的值绑定到一个临时的常量或变量，这些变量或常量在该case分支里就可以被引用————这种行为被称为值绑定
```swift
let point = (2,0)
switch point {
case (let x,0):
    //
case (0, let y):
    //
case let (x,y):
    // 
}
```
#### Where
case分支模式可以使用`where`语句来判断额外的条件
```swift
let point = (1,0)
switch point {
    case let (x,y) where x == y:
        //
}
```

### 控制转移语句（Control Transfer Statements）
控制转移语句改变你代码的执行顺序，通过它你可以实现代码的跳转。Swift有四种控制转移语句。- continue
- break
- fallthrough
- return
#### Continue
continue语句告诉一个循环体立刻停止本次循环迭代，重新开始下次循环迭代。
#### Break
break语句会立刻结束整个控制流的执行。
#### 贯穿（Fallthrough）
Swift不存在C语言中switch语句的贯穿行为，要实现需要使用`fallthrouth`
```swift
switch count{
    case 1:
        //
        fallthrough
    default:
        // 这里会被执行
}
```

### 带标签的语句（Labeled Statements）
环体和switch代码块两者都可以使用break语句来提前结束整个方法体，通过`带标签的语句`可以指定想要终止哪个循环或者switch代码块，如果有许多嵌套的循环体，也可以实现`continue`指定跳转
```swift
label name:while condition{
    statenments
}
```
```swift
loop:while count != 100{
    if count < 100 {count += 10}
    switch count {
        case 1..20 :
            break loop
        case 21...99:
            //
    }
}
```
函数(Functions)
## 函数的定义与调用(Defining and Calling Functions)
```swift
func sayHello(personName:String) -> String{
	let greeting = "Hello," + personName + "!"
	return greeting
}
println("\(sayHeloo("zoe"))")
```
## 函数参数与返回值(Function Parameters and Return Values)
- 多个参数
```swift
func fun(start: Int,end: Int) -> Int {
	return end - start
}
```
- 无参
```swift
func say() -> String{
	return "hello"
}
```
- 无返回值
```swift
func sayGoodbye(personName: String){
	println("Goodbye",\(personName)!)
}
```
- 多返回值
// 计算一个字符串中元音、辅音和其他字母的个数
```swift
func count(str:String) -> (vowels: Int, consonants: Int,others: Int) {
	 var vowels = 0, consonants = 0, others = 0
    for character in string {
        switch String(character).lowercaseString {
        case "a", "e", "i", "o", "u":
            ++vowels
        case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
          "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
            ++consonants
        default:
            ++others
        }
    }
    return (vowels, consonants, others)
}
let total = count("some arbitrary string!")
println("\(total.vowels) vowels and \(total.consonants) consonants")
```
### 函数参数名称
在函数定义时定义的参数为`局部参数名`，只能在函数体中使用
#### 外部函数名
类似于Objective-C的函数命名
```swift
func join(string s1:String,toString s2:String) -> String{
	return s1 + s2
}
join(string:"hello",toString:"world")
```
#### 简写外部参数名
上面的方法需要为参数提供外部参数名和内部参数名，可以通过`# `简写，将外部参数名和内部参数名等同起来
```swift
func join(# string:String,# toString:String) -> String{
	return string + toString
}
join(string:"hello",toString:"world")
```
### 默认参数值
为参数提供一个初始值，调用时，缺省可以忽略并使用默认值
```swift
func join(str1 String,str2 String = "world") -> String{
	return str1 + str2
}
```
### 可变参数
```swift
func add (numbers: Double...) -> Double {
	var total: Double = 0;
	for number in numbers {
		total += number
	}
	return total / Double(numbers.cout)
}
```
### 常量参数和变量参数
函数参数默认是常量，在函数体中更改参数值将会导致编译错误，如果参数定义为变量就可以当做参数的副本来使用
```swift
// 用来右对齐输入的字符串到一个长的输出字符串中。左侧空余的地方用指定的填充字符填充
func alignRight(var string: String, count: Int, pad: Character) -> String {
    let amountToPad = count - countElements(string)
    for _ in 1...amountToPad {
        string = pad + string
    }
    return string
}
```
### 输入输出参数
一个输入输出参数时，在参数定义前加`inout`关键字。一个输入输出参数有传入函数的值，这个值被函数修改，然后被传出函数，替换原来的值。当传入的参数作为输入输出参数时，需要在参数前加&符，表示这个值可以被函数修改。
```swift
func swap (inout a:Int,inout b:Int){
	let temp = a
	a = b
	b = temp
}
var a = 3,b = 4
swap(&a,&b) 
```
### 函数类型
每个函数都有种特定的函数类型，由函数的参数类型和返回类型组成。
`() -> ()`表示没用参数，返回`Void`，在Swift中，`Void`与空元组是一样的
### 使用函数类型
使用函数名来给另一个函数赋值
```swift
var fun2:(Int,Int) -> Int = fun1
```
#### 使用函数类型
```swift
var mathFunction:(Int,Int) -> Int = addTwoInts
// 只要matchFunction和addTwoInts的类型相同，该赋值就是合法的
```
#### 函数类型作为参数类型
```swift
func printMathResult(matchFunction:(Int,Int) -> Int,a:Int,b:Int){
	println("Result: \(mathFunction(a,b))")
}
printMathResult(addTwoInts,3,5)
```
#### 函数类型作为返回类型
```swift
func chooseStepFunction(backwards:Bool) -> (Int) -> Int {
	return backwards ? stepBackward : stepForward
}
```
闭包
闭包之自包含的函数代码块，可以在代码中传递和使用，与Objective-C的blocks相似
闭包可以捕获和存储其所在上下文任意常量和变量的引用。
全局和嵌套函数也是特殊的闭包，闭包采取如下三种形式：
- 全局函数是一个有名字但不会捕获任何值的闭包
- 嵌套函数是一个有名字并可以捕获其封闭函数内值的闭包
- 闭包表达式是一个利用轻量级语法所写的可以捕获其上下文中变量或常量值的匿名闭包

## 闭包表达式(Closure Expreessions)
闭包表达式是一种利用简洁语法构建内联闭包的方式，闭包表达式提供了一些语法优化，使得撰写闭包变得简单明了。
### 闭包表达式语法
闭包表带式语法一般形式：
```swift
{ (parameters) -> returnType in
	statemnts
}
```
Swift标准库提供了`sort`函数，会根据基于输出类型排序的闭包函数将已知类型数组的值进行排序，返回一个与原数组大小相同的新数组，并排序完成。

```