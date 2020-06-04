                    

# Swift中数据类型

### Swift类型的介绍

*   Swift中的数据类型也有:整型/浮点型/对象类型/结构体类型等等
*   先了解整型和浮点型
*   整型

        *   有符号
        *   Int8 : 有符号8位整型
        *   Int16 : 有符号16位整型
        *   Int32 : 有符号32位整型
        *   Int64 : 有符号64位整型
        *   Int : 和平台相关(默认,相当于OC的NSInteger)

        *   无符号

        *   UInt8 : 无符号8位整型
        *   UInt16 : 无符号16位整型
        *   UInt32 : 无符号32位整型
        *   UInt64 : 无符号64位整型
        *   UInt : 和平台相关(常用,相当于OC的NSUInteger)(默认)

*   浮点型

    *   Float : 32位浮点型
    *   Double : 64浮点型(默认)

   

 ### Swift中的`类型推导`

*   Swift是强类型的语言
*   Swift中任何一个标识符都有明确的类型
*   注意:

        *   如果定义一个标识符时有直接进行赋值,那么标识符后面的类型可以省略.
    *   因为Swift有类型推导,会自动根据后面的赋值来决定前面的标识符的数据类型
    *   可以通过`option`+`鼠标左键`来查看变量的数据类型
    ### Swift中基本运算

*   Swift中在进行基本运算时必须保证类型一致,否则会出错

*   相同类型之间才可以进行运算
*   因为Swift中没有隐式转换
*   数据类型的转化
```html
//
//  main.swift
//  基本数据类型
//
//  Created by 李南江 on 15/2/28.
//  Copyright (c) 2015年 520it. All rights reserved.
//

import Foundation


/*
基本数据类型
OC:
整型  int intValue = 10;
浮点型 double doubleValue = 10.10; float floatValue = 5.1;
长 long
短 short
有符号 signed
无符号 unsigned
各种类型的数据的取值范围在不同位的编译器下取值范围不同

Swift:注意关键字大写
*/

//整型
var intValue:Int = 10
//浮点型
var intValue1:Double = 10.10  // 表示64位浮点数
var intValue2:Float = 9.9  // 表示32位浮点数

//如果按照长度划分,Swift中的长短比OC更加精确
var intValue3:Int8 = 6
var intValue4:Int16 = 7
var intValue5:Int32 = 8
var intValue6:Int64 = 9

//有符号无符号, 默认是由符号的(UInt8/UInt16/UInt32/UInt64)
var uintValue7:UInt = 10
//注意: 无符号的数比有符号的取值范围更大, 因为符号位也用来存值

//Swift是类型安全的语言, 如果取值错误会直接报错, 而OC不会
/*
取值不对
OC:unsigned int intValue = -10; 不会报错
Swift:var intValue:UInt = -10 会报错
溢出:
OC:int intValue = INT_MAX + 1; 不会报错
Swift:var intValue:UInt = UInt.max + 1 会报错
*/

/*
数据类型的相互赋值(隐式类型转换)
OC可以
int intValue = 10;
double doubleValue = intValue;

Swift:不可以
var intValue:Int = 10
var doubleValue:Double = intValue
在Swift中“值永远不会被隐式转换为其他类型”(OC中可以隐式类型转换), 以上语句会报错
*/




```

