# 运行时(runtime)

通过 runtime 包 和系统运行时进行交互 类似协程控制 和 低级别反射函数

## 环境变量

通过 系统环境变量的设置 控制 golang 运行时行为

`GOGC` 设置垃圾收集百分比 当新分配的数据与上次收集后剩余的实时数据的比率达到此百分比时，将触发收集。 默认 GoGc=100 设置 GoGc=off 将完全禁止垃圾收集

代码

```
runtime/debug.SetGCPercent
```

运行时修改百分比

`GOMEMLIMIT` 设置内存使用量 ` B, KiB, MiB, GiB, and TiB` 默认 `math.MaxInt64`

代码

```
runtime/debug.SetMemoryLimit
```

运行时修改内存使用量

`GODEBUG`

```
allocfreetrace: setting allocfreetrace=1 causes every allocation to be
profiled and a stack trace printed on each object's allocation and free.

clobberfree: setting clobberfree=1 causes the garbage collector to
clobber the memory content of an object with bad content when it frees
the object.

cgocheck: setting cgocheck=0 disables all checks for packages
using cgo to incorrectly pass Go pointers to non-Go code.
Setting cgocheck=1 (the default) enables relatively cheap
checks that may miss some errors.  Setting cgocheck=2 enables
expensive checks that should not miss any errors, but will
cause your program to run slower.

efence: setting efence=1 causes the allocator to run in a mode
where each object is allocated on a unique page and addresses are
never recycled.

gccheckmark: setting gccheckmark=1 enables verification of the
garbage collector's concurrent mark phase by performing a
second mark pass while the world is stopped.  If the second
pass finds a reachable object that was not found by concurrent
mark, the garbage collector will panic.

gcpacertrace: setting gcpacertrace=1 causes the garbage collector to
print information about the internal state of the concurrent pacer.

gcshrinkstackoff: setting gcshrinkstackoff=1 disables moving goroutines
onto smaller stacks. In this mode, a goroutine's stack can only grow.

gcstoptheworld: setting gcstoptheworld=1 disables concurrent garbage collection,
making every garbage collection a stop-the-world event. Setting gcstoptheworld=2
also disables concurrent sweeping after the garbage collection finishes.

gctrace: setting gctrace=1 causes the garbage collector to emit a single line to standard
error at each collection, summarizing the amount of memory collected and the
length of the pause. The format of this line is subject to change.
Currently, it is:
	gc # @#s #%: #+#+# ms clock, #+#/#/#+# ms cpu, #->#-># MB, # MB goal, # P
where the fields are as follows:
	gc #         the GC number, incremented at each GC
	@#s          time in seconds since program start
	#%           percentage of time spent in GC since program start
	#+...+#      wall-clock/CPU times for the phases of the GC
	#->#-># MB   heap size at GC start, at GC end, and live heap
	# MB goal    goal heap size
	# MB stacks  estimated scannable stack size
	# MB globals scannable global size
	# P          number of processors used
The phases are stop-the-world (STW) sweep termination, concurrent
mark and scan, and STW mark termination. The CPU times
for mark/scan are broken down in to assist time (GC performed in
line with allocation), background GC time, and idle GC time.
If the line ends with "(forced)", this GC was forced by a
runtime.GC() call.

harddecommit: setting harddecommit=1 causes memory that is returned to the OS to
also have protections removed on it. This is the only mode of operation on Windows,
but is helpful in debugging scavenger-related issues on other platforms. Currently,
only supported on Linux.

inittrace: setting inittrace=1 causes the runtime to emit a single line to standard
error for each package with init work, summarizing the execution time and memory
allocation. No information is printed for inits executed as part of plugin loading
and for packages without both user defined and compiler generated init work.
The format of this line is subject to change. Currently, it is:
	init # @#ms, # ms clock, # bytes, # allocs
where the fields are as follows:
	init #      the package name
	@# ms       time in milliseconds when the init started since program start
	# clock     wall-clock time for package initialization work
	# bytes     memory allocated on the heap
	# allocs    number of heap allocations

madvdontneed: setting madvdontneed=0 will use MADV_FREE
instead of MADV_DONTNEED on Linux when returning memory to the
kernel. This is more efficient, but means RSS numbers will
drop only when the OS is under memory pressure.

memprofilerate: setting memprofilerate=X will update the value of runtime.MemProfileRate.
When set to 0 memory profiling is disabled.  Refer to the description of
MemProfileRate for the default value.

invalidptr: invalidptr=1 (the default) causes the garbage collector and stack
copier to crash the program if an invalid pointer value (for example, 1)
is found in a pointer-typed location. Setting invalidptr=0 disables this check.
This should only be used as a temporary workaround to diagnose buggy code.
The real fix is to not store integers in pointer-typed locations.

sbrk: setting sbrk=1 replaces the memory allocator and garbage collector
with a trivial allocator that obtains memory from the operating system and
never reclaims any memory.

scavtrace: setting scavtrace=1 causes the runtime to emit a single line to standard
error, roughly once per GC cycle, summarizing the amount of work done by the
scavenger as well as the total amount of memory returned to the operating system
and an estimate of physical memory utilization. The format of this line is subject
to change, but currently it is:
	scav # KiB work, # KiB total, #% util
where the fields are as follows:
	# KiB work   the amount of memory returned to the OS since the last line
	# KiB total  the total amount of memory returned to the OS
	#% util      the fraction of all unscavenged memory which is in-use
If the line ends with "(forced)", then scavenging was forced by a
debug.FreeOSMemory() call.

scheddetail: setting schedtrace=X and scheddetail=1 causes the scheduler to emit
detailed multiline info every X milliseconds, describing state of the scheduler,
processors, threads and goroutines.

schedtrace: setting schedtrace=X causes the scheduler to emit a single line to standard
error every X milliseconds, summarizing the scheduler state.

tracebackancestors: setting tracebackancestors=N extends tracebacks with the stacks at
which goroutines were created, where N limits the number of ancestor goroutines to
report. This also extends the information returned by runtime.Stack. Ancestor's goroutine
IDs will refer to the ID of the goroutine at the time of creation; it's possible for this
ID to be reused for another goroutine. Setting N to 0 will report no ancestry information.

asyncpreemptoff: asyncpreemptoff=1 disables signal-based
asynchronous goroutine preemption. This makes some loops
non-preemptible for long periods, which may delay GC and
goroutine scheduling. This is useful for debugging GC issues
because it also disables the conservative stack scanning used
for asynchronously preempted goroutines.
```

