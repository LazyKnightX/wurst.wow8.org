---
title: 组, 枚举, 迭代
sections:
- 导入
- 所有你需要的
- API总览
- 更高级的东西
---

### 导入

Wurst标准库提供了units的简单便利的封装.



名义上说  `ForUnitsInXXX` 函数族创建了一个基于 `group` 句柄类型的抽象.


### 所有你需要的
你只需要了解一下这个接口就行了.


```wurst
interface ForGroupCallback
	function callback(unit u)
```

`ClosureForGroups`包会提供给你一些函数, 将会用到`ForGroupcallback`.


### API总览

单位:

```wurst
group.forEachFrom() u ->
	// 删除单位组,每次遍历访问U
	// `u`.
```

```wurst
forUnitsOfPlayer(player p) u ->
	// 迭代玩家P的所有单位
	// 注意这个方法封装了 `GroupEnumUnitsOfPlayer`,它包含了蝗虫单位
```

```wurst
forUnitsAll() u ->
	//通过迭代每个玩家的单位迭代所有单位.
```

```wurst
forUnitsInRect(rect r) u ->
	// 迭代矩形区域r中的单位
```

```wurst
forUnitsInRange(vec2 pos, real radius) u ->
	// 迭代圆范围内单位.
```


### 更高级的东西

标准库提供了一些稍微更强悍一点的工具:

```wurst
forNearestUnit(vec2 pos, real range, filterfunc filter) u ->
	//在给定的过滤器下访问离pos最近的单位.
```

以及一些相似的,对可破坏物的封装

```wurst
forDestructablesInRect(r) d ->
forNearestDestructable(pos, range) ->
```
