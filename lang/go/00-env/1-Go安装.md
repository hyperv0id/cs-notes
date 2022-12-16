## Golang安装



下载地址：

官网（需要梯子）：[Downloads - The Go Programming Language](https://go.dev/dl/)

中文网：[Go下载 - Go语言中文网 - Golang中文社区 (studygolang.com)](https://studygolang.com/dl)



### windows安装

下载 `msi文件` 之后直接双击安装就行

### Linux下安装

#### 通用安装方式：

1. 安装 `mercurial` 、`git`、`gcc`
   1. centos: `yum install [包名]`
   2. ubuntu: `apt install [包名]`
2. 下载 go 压缩包并解压
   1. `cd /usr/local/`
   2. `wget https://go.dev/dl/go1.19.4.linux-amd64.tar.gz`
   3. `tar -zxvf go1.19.4.linux-amd64.tar.gz`



现在已经安装好 go 的包了，但是还得配环境

#### 配环境

编辑器打开 `/etc/profile`：

```bash
vim /etc/profile
```

添加对应环境变量：

```
export GOROOT=/usr/local/go        ##Golang安装目录
export PATH=$GOROOT/bin:$PATH
export GOPATH=/home/go  ##Golang项目目录
```

刷新环境变量：

```bash
source /etc/profile
```

测试环境：

```
go version
go env
```

![image-20221212163425463](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/12/image-20221212163425463-739643.png)

如果切换终端就失效的话：

```bash
sudo vi ~/.bashrc
```

在最后面添加：`source /etc/profile`





## 参考

1. [Go的安装 · Go语言中文文档 (topgoer.com)](https://www.topgoer.com/开发环境/go的安装.html)
2. [Downloads - The Go Programming Language](https://go.dev/dl/)
3. [Linux解决Source /etc/profile配置环境不生效_ZSYL的博客-CSDN博客_/etc/profile 不生效](https://blog.csdn.net/qq_46092061/article/details/118873206)