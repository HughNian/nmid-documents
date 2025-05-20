---
title: "📡Discovery注册中心"
linkTitle: "📡Discovery注册中心"
weight: 4
description:
---

目前nmid只使用etcd为注册中心，client和worker的使用处需要使用相应discovery的方法

## Client

client使用discovery，下面是一个http api服务列子

```go
const SERVERHOST = "127.0.0.1"
const SERVERPORT = "6808"

var client *cli.Client
var err error
var consumer *cli.Consumer

func getClient() *cli.Client {
	serverAddr := NMIDSERVERHOST + ":" + NMIDSERVERPORT
	client, err := cli.NewClient("tcp", serverAddr).SetIoTimeOut(30 * time.Second).Start()
	if nil == client || err != nil {
		logger.Error(err)
	}

	return client
}

func discovery(funcName string) *cli.Client {
	client := consumer.Discovery(funcName)
	if client != nil {
		client, err := client.SetIoTimeOut(30 * time.Second).Start()
		if nil == client || err != nil {
			logger.Error(err)
		}
	} else {
		client = getClient()
	}

	return client
}

func Test(ctx *fasthttp.RequestCtx) {
	funcName := "ToUpper"

	client := discovery(funcName)
	defer client.Close()

	if nil == client {
		fmt.Fprint(ctx, "nmid client error")
		return
	}

	client.SetParamsType(model.PARAMS_TYPE_JSON)

	client.ErrHandler = func(e error) {
		if model.RESTIMEOUT == e {
			logger.Warn("time out here")
		} else {
			logger.Error(e)
		}

		fmt.Fprint(ctx, e.Error())
	}

	respHandler := func(resp *cli.Response) {
		if resp.DataType == model.PDT_S_RETURN_DATA && resp.RetLen != 0 {
			if resp.RetLen == 0 {
				logger.Info("ret empty")
				return
			}

			var retStruct model.RetStruct
			err := msgpack.Unmarshal(resp.Ret, &retStruct)
			if nil != err {
				log.Fatalln(err)
				return
			}

			if retStruct.Code != 0 {
				log.Println(retStruct.Msg)
				return
			}

			fmt.Println(string(retStruct.Data))

			fmt.Fprint(ctx, string(retStruct.Data))
		}
	}

	paramsName := make(map[string]interface{})
	paramsName["name"] = "niansong1"
	//params, err := msgpack.Marshal(&paramsName)
	params1, err := json.Marshal(&paramsName)
	if err != nil {
		logger.Fatal("params msgpack error:", err)
	}
	err = client.Do(funcName, params1, respHandler)
	if nil != err {
		logger.Error(`do err`, err)
	}
}

func main() {
	consumer = &cli.Consumer{
		EtcdAddrs: discoverys,
		Username:  disUsername,
		Password:  disPassword,
	}
	consumer.EtcdCli = consumer.EtcdClient()
	consumer.EtcdWatch()

	router := fasthttprouter.New()
	router.GET("/test", Test)
	err := fasthttp.ListenAndServe(":5981", router.Handler)
	fmt.Println(`err info:`, err)
}
```


## Worker1

worker1注册到discovery

```go
const NMIDSERVERHOST = "127.0.0.1"
const NMIDSERVERPORT = "6808"

var discoverys = []string{"localhost:2379"}
var disUsername = "root"
var disPassword = "123456"

func ToUpper(job wor.Job) ([]byte, error) {
	resp := job.GetResponse()
	if nil == resp {
		return []byte(``), fmt.Errorf("response data error")
	}

	if len(resp.ParamsMap) > 0 {
		name := resp.ParamsMap["name"].(string)

		retStruct := model.GetRetStruct()
		retStruct.Msg = "ok"
		retStruct.Data = []byte(strings.ToUpper(name))
		ret, err := msgpack.Marshal(retStruct)
		if nil != err {
			return []byte(``), err
		}

		resp.RetLen = uint32(len(ret))
		resp.Ret = ret

		return ret, nil
	}

	return nil, fmt.Errorf("response data error")
}

func main() {
	wname := "Worker1"

	var worker *wor.Worker
	var err error

	serverAddr := NMIDSERVERHOST + ":" + NMIDSERVERPORT
	worker = wor.NewWorker().SetWorkerName(wname).WithTrace(SKYREPORTERURL)
	err = worker.AddServer("tcp", serverAddr)
	if err != nil {
		log.Fatalln(err)
		worker.WorkerClose()
		return
	}

	worker.AddFunction("ToUpper", ToUpper)
	//worker注册到注册中心
	worker.Register(wor.EtcdConfig{Addrs: discoverys, Username: disUsername, Password: disPassword})

	if err = worker.WorkerReady(); err != nil {
		log.Fatalln(err)
		worker.WorkerClose()
		return
	}

	go worker.WorkerDo()

	quits := make(chan os.Signal, 1)
	signal.Notify(quits, syscall.SIGHUP, syscall.SIGQUIT, syscall.SIGTERM, syscall.SIGINT /*syscall.SIGUSR1*/)
	switch <-quits {
	case syscall.SIGQUIT, syscall.SIGTERM, syscall.SIGINT:
		worker.WorkerClose()
	}
}

```