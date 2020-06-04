                        

# Swift中类的使用

### 主要内容

*   类的介绍和定义
*   类的属性
*   类的构造函数

### 一. 类的介绍和定义

*   Swift也是一门面向对象开发的语言
*   面向对象的基础是类,类产生了对象
*   在Swift中如何定义类呢?

        *   class是Swift中的关键字,用于定义类

    <span class="hljs-class"><span class="hljs-keyword">class</span> 类名 : <span class="hljs-title">SuperClass</span> </span>{
        <span class="hljs-comment">// 定义属性和方法</span>
    }
    `</pre>

*   注意:

        *   定义的类,可以没有父类.那么该类是rootClass
    *   通常情况下,定义类时.继承自NSObject(非OC的NSObject)

    ### 二. 如何定义类的属性

    ##### 类的属性介绍

*   Swift中类的属性有多种

        *   存储属性:存储实例的常量和变量
    *   计算属性:通过某种方式计算出来的属性
    *   类属性:与整个类自身相关的属性

    ##### 存储属性

*   存储属性是最简单的属性，它作为类实例的一部分，用于存储常量和变量
*   可以给存储属性提供一个默认值，也可以在初始化方法中对其进行初始化
*   下面是存储属性的写法

        *   age和name都是存储属性,用来记录该学生的年龄和姓名
    *   chineseScore和mathScore也是存储属性,用来记录该学生的语文分数和数学分数
    <pre>`<span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Student</span> : <span class="hljs-title">NSObject</span> </span>{
        <span class="hljs-comment">// 定义属性</span>
        <span class="hljs-comment">// 存储属性</span>
        <span class="hljs-keyword">var</span> age : <span class="hljs-type">Int</span> = <span class="hljs-number">0</span>
        <span class="hljs-keyword">var</span> name : <span class="hljs-type">String</span>?

        <span class="hljs-keyword">var</span> chineseScore : <span class="hljs-type">Double</span> = <span class="hljs-number">0.0</span>
        <span class="hljs-keyword">var</span> mathScore : <span class="hljs-type">Double</span> = <span class="hljs-number">0.0</span>
    }

    <span class="hljs-comment">// 创建学生对象</span>
    <span class="hljs-keyword">let</span> stu = <span class="hljs-type">Student</span>()

    <span class="hljs-comment">// 给存储属性赋值</span>
    stu.age = <span class="hljs-number">10</span>
    stu.name = <span class="hljs-string">"why"</span>

    stu.chineseScore = <span class="hljs-number">89.0</span>
    stu.mathScore = <span class="hljs-number">98.0</span>
    `</pre>

    ##### 计算属性

*   计算属性并不存储实际的值，而是提供一个getter和一个可选的setter来间接获取和设置其它属性
*   计算属性`一般`只提供getter方法
*   如果只提供getter，而不提供setter，则该计算属性为只读属性,并且可以省略get{}
*   下面是计算属性的写法

        *   averageScore是计算属性,通过chineseScore和mathScore计算而来的属性
    *   在setter方法中有一个newValue变量,是系统指定分配的
    <pre>`<span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Student</span> : <span class="hljs-title">NSObject</span> </span>{
        <span class="hljs-comment">// 定义属性</span>
        <span class="hljs-comment">// 存储属性</span>
        <span class="hljs-keyword">var</span> age : <span class="hljs-type">Int</span> = <span class="hljs-number">0</span>
        <span class="hljs-keyword">var</span> name : <span class="hljs-type">String</span>?

        <span class="hljs-keyword">var</span> chineseScore : <span class="hljs-type">Double</span> = <span class="hljs-number">0.0</span>
        <span class="hljs-keyword">var</span> mathScore : <span class="hljs-type">Double</span> = <span class="hljs-number">0.0</span>

        <span class="hljs-comment">// 计算属性</span>
        <span class="hljs-keyword">var</span> averageScore : <span class="hljs-type">Double</span> {
            <span class="hljs-keyword">get</span> {
                <span class="hljs-keyword">return</span> (chineseScore + mathScore) / <span class="hljs-number">2</span>
            }

            <span class="hljs-comment">// 没有意义,因为之后获取值时依然是计算得到的</span>
            <span class="hljs-comment">// newValue是系统分配的变量名,内部存储着新值</span>
            <span class="hljs-keyword">set</span> {
                <span class="hljs-keyword">self</span>.averageScore = newValue
            }
        }
    }

    <span class="hljs-comment">// 获取计算属性的值</span>
    <span class="hljs-built_in">print</span>(stu.averageScore)
    `</pre>

    ##### 类属性

*   类属性是与类相关联的，而不是与类的实例相关联
*   所有的类和实例都共有一份类属性.因此在某一处修改之后,该类属性就会被修改
*   类属性的设置和修改,需要通过类来完成
*   下面是类属性的写法

*   类属性使用static来修饰
*   courseCount是类属性,用来记录学生有多少门课程


##### 监听属性的改变

*   在OC中我们可以重写set方法来监听属性的改变
*   Swift中可以通过属性观察者来监听和响应属性值的变化
*   通常是监听存储属性和类属性的改变.(对于计算属性，我们不需要定义属性观察者，因为我们可以在计算属性的setter中直接观察并响应这种值的变化)
*   我们通过设置以下观察方法来定义观察者

        *   willSet：在属性值被存储之前设置。此时新属性值作为一个常量参数被传入。该参数名默认为newValue，我们可以自己定义该参数名
    *   didSet：在新属性值被存储后立即调用。与willSet相同，此时传入的是属性的旧值，默认参数名为oldValue
    *   willSet与didSet只有在属性第一次被设置时才会调用，在初始化时，不会去调用这些监听方法
*   监听的方式如下:

        *   监听age和name的变化
  
```html
//
//  main.swift
//  方法
//
//  Created by 李南江 on 15/4/4.
//  Copyright (c) 2015年 520it. All rights reserved.
//

