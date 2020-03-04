---
title: Wurst单元
sections:
- 导入
- 魔法函数
- 断言
---

### 导入

我们在
[BOTW 4](https://wurstlang.org/blog/bestofthewurst4.html) 已经讨论了这个话题.让我们进一步的研究一下.

### 魔法函数
首先这里有三个函数被`@compiletimenative` 注解着.而其中后两个并没有函数的实现.
他们用这个注解来表示他们是一个自定义的原生函数,由wurst编译器实现.且只有在编译时才有效.他们和war3是分离的.特殊情况是`println`,他既有一个在Jass中运行的实现,也有一个运行时的版本是由编译器实现的.


```wurst
@compiletimenative public function println(string msg)
	DisplayTimedTextToPlayer(GetLocalPlayer(), 0, 0, 60, msg)
@compiletimenative public function testFail(string msg)
@compiletimenative public function testSuccess()
```

`println` 只是一个在vscode 输出栏中输出信息的函数.
在你的代码里,你可以使用他和`print`来施行日志,结果会显示在测试单元里.
You can use it and `print` to perform logs in your code that will show up in tests.
`testFail`和 `testSuccess` 是让单元测试成功或者失败的函数.取决于哪个先调用.
测试默认成功,而你也应该确保自己使用断言而非这些原生的函数.


### 断言
在这些编译时的原生函数后我们有一列
__assertXX__ 函数,他们基于 `testFail` 和 `testSuccess`构建, 提供更好的API和debug输出.
作为代码规范,你应该更优先考虑类型扩展函数而非底层函数.让我们看看一个例子.首先一个不是很理想的对于两个整型的断言:

```wurst
// 这样做不好
assertTrue(someInt == someOtherInt)
```
如果测试失败,你只能获得一个断言失败的信息,而不能得到其他更多的信息.
If the test fails, you will only get a failed assertion without further information:


```
FAILED assertion:
Test failed: Assertion failed
```

另一边如果你使用了正确的断言方法.

```wurst
// Better
someInt.assertEquals(someOtherInt)
```
这样看上去代码样式更好看,而且提升了debug输出的效果.
```
FAILED assertion:
Test failed: Expected <2>, Actual <1>
```
为了让一个断言能够在自定义数据上实现,你可以为这个类写一个自定义的
 `assertEquals` 拓展函数.
 
考虑参考一下那些已经给你写好的函数,并复制一下功能.


```wurst
class MyData
	var x = 0
	var p = players[0]

public function MyData.assertEquals(MyData expected) returns boolean
		if  this.x != expected.x or this.p != expected.p
			testFail("Expected <" + expected.toString() + ">, Actual <" + this.toString() + ">")
```