`GOMAXPROCS` 配置 GOMAXPROCS 变量限制了可以同时执行用户级 Go 代码的操作系统线程数。 代表 Go 代码在系统调用中可以阻塞的线程数没有限制； 这些不计入 GOMAXPROCS 限制。 这个包的 GOMAXPROCS 函数查询和更改限制。

`GORACE` 配置程序的竞争状态检测 通过 `-race` 设置开启
单独开个 wiki 写下

`GOTRACEBACK`
GOTRACEBACK=none 忽略调用栈
GOTRACEBACK=single（默认值）打印用户级别的调用栈

```
package main

import (
	"fmt"
	"time"
)

func Func2() {
	fmt.Println("func2 ===>")
	panic("func2 panic")
}
func Func1() {
	fmt.Println("func1 ===>")
	Func2()
}
func Func() {
	for {
		time.Sleep(5 * time.Second)
	}
}
func main() {
	// debug.SetTraceback("all")
	go Func()
	Func1()
}

```

输出

```
PS D:\go-book\go-start> go run .\main.go
func1 ===>
func2 ===>
panic: func2 panic

goroutine 1 [running]:
main.Func2(...)
        D:/go-book/go-start/main.go:10
main.Func1()
        D:/go-book/go-start/main.go:14 +0xa8
main.main()
        D:/go-book/go-start/main.go:24 +0x2a
exit status 2
```

GOTRACEBACK=all 所有 打印全部的 go goroutine 调用栈

```
package main

import (
	"fmt"
	"runtime/debug"
	"time"
)

func Func2() {
	fmt.Println("func2 ===>")
	panic("func2 panic")
}
func Func1() {
	fmt.Println("func1 ===>")
	Func2()
}
func Func() {
	for {
		time.Sleep(5 * time.Second)
	}
}
func main() {
	debug.SetTraceback("all")
	go Func()
	Func1()
}

```

输出

```
PS D:\go-book\go-start> go run .\main.go
func1 ===>
func2 ===>
panic: func2 panic

goroutine 1 [running]:
main.Func2(...)
        D:/go-book/go-start/main.go:11
main.Func1()
        D:/go-book/go-start/main.go:15 +0xa8
main.main()
        D:/go-book/go-start/main.go:25 +0x36

goroutine 6 [sleep]:
time.Sleep(0x12a05f200)
        D:/Program Files/Go/src/runtime/time.go:195 +0x13c
main.Func()
        D:/go-book/go-start/main.go:19 +0x25
created by main.main
        D:/go-book/go-start/main.go:24 +0x31
exit status 2
```

