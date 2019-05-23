---
Title: Golang与C互用以及调用C的so动态库和a静态库
Sort: 1
---
Golang与C的关系非常密切，下面主要介绍在Golang中使用C。

### 一. Golang中嵌入C代码
```
 package main                                                                                                                    
   
 //#include <stdio.h>
 //#include <stdlib.h>
 /*
 void Hello(char *str) {
    printf("%s\n", str);
 }
 */
 import "C" //假设把C当成包，其实有点类似C++的名字空间
 import "unsafe" //C指针的使用，在C代码中申请的空间，GC垃圾回收机制不会管理，所以需要自己手动free申请的空间
  
 func main() {
     s := "Hello Cgo"
     cs := C.CString(s)
     C.Hello(cs)
     C.free(unsafe.Pointer(cs))
 }

```
3,4行的注释也可以写/* */形式 
第4行与第5行之间不能有空行，同样第9行与第10行之间也不 
能有行，否则编译时cgo会报错：

```
jack@jack-VirtualBox:~/mygo/src/myC/tmp$ go run goc.go
C库源程序代码：
# command-line-arguments
could not determine kind of name for C.Hello
could not determine kind of name for C.free
```
### Golang中调用C的动态库so
```
1 //foo.c               
2 #include <stdio.h>
3  
4 int Num = 8;
5  
6 void foo(){
7     printf("I dont have new line.");
8     printf("I have new line\n");
9     printf("I have return\r");
10     printf("I have return and new line\r\n");
11 }
```
```
1 //foo.h               
2 #ifndef __FOO_H
3 #define __FOO_H
4  
5 #ifdef __cplusplus
6 extern "C" {
7 #endif
8  
9 extern int Num;
10 extern void foo();
11  
12 #ifdef __cplusplus
13 }
14 #endif
15  
16 #endif
```
 
 Go调用C库的代码：
```
1 package main       
2  
3 /*
4 #cgo CFLAGS: -I.
5 #cgo LDFLAGS: -L. -lfoo
6 #include <stdio.h>
7 #include <stdlib.h>
8 #include "foo.h"
9 */
10 import "C"
11 import "unsafe"
12 import "fmt"
13  
14 func Prin(s string) {
15     cs := C.CString(s)
16     defer C.free(unsafe.Pointer(cs))
17     C.fputs(cs, (*C.FILE)(C.stdout))
18     //C.free(unsafe.Pointer(cs))
19     C.fflush((*C.FILE)(C.stdout))
20 }
21  
22 func main() {
23     fmt.Println("vim-go")
24     fmt.Printf("rannum:%x\n", C.random())
25     Prin("Hello CC")
26     fmt.Println(C.Num)
27     C.foo()
28 }
```
 注意import “C”这行与第9行之间也不能有空格，原因同上编译会报错。 
go与库、头文件的目录结构，以及编译：
```
jack@jack-VirtualBox:~/mygo/src/myC/sotest$go build cc.go //编译之后产生cc
jack@jack-VirtualBox:~/mygo/src/myC/sotest$ ls
cc  cc.go  foo.h  libfoo.so
jack@jack-VirtualBox:~/mygo/src/myC/sotest$ export LD_LIBRARY_PATH=./
jack@jack-VirtualBox:~/mygo/src/myC/sotest$ ./cc
vim-go
rannum:6b8b4567
Hello CC8
I dont have new line.I have new line
I have return and new line
```

如果c库的程序改为下面这样：
```
  1 //foo.c            
  2 #include <stdio.h>
  3  
  4 int Num = 8;
  5  
  6 void foo(){
  7     printf("I dont have new line.");
  8     //printf("I have new line\n");
  9     //printf("I have return\r");
 10     //printf("I have return and new line\r\n");
 11 }
```
此时，go中调用foo时没有任何反应，即不会输出，不会输出的原因是printf后面没有加换行符，但是如果加了8，9，10这些测试行后，第7行也会显示，原因是第10行最后有一个换行符，这个应该是向stdout输出时，字符流是放在buffer中，如果没有换行，buffer中的数据不会立即输出。在go调用C的测试程序中有写了一个测试小函数用来测试stdout，验证了没有fflush，stdout上不会显示输出信息。但平时在写C程序的时候，似乎没有这样的问题，这个是因为C的运行环境自动做了fflush的动作，这个可能是Golang的不足，也许golan表g认为这样做更好。这只是个人的分析，如有分析不对的地方，请跟贴，谢谢。

### Golang调用C的静态库a

所有源程序与动库演示代码类似，只是编译C库时，是用的静态编译，如下所示：

```
jack@jack-VirtualBox:~/mygo/src/myC$ ls
foo.c 
jack@jack-VirtualBox:~/mygo/src/myC$ gcc -c foo.c
foo.c foo.o
jack@jack-VirtualBox:~/mygo/src/myC$ ar -rv libfoo.a foo.o
foo.c foo.o libfoo.a
```
Go调用的代码与动态库一样，下面是目录结构：
```
jack@jack-VirtualBox:~/mygo/src/myC/atest$ ls
cc  cc.go  foo.h  libfoo.a
```
