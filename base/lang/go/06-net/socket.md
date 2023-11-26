## socket





## TCP-cs模型

### server

流程：

1. 监听端口
2. 接收客户端请求建立链接
3. 创建goroutine处理链接。




```go
package main

import (
	"bufio"
	"fmt"
	"io"
	"net"
)

// 处理函数
func process(conn net.Conn) {
	fmt.Println("连接到客户端：" + conn.RemoteAddr().String())
	defer conn.Close() // 关闭连接
	for {
		reader := bufio.NewReader(conn)
		var buf [128]byte
		n, err := reader.Read(buf[:]) // 读取数据
		if err != nil {
			if err == io.EOF {
				fmt.Println("客户端已退出")
				break
			}
			fmt.Println("read from client failed, err:", err)
			break
		}
		recvStr := string(buf[:n])
		fmt.Println("收到client端发来的数据：", recvStr)
		conn.Write([]byte(recvStr + "已收到")) // 发送数据
	}
}

func main() {
	// 1. 创建 tcp socket 并绑定到 2W 端口
	listen, err := net.Listen("tcp", "127.0.0.1:20000")
	fmt.Println("服务端启动成功，绑定到端口：" + listen.Addr().String())
	// 创建失败，可能是权限错误或者端口被占用
	if err != nil {
		fmt.Println("listen failed, err:", err)
		return
	}
	// 2. 连接客户端
	for {
		conn, err := listen.Accept() // 建立连接
		if err != nil {
			fmt.Println("accept failed, err:", err)
			continue
		}
		// 并发处理客户端请求
		go process(conn) // 启动一个goroutine处理连接
	}
}
```

### client

流程：

1. 监听端口
2. 接收客户端请求建立链接
3. 创建goroutine处理链接。

```go
package main

import (
   "bufio"
   "fmt"
   "net"
   "os"
   "strings"
)

// 客户端
func main() {
   // 1. 将客户端连接到 对应位置
   conn, err := net.Dial("tcp", "127.0.0.1:20000")
   if err != nil {
      fmt.Println("err :", err)
      return
   }
   fmt.Println("客户端已启动，连接到服务器: " + conn.RemoteAddr().String())
   defer conn.Close() // 关闭连接

   // 读控制台输入
   inputReader := bufio.NewReader(os.Stdin)
   for {
      fmt.Print("$:")
      input, _ := inputReader.ReadString('\n') // 读取用户输入
      inputInfo := strings.Trim(input, "\r\n") // \r\n 表示网络消息的结束
      if strings.ToUpper(inputInfo) == "Q" {   // 如果输入q就退出
         return
      }
      _, err = conn.Write([]byte(inputInfo)) // 向服务端发送数据
      if err != nil {
         return
      }
      // 从服务端读数据
      buf := [512]byte{}
      n, err := conn.Read(buf[:])
      if err != nil {
         fmt.Println("recv failed, err:", err)
         return
      }
      fmt.Println(string(buf[:n]))
   }
}
```

### 输出

![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/images/2022/12/13/20221213160512-161cf4.png)



## UDP-cs模型

### server

代码差不多：

```go
package main

import (
   "fmt"
   "net"
)

// UDP server端
func main() {
   listen, err := net.ListenUDP("udp", &net.UDPAddr{
      IP:   net.IPv4(0, 0, 0, 0), // 127.0.0.1
      Port: 30000,
   })
   if err != nil {
      fmt.Println("listen failed, err:", err)
      return
   }
   defer listen.Close()
   for {
      var data [1024]byte
      n, addr, err := listen.ReadFromUDP(data[:]) // 接收数据
      if err != nil {
         fmt.Println("read udp failed, err:", err)
         continue
      }
      fmt.Printf("data:%v addr:%v count:%v\n", string(data[:n]), addr, n)
      _, err = listen.WriteToUDP(data[:n], addr) // 发送数据
      if err != nil {
         fmt.Println("write to udp failed, err:", err)
         continue
      }
   }
}
```

可能需要权限：

<img src="https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/13/image-20221213161231516-0a3535.png" alt="image-20221213161231516" style="zoom:67%;" />

### client

```go
package main

import (
   "fmt"
   "net"
)

func main() {
   socket, err := net.DialUDP("udp", nil, &net.UDPAddr{
      IP:   net.IPv4(0, 0, 0, 0),
      Port: 30000,
   })
   if err != nil {
      fmt.Println("连接服务端失败，err:", err)
      return
   }
   defer socket.Close()
   sendData := []byte("Hello server")
   _, err = socket.Write(sendData) // 发送数据
   if err != nil {
      fmt.Println("发送数据失败，err:", err)
      return
   }
   data := make([]byte, 4096)
   n, remoteAddr, err := socket.ReadFromUDP(data) // 接收数据
   if err != nil {
      fmt.Println("接收数据失败，err:", err)
      return
   }
   fmt.Printf("recv:%v addr:%v count:%v\n", string(data[:n]), remoteAddr, n)
}
```

### 输出

启动一次就输出一行

![image-20221213161341669](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/13/image-20221213161341669-a6c188.png)



## tcp粘包问题

