# 小测试(预测代码输出)

## 数组 
### 数组修改
```go 
package main

import "fmt"

func editArr(arr [3]int) {
	for i := 0; i < len(arr); i++ {
		arr[i] *= 10
	}
	fmt.Println(arr)
}

func main() {
	arr := [...]int{1, 2, 3}
	editArr(arr)
	fmt.Println(arr)
}
```

## 切片
### 切片修改
```go 
package main

import "fmt"

func editSlice(s []int) {
	for i := 0; i < len(s); i++ {
		s[i] *= 10
	}
	fmt.Println(s)
}
func main() {
	arr := []int{1, 2, 3}
	editSlice(arr)
	fmt.Println(arr)
}
```
### 切片修改追加
```go 
package main

import "fmt"

func modifySlice(s []int, v ...int) {
	s = append(s, v...)
	for i := 0; i < len(s); i++ {
		s[i] *= 10
	}
	fmt.Println(s)
}

func main() {
	arr := []int{1, 2, 3}
	modifySlice(arr, 4, 5)
	fmt.Println(arr)
}

```
### 切片修改追加v2
```go 
package main

import "fmt"

func modifySlice(s []int, v ...int) {
	s = append(s, v...)
	for i := 0; i < len(s); i++ {
		s[i] *= 10
	}
	fmt.Println(s)
}

func main() {
	arr := make([]int, 3, 5)
	arr[0] = 1
	arr[1] = 2
	arr[2] = 3
	modifySlice(arr, 4, 5)
	fmt.Println(arr)
}

```

## 字符串
### 字符串修改
```go 
package main

import "fmt"

func main() {
	str := "hello world!"
	b := []byte(str)
	b[0] = 'H'
	fmt.Println(str)
	fmt.Println(string(b))
}
```