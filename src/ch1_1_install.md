# 安装

## 通用 windows 安装

https://golang.google.cn/dl/ 下载对应的 msi 文件 下一步 下一步安装

### 设置环境变量

右击我的电脑->属性->高级环境设置->环境变量
设置环境变量

```
GO111MODUL=on
GOPROXY=https://goproxy.io,direct
GOPATH=D:\go-path\
GOROOT=D:\Program Files\Go
```

PATH 追加 golang 二进制文件路径
我的环境设置 go env

```
C:\Users\l>go env
set GO111MODULE=on
set GOARCH=amd64
set GOBIN=
set GOCACHE=D:\go-build
set GOENV=C:\Users\l\AppData\Roaming\go\env
set GOEXE=.exe
set GOEXPERIMENT=
set GOFLAGS=
set GOHOSTARCH=amd64
set GOHOSTOS=windows
set GOINSECURE=
set GOMODCACHE=D:\go-path\pkg\mod
set GONOPROXY=
set GONOSUMDB=
set GOOS=windows
set GOPATH=D:\go-path\
set GOPRIVATE=
set GOPROXY=https://goproxy.io,direct
set GOROOT=D:\Program Files\Go
set GOSUMDB=sum.golang.org
set GOTMPDIR=
set GOTOOLDIR=D:\Program Files\Go\pkg\tool\windows_amd64
set GOVCS=
set GOVERSION=go1.19
set GCCGO=gccgo
set GOAMD64=v1
set AR=ar
set CC=gcc
set CXX=g++
set CGO_ENABLED=1
set GOMOD=NUL
set GOWORK=
set CGO_CFLAGS=-g -O2
set CGO_CPPFLAGS=
set CGO_CXXFLAGS=-g -O2
set CGO_FFLAGS=-g -O2
set CGO_LDFLAGS=-g -O2
set PKG_CONFIG=pkg-config
set GOGCCFLAGS=-m64 -mthreads -Wl,--no-gc-sections -fmessage-length=0 -fdebug-prefix-map=C:\Users\l\AppData\Local\Temp\go-build3670666351=/tmp/go-build -gno-record-gcc-switches
```

仅供参考

或者

使用 wsl 安装 golang 也是可以的
对应 liunx 安装即可

## liunx 安装

centos

```
yum install golang
```

ubuntu

```
apt get install golang
```

## macos 安装

需要安装好 homebrew

```
brew install golang
```
