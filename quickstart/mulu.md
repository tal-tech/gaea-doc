### 目录结构

```
gaea/
├── app                             #项目工程目录
│   ├── router                      #router目录
│   │   └── router.go               #router文件
│   ├── service                     #service目录，业务逻辑
│   │   └── demoservice             #demoservice目录
│   │       └── demoservice.go      #demoservice文件，业务实现逻辑
│   ├── controller                  #contronller目录，控制层
│   │   └── democontroller          #democontroller目录
│   │       └── DemoController.go   #democontroller实现
├── bin                             #可执行文件目录
│   └── gaea                       #可执行文件
├── conf                            #配置文件目录
│   ├── conf.ini                    #配置文件
│   ├── conf_dev.ini                #开发配置文件
│   ├── conf_release.ini            #测试线配置文件
│   └── gaea.ini                   #supervisor 配置文件    
├── cmd                            #main目录
│   └── main.go                     #进程启动,http服务配置代码
├── version                         #版本目录
│   └── version.go                  #版本文件
├── utils                           #工具目录
├── Makefile                        #编译文件 make
├── deploy.sh                       #发布系统编译成功，推送到目标机器会触发此脚本执行

```
