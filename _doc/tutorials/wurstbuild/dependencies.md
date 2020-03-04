---
title: wurst.build文件
sections:
- 欢迎
- 文件结构
- 依赖
---

## 欢迎

这篇教程记载了如何将wurst安装工具和wurst.buid文件组合起来,以创造一个无版本冲突的地图以及工程的配置的方法.
This tutorial documents the usage of the wurst setup tool in combination with the wurst.build file to create versionable map and project configuration.

## 文件结构

`wurst.build`文件是一个yaml文件,必须放在你工程的根目录下,理论上而言应该是靠近`Wurst`文件夹的文件.
这个文件控制着你的工程的依赖项,以及build task如何配置你的地图.
所有的值皆是可选的.不完整或者无效的配置会被地图已经存在的配置以及默认值所替换.下面是一个完整的,所有的域皆是默认值的wurst.build文件,如果你不想改变这些默认值,你可以忽视掉这些域.


```yml
projectName: Escape Builder Reloaded
dependencies:
- https://github.com/wurstscript/wurstStdlib2
- https://github.com/frotty/frentity
buildMapData:
  name: JustAnotherWurstMap # Name shown ingame, may be colored
  fileName: MyWurstMap # Name of the map file
  author: SomeWurstUser # Name of the author
  scenarioData:
    description: WurstScript powered! # The map's description
    suggestedPlayers: DefaultSuggestedPlayers # Hint text displayed in lobby
    loadingScreen:
      model: Generic.mdx
      title: DefaultTitle
      subTitle: DefaultSubtitle
      text: DefaultText
  optionsFlags:
    hideMinimapPreview: false
	forcesFixed: false
    maskedAreasPartiallyVisible: false
    showWavesOnCliffShores: false
    showWavesOnRollingShores: false
    useItemClassificationSystem: false
  players:
  - id: 0
    name: DefaultPlayer
    race: HUMAN
    controller: USER
    fixedStartLoc: false
  forces:
  - name: DefaultForce
    playerIds:
    - 0
    flags:
      allied: true
      alliedVictory: true
      sharedVision: true
      sharedControl: false
      sharedControlAdvanced: false
```

## 依赖

如今的安装不会锁定版本,但是将来会考虑这个功能.依赖链则不会被解析.
The setup does no version locking for now, but this is planned for a future version. Transitive dependencies will not be resolved.

依赖需要填写一个公共git repo的简单url,将会被setup程序拉去.

默认情况下,setup程序安装的工程会包含标准库 `https://github.com/wurstscript/wurstStdlib2`

如果你要添加一个依赖,那添加一行有效的git https url.然后通过setup UI或者CLI更新你的工程项目 `grill -projectDir ~./Documents/MyProject/ update`.

