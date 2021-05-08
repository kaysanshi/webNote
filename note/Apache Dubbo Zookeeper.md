## 分布式系统开发一定会遇到的四个问题以及解决方案：

1.服务众多，客户端如何访问。

2.服务众多，服务之间如何通信。

3.服务众多，如何治理。

4.服务众多，如果挂了怎么办；

这四个问题对应了解决四个问题的方式：

1.API网关，服务路由

2.Http,RPC,异步调用

3.服务注册与发现 -》高可用

4.熔断，限流，服务降级

#### 解决方案：

SpringCloud,spring Cloud是一套生态，是为了解决微服务架构遇到的问题，想要使用Spring Cloud必须基于Spring Boot

**1.Spring Cloud Netflix**

​	API网关，zuul组件

​	服务注册与发现，Eureka

​	Fegin -> http Client -->http通信方式，同步阻塞

​    熔断机制 Hystrix

**2.Apache Dubbo Zookeeper**

   Dubbo是一个高效性能的 Java RPC 通信框架，2.6.x

   服务注册与发现，Zookeeper，

   API网关 没有  找第三方或自己实现。

   服务挂了，Hystrix

**3.Spring Cloud Alibaba**

 Spring Cloud Alibaba致力于提供微服务开发的一站式解决方案。此项目包含开发分布式应用微服务的必需组件，方便开发者通过 Spring Cloud 编程模型轻松使用这些组件来开发分布式应用服务。

依托 Spring Cloud Alibaba，您只需要添加一些注解和少量配置，就可以将 Spring Cloud 应用接入阿里微服务解决方案，通过阿里中间件来迅速搭建分布式应用系统。

下一代会是什么呢，Service Mesh 服务网格化，Istio 可能是需要掌握的一套微服务解决方案。

本文主要介绍Apache Dubbo Zookeeper

## Apache Dubbo Zookeeper

目前市场上主流的 **第二套微服务架构解决方案：Spring Boot + Dubbo + Zookeeper**

- Apache Dubbo (incubating) |ˈdʌbəʊ| 是一款高性能、轻量级的开源 Java RPC 框架。
- ZooKeeper 是一种分布式协调服务，用于管理大型主机。在分布式环境中协调和管理服务是一个复杂的过程

#### 单一应用架构 

​			当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成此时，用于简化增删改查工作量的 数据访问框架(ORM) 是关键。

#### 垂直应用架构 

​		当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，将应用拆成互不相干的几个应用，以提升效率。此时，用于加速前端页面开发的 Web框架(MVC) 是关键。

#### 分布式服务架构

​	当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高业务复用及整合的 分布式服务框架(RPC) 是关键。

#### 流动计算架构 

​	当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。
​		此时，用于提高机器利用率的 资源调度和治理中心(SOA) 是关键。

**Dubbo就是资源调度和治理中心的管理工具。**

## Dubbo和Zookpper

### Dubbo

Dubbo是阿里巴巴B2B平台技术部开发的一款Java服务平台框架以及SOA治理方案。其功能主要包括：高性能NIO通信及多协议集成、服务动态寻址与路由、软负载均衡与容错、依赖分析与降级等。Dubbo简单的底层框架如图16-4所示。Registry是服务注册与发现的注册中心， Provider是暴露服务的服务提供方，Consumer是调用远程服务的服务消费方，Monitor是统计服务的调用次数和调用时间的监控中心，Container是服务运行容器。Dubbo简单的调用关系如下：

（1）服务容器Container负责启动、加载、运行服务提供者Provider。

（2）服务提供者Provider在启动时，向注册中心Registry注册自己提供的服务。

（3）服务消费者Consumer在启动时，向注册中心Registry订阅自己所需的服务。

（4）注册中心Registry返回服务提供者地址列表给消费者Provider，如果有变更，注册中心Registry将基于长连接推送，变更数据给消费者Consumer。

（5）服务消费者Consumer从提供者地址列表中基于软负载均衡算法选一台提供者进行调用，如果调用失败，就再选另一台调用。

（6）服务消费者Consumer和提供者Provider在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心Monitor。



