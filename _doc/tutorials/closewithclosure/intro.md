---
title: 介绍
sections:
- 序言
- 导入
- 匿名函数
---

![](/assets/images/tutorials/closure.jpg){: .img-responsive}

## 序言

该教程适用于中等水平,熟悉编程的基本常识,并想进一步扩展自己的知识与能力的开发者.

## 导入

在开发地图的过程中遇到某些情景,你可能不太想要受限于类型`code`,以实现提供自定义的事件监听器,延迟代码的执行,在选取单位时使用一个匹配函数，或者只是简单的,把函数当做对象一样存储在变量中之类的功能.

在Jass中,把函数(类型`code`)存储在变量中也是可能的,但终究没法大规模的对他们进行操作,究其原因还是因为jass并不允许`code array`,并且你也没法将`code`放入哈希表中。

在Jass中的确是存在着一些解决方案去调用和储存函数,这通常需要开发者的一些小技巧,这些技巧通常依赖于`boolexpr`和`trigger`,或者不安全的`ExecuteFunc(string name)`来制作伪函数变量。

而这些缺陷则都被闭包解决了.

## 匿名函数

但在我们接触闭包之前,为了防止出现概念的混淆,我们先来区分一些东西.

lambda表达式能够允许你定义匿名函数,闭包则与它结合使用.

但什么是匿名函数呢?很简单,没有名字去引用它的函数就是匿名函数.看一下以下例子.

```wurst
function foo()
    print("Hello")

init
    CreateTrigger().addAction(function foo)
```
我们正在给`addAction`传递一个函数名作为引用,自然这个函数(作为参数的那一个)是有名字的.或者说,这个函数是非匿名的.

那现在让我们用lambda表达式来实现一个匿名函数的版本.

```wurst
init
    CreateTrigger().addAction(() -> print("Hello"))
```


新的部分在这儿， `() -> print("Hello")`
正如你看到的,这就是跟在函数名后面的部分

```wurst
/*function foo*/()
    print("Hello")
```
通过添加箭头`->` 来表明方向和函数体的开始.
这在有参数的情况下也适用.

```wurst
function foo(int i1, int i2)
    print("Hello " + i1.toString + i2.toString())

// 可以写成
(i1, i2) -> print("Hello " + i1.toString + i2.toString())
```
但是这样的情况不能作用于类似`addAction`的函数,毕竟在jass中你无法去唤出(invoke)类型为`code`的且带参数的函数.你将不得不用全局函数`GetXXX`来获取事件相关的或者传递过来的数据.

因此,我们需要闭包.
