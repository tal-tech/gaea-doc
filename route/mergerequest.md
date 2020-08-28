### 路由注册
`app/router/router/router.go` ，所有路由均在本文件内添加

### 要点
 * 分组：具有相同URI前缀的可以放到同一个组，既可以增强阅读性，也可以按组进行统一设置，如中间件
 * 加载中间件：加载顺序按照书写顺序执行


```go
package router

import (
	"gaea/app/controller/democontroller"
	"github.com/gin-gonic/gin"
	"demo/middleware"
)

func RegisterRouter(router *gin.Engine) {
	//此路由对应的URI为：/demo/test
	entry := router.Group("/demo")
	//GET方式请求
	entry.GET("/test", democontroller.MyXesGoDemo)

	//公共中间件
	student := router.Group("/v1/student")
	student.Use(middleware.Common())
	studentNoLogin(student)

	//以下路由增加登录验证中间件
	student.Use(middleware.StuCheckLogin())
	studentNeedLogin(student)

	//注意：后续再次在路由组student中增加URI，则会继承上面的所有中间件，即会全部执行Common、StuCheckLogin 
}

func studentNoLogin(student *gin.RouterGroup) {
	//此路由对应的URI为：/v1/student/noNeedLogin/get
	student.POST("/noNeedLogin/get", func(ctx *gin.Context){
		//...
	})
}

func studentNeedLogin(student *gin.RouterGroup) {
	//对应的URI为：/v1/student/needLogin/get
	student.POST("/needLogin/get", func(ctx *gin.Context){
		//...
	})
}

```