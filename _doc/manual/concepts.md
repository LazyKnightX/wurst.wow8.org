---
title: 基本概念
sections:
- 表达式
- 级联操作符
- 语句
- 包
---

## 表达式

 Wurst 表达式的一些基本语法:
```wurst
Expr ::=
      Expr + Expr       // 加法
    | Expr - Expr       // 减法
    | Expr / Expr       //实数 除法
    | Expr div Expr     // 整数 除法
    | Expr % Expr       // 实数 取余(取模)
    | Expr mod Expr     // 整数 取余(取模)
    | Expr and Expr     //布尔值 和
    | Expr or Expr      // 布尔值 或
    | Expr ? Expr : Expr // 条件表达式  / 三元操作符
    | Expr < Expr       //小于
    | Expr <= Expr      //小于等于
    | Expr > Expr       // 大于
    | Expr >= Expr      // 大于等于
    | Expr == Expr      // 等于
    | Expr != Expr      // 不等于
    | - Expr            // 一元表达式 (负数)
    | not Expr          // 否定表达式
    | IDENTIFIER                    // 变量
    | IDENTIFIER(Expr, Expr, ...)   // 函数调用
    | Expr . IDENTIFIER             //成员变量
    | Expr . IDENTIFIER(Expr, Expr, ...)    // 成员函数
    | Expr .. IDENTIFIER(Expr, Expr, ...)   // 成员函数,和上面一样 但是会返回当前对象(见下文)

    | new IDENTIFIER(Expr, Expr, ...)       // 构造函数调用
    | destroy Expr                  // 析构函数
    | Expr castTo Type              // 类型转换
    | Expr instanceof Type          // 实例类型检测
    | begin                         // 多行 lambda 表达式
        Statements
        end // 多行 lambda 表达式
    | (param1, param2, ...) -> Expr  //匿名函数
    | (Expr)                        // 括号
```


 _IDENTIFIER_ is 只一个函数或者一个变量的名字.他可以以字母数字和下划线开头
 (译著:函数以下划线开头或结尾可能产生能编译但是无法运行的情况)

**提示**: 关于泛型函数的调用,可以查看泛型章节


### 级联操作符 (点点操作符)

级联操作符 `..`类似于普通的`.` 可以用来调用函数, 但是他不会返回函数的结果而是返回调用者的对象 . 这使得可以轻松的对同一个变量进行链式的调用.
来看看这个例子:
```wurst
CreateTrigger()
    ..registerAnyUnitEvent(EVENT_PLAYER_UNIT_ISSUED_ORDER)
    ..addCondition(Condition(function cond))
    ..addAction(function action)
```
以上代码等效于:
```wurst
let temp = CreateTrigger()
temp.registerAnyUnitEvent(EVENT_PLAYER_UNIT_ISSUED_ORDER)
temp.addCondition(Condition(function cond))
temp.addAction(function action)
```

### 条件操作符

条件操作符 (也叫三元表达式) 具有这样的格式 `条件 ? A : B`.
首先条件会先执行.
如果条件成立了,为 `true`, 那么 `A` 表达式会执行并作为整个语句的结果.
否则  `B` 表达式会执行并作为结果.

这个表达式结果的类型为两个子表达式结果的共同类型, 也就是说表达式 `A` 和  `B` 应该具有同样的类型才能在一个三元表达式中使用.

条件表达式一般用的不多.
一般用于结果在两个备选结果中选择,如下:

```wurst
// 处理一个敌人和多个敌人的情况:
let enemyCount = n.toString() + " " + (n == 1 ? "enemy" : "enemies")
```

## 语句

Wurst 为 Jass 添加了一些现代化的语句 , 如 switch 和 for 循环

### If语句
if 语句用于在满足某些条件的时候才执行一些动作.
在**else** 块中你可以处理条件不满足的情况.
```wurst
// 简单的例子
if u.getHP() < 100
    print("unit s hurt") //if中的动作必须缩进

// if 和else
if x > y
    print("x 比 y 大")
else if x < y // 关闭if块 开启 else-if块
    print("x 比 y 小")
else // 处理剩余的情况
    print("x 等于 y")

//通过缩进来编写if语句

// 更多例子:

if caster.getOwner() == BOT_PLAYER
    print("caster owned by BOT_PLAYER")

if GetSpellAbilityId() == SPELL_ID
    flashEffect(getSpellTargetPos(), FX_PATH)

if x > 0 and x < 100 and y > 0 and y < 100
    print("inside box")
```
### Switch 语句
```wurst
// 是一个整数变量
switch i
    case 1
        print("1")
    case 3
        print("3")
    case 88
        print("88")
    default
        print("都不满足的情况")
```
switch用于处理if ,else-if 条件很多的场景.

