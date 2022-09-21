# 实例&小测试

### 截取字符串
```go 
package main

import "fmt"

func main() {
	s := "hello world"
	sub := subString(s, 6, 11)
	fmt.Println(sub)

	fmt.Println(s[6:11]) // 只适合字符串中包含ASCII码的情况
}

func subString(str string, start, end int) string {
	runes := []rune(str)
	length := len(runes)
	if start < 0 {
		start = 0
	}
	if end > length {
		end = length
	}
	return string(runes[start:end])
}
```

### 循环实例
```go 
package main

import "fmt"

func main() {
	arr := []int{3, 2, 1, 4, 5, 8, 9, 7, 6}
	fmt.Println(arr)
	bubbleSort(arr)
	fmt.Println(arr)
}

func bubbleSort(arr []int) {
	for i := 0; i < len(arr)-1; i++ {
		for j := 0; j < len(arr)-i-1; j++ {
			if arr[j] > arr[j+1] {
				arr[j], arr[j+1] = arr[j+1], arr[j]
			}
		}
	}
}
```