[![BYdTMT.jpg](https://s1.ax1x.com/2020/10/30/BYdTMT.jpg)](https://imgchr.com/i/BYdTMT)

**节点角色说明：**

·        **Provider:** 暴露服务的服务提供方。

·        **Consumer:** 调用远程服务的服务消费方。

·        **Registry:** 服务注册与发现的注册中心。

·        **Monitor:** 统计服务的调用次调和调用时间的监控中心。

·        **Container:** 服务运行容器。

**调用关系说明：**

·        0. 服务容器负责启动，加载，运行服务提供者。

·        1. 服务提供者在启动时，向注册中心注册自己提供的服务。

·        2. 服务消费者在启动时，向注册中心订阅自己所需的服务。

·        3. 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。

·        4. 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。

·        5. 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

Dubbo将注册Registry中心进行抽象，使得它可以外接不同的存储媒介给注册中心Registry提供服务，可以作为存储媒介的有**ZooKeeper**、**Memcached**、**Redis**等。引入ZooKeeper作为存储媒介，也就是把ZooKeeper的特性引进来。

首先是负载均衡，单注册中心的承载能力是有限的，在流量达到一定程度的时候就需要分流，负载均衡就是为了分流而存在的。一个**ZooKeeper**群配合相应的Web应用就可以很容易达到负载均衡；然后是资源同步，单单有负载均衡还不够，节点之间的数据和资源需要同步，ZooKeeper集群就天然具备这样的功能；最后是命名服务，将树状结构用于维护全局的服务地址列表，服务提供者在启动的时候，向ZooKeeper的指定节点目录写入自己的URL地址，这个操作就完成了服务的发布。ZooKeeper其他特性还有**Mast选举、分布式锁**等

**使用注解进行服务的远程调用：**

```
@Reference(version = "1.0")
public UserDubboService userDubboService;
```

@Reference注解也是Dubbo框架提供的，在com.alibaba.dubbo.config.annotation. Reference包下。version是@Reference注解的属性，version的版本需要和@Service注解的version版本保持一致，否则服务将无法注入，这一点是需要特别注意的

#### Dubbo安装：

1. 下载代码: `git@github.com:apache/dubbo-admin.git`  **这个要到master分支一定要否者会出问题**

2. 在 `dubbo-admin-server/src/main/resources/application.properties`中指定注册中心地址

3. 构建

   > - `mvn clean package`

如果你使用的是Windows的Powershell，执行上面的命令可能会报错：Unknown lifecycle phase ".test.skip=true".

`mvn clean package '-Dmaven.test.skip=true'`

命令执行完后，会在dubbo-admin/dubbo-admin/target目录下生成一个jar包：dubbo-admin-0.0.2-SNAPSHOT.jar。

启动它： `java -jar dubbo-admin-0.2.0-SNAPSHOT.jar`

**构建dockerfile:**

```dockerfile
From openjdk:8
VOLUME /tmp
add dubbo-admin-0.2.0-SNAPSHOT.jar app.jar
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

执行命令：

```
docker build -t dubbo-admin:1.0 .
```

[![Dums0g.png](https://s3.ax1x.com/2020/11/19/Dums0g.png)](https://imgchr.com/i/Dums0g)



### Apache Zookeeper

ZooKeeper 是一种分布式协调服务，用于管理大型主机。在分布式环境中协调和管理服务是一个复杂的过程。ZooKeeper 通过其简单的架构和 API 解决了这个问题。ZooKeeper 允许开发人员专注于核心应用程序逻辑，而不必担心应用程序的分布式特性。ZooKeeper是一个开源的分布式应用程序协调服务，提供的功能包括命名服务、配置管理、集群管理、分布式锁。

（1）命名服务。可以简单理解为电话簿。电话号码不好记，但是人名好记。要打谁的电话，直接查人名就好了。在分布式环境下，经常需要对应用/服务进行统一命名，便于识别不同服务。类似于域名与IP之间的对应关系，域名容易记住。ZooKeeper通过名称来获取资源或服务的地址、提供者等信息。

（2）配置管理。分布式系统都有大量服务器，比如在搭建Hadoop的HDFS的时候，需要在一台Master主机器上配置好HDFS需要的各种配置文件，然后通过scp命令把这些配置文件复制到其他节点上，这样各个机器拿到的配置信息是一致的，才能成功运行HDFS服务。Zookeeper提供了这样的服务：一种集中管理配置的方法，我们在这个集中的地方修改了配置，所有对这个配置感兴趣的都可以获得变更。这样就省去手动复制配置，还保证了可靠和一致性。

（3）集群管理。集群管理包含两点：是否有机器退出和加入、选举Master。在分布式集群中，经常会由于各种原因（比如硬件故障、软件故障、网络问题等），有些新的节点会加入进来，也有老的节点会退出集群。这个时候，集群中有些机器（比如Master节点）需要感知到这种变化，然后根据这种变化做出对应的决策。Zookeeper集群管理就是感知变化，做出对应的策略。

（4）分布式锁。Zookeeper的一致性文件系统使得锁的问题变得容易。锁服务可以分为两类，一类是保持独占，另一类是控制时序。单机程序的各个进程对互斥资源进行访问时需要加锁，分布式程序分布在各个主机上的进程对互斥资源进行访问时也需要加锁。很多分布式系统有多个可服务的窗口，但是在某个时刻只让一个服务去干活，当这台服务出问题的时候锁释放，立即fail over到另外的服务。在很多分布式系统中都是这么做的，这种设计有一个更好听的名字Leader Election（Leader选举）。举个通俗点的例子，比如去银行取钱，有多个窗口，但是只能有一个窗口对你服务。如果正在对你服务的窗口的柜员突然有急事走了，怎么办呢？找大堂经理（Zookeeper），大堂经理会指定另外的窗口继续为你服务。

Zookeeper的一个最常用的使用场景是担任服务生产者和服务消费者的注册中心，这也是接下来的章节中会使用到的。服务生产者将自己提供的服务注册到Zookeeper中心，服务消费者在进行服务调用的时候先到Zookeeper中查找服务，获取服务生产者的详细信息之后，再去调用服务生产者的内容与数据

以下为 Zookeeper 的基本概念

#### Zookeeper 的数据模型

Zookeeper 的数据模型是什么样子呢？它很像数据结构当中的树，也很像文件系统的目录。

[![g8gnF1.png](https://z3.ax1x.com/2021/05/08/g8gnF1.png)](https://imgtu.com/i/g8gnF1)

树是由节点所组成，Zookeeper 的数据存储也同样是基于节点，这种节点叫做 **Znode**

但是，不同于树的节点，Znode 的引用方式是路径引用，类似于文件路径：

```text
/动物/猫
/汽车/宝马
```

这样的层级结构，让每一个 Znode 节点拥有唯一的路径，就像命名空间一样对不同信息作出清晰的隔离。

##### Znode 包含哪些元素

[![g8gJwd.png](https://z3.ax1x.com/2021/05/08/g8gJwd.png)](https://imgtu.com/i/g8gJwd)

- data：Znode 存储的数据信息。
- ACL：记录 Znode 的访问权限，即哪些人或哪些 IP 可以访问本节点。
- stat：包含 Znode 的各种元数据，比如事务 ID、版本号、时间戳、大小等等。
- child：当前节点的子节点引用

这里需要注意一点，Zookeeper 是为读多写少的场景所设计。Znode 并不是用来存储大规模业务数据，而是用于存储少量的状态和配置信息，`每个节点的数据最大不能超过 1MB`。

#### Zookeeper 的基本操作

创建节点

```text
create
```

删除节点

```text
delete
```

判断节点是否存在

```text
exists
```

获得一个节点的数据

```text
getData
```

设置一个节点的数据

```text
setData
```

获取节点下的所有子节点

```text
getChildren
```

这其中，`exists`，`getData`，`getChildren` 属于读操作。Zookeeper 客户端在请求读操作的时候，可以选择是否设置 **Watch**

#### Zookeeper 的事件通知

我们可以把 **Watch** 理解成是注册在特定 Znode 上的触发器。当这个 Znode 发生改变，也就是调用了 `create`，`delete`，`setData` 方法的时候，将会触发 Znode 上注册的对应事件，请求 Watch 的客户端会接收到异步通知。

具体交互过程如下：

- 客户端调用 `getData` 方法，`watch` 参数是 `true`。服务端接到请求，返回节点数据，并且在对应的哈希表里插入被 Watch 的 Znode 路径，以及 Watcher 列表。

![img](https://www.funtl.com/assets/Lusifer201810170012.png)

- 当被 Watch 的 Znode 已删除，服务端会查找哈希表，找到该 Znode 对应的所有 Watcher，异步通知客户端，并且删除哈希表中对应的 Key-Value。

![img](https://www.funtl.com/assets/Lusifer201810170013.png)

#### Zookeeper 的一致性

Zookeeper 身为分布式系统协调服务，如果自身挂了如何处理呢？为了防止单机挂掉的情况，Zookeeper 维护了一个集群。如下图：

![img](https://www.funtl.com/assets/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20181017182528.jpg)

Zookeeper Service 集群是一主多从结构。

在更新数据时，首先更新到主节点（这里的节点是指服务器，不是 Znode），再同步到从节点。

在读取数据时，直接读取任意从节点。

为了保证主从节点的数据一致性，Zookeeper 采用了 **ZAB 协议**，这种协议非常类似于一致性算法 **Paxos** 和 **Raft**。

##### 什么是 ZAB

Zookeeper Atomic Broadcast，有效解决了 Zookeeper 集群崩溃恢复，以及主从同步数据的问题。

##### ZAB 协议定义的三种节点状态

- Looking ：选举状态。
- Following ：Follower 节点（从节点）所处的状态。
- Leading ：Leader 节点（主节点）所处状态。

##### 最大 ZXID

最大 ZXID 也就是节点本地的最新事务编号，包含 epoch 和计数两部分。epoch 是纪元的意思，相当于 Raft 算法选主时候的 term。

##### ZAB 的崩溃恢复

假如 Zookeeper 当前的主节点挂掉了，集群会进行崩溃恢复。ZAB 的崩溃恢复分成三个阶段：

**Leader election**

选举阶段，此时集群中的节点处于 Looking 状态。它们会各自向其他节点发起投票，投票当中包含自己的服务器 ID 和最新事务 ID（ZXID）。

![img](https://www.funtl.com/assets/Lusifer201810170014.png)

接下来，节点会用自身的 ZXID 和从其他节点接收到的 ZXID 做比较，如果发现别人家的 ZXID 比自己大，也就是数据比自己新，那么就重新发起投票，投票给目前已知最大的 ZXID 所属节点。

![img](https://www.funtl.com/assets/Lusifer201810170015.png)

每次投票后，服务器都会统计投票数量，判断是否有某个节点得到半数以上的投票。如果存在这样的节点，该节点将会成为准 Leader，状态变为 Leading。其他节点的状态变为 Following。

![img](https://www.funtl.com/assets/Lusifer201810170016.png)

**Discovery**

发现阶段，用于在从节点中发现最新的 ZXID 和事务日志。或许有人会问：既然 Leader 被选为主节点，已经是集群里数据最新的了，为什么还要从节点中寻找最新事务呢？

这是为了防止某些意外情况，比如因网络原因在上一阶段产生多个 Leader 的情况。

所以这一阶段，Leader 集思广益，接收所有 Follower 发来各自的最新 epoch 值。Leader 从中选出最大的 epoch，基于此值加 1，生成新的 epoch 分发给各个 Follower。

各个 Follower 收到全新的 epoch 后，返回 ACK 给 Leader，带上各自最大的 ZXID 和历史事务日志。Leader 选出最大的 ZXID，并更新自身历史日志。

**Synchronization**

同步阶段，把 Leader 刚才收集得到的最新历史事务日志，同步给集群中所有的 Follower。只有当半数 Follower 同步成功，这个准 Leader 才能成为正式的 Leader。

自此，故障恢复正式完成。

##### ZAB 的数据写入

**Broadcast**

ZAB 的数据写入涉及到 Broadcast 阶段，简单来说，就是 Zookeeper 常规情况下更新数据的时候，由 Leader 广播到所有的 Follower。其过程如下：

- 客户端发出写入数据请求给任意 Follower。
- Follower 把写入数据请求转发给 Leader。
- Leader 采用二阶段提交方式，先发送 Propose 广播给 Follower。
- Follower 接到 Propose 消息，写入日志成功后，返回 ACK 消息给 Leader。
- Leader 接到半数以上ACK消息，返回成功给客户端，并且广播 Commit 请求给 Follower

![img](https://www.funtl.com/assets/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20181017192657.jpg)

ZAB 协议既不是强一致性，也不是弱一致性，而是处于两者之间的**单调一致性（顺序一致性）**。它依靠事务 ID 和版本号，保证了数据的更新和读取是有序的。

#### Zookeeper 的应用场景

##### 分布式锁

这是雅虎研究员设计 Zookeeper 的初衷。利用 Zookeeper 的临时顺序节点，可以轻松实现分布式锁。

##### 服务注册和发现

利用 Znode 和 Watcher，可以实现分布式服务的注册和发现。最著名的应用就是阿里的分布式 RPC 框架 Dubbo。

##### 共享配置和状态信息

Redis 的分布式解决方案 `Codis`，就利用了 Zookeeper 来存放数据路由表和 `codis-proxy` 节点的元信息。同时 `codis-config` 发起的命令都会通过 ZooKeeper 同步到各个存活的 `codis-proxy`。

此外，`Kafka`、`HBase`、`Hadoop`，也都依靠Zookeeper同步节点信息，实现高可用。

#### Zookeeper 如何实现分布式锁

##### Zookeeper 分布式锁的原理

##### Zookeeper和Redis分布式锁的比较

#### Dubbo和Zookpper安装

##### dubbo如何使用：

​		先要在注册中心注册(注册中心)：ZooKeeper使用

```
发布服务：
<!-- 和本地服务一样实现远程服务 -->
<bean id="xxxService" class="com.xxx.XxxServiceImpl" />
<!-- 增加暴露远程服务配置 -->
<dubbo:service interface="com.xxx.XxxService" ref="xxxService" />

调用服务：
<!-- 增加引用远程服务配置 -->
<dubbo:reference id="xxxService" interface="com.xxx.XxxService" />
<!-- 和本地服务一样使用远程服务 -->
<bean id="xxxAction" class="com.xxx.XxxAction">
<property name="xxxService" ref="xxxService" />
</bean>
dubbo使用只需要把jar包放到tomcat中
```


参考修改dubbo不启动的博客：https://blog.csdn.net/qq_37256896/article/details/90048639

dubbo配置dubbo.properties：

```
dubbo.registry.address=zookeeper://127.0.0.1:2181  
dubbo.admin.root.password=root
dubbo.admin.guest.password=guest 
```

##### ZooKeeper使用：
​				注册中心负责服务地址的注册与查找，相当于目录服务，服务提供者和消费者只在启动时与注册中心交互，注册中心不转发请求，压力较小。使用dubbo-2.3.3以上版本，建议使用zookeeper注册中心。

​				Zookeeper是Apacahe Hadoop的子项目，是一个树型的目录服务，支持变更推送，适合作为Dubbo服务的注册中心，工业强度较高，可用于生产环境，并推荐使用

Zookeeper安装：
			1、可以作为集群的管理工具使用。
			2、可以集中管理配置文件。
			安装步骤：
			第一步：安装jdk
			第二步：把zookeeper的压缩包上传到linux系统。
			第三步：解压缩压缩包
			tar -zxvf zookeeper-3.4.6.tar.gz
			第四步：进入zookeeper-3.4.6目录，创建data文件夹。
			第五步：把zoo_sample.cfg改名为zoo.cfg

```
[root@localhost conf]# mv zoo_sample.cfg zoo.cfg
tickTime = 2000
dataDir = /path/to/zookeeper/data
clientPort = 2181
initLimit = 5
syncLimit = 2
```

​			第六步：修改data属性：dataDir=/root/zookeeper-3.4.6/data
​			第七步：启动zookeeper
​			[root@localhost bin]# ./zkServer.sh start
关闭：[root@localhost bin]# ./zkServer.sh stop
查看状态：[root@localhost bin]# ./zkServer.sh status

重启服务：[root@localhost bin]# ./zkServer.sh restart

<font color="red">注意：需要关闭防火墙。</font>
			service iptables stop
			永久关闭修改配置开机不启动防火墙：
			chkconfig iptables off
			如果不能成功启动zookeeper，需要删除data目录下的zookeeper_server.pid文件。			

##### 基于Docker安装Zookeeper

Zookeeper 部署有三种方式，单机模式、集群模式、伪集群模式，以下采用 Docker 的方式部署

**注意：** 集群为大于等于3个奇数，如 3、5、7,不宜太多，集群机器多了选举和数据同步耗时长，不稳定。

###### 单机模式

<font color="red">**新建zookeeper文件夹。在zookeeper文件夹中新建dokcer-compose.yml**</font>

```yaml
version: '3.1'
services:
  zoo1:
    image: zookeeper
    restart: always
    hostname: zookeeper
    ports:
      - 2181:2181  
```

进入容器。

`docker exec -it zookeeper_zool_1 /bin/bash`

检查是否安装成功：l

`./bin/zkServer.sh status`

[![DugkjI.png](https://s3.ax1x.com/2020/11/19/DugkjI.png)](https://imgchr.com/i/DugkjI)

进入bin目录后查看bin目录下的文件：

```bash
root@zookeeper:/apache-zookeeper-3.6.2-bin# ls -l bin
total 64
-rwxr-xr-x. 1 zookeeper zookeeper   232 Sep  4 12:43 README.txt
-rwxr-xr-x. 1 zookeeper zookeeper  2066 Sep  4 12:43 zkCleanup.sh
-rwxr-xr-x. 1 zookeeper zookeeper  1158 Sep  4 12:43 zkCli.cmd
-rwxr-xr-x. 1 zookeeper zookeeper  1620 Sep  4 12:43 zkCli.sh
-rwxr-xr-x. 1 zookeeper zookeeper  1843 Sep  4 12:43 zkEnv.cmd
-rwxr-xr-x. 1 zookeeper zookeeper  3690 Sep  4 12:43 zkEnv.sh
-rwxr-xr-x. 1 zookeeper zookeeper  4559 Sep  4 12:43 zkServer-initialize.sh
-rwxr-xr-x. 1 zookeeper zookeeper  1286 Sep  4 12:43 zkServer.cmd
-rwxr-xr-x. 1 zookeeper zookeeper 11116 Sep  4 12:43 zkServer.sh
-rwxr-xr-x. 1 zookeeper zookeeper   988 Sep  4 12:43 zkSnapShotToolkit.cmd
-rwxr-xr-x. 1 zookeeper zookeeper  1377 Sep  4 12:43 zkSnapShotToolkit.sh
-rwxr-xr-x. 1 zookeeper zookeeper   996 Sep  4 12:43 zkTxnLogToolkit.cmd
-rwxr-xr-x. 1 zookeeper zookeeper  1385 Sep  4 12:43 zkTxnLogToolkit.sh
root@zookeeper:/apache-zookeeper-3.6.2-bin# 
```

通过 `zkCli` 来访问 Zookeeper 的控制台来进行管理。

```bash
root@zookeeper:/apache-zookeeper-3.6.2-bin# zkCli.sh -server 127.0.0.1:2181
Connecting to 127.0.0.1:2181
2020-11-20 02:26:42,393 [myid:] - INFO  [main:Environment@98] - Client environment:zookeeper.version=3.6.2--803c7f1a12f85978cb049af5e4ef23bd8b688715, built on 09/04/2020 12:44 GMT
2020-11-20 02:26:42,397 [myid:] - INFO  [main:Environment@98] - Client environment:host.name=zookeeper
2020-11-20 02:26:42,397 [myid:] - INFO  [main:Environment@98] - Client environment:java.version=11.0.9.1
2020-11-20 02:26:42,398 [myid:] - INFO  [main:Environment@98] - Client environment:java.vendor=Oracle Corporation
2020-11-20 02:26:42,398 [myid:] - INFO  [main:Environment@98] - Client environment:java.home=/usr/local/openjdk-11
...
...
Welcome to ZooKeeper!
2020-11-20 02:26:42,475 [myid:127.0.0.1:2181] - INFO  [main-SendThread(127.0.0.1:2181):ClientCnxn$SendThread@1167] - Opening socket connection to server localhost/127.0.0.1:2181.
2020-11-20 02:26:42,475 [myid:127.0.0.1:2181] - INFO  [main-SendThread(127.0.0.1:2181):ClientCnxn$SendThread@1169] - SASL config status: Will not attempt to authenticate using SASL (unknown error)
JLine support is enabled
2020-11-20 02:26:42,491 [myid:127.0.0.1:2181] - INFO  [main-SendThread(127.0.0.1:2181):ClientCnxn$SendThread@999] - Socket connection established, initiating session, client: /127.0.0.1:40896, server: localhost/127.0.0.1:2181
2020-11-20 02:26:42,511 [myid:127.0.0.1:2181] - INFO  [main-SendThread(127.0.0.1:2181):ClientCnxn$SendThread@1433] - Session establishment complete on server localhost/127.0.0.1:2181, session id = 0x10013cdbf990003, negotiated timeout = 30000

WATCHER::

WatchedEvent state:SyncConnected type:None path:null

[zk: 127.0.0.1:2181(CONNECTED) 0] create /hello-zone 'world'
Created /hello-zone
[zk: 127.0.0.1:2181(CONNECTED) 1] ls /
[dubbo, hello-zone, zookeeper]
[zk: 127.0.0.1:2181(CONNECTED) 2] 


```

去git 的dubbo 源码 ，然后打jar包，上传到服务器。

这个时候直接启动jar 就能访问到了。 `java -jar  dubbo-admin-0.2.0-SNAPSHOT.jar`

注意这个地方我的那个application.properties并未改动。

```
admin.registry.address=zookeeper://127.0.0.1:2181
admin.config-center=zookeeper://127.0.0.1:2181
admin.metadata-report.address=zookeeper://127.0.0.1:2181
```

请看页面效果：

[![DMvbnI.png](https://s3.ax1x.com/2020/11/20/DMvbnI.png)](https://imgchr.com/i/DMvbnI)



**通过构建镜像方式让其启动(失败)：**

将dubbo构建成一个jar包：

<font color="red">**将打包时的admin-server中的application.properties配置文件修该 将127.0.0.1修改为zookeeper**</font>

```properties
# centers in dubbo2.7
admin.registry.address=zookeeper://zookeeper:2181
admin.config-center=zookeeper://zookeeper:2181
admin.metadata-report.address=zookeeper://zookeeper:2181
```

<font color="red">**仍然出错：说找不到 zokeeper:2181**</font>

**发现原来时hotname这个属性导致的：**

**修改成以下的yml**

```yaml
version: '3.1'
services:
  zool:
    image: zookeeper
    restart: always
    hostname: zookeeper
    ports:
      - 2181:2181
  dubbo_admin:
    image: dubbo_admin:1.0
    links:
      - zool:zookeeper
    depends_on:
      - zool
    ports:
      - 3003:3003   
    
```

**修改后就可以启动了  但是如果单独运行 dubbo jar包还是不能找到 zookeeper:2181**：

[![Du5MdI.png](https://s3.ax1x.com/2020/11/19/Du5MdI.png)](https://imgchr.com/i/Du5MdI)

按道理说这样是不应该的，所以查看了网络端口：

`netstat -anp`

因为前几天装的nginx可以访问而这个不可以访问。我发限localIp和Foreign Adress与nginx不一致》
[![Du5IfK.png](https://s3.ax1x.com/2020/11/19/Du5IfK.png)](https://imgchr.com/i/Du5IfK)





###### 集群模式

**docker-compose.yml**

```yaml
version: '3.1'
services:
  zoo1:
    image: zookeeper
    restart: always
    environment:
      ZOO_MY_ID: 1
      ZOO_SERVERS: server.1=192.168.75.130:2888:3888 server.2=192.168.75.134:2888:3888 server.3=192.168.75.135:2888:3888
    network_mode: host
```

**验证测试**

```
root@UbuntuBase:/usr/local/docker/zookeeper# docker exec -it zookeeper_zoo1_1 /bin/bash
bash-4.3# ./bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /conf/zoo.cfg
Mode: leader
```

第二台主机

**docker-compose.yml**

```yaml
version: '3.1'
services:
  zoo2:
    image: zookeeper
    restart: always
    environment:
      ZOO_MY_ID: 2
      ZOO_SERVERS: server.1=192.168.75.130:2888:3888 server.2=192.168.75.134:2888:3888 server.3=192.168.75.135:2888:3888
    network_mode: host
```

验证测试

```text
root@UbuntuBase:/usr/local/docker/zookeeper# docker exec -it zookeeper_zoo2_1 /bin/bash
bash-4.3# ./bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /conf/zoo.cfg
Mode: follower
```

第三台主机

**docker-compose.yml**

```yaml
version: '3.1'
services:
  zoo3:
    image: zookeeper
    restart: always
    environment:
      ZOO_MY_ID: 3
      ZOO_SERVERS: server.1=192.168.75.130:2888:3888 server.2=192.168.75.134:2888:3888 server.3=192.168.75.135:2888:3888
    network_mode: host
```

验证测试

```text
root@UbuntuBase:/usr/local/docker/zookeeper# docker exec -it zookeeper_zoo3_1 /bin/bash
bash-4.3# ./bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /conf/zoo.cfg
Mode: follower
```

###### 伪集群模式

**需要配合dubbo:**

1. 下载代码: `git clone https://github.com/apache/dubbo-admin.git`

2. 在 `dubbo-admin-server/src/main/resources/application.properties`中指定注册中心地址

3. 构建

   > - `mvn clean package`

如果你使用的是Windows的Powershell，执行上面的命令可能会报错：Unknown lifecycle phase ".test.skip=true".

`mvn clean package '-Dmaven.test.skip=true'`

命令执行完后，会在dubbo-admin/dubbo-admin/target目录下生成一个jar包：dubbo-admin-0.0.2-SNAPSHOT.jar。

**构建dockerfile:**

```dockerfile
From openjdk:8
VOLUME /tmp
add dubbo-admin-0.2.0-SNAPSHOT.jar app.jar
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

执行命令：

```
docker build -t dubbo-admin:1.0 .
```

[![Dums0g.png](https://s3.ax1x.com/2020/11/19/Dums0g.png)](https://imgchr.com/i/Dums0g)

编排docker-compose.yml

```yaml
version: '3.1'
services:
  zoo1:
    image: zookeeper
    restart: always
    hostname: zoo1
    ports:
      - 2181:2181
    environment:
      ZOO_MY_ID: 1
      ZOO_SERVERS: server.1=0.0.0.0:2888:3888 server.2=zoo2:2888:3888 server.3=zoo3:2888:3888

  zoo2:
    image: zookeeper
    restart: always
    hostname: zoo2
    ports:
      - 2182:2181
    environment:
      ZOO_MY_ID: 2
      ZOO_SERVERS: server.1=zoo1:2888:3888 server.2=0.0.0.0:2888:3888 server.3=zoo3:2888:3888

  zoo3:
    image: zookeeper
    restart: always
    hostname: zoo3
    ports:
      - 2183:2181
    environment:
      ZOO_MY_ID: 3
      ZOO_SERVERS: server.1=zoo1:2888:3888 server.2=zoo2:2888:3888 server.3=0.0.0.0:2888:3888
```

前台启动： `docker-compose up` 

后台启动： `docker-compose up -d`

- 分别以交互方式进入容器查看 

```text
docker exec -it zookeeper_zoo1_1 /bin/bash
```

```text
docker exec -it zookeeper_zoo2_1 /bin/bash
```

```text
docker exec -it zookeeper_zoo3_1 /bin/bash
```