###  循环
```wurst
while a > b // 条件循环
    ...

for i = 0 to 10 // for-循环 i每次+1
    ...

for i = 0 to 10 step 2 // for-循环 每次+2
    ...

for i = 10 downto 0 // 也可以每次减少
    ...

for u in someGroup // 循环单位组中的单位
    ...

for u from someGroup //循环单位组中的单位,并将单位移除
    ...

for i in myList // 用于标准库中的集合类型
    ...
```

### For 循环

 for-in 可以用于循环任何一个可迭代的对象.
 for-in 可以简单的转换为while循环:

```wurst
for A a in b
    Statements

// 等效于:
let iterator = b.iterator()
while iterator.hasNext()
    A a = iterator.next()
    Statements*
iterator.close()
```

**提示**  iterator.close() 会在while类任何一个返回语句之前被调用 .

如果你已经拥有了迭代器,或者想要访问其中的每一个函数,那么你可以使用for-from循环, 转换也十分简单
```wurst
let iterator = myList.iterator()
for segment from iterator
    //Statements
iterator.close()

// is equivalent to:
let iterator = myList.iterator()
while iterator.hasNext()
    segment = iterator.next()
    //Statements
iterator.close() 
```
**提示** 你需要手动的关闭迭代器 ( `iterator.close()`  ).

### Skip 

最基本的语句就是 _skip_ 语句了,他没有任何作用, 在某些很少的情况下才有用.

### 迭代器

您可以通过提供一组函数为Wurst提供所需类型的迭代器。通过提供迭代器，该类型可用于for循环:
-  function **hasNext()** returns boolean (如果还有下一个元素,就返回true)
-  function **next()** returns TYPE (返回下一个元素)

使用这两个函数你也会获取一个可以用于for-from循环的迭代器

如果你想使某个类型能用for-in循环,你需要提供

-  function **iterator()** returns Iterator

对于您的类型，它返回迭代的对象。
这可以很像一个句柄，就像在group-iterator中一样，或者像List-iterator这样的对象。
您的迭代器还应提供一个close函数，用于清除它分配的所有资源。
通常，迭代器只是在close函数中摧毁它自己。

可以看标准库中的两个例子:

**Group-Iterator**
```wurst
public function group.iterator() returns group
    // return a copy of the group:
    bj_groupAddGroupDest = CreateGroup()
    ForGroup(this, function GroupAddGroupEnum)
    return bj_groupAddGroupDest

public function group.hasNext() returns boolean
    return FirstOfGroup(this) != null

public function group.next() returns unit
    let u = FirstOfGroup(this)
    GroupRemoveUnit(this, u)
    return u

public function group.close()
    DestroyGroup(this)
```

如您所见，这个迭代器是一个单位组，因此该单位组需要提供上述函数。标准库的做法是通过扩展函数来完成的

**LinkedList-Iterator**
```wurst
public class LinkedList<T>
    ...

    // get an iterator for this list
    function iterator() returns LLIterator<T>
        return new LLIterator(dummy)

class LLIterator<Q>
    LLEntry<Q> dummy
    LLEntry<Q> current

    construct(LLEntry<Q> dummy)
        this.dummy = dummy
        this.current = dummy

    function hasNext() returns boolean
        return current.next != dummy

    function next() returns Q
        current = current.next
        return current.elem

    function close()
        destroy this
```

LinkedList Iterator有点不同，因为它是一个类。
它仍然提供所需的功能，因此可以作为迭代器使用。
它还包含一些帮助迭代的成员。
如果从LinkedList的.iterator（）函数返回LLIterator，则为新实例。

### 运算简写

WurstScript 支持这些运算简写:
```wurst
i++         // i = i + 1
i--         // i = i - 1
x += y      // x = x + y
x -= y      // x = x - y
x *= y      // x = x * y
x /= y      // x = x / y
```

因为这些简称只是简单地转化为它们的等价物，所以它们可以与重载运算符一起使用。

# 包

如上所述，用Wurst编写的每个代码段都必须在_package_中，包定义代码组织和单独的名称空间。

包也可以有全局变量 - 每个不在函数/类/模块的变量就是全局变量.被叫做包的全局性


