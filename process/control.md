# 1. 循环控制Goto、Break、Continue
循环控制语句

循环控制语句可以控制循环体内语句的执行过程。

GO 语言支持以下几种循环控制语句：

### 1.1.1. Goto、Break、Continue
    1.三个语句都可以配合标签(label)使用
    2.标签名区分大小写，定以后若不使用会造成编译错误
    3.continue、break配合标签(label)可用于多层循环跳出
    4.goto是调整执行位置，与continue、break配合标签(label)的结果并不相同

### break配合标签
```go 
package main

import (
 "fmt"
 "sync"
 "time"
)

func main() {
 close := make(chan bool)
 var wg sync.WaitGroup
 go f(close, &wg)

 time.Sleep(time.Second * 5)
 close <- true
 wg.Wait()
}

func f(close <-chan bool, wg *sync.WaitGroup) {
 wg.Add(1)
 defer wg.Done()
out:
 for {
  select {
  case <-close:
   break out
  default:
   time.Sleep(time.Second)
   fmt.Println("test...")
  }
 }
}

```
