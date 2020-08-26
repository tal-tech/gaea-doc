### 服务层
----
#### 约定
为了便于文件目录管理，我们约定app/service为服务层的根目录, 也就是MVC思想的M层

#### 案例
文件所在位置：app/service/demoservice/demoservice.go

```go
package demoservice

import (
	"context"
	"math/rand"

	logger "github.com/tal-tech/loggerX"
)

var SYSTEM_ERR logger.XesError = logger.XesError{110110, "系统异常"}

func DoFun(ctx context.Context, param string) (ret map[string]interface{}, err error) {
	if rand.Intn(100)%2 == 0 { 
		logger.Ex(ctx , "DoFun", "err:%v, param:%+v", SYSTEM_ERR, param)
		return nil, logger.NewError("", SYSTEM_ERR)
	} else {
		ret = make(map[string]interface{}, 2)
		ret["ret1"] = "dofun ok"
		ret["ret1"] = "Welcome to use myproject!"

	}
	return
}

```

当然我们也可以把服务定义为一个结构体，然后为其添加各种`method`

