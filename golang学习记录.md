---
title: golang学习记录
tags:
  - go
  - 学习
abbrlink: 74fdcb9a
date: 2021-09-25 14:53:21
---

## 0x01 值

字符串可以通过+连接

```go
package main

import "fmt"

func main() {

    fmt.Println("go" + "lang")

    fmt.Println("1+1 =", 1+1)
    fmt.Println("7.0/3.0 =", 7.0/3.0)

    fmt.Println(true && false)
    fmt.Println(true || false)
    fmt.Println(!true)
}
```

## 0x02 变量

可以使`var`声明1个或多个变量，也可以一次性声明多个变量，Go可以自动判断其数据类型，声明时如果无初值，则会初始化为零值

例如`var f string = "short"`可以写为`f := "short"`

```go
package main

import "fmt"

func main() {

    var a = "initial"
    fmt.Println(a)

    var b, c int = 1, 2
    fmt.Println(b, c)

    var d = true
    fmt.Println(d)

    var e int
    fmt.Println(e)

    f := "short"
    fmt.Println(f)
}
```

## 0x03 常量

`const`用于声明一个常量，可以出现在任何`var`语句可以出现的地方，**常数表达式可以执行任意精度的运算**，数值型常量没有确定的类型，直到被给定某个类型。

一个数字可以根据上下文的需要（变量赋值，常数调用）自动确定类型。例如下文`math.Sin`需要一个`float64`的参数，n会自动确定类型。

```go
package main

import (
    "fmt"
    "math"
)

const s string = "constant"

func main() {
    fmt.Println(s)

    const n = 500000000

    const d = 3e20 / n
    fmt.Println(d)

    fmt.Println(int64(d))

    fmt.Println(math.Sin(n))
}
```

## 0x04 For循环

for是Go中唯一的循环结构，用法类同于C类语言

```go
package main

import "fmt"

func main() {

    i := 1
    for i <= 3 {
        fmt.Println(i)
        i = i + 1
    }

    for j := 7; j <= 9; j++ {
        fmt.Println(j)
    }

    for {
        fmt.Println("loop")
        break
    }

    for n := 0; n <= 5; n++ {
        if n%2 == 0 {
            continue
        }
        fmt.Println(n)
    }
}
```

## 0x05 If/Else分支

用法类同于C类语言

