# 文件下载

```go 
package main

import (
	"flag"
	"fmt"
	"io"
	"net/http"
	"os"
	"strings"
)

func main() {
	var uri string
	var savePath string
	flag.StringVar(&uri, "url", "https://seva-file.oss-cn-shenzhen.aliyuncs.com/iot/picture/1661830713817.png", "下载文件地址")
	flag.StringVar(&savePath, "savePath", "./", "保存路径")
	flag.Parse()
	fileName, err := downloadFile(uri, savePath)
	if err != nil {
		fmt.Println("download file error:", err.Error())
		return
	}
	fmt.Printf("download (%s) success.\n", fileName)
}

func openFile(path, uri string) (*os.File, error) {
	if !strings.HasSuffix(path, "/") {
		path += "/"
	}
	_fileName := getFileName(uri)
	fileName := fmt.Sprintf("%s%s", path, _fileName)
	file, err := os.OpenFile(fileName, os.O_CREATE|os.O_TRUNC, 0666)
	if err != nil {
		return nil, err
	}
	return file, nil
}

func getFileName(uri string) string {
	lastIdx := strings.LastIndex(uri, "/")
	if lastIdx < 0 {
		return uri
	}
	return uri[lastIdx+1:]
}

func downloadFile(uri string, savePath string) (string, error) {
	resp, err := http.Get(uri)
	if err != nil {
		return "", err
	}
	defer resp.Body.Close()

	file, err := openFile(savePath, uri)
	if err != nil {
		return "", err
	}
	defer file.Close()
	_, err = io.Copy(file, resp.Body)
	if err != nil {
		return "", err
	}

	return file.Name(), nil
}

```