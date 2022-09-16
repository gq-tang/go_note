# 1. runtime包
Golang进程权限调度包runtime三大函数Gosched、Goexit、GOMAXPROCS
runtime.Gosched()，用于让出CPU时间片，让出当前goroutine的执行权限，调度器安排其它等待的任务运行，并在下次某个时候从该位置恢复执行。这就像跑接力赛，A跑了一会碰到代码runtime.Gosched()就把接力棒交给B了，A歇着了，B继续跑。

runtime.Goexit()，调用此函数会立即使当前的goroutine的运行终止（终止协程），而其它的goroutine并不会受此影响。runtime.Goexit在终止当前goroutine前会先执行此goroutine的还未执行的defer语句。请注意千万别在主函数调用runtime.Goexit，因为会引发panic。

runtime.GOMAXPROCS()，用来设置可以并行计算的CPU核数最大值，并返回之前的值。

默认此函数的值与ＣＰＵ逻辑个数相同，即有多少个goroutine并发执行，当然可以设置它，它的取值是１～２５６。最好在主函数在开始前设置它，因为设置它会停止当前程序的运行。

GO默认是使用一个CPU核的，除非设置runtime.GOMAXPROCS
那么在多核环境下，什么情况下设置runtime.GOMAXPROCS会比较好的提高速度呢？

适合于CPU密集型、并行度比较高的情景。如果是IO密集型，CPU之间的切换也会带来性能的损失。
### 1.1.1. runtime.Gosched()
让出CPU时间片，重新等待安排任务
```
package main

import (
    "fmt"
    "runtime"
)

func main() {
    go func(s string) {
        for i := 0; i < 2; i++ {
            fmt.Println(s)
        }
    }("world")
    // 主协程
    for i := 0; i < 2; i++ {
        // 切一下，再次分配任务
        runtime.Gosched()
        fmt.Println("hello")
    }
}
```
### 1.1.2. runtime.Goexit()
退出当前协程
```
package main

import (
    "fmt"
    "runtime"
)

func main() {
    go func() {
        defer fmt.Println("A.defer")
        func() {
            defer fmt.Println("B.defer")
            // 结束协程
            runtime.Goexit()
            defer fmt.Println("C.defer")
            fmt.Println("B")
        }()
        fmt.Println("A")
    }()
    for {
    }
}
```
### 1.1.3. runtime.GOMAXPROCS
Go运行时的调度器使用GOMAXPROCS参数来确定需要使用多少个OS线程来同时执行Go代码。默认值是机器上的CPU核心数。例如在一个8核心的机器上，调度器会把Go代码同时调度到8个OS线程上（GOMAXPROCS是m:n调度中的n）。

Go语言中可以通过runtime.GOMAXPROCS()函数设置当前程序并发时占用的CPU逻辑核心数。

Go1.5版本之前，默认使用的是单核心执行。Go1.5版本之后，默认使用全部的CPU逻辑核心数。

我们可以通过将任务分配到不同的CPU逻辑核心上实现并行的效果，这里举个例子：
```
func a() {
    for i := 1; i < 10; i++ {
        fmt.Println("A:", i)
    }
}

func b() {
    for i := 1; i < 10; i++ {
        fmt.Println("B:", i)
    }
}

func main() {
    runtime.GOMAXPROCS(1)
    go a()
    go b()
    time.Sleep(time.Second)
}
```
两个任务只有一个逻辑核心，此时是做完一个任务再做另一个任务(问题点)。 将逻辑核心数设为2，此时两个任务并行执行，代码如下。
```
func a() {
    for i := 1; i < 10; i++ {
        fmt.Println("A:", i)
    }
}

func b() {
    for i := 1; i < 10; i++ {
        fmt.Println("B:", i)
    }
}

func main() {
    runtime.GOMAXPROCS(2)
    go a()
    go b()
    time.Sleep(time.Second)
}
```
Go语言中的操作系统线程和goroutine的关系：

1.一个操作系统线程对应用户态多个goroutine。
2.go程序可以同时使用多个操作系统线程。
3.goroutine和OS线程是多对多的关系，即m:n。