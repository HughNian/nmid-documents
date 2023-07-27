---
title: About nmid
linkTitle: 关于
menu: {main: {weight: 10}}
---

{{% blocks/cover title="About nmid" height="auto" %}}

分布式微服务系统一直以来都是服务端技术比较重要技术领域，微服务都是分布式的系统，微服务的架构离不开分布式，微服务架构是分布式服务架构的子集， 微服务架构通过更细粒度的服务切分，使得整个系统的迭代速度并行程度更高。 分布式系统的实现有复杂的诸如Map-Reduce,HBase,Dynamo等，也有简单的只是一个rpc服务。当然我们也可以根据这些复杂系统中的核心思想构建出属于我们自己的分布式微服务系统，这样的系统可能更符合你的业务需求，也可以更好的自我维护和自我扩展。  

nmid是用go语言实现的轻量级的分布式微服务框架，主要有服务调度服务端，服务调用客户端，服务提供工作端。这是rpc框架的主题功能结构，但同时

通过这里查看文档 [documentation](/docs/) to get started!
{{% /blocks/cover %}}

{{% blocks/section color="primary" %}}
## nmid介绍   

nmid意思为中场指挥官，足球场上的中场就是统领进攻防守的核心。咱们这里是服务程序的调度核心。是微服务调度系统。

1.server目录为nmid微服务调度服务端go实现，采用协程以及管道的异步通信，带有连接池，自有I/O通信协议，msgpack做通信数据格式。  

2.worker目录为nmid的工作端go实现，目前也有c语言实现，以及php扩展实现，可以实现golang, php, c等作为工作端，从而实现跨语言平台提供功能服务，目前在另外一个项目。[https://github.com/HughNian/nmid-c](https://github.com/HughNian/nmid-c)  

3.client目录为nmid的客户端go实现，目前也有c语言实现，以及php扩展实现，可以实现golang, php, c等作为客户端，从而实现跨语言平台调用功能服务，目前在另外一个项目。[https://github.com/HughNian/nmid-php-ext](https://github.com/HughNian/nmid-php-ext)  

4.cmd目录为demo运行目录。为go实现的客户端示例，调度服务端示例，客户端示例。目前调度服务端只有golang的实现。

### 项目分布
1.nmid调用系统，nmid客户端，nmid工作端（go语言实现）。 [https://github.com/HughNian/nmid](https://github.com/HughNian/nmid)  

2.nmid客户端，nmid工作端（c语言实现）。[https://github.com/HughNian/nmid-c](https://github.com/HughNian/nmid-c)  

3nmid php扩展客户端，nmid php扩展工作端 (c语言实现，php使用)。[https://github.com/HughNian/nmid-php-ext](https://github.com/HughNian/nmid-php-ext)  

## nmid-c介绍  

nmid-c是nmid微服务调度系统的客户端和工作端的C语言实现。同时可以编译成C动态库，在其他C程序中调用。nmid-php-ext作为php的扩展就是用了nmid-c项目的动态库。

1.client目录为客户端C语言源码目录，采用libev用作网络库，nmid的自有I/O通信协议，msgpack作为通信数据格式

2.worker目录为工作端C语言源码目录，采用libev用作网络库，nmid的自有I/O通信协议，msgpack作为通信数据格式

3.run目录为客户端，工作端C程序的可执行文件，可执行文件的编译用的是make

4.build目录为客户端，工作端C程序的动态库目录，动态库的编译用的是cmake 


## nmid-php-ext介绍  

nmid-php-ext是nmid微服务调度系统的客户端和工作端的php扩展实现。采用的是nmid-c的C动态库。目前client和worker是两个独立的扩展装入php中的，后期会合并成一个扩展。

1.clientext目录为客户端php扩展源码目录，主要用到nmid-c的客户端动态库

2.workerext目录为工作端php扩展源码目录，主要用到nmid-c的工作端动态库 

{{% /blocks/section %}}