```html
//
//  main.swift
//  枚举
//
//  Created by 李南江 on 15/4/3.
//  Copyright (c) 2015年 520it. All rights reserved.
//

import Foundation
/*
Swift枚举:
Swift中的枚举比OC中的枚举强大, 因为Swift中的枚举是一等类型, 它可以像类和结构体一样增加属性和方法
格式:
enum Method{
    case 枚举值
}
*/

enum Method{
//    case Add
//    case Sub
//    case Mul
//    case Div
    // 可以连在一起写
    case Add, Sub, Mul, Div
}
// 可以使用枚举类型变量或常量接收枚举值
var m: Method = .Add
// 注意: 如果变量或常量没有指定类型, 那么前面必须加上该值属于哪个枚举类型
var m1 = Method.Add


// 利用Switch匹配
// 注意: 如果case中包含了所有的值, 可以不写default. 如果case中没有包含枚举中所有的值, 必须写default
switch (Method.Add){
    case Method.Add:
        print("加法")
    case Method.Sub:
        print("减法")
    case Method.Mul:
        print("除法")
    case Method.Div:
        print("乘法")
//    default:
//        print("都不是")
}

/*
原始值:
 OC中枚举的本质就是整数,所以OC中的枚举是有原始值的,默认是从0开始
而Swift中的枚举默认是没有原始值的, 但是可以在定义时告诉系统让枚举有原始值
enum Method: 枚举值原始值类型{
    case 枚举值
}
*/

enum Method2: Int{
    // 可以连在一起写
    case Add, Sub, Mul, Div
}
// 和OC中的枚举一样, 也可以指定原始值, 后面的值默认+1
enum Method3: Int{
    case Add = 5, Sub, Mul, Div
}

// Swift中的枚举除了可以指定整形以外还可以指定其它类型, 但是如果指定其它类型, 必须给所有枚举值赋值, 因为不能自动递增
enum Method4: Double{
    // 可以连在一起写
    case Add = 5.0, Sub = 6.0, Mul = 6.1, Div = 8.0
}
// rawValue代表将枚举值转换为原始值, 注意老版本中转换为原始值的方法名叫toRaw
print(Method4.Sub.rawValue)


// 原始值转换为枚举值
enum Method5: String{
    case Add = "add", Sub = "sub", Mul = "mul", Div = "div"
}

// 通过原始值创建枚举值
/*
注意: 
1.原始值区分大小写
2.返回的是一个可选值,因为原始值对应的枚举值不一定存在
3.老版本中为fromRaw("add")
*/
let m2 = Method5(rawValue: "add")
print(m2)

//func chooseMethod(op:Method2)
    func chooseMethod(op:String)
{
    // 由于返回是可选类型, 所以有可能为nil, 最好使用可选绑定
    if let opE = Method5(rawValue: op){
        switch (opE){
        case .Add:
            print("加法")
        case .Sub:
            print("减法")
        case .Mul:
            print("除法")
        case .Div:
            print("乘法")
        }
    }
}


/*
枚举相关值:
可以让枚举值对应的原始值不是唯一的, 而是一个变量.
每一个枚举可以是在某种模式下的一些特定值
*/
enum lineSegmentDescriptor{
    case StartAndEndPattern(start:Double, end:Double)
    case StartAndLengthPattern(start: Double, length:Double)
}

var lsd = lineSegmentDescriptor.StartAndLengthPattern(start: 0.0, length: 100.0)
lsd = lineSegmentDescriptor.StartAndEndPattern(start: 0.0, end: 50.0)
// 利用switch提取枚举关联值
/*
switch lsd
{
    case .StartAndEndPattern(let s, let e):
        print("start = \(s) end = \(e)")
    case .StartAndLengthPattern(let s, let l):
        print("start = \(s) lenght = \(l)")
}
*/

// 提取关联值优化写法
switch lsd
{
        case let .StartAndEndPattern(s, e):
            print("start = \(s) end = \(e)")
        case .StartAndLengthPattern(let s, let l):
            print("start = \(s) lenght = \(l)")
}

```

