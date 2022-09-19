# 1. Go语言的主要特征

## 1.1 优点
自带gc。

静态编译，编译好后，扔服务器直接运行。

简单的思想，没有继承，多态，类等。

丰富的库和详细的开发文档。

语法层支持并发，和拥有同步并发的channel类型，使并发开发变得非常方便。

简洁的语法，提高开发效率，同时提高代码的阅读性和可维护性。

超级简单的交叉编译，仅需更改环境变量。

## 1.2 Go语言的主要特征
    1. 自动立即回收。
    2. 更丰富的内置类型。
    3. 函数多返回值。
    4. 错误处理。
    5. 匿名函数和闭包。
    6. 类型和接口。
    7. 并发编程。
    8. 反射。
    9. 跨平台肯支持交差编译。

## 1.3 命名规则
### 1.3.1 Go的函数、变量、常量、自定义类型、包(package)的命名方式遵循以下规则：
    1. 首字符可以是任意的Unicode字符或者下划线
    2. 剩余字符可以是Unicode字符、下划线、数字
    3. 字符长度不限
    4. 不能是关键字及保留字

### 1.3.2 Go只有25个关键字
```
    break        default      func         interface    select
    case         defer        go           map          struct
    chan         else         goto         package      switch
    const        fallthrough  if           range        type
    continue     for          import       return       var
```
### 1.3.3 Go还有37个保留字
```
    Constants:    true  false  iota  nil

    Types:    int  int8  int16  int32  int64  
              uint  uint8  uint16  uint32  uint64  uintptr
              float32  float64  complex128  complex64
              bool  byte  rune  string  error

    Functions:   make  len  cap  new  append  copy  close  delete
                 complex  real  imag
                 panic  recover
```
### 1.3.4 可见性：
    1. 声明在函数内部，是函数的本地值，类似private
    2. 声明在函数外部，是对当前包可见(包内所有.go文件都可见)的全局值，类似protect
    3. 声明在函数外部且首字母大写是所有包可见的全局值,类似public      

## 1.4 Go语言声明：
有四种主要声明方式：

    var（声明变量）, const（声明常量）, type（声明类型） ,func（声明函数）。
Go的程序是保存在多个.go文件中，文件的第一行就是package XXX声明，用来说明该文件属于哪个包(package)，package声明下来就是import声明，再下来是类型，变量，常量，函数的声明。