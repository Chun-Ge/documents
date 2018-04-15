# Shudong Backend Code Standard

## Framework

使用[Iris](https://github.com/kataras/iris/)框架， 声称各项性能均超过beego， 且功能更全更强大。 支持自定义中间件、 随意组合各种语法的路由而不用担心冲突、 任何错误代码、 Iris支持完整的MVC功能，可以在运行时注册。

sample有很多，基本涵盖各种情况下的用例（[Link](https://github.com/kataras/iris/tree/master/_examples), [官网的排版更好](https://iris-go.com/v10/recipe)， git仓库维护速度很快， 目前已有部分中文文档。

PS: vscode marketplace 里有Iris的插件 (Iris Web Framework (Go))， 可以方便地插入Iris的代码

## Go 代码规范

Go语言本身存在许多严格的规定（违反会直接导致编译错误）， 如必须使用驼峰命名法， 方法/属性的公有/私有取决于命名首字母是否大写。 `gofmt` 会自动将go代码文件按照钦定的风格进行格式化(vscode集成的go插件会在文件保存时自动调用， 其他编辑器请自查)， `godoc`会根据代码内具体位置的注释生成目标代码/代码包的文档， 所以通常情况下需要特别注意的东西不多， 在此列下较为重要的部分：

- 使用Tab缩进。
- 包应当以小写的单个单词来命名，且不应使用下划线或驼峰记法。
- 在包中，任何顶级声明前面的注释都将作为该声明的文档注释。 在程序中，每个可导出（首字母大写）的名称都应该有文档注释。
- 按照约定，只包含一个方法的接口应当以该方法的名称加上 - er 后缀来命名，如 Reader、Writer、 Formatter、CloseNotifier 等。
- Go源码大多不使用分号。 （实际上是编译器自动在合理的位置加入分号）
  - 通常Go源码只在诸如for循环子句这样的地方出现分号，以此来将初始化器、条件及增量元素分开。如果你在一行中写多个语句，也需要用分号隔开。
- 注释第一句应当以被声明的东西开头，并且是单句的摘要。 （若注释总是以名称开头，godoc 的输出就能通过 grep 变得更加有用。）
- 命名必须采用驼峰命名法，不要使用下划线。首字母小写表示私有属性/方法，首字母大写表示公有。
- Go的声明语法允许成组声明。单个文档注释应介绍一组相关的常量或变量。
- 枚举常量使用枚举器 `iota` 创建。
- if 和 switch 可接受初始化语句，可用它们来设置局部变量。
- 若 if 语句不会执行到下一条语句时，亦即其执行体 以 break、continue、goto 或 return 结束时，不必要的 else 会被省略。
- switch的表达式可以是任何类型的值，若省略表达式则匹配`true`， 因此，将多重`if-elseif-else`写成`switch`。
- switch 并不会自动下溯(匹配到一个case，运行完代码之后自动break)，但 case 可通过逗号分隔来列举相同的处理条件。
- Go 只有for循环。
- Go习惯使用切片(slices)而非数组。同样存在二维、多维切片。 分配新切片时使用`make`
- Go 并不对获取器（getter）和设置器（setter）提供自动支持。若你有个名为 owner （小写，未导出）的字段，其获取器应当名为 Owner（大写，可导出）而非 GetOwner
- Go函数支持多值返回(Multiple return values)， 每个返回值的类型可以不同。
- Go使用空白标识符来表示无用的返回值、for-range中不需要的下标、仅使用副作用(side effects)的包引入。
- Go源代码从`main`函数开始执行， 所有Go源码在被执行/引用之前都会先运行`init()`（如果`init()`函数存在）
- 并发通过Channels实现。
- 使用`defer`来在函数运行结束后关闭文件（或其他相关操作）

详见[Effective Go(中/英对照版)](https://legacy.gitbook.com/book/bingohuang/effective-go-zh-en/details)。

### 注释

```go
// Compile parses a regular expression and returns, if successful, a Regexp
// object that can be used to match against text.
func Compile(str string) (regexp *Regexp, err error) {
```

### 成组声明

```go
// Error codes returned by failures to parse an expression.
var (
    ErrInternal = errors.New("regexp: internal error")
    ErrUnmatchedLpar = errors.New("regexp: unmatched '('")
    ErrUnmatchedRpar = errors.New("regexp: unmatched ')'")
    ...
)
```

### Getter & Setter

```go
owner := obj.Owner()
if owner != user {
    obj.SetOwner(user)
}
```

### If & Switch 语句

If & Switch语句设置局部变量：

```go
if err := file.Chmod(0664); err != nil {
    log.Print(err)
    return err
}
```

switch 不自动下溯

```go
func shouldEscape(c byte) bool {
    switch c {
    case ' ', '?', '&', '=', '#', '+', '%':
        return true
    }
    return false
}
```

若要下溯，需要显式使用`fallthrough`，这点与C恰好相反。

```go
// switchFallthrough will return 3 if i > 5
func switchUseFallthrough(i int) int {
    ret := 0
    switch {
        case i > 5:
            ret += 1
            fallthrough
        case i > 3:
            ret += 1
            fallthrough
        case i > 1:
            ret += 1
            fallthrough
    }
    return ret
}
```

### Type Switch 类型选择

```go
var t interface{}
t = functionOfSomeType()
switch t := t.(type) {
default:
    fmt.Printf("unexpected type %T", t) // %T prints whatever type t has
case bool:
    fmt.Printf("boolean %t\n", t) // t has type bool
case int:
    fmt.Printf("integer %d\n", t) // t has type int
case *bool:
    fmt.Printf("pointer to boolean %t\n", *t) // t has type *bool
case *int:
    fmt.Printf("pointer to integer %d\n", *t) // t has type *int
}
```

### 多值返回

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

返回值可以是不同类型。 如`os.Write`函数签名如下：

```go
func (file *File) Write(b []byte) (n int, err error)
```

### Defer

```go
// Contents returns the file's contents as a string.
func Contents(filename string) (string, error) {
    f, err := os.Open(filename)
    if err != nil {
        return "", err
    }
    defer f.Close() // f.Close will run when we're finished.
    // do something
}
```

### 分配切片

make 只适用于映射、切片和信道且不返回指针。若要获得明确的指针， 请使用 new 分配内存。

```go
var p *[]int = new([]int) // 分配切片结构；*p == nil；基本没用
var v []int = make([]int, 100) // 切片 v 现在引用了一个具有 100 个 int 元素的新数组
// Unnecessarily complex:
var p *[]int = new([]int)
*p = make([]int, 100, 100)
// Idiomatic:
v := make([]int, 100)
```

### 枚举常量

在 Go 中，枚举常量使用枚举器 iota 创建。由于 iota 可为表达式的一部分，而表达式可以被隐式地重复，这样也就更容易构建复杂的值的集合了。

```go
type ByteSize float64
const (
    _ = iota // 通过赋予空白标识符来忽略第一个值
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