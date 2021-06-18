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
&ensp;&ensp;[构造方法与复合字面量](#构造方法与复合字面量)<br/>
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

### Interface

一般情况下， 如果接口（Interface）只有一个方法，那我们就用方法名加er后缀来命名这个接口，或者是使用对应的代理名词，比如：Reader、Writer、Formatter、CloseNotifier等。

这种名字有很多，而且都能很好的表达出它们的含义。Read、Write、Close、Flush、String等等，都使用了准确的签名和含义。为了避免混淆，如果不是签名和含义完全一致，那你自己的方法就不要起这种名字。同样，如果你的方法和一个常见的方法拥有相同的含义，那你也应该使用同样的签名；比如你自己的字符串转换方法就得是String而不是ToString。

### MixedCap

最后，Go中的习惯是使用MixedCap或者mixedCap这样的标记法，而不用下划线或者多字结构。

## 分号的用法

和C一样，Go的标准语法也使用分号作为语句的结束，但不同之处在于，分号不是写在源代码中的。词法分析器在扫描源码的时候，会使用一种简单的规则来自动插入分号，因此输入的文本一般就不需要加分号了。

规则如下。如果一行中的最后一个标记是一个标识符（包括int和float64这种关键字），或者一个基本的字面量，比如数字或者字符串常量，又或者是下面的标记之一

```go
break continue fallthrough return ++ -- ) }
```

词法分析器则总是会在它们的后面插入一个分号。这就可以总结为：“如果新的一行之前的最后一个标记能够结束一个语句，那就在这里插入一个分号”。

可以在右括号之前直接省略分号，比如这样的语句

```go
go func() { for { dst <- <-src } }()
```

就不需要分号。典型的Go程序只有在for循环这样的语法中会用到分号，用来区分初始化、条件判断和继续执行的部分。还有如果你把多个语句写在同一行里，那也需要加上分号用来区分。

分号插入规则导致的一个结果是，你不能把控制结构（if、for、switch或select）的左括号放在下一行。如果你这么干了，括号前就会插入一个分号，那就会导致不一样的结果。比如应该写成

```go
if i < f() {
    g()
}
```

而不是

```go
if i < f() // 不对了！
{          // 不对了！
    g()
}
```

## 控制流

Go的控制流源自C，但又有很重要的不同之处。这里没有do和while循环，只有一个稍显宽泛的for循环；这里的switch更加灵活；if和switch都能像for一样声明一个初始化部分；break和continue语句可以增加一个可选的标签来表明要跳出或继续的部分；而且这里还有一些新的控制流，比如类型switch以及多路复用器select。语法也稍有不同：这里没有圆括号，语法体必须要用花括号括起来。

### If

Go里面的if是这样：

```go
if x > 0 {
    return y
}
```

强制使用花括号可以让简单的if语句横跨多行。这样写是很好的风格，特别是当其中还会包含return或break这种控制语句。

因为if和switch可以声明一个初始化部分，一般就都会引入一个局部变量。

```go
if err := file.Chmod(0664); err != nil {
    log.Print(err)
    return err
}
```

在Go的标准库中你会发现，如果if语句导致控制流不继续往下走了——以break、continue、goto或return结尾——一般都会省略多余的else。

```go
f, err := os.Open(name)
if err != nil {
    return err
}
codeUsing(f)
```

下面的栗子是一个常见的判断多种错误情况的代码。如果按照正常执行的情况，代码的可读性就很好，直接跳过错误处理的部分。因为错误情况一般会用return来结束执行，也就不需要加多余的else了。

```go
f, err := os.Open(name)
if err != nil {
    return err
}
d, err := f.Stat()
if err != nil {
    f.Close()
    return err
}
codeUsing(f, d)
```

### 重新声明与重新赋值

上一节最后这个小栗子也展示了:=这个短声明语法。它调用了os.Open，

```go
f, err := os.Open(name)
```

这个语句声明了两个变量，f和err。接着往下，它又调用了f.Stat，

```go
d, err := f.Stat()
```

这里看上去它声明了d和err。注意这两个语句中都出现了err。这种复用是ok的：err由第一个语句声明，但是在第二个语句中进行了重新赋值。也就是说调用f.Stat的时候使用了上面已经声明的err变量，只不过给它赋了一个新值。

在:=声明中，可以使用已经声明过的变量v，比如：

- 该声明和变量v已有的声明在同一个作用域中（如果变量v在外层作用域中声明，此处会创建一个新的变量），
- 初始化部分生成的值会赋值给v，
- 这种声明至少还会创建一个其他的变量。

这种不常见的规矩非常实用，这样可以在一长串的if-else中仅使用一个err变量。你会发现你经常需要这么干。

这里值得注意的是，在Go中，函数的参数和返回值跟函数体处于同一个作用域中，即便它们在语法上是出现在花括号之外的。

### For循环

Go的for循环换和C类似但又不尽相同。它统一了for和while循环，但又不包括do-while。一共有三种形式，只有一种会包含分号。

```go
// C风格的for
for init; condition; post { }
// C风格的while
for condition { }
// C风格的for(;;)
for { }
```

短声明语法非常便于用来声明循环中用到的索引变量。

```go
sum := 0
for i := 0; i < 10; i++ {
    sum += i
}
```

如果你是遍历一个数组、slice、或者是map，又或者是从一个channel中往出读数据，可以用range来处理这种循环。

```go
for key, value := range oldMap {
    newMap[key] = value
}
```

如果你只需要每次range的第一个元素（key或者索引），那就直接去掉第二个：

```go
for key := range m {
    if key.expired() {
        delete(m, key)
    }
}
```

如果你只需要range的第二个元素（值），使用*空标识符（blank identifier）*，即一个下划线，来丢弃第一个元素：

```go
sum := 0
for _, value := range array {
    sum += value
}
```

空标识符用法很多，见[后面](#空标识符)。

对于字符串，range也可以帮你干很多事儿，它可以分析UTF-8并拆分出Unicode代码点。错误的编码则按字节进行读取，并以rune字符U+FFFD展示。（rune这个名字（同时也是一个内置类型）用来表示Go中的一个Unicode代码点。详见[Go语言规范](Go语言规范)。）下面这个循环

```go
for pos, char := range "日本\x80語" { // \x80 is an illegal UTF-8 encoding
    fmt.Printf("character %#U starts at byte position %d\n", char, pos)
}
```

输出

```text
character U+65E5 '日' starts at byte position 0
character U+672C '本' starts at byte position 3
character U+FFFD '�' starts at byte position 6
character U+8A9E '語' starts at byte position 7
```

最后，Go没有逗号操作符，++和--是语句而不是表达式。所以如果你要在for中声明多个变量，那就要用到并行赋值（不让用++和--）。

```go
// 反转a
for i, j := 0, len(a)-1; i < j; i, j = i+1, j-1 {
    a[i], a[j] = a[j], a[i]
}
```

### Switch

Go的switch比C的更加宽泛。它的表达式不需要为常量或者整型，case自上向下一个一个匹配计算，直到找到一个匹配的，如果swtich不带表达式，那就直接是true。因此就可以——也是常见用法——将一串if-else写成switch。

```go
func unhex(c byte) byte {
    switch {
    case '0' <= c && c <= '9':
        return c - '0'
    case 'a' <= c && c <= 'f':
        return c - 'a' + 10
    case 'A' <= c && c <= 'F':
        return c - 'A' + 10
    }
    return 0
}
```

执行流程不会自动向下fall through，但是case可以用逗号间隔写一堆。

```go
func shouldEscape(c byte) bool {
    switch c {
    case ' ', '?', '&', '=', '#', '+', '%':
        return true
    }
    return false
}
```

尽管我们在Go或者其他C风格的语言中不经常在switch中使用break，但还是要说一下它的作用，break可以让switch提前结束。有的时候还需要跳出外层的循环，不光是switch，在Go中就可以给循环加一个标签，然后直接跳到标签处。下面这个栗子展示了这些用法。

```go
Loop:
	for n := 0; n < len(src); n += size {
		switch {
		case src[n] < sizeOne:
			if validateOnly {
				break
			}
			size = 1
			update(src[n])

		case src[n] < sizeTwo:
			if n+1 >= len(src) {
				err = errShortInput
				break Loop
			}
			if validateOnly {
				break
			}
			size = 2
			update(src[n] + src[n+1]<<shift)
		}
	}
```

当然，还有continue也可以带标签，但是只能用在循环中。

最后，我们对比一下两种用switch处理字节slice的方法：

```go
// Compare returns an integer comparing the two byte slices,
// lexicographically.
// The result will be 0 if a == b, -1 if a < b, and +1 if a > b
func Compare(a, b []byte) int {
    for i := 0; i < len(a) && i < len(b); i++ {
        switch {
        case a[i] > b[i]:
            return 1
        case a[i] < b[i]:
            return -1
        }
    }
    switch {
    case len(a) > len(b):
        return 1
    case len(a) < len(b):
        return -1
    }
    return 0
}
```

### 类型Switch

switch还可以用来判断一个接口变量的动态类型。这种*类型swtich（type swtich）*使用类型断言语法，在圆括号中加上type关键字。如果在switch表达式部分中声明了一个变量，变量就会得到对应的类型。这里一般会重用变量名，也就是用同样的名字声明一个变量，然后在不同的case中拥有不同的类型。

```go
var t interface{}
t = functionOfSomeType()
switch t := t.(type) {
default:
    fmt.Printf("unexpected type %T\n", t)     // %T prints whatever type t has
case bool:
    fmt.Printf("boolean %t\n", t)             // t has type bool
case int:
    fmt.Printf("integer %d\n", t)             // t has type int
case *bool:
    fmt.Printf("pointer to boolean %t\n", *t) // t has type *bool
case *int:
    fmt.Printf("pointer to integer %d\n", *t) // t has type *int
}
```

## 函数

### 返回多个值

Go比较特殊的一点就是函数和方法都可以返回多个值。这种形式可以有效缓解C程序中遇到的一些疼痛之处，比如腰间盘突出，脑血栓，冠心病等：“In-Band”错误时返回-1表示EOF，又或者是需要修改一个通过指针传入的参数。

在C里面，写入错误发生时会得到一个负值，而错误码则隐藏在volatile位置中。在Go中，Write可以返回一个计数*和*一个错误：“是的，你写入了一些字节，但由于设备已满，并没有全部写入”。os包中的文件操作Write方法签名如下：

```go
func (file *File) Write(b []byte) (n int, err error)
```

正如我们所说，它返回了写入的字节数，并且当n!=len(b)时返回一个非空的error。这是一种常见的形式；更多内容可以去看错误处理相关的章节。

这种形式还可以避免为了模拟引用参数而返回指针类型。下面这个函数从一个字节slice中计算出一个数，返回这个数以及下一个可以开始计算的位置。

```go
func nextInt(b []byte, i int) (int, int) {
    for ; i < len(b) && !isDigit(b[i]); i++ {
    }
    x := 0
    for ; i < len(b) && isDigit(b[i]); i++ {
        x = x*10 + int(b[i]) - '0'
    }
    return x, i
}
```

你可以用这个函数来扫描某个slice：

```go
    for i := 0; i < len(b); {
        x, i = nextInt(b, i)
        fmt.Println(x)
    }
```

### 返回值命名

Go函数的返回值，或者叫结果“参数”，可以给它们加上命名，当作一个普通的变量来用，就像是一个输入参数一样。加了名字以后，当函数开始执行的时候，就会将它们初始化成对应类型的零值；如果函数中执行了一个不带参数的return语句，则结果参数的当前值就作为返回值。

我们并不强制你命名，但是有了名字可以让代码更加简洁：文档化。比如我们给nextInt的返回值加了命名，那么就很容易看出返回的int是啥玩意。

```go
func nextInt(b []byte, pos int) (value, nextPos int) {
```

因为命名的返回值会进行初始化并绑定到无参的return上，因此它们生而简洁。io.ReadFull就很好的利用了这一点：

```go
func ReadFull(r Reader, buf []byte) (n int, err error) {
    for len(buf) > 0 && err == nil {
        var nr int
        nr, err = r.Read(buf)
        n += nr
        buf = buf[nr:]
    }
    return
}
```

### Defer

Go的defer声明可以用来在函数返回时立即调度执行一个函数（*延迟*函数）。这种形式同样不太常见，但是可以有效的处理例如函数无论如何返回必须释放某些资源的这种场景。典型的场景就是解锁mutext或者关闭文件。

```go
// 将文件内容作为字符串返回。
func Contents(filename string) (string, error) {
    f, err := os.Open(filename)
    if err != nil {
        return "", err
    }
    defer f.Close()  // 结束时调用f.Close。

    var result []byte
    buf := make([]byte, 100)
    for {
        n, err := f.Read(buf[0:])
        result = append(result, buf[0:n]...) // 后面会讲append。
        if err != nil {
            if err == io.EOF {
                break
            }
            return "", err  // 如果在这里return，f会被close掉。
        }
    }
    return string(result), nil // 如果在这里return，f会被close掉。
}
```

延迟执行类似Close这种方法有两个好处。第一，保证你永远不会忘记关闭文件，比如你过两天又来改这个函数，又增加一处可以return的地方，就很可能忘记关闭文件。第二，文件的关闭和打开都离得很近，这样看上去比在函数最后再执行关闭要更加清晰。

延迟函数的参数（如果函数是一个方法的话也包括它的receiver）是在*defer*声明的时候进行计算的，而不是在调用执行的时候。这样就不用担心后面的计算过程是否会对这里的变量进行修改，这样就意味着一处延迟执行实际上可以延迟多个函数的执行。下面的栗子看上去会有些呆逼。

```go
for i := 0; i < 5; i++ {
    defer fmt.Printf("%d ", i)
}
```

延迟函数按照LIFO的顺序执行，因此当函数返回时，上面的栗子会输出4 3 2 1 0。另一个看上去更有用的栗子是跟踪函数的执行。我们可以写两个简单的跟踪过程：

```go
func trace(s string)   { fmt.Println("entering:", s) }
func untrace(s string) { fmt.Println("leaving:", s) }

// 来看:
func a() {
    trace("a")
    defer untrace("a")
    // 在这里搞一些小阴谋....
}
```

因为延迟函数的参数是在defer声明时计算的，我们可以更好的利用这一点。在开始跟踪的时候就可以给结束跟踪的函数设置好参数。这样：

```go
func trace(s string) string {
    fmt.Println("entering:", s)
    return s
}

func un(s string) {
    fmt.Println("leaving:", s)
}

func a() {
    defer un(trace("a"))
    fmt.Println("in a")
}

func b() {
    defer un(trace("b"))
    fmt.Println("in b")
    a()
}

func main() {
    b()
}
```

输出

```text
entering: b
in b
entering: a
in a
leaving: a
leaving: b
```

对于那些在其他语言中习惯了块级别资源管理的程序员来说，defer看上去很奇妙，但它最有趣并且最强大之处就在于它不是块级别的而是函数级别的。在panic和recover的章节中我们会看到更多的相关用法。

## 数据类型

### 用用new

Go有两种分配原语，内置的new和make函数。它们干的事儿不一样，可针对的类型也不一样，这里可能会引起混淆，但其中的规则很简单。我们先来看下new。它是一个内置函数，用来分配内存，但是不要被它的名字蒙蔽，它并不会对内存进行*初始化*，而只是做*零值化*。也就是说，new(T)会分配一个T类型的零值存储，然后返回它的地址，也就是一个*T类型的值。用Go的说法就是，它返回了一个新分配的T类型的零值指针。

因为new返回的内存是零值化的，零值化的类型可以不用进一步初始化直接使用，这样在你设计数据结构的时候就可以更好的组织类型。也就是说用户对这个数据结构new了之后就可以立即使用了。比如bytes.buffer的文档中就提到“Buffer的零值就是一个可以立即使用的空缓冲区。”同样，sync.Mutext也没有一个显式的构造函数或者init方法。sync.Mutex的零值直接被定义成了一个未被锁住的mutex。

零值万岁，这个特点还具有传递性。比如下面的声明。

```go
type SyncedBuffer struct {
    lock    sync.Mutex
    buffer  bytes.Buffer
}
```

SyncedBuffer类型的值可以在分配或是声明后立即使用。在下面的栗子中，p和v都可以不用进一步调整就直接拿来用。

```go
p := new(SyncedBuffer)  // type *SyncedBuffer
var v SyncedBuffer      // type  SyncedBuffer
```

### 构造方法与复合字面量

有的时候零值也不好用，那就需要用到构造方法了，比如下面这个栗子，来自os包。

```go
func NewFile(fd int, name string) *File {
    if fd < 0 {
        return nil
    }
    f := new(File)
    f.fd = fd
    f.name = name
    f.dirinfo = nil
    f.nepipe = 0
    return f
}
```

这里面存在比较多的模板代码。我们可以用*复合字面量（composite literal）*来简化一下，这是一种在声明时创建新实例的表达式。

```go
func NewFile(fd int, name string) *File {
    if fd < 0 {
        return nil
    }
    f := File{fd, name, nil, 0}
    return &f
}
```

注意，这里跟C不一样，这里可以安全的将局部变量的指针作为返回值；变量对应的存储在函数返回后依然存在。实际上，每次取复合字面量的地址时都会分配一个新的实例，因此我们可以把最后两行合并起来。

```go
    return &File{fd, name, nil, 0}
```

复合字面量中的字段要按序排列，并且要把所有字段都列出来。但是如果用*字段名:字段值*这种标签形式，那么顺序就可以随意了，而且也不用全列出来，没出现的回初始化成对应的零值。因此我们可以这样

```go
    return &File{fd: fd, name: name}
```

较少见的情况是一个复合字面量没有任何字段，那么它会为这个类型创建一个零值。所以表达式new(File)和&File{}是等价的。

复合字面量还可以用来创建数组、slice以及map，其中的字段标签则是作为索引或者map的key。在下面的栗子中，尽管Enone、Eio、Einval的值各不相同，但是初始化依然能够进行。（这句话没太看懂，栗子也没明白是要干啥。）

```go
a := [...]string   {Enone: "no error", Eio: "Eio", Einval: "invalid argument"}
s := []string      {Enone: "no error", Eio: "Eio", Einval: "invalid argument"}
m := map[int]string{Enone: "no error", Eio: "Eio", Einval: "invalid argument"}
```

### 用用make

回到关于分配的话题。内置函数make(T, *args*)的作用不同于new(T)。它只能用来创建slice、map以及channel，并且返回的是*初始化的*（而非*零值化*）的T（而非*T）类型的值。之所以有这种不同，是因为这三种类型引用的数据结构都是需要在使用之前进行初始化的。比如slice，它是一个三元描述符，包含了数据（在某个数组中）的指针，长度以及容量，因此只有对它们进行初始化才行，否则这个slice就是个nil。对于它们仨来说，make都会初始化它们内部的数据结构。比如，

```go
make([]int, 10, 100)
```

它分配了一个包含100个int的数组，然后创建了一个slice，长度10，容量100，指向了数组的头10个元素。（构造slice的时候可以忽略容量参数；详见slice相关章节。）相反，new([]int)则是返回了一个新分配的零值slice的指针，也就是一个nil的slice的指针。

下面的栗子展示出了new和make的不同。

```go
var p *[]int = new([]int)       // 分配了一个slice；*p == nil；没什么卵用
var v  []int = make([]int, 100) // slice v引用了一个新分配的数组，包含了100个int

// 脱裤子放屁：
var p *[]int = new([]int)
*p = make([]int, 100, 100)

// 阳春白雪：
v := make([]int, 100)
```

记住make只能用在make、slice和channel上，并且返回的不是指针。要拿指针就用new，要不就显式取地址。

### 数组

数组可以帮助我们规划内存布局的细节，而且有的时候还可以减少多余的分配，但它的作用主要还是支撑slice，也就是下一节的东西。这里我们先打个前站，提两句数组的东西。

数组在Go和C中有几处明显的不同之处。在Go中，

- 数组是值。把一个数组赋值给另一个数组会复制其中所有的元素。
- 如果把数组传给一个函数，函数得到的是数组的一个*副本*，而非指针。
- 数组的长度也是其类型的一部分。因此[10]int和[20]int是不同的类型。

这种值类型也是把双刃剑；如果你想用C风格的那种数组，那你可以创建一个数组的指针。

```go
func Sum(a *[3]float64) (sum float64) {
    for _, v := range *a {
        sum += v
    }
    return
}

array := [...]float64{7.0, 8.5, 9.1}
x := Sum(&array)  // 这里显式取地址了
```

但这种方式也不是典型的Go。用slice吧。

## 空标识符

前面在讲[For循环](#For循环)和[Map]()的时候已经提到过空标识符了。