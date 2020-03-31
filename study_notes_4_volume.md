## 1. 简述

1个pod上的多个container共享如CPU、RAM、Network的资源，但filesystem是隔离的。如果多个container之间需要共享filesystem 共享持久化存储，则需要使用volume。


## 2. Volume

pod内定义volume，volume的类型有：
- emptyDir：空目录
- gitRepo：git路径，不会自动同步git最新版本
- hostPath：使用node的filesystem，node上多个pod可以共享，因为持久化在node里，而非pod里，即使pod消失也能持久化
- nfs
- configMap、secret等k8s的特殊存储资源
- persistentVolumeClaim：一种动态绑定持久化存储的方式
- 其他外部持久化存储

container可以选择mount或者不mount volume，只有mount的volume才能从container访问

## 3. PersistentVolume & PersistentVolumeClaim

### 3.1 为什么需要PV和PVC

为了将pod使用的volume和具体的底层存储技术解耦，开发者不需要关系volume到底是什么物理存储。

### 3.2 PV和PVC定义方式

admin角色，创建PV，指定具体的物理存储类型、规格、支持的访问方式。

dev角色，创建PVC，指定需要申请的物理存储规格、支持的访问方式。系统自动分配绑定PVC和PV。dev角色，定义pod时mount PV。

### 3.3 PV回收方式

PV属于cluster级别的资源，不属于某个namespace。可以被分配，也可以被释放。

PV的3种回收策略：
- Retain：当绑定的PVC删除时，k8s保留PV内容，PV不能立即被重新分配，手动决定是删除PV还是继续使用
- Recycle：当绑定的PVC删除时，k8s删除PV内容，PV自动保留，可以被重新分配
- Delete：当绑定的PVC删除时，k8s删除PV

## 4. StorageClass

### 4.1 作用

admin角色不用事先创建好PV，而是在用户申请PV时，动态创建。

### 4.2 使用方式

admin角色部署PersistentVolume provisioner，对应不同的存储类型、性能特性等。

admin角色创建StorageClass，使用已定义的provisioner。

dev角色，创建PVC，指定StorageClass，k8s分配已存在的PV或者新建对应类型的PV。 - 所谓Dynamic Provisioning

dev角色，创建pod，mount volume PVC。
