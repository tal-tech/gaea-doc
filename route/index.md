### 约定： 后续在没有特殊说明的情况下，均以gaea为根目录
------

Gin 的路由支持 `GET`, `POST`, `PUT`, `DELETE`, `PATCH`, `HEAD`, `OPTIONS` 请求, 注：这些方法是强匹配，即定义为GET方法，则仅支持GET请求

其他方法访问会提示：`404 Nod Found` 

若想同时支持GET、POST，有两种方法：
1. 分别注册
2. 用 `Any`注册, 可以同时支持以上的所有请求。
