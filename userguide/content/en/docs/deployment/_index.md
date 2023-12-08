---
title: 🚂部署使用
linkTitle: 🚂部署使用
weight: 5
description:
---

nmid的使用可以分为两类，一个是作为微服务的rpc框架使用，同时兼具调度和分发服务的作用，这是最主要的使用场景。
还有一个是可以结合k8s使用，作为k8s微服务容器的sidecar使用。

## rpc使用  

作为rpc使用，就是典型的cliet,nmid-server,worker三者的使用关系。当然可以中间加入注册中心进行使用。
具体的示例可以查看[🌰例子](/docs/examples)和[📡Discovery注册中心](/docs/discovery)

## k8s作为sidecar使用

这里以npartword分词微服务为例

 {{< tabpane persistLang=false >}}
{{< tab header="Deployment file:" disabled=true />}}
{{< tab header="Deployment.yaml" lang="yaml" >}}
apiVersion: apps/v1
kind: Deployment
metadata:
  name: npw-deployment
  namespace: npw
  labels:
    app: npw
spec:
  replicas: 3 #根据情况定义副本数
  selector:
    matchLabels:
      app: npw
  template:
    metadata:
      labels:
        app: npw
    spec:
      containers:
        - name: nmid-sidecar #使用nmid作为npartword服务sidecar
          image: docker.io/hughnian/nmid:56 #这里的tag根据具体镜像tag取值
          command: ["/etc/service/run"]
          ports:
            - name: tcp
              containerPort: 6808
            - name: http
              containerPort: 6809
        - name: npartword
          image: docker.io/hughnian/npartword:3 #这里的tag根据具体镜像tag取值
          command: ["/etc/service/run"]
---
apiVersion: v1
kind: Service
metadata:
  name: npw-service
  namespace: npw
spec:
  selector:
    app: npw
  ports:
    - name: npw-tcp
      nodePort: 30688
      port: 6808
      protocol: TCP
      targetPort: 6808
    - name: npw-http
      nodePort: 30689
      port: 6809
      protocol: TCP
      targetPort: 6809
  type: NodePort
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    field.cattle.io/description: npw-service-http
  name: npw-service-http
  namespace: npw
spec:
  defaultBackend:
    service:
      name: npw-service
      port:
        number: 6809
  rules:
    - host: npw.mytest.com #使用自己相应域名
      http:
        paths:
          - backend:
              service:
                name: npw-service
                port:
                  number: 6809
            path: /
            pathType: Prefix
{{< /tab >}}
    {{< /tabpane >}}