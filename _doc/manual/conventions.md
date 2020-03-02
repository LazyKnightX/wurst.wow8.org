---
title: 代码规范
sections:
- 源码的组织结构
- 命名规则
- 格式化
- 文档注释
- 避免warnings
- 通顺的使用语言特性
---

代码规范描述了一套规则,这些规则不会强制生成errors,但在开发者间作为建议被广泛接受,以生成一致并规范的代码.


### 源码的组织结构

#### 文件结构

你的所有包应该丢进你工程的`wurst/`文件夹里。你可以在这个文件夹内创建任何形式的自定义文件夹结构,因为包不会受到文件夹结构的影响.

文件夹应该用来组织包和文件.

#### 源文件名


如果一个 `.wurst` 文件只包含一个类或者元组(with potentially closely related declarations), 则该文件应该与该类或元组同名.如果一个文件包含多个类,元组,或者仅仅是一些顶部声明,那么选个能描述这个文件包含什么内容的名字.多个单词用驼峰命名法.(比如 `ClosureEvents.wurst`).

#### 源文件的组织
我们鼓励把多个声明(类,元组,顶层的函数)放置在同一wurst源文件中的行为,只要这些声明在语义上高度关联.并且文件的长度足够合理(没有超过几百行)

#### 类的布局

通常来说,类的内容以如下顺序保存.
* 成员变量的声明
* 构造函数
* 一般函数声明

不要按字母序来排列函数声明,不要在扩展方法中分离常规函数.作为替代,将相关联的事物放在一起,这样的话如果有人自上而下阅读该类的时候,就能够搞清楚发生了什么.选择一个组织的顺序(要么更高层次的事物优先,或者与之相反),并长久的保持这样的习惯.包的API应该放在文件顶部,并配上一个文档说明,使得其他开发者能开门见山的看到。

将嵌套的类放在使用这些类的代码旁,如果这些类被设计为外部使用的,并且在类(文件?)内部没有被引用.则将他们放置在最后.


#### 实现接口
实现接口的时候,保持接口的成员在实现中的顺序与在接口中声明的顺序一致.(如有需要的话,也可以跟该实现使用的额外私有方法一起散置开,)


#### 重载布局

在一个类中,把相同的重载放在一起


### 命名规则
Wurst和Java的命名规则类似,特别要强调的是:

包名和类名开头大写,并用驼峰命名法
 (MyExamplePackage).

类名开头大写,并用驼峰命名法

元组名开头小学,并用驼峰命名法

函数,属性,局部变量开头小写,用驼峰而非下划线

常量名使用全大写的下划线分割命名法.


#### 选一个好名字

类名和元组名通常应该是名词或者名词组成的短句,以解释这个类或者元组是什么:
`LinkedList`, `InstantDummyCaster`.

函数名通常是动词或者动词短语,以解释这些方法的作用: `add`, `castImmediate`.
该名字应该提示该方法是否修改了对象或返回了一个新对象

名字应该凸显出该实例的目的.所以最好不要用无意义的单词:(Manager, Wrapper etc.) 

在用缩写命名的时候,如果缩写仅为两个字符,两个字符都大写
 (`IOTaskExecutor`); 更长的话则单单大写首字符(`LzwCompressor`, `Wc3String`).

### 格式化

通常情况下,Wurst遵循JAVA的代码规范.
In most cases, Wurst follows the Java coding conventions.

使用4个空格或者tap来做缩进.不要在同一个文件中混用两者


#### 平行的空格

在二元操作符间添加空格 (`a + b`).

一元操作符不要添加空格(`a++`)

在流程控制的关键词间加一个单一的空格(`if`, `switch`, `for` and `while`) ,并跟着表达式,如果不是为了提升可读性,不要用括号.

函数的调用和声明,在开括号(`就是()`)前不要有空格.

```wurst
function foo(int x)

init
    foo(1)
```

不要在 `(`, `[`之前,或`]`, `)`之后加空格.

不要在 `.` 或 `..` 周围加空格

在注释后跟一个空格 `//`: `// This is a comment`

不要在用来指定参数类型的尖括号周围加空格
`class HashMap<K, V>`

作为通用的规则,避免任何形式的水平排列.将一个标识符重命名为不同的长度不应该影响声明和使用的格式.


#### Lambda 格式

In lambda expressions, spaces should be used around the `begin` and `end` keywords, as well as around the arrow which separates the parameters from the body. If a call takes a single lambda, it should be passed outside of parentheses whenever possible. Prefer putting lambda parameters as the last argument, so it can be written without `begin` and `end`.

```wurst
list.filter(t -> t > 10)

execute() ->
	hash = hash()
```

### Documentation comments (hot doc)

For documentation comments, also known as hot doc, that will show up in auto-completion, place the opening `/**` and close them with `*/`. Short comments can be placed on a single line.

`/** This is a short documentation comment. */`

Wurst does not provide support @param and @return tags right now. Instead, you should incorporate the description of parameters and return values directly into the documentation comment, and add links to parameters wherever they are mentioned.

### Avoiding warnings

Address any warnings the compiler shows for your code. If you need to ignore a warning for an intentionally unused variable, prefix it with an underscore `_`.

### Idiomatic use of language features

#### Local variable declaration

Prefer declaring locals at the location where they are needed over declaring them all at the top like in Jass. Merge declarations and first assignments if sensible.

#### Type inference

Prefer using `var` and `let` over explicit types whenever possible.

#### Immutability

Prefer using immutable data to mutable. Always declare local variables and members as `let` rather than `var` if they are not modified after initialization.

#### Using loops

Prefer using higher-order functions (`filter`, `map` etc.) to loops. Exception: `forEach` (prefer using a regular for loop instead, unless the receiver of `forEach` is nullable or `forEach` is used as part of a longer call chain).

When making a choice between a complex expression using multiple higher-order functions and a loop, understand the cost of the operations being performed in each case and keep performance considerations in mind.

### Unit Testing

Prefer test driven development if the feature is not too dependent on wc3 game mechanics. Create small, self-contained functions, annotate them with `@Test` individually and give them a descriptive name.
Make sure to have at least one **assertion** inside your test to verify the behavior.

Tests should either be placed at the end of a package, or into a separate package suffixed `Tests` which will be ignored for autocomplete suggestions. Code and Tests should not be mixed.
