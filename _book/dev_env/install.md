# 1. go的安装

## 1.1 下载地址(请选择相应平台的安装包下载安装)
Go官网下载地址(需翻墙)：[https://golang.org/dl](https://golang.org/dl)

Go语言中文网[https://studygolang.com/dl](https://studygolang.com/dl)

## 1.2 设置环境变量
```shell
export GOROOT=/usr/local/go      // Go的安装目录
export PATH=$GOROOT/bin:$PATH    // go install安装目录
export GOPATH=/home/go           // Go mod安装目录
```
## 1.3 验证Go安装
``` 
PS C:\Users\DELL> go version
go version go1.18.2 windows/amd64
PS C:\Users\DELL>
```
## 1.4 设置GO模块代理
```
go env -w GOPROXY=https://goproxy.cn,direct
```
## 1.4 查看Go环境配置项
```
PS C:\Users\DELL> go env
set GO111MODULE=
set GOARCH=amd64
set GOBIN=
set GOCACHE=C:\Users\DELL\AppData\Local\go-build
set GOENV=C:\Users\DELL\AppData\Roaming\go\env
set GOEXE=.exe
set GOEXPERIMENT=
set GOFLAGS=
set GOHOSTARCH=amd64
set GOHOSTOS=windows
set GOINSECURE=
set GOMODCACHE=E:\go_path\pkg\mod
set GONOPROXY=
set GONOSUMDB=
set GOOS=windows
set GOPATH=E:\go_path
set GOPRIVATE=
set GOPROXY=https://goproxy.cn,direct
set GOROOT=C:\Program Files\Go
set GOSUMDB=sum.golang.org
set GOTMPDIR=
set GOTOOLDIR=C:\Program Files\Go\pkg\tool\windows_amd64
set GOVCS=
set GOVERSION=go1.18.2
set GCCGO=gccgo
set GOAMD64=v1
set AR=ar
set CC=gcc
set CXX=g++
```
# 2. 线上运行环境
[https://play.goplus.org/](https://play.goplus.org/)