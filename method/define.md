# 1. 方法定义
Golang 方法总是绑定对象实例，并隐式将实例作为第一实参 (receiver)。

• 只能为当前包内命名类型定义方法。
• 参数 receiver 可任意命名。如方法中未曾使用 ，可省略参数名。
• 参数 receiver 类型可以是 T 或 *T。基类型 T 不能是接口或指针。 
• 不支持方法重载，receiver 只是参数签名的组成部分。
• 可用实例 value 或 pointer 调用全部方法，编译器自动转换。
一个方法就是一个包含了接受者的函数，接受者可以是命名类型或者结构体类型的一个值或者是一个指针。

所有给定类型的方法属于该类型的方法集。

1.1.1. 方法定义：
    func (recevier type) methodName(参数列表)(返回值列表){}

    参数和返回值可以省略
package main

type Test struct{}

// 无参数、无返回值
func (t Test) method0() {

}

// 单参数、无返回值
func (t Test) method1(i int) {

}

// 多参数、无返回值
func (t Test) method2(x, y int) {

}

// 无参数、单返回值
func (t Test) method3() (i int) {
    return
}

// 多参数、多返回值
func (t Test) method4(x, y int) (z int, err error) {
    return
}

// 无参数、无返回值
func (t *Test) method5() {

}

// 单参数、无返回值
func (t *Test) method6(i int) {

}

// 多参数、无返回值
func (t *Test) method7(x, y int) {

}

// 无参数、单返回值
func (t *Test) method8() (i int) {
    return
}

// 多参数、多返回值
func (t *Test) method9(x, y int) (z int, err error) {
    return
}

func main() {}
下面定义一个结构体类型和该类型的一个方法：
```go 
package main

import (
    "fmt"
)

//结构体
type User struct {
    Name  string
    Email string
}

//方法
func (u User) Notify() {
    fmt.Printf("%v : %v \n", u.Name, u.Email)
}
func main() {
    // 值类型调用方法
    u1 := User{"golang", "golang@golang.com"}
    u1.Notify()
    // 指针类型调用方法
    u2 := User{"go", "go@go.com"}
    u3 := &u2
    u3.Notify()
}
```
输出结果：

    golang : golang@golang.com 
    go : go@go.com
解释： 首先我们定义了一个叫做 User 的结构体类型，然后定义了一个该类型的方法叫做 Notify，该方法的接受者是一个 User 类型的值。要调用 Notify 方法我们需要一个 User 类型的值或者指针。

在这个例子中当我们使用指针时，Go 调整和解引用指针使得调用可以被执行。注意，当接受者不是一个指针时，该方法操作对应接受者的值的副本(意思就是即使你使用了指针调用函数，但是函数的接受者是值类型，所以函数内部操作还是对副本的操作，而不是指针操作。

我们修改 Notify 方法，让它的接受者使用指针类型：
```go 
package main

import (
    "fmt"
)

//结构体
type User struct {
    Name  string
    Email string
}

//方法
func (u *User) Notify() {
    fmt.Printf("%v : %v \n", u.Name, u.Email)
}
func main() {
    // 值类型调用方法
    u1 := User{"golang", "golang@golang.com"}
    u1.Notify()
    // 指针类型调用方法
    u2 := User{"go", "go@go.com"}
    u3 := &u2
    u3.Notify()
}
```
输出结果：

    golang : golang@golang.com 
    go : go@go.com
注意：当接受者是指针时，即使用值类型调用那么函数内部也是对指针的操作。

方法不过是一种特殊的函数，只需将其还原，就知道 receiver T 和 *T 的差别。
```go 
package main

import "fmt"

type Data struct {
    x int
}

func (self Data) ValueTest() { // func ValueTest(self Data);
    fmt.Printf("Value: %p\n", &self)
}

func (self *Data) PointerTest() { // func PointerTest(self *Data);
    fmt.Printf("Pointer: %p\n", self)
}

func main() {
    d := Data{}
    p := &d
    fmt.Printf("Data: %p\n", p)

    d.ValueTest()   // ValueTest(d)
    d.PointerTest() // PointerTest(&d)

    p.ValueTest()   // ValueTest(*p)
    p.PointerTest() // PointerTest(p)
}
```
输出:

    Data: 0xc42007c008
    Value: 0xc42007c018
    Pointer: 0xc42007c008
    Value: 0xc42007c020
    Pointer: 0xc42007c008
1.1.2. 普通函数与方法的区别
1.对于普通函数，接收者为值类型时，不能将指针类型的数据直接传递，反之亦然。

2.对于方法（如struct的方法），接收者为值类型时，可以直接用指针类型的变量调用方法，反过来同样也可以。
```go 
package main

//普通函数与方法的区别（在接收者分别为值类型和指针类型的时候）

import (
    "fmt"
)

//1.普通函数
//接收值类型参数的函数
func valueIntTest(a int) int {
    return a + 10
}

//接收指针类型参数的函数
func pointerIntTest(a *int) int {
    return *a + 10
}

func structTestValue() {
    a := 2
    fmt.Println("valueIntTest:", valueIntTest(a))
    //函数的参数为值类型，则不能直接将指针作为参数传递
    //fmt.Println("valueIntTest:", valueIntTest(&a))
    //compile error: cannot use &a (type *int) as type int in function argument

    b := 5
    fmt.Println("pointerIntTest:", pointerIntTest(&b))
    //同样，当函数的参数为指针类型时，也不能直接将值类型作为参数传递
    //fmt.Println("pointerIntTest:", pointerIntTest(b))
    //compile error:cannot use b (type int) as type *int in function argument
}

//2.方法
type PersonD struct {
    id   int
    name string
}

//接收者为值类型
func (p PersonD) valueShowName() {
    fmt.Println(p.name)
}

//接收者为指针类型
func (p *PersonD) pointShowName() {
    fmt.Println(p.name)
}

func structTestFunc() {
    //值类型调用方法
    personValue := PersonD{101, "hello world"}
    personValue.valueShowName()
    personValue.pointShowName()

    //指针类型调用方法
    personPointer := &PersonD{102, "hello golang"}
    personPointer.valueShowName()
    personPointer.pointShowName()

    //与普通函数不同，接收者为指针类型和值类型的方法，指针类型和值类型的变量均可相互调用
}

func main() {
    structTestValue()
    structTestFunc()
}
```
输出结果：

    valueIntTest: 12
    pointerIntTest: 15
    hello world
    hello world
    hello golang
    hello golang