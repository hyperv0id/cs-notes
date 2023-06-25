## goroutine

>  goroutine 奉行通过通信来共享内存，而不是共享内存来通信。

goroutine 是由官方实现的超级"线程池"。

每个实力`4~5KB`的栈内存占用和由于实现机制而大幅减少的创建和销毁开销是go高并发的根本原因。

在golang中，你不需要关心如何管理分配线程，只需要执行 `go func(...)` 就可以实现并发。



## GMP

![golang-scheduler](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/13/2020-02-05-15808864354595-golang-scheduler-eec2f0.png)

（图片来自：[Go 语言调度器与 Goroutine 实现原理 | Go 语言设计与实现 (draveness.me)](https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-goroutine/#g)）





## resources

1. [[Golang三关-典藏版\] Golang 调度器 GMP 原理与调度全分析 | Go 技术论坛 (learnku.com)](https://learnku.com/articles/41728)
2. 