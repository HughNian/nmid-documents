---
title: About nmid
linkTitle: 关于
menu: {main: {weight: 10}}
---

{{% blocks/about title="About nmid" height="auto" %}} 

nmid是golang实现的轻量级的分布式微服务框架，主要有服务调度服务端，服务调用客户端（Consumer），服务提供工作端（Provider）。主要向用户提供跨进程的RPC远程调用能力，同时可以结合K8S使其成为sidecar，为service提供流量代理等功能。nmid本身是以函数为最小单元的服务提供，所以也可以成为一个轻型的faas服务。

通过这里查看文档 [documentation](/docs/) to get started!
{{% /blocks/about %}}

{{% blocks/section color="primary" %}}
## nmid目录结构   

1.server目录为nmid微服务调度服务端go实现，采用协程以及管道的异步通信，带有连接池，自有I/O通信协议，msgpack做通信数据格式。  

2.worker目录为nmid的工作端go实现，目前也有c语言实现，以及php扩展实现，可以实现golang, php, c等作为工作端，从而实现跨语言平台提供功能服务，目前在另外一个项目。[https://github.com/HughNian/nmid-c](https://github.com/HughNian/nmid-c)  

3.client目录为nmid的客户端go实现，目前也有c语言实现，以及php扩展实现，可以实现golang, php, c等作为客户端，从而实现跨语言平台调用功能服务，目前在另外一个项目。[https://github.com/HughNian/nmid-php-ext](https://github.com/HughNian/nmid-php-ext)  

4.cmd目录为demo运行目录。为go实现的客户端示例，调度服务端示例，客户端示例。目前调度服务端只有golang的实现。   


## nmid关联项目
1.nmid调用系统，nmid客户端，nmid工作端（go语言实现）。 [https://github.com/HughNian/nmid](https://github.com/HughNian/nmid)  

2.nmid客户端，nmid工作端（c语言实现）。[https://github.com/HughNian/nmid-c](https://github.com/HughNian/nmid-c)  

3nmid php扩展客户端，nmid php扩展工作端 (c语言实现，php使用)。[https://github.com/HughNian/nmid-php-ext](https://github.com/HughNian/nmid-php-ext)  


{{% /blocks/section %}}
