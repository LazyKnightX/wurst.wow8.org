---
title: 介绍 (Part 2)
sections:
- 说明
- 标准库
- 代码规范
- Build, deploy, and release
- How to find library code
---

------

# 说明

让我们进一步详细分析我们刚刚做了什么。

## 标准库

如果您以前使用过Jass/vJass，您可能会想知道**print**是从哪里来的。

它既不是针对`BJDebugMsg()`特别编写的封装，也不是wurst的内部函数，而是一个定义在*标准库*（又名stdlib/stl）中的函数。

WurstScript提供了一个标准库，其中包含大量方便的封装、实用的程序包和强大的系统，它们能够帮助几乎所有的地图，并帮助维护高级编程的一致语法。

当您使用安装工具创建项目时，它已被**自动导入**，并且现在位于项目根目录下的`_build/dependencies/wurststdlib2`文件夹中。由于这是外部依赖项，因此您**不应该**修改这些文件。但是您可以随时打开、查看其中的内容并可以通过它们来学习。

您可以通过按住Ctrl键然后单击函数（如单词**print**上的任何位置）来访问vscode中的Printing包。

![](/assets/images/beginner/jumptodecl.gif){: .img-rounded .img-responsive}

`Printing`包中的函数引用了原始的魔兽争霸3的native函数（位于**common.j**和**blizzard.j**中）。

您可以使用vscode的全局搜索或文件搜索（`ctrl+p`）来搜索您要查看的包。

## 代码规范

在Wurst中，我们通常不建议直接调用大多数Native函数。相反，你应该扩展函数，使代码更加简洁、拥有更好的可读性、一致性并且便于撰写文档。扩展函数也可以通过函数自动填充功能更容易找到。

我们来看一些例子。

### 嵌套函数调用

Jass:

```wurst
local integer id = GetPlayerId(GetOwningPlayer(GetTriggerUnit()))
```

在Jass语句中，即便代码是从左向右读的，但实际的执行顺序却是反过来的。`GetTriggerUnit()`在这里是第一格被执行的函数。

与Wurst对比:

```wurst
let id = GetTriggerUnit().getOwner().getId()
```

在Wurst中，代码的执行顺序与其阅读顺序相同，并且没有嵌套括号。Java和其他高级编程语言的用户会觉得这很自然，新用户也可能会觉得这种编码方式更舒服。

这些功能来自另一个自动导入的标准库 - “Unit”API。

### 函数链

Jass:

```wurst
local unit u = GetTriggerUnit()
call SetUnitX(u, 1000.)
call SetUnitY(u, 2000.)
call UnitAddAbility(u, 'hble')
call SetUnitPaused(u, true)
```

Jass不支持在多个目标上的 级联/链接 功能。你必须依赖于局部变量或重复的本地调用的帮助。

Wurst支持链式调用，基于 *级联* 操作符(`..`):

```wurst
GetTriggerUnit()..setX(1000.)
                ..setY(2000.)
                ..addAbility('hble')
                ..setPaused(true)
```

*拓展函数* 不仅仅可以封装现存的native函数，还提供了更便利的函数。上方代码的改进版如下：

```wurst
GetTriggerUnit()..setXY(1000., 2000.)
                ..addAbility('hble')
                ..pause()
```

### 向量数学

向量是三维计算中的非常有用的结构。但是，如果以`class`的形式实现，它们会增加很多开销，这就是为什么vJass不会在任何公共库中使用它们。尽管魔兽提供了`location`类型 - 但使用`location`，并且处理点泄露是非常痛苦的。

Wurst通过[元组类型](/manual.html#tuple-types)实现了向量，不需要像类、结构体那样分配和解除分配，但仍然可以提供优雅的API。

vJass演示:

```wurst
struct PreviousPointTracker
    real x = 0.
    real y = 0.
    real angleToLast = 0.
    
    method add(real addX, real addY) returns nothing
        local real oldX = x
        local real oldY = y
        set x = x + addX
        set y = y + addY
        set angleToLast = Atan2(y - oldX, x - oldY)
    endmethod
endstruct
```

在下方的wurst示例中，你可以看到向量类型使用了重载运算符来实现对向量数学的使用。

```wurst
class PreviousPointTracker
    var pos = vec2(0., 0.)
    var angleToLast = angle(0.)
    
    function add(vec2 addedVec)
        let oldPos = pos
        pos += addedVec
        angleToLast = pos.angleTo(oldPos)
```

注意我们使用了 `angle` 类型来替代使用普通的 `real` 类型。这是一个元祖类型，并且和普通的实数类型一样高效，但它提供了用于处理角度的特定方法来协助使用。此外它还能够避免一些常见的错误，比如弧度和角度的混淆。

# 构建、部署和发布

把 `MyMap.w3x` 视为一个“地形”图。这张地图是你编辑场景、装饰物和可破坏物的地方。Wurst使用这张地图作为编译一张测试地图或者正式发布的地图的起始点。其他的一些变化（比如游戏逻辑相关的常量）也可以在地形图中，但编译的起始点始终是相同的。

地形图对于Wurst来说是“只读”的，这意味着Wurst绝对不会修改或者覆盖地形图。这是一个很好的特性，意味着你可以在地图编辑器中编辑地形的同时，在VSCode中编辑你的代码。

当你在vscode中执行 `buildmap` 时，最终的地图文件（基于地形图和wurst.build文件，以及你的所有编译后的代码）会被输出到 `/_build` 文件夹中。你可以把这里的文件发布或者分发给其他人。
`runmap` 命令是类似的，不过地图名字始终是 __WurstRunMap.w3x__，并且Wurst将会把该地图复制到你的魔兽地图文件夹下，然后魔兽会直接运行这个地图。

你仍然可以将生成的地图拿去发布，不过`runmap`更多时候只用于测试地图。

另一个很强大的特性是，Wurst会自动导入 `imports/` 文件夹内的内容到构建的地图文件中。这个特性可以协助你快速添加模型、贴图文件等等。

比如，如果你想要在地形图中使用导入的资源，将被生成到`/_build`目录下的地图复制到项目根目录，随后使用WE打开它、清除Wurst生成的代码后，然后保存它。你就拥有了一个带有被导入的模型、贴图等的地形图。

# 如何查找函数库代码

正如你所看到的，Wurst拥有一个强大的API来基于Jass提供的类型和函数编写等级更高的代码。作为一个初学者，你的第一个问题可能是如何找到合适的函数库的代码来做你想做的事，而不是重新使用Jass的native函数。这里有3个提示来让你快速上手：

* 查看标准库目录下的子文件夹，以了解您可能找到哪些您需要的标准库代码
* 尝试使用自动补全功能。如果你有一个单位`u`，并且你想要杀死这个单位，你可以试试输入`u.k`后，按下`ctrl-space`键来查看vscode会给你提供哪些函数。
* Avoid writers block. Sometimes it's better to write it the jass way and come back afterwards in case you can't find the wurst equivalent. After all, there's no reason wurst would disallow this! Keep in mind as well that wurst doesn't have fancy API equivalents for every single jass call.

如果您正在寻找能够替代hiveworkshop上某个vJass库功能的Wurst库或函数，您可能会需要使用一些不同的方法。

* 考虑是否可能有更好的东西。例如，我们鼓励Wurst用户使用通用的`HashMap<k, v>`，而不是`Table`，这使得你能够使用更安全的强类型代码。
* 问问其他人。有时候，我们其中的一人已经翻译了你喜欢的vJass库。比如Cokemonkey11已经维护了许多先前使用vJass制作的地图。
* 自己写一个！我们期待贡献者。
