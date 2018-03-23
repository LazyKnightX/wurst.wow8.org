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

This file as __all .j files will not be autmatically reconciled__. This means wurst will use a cached version and not detect errors nor take changes into account unless you clean your workspace via `F1 -> clean`.
Also, if you make changes to the war3map.j and want them applied to the map, you have to add the RunArg **-noExtractMapScript**
to your wurst_run.args file, which will prevent the script extraction from the map and use the provided war3map.j inside the wurst folder instead.

![](/assets/images/legacy/uselegacycode.png){: .img-responsive .img-rounded}

## Jurst dialect

Jurst is a dialect of Wurst, which has the same features as Wurst, but with a Syntax similar to vJass. You can use Jurst to adapt vJass code, but there are still a few manual steps involved, because of the difference in supported features.
Jurst also allows for parsing of regular Jass code, making it a less strict mix of Jass and Wurst.
Take a look at these following examples, which all compile:

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

As you can see, Jurst is able to access wurst code including packages from the wurst standard library, such as print. However, you should not try to access Jurst (or jass/vjass) code from Wurst packages.

