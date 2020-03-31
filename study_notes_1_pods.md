# 1. 核心概念

1. 物理概念

>- master：主控节点

>- nodes：工作节点

2. 逻辑概念

>- process：应用的进程

>- container：容器，一般建议1个container中只包含1个服务对应的1个process（process创建的子process除外），1对1的关系有利于按照container粒度来管理服务，而不用关心container内的每一个服务。

>- pod：pod拥有独立的ip，是k8s调度的最小资源单位（注意最小资源单位不是container哦）。一般建议1个pod中只包含1个container。因为扩缩容都是针对pod来说的，如果将多个container放在一个pod中，只能同时扩缩容。除非多个container的关系非常紧密，比如主服务和主服务的日志采集服务，需要共享日志volume。

>- label：可以为【任意】资源打标，以kv对的形式。label selector：打标后，可以用label来选择对应的资源，支持= != in notin等运算。比如为node按照硬件类型打标，为pod按照计算类型打标，将属于同一标签的node和pod进行配对使用。不同打标分组可以相互重叠。

>- annotation：类似label，但没有annotation selector。更像是长文本的备注。

>- namespace：另一种分组方式，分组之前相互不重叠。不同的namespace下，可以存在同名的资源。常用场景，环境区分，比如namespace=生产环境/测试环境。