# 开始

初始化一个 golang 工程

```
mkdir go-start
cd go-start
go mod init crtrpt/go-start
```

写一个 go 文件 `main.go`

```
package main

import "fmt"

func main() {
	fmt.Printf("hello %s","world")
}
```

编译 go 源代码

```
go build main.go
```

执行可执行程序

```
main.exe
```

或者 直接执行

```
go run main.go
```
