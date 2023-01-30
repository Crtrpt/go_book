# 多个模块的工作区

使用 gowork 模块 2 调用模块 1 输出字符串

创建模块 1

```
mkdir func1
cd func1
go mod init func1
```

创建文件 `func1.go`

```
package func1

func Func1() string {
	return "Func1  =====>"
}

```

创建 go work

```
go work init ./func1
```

自动创建 `go.work` 文件

```
go 1.19

use ./func1

```

创建模块 2

```
mkdir func2
cd func2
go mod init func2
```

创建 main.go 文件

```
package main

import (
	"crtrpt.com/func1"
)

func main() {
	println(func1.Func1())
}

```

执行

```
go run .\main.go
```

输出

```
Func1  =====>
```

非常好用比原先的 gopath 管理项目舒服多了.

目录结构

```
PS D:\go-book\go-start> tree /F
卷 LENOVO 的文件夹 PATH 列表
卷序列号为 9E4A-CC11
D:.
│  go.work
│
├─func1
│      func1.go
│      go.mod
│
└─func2
        go.mod
        main.go
```
