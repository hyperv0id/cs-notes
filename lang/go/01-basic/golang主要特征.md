## 语法	

## 关键字

下面列举了 Go 代码中会使用到的 25 个关键字或保留字：

| break    | default     | func   | interface | select |
| -------- | ----------- | ------ | --------- | ------ |
| case     | defer       | go     | map       | struct |
| chan     | else        | goto   | package   | switch |
| const    | fallthrough | if     | range     | type   |
| continue | for         | import | return    | var    |

除了以上介绍的这些关键字，Go 语言还有 36 个预定义标识符：

| append | bool    | byte    | cap     | close  | complex | complex64 | complex128 | uint16  |
| ------ | ------- | ------- | ------- | ------ | ------- | --------- | ---------- | ------- |
| copy   | false   | float32 | float64 | imag   | int     | int8      | int16      | uint32  |
| int32  | int64   | iota    | len     | make   | new     | nil       | panic      | uint64  |
| print  | println | real    | recover | string | true    | uint      | uint8      | uintptr |

程序一般由关键字、常量、变量、运算符、类型和函数组成。

程序中可能会使用到这些分隔符：括号 ()，中括号 [] 和大括号 {}。

程序中可能会使用到这些标点符号：.、,、;、: 和 …。

## 数据类型

- 布尔：`bool`
- 整数：
  - `int{N}` ，N取 8,16,32,64
  - 无符号整型：`uint{N}`
- 浮点：`float{N}`：N取 32,64
- 复数：`complex64`、`complex128`
- `byte`：类 `uint8`
- `rune`：类似 `int32`，常用于字符串解析
- `int`、`uint`：和 `(u)int32`相同，但go不会认为`int`和`int32`是同一类型，需要强制转换
- `uintptr`：指针



## 变量声明



### 普通变量

```go
package main // 声明 main 包，表明当前是一个可执行程序

import "fmt" // 导入内置 fmt

// 5. 因式分解法，常用于全局变量
var (
	NAME = "名字"
	AGE  = 188
)

func main() { // main函数，是程序执行的入口
	// 1. 声明，然后赋值
	var a string
	a = "sadasd"

	// 2. 声明时就赋值
	var b string = "asjkdnwef"

	// 3. 直接类型推断，":=" 只用于声明变量
	c := "asda"
	// c := "sad" // 报错，声明过

	// 4. 一次声明多个
	var d, e, f = 1, 100, "12"
	fmt.Printf(a, b, c, d, e, f)
	fmt.Println("Hello World!") // 在终端打印 Hello World!
}

```

### 常量

```
// 常量
// 普通常量
const PI = 3.14

// 枚举常量
const (
	Unknown = 0
	Female  = 1
	Male    = 2
)
```

### 数组

语法：

```go
// 1. 声明
var b1 [10]float32
// 2. 声明 + 赋值
var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
// 3. 不知道大小
var b2 = [...]int{1, 2, 3,3,2,2,2,2,1,1,2,3,32,13,13,13,312,3,32,13,1331,31,13}
// 4. 二维数组
var b22 = [2][2]int{{1,2},{3,4}}
var b20 = [...][2]int{{1,1},{2,2}}
```

### 结构体

```go
type Books struct {
   title string
   author string
   subject string
   book_id int
}

func main() {
    // 创建一个新的结构体
    fmt.Println(Books{"Go 语言", "www.runoob.com", "Go 语言教程", 6495407})

    // 也可以使用 key => value 格式
    fmt.Println(Books{title: "Go 语言", author: "www.runoob.com", subject: "Go 语言教程", book_id: 6495407})

    // 忽略的字段为 0 或 空
   fmt.Println(Books{title: "Go 语言", author: "www.runoob.com"})
}
```

### 切片

Go 语言切片是对数组的抽象。

Go 数组的长度不可改变，在特定场景中这样的集合就不太适用，Go 中提供了一种灵活，功能强悍的内置类型切片("动态数组")，与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。



语法：

```go
// 创建T类型的数组，长度为length
// capacity可选，指定容量
make([]T, length, capacity)
```

### 定义 Map

可以使用内建函数 make 也可以使用 map 关键字来定义 Map:

```go
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type

/* 使用 make 函数 */
map_variable := make(map[key_data_type]value_data_type)
```

eg：

```go
func main() {
    var countryCapitalMap map[string]string /*创建集合 */
    countryCapitalMap = make(map[string]string)

    /* map插入key - value对,各个国家对应的首都 */
    countryCapitalMap [ "France" ] = "巴黎"
    countryCapitalMap [ "Italy" ] = "罗马"
    countryCapitalMap [ "Japan" ] = "东京"
    countryCapitalMap [ "India" ] = "新德里"

    /*使用键输出地图值 */
    for country := range countryCapitalMap {
        fmt.Println(country, "首都是", countryCapitalMap [country])
    }

    /*查看元素在集合中是否存在 */
    capital, ok := countryCapitalMap [ "American" ] /*如果确定是真实的,则存在,否则不存在 */
    /*fmt.Println(capital) */
    /*fmt.Println(ok) */
    if (ok) {
        fmt.Println("American 的首都是", capital)
    } else {
        fmt.Println("American 的首都不存在")
    }
}
```

## 内置函数



Go 语言拥有一些不需要进行导入操作就可以使用的内置函数。它们有时可以针对不同的类型进行操作，例如：len、cap 和 append，或必须用于系统级的操作，例如：panic。因此，它们需要直接获得编译器的支持。

| 函数名  | 作用                                  |
| ------- | ------------------------------------- |
| append  | 追加元素                              |
| close   | 关闭channel                           |
| delete  | 从map中删除key对应value               |
| panic   | 和recover一起做错误处理               |
| recover | 和panic一起做错误处理                 |
| real    | 返回 complex 值的实数部分             |
| imag    | 返回 complex 值的虚数部分             |
| make    | 分配内存（只用于slice、map、channel） |
| new     | 分配内存，一般为 int、float类型       |
| cap     | 容量                                  |
| copy    | 复制，返回赋值的数目                  |
| len     | 长度                                  |

