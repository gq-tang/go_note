# Go学习笔记-从入门到放弃

## 1. go 语言特点
* 天生支持并发
* 语法简单，容易上手
* 内置runtime，支持垃圾回收
* 可直接编译成机器码，不依赖其他库
* 丰富的标准库
* 可跨平台编译
* 部署维护成本低
## 2. go 语言应用领域
* 服务器编程
* 开发云平台
* 区块链
* 分布式系统
* 网络编程(适应网络IO密集型应用)

## 问题探讨
1、Golang的运行时环境，是基于什么写的？是C吗？

go是编译型语言,不需要单独的运行时环境.编译过程会内置运行时

[参考资料](https://blog.csdn.net/futurewu/article/details/104692651)

2、Golang的语法跟C相似吗？有指针操作吗？

go是类C语言.有指针操作.指针运算比较麻烦(暂时接触的应用场景比较少)
```
// 指针运算实例
func main() {
arr := [...]int{10, 20, 30}
arrP := unsafe.Pointer(&arr[0])
arr2 := uintptr(arrP) + unsafe.Sizeof(arr[0])
arr2Value := (*int)(unsafe.Pointer(arr2))
fmt.Println(*arr2Value)
}

```
3、Golang多进程中，地址空间是相互隔离的，还是有共享地址？

golang不适合做创建多进程的应用.一般是通过协程开发高并发应用.
go提倡不要通过共享内存来通信，要通过通信来共享内存

4、Golang的线程安全问题

go针对并发安全,提供了  原子操作(atomic.AddInt32只针对整数类型)/互斥锁(sync.Mutex)/读写互斥锁(sync.RWMutex)

5、Golang有好的web开发框架吗？类似nodejs的express这样的？

[https://github.com/labstack/echo](https://github.com/labstack/echo)

[https://github.com/gin-gonic/gin](https://github.com/gin-gonic/gin)

[其它web框架](https://github.com/smallnest/go-web-framework-benchmark)

6、Golang的资源调度是基于异步事件模型的吗？

7、Golang对异步和同步的处理是怎样的？

8、Golang的内存申请和销毁，可以人工介入吗？

不可以,只可以通过 runtime.GC()手动触发GC

9、Golang的垃圾回收机制是怎样的？级别是怎样定义的？

golang垃圾回收是通过三色标记法,混合写屏障机制实现
[参考资料](https://www.cnblogs.com/cxy2020/p/16321884.html)

10、Golang的线程调度算法有哪些？

[GMP 原理与调度](https://www.topgoer.com/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B/GMP%E5%8E%9F%E7%90%86%E4%B8%8E%E8%B0%83%E5%BA%A6.html)

11、Golang线程、进程间通信有哪几种方法？分别介绍下其特点

go协程是通过chanel通信.进程间通信支持pipe/signal/socket
[参考资料](https://blog.csdn.net/qq_42606136/article/details/121649364)
