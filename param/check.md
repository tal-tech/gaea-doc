### 参数校验
------

#### 通过参数结构体的tag 来定义校验规则
```go
//获取在线状态参数
type GetOnlinestatusArgs struct {
	BizId   int `form:"bizId" json:"bizId" binding:"required"`
	PlanId  int `form:"planId" json:"planId" binding:"required"`
	ClassId int `form:"classId" json:"classId" binding:"required"`
}
```
上面的结构体中，我们定义了接口需要传递三个参数，且都是必传项
此处校验tag 固定为`binding`

#### 支持的校验规则



| 校验标识        | 说明   | 使用 |
| --------   | -----:  | ----- |
| required     | 必选参数 |  |
| gte     | 大于等于 | gte=3 |
| lte     | 小于等于 |  |
| email     | 邮箱 | 使用 |
| hexcolor     | 十六进制颜色 |  |
| hexcolor     | 十六进制颜色 |  |
| len     | 长度 | len=10 |
| eq     | 等于 |  |
| ne     | 不等于 |  |
| url     | url格式校验 |  |
| ...| ... |  |

[更多](https://godoc.org/gopkg.in/go-playground/validator.v8)

#### 使用方法
**多种校验以逗号分隔**
```go
// User contains user information
type User struct {
	LastName       string     `validate:"required"`
	Age            uint8      `validate:"gte=0,lte=130"`
	Email          string     `validate:"required,email"`
	FavouriteColor string     `validate:"hexcolor|rgb|rgba"`
	Addresses      []*Address `validate:"required,dive,required"` // a person can have a home and cottage...
}
```