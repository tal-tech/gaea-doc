### 绑定示例
------

#### Gin的参数绑定解析函一共两种类型：动态和固定类型
```go
//动态解析：根据http header头的Content-type 来自动选择解析类型
func (c *Context) ShouldBind(obj interface{}) error {
    //todo
}

//固定解析类型：只能解析json传参形式
func (c *Context) ShouldBindJSON(obj interface{}) error {
    //todo
}

...
```

#### 示例
```go

package main

import (
	"github.com/gin-gonic/gin"
)

type LoginForm struct {
	User     string `form:"user" binding:"required"`
	Password string `form:"password" binding:"required"`
}

func main() {
	router := gin.Default()
	router.POST("/login", func(c *gin.Context) {
		// you can bind multipart form with explicit binding declaration:
		// c.ShouldBindWith(&form, binding.Form)
		// or you can simply use autobinding with ShouldBind method:
		var form LoginForm
		// in this case proper binding will be automatically selected
		if c.ShouldBind(&form) == nil {
			if form.User == "user" && form.Password == "password" {
				c.JSON(200, gin.H{"status": "you are logged in"})
			} else {
				c.JSON(401, gin.H{"status": "unauthorized"})
			}
		}
	})
	router.Run(":8080")
}
```
#### 请求
1. urlencod 默认参数形式
```bash
$ curl -d 'user=uuu&password=pppp' http://yourhost.com/login
```
2. json字符串形式
```bash
$ curl  -H 'Context-type:application/json' -d '{"user":"uuu", "password":"pppp"}' http://yourhost.com/login
```
3. 当然还有其他很多形式，一下是gin所支持的绑定参数形式
```go
const (
	MIMEJSON              = "application/json"
	MIMEHTML              = "text/html"
	MIMEXML               = "application/xml"
	MIMEXML2              = "text/xml"
	MIMEPlain             = "text/plain"
	MIMEPOSTForm          = "application/x-www-form-urlencoded"
	MIMEMultipartPOSTForm = "multipart/form-data"
	MIMEPROTOBUF          = "application/x-protobuf"
	MIMEMSGPACK           = "application/x-msgpack"
	MIMEMSGPACK2          = "application/msgpack"
	MIMEYAML              = "application/x-yaml"
)
```

### 结构体tag的用法
1. 如果未定义tag, 请求参数字段必须和结构体定义的字段名称一致，否则解析失败
2. 如果tag 定义，则必须是`form:"字段名称"` ,这样我们就可以，以定义的字段名称来进行传参了