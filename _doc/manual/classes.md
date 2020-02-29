---
title: 类
sections:
- 构造函数
- 'This'
- 析构函数
- 静态元素
- 继承
- 接口
- 泛型
---

类是一种简单、强大和非常有用的结构。一个类定义了数据和处理这些数据的相关函数。看看这个小例子：

```wurst
let dummyCaster = new Caster(200.0, 400.0)
dummyCaster.castFlameStrike(500.0, 30.0)
destroy dummyCaster
```

在这个例子中，我们在坐标x:200 y:400创建了一个名为“dummyCaster”的**施法马甲**单位

`译注:马甲单位指的是帮助模拟某些功能用的辅助单位。`

然后我们命令**施法马甲**向另一个位置施放**烈焰风暴**，最后我们摧毁了**施法马甲**这个类的实例。

这个例子向您展示了如何创建一个新对象（第1行）、调用对象上的方法（第2行）以及如何销毁对象（第3行）。
但是如何定义一个新的对象类型，比如“Caster”？这就是类的功能。类定义了一种新的对象。
类定义变量和方法，该类的每个实例都应该调用这些变量和方法。

`译注:在类中的非静态函数一般称为方法。`

类还可以定义如何构造新对象（构造函数）以及当它被破坏时应该发生什么（析构函数）。

可以像这样定义一个施法者会类

```wurst
class Caster // 定义一个'类'结构. "Caster" 是这个类的名字
    unit u // 类内部的变量一般叫做成员变量,可以定义在类的任何地方

    construct(real x, real y) // 构造函数
        u = createUnit(...)

    function castFlameStrike(real x, real y) //一个方法
        u.issueOrder(...)

    ondestroy //析构函数
        u.kill()
```

## 构造函数

WurstScript允许您为每个类定义自己的构造函数。构造函数是用来构造类的新实例的函数。

在通过**new**关键字创建类时调用构造函数，并在返回类实例之前对其执行操作。


```wurst
class Pair
    int a
    int b

    construct(int pA, int pB)
        a = pA
        b = pB

    function add() returns int
        return a + b


function test()
    let pair = new Pair(2,4) //构造函数
    let sum = p.add() //调用方法
    print(sum) // 打印6
```

在这个例子中，构造函数将两个整数a和b作为参数，并将类变量设置为它们。

只要参数不同，就可以定义多个构造函数。

```wurst
class Pair
    int a
    int b

    construct(int pA, int pB)
        a = pA
        b = pB

    construct(int pA, int pB, int pC)
        a = pA
        b = pB
        a += pC
        b += pC

function test()
    let p = new Pair(2, 4)
    let p2 = new Pair(3, 4, 5)
```


在本例中，类对有两个构造函数-一个取2，另一个取3个参数。

根据参数类型和参数数量，Wurst自动决定在使用“new”时使用哪个构造函数。

## This

关键字**this**是指调用函数的类的当前实例。这还允许我们将参数命名为与类变量相同的名称。

但是在类函数中可以忽略它，如例子所示。

```wurst
class Pair
    int a
    int b

    // 使用这个关键字来访问类的成员变量
    construct(int a, int b)
        this.a = a
        this.b = b
```

## 析构函数

每个类可以有一个ondestroy块。此块在实例被销毁之前执行。

Ondestroy块的定义如下

```wurst
class UnitWrapper
    unit u
    ...

    ondestroy
        u.remove()
```


## 静态元素

通过在声明前面写入关键字“static”，可以将变量和函数声明为“u static”。
静态变量和方法/函数属于类，而其他元素属于类的实例。
因此，您可以调用静态方法，而无需类的实例。

```wurst
class Terrain
    static var someVal = 12.

    static int array someArr

    static function getZ(real x, real y) returns real
        ...

function foo()
    let z = Terrain.getZ( 0, 0 ) // 使用 $类名$.$静态方法名字$()来访问静态方法
    let r = Terrain.someVal // 静态变量也是一样的 
    let s = Terrain.someArr[0]
```

`译注:非私有的静态变量认为是全局变量.和全局变量相比只是访问时需要使用 类名.静态变量名`

## 数组成员

香肠通过将大小数组转换为大小乘以数组，然后通过二叉树搜索在get/set函数中解析数组，从而支持将大小数组作为类成员。

`译注:这一点不用太在意,只需要知道成员数组变量需要标明数组大小即可(数组的大小不应过大)`

例子:

```wurst
class Rectangle
    Point array[4] points

    function getPoint(int index)
        return points[index]

    function setPoint(int index, Point nP)
        points[index] = nP
```

