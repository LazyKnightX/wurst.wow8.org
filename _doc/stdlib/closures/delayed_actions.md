---
title: 闭包计时器
sections:
- 导入
- 延时动作
- 周期动作
---

### 导入

这个 `ClosureTimers.wurst`包,他使用Wurst闭包封装了一系列War3的计时器的机制.利用这些闭包,你可以跨域传递数据,或是将回调函数存储在某个变量中


> 尽管闭包计时器能够处理大多问题, 但在性能吃紧的情况下, TimedLoop模块能够表现出更好的结果.

### 延时动作

计时器最经常被用于在一定时间后执行某些事情,而无需停止其他代码的执行的情景中.你可以使用`doAfter()`函数来做到这一点.

一些例子是:

```wurst
// Destroy effect after 5 seconds
let eff = addEffect(Abilities.vengeanceMissile, ZERO2)
doAfter(5.) ->
	eff.destr()

// Inflict damage after time
let attacker = GetEventDamageSource()
let target = GetTriggerUnit()
doAfter(2.) ->
	attacker.damageTarget(target, 20)
```
和vJass的计时器数据绑定比较一下你会立刻注意到,这些闭包的数据绑定是自动.

在创建的时候,闭包会去捕获它内部的数据,在这个例子里.分别是`eff`和`attacker,target`.你可以在之后的回调中引用他们

在底层,闭包计时器也使用`TimerUtils`来实现数据绑定

### 周期动作

另一个典型的用例是周期的执行某些特定的动作,这个情况请使用`doPeriodically()` 或 `doPeriodicallyCounted`.

```wurst
doPeriodically(ANIMATION_PERIOD) cb ->
	if missiles.size() == 0
		destroy cb
	else
		for missile in missiles
			missile.update()

// 计时
doPeriodicallyCounted(1, 3) cb ->
	print(cb.getCount().toString() + "..")
```
你看,在这个变体,回调接收一个闭包对象作为参数.所以你可以销毁或者修改它.你也可以将这个回调保存在变量中,然后比方说,在相关联的单位死亡后销毁它.

```wurst
class MyClass
	CallbackPeriodic cb

	construct()
		cb = doPeriodically(0.5, () -> checkAura())

	function checkAura()
		...

	ondestroy
		destroy cb
```

