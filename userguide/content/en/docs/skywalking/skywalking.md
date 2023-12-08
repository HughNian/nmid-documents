---
title: "🌙SkyWalking例子"
linkTitle: "🌙SkyWalking例子"
weight: 3
description:
---

client调用worker1，worker1调用worker2，调用层级2层，后面又多层调用以此类推，本示例展示多层调用，并使用
skywalking进行调用链的跟踪查看

## Client

普通golang代码中运行

```go
const SERVERHOST = "127.0.0.1"
const SERVERPORT = "6808"

func main() {
	var client *cli.Client
	var err error

	serverAddr := SERVERHOST + ":" + SERVERPORT
	client, err = cli.NewClient("tcp", serverAddr).Start()
	if nil == client || err != nil {
		log.Println(err)
		return
	}
	defer client.Close()

	client.ErrHandler = func(e error) {
		if model.RESTIMEOUT == e {
			log.Println("time out here")
		} else {
			log.Println(e)
		}
		fmt.Println("client err here")
	}

	respHandler := func(resp *cli.Response) {
		if resp.DataType == model.PDT_S_RETURN_DATA && resp.RetLen != 0 {
			if resp.RetLen == 0 {
				log.Println("ret empty")
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
		}
	}

	paramsName1 := make(map[string]interface{})
	paramsName1["name"] = "nmid"
	params1, err := msgpack.Marshal(&paramsName1)
	if err != nil {
		log.Fatalln("params msgpack error:", err)
		os.Exit(1)
	}
	err = client.Do("ToUpper", params1, respHandler)
	if nil != err {
		fmt.Println(err)
	}
}
```


## Worker1

worker1使用SkyWalking进行链路追踪

```go
const NMIDSERVERHOST = "127.0.0.1"
const NMIDSERVERPORT = "6808"
const SKYREPORTERURL = "192.168.10.176:11800"

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
	//register to discovery server
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


## Worker2

worker2使用SkyWalking进行链路追踪

```go
const NMIDSERVERHOST = "127.0.0.1"
const NMIDSERVERPORT = "6808"
const SKYREPORTERURL = "192.168.10.176:11800"

func ToUpper2(job wor.Job) (ret []byte, err error) {
	resp := job.GetResponse()
	if nil == resp {
		return []byte(``), fmt.Errorf("response data error")
	}

	var name string
	if len(resp.ParamsMap) > 0 {
		name = resp.ParamsMap["name"].(string)
	}

	errHandler := func(e error) {
		if model.RESTIMEOUT == e {
			log.Println("time out here")
		} else {
			log.Println(e)
		}
	}

	respHandler := func(resp *cli.Response) {
		if resp.DataType == model.PDT_S_RETURN_DATA && resp.RetLen != 0 {
			if resp.RetLen == 0 {
				log.Println("ret empty")
				err = errors.New("ret empty")
				return
			}

			var cretStruct model.RetStruct
			uerr := msgpack.Unmarshal(resp.Ret, &cretStruct)
			if nil != uerr {
				log.Fatalln(uerr)
				err = uerr
				return
			}

			if cretStruct.Code != 0 {
				log.Println(cretStruct.Msg)
				err = errors.New(cretStruct.Msg)
				return
			}
			fmt.Println(string(cretStruct.Data))

			wretStruct := model.GetRetStruct()
			wretStruct.Msg = "ok"
			wretStruct.Data = cretStruct.Data
			ret, err = msgpack.Marshal(wretStruct)

			resp.RetLen = uint32(len(ret))
			resp.Ret = ret
		}
	}

	callAddr := NMIDSERVERHOST + ":" + NMIDSERVERPORT
	funcName := "ToUpper"
	paramsName1 := make(map[string]interface{})
	paramsName1["name"] = name
	job.ClientCall(callAddr, funcName, paramsName1, respHandler, errHandler)

	return
}

func main() {
	wname := "Worker2"

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

	worker.AddFunction("ToUpper2", ToUpper2)
	//register to discovery server
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

## SkyWalking 后台展示 

### 拓扑结构

<img src="/images/topology.png" alt="nmid architecture"/>  

### worker1链路

<img src="/images/worker1.png" alt="nmid architecture"/>  

### worker2链路

<img src="/images/worker2.png" alt="nmid architecture"/>