import Foundation

/*
隶属于每一个类或结构体的函数称之为方法:
方法分为类方法和实例方法, 对应OC中的+ - 方法
实例方法:实例方法一定是通过对象来调用的, 实例方法隶属于某一个类
*/

class Person {
    var _name:String = "lnj"
    var _age:Int = 30
    // 实例方法一定是通过对象来调用的, 实例方法隶属于某一个类
//    func setName(name:String, age:Int)
    // 如果不希望某个参数作为外部参数, 可以在参数前面加上_, 忽略外部参数
    func setName(name:String, _ age:Int)
    {
        _name = name
        _age = age
    }
    func show()
    {
        print("name = \(_name) age = \(_age)")
    }
}

var p = Person()
// 由于第一个参数可以通过方法名称指定, 所以默认第一个参数不作为外部参数
//p.setName("zs", age: 88)
p.setName("zs", 88)

//func setName(name:String, age:Int){
//func setName(name:String,myAge age:Int){
func setName(name:String, age:Int){

}
// 实例方法和函数的区别在于, 实例方法会自动将除第一个参数以外的其它参数即当做外部参数又当做内部参数, 而函数需要我们自己指定才会有外部参数, 默认没有
setName("ls", age:55)


/*
self关键字,  Swift中的self和OC中的self基本一样. self指当前对象, 如果self在对象方法中代表当前对象. 但是在类方法中没有self
*/

class Person2 {
    var name:String = "lnj"
    var age:Int = 30
    // 当参数名称和属性名称一模一样时, 无法区分哪个是参数哪个是属性, 这个时候可以通过self明确的来区分参数和属性
    func setName(name:String, age:Int)
    {
        // 默认情况下, _name和_age前面有一个默认的self关键字, 因为所有变量都需要先定义再使用, 而setName方法中并没有定义过_name和_age, 而是在属性中定义的, 所以setName中访问的其实是属性, 编译器默认帮我们在前面加了一个self.
//        _name = name
//        _age = age
        self.name = name
        self.age = age
    }
    func show()
    {
        print("name = \(name) age = \(age)")
    }
}


/*
mutating方法
值类型(结构体和枚举)默认方法是不可以修改属性的, 如果需要修改属性, 需要在方法前加上mutating关键字, 让该方法变为一个改变方法
*/

