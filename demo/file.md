# 文件读写

```
package main

import (
	"fmt"
	"github.com/pkg/errors"
	"io/ioutil"
	"log"
	"os"
)

func main() {
	fs, err := open("demo.txt")
	if err != nil {
		log.Fatal(err)
	}
	if err := write(fs, []byte("hello world\n")); err != nil {
		fmt.Println(err)
	}
	fs.Seek(0, 0)
	data, err := read(fs)
	if err != nil {
		log.Println("read error:", err)
		return
	}
	fmt.Println("data:", string(data))
}

func open(filename string) (*os.File, error) {
	fs, err := os.OpenFile(filename, os.O_CREATE|os.O_RDWR|os.O_APPEND, 0666)
	if err != nil {
		return nil, errors.Wrap(err, "open file error")
	}
	return fs, nil
}

func write(fs *os.File, data []byte) error {
	_, err := fs.Write(data)
	return err
}

func read(fs *os.File) ([]byte, error) {
	return ioutil.ReadAll(fs)
}
```