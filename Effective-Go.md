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

Go支持C风格的/**/多行注释以及C++风格的//单行注释。一般都用单行注释；多行注释经常用在包级别的注释上，它在表达式中也是有用的，而且还能避免过多单行注释影响美观。

`godoc`程序——同时还是一个web服务——可以处理Go的源文件并其提出每个包相关的文档。出现在声明之前的注释，会被无差别的提取出来作为它的说明文档。因此这些注释的风格决定了`godoc`生成的文档的水平。

每个包都应该有一个*包注释（package comment）*，也就是在package语句之前的一段多行注释，包注释只能出现在某一个文件中。包注释要负责介绍整个包的作用和相关信息。它会出现在`godoc`生成的第一个页面中，因此其中需要给出整体的概括。

```go
/*
Package regexp implements a simple library for regular expressions.

The syntax of the regular expressions accepted is:

    regexp:
        concatenation { '|' concatenation }
    concatenation:
        { closure }
    closure:
        term [ '*' | '+' | '?' ]
    term:
        '^'
        '$'
        '.'
        character
        '[' [ '^' ] character-ranges ']'
        '(' regexp ')'
*/
package regexp
```

如果包很简单，那也可以用单行注释。

```go
// Package path implements utility routines for
// manipulating slash-separated filename paths.
```

注释不需要什么添油加醋的东西，比如什么星星banner，有好事者经常这么干。由于生成的内容有可能在定宽字体的布局中根本显示不出来，所以不要依赖使用空格实现的对齐效果——`godoc`和`gofmt`一样，都会自己处理这些事。注释就是普通的文本，不会进行解释处理，所以像是HTML或者其他的比如_this_这样的标记，会被一字不差的展示出来，所以不要用这些不干不净的东西。`godoc`会做的一项调整就是用等宽字体来显示内容，这样也便于展示代码。[fmt包]()的注释就给出了一个很好的示例。

`godoc`根据实际情况，有可能不会对注释的内容进行格式调整，所以要确保它们格式清晰：请正确拼写，写好标点，规范句子结构，做好换行。

进了package之后，所有出现在顶层声明之前的注释都会作为它的*文档注释*。所有被导出的（大写开头的）名字都应该有一个文档注释。

文档注释最好是一个完整的句子，这样对于自动化展示工具都具有普适性。第一句应当以声明的内容开头然后用一句话进行概括。

```go
// Compile parses a regular expression and returns, if successful,
// a Regexp that can be used to match against text.
func Compile(str string) (*Regexp, error) {
```

如果每一处文档注释都时以它声明的内容开头，那你就可以用可以用[go]()的[doc]()子命令然后做`grep`。比如你已经记不得“Compile”这个名字了，但又想找正则的解析函数，那你就可以，

```shell
$ go doc -all regexp | grep -i parse
```

如果所有的文档注释都是用“这个函数是……”这样的东西开头，那`grep`有可能也帮不上你什么，没法帮你记名字。但如果是用名字开头，那你就会看到下面这样的内容，它可以帮你想起你到底要找什么了。

```shell
$ go doc -all regexp | grep -i parse
    Compile parses a regular expression and returns, if successful, a Regexp
    MustCompile is like Compile but panics if the expression cannot be parsed.
    parsed. It simplifies safe initialization of global variables holding
$
```

Go的声明语法允许声明分组，这样，一个单行的文档注释就可以用来解释一组相关的变量或者常量。因为是作为整体进行声明，这样的注释也往往有些敷衍。

```go
// Error codes returned by failures to parse an expression.
var (
    ErrInternal      = errors.New("regexp: internal error")
    ErrUnmatchedLpar = errors.New("regexp: unmatched '('")
    ErrUnmatchedRpar = errors.New("regexp: unmatched ')'")
    ...
)
```

分组可以同时展示出它们之间的关系，比如下面的一组变量都要用一个互斥锁来保护。

```go
var (
    countLock   sync.Mutex
    inputCount  uint32
    outputCount uint32
    errorCount  uint32
)
```

## 命名规范

名字么，这个东西在哪个语言中都很重要滴。这里面甚至还有语义上的影响：一个名字是否在包外可见就在于它是否是大写开头。所以这里值得我们花一点时间来看看Go程序中的命名规范。

### Package

当我们导入了一个包，那么包的名字就成了我们用来访问它的标识。比如

```go
import "bytes"
```

导入了之后就可以调用`bytes.Buffer`了。如果大家都能用同样的名字来访问同一个包那真是再好不过了，这就意味着你的包名起的很好：简洁大方，人如其名。一般我们的包名都要求小写，一个单词完事儿；用不着什么下划线或者驼峰。之所以要简洁，是因为用你包的每个人都要去打这个名字。你用不着担心名字冲突。包名只是导入后默认的名字；它不需要在所有源文件中保持唯一，而且在一些极端恶劣天气情况下，真的出现名字冲突的时候，那我们在导入的时候可以为本地使用声明一个别的名字。所以真的不用怕混淆，因为导入的文件名只是用来声明我们要用哪个包。

另一项规范是，包名是源码目录的基础名；比如位于`src/encoding/base64`的包在导入时是`"encoding/base64"`但名字叫`base64`，而不是什么`encoding_base64`，也不是`encodingBase64`。

导入包之后，用的时候就用包名来访问，因此，包中导出的名字基于这一点就不用担心冲突的问题了。（不要使用`import .`语法，虽然它可以简化那些必须运行在包外的测试类，但也应该尽量避免这么用。）例如，`bufio`包中的缓冲读取器叫`Reader`，而不是`BufReader`，因为用户在用的时候写的是`bufio.Reader`，已经表明了缓冲的含义，多么简洁的名字啊，真让人神往。而且，因为导入的实体通常都是通过包名来访问，`bufio.Reader`就不会跟`io.Reader`冲突了。同样，用来创建`ring.Ring`实例的函数——在Go中叫*构造器（constructor）*——一般我们会写成`NewRing`，但是因为`Ring`是这个包里唯一一个导出的类型，而且包叫`ring`，所以就直接一个`New`就好了，用的时候就是`ring.New`。用包名来更好的规范的你的命名。

另一个栗子是`once.Do`；`once.Do(setup)`读起来就很简单，如果是`once.DoOrWaitUntilDone(setup)`就不是那么回事儿了。名字越长不一定越好。有用的文档注释要比长长的名字更有用。

### Getter

Go不会自动提供getter和setter。无疑你要自己做，而且也应该自己做，但是一般不要把`Get`这种字眼加到getter的名字中。如果你有一个字段`owner`（小写，未导出），那么getter方法就应该叫`Owner`（大写，导出），而不是`GetOwner`。将大写开头的名字导出，这也将属性和方法区分开来。如果需要setter函数，一般叫`SetOwner`。在实际场景中，这样的可读性也很好：

```go
owner := obj.Owner()
if owner != user {
    obj.SetOwner(user)
}
```