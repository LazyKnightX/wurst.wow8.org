---
title: 高级API
sections:
- 欢迎
- 背后原理
- 保存数据
- 载入数据
- 极限和分区
---

## 欢迎
标准库有一套简单的API用以保存,载入和同步玩家数据.导入
 `SaveLoadData`来使用他

## 背后原理
载入和保存包在内部使用利用preload的行为,这也是如今暴雪官方建议并鼓励的,用于魔兽3根目录下的自定义地图数据文件夹(CustomMapData)载入和保存自定义地图数据的方式.
数据从指定玩家那儿读入,因此只会载入到玩家个人的机器上,在同步环境下(`译:多人联机`),需要通过与其他玩家同步才能使用这些数据.
但Wurst把这些东西进了简单的API中,所以你可以不用关心这方面的细节.


## 保存数据

保存指定玩家的数据,调用`saveData`方法,传入你想保存的数据
注意已经存在的文件会被覆盖.
由于存储不需要同步,因此更简单一些

```wurst
init
	players[0].saveData("MyFileName", "someDataString")
```

这将会在CustomMapData文件夹下的文件`MyFileName.pld`中保存`someDataString`

## 载入数据

从指定玩家那读数据,调用 `loadData`函数,将你保存时使用的文件名传入.由于载入需要同步.`loadData`需要传入了一个闭包作为第二个参数,该闭包会在载入数据和同步都完成时被调用.
载入也可能会失败,倘若文件是空的,亦或者被损坏了.你需要检查`status`参数以确保载入的有效性.
如果状态(`译:status`)是`SUCCESS`,那`data`参数会包含已同步版的文件里的内容.在一个同步上下文(`译:某种程度上可以说是非异步的代码中`)中使用这个`data`数据即可.


```wurst
init
	players[0].loadData("MyFileName") (status, data) ->
		if status == LoadStatus.SUCCESS
			Log.info("Loaded: " + data.readStringUnsafe())
		else
			// some error handling
```

## 极限和分区
我们在上面的例子中使用了`.readStringUnsafe()`. 之所以说是不安全(`unsafe`),是因为`string`的未连接最大长度被限制在了1024bytes,而已连接的最大长度也被限制在了4099个字符.这不仅会限制我们能读取多少的数据,也会导致在寻常情况下的使用更复杂.
而且1024bytes也不等于与1024个字符,由于一些unicode字符比1个byte大.

因此我们对于超过500个字符的情况,通常建议使用 `ChunkedString` .  `ChunkedString`会把大的字符串分割成更小的多个字符串,可以分别访问.

保存函数被重载成了可以允许string和ChunkedString作为输入的版本.
