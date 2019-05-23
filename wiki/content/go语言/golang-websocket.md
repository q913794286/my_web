---
Title: Golang Websocket
---
环境：Win10 + Go1.9.2

1.先下载并引用golang的websocket库

①golang的官方库都在[https://github.com/golang](https://github.com/golang)下，而websocket库在/net下。

②如果没有安装Git，需要先安装Git。

③使用`go get -u github.com/golang/net/websocket`下载代码，将安装在环境变量GOPATH配置的路径中。

代码中使用路径为 "golang.org/x/net/websocket"，在对应路径下没有代码的话则引用出错，可将对应代码放在`GOPAHT/golang.org/x/net下`。

 

2.服务端Go代码

复制代码
```
package main

import (
    "fmt"
    "golang.org/x/net/websocket"
    "net/http"
    "os"
    "time"
)

//错误处理函数
func checkErr(err error, extra string) bool {
    if err != nil {
        formatStr := " Err : %s\n";
        if extra != "" {
            formatStr = extra + formatStr;
        }

        fmt.Fprintf(os.Stderr, formatStr, err.Error());
        return true;
    }

    return false;
}

func svrConnHandler(conn *websocket.Conn) {
    request := make([]byte, 128);
    defer conn.Close();
    for {
        readLen, err := conn.Read(request)
        if checkErr(err, "Read") {
            break;
        }

        //socket被关闭了
        if readLen == 0 {
            fmt.Println("Client connection close!");
            break;
        } else {
            //输出接收到的信息
            fmt.Println(string(request[:readLen]))

            time.Sleep(time.Second);
            //发送
            conn.Write([]byte("World !"));
        }

        request = make([]byte, 128);
    }
}

func main() {
    http.Handle("/echo", websocket.Handler(svrConnHandler));
    err := http.ListenAndServe(":6666", nil);
    checkErr(err, "ListenAndServe");
    fmt.Println("Func finish.");
}
```
复制代码
 PS:《Golang socket》中使用了go coroutine来处理connection的消息阻塞接收，websocket不需要进行这样的处理，否则将报use of closed network connection的错误！

 

 3.

①客户端Go代码

复制代码
```
package main

import (
    "fmt"
    "golang.org/x/net/websocket"
    "os"
    "sync"
)

var gLocker sync.Mutex;    //全局锁
var gCondition *sync.Cond; //全局条件变量

var origin = "http://127.0.0.1:6666/"
var url = "ws://127.0.0.1:6666/echo"

//错误处理函数
func checkErr(err error, extra string) bool {
    if err != nil {
        formatStr := " Err : %s\n";
        if extra != "" {
            formatStr = extra + formatStr;
        }

        fmt.Fprintf(os.Stderr, formatStr, err.Error());
        return true;
    }

    return false;
}
```
//连接处理函数
```
func clientConnHandler(conn *websocket.Conn) {
    gLocker.Lock();
    defer gLocker.Unlock();
    defer conn.Close();
    request := make([]byte, 128);
    for {
        readLen, err := conn.Read(request)
        if checkErr(err, "Read") {
            gCondition.Signal();
            break;
        }

        //socket被关闭了
        if readLen == 0 {
            fmt.Println("Server connection close!");

            //条件变量同步通知
            gCondition.Signal();
            break;
        } else {
            //输出接收到的信息
            fmt.Println(string(request[:readLen]))

            //发送
            conn.Write([]byte("Hello !"));
        }

        request = make([]byte, 128);
    }
}

func main() {
    conn, err := websocket.Dial(url, "", origin);
    if checkErr(err, "Dial") {
        return;
    }

    gLocker.Lock();
    gCondition = sync.NewCond(&gLocker);
    _, err = conn.Write([]byte("Hello !"));
    go clientConnHandler(conn);

    //主线程阻塞，等待Singal结束
    for {
        //条件变量同步等待
        gCondition.Wait();
        break;
    }
    gLocker.Unlock();
    fmt.Println("Client finish.")
}
```
复制代码
 

②如果客户端不使用Go代码，可以使用Cocos Creator的js代码

复制代码
```
cc.Class({
    extends: cc.Component,

    properties: {

    },

    ctor: function () {
        this.ws = null;
    },

    onLoad: function () {
        var self = this;

        this.ws = new WebSocket("ws://127.0.0.1:6666/echo");
        this.ws.onopen = function (event) {

            console.log("Send Text WS was opened.");

            if (self.ws.readyState === WebSocket.OPEN) {
                self.ws.send("Hello !");
            }
            else{
                console.log("WebSocket instance wasn't ready...");
            }
        };

        this.ws.onmessage = function (event) {
            console.log("response text msg: " + event.data);
            self.ws.send("Hello !");
        };

        this.ws.onerror = function (event) {
            console.log("Send Text fired an error");
        };

        this.ws.onclose = function (event) {
            console.log("WebSocket instance closed.");
        };

    },

    // called every frame
    update: function (dt) {

    },
});
```
复制代码
 

以上。