当在vscode中创建一个`.wurst`文件时,包的顶部会自动的添加:

```wurst
package SomeVSCodePackage // 文件的名字,也是包的名字
...
```

## Imports 导入 / 依赖

包可以导入( 或者叫 依赖)另一个包,从而访问另一个包中的被定义为公开(public)的函数,变量.

```wurst
// 声明
package SomePackage

// 导入
package Blub
import SomePackage
import AnotherPackge //导入多个包
import public PackageX // 公开导入 (看下面)
```

import指令在项目的wurst文件夹中搜索包，并从wurst.dependencies文件中搜索所有依赖的项目。

### import public 公开导入

默认情况下，程序包不会传递自己导入的包, 如以下的代码是错误的:
```wurst
package A
public constant x = 5

package B
import A

package C
import B
constant z = x
```

变量x可用于包B，但不从B传递。因此，在包C中，我们不能使用变量x。
我们可以通过简单地将A导入C来解决这个问题，但有时您希望避免这些导入。
使用公开导入解决了这个问题，因为它们将传递导入的所有内容。因此，以下代码将起作用：

```wurst
package A
public constant x = 5

package B
import public A

package C
import B
constant z = x
```

### 特殊的wurst包

默认情况下，每个包都会导入一个名为Wurst.wurst的包，该包在标准库中定义。
这个包拥有一些经常被使用的包

如果您不想要此标准导入，可以通过导入包NoWurst来禁用它。
该指令主要用于解决标准库中的一些循环依赖关系


## 公开声明
默认情况下，程序包的所有成员都是私有的。如果要在导入该包的包中使用它们，则必须添加关键字“public”。

## 常量
您可以声明一个变量常量来禁止这个变量初始化后被更改。这对生成的代码没有影响，但在尝试编译时会抛出错误。


### 例子
```wurst
package First
int i // (private by default)只在这个包中可用
public int j // 公开的,导入这个类就可以使用

package Second
import First

int k = i // 错误
int m = j // 正确, 因为是公开的


package Foo
constant int myImportantNumber = 21364 // 常量必须初始化

function blub()
    myImportantNumber = 123 // Error

public constant int myPrivateNumber2 = 123 //这是错误的 包中的变量和常量需要定义在调用它们的函数的上面
```


## Init 初始化块

包的另一个功能是init块。
每个包都可以在其中的任何位置具有一个或多个init块。
包的init块内的所有操作都在地图载入中执行。

在init块的开头，您可以假设所有全局变量都在
当前包也已初始化。
```wurst
package MyPackage
init
    print("this is the initblock")
```

*提示:* 由于魔兽3具有字节码限制，因此主函数内的操作过多会使其无法完全执行。
为了避免这个问题，Wurst使用`TriggerEvaluate`在单独的`线程`中执行包init，以确定包是否已成功初始化。
在包内部过度计算时仍会出现问题，在这种情况下会显示警告。


## 初始化和延迟初始化

Wurst的初始化规则很简单：

1. 包内部初始化从上到下完成。
包的初始化程序是只所有全局变量静态初始化程序（包括静态类变量）和所有init块。
2. 如果包A导入包B并且导入不是'initlater`导入，
然后包A的初始化程序在B之前运行。
不允许使用不带“initlater”的循环导入。

通常，您应该通过移动受影响的代码来避免包之间的循环依赖关系。
但是，如果有必要，您可以手动通过将关键字`initlater`定义将使用的包稍后初始化：


```wurst
package A
import initlater B
import public initlater C
import D
```

这里只有'D`包保证在包'A`之前被初始化。
包'B`和`C`允许稍后初始化。

## 配置包

可以配置全局变量和函数。配置通过配置包完成。
每个包最多只有一个配置包，每个配置包只配置一个包。
包及其配置包之间的关系通过以下命名约定表示：
对于名为`Blub`的包，配置包必须命名为`Blub_config`。

在配置中，全局变量可以使用`@config`注释进行注解。
这样配置包中的变量可以覆盖原始包中具有相同名称的变量。
在原始包中，变量应该用`@configurable`注释，来标志这个变量是可以被配置覆盖的。

例子:
```wurst
package Example
@configurable int x = 5

package Example_config
@config int x = 42
```
配置功能基本相同：
```wurst
package Example
@configurable public function math(int x, int y) returns int
    return x + y


package Example_config
@config public function math(int x, int y) returns int
    return x*y
```
