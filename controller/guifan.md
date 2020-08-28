### 规范
* 我们约定app/controller 为控制器的根目录
* API返回格式统一 

    1. 当控制器调用下游发生err时使用`utils.Error()` 对错误信息格式化
```go
func ExampleErr(ctx *gin.Context) {
            err := callMethod(ctx, arg1, arg2)
            if err != nil {
                logger.Ex(ctx, "callMethod", "调用callMethod时发生错误，err:%+v, 参数：arg1:%+v, arg2:%+v", err, arg1, arg2)
                resp := utils.Error(logger.NewError("", utils.PARAM_ERROR))
                ctx.JSON(http.StatusOK, resp)
                
                //注意这里的 return关键字。如果if之后还有业务代码，则不能省略，因为ctx.JSON 是不终止执行的
                return
            }
}
```

    2. 调用下游成功时使用`utils.Success()` 对返回值进行格式化输出
```go
func ExampleSuccess(ctx *gin.Context) {
            returnData, _ := callMethod(ctx, arg1, arg2)
            
            resp := utils.Success(returnData)
            ctx.JSON(http.StatusOK, resp)
}
```

* 合理利用http状态码
* `utils.TransferToContext()` 把gin.Context转换成原生context.Context 对象，然后向下游传递
* 控制器的作用仅做流程控制，不应包含具体的业务逻辑代码