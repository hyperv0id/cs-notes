## http协议



- 建立在**TCP**之上（http3.0 改用UDP了）



## 案例

```go
package main

import (
   "fmt"
   "net/http"
)

func main() {
   //http://127.0.0.1:8000/go
   // 单独写回调函数
   http.HandleFunc("/go", myHandler)
   //http.HandleFunc("/ungo",myHandler2 )
   // addr：监听的地址
   // handler：回调函数
   http.ListenAndServe("127.0.0.1:8000", nil)
}

// handler函数
func myHandler(w http.ResponseWriter, r *http.Request) {
   fmt.Println(r.RemoteAddr, "连接成功")
   // 请求方式：GET POST DELETE PUT UPDATE
   fmt.Println("method:", r.Method)
   // /go
   fmt.Println("url:", r.URL.Path)
   fmt.Println("header:", r.Header)
   fmt.Println("body:", r.Body)
   // 回复
   w.Write([]byte("<h1>Hello Stranger</h1>"))
}
```

启动后打开浏览器对应端口会有网页

![image-20221213162432607](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/13/image-20221213162432607-837a19.png)



