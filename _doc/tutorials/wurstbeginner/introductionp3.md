---
title: 介绍 (Part 3)
sections:
- 来做个技能吧
- 导入
- Buff效果
- Buff map
- 结语
---

------

# 来做个技能吧

我们制作了一个技能样例,你可以点击这里查看[march's blog post](https://wurstlang.org/blog/bestofthewurst5.html). 

让我们进一步看一下这些代码.

```wurst
package Conflagration
import ConflagrationObjects
import ClosureTimers
import ClosureForGroups
import HashMap
import ClosureEvents

let buffId = BUFF_OBJ.abilId
let buffMap = new HashMap<unit, CallbackSingle>()
init
	EventListener.onPointCast(SPELL_ID) (caster, tpos) ->
		flashEffect(SPELL_EFFECT_PATH, tpos)
		doAfter(SPELL_EFFECT_DURATION) ->
			forUnitsInRange(tpos, SPELL_RADIUS) u ->
				if u.hasAbility(buffId)
					caster.damageTarget(u, BONUS_DAMAGE)
					flashEffect(BONUS_EFFECT_PATH, tpos)

				caster.damageTarget(u, BASE_DAMAGE)
				u.addAbility(buffId)
				if buffMap.has(u)
					destroy buffMap.get(u)
				let cb = doAfter(BUFF_DURATION) ->
					if buffMap.has(u)
						buffMap.remove(u)
						u.removeAbility(buffId)
				buffMap.put(u, cb)
```

## 导入

为了让技能创建的过程更加的方便,我们导入了不少包.`ClosureTimers` 和 `ClosureForGroups`能帮助我们更好的处理在 (`doAfter`) 和单位组选取(`forUnitsInRange`)中的代码. `ClosureEvents` 给了我们更好的事件API, 包括闭包. 最后 `HashMap` 使得我们为其他数据绑定任意数据 - 在这儿我们用来将buff效果绑定在目标单位上.

## Buff效果

这个技能是这样工作的:我们使用`EventListener.onPointCast`监听一个特殊的技能释放事件.在lambda区块内写上我们的事件回调.回调函数中我们使用`flashEffect`在合适的位置创建了一个彗星特效.由于技能特效需要花费一定的时间掉落在地上,我们同样希望能够延迟技能的伤害效果.因此用`doAfter(SPELL_EFFECT_DURATION) ->`启用了一个计时器

这个紧跟着的新lambda区块内的代码会在一段时间后被执行.

现在假设特效已经着陆了,我们就可以给目标点圆范围内的所有单位施加伤害.

我们用 `forUnitsInRange(tpos, SPELL_RADIUS) u ->` 选取目标范围内的所有单位,并传递给接下来的lambda函数. 注意`SPELL_RADIUS) u ->`这儿有个 `u`在箭头前 `->`.

在Lambda区块中,我们现在就可以通过u来引用这些在范围内的单位,并应用我们的特效.

这种情况下,如果有的单位已经受到了buff的影响,我们施加一些额外的伤害,并且在接下来的If语句中创建一个即刻的特效.之后我们做常规的操作,施加普通的技能伤害,并给目标添加buff的技能马甲.(如果某个单位已经有了这个技能,就不会有任何影响.)

接下来的分支中我们判断了buff map中是否有一个条目是当前单位,如果是,我们销毁当前存在的计时器,以实现后来的重新计时.

之后,我们新建一个计时器,用来在buff持续时间结束后销毁buff.


`译注:这个技能会在单位拥有buff的时候造成双倍伤害.`
## Buff Map

`let buffMap = new HashMap<unit, CallbackSingle>()`
在这儿我们创建了一个之前提到的buffmap.基本上来说它让我们为每个单位保存一个`ClosureTimers`的闭包实例,这个闭包实例指代一个用来控制buffs持续时间的计时器.


# 结语
我们由衷的希望这篇指南能够帮助你开始wurst地图开发之旅.如果你还有更多的疑问,或者想要和更多的wurst开发者交朋友,欢迎戳这里[Chat](https://kiwiirc.com/nextclient/#irc://irc.quakenet.org/#inwc.de-maps).
