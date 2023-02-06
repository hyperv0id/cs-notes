# lab1-util

## 启动xv6（简单）

下载代码并切换分支：

```bash
$ git clone git://g.csail.mit.edu/xv6-labs-2021
Cloning into 'xv6-labs-2021'...
...
$ cd xv6-labs-2021
$ git checkout util
Branch 'util' set up to track remote branch 'util' from 'origin'.
Switched to a new branch 'util'
```

编译运行，make编译，qemu模拟

```bash
$ make qemu # <C-a> + x退出
```

![image-20230130140010733](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/images/2023/01/30/image-20230130140010733-b9b57d.png)

## sleep（简单）

实现unix`sleep`程序，sleep程序可以按照用户指定的时间停止程序。

程序需要在 `user/sleep.c`中实现，文件未创建，我们将`user/echo.c`拷贝成我们的模板：

```c
#include "kernel/types.h"
#include "kernel/stat.h"
#include "user/user.h" # 三者引用关系不能交换顺序

int main(int argc, char *argv[])
{
  exit(0);
}
```

在`user.h`中已经定义了sleep函数，这里直接使用

```c
#include "kernel/types.h"
#include "kernel/stat.h"
#include "user/user.h" // 会使用 user.h 中的 atoi 函数

int main(int argc, char *argv[])
{
  // 如果用户未指定sleep时间，那么sleep 一秒
  if (argc < 2)
  {
    printf("Require sleep time, use: sleep 1");
    exit(1);
  }
  int sleep_time = atoi(argv[1]);
  printf("(nothing happens for a little while)");
  sleep(sleep_time);
  exit(0);
}

```

将`sleep`程序写入`MakeFile`中的`UPROGS`里，

```makefile
UPROGS=\
	...
	$U/_sleep\ # here!!!
```

测试程序：

```bash
$ ./grade-lab-util sleep
```

![image-20230130152357307](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/images/2023/01/30/image-20230130152357307-cc297e.png)

## pingpong（简单）

使用UNIX系统调用在两个进程之间通过一对管道“乒乓”一个字节，每个方向一个管道。

1. 父进程应该向子进程发送一个字节
2. 子进程应该输出：`<pid>: received ping`"，其中`<pid>`是它的进程号，把这个字节写在管道上给父进程，然后退出
3. 父进程应该从子进程读取字节，打印`<pid>: received pong`，然后退出。





## primes（中难）







## find（中等）







## xargs（中等）







## 提交