GOTRACEBACK=system 系统
代码

```
package main

import (
	"fmt"
	"runtime/debug"
	"time"
)

func Func2() {
	fmt.Println("func2 ===>")
	panic("func2 panic")
}
func Func1() {
	fmt.Println("func1 ===>")
	Func2()
}
func Func() {
	for {
		time.Sleep(5 * time.Second)
	}
}
func main() {
	debug.SetTraceback("system")
	go Func()
	Func1()
}
PS D:\go-book\go-start> go run .\main.go
func1 ===>
func2 ===>
panic: func2 panic

goroutine 1 [running]:
panic({0xfd6da0, 0x100b8a8})
        D:/Program Files/Go/src/runtime/panic.go:987 +0x3ba fp=0xc000095f08 sp=0xc000095e48 pc=0xf7463a
main.Func2(...)
        D:/go-book/go-start/main.go:11
main.Func1()
        D:/go-book/go-start/main.go:15 +0xa8 fp=0xc000095f60 sp=0xc000095f08 pc=0xfce7a8
main.main()
        D:/go-book/go-start/main.go:25 +0x36 fp=0xc000095f80 sp=0xc000095f60 pc=0xfce836
runtime.main()
        D:/Program Files/Go/src/runtime/proc.go:250 +0x1fe fp=0xc000095fe0 sp=0xc000095f80 pc=0xf771be
runtime.goexit()
        D:/Program Files/Go/src/runtime/asm_amd64.s:1594 +0x1 fp=0xc000095fe8 sp=0xc000095fe0 pc=0xf9e481

goroutine 2 [force gc (idle)]:
runtime.gopark(0x0?, 0x0?, 0x0?, 0x0?, 0x0?)
        D:/Program Files/Go/src/runtime/proc.go:363 +0xd6 fp=0xc00004ffb0 sp=0xc00004ff90 pc=0xf77556
runtime.goparkunlock(...)
        D:/Program Files/Go/src/runtime/proc.go:369
runtime.forcegchelper()
        D:/Program Files/Go/src/runtime/proc.go:302 +0xb1 fp=0xc00004ffe0 sp=0xc00004ffb0 pc=0xf773f1
runtime.goexit()
        D:/Program Files/Go/src/runtime/asm_amd64.s:1594 +0x1 fp=0xc00004ffe8 sp=0xc00004ffe0 pc=0xf9e481
created by runtime.init.6
        D:/Program Files/Go/src/runtime/proc.go:290 +0x25

goroutine 3 [GC sweep wait]:
runtime.gopark(0x0?, 0x0?, 0x0?, 0x0?, 0x0?)
        D:/Program Files/Go/src/runtime/proc.go:363 +0xd6 fp=0xc000051f90 sp=0xc000051f70 pc=0xf77556
runtime.goparkunlock(...)
        D:/Program Files/Go/src/runtime/proc.go:369
runtime.bgsweep(0x0?)
        D:/Program Files/Go/src/runtime/mgcsweep.go:278 +0x8e fp=0xc000051fc8 sp=0xc000051f90 pc=0xf6218e
runtime.gcenable.func1()
        D:/Program Files/Go/src/runtime/mgc.go:178 +0x26 fp=0xc000051fe0 sp=0xc000051fc8 pc=0xf56f26
runtime.goexit()
        D:/Program Files/Go/src/runtime/asm_amd64.s:1594 +0x1 fp=0xc000051fe8 sp=0xc000051fe0 pc=0xf9e481
created by runtime.gcenable
        D:/Program Files/Go/src/runtime/mgc.go:178 +0x6b

goroutine 4 [GC scavenge wait]:
runtime.gopark(0xc000024070?, 0x100b820?, 0x1?, 0x0?, 0x0?)
        D:/Program Files/Go/src/runtime/proc.go:363 +0xd6 fp=0xc000061f70 sp=0xc000061f50 pc=0xf77556
runtime.goparkunlock(...)
        D:/Program Files/Go/src/runtime/proc.go:369
runtime.(*scavengerState).park(0x1083780)
        D:/Program Files/Go/src/runtime/mgcscavenge.go:389 +0x53 fp=0xc000061fa0 sp=0xc000061f70 pc=0xf60213
runtime.bgscavenge(0x0?)
        D:/Program Files/Go/src/runtime/mgcscavenge.go:617 +0x45 fp=0xc000061fc8 sp=0xc000061fa0 pc=0xf60805
runtime.gcenable.func2()
        D:/Program Files/Go/src/runtime/mgc.go:179 +0x26 fp=0xc000061fe0 sp=0xc000061fc8 pc=0xf56ec6
runtime.goexit()
        D:/Program Files/Go/src/runtime/asm_amd64.s:1594 +0x1 fp=0xc000061fe8 sp=0xc000061fe0 pc=0xf9e481
created by runtime.gcenable
        D:/Program Files/Go/src/runtime/mgc.go:179 +0xaa

goroutine 5 [finalizer wait]:
runtime.gopark(0xf778f7?, 0xf77325?, 0x0?, 0x0?, 0xc000053f70?)
        D:/Program Files/Go/src/runtime/proc.go:363 +0xd6 fp=0xc000053e28 sp=0xc000053e08 pc=0xf77556
runtime.goparkunlock(...)
        D:/Program Files/Go/src/runtime/proc.go:369
runtime.runfinq()
        D:/Program Files/Go/src/runtime/mfinal.go:180 +0x10f fp=0xc000053fe0 sp=0xc000053e28 pc=0xf5602f
runtime.goexit()
        D:/Program Files/Go/src/runtime/asm_amd64.s:1594 +0x1 fp=0xc000053fe8 sp=0xc000053fe0 pc=0xf9e481
created by runtime.createfing
        D:/Program Files/Go/src/runtime/mfinal.go:157 +0x45

goroutine 6 [sleep]:
runtime.gopark(0xa22fc9e1b7d40?, 0x0?, 0x0?, 0x0?, 0x0?)
        D:/Program Files/Go/src/runtime/proc.go:363 +0xd6 fp=0xc000063f88 sp=0xc000063f68 pc=0xf77556
time.Sleep(0x12a05f200)
        D:/Program Files/Go/src/runtime/time.go:195 +0x13c fp=0xc000063fc8 sp=0xc000063f88 pc=0xf9b23c
main.Func()
        D:/go-book/go-start/main.go:19 +0x25 fp=0xc000063fe0 sp=0xc000063fc8 pc=0xfce7e5
runtime.goexit()
        D:/Program Files/Go/src/runtime/asm_amd64.s:1594 +0x1 fp=0xc000063fe8 sp=0xc000063fe0 pc=0xf9e481
created by main.main
        D:/go-book/go-start/main.go:24 +0x31
exit status 2
```

