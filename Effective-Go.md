# Effective Go

[介绍](#介绍)<br/>
&ensp;&ensp;[栗子](#栗子)<br/>
[代码格式化](#代码格式化)<br/>
[注释](#注释)<br/>
[命名规范](#命名规范)<br/>
&ensp;&ensp;[Package](#Package)<br/>
&ensp;&ensp;[Getter](#Getter)<br/>
&ensp;&ensp;[Interface](#Interface)<br/>
&ensp;&ensp;[MixedCap](#MixedCap)<br/>
[分号的用法](#分号的用法)<br/>
[控制流](#控制流)<br/>
&ensp;&ensp;[If](#If)<br/>
&ensp;&ensp;[重新声明与重新赋值](#重新声明与重新赋值)<br/>
&ensp;&ensp;[For循环](#For循环)<br/>
&ensp;&ensp;[Switch](#Switch)<br/>
&ensp;&ensp;[类型Switch](#类型Switch)<br/>
[函数](#函数)<br/>
&ensp;&ensp;[返回多个值](#返回多个值)<br/>
&ensp;&ensp;[返回值命名](#返回值命名)<br/>
&ensp;&ensp;[Defer](#Defer)<br/>
[数据类型](#数据类型)
&ensp;&ensp;[用用new](#用用new)<br/>
&ensp;&ensp;[构造方法与字面量初始化](#构造方法与字面量初始化)<br/>
&ensp;&ensp;[用用make](#用用make)<br/>
&ensp;&ensp;[数组](#数组)<br/>
&ensp;&ensp;[切片](#切片)<br/>
&ensp;&ensp;[二维切片](#二维切片)<br/>
&ensp;&ensp;[Map](#Map)<br/>
&ensp;&ensp;[Print](#Print)<br/>
&ensp;&ensp;[Append](#Append)<br/>
[初始化](#初始化)<br/>
&ensp;&ensp;[常量](#常量)<br/>
&ensp;&ensp;[变量](#变量)<br/>
&ensp;&ensp;[init函数](#init函数)<br/>
[方法](#方法)<br/>
&ensp;&ensp;[指针与值](#指针与值)<br/>
[接口和其他类型](#接口和其他类型)<br/>
&ensp;&ensp;[接口](#接口)<br/>
&ensp;&ensp;[类型转换](#类型转换)<br/>
&ensp;&ensp;[接口转换与类型断言](#接口转换与类型断言)<br/>
&ensp;&ensp;[](#)<br/>
&ensp;&ensp;[接口和方法](#接口和方法)<br/>
[空标识符](#空标识符)<br/>
&ensp;&ensp;[多变量赋值中的空标识符](#多变量赋值中的空标识符)<br/>
&ensp;&ensp;[无用的import和变量](#无用的import和变量)<br/>
&ensp;&ensp;[利用import的副作用](#利用import的副作用)<br/>
&ensp;&ensp;[接口检查](#接口检查)<br/>
[嵌入](#嵌入)<br/>
[并发](#并发)<br/>
&ensp;&ensp;[利用通信来共享数据](#利用通信来共享数据)<br/>
&ensp;&ensp;[Go协程](#Go协程)<br/>
&ensp;&ensp;[Channel](#Channel)<br/>
&ensp;&ensp;[Channel嵌套](#Channel嵌套)<br/>
&ensp;&ensp;[并行](#并行)<br/>
&ensp;&ensp;[缓冲泄露](#缓冲泄露)<br/>
[错误](#错误)<br/>
&ensp;&ensp;[Panic](#Panic)<br/>
&ensp;&ensp;[Recover](#Recover)<br/>
[一个web服务](#一个web服务)<br/>


## 介绍

Go是一门儿挺新的语言。当然Go也是借鉴了很多已有的语言，但是Go的一些不寻常的特性又让Go在这些语言中显得与众不同。如果直白的将一个C++或者Java程序翻译成Go写的，则很难得到想象中的结果——Java是用Java写，不是用Go。换句话说，要想写好Go，先要懂Go。还要懂得那些在Go语言中已经建立起来的约定，比如命名、格式化、程序构造等等，这样别人才能看懂你写的Go。

本文讲的是如何编写清晰的、符合习惯的Go代码。其中涵盖[语言规范]()、[Go之旅]()、[如何编写Go]()，你应该提前把这些东西都看一遍。

### 栗子

[Go源码](https://golang.org/src/)的作用不光是作为核心代码库，同样也是教你如何使用这门语言。而且这里面包含了很多那种自包含且可以直接拿来在[golang.org](https://golang.org/)上运行的栗子，比如[这个](https://golang.org/pkg/strings/#example_Map)（需要的话点击里面的"Example"展开一下）。如果你想了解某种处理方法及其实现方式，它里面的文档、代码以及示例都可以问题提供相应的答案、思考以及背景知识。

## 代码格式化

代码格式化问题是最容易引起争论的了，但它又是最没什么影响的问题，简直就是鸡毛蒜皮。大家可以使用不同的格式化风格，但如果不用费心选择也是很好的效果，如果大家坚持使用同样的风格，那我们也就不用花太多时间来讨论这个东西了。问题在于如何避免大段大段的说明来达到这种理想中的乌托邦。

我们在Go语言中采取了一个不寻常的手段，让机器来处理大部分的格式化问题。`gofmt`这个程序（也就是`go fmt`命令，它会操作package级别而不是源文件级别）读取Go程序然后将源代码调整为标准的缩进格式，并进行垂直对齐，它会保留你的注释，必要时还会对注释重新格式化。如果你碰到了某种新的布局，那就运行一下`gofmt;`如果结果不nice，尝试重新调整你的代码（或者提一个`gofmt`的bug出来），不要陷入到工具本身的调试中。

举个栗子，不要把时间浪费到结构体字段的注释格式上。`Gofmt`会帮你完成这些事。比如下面的定义

```go
type T struct {
    name string // name of the object
    value int // its value
}
```

`gofmt`会进行自动对齐：

```go
type T struct {
    name    string // name of the object
    value   int    // its value
}
```

标准库中的所有Go代码都已经基于`gofmt`进行格式化了。

还有一些小细节，这里简单一提：

缩进

&ensp;&ensp;`gofmt`默认会自动用tab进行缩进。只在绝对必需的时候才用空格。

行长度

&ensp;&ensp;Go没有行长度限制。不要怕打满穿孔卡片（WHAT？？？？）。如果行太长，折叠一下，加个tab缩进一下。

括号

&ensp;&ensp;Go代码中的括号比C和Java的要少很多：控制流（if、for、switch）语法中都没有括号。而且运算符优先级也更加简洁，比如

&ensp;&ensp;``x<<8 + y<<16``

&ensp;&ensp;它的含义通过空格就可以看出，这里跟其他语言就不一样了

## 注释