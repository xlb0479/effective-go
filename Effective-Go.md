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
&ensp;&ensp;[Slice](#Slice)<br/>
&ensp;&ensp;[二维Slice](#二维Slice)<br/>
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
&ensp;&ensp;[泛型](#泛型)<br/>
&ensp;&ensp;[接口和方法](#接口和方法)<br/>
[空标识符](#空标识符)<br/>
&ensp;&ensp;[多变量赋值中的空标识符](#多变量赋值中的空标识符)<br/>
&ensp;&ensp;[无用的import和变量](#无用的import和变量)<br/>
&ensp;&ensp;[利用import的副作用](#利用import的副作用)<br/>
&ensp;&ensp;[接口检查](#接口检查)<br/>
[内嵌](#内嵌)<br/>
[并发](#并发)<br/>
&ensp;&ensp;[利用通信来共享数据](#利用通信来共享数据)<br/>
&ensp;&ensp;[Go协程](#Go协程)<br/>
&ensp;&ensp;[香奈儿](#香奈儿)<br/>
&ensp;&ensp;[香奈儿嵌套](#香奈儿嵌套)<br/>
&ensp;&ensp;[并行](#并行)<br/>
&ensp;&ensp;[缓冲漏桶](#缓冲漏桶)<br/>
[错误](#错误)<br/>
&ensp;&ensp;[慌的一批](#慌的一批)<br/>
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

### Slice

Slice封装了数组，为数据序列提供了更加通用、强大且便捷的接口。除了需要显式多维计算，比如矩阵转换，Go中的大部分数组处理都可以用slice来代替。

Slice持有的是底层数组的引用，如果你把一个slice赋值给另一个，那么它俩引用的是同一个数组。如果一个函数接收一个slice参数，函数内对slice元素的更新就会传播到函数外部，就跟传递数组的指针是一样的。像是Read这种函数，它就可以接收一个slice参数，而不是一个指针加一个计数；slice中的长度可以限制要读取的数据量。下面是os包中File类型的Read方法签名：

```go
func (f *File) Read(buf []byte) (n int, err error)
```

这个方法返回读取的字节数量以及一个错误信息。如果要想把数据读到buf的头32个字节中，那就做一下slice（这里用作动词了，是不是很巧妙呀，嘻嘻）。

```go
    n, err := f.Read(buf[0:32])
```

这种切片很常用。那我们暂且不谈效率，下面这段代码也能达到同样的效果。

```go
    var n int
    var err error
    for i := 0; i < 32; i++ {
        nbytes, e := f.Read(buf[i:i+1])  // 读了一字节。
        n += nbytes
        if nbytes == 0 || e != nil {
            err = e
            break
        }
    }
```

只要不超出底层数组的限制，可以各种修改slice的长度；改完之后赋值给自己就好了。slice的*容量（capacity）*可以用内置函数cap来得到，它返回slice可以接受的最大长度。下面这个函数可以往slice里面追加数据。如果数据超限，slice会被重新分配。最后返回最终的slice。这个函数还彰显了一处发人深省的特点，那就是len和cap函数可以用在slice等于nil的场景中，并且返回0。

```go
func Append(slice, data []byte) []byte {
    l := len(slice)
    if l + len(data) > cap(slice) {  // reallocate
        // Allocate double what's needed, for future growth.
        newSlice := make([]byte, (l+len(data))*2)
        // The copy function is predeclared and works for any slice type.
        copy(newSlice, slice)
        slice = newSlice
    }
    slice = slice[0:l+len(data)]
    copy(slice[l:], data)
    return slice
}
```

最后必须要把slice返回，因为尽管Append函数可以修改slice的元素，但slice本身（运行时数据结构，保存了指针、长度以及容量）是值传递的。

slice追加数据的场景太多了，因此我们提供了内置的append函数。要想理解其中的奥妙，我们需要提前准备更多的知识，后面我们会回过头来继续讲这块的东西。

### 二维Slice

Go的数组和slice都是一维的。如果要想闹一个二维的，那就要搞一个数组的数组，或者slice的slice，比如：

```go
type Transform [3][3]float64  // 三乘三的数组，数组的数组。
type LinesOfText [][]byte     // 字节slice的slice。
```

因为slice是变长的，所以内部的每个slice的长度都有可能不一样。这种情况也是很常见的，比如上面的栗子里，LinesOfText的每一行都有自己单独的长度。

```go
text := LinesOfText{
	[]byte("Now is the time"),
	[]byte("for all good gophers"),
	[]byte("to bring some fun to the party."),
}
```

很多时候都需要用到二维slice，比如要按行扫描像素。有两种方法可以实现。一个就是单独分配每一行的slice；另一种是分配一个单独的数组，然后在里面划分每一个slice。具体怎么用还是要看你的场景。如果slice会扩缩，那就要单独分配，避免相互覆盖；如果没有扩缩，那就可以都涵盖在一次分配中。为了便于理解，这里我们大概写了一下。首先是单独分配法：

```go
// 分配顶层slice。
picture := make([][]uint8, YSize) // y行。
// 循环每一行，为每一行单独分配。
for i := range picture {
	picture[i] = make([]uint8, XSize)
}
```

然后是统一分配再做slice：

```go
// 分配顶层slice，跟上面一样。
picture := make([][]uint8, YSize) // y行。
// 分配一个用来包含所有像素的大slice。
pixels := make([]uint8, XSize*YSize) // 类型为[]uint8。
// 循环每一行，基于每次剩余的slice划分新的slice。
for i := range picture {
	picture[i], pixels = pixels[:XSize], pixels[XSize:]
}
```

### Map

Map可是个方便快捷的数据结构，它能把某种类型的值（*key*）和另外某种类型的值（*element或叫value*）关联起来。key可以是任意的类型，只要能够对它们进行相等比较即可，比如整数、浮点数、复数、字符串、指针、接口（只要其动态类型能进行相等判断即可）、结构体、数组。Slice不能作为map的key，因为它们无法进行相等判断。和slice一样，map也是引用了一个底层的数据结构。如果你把map传给了一个函数，这个函数里又更改了map的内容，那么这种更改也会传播给函数的调用者。

可以用常规的复合字面量语法来创建map，即逗号间隔的键值对形式，所以在初始化的时候就可以很方便的进行构建。

```go
var timeZone = map[string]int{
    "UTC":  0*60*60,
    "EST": -5*60*60,
    "CST": -6*60*60,
    "MST": -7*60*60,
    "PST": -8*60*60,
}
```

Map数据的赋值和获取方式也是很讲道理的，跟数组和slice的方式差不多，只不过索引不用非得是整数。

```go
offset := timeZone["EST"]
```

如果尝试从map中获取一个不存在的key的值，那就会返回对应类型的零值。比如当map包含的是整数，访问不存在的key，就会返回0。可以用map来构建一个集合，映射的值为bool类型。给集合设置值的时候，对应的value就设置为true，然后就可以用索引的方式来进行测试了。

```go
attended := map[string]bool{
    "Ann": true,
    "Joe": true,
    ...
}

if attended[person] { // 如果不存在则返回false。
    fmt.Println(person, "was at the meeting")
}
```

喏，有的时候你需要区分出来到底是key不存在还是真的是零值。到底是不存在才返回的0，还是说本身就是要返回0？你可以用多重赋值的形式来进行判断。

```go
var seconds int
var ok bool
seconds, ok = timeZone[tz]
```

出于显而易见的理由，这种调用方式被称为“逗ok”。此处，如果tz存在，seconds会被赋予相应的值，ok则是true；如果不存在，seconds会得到一个零值，而ok则是false。这里我们再整合一下，并加好错误提示：

```go
func offset(tz string) int {
    if seconds, ok := timeZone[tz]; ok {
        return seconds
    }
    log.Println("unknown time zone:", tz)
    return 0
}
```

如果只是想看看有没有值，而不想真的取值，那就可以用[空标识符](#空标识符)来替换掉值的位置。

```go
_, present := timeZone[tz]
```

如果要删除一个条目，可以使用内置的delete函数，参数为map以及要删除的key。即便key已经不存在了，依然可以正常调用。

```go
delete(timeZone, "PDT")
```

### Print

Go的格式化输出方式采用了跟C的printf系列函数类似的方法，但是更加强大且通用。这些函数都位于fmt包中，并且开头字母大写：fmt.Printf、fmt.Fprintf、fmt.Sprintf等等。其中的字符串输出（Springf等）函数返回的是一个字符串，而不是填充某个预设的缓冲。

当然也不是总得有什么格式。比如Printf、Fprintf和Sprintf，它们都有对应的另一种函数形式，比如Print和Println。这些函数都不需要格式参数，而是按默认格式进行输出。Println还会给每个参数之间加一个空格，然后再末尾加上换行，而print则只有在两边都没有字符串的情况下才会添加空格。下面的例子中，每一行的输出都是一样的。

```go
fmt.Printf("Hello %d\n", 23)
fmt.Fprint(os.Stdout, "Hello ", 23, "\n")
fmt.Println("Hello", 23)
fmt.Println(fmt.Sprint("Hello ", 23))
```

格式化函数fmt.Fprint的第一个参数要求实现io.Writer接口；常见的就是os.Stdout和os.Stderr。

然后，一切开始变得跟C就不太一样了。首先，数字格式，例如%d，不接受符号标识以及长度标识；输出的时候会根据参数的类型来决定这些属性。

```go
var x uint64 = 1<<64 - 1
fmt.Printf("%d %x; %d %x\n", x, x, int64(x), int64(x))
```

输出

```go
18446744073709551615 ffffffffffffffff; -1 -1
```

如果你想用默认的格式转换策略，比如整数的浮点格式，你可以用通用的%v格式（即value的缩写）；Print和Println实际上就是在用这种输出格式。这种格式可以输出*任意*的值，甚至是数组、slice、结构体以及map。下面我们输出一下上面定义过的map。

```go
fmt.Printf("%v\n", timeZone)  // 用fmt.Println(timeZone)也行
```

输出：

```go
map[CST:-21600 EST:-18000 MST:-25200 PST:-28800 UTC:0]
```

对于map数据，Printf及其同门，在输出的时候按key的字母排序。

输出struct类型的时候，%+v可以同时输出字段名，%#v则是按照Go语法进行完整输出。

```go
type T struct {
    a int
    b float64
    c string
}
t := &T{ 7, -2.35, "abc\tdef" }
fmt.Printf("%v\n", t)
fmt.Printf("%+v\n", t)
fmt.Printf("%#v\n", t)
fmt.Printf("%#v\n", timeZone)
```

输出

```go
&{7 -2.35 abc   def}
&{a:7 b:-2.35 c:abc     def}
&main.T{a:7, b:-2.35, c:"abc\tdef"}
map[string]int{"CST":-21600, "EST":-18000, "MST":-25200, "PST":-28800, "UTC":0}
```

（注意其中的取地址符。）当我们输出string或者[]byte类型的时候，可以用%q输出带双引号的字符串格式。用%#q则是换成了反引号（`）。（%q也可以用在整数和rune上，输出的是单引号的rune常量。）此外，%x可以用在字符串、字节数组、字节slice以及整数上，输出对应的十六进制字符串，如果标记中加入一个空格（% x），则输出的时候会在每个字节中间加一个空格。

另一种常用的标记是%T，它输出对应参数所属的*类型*。

```go
fmt.Printf("%T\n", timeZone)
```

输出

```go
map[string]int
```

如果你想调整自定义类型的输出格式，可以为该类型添加一个签名为String() string的方法。比如一个简单的类型T，就可以这样。

```go
func (t *T) String() string {
    return fmt.Sprintf("%d/%g/%q", t.a, t.b, t.c)
}
fmt.Printf("%v\n", t)
```

输出

```go
7/-2.35/"abc\tdef"
```

（如果你想让T的*值*和指针输出一样，那么上面的String方法的接收者必须得是值类型；这里我们用的是指针接收，因为这种形式最高效并且最富个struct类型的使用习惯。详见下面的[指针与值](#指针与值)。）

我们的String方法内可以调用Sprintf，因为输出过程是完全可重入的才能这么调用。这里有一点必须要注意：String方法中调用Sprintf的时候不要出现递归调用String方法的情况。当Sprintf尝试直接输出接收者的值的时候就会出现这种情况，会出现递归调用。这种错误很常见，比如下面这样。

```go
type MyString string

func (m MyString) String() string {
    return fmt.Sprintf("MyString=%s", m) // 错误：这里会出现递归。
}
```

想改也很简单：把参数转换成基本的字符串类型，它没有String方法。

```go
type MyString string
func (m MyString) String() string {
    return fmt.Sprintf("MyString=%s", string(m)) // 可以可以。
}
```

在[初始化](#初始化)一节中会介绍另一种方式。

另一种输出的技巧是将输出的参数传给另一个输出函数。Printf函数的签名中先是格式参数，然后将...interface{}放在最后，即可以接受任意多个参数（任意类型）。

```go
func Printf(format string, v ...interface{}) (n int, err error) {
```

在Printf函数内，v类似一个[]interface{}类型的变量，但是又将它传给了另一个变参函数，看着就像是传递了一个参数列表。下面就是我们用到的log.Println方法的实现。它把它的参数直接传给了fmt.Sprintln来完成实际的格式化输出。

```go
// Println用fmt.Println的风格输出到标准的日志中
func Println(v ...interface{}) {
    std.Output(2, fmt.Sprintln(v...))  // Output 需要 (int, string)
}
```

调用Sprintln的时候，我们在v后面加了...就是告诉编译器要把v当作一个参数列表；否则只会将v作为一个slice参数传进去。

关于输出还有好多奇技淫巧。详见fmt包的godoc文档。

对了，点点点参数是可以声明具体类型的，比如一个求最小值函数的参数就可以是...int。

```go
func Min(a ...int) int {
    min := int(^uint(0) >> 1)  // int最大值
    for _, i := range a {
        if i < min {
            min = i
        }
    }
    return min
}
```

### Append

现在我们来填补一下关于内置函数append的空白。append跟我们上面自己定义的Append的签名不一样。它基本上是这个样子：

```go
func append(slice []T, elements ...T) []T
```

其中的T是一个占位符，可以是任何类型。Go中是不允许存在这种由调用者决定T的具体类型的函数的。这也就是为什么append是一个内置函数，它需要编译器的支持。

append干的事儿就是把元素追加到slice后面，然后返回结果。这里必须要返回结果，正如我们自己写的那个Append，因为底层数组是有可能发生变化的。下面这个

```go
x := []int{1,2,3}
x = append(x, 4, 5, 6)
fmt.Println(x)
```

输出[1 2 3 4 5 6] 。这么看的话，append有点像Printf，都是可以接受任意多个参数。

但是我们自己写的Append可以把一个slice追加到另一个slice后面，这里该怎么弄呢？很简单：用点点点参数，就跟我们上面写的调用Output一样。下面的栗子跟上面的输出是一样的。

```go
x := []int{1,2,3}
y := []int{4,5,6}
x = append(x, y...)
fmt.Println(x)
```

如果没有点点点则无法通过编译，因为出现了类型错误；y不是int类型。

## 初始化

关于初始化的东西，尽管看上去和C、C++的初始化大差不差，但是它在Go里面可是很强大的。复杂类型可以在初始化过程中完成构建，其中的初始化对象的顺序，甚至是跨越多个不同的包，都可以被正确的处理。

### 常量

Go里面的常量就是……常量，嘿嘿。它们是在编译时创建，即便是在函数中作为局部对象，常量只能是数字、字符（rune）、字符串或者bool类型。由于编译时的限制，用来定义常量的表达式也必须是常量表达式，由编译器来完成计算。比如1<<3就是一个常量表达式，而math.Sin(math.Pi/4)则不是，因为math.Sin这种函数调用只能发生在运行时。

在Go中，可以用iota枚举器来创建枚举常量。因为iota可以作为表达式的一部分，而表达式可以隐式重复执行，这样就可以轻而易举创建一些非常复杂的集合。

```go
type ByteSize float64

const (
    _           = iota // 用空标识符来忽略第一个值
    KB ByteSize = 1 << (10 * iota)
    MB
    GB
    TB
    PB
    EB
    ZB
    YB
)
```

因为可以将String这种方法加给任意的类型，那就是说这些类型在输出的时候都可以自动的实现格式化。尽管这个方法一般是加给struct类型，但是加给一些标量类型也是有用的，比如ByteSize这种浮点类型。

```go
func (b ByteSize) String() string {
    switch {
    case b >= YB:
        return fmt.Sprintf("%.2fYB", b/YB)
    case b >= ZB:
        return fmt.Sprintf("%.2fZB", b/ZB)
    case b >= EB:
        return fmt.Sprintf("%.2fEB", b/EB)
    case b >= PB:
        return fmt.Sprintf("%.2fPB", b/PB)
    case b >= TB:
        return fmt.Sprintf("%.2fTB", b/TB)
    case b >= GB:
        return fmt.Sprintf("%.2fGB", b/GB)
    case b >= MB:
        return fmt.Sprintf("%.2fMB", b/MB)
    case b >= KB:
        return fmt.Sprintf("%.2fKB", b/KB)
    }
    return fmt.Sprintf("%.2fB", b)
}
```

直接输出YB，那就是输出1.00YB，ByteSize(1e13)则会输出9.09TB。

这里之所以用Sprintf来实现ByteSize的String方法（避免无限递归），不光是做了转换的原因，还有就是它调用Sprintf的时候用了%f，而它不是字符串格式：Sprintf只有在需要字符串的时候才会调用String方法，而这里它需要的是一个浮点值。

### 变量

变量也可以像常量内种初始化，但是初始化器可以是一个一般的表达式，在运行时计算。

```go
var (
    home   = os.Getenv("HOME")
    user   = os.Getenv("USER")
    gopath = os.Getenv("GOPATH")
)
```

### init函数

最后，每一个源文件中都可以定义无参的init函数，用来初始化它需要的各种状态。（实际上每个文件可以有多个init函数。）我们说最后，是因为它真的是最后：init函数要等到该包中所有的变量完成初始化后才会被调用，而在这之前，还要等所有被导入的包完成初始化。

初始化无法表达成声明（我去，这句没明白啥意思），init函数一般是用来确认或者调整程序开始执行前的状态是否正确。

```go
func init() {
    if user == "" {
        log.Fatal("$USER not set")
    }
    if home == "" {
        home = "/home/" + user
    }
    if gopath == "" {
        gopath = home + "/go"
    }
    // 可以用命令行的-gopath选项来覆盖gopath的值。
    flag.StringVar(&gopath, "gopath", gopath, "override default GOPATH")
}
```

## 方法

### 指针与值

正如我们前面对ByteSize的所作所为，可以为任意的（除了指针和接口）命名类型定义方法；接收者不是非得是struct。

上面我们讨论slice的时候，我们写了一个Append函数。我们可以把它定义成slice的一个方法。首先我们需要声明一个命名类型才能把方法绑上去，然后我们把方法的接收者改成对应的类型。

```go
type ByteSlice []byte

func (slice ByteSlice) Append(data []byte) []byte {
    // 这里跟之前写的Append函数体一样就行了。
}
```

这里我们仍然需要返回更新后的slice。我们可以重构一下，避免这种尴尬，把接收者改成ByteSlice的*指针*，这样方法就可以修改调用者的slice了。

```go
func (p *ByteSlice) Append(data []byte) {
    slice := *p
    // 这里跟之前的一样，只不过不用返回了。
    *p = slice
}
```

我们还可以改的更好。我们可以按照标准的Write方法风格进一步调整，这样，

```go
func (p *ByteSlice) Write(data []byte) (n int, err error) {
    slice := *p
    // 这里还是不变。
    *p = slice
    return len(data), nil
}
```

这样的话，*ByteSlice就满足了标准接口io.Writer，用起来就方便多了。比如，我们可以把数据输出到里面。

```go
    var b ByteSlice
    fmt.Fprintf(&b, "This hour has %d days\n", 7)
```

我们传的是ByteSlice的地址，因为只有*ByteSlice才满足io.Writer接口。选择指针还是值作为接收者，这里的规则是，值方法可以被指针和值调用，而指针方法只能被指针调用。

这是因为指针方法可以修改它的调用者；如果用值来调用，那方法的接收者实际上是该值的一个副本，因此所有的改动都会被丢弃。因此在语言层面上就避免了这种错误。这里我们特意提供了一些语法糖。如果值是可以取地址的，那么语言本身知道这是一种常见的情况，用值调用了指针方法，它会自动添加取地址操作符。在上例中，变量b是可以取地址的，因此我们可以直接用b.Write来调用它的Write方法。编译器会替我们把它重写成(&b).Write。

顺便说一下，bytes.Buffer的中心思想就是在字节slice上调用Write。

## 接口和其他类型

### 接口

Go的接口用来定义对象的行为：如果它能这么干，那就可以这么用。前面我们已经见识了几个栗子；可以实现String方法来自定义输出，Fprintf则可以将输出内容写入到带有Write方法的对象上。Go里面经常会有只包含一两个方法的接口，接口名字通常也是由方法名来决定，比如Write方法的接口名就是io.Writer。

一个类型可以实现多个接口。比如一个集合如果实现了sort.Interface，那就可以用sort包中的工具进行排序，其中包含Len()、Less(i, j int) bool、以及Swap(i, j int)，而且还可以加一个自定义的格式化输出函数。下面特意搞了一个戏精附体的Sequence，满足以上所有需要。

```go
type Sequence []int

// sort.Interface需要的方法。
func (s Sequence) Len() int {
    return len(s)
}
func (s Sequence) Less(i, j int) bool {
    return s[i] < s[j]
}
func (s Sequence) Swap(i, j int) {
    s[i], s[j] = s[j], s[i]
}

// 返回一个Sequence副本。
func (s Sequence) Copy() Sequence {
    copy := make(Sequence, 0, len(s))
    return append(copy, s...)
}

// 格式化输出——排序后输出。
func (s Sequence) String() string {
    s = s.Copy() // 别动已有的，先搞个副本出来。
    sort.Sort(s)
    str := "["
    for i, elem := range s { // 这里是O(N²)；后面我们会优化。
        if i > 0 {
            str += " "
        }
        str += fmt.Sprint(elem)
    }
    return str + "]"
}
```

### 类型转换

Sequence的String方法覆盖了Sprint原本对slice的输出方式。（但实现成了O(N²)，效率太差。）我们可以把任务分摊一下（同时也是提高效率），我们在调用Sprint前把Sequence转换成一个普通的[]int。

```go
func (s Sequence) String() string {
    s = s.Copy()
    sort.Sort(s)
    return fmt.Sprint([]int(s))
}
```

该方法同时也展示了另一种在String方法中安全调用Sprintf的栗子。如果我们不看名字的话，实际上两种类型（Sequence和[]int）是同一种类型，在它们之间做转换也是可以的。转换本省并不会创建新的值出来，它只是临时性的让当前值扮演了一个新的类型。（其他的一些合法转换则会创建新的值，比如整数转浮点数。）

在Go中经常会将一个表达式的类型转换成另一个，从而访问到不通的方法。比如我们可以用sort.IntSlice来简化上面的栗子：

```go
type Sequence []int

// 格式化输出——排序后输出。
func (s Sequence) String() string {
    s = s.Copy()
    sort.IntSlice(s).Sort()
    return fmt.Sprint([]int(s))
}
```

现在我们就不用让Sequence实现多个接口了（排序和输出接口），因为我们可以将数据转换成不通的类型（Sequence、sort.IntSlice以及[]int），每种类型都可以承担一部分工作。实际开发中可能并不常用，但的确高效。

### 接口转换与类型断言

[类型swtich](#类型Switch)也是转换的一种形式：它接收一个接口，然后在每个case中转换成对应的类型。下面是一个简化版的栗子，展示了fmt.Printf是如何用类型switch把一个值转换成字符串的。如果它已经是一个字符串了，那我们要的就是接口持有的真实的字符串，如果它有一个String方法，那我们要的就是调用该方法返回的结果。

```go
type Stringer interface {
    String() string
}

var value interface{} // 调用者提供的value。
switch str := value.(type) {
case string:
    return str
case Stringer:
    return str.String()
}
```

第一个case拿到了具体的值；第二个case把当前接口转换成了另一个接口。这种类型混用的方式是完全ok的。

如果我们只关心某一种类型怎么办？假如我们现在知道这个值就是一个字符串，我们想得到它该怎么弄？可以用单case类型swtich，但也可以使用*类型断言*。类型断言需要一个接口值，然后按照显式声明的类型把里面的值拽出来。这种语法借用了类型switch开头的部分，但用的是显式类型，而不是一个type关键字：

```go
value.(typeName)
```

其结果就是指定静态类型typeName的一个新值。这个新的类型必须要么是接口真实对应的类型，要么是这个值可以进行正确转换的一个类型。比如我们已经知道value就是个字符串，想把里面的值拽出来，那就这样：

```go
str := value.(string)
```

但如果真实情况并非我们所想，value没有字符串，程序就会崩溃，报出运行时错误。要想提高健壮性，我们可以用“逗OK”语法来保护一下，判断value到底是不是字符串：

```go
str, ok := value.(string)
if ok {
    fmt.Printf("string value is: %q\n", str)
} else {
    fmt.Printf("value is not a string\n")
}
```

如果类型断言失败，str依然存在，并且类型是字符串，但是个零值字符串，空串。

为了诠释它的作用，我们把上面的类型switch改成了下面这种if-else语句。

```go
if str, ok := value.(string); ok {
    return str
} else if str, ok := value.(Stringer); ok {
    return str.String()
}
```

### 泛型

如果一个类型只需要实现一个接口，并且除了接口定义的方法外没有其他需要导出的方法，那这个类型本身也就没必要导出了。只导出接口就行，这样就很清晰的表明除了接口声明的东西以外没有什么其他的东西了。这样还可以避免在每个接口的实现方法上都加一段重复的注释文档。

此时，构造方法要返回的就应该是接口值而不是具体的实现类型。比如在哈希库中，crc32.NewIEEE和adler32.New返回的都是hash.Hash32这个接口类型。如果要在Go程序中把CRC-32算法改成Adler-32算法，只需要改一下调用的构造方法即可；修改算法并不会影响已有的代码。

同样的方式在crypto包中也经常出现，使得分组密码和流密码既有关联又能加以区分。crypto/cipher包的Block接口定义了一个分组密码的行为，它只提供对一组密码进行加密。然后，对比bufio包可以发现，实现了这个接口的密码可以用来构建流密码，也就是Stream接口定义的行为，而这个过程中并不用操心底层分组密码的实现方式。

crypto/cipher接口：

```go
type Block interface {
    BlockSize() int
    Encrypt(dst, src []byte)
    Decrypt(dst, src []byte)
}

type Stream interface {
    XORKeyStream(dst, src []byte)
}
```

下面定义了计数模式（CTR）流，它将一个分组密码转换成了一个流密码；注意这里分组密码的细节被抽象出去了：

```go
// NewCTR returns a Stream that encrypts/decrypts using the given Block in
// counter mode. The length of iv must be the same as the Block's block size.
func NewCTR(block Block, iv []byte) Stream
```

NewCTR的使用场景并不受特定加密算法和数据源的限制，只要实现了Block和Stream接口即可。因为它返回的是接口值，因此变换CTR加密模式只会产生小范围的影响。到时候肯定要改构造方法，但是其他的部分因为用的都是Stream，所以都是无感知的。

### 接口和方法

基本上啥玩意都会有几个方法，所以基本上啥玩意都能满足某个接口。http包中就有这么个栗子，它定义了一个Handler接口。所有实现了Handler的对象都可以用来处理HTTP请求。

```go
type Handler interface {
    ServeHTTP(ResponseWriter, *Request)
}
```

ResponseWriter也是个接口，提供了用来为客户端生成返回结果的方法。比如Write方法，这样，能用io.Writer的地方就能用http.ResponseWriter。Request是个struct，包含了客户端请求被解析后的形式。

我们讲的简单一点，不考虑POST，假设HTTP请求都是GET；这种简化不影响handler的构造方式。下面的栗子做了一个PV统计。

```go
// 简易计数服务。
type Counter struct {
    n int
}

func (ctr *Counter) ServeHTTP(w http.ResponseWriter, req *http.Request) {
    ctr.n++
    fmt.Fprintf(w, "counter = %d\n", ctr.n)
}
```

（跟上思路，想想为什么Fprintf可以用http.ResponseWriter进行输出。）在真正的服务中，访问ctr.n是要做并发安全的。这部分知识可以去看sync和atomic包。

为了完整一点，下面把这个服务关联到一个URL路径上。

```go
import "net/http"
...
ctr := new(Counter)
http.Handle("/counter", ctr)
```

那么为什么要把Counter实现成一个struct呢？用整数就好了呀。（接收者得是指针，这样做的更新才能对调用者可见。）

```go
// 简易计数服务。
type Counter int

func (ctr *Counter) ServeHTTP(w http.ResponseWriter, req *http.Request) {
    *ctr++
    fmt.Fprintf(w, "counter = %d\n", *ctr)
}
```

如果页面被访问的时候需要通知到一些内部状态该怎么办？那就绑一个channel。

```go
// 每次访问都会给channel发一个通知。
// （channel可能需要加缓冲。）
type Chan chan *http.Request

func (ch Chan) ServeHTTP(w http.ResponseWriter, req *http.Request) {
    ch <- req
    fmt.Fprint(w, "notification sent")
}
```

最后，假设我们想把启动服务进程时设置的命令行参数展示在/args页面上。很简单，写一个函数输出参数即可。

```go
func ArgServer() {
    fmt.Println(os.Args)
}
```

然后怎么变成一个HTTP服务捏？我们可以把ArgServer弄成某个类型的方法，可以不去关心类型具体的值，但是还有更简单的方法。因为除了指针和接口，我们还可以给其他各种类型加上方法，没错，我们还可以给函数定义一个方法。http包里就包含这样的代码：

```go
// HandlerFunc类型是一个适配器，可以让普通的函数来作为HTTP处理器。比如函数f具有匹配的签名，那么HandlerFunc(f)就是一个可以调用f的处理器对象。
type HandlerFunc func(ResponseWriter, *Request)

// ServeHTTP调用f(w, req)。
func (f HandlerFunc) ServeHTTP(w ResponseWriter, req *Request) {
    f(w, req)
}
```

这样的HandlerFunc就是一个有ServeHTTP方法的类型，所以对应的值就可以用来处理HTTP请求。来看下这个方法的实现：接收者是一个函数，f，然后方法会调用f。看上去可能有点奇怪，但是也见怪不怪了，比如上面的栗子中，接收者是一个channel然后让方法往channel上发东西。

现在我们要把ArgServer改成一个HTTP服务，先调整它的签名。

```go
// 参数服务。
func ArgServer(w http.ResponseWriter, req *http.Request) {
    fmt.Fprintln(w, os.Args)
}
```

ArgServer现在和HandlerFunc的签名就一致了，此时就可以转换成这种类型然后访问它的方法了，就跟我们之前把Sequence转成IntSlice然后访问IntSlice.Sort一样。最后要跑起来就是这样：

```go
http.Handle("/args", http.HandlerFunc(ArgServer))
```

当访问/args页面的时候，部署在这个页面的处理器的值是ArgServer，类型是HandlerFunc。HTTP服务会调用该类型的ServeHTTP方法，并以ArgServer作为接收者，然后它会反过来调用ArgServer（通过HandlerFunc.ServeHTTP内部的f(w, req)来调用）。然后参数就展示出来了。

在本节中，我们用struct、整数、channel、函数都实现了HTTP服务，这一切都是因为接口的本质就是一组方法，（几乎）任何类型都可以定义这些方法。

## 空标识符

前面在讲[For循环](#For循环)和[Map](#Map)的时候已经提到过空标识符了。可以给空标识符赋予任意类型的任意值，这些值都会被安全的丢弃。这跟往Unix的/dev/null文件写数据差不多：我们把它当作一个只可以写入的占位符，该位置上我们需要一个变量，但是我们毫不关心它的值是什么。除了我们上面讲过的栗子，它还有其他的用法。

### 多变量赋值中的空标识符

在for range循环中使用空标识符属于一种特殊用法，总会伴随着一种常见的场景：多变量复制。

如果一个赋值语句左边需要多个变量，但是其中某个变量的值我们用不上，那我们就可以用空标识符来代替这个变量，避免创建了一个傻乎乎的变量，而且看上去也很清晰，变量的值被丢弃了。比如调用的函数返回一个值和一个错误，但我们只关心错误信息，那就用空标识符把值扔了。

```go
if _, err := os.Stat(path); os.IsNotExist(err) {
	fmt.Printf("%s does not exist\n", path)
}
```

有的时候你可能会看到故意丢弃错误信息的代码；这是一种令人发指的行为。一定要检查错误；它们报出来都是有原因的。

```go
// 不好不好！如果路径不存在就全完了。
fi, _ := os.Stat(path)
if fi.IsDir() {
    fmt.Printf("%s is a directory\n", path)
}
```

### 无用的import和变量

导了包不用，或者声明了变量不用，这种都是会报错的。无用的导入会让程序显得肿胀并且降低编译的速度，而如果是变量已经初始化了但是又不用，至少是浪费了一定的计算资源，而且可能还是一种潜在的八哥。当一个程序处于快速开发阶段时，可能会经常出现无用的导入和变量声明，而且为了让程序正常编译去删除这些无用操作也容易让人非常烦躁，失去了年轻的生命，而且后面有可能又用到了，还得加回来。这时我们的空标识符就派上用场了。

下面这个没写完的程序又两处无用的导入（fmt和io），一处无用的变量（fd），所以肯定是编译不通的，但如果能看出当前的代码是否有什么真正的问题当然才是最好的。

```go
package main

import (
    "fmt"
    "io"
    "log"
    "os"
)

func main() {
    fd, err := os.Open("test.go")
    if err != nil {
        log.Fatal(err)
    }
    // TODO: 使用fd。
}
```

如果想正常编译，那就用空标识符引用被导入包中的某个标记即可。同样，还可以把fd赋值给空标识符。下面这个程序就可以正常编译了。

```go
package main

import (
    "fmt"
    "io"
    "log"
    "os"
)

var _ = fmt.Printf // 调试用，不用了就删了。
var _ io.Reader    // 调试用，不用了就删了。

func main() {
    fd, err := os.Open("test.go")
    if err != nil {
        log.Fatal(err)
    }
    // TODO: 使用fd。
    _ = fd
}
```

习惯上我们要把对无用导入的抑制紧跟在导入声明的后面而且要加上注释，这样可以快速定位，并且留下提示记得清理。

### 利用import的副作用

上面我们提到的无用的导入，比如imt和io，最终还是要被用到，或者删掉：这些地方的空标识符往往意味着一些要做但还未做的事情。但有的时候导入一个包可能只是为了享受它带来的额外好处，而不是直接的加以利用。比如在执行init函数的时候，net/http/pprof包会注册一些能够提供调试信息的HTTP处理器。它还包括一个导出的API，但大部分客户端只需要这个处理器注册上就可以了，然后从web页面访问它的数据。如果只是为了享受这些额外的小福利，那就可以使用空标识符：

```go
import _ "net/http/pprof"
```

这样就可以表明此处导入是为了利用它的副作用，因为没有什么其他地方需要用到它：此处，它没名字。（如果加了名字，而我们又不用，那编译就报错了。）

### 接口检查

我们上面讲[接口和其他类型](#接口和其他类型)的时候提到，类型不需要显式声明它所实现的接口。类型只需要实现接口的方法即可。实际上大部分接口转换都是静态的，因此会在编译时进行检查。比如把\*os.File传给了一个需要io.Reader的函数，编译就会报错，除非\*os.File实现了io.Reader接口。

但也有一些接口检查是发生在运行时的。比如encoding/json包中的Marshaler接口。当一个JSON编码器收到一个实现该接口的值时，编码器要调用它的序列化方法来把它转成一个JSON，而不是用标准的类型转换。编码器会使用[类型断言](#接口转换与类型断言)在运行时进行检查：

```go
if _, ok := val.(json.Marshaler); ok {
    fmt.Printf("value %v of type %T implements json.Marshaler\n", val, val)
}
```

这个例子就是说有的时候还是要确保类型的确实现了某个接口。如果一个类型——比如json.RawMessage——需要自定义的JSON格式，那它就要实现json.Marshaler，但是没有哪种静态转换能让编译器来对它进行验证。如果类型恰巧无法满足接口定义，JSON编码器仍能正常工作，但是不会使用自定义的实现方式。要保证实现正确，可以用空标识符做一个全局声明：

```go
var _ json.Marshaler = (*RawMessage)(nil)
```

这样，这个赋值语句就要将\*RawMessage转成Marshaler，\*RawMessage就要实现Marshaler，而且这件事会在编译时进行检查。一旦json.Marshaler接口发生变化，那这个包就无法通过编译，我们就会知道此时需要对实现进行更新了。

## 内嵌

Go并没有提供那种典型的基于类型驱动的继承模型，但是它依然有能力“借用”一部分实现，那就是在struct或接口中使用*内嵌*。

接口内嵌很简单。我们上面提到过io.Reader和io.Writer；下面是它们的定义。

```go
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}
```

io包还导出了一些其他的接口，它们所定义的对象可以实现多个此类方法。比如有一个io.ReadWriter，这个接口包含了Read和Write。在io.ReadWriter中我们可以把这两个方法都加进去，但是更简单且聪明的方式是将这两个接口进行内嵌，形成一个新的接口，比如这样：

```go
// ReadWriter接口组合了Reader和Writer接口。
type ReadWriter interface {
    Reader
    Writer
}
```

见字如面：一个ReadWriter可以把Reader*和*Writer的活儿都干了；它是二者的结合。接口中只能内嵌接口。

同样的道理，struct也一样，但拥有更加深远的影响。bufio包中包含两个struct类型，bufio.Reader和bufio.Writer，分别实现了io包中对应的接口。bufio包中还实现了一个可缓冲的读写器，也就是通过内嵌组合了Reader和Writer：在struct中罗列它们的类型，但不用给出字段的名字。

```go
// ReadWriter保存了Reader和Writer的指针。
// 它实现了io.ReadWriter。
type ReadWriter struct {
    *Reader  // *bufio.Reader
    *Writer  // *bufio.Writer
}
```

内嵌的元素是struct的指针，因此在使用前必须要初始化成有效的struct类型的指针。我们可以试着把ReadWriter改成

```go
type ReadWriter struct {
    reader *Reader
    writer *Writer
}
```

但此时为了满足io接口的定义我们要提供字段对应的方法，我们还需要对方法进行转发，比如：

```go
func (rw *ReadWriter) Read(p []byte) (n int, err error) {
    return rw.reader.Read(p)
}
```

如果直接使用struct内嵌，我们就不需要这种累赘的代码了。内嵌类型的方法开箱即用，就是说bufio.ReadWriter不光有bufio.Reader和bufio.Writer的方法，它还同时满足了三个接口：io.Reader、io.Writer、io.ReadWriter。

内嵌和继承存在一种非常重要的不同。当我们内嵌类型时，内嵌类型的方法变成了外部类型的方法，但它们被调用的时候，实际的接收者却是内嵌类型，而不是外部类型。在上面的栗子中，当调用bufio.ReadWriter的Read方法时，它的效果跟我们写的使用方法转发的栗子完全一样；接收者是ReadWriter中的reader字段，而不是ReadWriter本身。

内嵌还可以简化你的代码。下面的栗子包含一个内嵌字段和一个普通的命名字段。

```go
type Job struct {
    Command string
    *log.Logger
}
```

Job类型现在有了Print、Printf、Println以及\*log.Logger的其他方法。我们可以给Logger加个字段名，但真的没必要。现在，当初始化完成后，我们可以用Job进行日志操作：

```go
job.Println("starting now...")
```

Logger也是Job的一个普通字段，因此我们可以在Job的构造方法中照常对它进行初始化，比如，

```go
func NewJob(command string, logger *log.Logger) *Job {
    return &Job{command, logger}
}
```

或者直接用字面量，

```go
job := &Job{command, log.New(os.Stderr, "Job: ", log.Ldate)}
```

如果我们想直接引用内嵌的字段，那我们可以忽略它的包名，直接用类型名当字段名，就像ReadWriter结构体中的Read方法一样。比如这里我们要访问一个Job类型的变量job中的\*log.Logger，我们可以直接用job.Logger，当我们想优化Logger的方法时就变得很方便了。

```go
func (job *Job) Printf(format string, args ...interface{}) {
    job.Logger.Printf("%q: %s", job.Command, fmt.Sprintf(format, args...))
}
```

类型内嵌带来了命名冲突的风险，但解决的办法也很简单。首先，如果有一个名为X的方法或者字段，则覆盖内嵌类型的同名方法或字段。如果log.Logger包含了一个名为Command的方法或字段，则Job结构体中实际有效的Command为Job中的Command字段。

其次，如果在同级内嵌中出现了同样的名字，通常都不太妙；如果Job中包含了一个名为Logger的字段或者方法，那就不应该再嵌入log.Logger。但是如果这些重复的名字并没有被外部实际的用到，那就还OK。这样可以避免内嵌的内容被外部修改；当内嵌字段出现冲突时，不用就不出错。

## 并发

### 利用通信来共享数据

并发编程是个大事儿，Go在这上面可是有点儿东西的。

很多时候并发编程都会因为要正确使用共享变量，导致变得复杂度较高。Go语言另辟蹊径，鼓励大家将需要共享的值在channel中进行传递，这样实际上就不存在各个线程同时处理一个共享值的情况。某一时刻只有一个协程能访问到这个值。这样从设计阶段就避免了资源竞争。为了鼓励这种思考方式，我们把它总结成了一句万古流芳的话：

```text
不要通过共享内存来通信；而是通过通信的方式来共享内存。
```

这种方式意义深远。比如，本来我们在一个整数变量前加一个互斥锁，可以很好的实现一个引用计数的功能。但更高端的玩儿法是，使用channel来进行访问控制，这样的程序才更加简洁、清晰且正确。

要想更好的理解这个东西，我们可以举一个简单的栗子，考虑一个单线程的程序，运行在一个CPU上。此时它不需要同步原语。然后我们再加一个实例；它同样不需要同步处理。现在，我们要让它们俩之间进行通信；如果基于同步器，那就不需要其他的同步措施了。如果用Unix管道，也非常适合这种场景。尽管Go的并发模型来源于CSP，但其实看上去类似一种类型安全的Unix管道衍生品。

### Go协程

之所以要叫*Go协程*是因为现有的那几个类似的词儿吧——比如线程、协程、进程等——总会传达出一些错误的信息。一个Go协程的模型很简单：它是一个可以跟其他Go协程在同一个地址空间中并发执行的函数。它很轻量，除了分配栈空间，消耗不了太多别的东西。而且栈空间在一开始都很小，所以就很轻，然后随着堆存储的不断分配（和释放）慢慢同步增长。

Go协程出现阻塞时，比如等待IO，在多个系统线程上会进行多路复用，其他的Go协程能够继续运行。其中的设计隐藏了很多线程的创建和管理的复杂工作。

在方法或者函数调用前加一个go，这样就会在一个新的Go协程中执行了。调用完成后Go协程悄无声息自动退出。（效果类似Unix中的后台运行标记&。）

```go
go list.Sort()  // 并发执行，不用等它。
```

通过Go协程调用字面量函数很方便。

```go
func Announce(message string, delay time.Duration) {
    go func() {
        time.Sleep(delay)
        fmt.Println(message)
    }()  // 注意括号——必须要进行函数调用。
}
```

在Go中，字面量函数即闭包：它的实现保证了只要函数还在，那么它所引用的变量就一直存在。

这些例子都不太实用，因为我们不知道函数什么时候执行完了。所以，我们需要香奈儿。

### 香奈儿

和map类似，香奈儿也要用make来建，返回的结果也是对底层数据结构的引用。如果提供了额外的整数参数，则作为香奈儿的缓冲大小。默认是零，也就是无缓冲或者叫同步香奈儿。

```go
ci := make(chan int)            // 无缓冲的整数香奈儿
cj := make(chan int, 0)         // 无缓冲的整数香奈儿
cs := make(chan *os.File, 100)  // File指针的缓冲香奈儿
```

无缓冲的香奈儿将通信——值的交换——和同步结合起来——保证两边的计算（Go协程）处于一个已知的状态。

香奈儿的场景也比较多。我们先找一个看一下。上一节我们在后台运行了一个排序。香奈儿可以让当前的Go协程等待排序完成。

```go
c := make(chan int)  // 分配一个香奈儿。
// 在一个Go写成中开始排序；结束后，给香奈儿发个信号。
go func() {
    list.Sort()
    c <- 1  // 发个信号；具体发的啥无所谓。
}()
doSomethingForAWhile()
<-c   // 等待排序结束；丢弃发过来的值。
```

接收方会一直阻塞在那里，直到有数据过来。如果香奈儿时无缓冲的，发送方会一直阻塞，直到数据被接收方拿走。如果香奈儿有缓冲，发送方只需要将数据复制到缓冲中即可；如果缓冲满了，那就要等接收方拿走里面的一个值。

带缓冲的香奈儿可以当信号量使，比如用来限制吞吐量。下面的栗子中，过来的请求会传给handle，它要发一个值给香奈儿，然后处理请求，然后再从香奈儿中拿一个值，让“信号量”准备好为下一个消费者提供服务。香奈儿缓冲的容量决定了能够同时执行多少个process调用。

```go
var sem = make(chan int, MaxOutstanding)

func handle(r *Request) {
    sem <- 1    // 排出才能继续塞。
    process(r)  // 可能要执行很久哦。
    <-sem       // 完成；下一个过来吧。
}

func Serve(queue chan *Request) {
    for {
        req := <-queue
        go handle(req)  // 不用等。
    }
}
```

当有MaxOutstanding个handler在执行process函数时，此时再想往香奈儿缓冲中发消息就会被阻塞，因为已经满了，直到其中一个handler完成它的工作并从缓冲中取出一个消息。

但是这种设计有个问题：Serve函数为每次请求都创建了一个Go协程，即便它们之中同一时间只能有MaxOutstanding个可以继续执行。结果就是如果请求来的太快，这个程序可能要无限制的消耗资源。我们可以改一下Serve，让它守住Go协程的创建时机。下面的解决办法很直白，但是它也有bug，我们后面会解决：

```go
func Serve(queue chan *Request) {
    for req := range queue {
        sem <- 1
        go func() {
            process(req) // 有bug；后面解释。
            <-sem
        }()
    }
}
```

问题就在于Go中的for循环，每次迭代都会重用循环变量，所以req会被所有的Go协程共享。这并非我所愿也。我们要确保每个Go协程中的req是唯一的。一种办法是，将req的值作为Go协程闭包的参数传进去：

```go
func Serve(queue chan *Request) {
    for req := range queue {
        sem <- 1
        go func(req *Request) {
            process(req)
            <-sem
        }(req)
    }
}
```

和之前的栗子对比一下，注意闭包的声明和运行方式。另一种处理方式是起一个同名的变量，比如：

```go
func Serve(queue chan *Request) {
    for req := range queue {
        req := req // 为协程创建一个req的新实例。
        sem <- 1
        go func() {
            process(req)
            <-sem
        }()
    }
}
```

这么写看着有点别扭

```go
req := req
```

但这的确合法，而且是Go中的惯用伎俩。同样的名字可以让你得到一个全新版本的变量，它刻意覆盖了循环变量，保证每个Go协程中变量的唯一性。

回到编写服务的问题上来，另一种资源管理的方式是启动固定数量的handle协程，且全部处于从请求香奈儿中读取数据的状态。Go协程的数量限制了同时进行process的数量。这个版本的Serve函数同时还监听着一个负责通知退出的香奈儿；程序启动后，它就会一直阻塞在这个香奈儿的读取上。

```go
func handle(queue chan *Request) {
    for r := range queue {
        process(r)
    }
}

func Serve(clientRequests chan *Request, quit chan bool) {
    // 启动处理器
    for i := 0; i < MaxOutstanding; i++ {
        go handle(clientRequests)
    }
    <-quit  // 等待退出。
}
```

### 香奈儿嵌套

Go里面很重要的一点就是香奈儿是一等公民，可以跟其他的东西一样进行分配和传递。利用这一点就可以进行安全的并行多路分解编程。

再前面的栗子中，对于一个请求，我们已经把handle优化得可以的了，但是我们还没有定义什么是请求。如果请求中包含了一个可以用来响应的香奈儿，每个客户端就可以提供它自己的回复路径。下面我们给出Request类型的定义。

```go
type Request struct {
    args        []int
    f           func([]int) int
    resultChan  chan int
}
```

客户端提供了一个函数以及对应的参数，还有一个用来接收响应的香奈儿。

```go
func sum(a []int) (s int) {
    for _, v := range a {
        s += v
    }
    return
}

request := &Request{[]int{3, 4, 5}, sum, make(chan int)}
// 发送请求
clientRequests <- request
// 等待响应。
fmt.Printf("answer: %d\n", <-request.resultChan)
```

服务端我们只需要改一下handler函数。

```go
func handle(queue chan *Request) {
    for req := range queue {
        req.resultChan <- req.f(req.args)
    }
}
```

这些代码要想真的派上用场，当然还要做很多调整，但这里给出的是一个框架，它可以用来做可受限的、并行的、非阻塞的RPC程序，而且明面儿上看不到互斥锁。

### 并行

这些概念的另一种利用方式就是在多个CPU上进行并行计算。如果一项计算能够分解成可单独执行的片段，那就可以并行化，每个片段执行完后通过香奈儿来发出信号。

假设我们现在要对一个向量进行一个复杂的计算，对于向量中的每个元素的计算结果是相互独立的，这样就是一个理想化的用例了。

```go
type Vector []float64

// 对v[i], v[i+1] ... v[n-1]分别进行计算。
func (v Vector) DoSome(i, n int, u Vector, c chan int) {
    for ; i < n; i++ {
        v[i] += u.Op(v[i])
    }
    c <- 1    // 通知本次计算完成
}
```

我们在一个循环中运行这些片段，每个片段分配到一个CPU上。它们的执行顺序并不确定，但这无所谓；在启动所有的Go协程后，我们只需要将香奈儿不断抽取，对结束信号进行计数即可。

```go
const numCPU = 4 // CPU核数

func (v Vector) DoAll(u Vector) {
    c := make(chan int, numCPU)  // 缓冲虽是可选，但在这里是有用处的。
    for i := 0; i < numCPU; i++ {
        go v.DoSome(i*len(v)/numCPU, (i+1)*len(v)/numCPU, u, c)
    }
    // 抽干香奈儿。
    for i := 0; i < numCPU; i++ {
        <-c    // 等待其中一个任务完成
    }
    // 全完了。
}
```

其实我们不用固定numCPU的值，我们可以询问运行时来得到合适的值。runtime.NumCPU可以返回当前机器的硬件CPU核数，所以我们可以

```go
var numCPU = runtime.NumCPU()
```

这里还有一个函数是runtime.GOMAXPROCS，可以返回（或设置）由用户设定的Go程序可以用来同时运行的核心数。它的默认值等于runtime.NumCPU，但是可以用类似的环境变量来覆盖这个值，或者直接用一个正数作为参数来调用这个函数。如果传零，那就代表是要查询。所以如果我们要遵守用户的设定，那就应该这么写

```go
var numCPU = runtime.GOMAXPROCS(0)
```

注意并发——将程序分成独立执行的部分——和并行——让计算高效的并行运行在多个CPU上，不要弄混了。尽管Go的并发特性有助于进行并行编程，但Go是并发语言，而不是并行语言，Go的这种模型并不适用于所有并行问题。关于这个问题的讨论，可以看看[这篇文章](https://blog.golang.org/waza-talk)。

### 缓冲漏桶

为并发编程提供的这些工具甚至可以让一些非并发的场景更容易表达。下面的栗子抽象了一个RPC包。客户端的Go协程通过循环不断地从某种源上读取数据，也许是一个网络连接。为了避免分配和释放缓冲，它设计了一个空闲列表，具体就是用带缓冲的香奈儿来实现的。如果香奈儿是空的，那就分配一个新的缓冲。当消息缓冲准备就绪时就通过serverChan发送给服务端。

```go
var freeList = make(chan *Buffer, 100)
var serverChan = make(chan *Buffer)

func client() {
    for {
        var b *Buffer
        // 有的化直接拿一个；没有就分配一个。
        select {
        case b = <-freeList:
            // 有了；直接继续。
        default:
            // 没有闲的，分配一个。
            b = new(Buffer)
        }
        load(b)              // 从网络上读取下一条消息。
        serverChan <- b      // 发给服务端。
    }
}
```

服务端的循环就是读取每一条从客户端发过来的消息，然后处理，然后将缓冲返还给空闲列表。

```go
func server() {
    for {
        b := <-serverChan    // 等待新任务到达。
        process(b)
        // 如果还有空间，那就重用缓冲。
        select {
        case freeList <- b:
            // 缓冲已存入空闲列表；直接继续。
        default:
            // 空闲列表已满，继续下一轮。
        }
    }
}
```

客户端尝试从freeList中获取缓冲；如果没有，那就分配一个新的。当空闲列表还有空间的时候，服务端会将b返还给freeList，如果空闲列表已满，那么缓冲就直接被丢弃，等待垃圾回收。（当select语句中没有任何条件满足时，就会执行default，这样select就永远不会被阻塞。）这个栗子用若干行代码就实现了一个漏桶空闲列表，它依赖带缓冲的香奈儿以及垃圾回收器做记账工作。

## 错误

库函数经常需要给调用者返回某种错误提示。上面已经讲过，Go的多值返回可以让详细的错误信息跟正常的返回值一并返回。利用这种特性来返回详细的错误信息，这是一种非常良好的风格。比如os.Open在出错的时候不会仅返回一个nil，还会返回一个告诉你到底出什么问题的错误值。

习惯上，错误信息都得是error类型，这是一个简单的内置接口。

```go
type error interface {
    Error() string
}
```

库函数的编写者可以自由实现这个接口，在内部加入更多丰富的功能，这样不仅可以返回错误信息，还可以返回一些上下文。比如上面提到的，os.Open的返回值中，跟*os.File一起的还有一个错误值。如果文件正常打开，错误值就是nil，如果真出了问题，那就会得到一个os.PathError：

```go
// PathError记录了错误信息以及导致错误的操作和文件路径。
type PathError struct {
    Op string    // “open”、“unlink”，等等。
    Path string  // 对应的文件。
    Err error    // 由系统调用返回。
}

func (e *PathError) Error() string {
    return e.Op + " " + e.Path + ": " + e.Err.Error()
}
```

PathError的Error生成的字符串格式：

```text
open /etc/passwx: no such file or directory
```

这种错误信息，包含了程序中生成的文件名、文件操作，以及触发的操作系统错误，即便和真实调用它的地方离得很远，这种错误信息打印出来依然极具效果；这种信息比一句简单的“no such file or directory”更有用。

可以的话，错误信息应该表明它们的发祥地，比如通过命名前缀来告诉我们产生错误的操作或者来自哪个包。比如在image包中，由于未知格式导致解码错误时，给出的提示就是“image: unknown format”。

如果调用者需要了解错误的详细信息，可以用类型switch或者类型断言来查看具体的错误并提取出其中的细节。比如对于PathErrors，可能会需要检查一下内部的Err字段，发现某些可以恢复的错误。

```go
for try := 0; try < 2; try++ {
    file, err = os.Create(filename)
    if err == nil {
        return
    }
    if e, ok := err.(*os.PathError); ok && e.Err == syscall.ENOSPC {
        deleteTempFiles()  // 释放一些磁盘空间。
        continue
    }
    return
}
```

第二个if是另一种[类型断言](#接口转换与类型断言)。如果失败，ok就是false，e就是nil。如果成功，ok就是true，而错误对象就是*os.PathError类型，也就是e，然后就可以进一步探查其中的信息了。

### 慌的一批

一般我们都是返回一个额外的error来告诉调用者出现了什么问题。比如典型的Read方法；它返回一个字节数以及一个error。但如果错误是无法挽回的怎么办？有的时候程序可能直接就无法继续运行下去了。

考虑到这种情况，我们设计了一个内置的panic函数，它可以创建一个运行时错误并让程序停止运行（下一节讲补救措施）。这个函数接收一个任意类型的参数——一般是字符串——当程序嗝屁的时候输出一下。这种方式通常用来表明某个不可能出现的情况真的出现了，比如退出了一个死循环。

```go
// 牛顿法计算立方根。
func CubeRoot(x float64) float64 {
    z := x/3   // 任意初始值
    for i := 0; i < 1e6; i++ {
        prevz := z
        z -= (z*z*z-x) / (3*z*z)
        if veryClose(z, prevz) {
            return z
        }
    }
    // 一百万次迭代还不够；出问题喽。
    panic(fmt.Sprintf("CubeRoot(%g) did not converge", x))
}
```

这里只是个栗子，真的库函数应当避免使用panic。如果问题能够被隐藏或者消解掉，那么最好还是让程序继续，而不是把整个程序直接干死。一个反例情况是初始化：如果库函数的确无法完成初始化，可以说它就有理由慌的一批。

```go
var user = os.Getenv("USER")

func init() {
    if user == "" {
        panic("no value for $USER")
    }
}
```

### Recover

调用pannic的时候，包括运行时的隐式调用，比如slice越界，或者是错误的类型断言，它会立即停止执行当前函数并开始退出Go协程栈，同时执行所有的defer函数。如果退到了Go协程的栈顶，程序也就嗝屁了。但是可以使用内置的recover函数来重获新生，让程序继续运行。

调用recover会停止退栈并返回panic的参数。因为在退栈的时候能执行的代码只有defer函数中的代码，所以recover只有在defer函数中才有效。

recover的一种用法是在服务器程序中关闭一个异常的Go协程，避免其他的Go协程也被杀死。

```go
func server(workChan <-chan *Work) {
    for work := range workChan {
        go safelyDo(work)
    }
}

func safelyDo(work *Work) {
    defer func() {
        if err := recover(); err != nil {
            log.Println("work failed:", err)
        }
    }()
    do(work)
}
```

这里，如果do(work)慌球了，返回的错误信息会被记录下来，当前Go协程正常退出，不会打扰到其他人。defer函数中不用干什么特殊的事儿，调用一下recover就全包了。

如果不是在defer函数中，调用recover则直接返回nil，defer调用库函数时不受panic和recover的影响。比如safelyDo中的defer函数，它可以在调用recover之前调用一个日志函数，日志函数的执行不受当前panic的影响。

在我们的栗子中，do函数（以及它的调用方）对于panic调用都可以全身而退。我们基于这种思路就可以简化复杂软件中的错误处理机制。来看一个理想化的regexp包，当出现解析异常时会调用panic，并传入一个自定义的错误类型。下面的代码中包含了Error的定义、error方法，以及Compile函数。

```go
// 代表解析异常的Errir类型，满足了error接口。
type Error string
func (e Error) Error() string {
    return string(e)
}

// errir是*Regexp的一个方法，调用panic来报告解析异常，并传入一个Error。
func (regexp *Regexp) error(err string) {
    panic(Error(err))
}

// Compile函数返回一个正则表达式的解析结果。
func Compile(str string) (regexp *Regexp, err error) {
    regexp = new(Regexp)
    // 如果发生异常，doParse就慌球了。
    defer func() {
        if e := recover(); e != nil {
            regexp = nil    // 清理返回值。
            err = e.(Error) // 如果不是解析异常，那就继续慌球了。
        }
    }()
    return regexp.doParse(str), nil
}
```

如果doParse慌球了，异常处理代码会将返回值设置为nil——defer函数可以修改有名字的返回值。然后在给err赋值的时候进行检查，通过断言错误类型是否为自定义的Error来判断问题是不是由于解析异常导致的。如果不是，类型断言失败，触发运行时异常，继续退栈，好似没有被中断过一样。也就是说，如果发生了某些意想不到的情况，比如索引越界，那么即便我们用了panic和recover来处理解析异常，但程序依然会慌球了。

上面栗子中的error方法（因为它是绑定到一个类型上的方法，所以即便跟内置的error类型重名也ok，这很平常，不用大惊小怪，并吃了一碗泡面）可以很方便的报告解析异常，而且还不用手动去搞什么解析栈退出的操作。

```go
if pos == 0 {
    re.error("'*' illegal at start of expression")
}
```

这种模式很有效，但是应该只限于包内使用。Parse将内部的panic调用转成了error值；它不会把panic暴露给客户端。这种风格我们都应该遵守。

注意一下，这种重新发慌的路子会让真实的错误信息发生变化。但是在崩溃信息中会把原始的和新的异常都打出来，因此问题的根因依然可见。因此这种二慌法通常也就足够了——毕竟是崩溃了——但如果你只想显示根因，那可以再改改代码，过滤掉不需要的东西，用原始异常来进行二慌。这个事儿我们就留给读者解闷儿吧。

## 一个web服务

最后我们来搞一个完整的Go程序，一个Web服务。这个栗子实际上类似于某种代理服务。谷歌在chart.apis.google.com上提供了将数据转成图形图表的接口。但是没法交互式的使用，因为你要把数据放到URL里面。本例为其中一种数据格式提供了一个美观的交互界面：输入一段文本，它就会调用图形服务生成一个二维码，也就是这段文本的一个格子矩阵编码。你可以用手机摄像头来扫描这个图形并自动解释出来，比如你可以输入一个URL，这样后面你在手机上就不用再输入这个URL了，直接扫就行了。（看来这个世界上还真是有人不知道二维码啊。）

下面我们给出完整程序。后面会进行说明。

```go
package main

import (
    "flag"
    "html/template"
    "log"
    "net/http"
)

var addr = flag.String("addr", ":1718", "http service address") // Q=17, R=18

var templ = template.Must(template.New("qr").Parse(templateStr))

func main() {
    flag.Parse()
    http.Handle("/", http.HandlerFunc(QR))
    err := http.ListenAndServe(*addr, nil)
    if err != nil {
        log.Fatal("ListenAndServe:", err)
    }
}

func QR(w http.ResponseWriter, req *http.Request) {
    templ.Execute(w, req.FormValue("s"))
}

const templateStr = `
<html>
<head>
<title>QR Link Generator</title>
</head>
<body>
{{if .}}
<img src="http://chart.apis.google.com/chart?chs=300x300&cht=qr&choe=UTF-8&chl={{.}}" />
<br>
{{.}}
<br>
<br>
{{end}}
<form action="/" name=f method="GET">
    <input maxLength=1024 size=70 name=s value="" title="Text to QR Encode">
    <input type=submit value="Show QR" name=qr>
</form>
</body>
</html>
`
```

这一整段代码应该都不难理解。一开始的参数设置了默认的HTTP端口。模板变量templ是一切奥妙的核心所在。它构建了一个HTML模板，服务端用它来显示页面；一会儿再细说。

main函数通过我们之前讲过的机制来解析参数，将QR函数绑定到服务的根路径上。然后调用http.LinstenAndServe来启动服务，服务运行时它将一直处于阻塞状态。

QR函数收到请求，其中包含了表单数据，然后使用表单数据中名为s的值来执行模板。

模板包html/template可厉害了；我们的程序只是看到了冰山一角。本质上讲，它就是要重写HTML文本，用templ.Execute的参数来替换指定的元素，本例中就是用了表单传过来的值。在模板文本（templateStr）中，双花括号括起来的部分表示模板动作。只有调用.（点）得到的当前值不为空的情况下才会执行{{if .}}到{{end}}的代码段。也就是说如果字符串为空，这段模板就会被抑制。

两处{{.}}的作用就是将模板数据——查询字符串——展示到web页面上。HTML模板包自动进行转义，文本们可以放心展示自我。

模板中剩余的部分就是些页面要加载的HTML而已。如果这块的东西你觉得讲得太快，那可以去看模板包的[文档](#文档)系统性的学习一下。

到这儿就没了：几行代码搞出了一个有意义的web服务，还带着数据驱动的HTML文本。Go就是这样，几行文本就可以搞出很牛逼的东西。