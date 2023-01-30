# 泛型

go golang 1.8 增加了 泛型特性

```
go mod init go-start
```

main.go 文件

```
package main

import "fmt"

//int 数据类型相加
func AddI(a int, b int) int {
	return a + b
}

//float 数据类型相加
func AddF(a float32, b float32) float32 {
	return a + b
}

//string数据类型相加 数据类型相加
func AddS(a string, b string) string {
	return a + b
}

func Add[T byte | int | int32 | float32 | string](a T, b T) T {
	return a + b
}

type PersonType interface {
	Crtrpt | Alice
	GetScore() int32
}

type Crtrpt struct {
	Score int32
}

func (c Crtrpt) GetScore() int32 {
	return c.Score
}

type Alice struct {
	Score int32
}

func (c Alice) GetScore() int32 {
	return c.Score
}

func PersonAdd[T PersonType, S PersonType](a T, b S) int32 {
	return (a).GetScore() + b.GetScore()
}

func main() {
	fmt.Printf("%v \r\n", AddI(1, 2))
	fmt.Printf("%v \r\n", AddF(1.1, 2.2))
	fmt.Printf("%v \r\n", AddS("20", "20"))
	fmt.Printf("%v \r\n", Add[int32](1, 2))
	fmt.Printf("%v \r\n", Add[float32](1, 2))
	fmt.Printf("%v \r\n", Add[string]("20", "20"))

	//返回 byte 1
	fmt.Printf("%v \r\n", Add[byte](255, 2))
	//返回 int32 257
	fmt.Printf("%v \r\n", Add[int32](255, 2))
	fmt.Printf("%v \r\n", Add(255, 2))
	fmt.Printf("%v \r\n", Add(byte(255), byte(2)))

	fmt.Printf("%v \r\n", PersonAdd(Crtrpt{Score: 20}, Alice{Score: 30}))

}
```

go 强类型语言
1.8 之前
如果 需要 实现相同的方法 需要对每个类型 实现一个
int 加 flat32 加 string 加
1.8 之后
可以类型参数化了

```
func Add[T int32 | float32 | string](a T, b T) T {
	return a + b
}
```

大大的减少了代码量

增加 byte 加

```
func Add[T byte | int32 | float32 | string](a T, b T) T
```

调用

```
	//返回 byte 1
	fmt.Printf("%v \r\n", Add[byte](255, 2))
	//返回 int32 257
	fmt.Printf("%v \r\n", Add[int32](255, 2))
```

注意当前版本的泛型

不支持 泛型继承 类似 java 的 < > 泛型约束
