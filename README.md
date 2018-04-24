# Public & Subscribe [![GoDoc](https://godoc.org/github.com/go-mego/pubsub?status.svg)](https://godoc.org/github.com/go-mego/pubsub)

Public & Subscribe 是實作事件來源模式的「發布」和「訂閱」套件。你能夠在 Mego 中以此套件來透過第三方事件中心在不同的服務中進行事件廣播和訂閱接收，且無需將資料存儲至資料庫以節省開銷。

# 索引

* [安裝方式](#安裝方式)
* [使用方式](#使用方式)
    * [NSQ](#NSQ)
    * [Event Store](#EventStore)

# 安裝方式

打開終端機並且透過 `go get` 安裝此套件即可。

```bash
$ go get github.com/go-mego/pubsub
```

# 使用方式

此套件目前支援下列事件存儲中心、訊息佇列軟體。

* NSQ
* Event Store

## NSQ

NSQ 是一套訊息佇列系統，透過 `nsq.NewClient` 來初始化相對應的客戶端。

```go
package main

import (
	api "github.com/nsqio/go-nsq"
	"github.com/go-mego/pubsub/nsq"
)

func main() {
	// 建立一個 NSQ 客戶端，並指定發布者與叢集的位置。
	c, _ := nsq.NewClient(&nsq.Config{
		Producer: nsq.Producer{
			TCP:  "127.0.0.1:4150",
			HTTP: "127.0.0.1:4151",
		},
		Lookupds: []string{"127.0.0.1:4161"},
	}, api.NewConfig())
}
```

## Event Store

事件存儲中心 Event Store 能夠重播、發布和訂閱事件，透過 `eventstore.NewClient` 來初始化一個客戶端。

```go
package main

import (
	"github.com/go-mego/pubsub/eventstore"
)

func main() {
	// 建立一個基於 Event Store 的客戶端連線。
	c, _ := eventstore.NewClient(&eventstore.Config{
		URL:      "http://127.0.0.1:2113",
		Username: "admin",
		Password: "changeit",
	})
}
```