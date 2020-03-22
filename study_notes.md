# K8S In Action 读书笔记
写在开始之前

最近发现工作时间长了，会觉得没意思，也会焦虑。究其原因大概因为工作是个消耗的过程，如果只是重复相同的工作没有思考，就是不进则退了，被淘汰是早早晚晚的事情。让整个人都能充实起来的方式就是读书和尝试新东西。就像当年读书时那样，不提最后真的记住了多少，至少总是在努力（被迫）吸收新东西的，所以整个身心都是充盈的。
希望从k8s重新出发。把学习笔记放到这里。和上学时比长进的大概是，不希望单纯是抄书记笔记，而是能消化掉每个技术最精妙的地方。反正也不是准备考试，就写些自己觉得有意思的感受吧。😉

K8S In Action/K8S 妙在哪里

1. 看的是K8S In Action原版，一句话“图清楚，话简单”。这大概是国外书籍共同的优点？当年经双学经济学原理的时候也是，国内的书总是上来就原理，兴致全无。国外的书例子有趣，make your hands dirty，配图也是让人羡慕，一个图大概能囊括一节的内容了。
2. K8S是声明式操作。你只要告诉它你想要什么，而无需告诉它你要怎么要。对于使用者来说，十分友好。当然，了解K8S内在，是排查问题和定制处理自己应用场景不可少的。
3. K8S内部设计，单一职责、消息通知模式，这些设计方式都是我们设计自己系统时可以借鉴的。

学习一门技术的套路

1. why？为啥要有这个技术，历史是怎样的
2. what？这个技术是什么？先上手用一用，再了解理论知识，就像学开车，先让车开起来，再研究为什么
3. how？这个技术内部是怎么实现的？理论&源码

# 历史
单一应用 VS. 微服务应用
- 单一应用：劣势：重量级，不易升级，不易扩容（横向、纵向）
- 微服务：优势：轻量级，可独立扩容；劣势：不易管理（如通讯）

VM VS. Docker
- VM：优势：独立的OS，完全隔离；劣势：重，创建需要较为复杂的准备
- Docker：优势：使用宿主机OS，轻量级即时创建，镜像分层可独立更新；劣势：不是完全隔离，共用宿主机OS和kernel

Docker的隔离是怎么做到的？基于linux的namespace和control groups

K8S的好处
- 简化部署：只要描述你想要怎样的应用部署，不用关心具体容器如何启动在哪里启动
- 提高硬件利用率：容器混部
- 有健康监测和管理
- 自动扩缩容
- 简化开发：测试和生产环境同样，便于问题排查复现

# Docker基础命令
- 查看本地镜像：docker images
- 拉取远端镜像：docker pull
- build自定义镜像：定义Dockerfile，docker build
- 启动容器（后台）：docker run -d
- 查看容器：docker ps
- 查看容器详情：docker inspect
- 推送本地镜像到远端：docker push
- 交互式进入容器：docker exec -it
- 镜像打标：docker tag

# K8S kubectl基础命令
- 查看集群：kubectl cluster-info
- 查看不同资源如node、pod、rc：kubectl get
- 查看不同资源详情：kubectl describe
- 启动资源：kubectl run
- 删除资源：kubectl delete
- 暴露service：kubectl expose
- 扩缩容：kubectl scale