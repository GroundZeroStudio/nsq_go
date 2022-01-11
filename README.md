# nsq_go

The `nsq_go` package wraps NSQ consumers to help reduce the amount of boilerplate
that each program has to implement to get rolling. It does not attempt to abstract NSQ away entirely.

View the go-nsq [documentation](http://godoc.org/github.com/bitly/go-nsq#Config) for configuration options.

## Example

```go
func TestNsqGoStart(t *testing.T){
	//1.nsqGo 启动
	logger, _ := zap.NewProduction()
	NsqGoStart([]string{"127.0.0.1:4161"},logger,30)

	//2.nsqGo 初始化生产者
	InitNsqProducer(nsq.NewConfig(),3)

	//3.nsqGo 初始化消费者
	NewNsqConsumer("channelTest","topicTest",OnNsqMsg,true,nsq.NewConfig())

	//4.nsqGo 生产者发消息
	NsqPush("topicTest",[]byte("helloWorld"))
}

//消息Nsq消息处理
func OnNsqMsg(msg *nsq.Message) {
	nsqGoLogInfo("ok",zap.Reflect("msg",msg))
}

```

# License

 MIT