struct Person3 {
    var name:String = "lnj"
    var age:Int = 30
    // 值类型(结构体和枚举)默认方法是不可以修改属性的, 如果需要修改属性, 需要在方法前加上mutating关键字, 让该方法变为一个改变方法
    // 注意: 类不需要, 因为类的实例方法默认就可以修改
    mutating func setName(name:String, age:Int)
    {
        self.name = name
        self.age = age
    }
    func show()
    {
        print("name = \(name) age = \(age)")
    }
}
var p3 = Person3()
p3.setName("zs", age: 99)
p3.show()

enum LightSwitch{
    case OFF, ON
    mutating func next()
    {
        switch self{
            case OFF:
                self = ON
            case ON:
                self = OFF
        }
    }
}
var ls:LightSwitch = LightSwitch.OFF
if ls == LightSwitch.OFF
{
    print("off")
}
ls.next()
if ls == LightSwitch.ON
{
    print("on")
}

/*
类方法:
和类属性一样通过类名来调用, 类方法通过static关键字(结构体/枚举), class(类)
类方法中不存在self
*/

struct Person4 {
    var name:String = "lnj"
    static var card: String = "123456"
    func show()
    {
        print("name = \(self.name) card = \(Person4.card)")
    }
    static func staticShow()
    {
        // 类方法中没有self
//        print("name = \(self.name) card = \(Person.card)")
        // 静态方法对应OC中的+号方法, 和OC一样在类方法中不能访问非静态属性
        print("card = \(Person4.card)")
    }
}
var p4 = Person4()
p4.show()
Person4.staticShow()


class Person5 {
    var name:String = "lnj"
    class var card: String {
        return "123456"
    }
    func show()
    {
        print("name = \(self.name) card = \(Person5.card)")
    }
    class func staticShow()
    {
        // 类方法中没有self
        //        print("name = \(self.name) card = \(Person.card)")
        // 静态方法对应OC中的+号方法, 和OC一样在类方法中不能访问非静态属性
        print("card = \(Person5.card)")
    }
}
var p5 = Person5()
p5.show()
Person5.staticShow()

```
```
//
//  main.swift
//  类
//
//  Created by 李南江 on 15/4/4.
//  Copyright (c) 2015年 520it. All rights reserved.
//

import Foundation

/*
类的基本定义
Swift中的结构体和类非常相似, 但是又有不同之处
类是具有相同属性和方法的抽象
格式:
class 类名称 {
    类的属性和方法
}
*/
class Rect {
    var width:Double = 0.0
    var height:Double = 0.0
    func show() -> Void{
        print("width = \(width) height = \(height)")
    }
}
// 类没有逐一构造器
//var r1 = Rect(width: 10.0, height: 10.0)
var r1 = Rect()
r1.show()
var r2 = r1
r2.show()
// 类是引用类型, 结构体之间的赋值其实是将r2指向了r1的存储控件, 所以他们是两个只想同一块存储空间, 修改其中一个会影响到另外一个
r1.width = 99
r1.show()
r2.show()


/*
恒等运算符, 用于判断是否是同一个实例, 也就是是否指向同一块存储空间
=== !==
*/
var r3 = Rect()
if r1 === r3
{
    print("指向同一块存储空间")
}

```

```html
//
//  main.swift
//  析构方法
//
//  Created by 李南江 on 15/4/5.
//  Copyright (c) 2015年 520it. All rights reserved.
//

import Foundation

/*
析构方法
对象的内存被回收前夕被隐式调用的方法, 对应OC的dealloc方法
主要执行一些额外操作, 例如释放一些持有资源, 关闭文件, 断开网络等
*/

class FileHandler{
    var fd: Int32? // 文件描述符
    // 指定构造器
    init(path:String){
        // 需要打开的文件路径, 打开方式(只读)
        // open方法是UNIX的方法
        let ret = open(path, O_RDONLY)
        if ret == -1{
            fd = nil
        }else{
            fd = ret
        }
        print("对象被创建")
    }
    // 析构方法
    deinit{
        // 关闭文件
        if let ofd = fd{
             close(ofd)
        }
        print("对象被销毁")
    }
}

var fh:FileHandler? = FileHandler(path: "/Users/Jonathan_Lee/Desktop/老员工奖.xlsx")
// 当对象没有任何强引用时会被销毁
fh = nil



/*
析构方法的自动继承
父类的析构方法会被自动调用, 不需要子类管理
*/
class Person {
    var name:String
    init(name:String){
        self.name = name
        print("Person init")
    }
    deinit{
        print("Person deinit")
    }
}

