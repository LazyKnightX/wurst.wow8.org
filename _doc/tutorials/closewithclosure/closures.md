---
title: 进击的闭包
sections:
- 闭包
- 数据的保留
- 在变量中保存闭包
- 别被惹毛了
---

## 闭包

闭包(closure)使用类来突破这些限制,并让编译器来自动的为你处理这些糟心的的工作.闭包是通过隐式创建的,这意味着你不会去调用`new`来实例化它们.要用闭包,你得先定义一个构建你的闭包对象的类.让我们来举个例子,实现一个有两个整型作为参数的回调函数.
通过实现一个包含单一抽象函数的接口,或者抽象类，我们就能做到这一点了.


```wurst
interface Calculator
    abstract function doCalc(int i1, int i2)
```
我们如期的使用这个类型,目前为止没有什么特别的地方.
让我们来快速测试一下.


```wurst
class IntGenerator
    let calculators = new LinkedList<Calculator>

    function doCalculations()
        let i1 = GetRandomInt(-100, 100)
        let i2 = GetRandomInt(-100, 100)
        print("i1: " + i1.toString() + " i2: " + i2.toString())
        for calculator in calculators
            print(calculator.doCalc(i1, i2).toString())

    function addCalculator(Calculator calculator)
        calculators.add(calculator)

@Test
function testCalculations()
    let gen = new IntGenerator()
    // gen.addCalculator()
    gen.doCalculations()
```
在这我们用了一个 `IntGenerator` 来接收`Calculator`对象,以比较两个随机数在不同的计算方法上的结果.
如果我们不用闭包,我们会需要声明一个类来实现`Calculator`接口,重写`doCalc`函数,然后传递一个通过`new`实例化的对象给 `addCalculator`

像这样:

```wurst
class MyCalculator implements Calculator
    override function doCalc(int i1, int i2)
        return i1 + (i1 * i2)

...
    gen.addCalculator(new MyCalculator())
```
这其实就是Wurst会悄悄替你做的事情.再一次,函数名后面的部分转换为lambda表达式:

```wurst
    let gen = new IntGenerator()
    gen.addCalculator() (int i1, int i2) ->
        return i1 + (i1 * i2)
    gen.doCalculations()
```
你会注意到,这再也没有类`MyCalculator`了,但是Wurst会为你生成一个类似的.由于你没有名字去访问它,就行匿名函数一样,这是匿名的.某种程度上你可以把闭包当成匿名的类,它们由每一个对接口的实现来新建.顺带一提,由于`interface Calculator`中函数的参数类型已经给定了,我们可以在lambd中忽略这些参数类型.


```wurst
    let gen = new IntGenerator()
    gen.addCalculator() (i1, i2) ->
        return i1 + (i1 * i2)
    gen.addCalculator() (i1, i2) ->
        return i1 + i2
    gen.doCalculations()
```
如果我们添加了两个计算器(Calculator),Wurst会为我们生成两个闭包.三个的话就会生成三个.
这便已经超越了`code`的可能性.不过额外的,我们还可以通过使用类来保留数据.


## 数据的保留

你通常会想着把外部的数据传递给匿名函数,一个比较常规的例子是延迟执行代码。
Often you want to pass data from the outside into the anonymous function.
A common example for this is the delayed execution of code.

比方说你想创建一个类似地狱火的技能,先表演一段彗星从天而降的动画.当然,伤害以及其他的潜在事件必须得发生在一段时间之后————在彗星的特效撞击地标之后.在vjass中你或许会使用类似于TimerUtils以及一个数据结构来做到这一点.


```wurst
struct DelayData
    unit caster
    int abilLvl
    real x
    real y

function doEffect() takes nothing returns nothing
    local timer tim = GetExpiredTimer()
    local DelayData data = (DelayData) tim.getData()
    // Do effect..
endfunction

function onSpell() takes nothing returns nothing
    local timer tim = getTimer()
    local DelayData data = DelayData.create()
    // Assign data..
    tim.setData((int)data)
    TimerStart(tim, 0.3, false, function doEffect)
endfunction
```
如果你有好几年的jass编程经验,这对于你来说也许已经司空见惯了,不过这段代码会产生一些问题.
首先是牵涉了开发者大量的手动工作,因此更容易犯错.其流程也是反直觉的自底向上.而Wurst可以替你做这些事情.

使用`CLosureTimers`,例子如下.


```wurst
function onSpell()
    let caster = GetSpellAbilityUnit()
    let lvl = caster.getAbilityLevel(SPELL_ID)
    let target = getSpellTargetPos()
    doAfter(0.3) ->
        caster.damageTarget(.., lvl * 100)
        flashEffect("xxx", target)
```
在这个场景的背后,wurst做了和你自己在vjass中做得一模一样的事情.创建一个类,实例化它,将它绑定在计时器上,只是这儿用了Wurst版本的`TimerUtils`.在wurst的包中可以看到这是如何实现的.
.
## 在变量中保存闭包
正如你在`IntGenerator`中看到的,闭包类型可以轻松的保存在变量,类和哈希表中.常见的使用情景是在周期性的计时器或者临时事件的回调中,你想要在它的回调函数中去销毁它.一个简单的例子是在10秒内单位死亡则召唤生物中:


```wurst
function spellEffect()
    let listener = EventListener.add(EVENT_UNIT_DEATH) ->
        print("Unit died, summoun spawn for caster")
        createUnit(...)

    doAfter(10.) ->
        // Buff over
        destroy listener
```
如你所见我们在一个变量中保存了`EventListener.add` 返回的监听器,以在10秒后销毁它.
As you can see we save the listener returned by `EventListener.add` in a variable to destroy it after 10 seconds.

记住闭包不会被自动的回收,除非底层系统替你做了.标准库会销毁计划中为临时性的闭包.
## 别被惹毛了

(不用触发器)

如在导入中提及的那一,闭包也是一个触发器的替代品,因为触发器在jass中也被用来提供自定义事件.
代替掉使用`addAction` 并提供回调函数的函数名作为参数的方法,我们定义一个闭包接口/类,并按需保存.

想一下在一张地图中,我们通过`Level`类表达大量的关卡.而我们想要注册一些在通关后的自定义的事件.

你可以复制粘贴这个包到你的地图中去,看一看打印输出.

```wurst
package Level

interface LevelFinishedListener
	function onFinish()

class Level
	private LevelFinishedListener listener

	function addFinishListener(LevelFinishedListener listener)
		this.listener = listener

	function finishLevel()
		print("Level finished")
		listener.onFinish()

init
	let lvl1 = new Level()
	..addFinishListener() ->
		print("You beat level 1 - prepare to die")
	let lvl1 = new Level()
	..addFinishListener() ->
		print("You beat level 2 - how unexpected")

	// To test the listener
	lvl1.finishLevel()
```

## 结语

我们希望该教程能帮助你更进一步理解这一高级话题.来看看[manual section](https://wurstlang.org/manual.html#lambdas-and-closures) 获取更多信息吧.