```html
//
//  main.swift
//  运算符
//
//  Created by 李南江 on 15/2/28.
//  Copyright (c) 2015年 520it. All rights reserved.
//

import Foundation

/*
算术运算符: 除了取模, 其它和OC一样, 包括优先级
+ - * / % ++ --
*/

var result = 10 + 10
result = 10 * 10
result = 10 - 10
result = 10 / 10
print(result)

/*
注意:Swift是安全严格的编程语言, 会在编译时候检查是否溢出, 但是只会检查字面量而不会检查变量, 所以在Swift中一定要注意隐式溢出
可以检测
var num1:UInt8 = 255 + 1;
无法检测
var num1:UInt8 = 255
var num2:UInt8 = 250
var result = num1 + num2
println(result)

遇到这种问题可以利用溢出运算符解决该问题:http://www.yiibai.com/swift/overflow_operators.html

&+ &- &* &/ &%
*/


/*
取模 %
OC: 只能对整数取模
NSLog(@"%tu", 10 % 3);

Swift: 支持小数取模
*/
print(10 % 3.5)


/*
自增 自减
*/
var number = 10
number++
print(number)
number--
print(number)

/*
赋值运算
= += -= /= *= %=
*/

var num1 = 10
num1 = 20
num1 += 10
num1 -= 10
num1 /= 10
num1 *= 10
num1 %= 5
print(num1)

/*
基本用法和OC一样, 唯一要注意的是表达式的值
OC: 表达式的结合方向是从左至右, 整个表达式的值整体的值等于最后一个变量的值, 简而言之就是连续赋值

Swift: 不允许连续赋值, Swift中的整个表达式是没有值的. 主要是为了避免 if (c == 9) 这种情况误写为 if (c = 9) , c = 9是一个表达式, 表达式是没有值的, 所以在Swift中if (c = 9)不成立

var num2:Int
var num3:Int
num3 = num2 = 10
*/


/*
关系运算符, 得出的值是Bool值, 基本用法和OC一样
> < >= <= == != ?:
*/
var result1:Bool = 250 > 100
var result2 = 250 > 100 ? 250 : 100
print(result2)


/*
逻辑运算符,基本用法和OC一样, 唯一要注意的是Swift中的逻辑运算符只能操作Bool类型数据, 而OC可以操作整形(非0即真)
!  && ||
*/
var open = false
if !open
{
    print("打开")
}

var age = 20
var height = 180
var wage  = 30000
if (age > 25 && age < 40 && height > 175) || wage > 20000
{
    print("约")
}

/*
区间
闭区间: 包含区间内所有值  a...b 例如: 1...5
半闭区间: 包含头不包含尾  a..<b  例如: 1..<5
注意: 1.Swift刚出来时的写法是 a..b
     2.区间只能用于整数, 写小数会有问题
应用场景: 遍历, 数组等
*/
for i in 1...5
{
    print(i)
}

for i in 1..<5
{
    print(i)
}


```

```
//
//  main.swift
//  Bool类型
//
//  Created by 李南江 on 15/2/28.
//  Copyright (c) 2015年 520it. All rights reserved.
//

import Foundation

/*
C语言和OC并没有真正的Bool类型
C语言的Bool类型非0即真
OC语言的Bool类型是typedef signed char BOOL;
Swift引入了真正的Bool类型
Bool true false
*/

let isOpen = true;
//let isOpen = 1;
// Swift中if的条件只能是一个Bool的值或者是返回值是Bool类型的表达式(==/!=/>/<等等)
// OC中if可以是任何整数(非0即真), 但是存在的问题是可能将判断写错, 写成了赋值 if(isOpen = 2) , 在开发中为了避免这个问题有经验的程序员会这样写 if(2 == isOpen)来避免这个问题. 在Swift中很好的解决了这个问题
if isOpen
{
    print("打开")
}else
{
    print("关闭")
}
```

```html
//
//  main.swift
//  类型转换
//
//  Created by 李南江 on 15/2/28.
//  Copyright (c) 2015年 520it. All rights reserved.
//

import Foundation

/*
Swift不允许隐式类型转换, 但可以使用显示类型转换(强制类型转换)
OC:
int intValue = 10;
double doubleValue = (double)intValue;

Swift:
*/
var intValue:Int = 10
var doubleValue:Double
doubleValue = Double(intValue)
// 注意:Double()并不会修改intValue的值, 而是通过intValue的值生成一个临时的值赋值给doubleValue
print(intValue)
print(doubleValue)



```