输出

```
PS D:\go-book\go-start> go run .\main.go
func1 ===>
func2 ===>
panic: func2 panic

goroutine 1 [running]:
panic({0xfd6da0, 0x100b8a8})
        D:/Program Files/Go/src/runtime/panic.go:987 +0x3ba fp=0xc000095f08 sp=0xc000095e48 pc=0xf7463a
main.Func2(...)
        D:/go-book/go-start/main.go:11
main.Func1()
        D:/go-book/go-start/main.go:15 +0xa8 fp=0xc000095f60 sp=0xc000095f08 pc=0xfce7a8
main.main()
        D:/go-book/go-start/main.go:25 +0x36 fp=0xc000095f80 sp=0xc000095f60 pc=0xfce836
runtime.main()
        D:/Program Files/Go/src/runtime/proc.go:250 +0x1fe fp=0xc000095fe0 sp=0xc000095f80 pc=0xf771be
runtime.goexit()
        D:/Program Files/Go/src/runtime/asm_amd64.s:1594 +0x1 fp=0xc000095fe8 sp=0xc000095fe0 pc=0xf9e481

goroutine 2 [force gc (idle)]:
runtime.gopark(0x0?, 0x0?, 0x0?, 0x0?, 0x0?)
        D:/Program Files/Go/src/runtime/proc.go:363 +0xd6 fp=0xc00004ffb0 sp=0xc00004ff90 pc=0xf77556
runtime.goparkunlock(...)
        D:/Program Files/Go/src/runtime/proc.go:369
runtime.forcegchelper()
        D:/Program Files/Go/src/runtime/proc.go:302 +0xb1 fp=0xc00004ffe0 sp=0xc00004ffb0 pc=0xf773f1
runtime.goexit()
        D:/Program Files/Go/src/runtime/asm_amd64.s:1594 +0x1 fp=0xc00004ffe8 sp=0xc00004ffe0 pc=0xf9e481
created by runtime.init.6
        D:/Program Files/Go/src/runtime/proc.go:290 +0x25

goroutine 3 [GC sweep wait]:
runtime.gopark(0x0?, 0x0?, 0x0?, 0x0?, 0x0?)
        D:/Program Files/Go/src/runtime/proc.go:363 +0xd6 fp=0xc000051f90 sp=0xc000051f70 pc=0xf77556
runtime.goparkunlock(...)
        D:/Program Files/Go/src/runtime/proc.go:369
runtime.bgsweep(0x0?)
        D:/Program Files/Go/src/runtime/mgcsweep.go:278 +0x8e fp=0xc000051fc8 sp=0xc000051f90 pc=0xf6218e
runtime.gcenable.func1()
        D:/Program Files/Go/src/runtime/mgc.go:178 +0x26 fp=0xc000051fe0 sp=0xc000051fc8 pc=0xf56f26
runtime.goexit()
        D:/Program Files/Go/src/runtime/asm_amd64.s:1594 +0x1 fp=0xc000051fe8 sp=0xc000051fe0 pc=0xf9e481
created by runtime.gcenable
        D:/Program Files/Go/src/runtime/mgc.go:178 +0x6b

goroutine 4 [GC scavenge wait]:
runtime.gopark(0xc000024070?, 0x100b820?, 0x1?, 0x0?, 0x0?)
        D:/Program Files/Go/src/runtime/proc.go:363 +0xd6 fp=0xc000061f70 sp=0xc000061f50 pc=0xf77556
runtime.goparkunlock(...)
        D:/Program Files/Go/src/runtime/proc.go:369
runtime.(*scavengerState).park(0x1083780)
        D:/Program Files/Go/src/runtime/mgcscavenge.go:389 +0x53 fp=0xc000061fa0 sp=0xc000061f70 pc=0xf60213
runtime.bgscavenge(0x0?)
        D:/Program Files/Go/src/runtime/mgcscavenge.go:617 +0x45 fp=0xc000061fc8 sp=0xc000061fa0 pc=0xf60805
runtime.gcenable.func2()
        D:/Program Files/Go/src/runtime/mgc.go:179 +0x26 fp=0xc000061fe0 sp=0xc000061fc8 pc=0xf56ec6
runtime.goexit()
        D:/Program Files/Go/src/runtime/asm_amd64.s:1594 +0x1 fp=0xc000061fe8 sp=0xc000061fe0 pc=0xf9e481
created by runtime.gcenable
        D:/Program Files/Go/src/runtime/mgc.go:179 +0xaa

goroutine 5 [finalizer wait]:
runtime.gopark(0xf778f7?, 0xf77325?, 0x0?, 0x0?, 0xc000053f70?)
        D:/Program Files/Go/src/runtime/proc.go:363 +0xd6 fp=0xc000053e28 sp=0xc000053e08 pc=0xf77556
runtime.goparkunlock(...)
        D:/Program Files/Go/src/runtime/proc.go:369
runtime.runfinq()
        D:/Program Files/Go/src/runtime/mfinal.go:180 +0x10f fp=0xc000053fe0 sp=0xc000053e28 pc=0xf5602f
runtime.goexit()
        D:/Program Files/Go/src/runtime/asm_amd64.s:1594 +0x1 fp=0xc000053fe8 sp=0xc000053fe0 pc=0xf9e481
created by runtime.createfing
        D:/Program Files/Go/src/runtime/mfinal.go:157 +0x45

goroutine 6 [sleep]:
runtime.gopark(0xa22fc9e1b7d40?, 0x0?, 0x0?, 0x0?, 0x0?)
        D:/Program Files/Go/src/runtime/proc.go:363 +0xd6 fp=0xc000063f88 sp=0xc000063f68 pc=0xf77556
time.Sleep(0x12a05f200)
        D:/Program Files/Go/src/runtime/time.go:195 +0x13c fp=0xc000063fc8 sp=0xc000063f88 pc=0xf9b23c
main.Func()
        D:/go-book/go-start/main.go:19 +0x25 fp=0xc000063fe0 sp=0xc000063fc8 pc=0xfce7e5
runtime.goexit()
        D:/Program Files/Go/src/runtime/asm_amd64.s:1594 +0x1 fp=0xc000063fe8 sp=0xc000063fe0 pc=0xf9e481
created by main.main
        D:/go-book/go-start/main.go:24 +0x31
exit status 2
```

GOTRACEBACK=crash
类似系统转存 coredump

```

```

代码

```
debug.SetTraceback("system")
```

设置输出级别
级别越高输出越详细

`GOARCH` `GOOS` `GOPATH` `GOROOT`
架构 x86 arm ===
OS windows maxos bsd liunx?
这些参数只影响构建 不影响运行时的执行