需要注意的是，**在 Go 中，条件语句的圆括号不是必需的，但是花括号是必需的。**Go 没有[三目运算符](http://en.wikipedia.org/wiki/%3F:)， 即使是基本的条件判断，依然需要使用完整的 `if` 语句。

```go
package main

import "fmt"

func main() {

    if 7%2 == 0 {
        fmt.Println("7 is even")
    } else {
        fmt.Println("7 is odd")
    }

    if 8%4 == 0 {
        fmt.Println("8 is divisible by 4")
    }

    if num := 9; num < 0 {
        fmt.Println(num, "is negative")
    } else if num < 10 {
        fmt.Println(num, "has 1 digit")
    } else {
        fmt.Println(num, "has multiple digits")
    }
}
```

## 0x06 Switch语句

Go中switch可以不使用常量，从而达到实现if/else逻辑的另一种方式

>类型开关 (`type switch`) 比较类型而非值。可以用来发现一个接口值的类型。 在这个例子中，变量 `t` 在每个分支中会有相应的类型。

```go
package main

import (
    "fmt"
    "time"
)

func main() {

    i := 2
    fmt.Print("write ", i, " as ")
    switch i {
    case 1:
        fmt.Println("one")
    case 2:
        fmt.Println("two")
    case 3:
        fmt.Println("three")
    }

    switch time.Now().Weekday() {
    case time.Saturday, time.Sunday:
        fmt.Println("It's the weekend")
    default:
        fmt.Println("It's a weekday")
    }

    t := time.Now()
    switch {
    case t.Hour() < 12:
        fmt.Println("It's before noon")
    default:
        fmt.Println("It's after noon")
    }

    whatAmI := func(i interface{}) {
        switch t := i.(type) {
        case bool:
            fmt.Println("I'm a bool")
        case int:
            fmt.Println("I'm an int")
        default:
            fmt.Printf("Don't know type %T\n", t)
        }
    }
    whatAmI(true)
    whatAmI(1)
    whatAmI("hey")
}
```

## 0x07 数组

在Go中，数组是一个具有编号且长度固定的元素序列，数组默认值均为零值，内置len函数可以返回数组的长度。

注意，使用 `fmt.Println` 打印数组时，会按照 `[v1 v2 v3 ...]` 的格式打印。

```go
package main

import "fmt"

func main() {

    var a [5]int
    fmt.Println("emp:", a)

    a[4] = 100
    fmt.Println("set:", a)
    fmt.Println("get:", a[4])

    fmt.Println("len:", len(a))

    b := [5]int{1, 2, 3, 4, 5}
    fmt.Println("dcl:", b)

    var twoD [2][3]int
    for i := 0; i < 2; i++ {
        for j := 0; j < 3; j++ {
            twoD[i][j] = i + j
        }
    }
    fmt.Println("2d: ", twoD)
}
```

## 0x08 切片Slice

除了基本操作外，slice 支持比数组更丰富的操作。比如 slice 支持内建函数 `append`， 该函数会返回一个包含了一个或者多个新值的 slice。 注意由于 `append` 可能返回一个新的 slice，我们需要接收其返回值。

slice 还可以 `copy`。这里我们创建一个空的和 `s` 有相同长度的 slice——`c`， 然后将 `s` 复制给 `c`。

slice 支持通过 `slice[low:high]` 语法进行“切片”操作。 例如，右边的操作可以得到一个包含元素 `s[2]`、`s[3]` 和 `s[4]` 的 slice。

我们可以在一行代码中声明并初始化一个 slice 变量。

Slice 可以组成多维数据结构。内部的 slice 长度可以不一致，这一点和多维数组不同。

slice虽然和数组是不同的类型，但是通过`fmt.Println`打印的输出结果是类似的

```go
package main

import "fmt"

func main() {

    s := make([]string, 3)
    fmt.Println("emp:", s)

    s[0] = "a"
    s[1] = "b"
    s[2] = "c"
    fmt.Println("set:", s)
    fmt.Println("get:", s[2])

    fmt.Println("len:", len(s))

    s = append(s, "d")
    s = append(s, "e", "f")
    fmt.Println("apd:", s)

    c := make([]string, len(s))
    copy(c, s)
    fmt.Println("cpy:", c)

    l := s[2:5]
    fmt.Println("sl1:", l)

    l = s[:5]
    fmt.Println("sl2:", l)

    l = s[2:]
    fmt.Println("sl3:", l)

    t := []string{"g", "h", "i"}
    fmt.Println("dcl:", t)

    twoD := make([][]int, 3)
    for i := 0; i < 3; i++ {
        innerLen := i + 1
        twoD[i] = make([]int, innerLen)
        for j := 0; j < innerLen; j++ {
            twoD[i][j] = i + j
        }
    }
    fmt.Println("2d: ", twoD)
}
```

```shell
$ go run slices.go
emp: [  ]
set: [a b c]
get: c
len: 3
apd: [a b c d e f]
cpy: [a b c d e f]
sl1: [c d e]
sl2: [a b c d e]
sl3: [c d e f]
dcl: [g h i]
2d:  [[0] [1 2] [2 3 4]]
```

## 0x09 Map

map与C++的STL的map类似

要创建一个空 map，需要使用内建函数 `make`：`make(map[key-type]val-type)`。使用`fmt.Println`可以输出它的所有键值，内建`len`获取长度和`delete`移除一个键值对

当从一个 map 中取值时，还有可以选择是否接收的第二个返回值，该值表明了 map 中是否存在这个键。 这可以用来消除 `键不存在` 和 `键的值为零值` 产生的歧义， 例如 `0` 和 `""`。这里我们不需要值，所以用 *空白标识符(blank identifier)* _ 将其忽略。

注意，使用 `fmt.Println` 打印一个 map 的时候， 是以 `map[k:v k:v]` 的格式输出的。

```go
package main

import "fmt"

func main() {

    m := make(map[string]int)

    m["k1"] = 7
    m["k2"] = 13

    fmt.Println("map:", m)

    v1 := m["k1"]
    fmt.Println("v1: ", v1)

    fmt.Println("len:", len(m))

    delete(m, "k2")
    fmt.Println("map:", m)

    _, prs := m["k2"]
    fmt.Println("prs:", prs)

    n := map[string]int{"foo": 1, "bar": 2}
    fmt.Println("map:", n)
}
```

```shell
$ go run maps.go 
map: map[k1:7 k2:13]
v1:  7
len: 2
map: map[k1:7]
prs: false
map: map[foo:1 bar:2]
```

## 0x10 Range遍历

`range` 在数组和 slice 中提供对每项的索引和值的访问。 有时我们不需要索引，所以我们使用 *空白标识符* `_` 将其忽略。

```go
package main

import "fmt"

func main() {

    nums := []int{2, 3, 4}
    sum := 0
    for _, num := range nums {
        sum += num
    }
    fmt.Println("sum:", sum)

    for i, num := range nums {
        if num == 3 {
            fmt.Println("index:", i)
        }
    }

    kvs := map[string]string{"a": "apple", "b": "banana"}
    for k, v := range kvs {
        fmt.Printf("%s -> %s\n", k, v)
    }

    for k := range kvs {
        fmt.Println("key:", k)
    }

    for i, c := range "go" {
        fmt.Println(i, c)
    }
}
```

```shell
	
$ go run range.go
sum: 9
index: 1
a -> apple
b -> banana
key: a
key: b
0 103
1 111
```

## 0x11 函数

Go 需要明确的 return，也就是说，它不会自动 return 最后一个表达式的值

**当多个连续的参数为同样类型时， 可以仅声明最后一个参数的类型，忽略之前相同类型参数的类型声明。**

```go
package main

import "fmt"

func plus(a int, b int) int {

    return a + b
}

func plusPlus(a, b, c int) int {
    return a + b + c
}

func main() {

    res := plus(1, 2)
    fmt.Println("1+2 =", res)

    res = plusPlus(1, 2, 3)
    fmt.Println("1+2+3 =", res)
}
```

## 0x12 多返回值

Go 原生支持 _多返回值_。 这个特性在 Go 语言中经常用到，例如用来同时返回一个函数的结果和错误信息。`(int, int)` 在这个函数中标志着这个函数返回 2 个 `int`。如果你仅仅需要返回值的一部分的话，你可以使用空白标识符 `_`。

```go
package main

import "fmt"

func vals() (int, int) {
    return 3, 7
}

func main() {

    a, b := vals()
    fmt.Println(a)
    fmt.Println(b)

    _, c := vals()
    fmt.Println(c)
}
```

## 0x13 变参

如果你有一个含有多个值的 slice，想把它们作为参数使用， 你需要这样调用 `func(slice...)`。

```go
package main

import "fmt"

func sum(nums ...int) {
    fmt.Print(nums, " ")
    total := 0
    for _, num := range nums {
        total += num
    }
    fmt.Println(total)
}

func main() {

    sum(1, 2)
    sum(1, 2, 3)

    nums := []int{1, 2, 3, 4}
    sum(nums...)
}
```

```shell
$ go run variadic-functions.go 
[1 2] 3
[1 2 3] 6
[1 2 3 4] 10
```

## 0x14 闭包

`intSeq` 函数返回一个在其函数体内定义的匿名函数。 返回的函数使用闭包的方式 *隐藏* 变量 `i`。 返回的函数 *隐藏* 变量 `i` 以形成闭包。

```go
package main

import "fmt"

func intSeq() func() int {
    i := 0
    return func() int {
        i++
        return i
    }
}

func main() {

    nextInt := intSeq()

    fmt.Println(nextInt())
    fmt.Println(nextInt())
    fmt.Println(nextInt())

    newInts := intSeq()
    fmt.Println(newInts())
}
```

```shell
$ go run closures.go
1
2
3
1
```

## 0x15 递归

```go
package main

import "fmt"

func fact(n int) int {
    if n == 0 {
        return 1
    }
    return n * fact(n-1)
}

func main() {
    fmt.Println(fact(7))
}
```

## 0x16 指针

与C类语言相同

```go
package main

import "fmt"

func zeroval(ival int) {
    ival = 0
}

func zeroptr(iptr *int) {
    *iptr = 0
}

func main() {
    i := 1
    fmt.Println("initial:", i)

    zeroval(i)
    fmt.Println("zeroval:", i)

    zeroptr(&i)
    fmt.Println("zeroptr:", i)

    fmt.Println("pointer:", &i)
}
```

```shell
$ go run pointers.go
initial: 1
zeroval: 1
zeroptr: 0
pointer: 0x42131100
```

## 0x17 结构体

```go
package main

import "fmt"

type person struct {
    name string
    age  int
}

func main() {

    fmt.Println(person{"Bob", 20})

    fmt.Println(person{name: "Alice", age: 30})

    fmt.Println(person{name: "Fred"})

    fmt.Println(&person{name: "Ann", age: 40})

    s := person{name: "Sean", age: 50}
    fmt.Println(s.name)

    sp := &s
    fmt.Println(sp.age)

    sp.age = 51
    fmt.Println(sp.age)
}
```

```shell
$ go run structs.go
{Bob 20}
{Alice 30}
{Fred 0}
&{Ann 40}
Sean
50
51
```

## 0x18 方法

个人感觉类似于C++里面的class类内的public函数吧

```go
package main

import "fmt"

type rect struct {
    width, height int
}

func (r *rect) area() int {
    return r.width * r.height
}

func (r rect) perim() int {
    return 2*r.width + 2*r.height
}

func main() {
    r := rect{width: 10, height: 5}

    fmt.Println("area: ", r.area())
    fmt.Println("perim:", r.perim())

    rp := &r
    fmt.Println("area: ", rp.area())
    fmt.Println("perim:", rp.perim())
}
```

```shell
$ go run methods.go
area:  50
perim: 30
area:  50
perim: 30
```

## 0x19 接口

可以通过接口实现对所有类似抽象问题的求解方法进行集合，从而简化程序的编写逻辑

```go
package main

import (
    "fmt"
    "math"
)

type geometry interface {
    area() float64
    perim() float64
}

type rect struct {
    width, height float64
}
type circle struct {
    radius float64
}

func (r rect) area() float64 {
    return r.width * r.height
}
func (r rect) perim() float64 {
    return 2*r.width + 2*r.height
}

func (c circle) area() float64 {
    return math.Pi * c.radius * c.radius
}
func (c circle) perim() float64 {
    return 2 * math.Pi * c.radius
}

func measure(g geometry) {
    fmt.Println(g)
    fmt.Println(g.area())
    fmt.Println(g.perim())
}

func main() {
    r := rect{width: 3, height: 4}
    c := circle{radius: 5}

    measure(r)
    measure(c)
}
```

```shell
$ go run interfaces.go
{3 4}
12
14
{5}
78.53981633974483
31.41592653589793
```

## 0x20 错误处理

符合 Go 语言习惯的做法是使用一个独立、明确的返回值来传递错误信息。 这与 Java、Ruby 使用的异常（exception） 以及在 C 语言中有时用到的重载 (overloaded) 的单返回/错误值有着明显的不同。 Go 语言的处理方式能清楚的知道哪个函数返回了错误，并使用跟其他（无异常处理的）语言类似的方式来处理错误。

`errors.New` 使用给定的错误信息构造一个基本的 `error` 值。返回错误值为 nil 代表没有错误。还可以通过实现 `Error()` 方法来自定义 `error` 类型。

```go
package main

import (
    "errors"
    "fmt"
)

func f1(arg int) (int, error) {
    if arg == 42 {

        return -1, errors.New("can't work with 42")

    }

    return arg + 3, nil
}

type argError struct {
    arg  int
    prob string
}

func (e *argError) Error() string {
    return fmt.Sprintf("%d - %s", e.arg, e.prob)
}

func f2(arg int) (int, error) {
    if arg == 42 {

        return -1, &argError{arg, "can't work with it"}
    }
    return arg + 3, nil
}

func main() {

    for _, i := range []int{7, 42} {
        if r, e := f1(i); e != nil {
            fmt.Println("f1 failed:", e)
        } else {
            fmt.Println("f1 worked:", r)
        }
    }
    for _, i := range []int{7, 42} {
        if r, e := f2(i); e != nil {
            fmt.Println("f2 failed:", e)
        } else {
            fmt.Println("f2 worked:", r)
        }
    }

    _, e := f2(42)
    if ae, ok := e.(*argError); ok {
        fmt.Println(ae.arg)
        fmt.Println(ae.prob)
    }
}
```

```shell
$ go run errors.go
f1 worked: 10
f1 failed: can't work with 42
f2 worked: 10
f2 failed: 42 - can't work with it
42
can't work with it
```

