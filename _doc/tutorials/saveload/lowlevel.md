---
title: 底层API
sections:
- 动机
- 文件IO
- 同步例子
---

## 动机
有时候你不想要高层的API,比如同步不需要在单人模式中实现,因此你不需要这些特性,或者你就是想写一些新的组件.

## 文件IO

读

```wurst
let fileContent = new File("MyFileName.pld").readAsString()
```

写

```wurst
new File("MyFileName.pld")..write("MyContent")..close()
```

## 同步例子

Sync a string:
```wurst
myString.sync() (synced) ->
	Log.info("string synced: " + synced.getUnsafeString())
```

Sync a ChunkedString:
```wurst
myChunkedString.sync() (synced) ->
	while synced.hasChunk()
		Log.info("chunked string synced: " + synced.readChunk())
```