class SuperMan: Person {
    var age:Int
    init(age:Int){
        self.age = age
        super.init(name: "lnj")
        print("SuperMan init")
    }
    deinit{
        // 如果父类的析构方法不会被自动调用,那么我们还需要关心父类
        // 但是如果这样做对子类是比较痛苦的
        print("SuperMan deinit")
    }
}
var sm: SuperMan? = SuperMan(age: 30)
sm = nil

```


```
//
//  main.swift
//  内外部函数
//
//  Created by 李南江 on 15/3/17.
//  Copyright (c) 2015年 520it. All rights reserved.
//

import Foundation

/*
内部函数: 默认情况下的参数都是内部参数
外部函数: 如果有多个参数的情况, 调用者并不知道每个参数的含义, 只能通过查看头文件的形式理解参数的含义
        那么能不能和OC一样让调用者直观的知道参数的含义呢? 使用外部参数
        外部参数只能外部用, 函数内部不能使用, 函数内部只能使用内部参数
*/
func divisionOpertaion1(a: Double, b:Double) -> Double{
    return a / b
}

func divisionOpertaion2(dividend: Double, divisor:Double) -> Double{
    return dividend / divisor
}

func divisionOpertaion3(dividend a: Double, divisor b:Double) -> Double{
//    return dividend / divisor
    return a / b
}
print(divisionOpertaion3(dividend: 10, divisor: 3.5))

func divisionOpertaion4(a: Double, divisor b:Double) -> Double{
    return a / b
}
print(divisionOpertaion4(10, divisor: 3.5))

/*
// Swift2.0过时
// 在参数前面加上#相当于该参数即是内部参数, 也是外部参数
// 等价于dividend dividend: Double, divisor divisor:Double
func divisionOpertaion5(#dividend: Double, #divisor:Double) -> Double{
    return dividend / divisor
}
print(divisionOpertaion5(dividend: 10, divisor: 3.5))
*/
// 取而代之第二个参数开始默认既是外部又是内部
func divisionOpertaion5(dividend: Double, divisor:Double) -> Double{
    return dividend / divisor
}
print(divisionOpertaion5(10, divisor: 3.5))


/*
默认参数:
可以在定义函数的时候给某个参数赋值, 当外部调用没有传递该参数时会自动使用默认值
*/

func joinString(s1:String ,toString s2:String, jioner s3:String) ->String
{
    return s1 + s3 + s2;
}

func joinString2(s1:String ,toString s2:String, jioner s3:String = "❤️") ->String
{
    return s1 + s3 + s2;
}
print(joinString2("hi", toString:"beauty"))

//如果指定了默认参数, 但是确没有声明外部参数时, 系统会自动把内部参数名称既作为内部参数也作为外部参数名称, 并且在调用时如果需要修改默认参数的值必须写上外部参数名称
func joinString3(s1:String ,toString s2:String, jioner:String = "❤️") ->String
{
    return s1 + jioner + s2;
}
print(joinString3("hi", toString:"beauty", jioner:"🐔"))

//在其它语言中默认参数智能出现在参数列表的最后面, 但是在Swift中可以出现在任何位置
func joinString4(s1:String , jioner:String = "❤️",toString s2:String) ->String
{
    return s1 + jioner + s2;
}
print(joinString4("hi", jioner:"🐔", toString:"beauty"))

/*
常量参数和遍历参数:
默认情况下Swift中所有函数的参数都是常量参数, 如果想在函数中修改参数, 必须在参数前加上var
OC: (OC中函数的参数是便利参数)
- (void)swap:(int)a b:(int)b
{
    NSLog(@"交换前:%d %d", a, b);
    int temp = a;
    a = b;
    b = temp;
    NSLog(@"交换后:%d %d", a, b);
}
*/

func swap(var a:Int, var b:Int)
{
    print("交换前 a = \(a) b = \(b)")
    let temp = a;
    a = b;
    b = temp;
    print("交换后 a = \(a) b = \(b)")
}

swap(10, b: 20)

