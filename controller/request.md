### 编写控制器

进入到文件app/controller/democontroller/DemoController.go

------
#### 案例

控制器方法是一个统一的类型： `type HandlerFunc func(*Context)`，只要符合`HandlerFunc` 定义格式即可

```go
package democontroller

import (
	"net/http"
	"gaea/app/service/demoservice"
	"github.com/tal-tech/loggerX"
	"gaea/utils"
	"github.com/gin-gonic/gin"
)

//Gin handler
func MyXesGoDemo(ctx *gin.Context) {
	//把gin.Context 对象转换成原生 context.Context 对象
	goCtx := utils.TransferToContext(ctx)
	//接受参数
	param := ctx.PostForm("param")
	ret, err := demoservice.DoFun(goCtx, param)
	if err != nil {
		logger.Ex(goCtx, "MyXesGoDemo", "demoservice.DoFun err:%v, param:%+v", err, param)
		//转换成统一的输出格式
		resp := utils.Error(err)
		ctx.JSON(http.StatusOK, resp)
	} else {
		//转换成统一的输出格式
		resp := utils.Success(ret)
		ctx.JSON(http.StatusOK, resp)
	}
}

```

#### 注意
* 控制器内严禁在不加锁的情况下，对全局变量进行写操作，否则可能由于并发导致的数据错乱问题