## 可见性规则

默认情况下，类元素在任何地方都可见。可以在变量或函数定义前面添加修饰符private(私有)或protected(受保护)来限制其可见性。

私有的元素只能从类中看到。在封闭包和子类中可以看到受保护的元素。

`译注:封闭包的概念极少用到,实际上或protected这个修饰符也比较少用`

## 继承

一个类可以扩展另一个类。

`译注:被继承的类称为这个类的父类. 比如A继承B,那么B是A的父类,A是B的子类`

这个类继承父类的所有非私有函数和变量并且可以在任何可以使用父类的地方使用。

类的构造函数必须指定如何构造父类。通过用**super()**来调用，

它定义了父类构造函数的参数。在此调用之前不能有任何语句。

如果父类的构造函数没有参数,可以省略这句话.

从父类继承的函数可以在子类中重写。这些函数必须用“override”关键字进行重写。

### 例子

```wurst
//弹幕
class Missile 
    construct(string fx, real x, real y)
        // ...
    //碰撞
    function onCollide(unit u)


// 定义一种特殊的弹幕 火球弹幕 它继承于弹幕
class FireballMissile extends Missile
    // we construct it using a fireball fx
    construct(real x, real y)
        //译注:调用父类的构造函数
        super("Abilities\\Weapons\\RedDragonBreath\\RedDragonMissile.mdl", x, y)
    // 火球弹幕可能会和普通弹幕有一些不同的作用,通过重写来实现
    override function onCollide(unit u)
        // 这里就可以写出火球弹幕的额外效果 普通弹幕的效果不再执行
```

`译注:可以在火球弹幕中用过super.onCollide(u)来执行普通弹幕的效果`

## 类型转换

为了进行类型转换，您可以使用 **castTo**关键字.

`译注:类型转换可以把一个类的实例转换为一个整数,也有一些其他用法。`

作用:
一种是保存类实例，例如将它们附加到计时器上，就像在TimerUtils.wurst中那样

这个过程也可以反转（从整数转换为类的实例）。


```wurst
class Test
    int val

init
    let t = new Test()
    let i = t castTo int
```

使用子类型时，类型转换有时很有用。如果你有一个类型A的实例a但是如果你知道对象的动态类型是B，则可以将对象强制转换为B以更改类型。

```wurst
class Shape

class Triangle extends Shape
    function calculate()
        ...

init
    A a = new B()
    // 我们知道a其实是B类型的,所以可以转换成B来执行B中定义的额外方法
    B b = a castTo B
    b.special()
```

提示: 你别瞎鸡儿转换,只有在你知道他是什么类型的时候才转换.在运行时不会检查强制类型转换，因此它们可能会出现可怕的错误.
使用高级库和面向对象编程可以避免强制转换.


## 多态(动态调度)

Wurst在访问类实例时具有多态功能。

这意味着很简单：如果你有一个B类的实例，它被转换成父类A调用具有该引用的函数时，将从依然执行B中的操作.

举个例子更容易理解：

### 例子 1

```wurst
class Recipe
    function printOut()
        print("I'm a recipe")

class SwordRecipe extends Recipe
    override function printOut()
        print("I'm a sword recipe")

init
    A a = new B()
    a.printOut()//  会输出"I'm B"
```

### 例子 2

```wurst
class A
    string name

    construct(string name)
        this.name = name

    function printName()
        print("实例 A 的名字是: " + name )


class B extends A

    construct(string name)
        super(name)

    override function printName()
        print("实例 B 的名字是: " + name )

init
    A a = new B("first")
    a.printName() // 会输出 "Instance of B named: first", 因为他真的是B
```

这在遍历同一父型的类实例时特别有用，这意味着您不必将实例强制转换为适当的子类型。

## 父类调用

当重写方法时，仍然可以使用**super**关键字调用原始实现。这很有用，因为

子类通常只是向其父类添加一些功能.

例子，设计一个火球技能，我们想为它创建一个更强大的版本:

```wurst
class FireBall
    ...
    function hitUnit(unit u, real damage)
        ...

class PowerFireBall extends FireBall
    ...
    // 这里我们重写hitUnit方法
    override function hitUnit(unit u, real damage)
        // 第一步,我们创建一个更大的火球特效
        createSomeBigExplosionEffect(u)
        // 然后我们造成2倍火球的伤害
        super.hitUnit(u, damage*2)
```

