# 第一个 go 程序



### 写文件

在 `src` 新建一个目录，到目录里面写一个文件。

这里分别是是 `hello/` 和 `main.go`

```go
package main // 声明 main 包，表明当前是一个可执行程序

import "fmt" // 导入内置 fmt

func main() { // main函数，是程序执行的入口
	fmt.Println("Hello World!") // 在终端打印 Hello World!
}
```

### 运行：

```
go run main.go
```

### 构建：

```
go build main.go
```

这会生成一个叫 `main` 的可执行文件

![image-20221212185509929](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/12/image-20221212185509929-58c9ed.png)