```html
//
//  main.swift
//  结构体
//
//  Created by 李南江 on 15/4/3.
//  Copyright (c) 2015年 520it. All rights reserved.
//

import Foundation
/*
结构体:
结构体是用于封装不同或相同类型的数据的, Swift中的结构体是一类类型, 可以定义属性和方法(甚至构造方法和析构方法等)
格式:
struct 结构体名称 {
    结构体属性和方法
}
*/


struct Rect {
    var width:Double = 0.0
    var height:Double = 0.0
}
// 如果结构体的属性有默认值, 可以直接使用()构造一个结构体
// 如果结构体的属性没有默认值, 必须使用逐一构造器实例化结构体
var r = Rect()
print("width = \(r.width) height = \(r.height)")
// 结构体属性的访问使用.语法
r.width = 100
r.height = 99
print("width = \(r.width) height = \(r.height)")


/*
结构体构造器
Swift中的结构体和类跟其它面向对象语言一样都有构造函数, 而OC是没有的
Swift要求实例化一个结构体或类的时候,所有的成员变量都必须有初始值, 构造函数的意义就是用于初始化所有成员变量的, 而不是分配内存, 分配内存是系统帮我们做的.
如果结构体中的所有属性都有默认值, 可以调用()构造一个结构体实例
如果结构体中的属性没有默认值, 可以自定义构造器, 并在构造器中给所有的属性赋值
其实结构体有一个默认的逐一构造器, 用于在初始化时给所有属性赋值
*/

struct Rect2 {
    var width:Double
    var height:Double = 0.0
}
// 逐一构造器
var r1 = Rect2(width: 10.0, height: 10.0);
// 错误写法, 顺序必须和结构体中成员的顺序一致
//var r1 = Rect2(height: 10.0, width: 10.0);
// 错误写法, 必须包含所有成员
//var r1 = Rect2(height: 10.0);


/*
结构体中定义成员方法
在C和OC中结构体只有属性, 而Swift中结构体中还可以定义方法
*/

struct Rect3 {
    var width:Double
    var height:Double = 0.0
    // 给结构体定义一个方法, 该方法属于该结构体
    // 结构体中的成员方法必须使用某个实例调用
    // 成员方法可以访问成员属性
    func getWidth() -> Double{
        return width
    }
}

var r2 = Rect3(width: 10.0, height: 20.0)
// 结构体中的成员方法是和某个实例对象绑定在一起的, 所以谁调用, 方法中访问的属性就属于谁
print(r2.getWidth())

var r3 = Rect3(width: 30.0, height: 20.0)
// 取得r2这个对象的宽度
print(r3.getWidth())

/*
结构体是值类型
*/

struct Rect4 {
    var width:Double
    var height:Double = 0.0
    func show() -> Void{
        print("width = \(width) height = \(height)")
    }
}

var r4 = Rect4(width: 10.0, height: 10.0)
var r5 = r4

/*
赋值有两种情况
1.指向同一块存储空间
2.两个不同实例, 但内容相同
*/
r4.show()
r5.show()
r4.width = 20.0
// 结构体是值类型, 结构体之间的赋值其实是将r1中的值完全拷贝一份到r2中, 所以他们是两个不同的实例
r4.show()
r5.show()

```
```
//
//  main.swift
//  下表subscripts
//
//  Created by 李南江 on 15/4/4.
//  Copyright (c) 2015年 520it. All rights reserved.
//

import Foundation
/*
subscripts(下标): 访问对象中数据的快捷方式
所谓下标脚本语法就是能够通过, 实例[索引值]来访问实例中的数据
类似于以前我们访问数字和字典, 其实Swift中的数组和字典就是一个结构体

Array:      subscript (index: Int) -> T
Dictionary: subscript (key: Key) -> Value?
arr[0] == arr.subscript(0)
dict["key"] == dict.subscript("key")
*/

struct Student {
    var name:String = "lnj"
    var math:Double = 99.0
    var chinese:Double = 99.0
    var english:Double = 99.0
    
    func score(course:String) ->Double?
    {
        switch course{
            case "math":
                return math
            case "chinese":
                return chinese
            case "english":
                return english
            default:
                return nil
        }
    }
    // 要想实现下标访问, 必须实现subscript方法
    // 如果想要通过下标访问, 必须实现get方法
    // 如果想要通过下表赋值, 必须实现set方法
    subscript(course:String) ->Double?{
        get{
            switch course{
            case "math":
                return math
            case "chinese":
                return chinese
            case "english":
                return english
            default:
                return nil
            }
        }
        set{
            switch course{
            case "math":
                // 因为返回的是可选类型
                math = newValue!
            case "chinese":
                chinese = newValue!
            case "english":
                english = newValue!
            default:
                print("not found")
            }

        }
    }
}
var stu = Student(name: "zs", math: 99.0, chinese: 88.0, english: 10.0)
print(stu.score("math"))
stu["chinese"] = 100.0
print(stu["chinese"])


/*
Swift中是允许多索引的下标的
*/
struct Mul {
    subscript(a:Int, b:Int) -> Int
    {
        return a * b
    }
}
var m = Mul()
print(m[3, 5])



```