在定义自定义构造函数时，还应使用**super**关键字。必须在子类的构造函数中调用父类的构造函数。当没有给出父类构造函数调用时，编译器将尝试调用没有参数的构造函数。如果不存在这样的构造函数，则会出现如下错误：“对父类构造函数的调用不正确：缺少参数。”

```wurst
class Missile
    ...
    construct(vec2 position)
        ...

class TimedMissile extends Missile
    ...
    construct(vec2 position, real duration)
        // 这里调用父类的构造函数
        // 而且必须写在第一句
        super(position)
        ...
```

## instanceof

在Wurst中，您可以使用关键字**instanceof**检查一个类实例的类型。

*注意：您应该尽可能避免instanceof检查，而使用面向对象的编程。*

如果对象o是C类型的子类型，则instanceof表达式“o instanceof C”返回true。

不能将instanceof与来自不同类型分区的类型一起使用，因为instanceof
基于类型ID（见下一章）。这也是为什么不能使用instanceof检查整数的类型。
编译器将尝试拒绝instanceof表达式，该表达式将始终产生true或始终产生false。

### 例子1

```wurst
class A

class B extends A

init
    A a = new B()

    if a instanceof B
        print("It's a B")
```

## typeId(类型ID)

*提示*: typeId是一个实验特性。尽量避免使用它们。

有时需要检查对象的确切类型。为此，如果a是一个对象，可以写“a.typeId”；如果A是一个类，可以写“A.typeId”。

```wurst
// 检查a是否是A的实例
if a.typeId == A.typeId
    print("It's an A")
```

typeId是一个整数，对于类型分区内的每个类都是唯一的。

### 类型分区

A的类型分区是包含A的最小集合，对于所有类或接口T1和T2，它包含：

如果T1在集合中，并且T1是T2的子类型或父类型，则T2也在集合中。

或者用不同的方式表示：如果且仅当它们的类型层次结构以某种方式连接时，A和B在同一个分区中。


## 抽象类

抽象类是一个被声明为抽象的类，它可以包括抽象函数。不能创建抽象类的实例，

但是您可以为它创建非抽象的子类。

用关键字“abstract”声明抽象函数，并省略实现。

```wurst
abstract function onHit()
```

抽象类类似于接口，但它们可以拥有自己的、实现的函数和变量，就像普通类一样。

抽象类的优点是，您可以引用并调用抽象类中的抽象函数。

以碰撞响应为例。你的地图上有好几个单位，你希望每一个单位都有不同的表现。

可以通过集中更新函数或将他抽象交给子类去处理

像这样:

```wurst
abstract class CollidableObject

    abstract function onHit()

    function checkCollision(CollidableObject o)
        if this.inRange(o)
            onHit()
            o.onHit()

class Ball extends CollidableObject

    override function onHit()
        print("I'm a ball")

class Rect extends CollidableObject

    override function onHit()
        print("I'm a Rect")
```

因为CollidableObject需要它的子类来实现函数onHit；它可以在抽象类中调用，而不知道他的类型。

如果抽象类的子类没有实现所有抽象函数,那么它也必须抽象


# 接口

```wurst
interface Listener
    function onClick()

    function onAttack(real x, real y) returns boolean


class ExpertListener implements Listener
    function onClick()
        print("clicked")


    function onAttack(real x, real y) returns boolean
        flashEffect(EFFECT_STRING, x, y)
```

接口是一组具有空主体的相关方法。

如果一个类实现了某个接口，它必须实现接口中的所有空方法。

一个类可以实现多个接口，这意味着它同时满足更多的接口需求。

```wurst
class ExampleClass implements Interface1, Interface2, ...
    // 实现接口中所有的空方法
```

有了接口（如果是隐式的，还有模块），现在可以上下转换任何实现它的类。
这对于保存来自仅在1个集合列表或者数组中继承1个接口的类的所有实例特别有用

### Defender methods

接口可以具有带有实现的函数。当实现接口的类不提供方法本身的实现。
通常这是不需要的，但在某些情况下可能为了在不破坏接口实现类的情况下实现接口，必须这样做。

`译注:这一块可能是试验性的特性,我并没有使用过.为了避免翻译出现错误,以下是原文`

*An interface can have functions with an implementation. This implementation is used, when a class implementing the interface
does not provide an implementation of the method itself. Usually this is not needed but in some cases it might
be necessary in order to evolve an interface without breaking its implementing classes.*



## Generics

Generics make it possible to abstract from specific types and only program with placeholders
for types. This is especially useful for container types (e.g. Lists) where we do not want to code a
ListOfA, ListOfB, ListOfC for every class A, B, C which we need a list for.
Think of it as creating a general List with all it's functionality, but for an
unknown type, that gets defined when creating the instance.

