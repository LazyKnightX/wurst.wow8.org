---
title: 错误处理
sections:
- 导入
- 重复的错误
- 自定义错误
---

### 导入
你知道的,Wurst有着将栈轨迹(`调用栈的调用轨迹`)注入你的代码的能力.这意味着你可以看到Error发生时代码的前后的运行轨迹.这些Error在wurst内部产生问题时发生.比如内存用尽或者空指针解引用.
堆栈轨迹由一个运行参数`-stacktraces`控制,在默认的时候这个是开着的.

![](/assets/images/blog/bestof9/stacktrace.png){: .img-responsive}

### 重复的错误
`ErrorHandling`唯一的额外逻辑是是防止重复的相同的错误信息轰炸玩家.Error和栈轨迹只会被显示一次.重复的错误会被忽视
`MUTE_ERROR_DURATION` 秒 (默认60秒).

### 自定义错误
抛出错误并不是编译器的专有权利.他实际上指向一个标准库内的`error`函数.
你可以在你的代码中通过调用`error`,并附上合适的信息,自己生成error.

```wurst
public function myAPI(unit target)
	if target == null
		error("Target may not be null")
```
