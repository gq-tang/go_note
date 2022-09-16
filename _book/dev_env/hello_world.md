# 终端 Hello world 
```go 
package main // 包名

import (
	"fmt"  // 导入依赖包
)

// main包的main函数为程序入口
func main(){
	fmt.Println("Hello world!")
}
```

# web 
```go 
package main

import (
	"fmt"
	"log"
	"net/http"
)

func main() {
	http.HandleFunc("/hello", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "hello world!")
	})
	log.Fatal(http.ListenAndServe(":8080", nil))
}
```