With generics we can instead write only one implementation for lists and use it with all types.
Generic type parameter and arguments are
written in angled  bracket s  (<  an d >) after the identifier.


```wurst
// a generic interface for Sets with type parameter T
interface Set<T>
    // adds an element to the set
    function add(T t)

    // removes an element from the set
    function remove(T t)

    // returns the number of elements in the set
    function size() returns int

    // checks whether a certain element is in the set
    function contains(T t) returns boolean

class SetImpl<T> implements Set<T>
    // [...] implementation of the interface
```
If we have a class defined like this, we can then use it with a concrete type (e.g. Missile):
```wurst
// create a set of missiles
Set<Missile> missiles = new SetImpl<Missile>();
// add a missile m
missiles.add(m);
```
Generic parameters in Wurst can be bound to integers, class types and interface types directly.
In order to bind generic parameters to primitive-, handle- and tuple types you have to provide the functions
```wurst
function [TYPE]ToIndex([TYPE] t) returns int

function [TYPE]FromIndex(int index) returns [TYPE]
    return ...
```
The typecasting functions for primitive- and handle types are provided in _Typecasting.wurst_ using the fogstate bug.
```wurst
function unitToIndex(unit u) returns int
    return u.getHandleId()

function unitFromIndex(int index) returns unit
    data.saveFogState(0,ConvertFogState(index))
    return data.loadUnit(0)
```
### Generic Functions

Functions can use generic types. The type parameter is written after the name of the function.
In the following example the function *forall* tests if a predicate is true for all elements in a list.
The function has to be generic, because it has to work on all kinds of lists.
```wurst
function forall<T>(LinkedList<T> l, LinkedListPredicate<T> pred) returns boolean
    for x in l
        if not pred.isTrueFor(x)
            return false
    return true

// usage:
    LinkedList<int> l = ...
    // check if all elements in the list are even
    if forall<int>(l, (int x) -> x mod 2 == 0)
        print("true")
```
When calling a generic function, the type arguments can be omitted if they can be inferred
from the arguments to the function:
```wurst
...
if forall(l, (int x) -> x mod 2 == 0)
    ...
```
Extension functions can also be generic, as shown by the following example:
```wurst
function LinkedList<T>.forall<T>(LinkedListPredicate<T> pred) returns boolean
    for x in this
        if not pred.isTrueFor(x)
            return false
    return true

// usage:
    ...
    if l.forall((int x) -> x mod 2 == 0)
        ...
```

## Modules

A _module_ is a small package which provides some functionality for classes. Classes can _use_ modules to inherit the functionality of the module.

You can use the functions from the used module as if they were declared in the class. You can also _override_ functions defined in a module to adjust its behavior.

If you know object oriented languages like Java or C#: Modules are like abstract classes and using a module is like inheriting from an abstract class but *without the sub-typing*. Modules encapsulate behaviour that you might want to add to several classes, without overhead and hierarchical structure. 

### Example 1

In this example we just have a class which uses a module A. The resulting program behaves as if the code from module A would be pasted into Class C.

```wurst
module A
    public function foo()
        ...

class C
    use A
```

### Example 2

Modules are more than just a mechanism to copy code. Classes and modules can override functions defined in used modules:

```wurst
// a module to store an integer variable
module IntContainer
    int x

    public function getX() returns int
        return int

    public function setX(int x)
        this.x = x

// a container which only stores positive ints
module PositiveIntContainer
    use IntContainer

    // override the setter to only store positive integers
    override function setX(int x)
        if x >= 0
            IntContainer.setX(x)
```

### Visibility & Usage Rules

 * Variables of modules are always private
 * private functions are only usable from the module itself
 * each function of a module must be declared public or private
 * if a class uses a module it inherits only the public functions of the module
    * you can use a module with *private* (not implemented yet). This will let you use the functionality of the module without exposing its functions to the outside.


### Overriding Functions

 * You can *override* functions which are defined in used modules by writing the override keyword in front of a function.
 * If two modules are used, which have the same function, it *must* be overridden by the underlying class or module in order to avoid ambiguousness (of course this is only possible if the function signatures match. We are thinking about a solution for this)
 * private functions cannot be overridden

### Abstract Functions

Modules can declare abstract functions: Functions without a given implementation. Abstract functions have to be implemented by the underlying classes.

### Thistype

You can use _thistype_ inside a module to refer to the type of the class which uses the module. This can be useful if you need to cast the class to an integer and back.
