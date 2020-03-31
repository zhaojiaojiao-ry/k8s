## 1. 简述

应用部署后，肯定需要被调用方访问，service打通网络访问，实现负载均衡等。

按照服务的server、client方，server/client是属于cluster内部、外部，可以分为4种情况。

## 2. server方

### 2.1 内部server

定义service的3元素
- label selector：提供service的pod选择器
- target port：pod上用于提供服务的port
- port：client可见的service port，该port上的请求会被转发到对应pod的target port （一个service内可以定义多个port提供服务，如http port和https port）

k8s为service分配：
- cluster ip：cluster内部可访问的ip

### 2.2 endpoints

用于表明service访问点的一种资源。对于使用label selector定义的service，endpoints就是被选中的pod的地址。

### 2.3 外部server

如何指定外部server挂载在service下：
- 定义endpoints：指明外部server地址（ip+port），endpoints名称要和service名称一致，才能自动匹配
- 指定外部server的域名

## 3. client方

### 3.1 内部client

2种访问方式
- 环境变量：晚于service创建而创建的pod，可以从环境变量获得service的cluster ip和port （是旁路转发？还是代理？需要了解k8s内部实现方式？）
- DNS：k8s为每个service配置了内部dns域名

### 3.2 外部client

3种访问方式
- NodePort：node ip+node port访问，node ip可以是cluster中任意一个node的ip，不论这个node上是否有对应的pod server。client -> node ip+node port -> pod ip+target port
- LoadBalancer：NodePort的扩展，client -> loadbalancer ip -> node ip+node port(非用户指定，系统内部分配) -> pod ip+target port
- Ingress：HTTP层，1个Ingress可设置多个域名对应多个Service。可以在Ingress上设置TLS，支持https


## 4. readiness probe

健康检查，pod是否可以提供service，注意和liveness probe区分，失败不会重建pod，只是不转发service请求了

3种检查方式：（配置在container上）
- exec执行命令，看exit code
- HTTP GET，看status code
- TCP socket，看connection是否建立


