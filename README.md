# Go学习笔记-从入门到不放弃

## 1. go 语言特点
* 天生支持并发
* 语法简单，容易上手
* 内置runtime，支持垃圾回收
* 可直接编译成机器码，不依赖其他库
* 丰富的便准库
* 可跨平台编译
* 部署维护成本低
## 2. go 语言应用领域
* 服务器编程
* 开发云平台
* 区块链
* 分布式系统
* 网络编程

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
4、Golang的线程安全问题
5、Golang有好的web开发框架吗？类似nodejs的express这样的？
6、Golang的资源调度是基于异步事件模型的吗？
7、Golang对异步和同步的处理是怎样的？
8、Golang的内存申请和销毁，可以人工介入吗？
9、Golang的垃圾回收机制是怎样的？级别是怎样定义的？
10、Golang的线程调度算法有哪些？
11、Golang线程、进程间通信有哪几种方法？分别介绍下其特点