---
title: 旧代码
sections:
- 编译旧代码
- 在Wurst内使用传统代码
- Jurst dialect
---

## 编译旧代码

### Jass

如果您只使用原生的触发编辑器，或者原生的Jass，则代码集成应该是无缝的。
使用魔兽地图编辑器保存你的地图会将你的T或者J输出到地图本身，随后在执行`runmap`命令时，编译器会提取该脚本，并将其与生成的Wurst代码合并。

### vJass和其它语言

为了避免任何的 __Wurst does not support vJass directly! （Wurst不支持直接在脚本中编写vJass！）__ 错误（注意未来Wurst也不会兼容vJass），你需要手动运行JassHelper（或者其它的预处理器），然后将生成的Jass合并到Wurst中。
无论是在拥有JassHelper的编辑器（如JNGP，WEX，YDWE），还是手动把JassHelper添加到你的构建链中，你必须做的事情都是把vJass转换为Jurst，这需要你手动对代码做一些调整。

如果您使用像JNGP或WEX这样的工具来自动将修改后的脚本注入到地图中，则Wurst会从地图中提取生成的Jass代码并使用它。
这与上面提到的如何使用常规Jass相同，但是它禁止在原生WorldEditor中直接保存，因为那会覆盖地图的脚本。

## 在Wurst内使用传统代码

在少数情况下，在Wurst内使用传统代码，能够在没有编译错误的同时，获取Wurst自带的好处（比如自动补全、跳转到函数声明）。
为了达成这个效果，你必须从你的传统地图中把war3map.j提取出来，并把它放置在你项目下的 **wurst/** 文件夹中。

![](/assets/images/legacy/war3map.j.png){: .img-responsive .img-rounded}

这些文件和所有的J文件一样,__不会被自动的核对更新__.这意味着Wurst会使用一个缓存的版本,而不是去考虑检测错误亦或者改变什么,除非你通过`F1 -> clean`清空工作空间。
所以,如果你改动了war3map.j,并且希望这些改动能作用于地图,你必须在你的wurst_run.args文件添加运行参数 **-noExtractMapScript**,这样能阻止从地图中导出脚本,并使用在wurst文件夹内所提供的war3map.j.

![](/assets/images/legacy/uselegacycode.png){: .img-responsive .img-rounded}

## Jurst dialect

Jurst是Wurst的一个方言,功能和Wurst类似,但是具有类似vjass的语法.你可以使用Jurst去兼容vjass代码.但是由于支持特性的差异,这儿依然有一些你需要手动需要做的步骤,

Jurst也允许解析常规的jass代码,使得wurst和jass的混编更加的宽松.

看看下面这些例子,皆可以编译通过:
```wurst
library LooksLikeVjass initializer ini
    private function ini takes nothing returns nothing
        local trigger t = CreateTrigger()
        t = null
    endfunction
endlibrary
```
and

```wurst
package NearlyTheSameAsWurst
    function act()
        print(GetTriggerUnit().getName() + " died.")
    end

    init
        CreateTrigger()..registerAnyUnitEvent(EVENT_PLAYER_UNIT_DEATH)
                       ..addAction(function act)
    end

// Note: "end" intentionally not present here.
```
如果你所见,Jurst能够访问wurst代码,包括wurst的标准库内的包,比如print之类的.不过,在Wurst包内,你无法访问Jurst代码

