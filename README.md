**참고**

- [[Blog] [Go/Golang] Go Application에 Prometheus Exporter 연동하기](https://wookiist.dev/108#recentComments)

---

golang 의 gin 웹 프레임워크에서 promhttp 를 사용해서 prometheus exporter 를 연동해보자

모듈 초기화 및 패키지 다운로드

```bash
go mod init github.com/bellship24/bellship-promhttp
go get github.com/gin-gonic/gin
go get github.com/prometheus/client_golang/prometheus/promhttp
```

`main.go` 작성

```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
	"github.com/prometheus/client_golang/prometheus/promhttp"
)

func main() {
	router := gin.Default()
	router.GET("/", func(c *gin.Context) {
		c.String(http.StatusOK, "Hello, World!")
	})
	router.GET("/metrics", gin.WrapH(promhttp.Handler()))
	router.Run(":3000")
}
```

- gin.WrapH() 는 `func WrapH(h http.Handler) HandlerFunc` 으로 http.Handler 를 wrapping 하고 Gin middleware 를 리턴하는 helper 함수.

웹 서버 실행

```bash
go run main.go
```

확인

![](/.uploads/2022-01-11-11-37-45.png)

![](/.uploads/2022-01-11-11-38-15.png)