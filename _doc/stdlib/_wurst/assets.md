---
title: 资源
sections:
- 导入
- 对象编辑
- 在运行时
---

### 导入

War3有一点很棒,他有着大量的贴图,音效,模型等我们可以直接利用的资源.由于我们在Wurst开发中尝试避免使用WE,因此去手动追踪这些资源的路径和ID就变得非常艰难了.
为了减轻这一负担,我们引入了资源管理包,将这些内部的值作为简单的Wurst变量暴露出来.


### Object Editing

在编译时创建物体的时候你应该总是使用一个资源变量,而不是硬编码原生的路径和ID.这样能够有效地防止你打错字,还能避免一些其他不必要的错误.


```wurst
new UnitDefinition(MY_ID, UnitIds.footman)
..setIconGameInterface(Icons.bTNCentaur)
```

### 运行时

这些资源在运行时也能提供不少帮助,比如创建一个特效或者音效.


```wurst
flashEffect(Abilities.pileofGold, ZERO2)

new SoundDefinition(Sounds.zombieDeath1, false).play()
```
