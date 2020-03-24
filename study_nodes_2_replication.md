## 简述

K8S的重要作用之一，就是可以帮助用户管理容器，健康检查、恢复服务、扩缩容等。

## pod健康检查（liveness probe）

3种方式：

1. http get请求，看status code
2. tcp，看connection建立
3. 执行cmd，看exit code

如果健康检查失败，且pod是被管理中的，则k8s会新启动pod，来替代健康检查失败的pod。

何为被管理中的pod？手动启动的pod是不被管理的，崩溃就崩溃了。通过Replication Controller、Replication Sets、Daemon Sets等资源启动的pod是被管理的。

## Replication Controller

3部分构成：

1. label selector：用于筛选哪些pod在被管理的范围内，rc上的label selector需要和pod template中的label一致
2. replica count：pod需要保证有多少个
3. pod template：新建pod使用的模板，只对新建的pod生效，已经创建的pod不会受影响

这3部分设置，随时可以更新。

rc会对cluster内符合label selector的pod进行监控，如果健康数目和指定的replica count不一致，则rc会进行pod的新建或者删除，新建时使用pod template。

删除rc时，可以只删除rc，不删除pod；也可以同时删除rc和pod。

## Replication Set

和Replication Controller非常类似，唯一不同是label selector支持更高级的selector运算，比如指定label key存在但不考虑label value。

rs将逐步替代rc。

## Daemon Set

和Replcation Set非常类似，唯一不同是ds是保证在每个node上有且只建立一个指定pod。通常用于一些系统级别服务的管理。而rc和rs，是在cluster内随意的进行pod调度，不保证每个node上一个pod。

## Job

有些服务是需要持续存在的，可以使用rc、rs管理。而有些服务是只需要执行完成就退出的，此时用job管理。

>- 如果持续性服务崩溃，rc会重建pod，继续执行。

>- 如果一次性服务崩溃，job会重建pod，继续执行，直到job成功执行结束。

## CronJob

job需要周期性定时调度执行，使用CronJob。
