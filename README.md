# ali-logger-golang
aliyun k8s logger

## 安转
```shell script
go install github.com/yunkeCN/ali-logger-golang
```

## 使用
```go

import "github.com/yunkeCN/ali-logger-golang/logger"

logger.Init(logger.Options{ProjectName: "middleman-server2", IsDev: true})

logger.Businessf("connect to http://localhost:%s/ for GraphQL playground", port)
logger.Business("connect to http://localhost:%s/ for GraphQL playground")
logger.Accessf("connect to http://localhost:%s/ for GraphQL playground", port)
logger.Access("connect to http://localhost:%s/ for GraphQL playground")
logger.Errorf("connect to http://localhost:%s/ for GraphQL playground", port)
logger.Error("connect to http://localhost:%s/ for GraphQL playground")
```

## 结合gin
```go
gin.DefaultWriter = logger.AccessWriter
gin.DefaultErrorWriter = logger.ErrorWriter

router.Use(gin.LoggerWithFormatter(func(param gin.LogFormatterParams) string {
    m := map[string]interface{}{
        "ClientIP":      param.ClientIP,
        "TimeStamp":     param.TimeStamp.Format(time.RFC1123),
        "Method":        param.Method,
        "Path":          param.Path,
        "Request.Proto": param.Request.Proto,
        "StatusCode":    param.StatusCode,
        "Latency":       param.Latency,
        "Agent":         param.Request.UserAgent(),
    }
    if param.ErrorMessage != "" {
        m["ErrorMessage"] = param.ErrorMessage
    }
    empData, err := json.Marshal(m)
    if err != nil {
        return ""
    }
    return string(empData) + "\n"
}))
```
