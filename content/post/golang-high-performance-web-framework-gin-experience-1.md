+++
title = "Golang 高性能 Web 框架 Gin 体验（一）"
date = "2016-05-03T04:04:22+08:00"
comments = true
tags = [
    "go",
    "golang",
    "web framework",
]

+++

[pic-0]: http://i3.buimg.com/8e2b0c7792fc9ded.png "gin-first-test-hello-json"
[pic-1]: http://i3.buimg.com/1a6cc4ab5291af6d.png "gin-first-test-hello-json-mingw64"

> Gin 是一个高性能 HTTP web 框架，使用 Go（Golang） 语言开发，项目介绍说：它有着和 Martini 相似的 API，而超高的性能得益于使用了 [httprouter](https://github.com/julienschmidt/httprouter)，如果你需要开发高性能应用，可以试试 Gin！

> gin 地址： https://gin-gonic.github.io/gin/

> httprouter 地址： https://github.com/julienschmidt/httprouter

# 第一次使用 Gin 框架

---

### 1. 第一次使用需要先获取相应包

**注：这里需要先安装好 Go 语言**

`$ go get github.com/gin-gonic/gin`

---

### 2. 导入相关包即可

需要了解如何使用，可以编写一个简单的测试程序，流程如下

* 新建一个文件 test.go
* 键入测试程序代码
* 编译测试文件
* 运行编译成功的可执行文件
* 使用浏览器查看结果

**测试代码如下：**

```
package main

// 引入 Gin 的框架包
import "github.com/gin-gonic/gin"

func main() {
    // 先获取默认路由
    r := gin.Default()
    // 这里使用 GET 方式进行响应，可以直接返回 JSON 格式的文本
    r.GET("/hello", func(c *gin.Context) {
        c.JSON(200, gin.H{
            "message": "Hello ,Gin Framework.",
        })
    })
    // 这里运行后启动WEB服务；监听的IP和端口为： 0.0.0.0:8080
    r.Run()
}
```

**编译（这里测试环境使用的 Windows 10，Go 1.6）**  
`$ go build -o test.exe`  

**运行后出现如下界面**  

```
$ ./test.exe
[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:   export GIN_MODE=release
 - using code:  gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /hello                     --> main.main.func1 (3 handlers)
[GIN-debug] Environment variable PORT is undefined. Using port :8080 by default
[GIN-debug] Listening and serving HTTP on :8080
```
这里运行后反馈的消息还是比较明了的；开始就提示了是 debug 模式
下面有路由，访问方式，对应函数，访问端口等信息。

**接下来使用浏览器输入地址 127.0.0.1:8080 查看结果**

![gin-first-test-hello-json][pic-0]

![gin-first-test-hello-json-mingw64][pic-1]

**至此，已经完成了第一个测试程序。**
**高性能这点代码肯定体现不出来，只有在接下来继续深入了解后才能得知**

# Gin 的访问方式

在上面的测试代码中，只使用了 GET 访问方式；

而 Gin 支持的可选访问方式从官方例子中得知有以下几种。

* GET
* POST
* PUT
* PATCH
* DELETE
* OPTIONS

官方例子如下：
```
package main
import "github.com/gin-gonic/gin"
import "net/http"   // 使用HTTP相关状态码用到

// 官方 API 示例
func main() {
    // Creates a gin router with default middleware:
    // logger and recovery (crash-free) middleware
    router := gin.Default()

    // 可选的访问方式
    router.GET("/someGet", getting)
    router.POST("/somePost", posting)
    router.PUT("/somePut", putting)
    router.DELETE("/someDelete", deleting)
    router.PATCH("/somePatch", patching)
    router.HEAD("/someHead", head)
    router.OPTIONS("/someOptions", options)

    // By default it serves on :8080 unless a
    // PORT environment variable was defined.
    router.Run()
    // 参数可以设置监听端口，默认 8080
    // router.Run(":3000") for a hard coded port
}
```
