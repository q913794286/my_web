---
Title: go语言如何调用c函数
Sort: 2
---
##go语言如何调用c函数

1.直接嵌入c源代码到go代码里面
```
package main

/*
#include <stdio.h>

void myhello(int i) {
  printf("Hello C: %d\n", i);
}
*/
import "C"

import "fmt"

func main() {
  C.myhello(C.int(12))
  fmt.Println("Hello Go");
}
```
2.需要注意的是C代码必须放在注释里面
3.import "C"语句和前面的C代码之间不能有空行
运行结果
```
$ go build main.go && ./main
Hello C: 12
Hello Go
```
分开c代码到单独文件
嵌在一起代码结构不是很好看，很多人包括我，还是喜欢把两个分开，放在不同的文件里面，显得干净，go源文件里面是go的源代码，c源文件里面是c的源代码。
```
$ ls 
hello.c  hello.h  main.go
$ cat hello.h 
void hello(int);
$ cat hello.c
#include <stdio.h>

void hello(int i) {
  printf("Hello C: %d\n", i);
}
$ cat main.go 
package main

// #include "hello.h"
import "C"

import "fmt"

func main() {
  C.hello(C.int(12))
  fmt.Println("Hello Go");
}
```
编译运行
```
$ go build && ./main
Hello C: 12
Hello Go
```
编译成库文件
如果c文件比较多，最好还是能够编译成一个独立的库文件，然后go来调用库。
```
$ find mylib main  
mylib
mylib/hello.h
mylib/hello.c
main
main/main.go
```
编译库文件
```
$ cd mylib
# gcc -fPIC -shared -o libhello.so hello.c
```
编译go程序
```
$ cd main
$ cat main.go 
package main

// #cgo CFLAGS: -I../mylib
// #cgo LDFLAGS: -L../mylib -lhello
// #include "hello.h"
import "C"

import "fmt"

func main() {
  C.hello(C.int(12))
  fmt.Println("Hello Go");
}
$ go build main.go
```
运行
```
$ export LD_LIBRARY_PATH=../mylib
$ ./main 
Hello C: 12
Hello Go
```
在我们的例子中，库文件是编译成动态库的，main程序链接的时候也是采用的动态库
```
$ ldd main
        linux-vdso.so.1 =>  (0x00007fffc7968000)
        libhello.so => ../mylib/libhello.so (0x00007f513684c000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00007f5136614000)
        libc.so.6 => /lib64/libc.so.6 (0x00007f5136253000)
        /lib64/ld-linux-x86-64.so.2 (0x000055d819227000)
```
理论上讲也是可以编译成整个一静态链接的可执行程序，由于我的机器上缺少静态链接的系统库，比如libc.a，所以只能编译成动态链接。

最后
这篇文章介绍的是一般的go语言如何调用c的函数的过程，其实go语言和c语言毕竟是两种不同的语言，很多的内容，结构，数据类型并不是完全兼容的，具体的细节需要查询go语言cgo规范的说明。