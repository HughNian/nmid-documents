
---
title: 🚀开始
weight: 1
aliases: [/docs/getting-started/]
date: 2018-07-30
description:
---

## 👋nmid介绍

nmid意思为中场指挥官，足球场上的中场就是统领进攻防守的核心。咱们这里是服务程序的调度核心。是一个轻量级分布式微服务RPC框架。

1.pkg/server目录为nmid微服务调度服务端go实现，采用协程以及管道的异步通信，带有连接池，自有I/O通信协议，msgpack做通信数据格式。      

2.pkg/worker目录为nmid的工作端go实现，目前也有c语言实现，以及php扩展实现，可以实现golang, php, c等作为工作端，从而实现跨语言平台提供功能服务。             

3.pkg/client目录为nmid的客户端go实现，目前也有c语言实现，以及php扩展实现，可以实现golang, php, c等作为客户端，从而实现跨语言平台调用功能服务。      

4.example目录为demo运行目录。为go实现的客户端示例，调度服务端示例，客户端示例。目前调度服务端只有golang的实现。  

5.C语言版本：https://github.com/HughNian/nmid-c  

6.PHP扩展：https://github.com/HughNian/nmid-php-ext  

7.支持http请求nmid服务

## 💪what can do  
1.作为rpc微服务使用 

2.作为http微服务使用    

2.作为k8s微服务的sidecar使用

4.作为简单faas的函数运行时

## 📐建议配置

```shell
cat /proc/version
Linux version 3.10.0-957.21.3.el7.x86_64 ...(centos7)

go version
go1.18.5 linux/amd64

gcc --version
gcc (GCC) 4.8.5 20150623 (Red Hat 4.8.5-36)

cmake --version
cmake version 3.11.4

```

## 🔨编译安装步骤

```shell
git clone https://github.com/HughNian/nmid.git

1.client
cd nmid/example/client/testclient
make

2.server
cd nmid/cmd/server
make

3.worker
cd nmid/example/worker/worker1
make

```