/*
inout参数, 如果想在函数中修改外界传入的参数, 可以将参数的var换成inout, 这回会传递参数本身而不是参数的值
OC:
- (void)swap:(int *)a b:(int *)b
{
    int temp = *a;
    *a = *b;
    *b = temp;

}

Swift:
// 在以前的语法中是不能传递指针的, 必须把参数的var换成inout才可以

func swap2(var a:Int, var b:Int)
{
    let temp = a;
    a = b;
    b = temp;
}
var x = 10;
var y = 20;
print("交换前 a = \(x) b = \(y)")
swap2(&x, b: &y) // 会报错
print("交换后 a = \(x) b = \(y)")
*/

func swap3(inout a:Int, inout b:Int)
{
    let temp = a;
    a = b;
    b = temp;
}
var x1 = 10;
var y1 = 20;
print("交换前 a = \(x1) b = \(y1)")
swap3(&x1, b: &y1)
print("交换后 a = \(x1) b = \(y1)")


/*
变参函数
如果没有变参函数 , 并且函数的参数个数又不确定那么只能写多个方法或者用将函数参数改为集合
变参只能放到参数列表的最后一位, 变参必须指定数据类型, 变参只能是同种类型的数据
*/
func add(num1:Int, num2:Int, num3:Int) -> Int
{
    let sum = num1 + num2 + num3
    return sum
}
print(add(1, num2: 2, num3: 3))

func add(nums:[Int]) -> Int
{
    var sum = 0;
    for num in nums
    {
        sum += num
    }
    return sum
}
print(add([1, 2, 3]))


func add(nums:Int...) -> Int
{
    var sum = 0;
    for num in nums
    {
        sum += num
    }
    return sum
}
print(add(1, 2, 3))


func add(other:Int, nums:Int...) -> Int
{
    var sum = 0;
    for num in nums
    {
        sum += num
    }
    return sum
}
print(add(99, 1, 2, 3))// 会将99传递给第一个参数, 后面的传递给nums

```

```
//
//  main.swift
//  继承
//
//  Created by 李南江 on 15/4/4.
//  Copyright (c) 2015年 520it. All rights reserved.
//

import Foundation

/*
继承语法
继承是面向对象最显著的一个特性, 继承是从已经有的类中派生出新的类
新的类能够继承已有类的属性和方法, 并能扩展新的能力
术语: 基类(父类, 超类), 派生类(子类, 继承类)
语法:
 class 子类: 父类{
}

继承有点: 代码重用
继承缺点: 增加程序耦合度, 父类改变会影响子类
注意:Swift和OC一样没有多继承
*/

class Man {
    var name:String = "lnj"
    var age: Int = 30
    func sleep(){
        print("睡觉")
    }
}

class SuperMan: Man {
    var power:Int = 100
    func fly(){
        // 子类可以继承父类的属性
        print("飞 \(name) \(age)")
    }
}
var m = Man()
m.sleep()
//m.fly() // 父类不可以使用子类的方法

var sm = SuperMan()
sm.sleep()// 子类可以继承父类的方法
sm.fly()


/*
super关键字:
 
派生类中可以通过super关键字来引用父类的属性和方法
*/

class Man2 {
    var name:String = "lnj"
    var age: Int = 30
    func sleep(){
        print("睡觉")
    }
}

class SuperMan2: Man2 {
    var power:Int = 100
    func eat()
    {
        print("吃饭")
    }
    func fly(){
        // 子类可以继承父类的属性
        print("飞 \(super.name) \(super.age)")
    }
    func eatAndSleep()
    {
        eat()
        super.sleep()
        // 如果没有写super, 那么会现在当前类中查找, 如果找不到再去父类中查找
        // 如果写了super, 会直接去父类中查找
    }
}
var sm2 = SuperMan2()
sm2.eatAndSleep()

/*
方法重写: override
重写父类方法, 必须加上override关键字
*/

class Man3 {
    var name:String = "lnj"
    var age: Int = 30
    func sleep(){
        print("睡觉")
    }
}

