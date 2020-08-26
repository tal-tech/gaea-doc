### 快速开始
------

### Download

```golang
//Recommended  $GOPATH/src  as your workspace
$ cd $GOPATH/src/

//clone the framework to local
$ git clone git@github.com:tal-tech/gaea.git
```

### Build

```golang
//Will use makefile to compile and generate binary files to the bin directory
$ cd gaea
$ make
```

#### help
```go
$ ./bin/myproject --help
Usage of ./bin/myproject:
  -c string
    	指定ini配置文件，以程序的二进制文件(myproject)为相对目录，正确的相对目录加载方式： -c ../conf/conf_xxx.ini； 默认为加载 ../conf/conf.ini
  -p string
        配置文件的前缀设置，用于以绝对路径形式加载，如  -c conf.ini -p /usr/pathto/myproject/conf
  -cfg string
    	json config path (default "conf/config.json")		
  -extended string
    	扩展参数，程序未定义使用用途，用户可自行处理
  -f	foreground
  -m	mock 开关	
  -s string
    	start or stop
  -usr1 string
    	user defined flag -usr1
  -usr2 string
    	user defined flag -usr2
  -usr3 string
    	user defined flag -usr3
  -usr4 string
    	user defined flag -usr4
  -usr5 string
    	user defined flag -usr5
  -v	version
```

## Example
1.Config server port
```golang
//conf/conf.ini
[HTTP]
port = 9898
;...
```

2.Add router
```golang
//app/router/router.go is the file that manage all URI
func RegisterRouter(router *gin.Engine) {
	entry := router.Group("/demo", middleware.PerfMiddleware(), middleware.XesLoggerMiddleware())
	entry.GET("/test", democontroller.GaeaDemo)
}
```

4.Controller (mvc programming style)
```golang
//app/router/
func GaeaDemo(ctx *gin.Context) {
	goCtx := xesgin.TransferToContext(ctx)
	param := ctx.PostForm("param")
	ret, err := demoservice.DoFun(goCtx, param)
	if err != nil {
		resp := xesgin.Error(err)
		ctx.JSON(http.StatusOK, resp)
	} else {
		resp := xesgin.Success(ret)
		ctx.JSON(http.StatusOK, resp)
	}
}
```

#### Run
```go
$ ./bin/gaea
2020/08/26 14:40:02 CONF INIT,path:../conf/conf.ini
[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:	export GIN_MODE=release
 - using code:	gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /demo/test                --> gaea/app/controller/democontroller.GaeaDemo (1 handlers)
2020/08/26 14:40:02 [overseer master] run
2020/08/26 14:40:02 [overseer master] starting /mnt/d/codes/go/src/gaea/bin/gaea
2020/08/26 14:40:02 CONF INIT,path:../conf/conf.ini
[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:	export GIN_MODE=release
 - using code:	gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /demo/test                --> gaea/app/controller/democontroller.GaeaDemo (1 handlers)
2020/08/26 14:40:02 [overseer slave#1] run
2020/08/26 14:40:02 [overseer slave#1] start program
2020/08/26 14:40:09 [overseer master] proxy signal (window changed)

```

#### Try it!
```go
$ curl  http://127.0.0.1:9898/demo/test
{"code":0,"data":{"ret1":"Welcome to use myproject!"},"msg":"ok","stat":1}
```

----------
至此，我们已经通过Gaea搭建了一个web服务！
接下来我们进一步学习Gaea 相关配置、特性、组件应用等主题