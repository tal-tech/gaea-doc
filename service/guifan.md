### 规范
* 业务函数必须通过 context.Context 来传递上下文信息
* 通过`logger.XesError{}` 自定义全局带错误码的err对象

```go
package common

import "github.com/tal-tech/loggerX"

var (
	ConfErr           = logger.XesError{61301, "cannot found mysql[et] configuration in conf file"}
	GetPackageInfoERR = logger.XesError{61302, "GetPackageInfo rpc err"}
	TokenERR          = logger.XesError{61303, "签名校验错误"}
)

```
* 错误生成避免使用`errors.New("")` , 请使用 `logger.NewError()`替代
   
    使用
    ```js
    logger.NewError("", TokenERR)
    ```

    * 这样做的好处如下：
    
    1. 自动记录报错的调用栈信息，如错误所在的文件、行号等等。
    2. 统一错误输出形式便于与其他系统调试    


* 出现错误向上返回之前，强烈建议打一条日志：`logger.E()` 或`logger.Ex()` ,后续错误日志告警会处理的更加友好，能清晰地展示调用链！