class SuperMan3: Man3 {
    var power:Int = 100
    // override关键字主要是为了明确表示重写父类方法, 
    // 所以如果要重写父类方法, 必须加上override关键字
    override func sleep() {
//        sleep() // 不能这样写, 会导致递归
        super.sleep()
        print("子类睡觉")
    }
    func eat()
    {
        print("吃饭")
    }
    func fly(){
        // 子类可以继承父类的属性
        print("飞 \(super.name) \(super.age)")
    }
    func eatAndSleep()
    {
        eat()
        sleep()
    }

}
var sm3 = SuperMan3()
// 通过子类调用, 优先调用子类重写的方法
//sm3.sleep()
sm3.eatAndSleep()


/*
重写属性
无论是存储属性还是计算属性, 都只能重写为计算属性
*/

class Man4 {
    var name:String = "lnj" // 存储属性
    var age: Int { // 计算属性
        get{
            return 30
        }
        set{
            print("man new age \(newValue)")
        }
    }
    func sleep(){
        print("睡觉")
    }
}
class SuperMan4: Man4 {
    var power:Int = 100
    // 可以将父类的存储属性重写为计算属性
    // 但不可以将父类的存储属性又重写为存储属性, 因为这样没有意义
//    override var name:String = "zs"
    override var name:String{
        get{
            return "zs"
        }
        set{
            print("SuperMan new name \(newValue)")
        }
    }
    // 可以将父类的计算属性重写为计算属性, 同样不能重写为存储属性
    override var age: Int { // 计算属性
        get{
            return 30
        }
        set{
            print("superMan new age \(newValue)")
        }
    }
}
let sm4 = SuperMan4()
// 通过子类对象来调用重写的属性或者方法, 肯定会调用子类中重写的版本
sm4.name = "xxx"
sm4.age = 50


/*
重写属性的限制
1.读写计算属性/存储属性, 是否可以重写为只读计算属性? (权限变小)不可以
2.只读计算属性, 是否可以在重写时变成读写计算属性? (权限变大)可以
3.只需
*/
class Man5 {
    var name:String = "lnj" // 存储属性
    var age: Int { // 计算属性
        get{
            return 30
        }
        set{
            print("man new age \(newValue)")
        }
    }
    func sleep(){
        print("睡觉")
    }
}
class SuperMan5: Man5 {
    var power:Int = 100
    override var name:String{
        get{
            return "zs"
        }
        set{
            print("SuperMan new name \(newValue)")
        }
    }
    override var age: Int { // 计算属性
        get{
            return 30
        }
        set{
            print("superMan new age \(newValue)")
        }
    }
}



/*
重写属性观察器
只能给非lazy属性的变量存储属性设定属性观察器, 
不能给计算属性设置属性观察器,给计算属性设置属性观察器没有意义
属性观察器限制:
    1.不能在子类中重写父类只读的存储属性
    2.不能给lazy的属性设置属性观察器
*/

class Man6 {
    var name: String = "lnj"
    var age: Int = 0 { // 存储属性
        willSet{
            print("super new \(newValue)")
        }
        didSet{
            print("super new \(oldValue)")
        }
    }
    var height:Double{
        get{
            print("super get")
            return 10.0
        }
        set{
            print("super set")
        }
    }
}
class SuperMan6: Man6 {
    // 可以在子类中重写父类的存储属性为属性观察器
    override var name: String {
        willSet{
            print("new \(newValue)")
        }
        didSet{
            print("old \(oldValue)")
        }
    }
    // 可以在子类中重写父类的属性观察器
    override var age: Int{
        willSet{
            print("child new \(newValue)")
        }
        didSet{
            print("child old \(oldValue)")
        }

    }
    // 可以在子类重写父类的计算属性为属性观察器
    override var height:Double{
        willSet{
            print("child height")
        }
        didSet{
            print("child height")
        }
    }
}

var m6 = SuperMan6()
//m6.age = 55
//print(m.age)
m6.height = 20.0


/*
利用final关键字防止重写
final关键字既可以修饰属性, 也可以修饰方法, 并且还可以修饰类
被final关键字修饰的属性和方法不能被重写
被final关键字修饰的类不能被继承
*/
final class Man7 {
    final var name: String = "lnj"
    final var age: Int = 0 { // 存储属性
        willSet{
            print("super new \(newValue)")
        }
        didSet{
            print("super new \(oldValue)")
        }
    }
    final var height:Double{
        get{
            print("super get")
            return 10.0
        }
        set{
            print("super set")
        }
    }
    final func eat(){
        print("吃饭")
    }
}

```
