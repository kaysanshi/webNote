## SpringCloud

学习微服务架构必须有三年以上的开发的经验：

### 微服务概念

把一个大型的单体应用拆分为数十个支持微服务，他可扩展单个组件而不是整个的应用程序堆栈，从而满足服务等级协议

​        `定义：`围绕业务领域组件进行创建组件，这些应用可独立进行开发，管理迭代，在分散的组件中使用云架构和平台式部署，管理，和服务功能，使得产品交互更加的简单

​        `本质:`是用一些功能比较明确的，业务精练的服务去解决更大的更实际的问题，（2012年为微服务元年）

### 微服务架构：

微服务架构是一种架构概念，旨在通过将功能分解到各个离散的服务中以实现对解决方案的解耦。 它的主要作用是将功能分解到离散的各个服务当中，从而降低系统的耦合性，并提供更加灵活的服务支持。

#### 缺点

​     1.微服务的另一个挑战是分区数据库架构。更新多个业务实体的业务事务是相当普遍的。这些事务在单体应用中的实现显得微不足道，因为单体只存在一个单独的数据库。在基于微服务的应用程序中，您需要更新不同服务所用的数据库。

  ​    通常不会选择分布式事务，不仅仅是因为 CAP 定理。他们根本不支持如今高度可扩展的 NoSQL 数据库和消息代理。

​    2.部署复杂

​    3.测试微服务应用程序也很复杂

​    4.微服务架构模式的另一个主要挑战是实现了跨越多服务变更

​    5.每个服务都有多个运行时实例。还有更多的移动部件需要配置、部署、扩展和监控

​       构建复杂的微服务应用程序本质上是困难的。单体架构模式只适用于简单、轻量级的应用程序，如果您使用它来构建复杂应用，您最终会陷入痛苦的境地。

  ​      微服务架构模式是复杂、持续发展应用的一个更好的选择。尽管它存在着缺点和实现挑战

  ​      微服务的数据的库的设置：是反范式的设计模式：根据情况来设计不同的数据库

#### 优点

-  1.第一，它解决了复杂问题。它把可能会变得庞大的单体应用程序分解成一套服务。虽然功能数量不变，但是应用程序已经被分解成可管理的块或者服务。

- 2.这种架构使得每个服务都可以由一个团队独立专注开发。开发者可以自由选择任何符合服务 API 契约的技术。

- 3.微服务架构模式可以实现每个微服务独立部署。开发人员根本不需要去协调部署本地变更到服务。这些变更一经测试即可立即部署

- 4.微服务架构模式使得每个服务能够独立扩展。您可以仅部署满足每个服务的容量和可用性约束的实例数目。此外，您可以使用与服务资源要求最匹配的硬件。目标在于充分分解应用程序以方便应用的敏捷开发和部署，

  

##### CAP定理：

  ​       一个分布式系统最多只能同时满足一致性（Consistency）、可用性（Availability）和分区容错性（Partition tolerance）这三项中的两项。

  ​        C:即更新操作成功并返回客户端完成后，所有节点在同一时间的数据完全一致。

  ​        A:即服务一直可用，而且是正常响应时间

  ​        P:即分布式系统在遇到某节点或网络分区故障的时候，仍然能够对外提供满足一致性和可用性的服务

  ##### CAP权衡：

​                C 必须保证。网络发生故障宁可停止服务，这是保证 CA，舍弃 P。貌似这几年国内银行业发生了不下 10 起事故，但影响面不大，报到也不多，广大群众知道的少。

​		还有一种是保证 CP，舍弃 A。例如网络故障是只读不写。孰优孰略，没有定论，只能根据场景定夺，适合的才是最好的   

  #####   BASE理论：

  ​           ebay架构师在 ACM 上发表文章提出 BASE 理论，BASE 理论是对 CAP 理论的延伸，核心思想是即使无法做到强一致性（Strong Consistency，CAP 的一致性就是强一致性），但应用可以采用适合的方式达到最终一致性（Eventual Consitency）。     

### 服务间的通信

  #### 同步调用：网络间只有字符串可以通过穿透防火墙

>   ​            Rest:（对外）Http通信 
>
>   ​                Rest API 
>
>   ​                        String json=/usr/list;
>   ​                        User usr=new User();
>   ​                        usr.setId(json.getId)
>
>   ​                使用框架：springboot+Spring Cloud;
>
>   ​            RPC：（对内） 远程过程调用
>
>   ​                调用也是对内部的，同样需要new出的，但是不是直接new User()的，而是new 的另一个框架：Dubbo
>
>   ​            问题：就会出现阻塞，出现单点故障，
>
>   ​            RPC 也有自己的优点，传输协议更高效，安全更可控，
>
>   ​            特别在一个公司内部，如果有统一个的开发规范和统一的服务框架时，
>
>   ​            他的开发效率优势更明显些。就看各自的技术积累实际条件，自己的选择了

  #### 异步消息调用：

  ​            异步消息的方式在分布式系统中有特别广泛的应用，他既能减低调用服务之间的耦合，又能成为调用之间的缓冲，确保消息积压不会冲垮被调用方， 同时能保证调用方的服务体验，继续干自己该干的活，不至于被后台性能拖慢。不过需要付出的代价是一致性的减弱，需要接受数据 最终一致性；
 还有就是后台服务一般要实现 幂等性，因为消息送出于性能的考虑一般会有重复（保证消息的被收到且仅收到一次对性能是很大的考验）；最后就是必须引入一个独立的 Broker
  ​            Kafka

  ​            Notify

  ​            MessageQueue  ​    

####  服务间是如何发现的呢：

  ​        在微服务架构中，一般每一个服务都是有多个拷贝，来做负载均衡。一个服务随时可能下线，也可能应对临时访问压力增加新的服务节点。

  ​        服务之间如何相互感知？服务如何管理？

  ​        这就是服务发现的问题了。一般有两类做法，也各有优缺点。基本都是通过 Zookeeper 等类似技术做服务注册信息的分布式管理。

  ​        当服务上线时，服务提供者将自己的服务信息注册到 ZK（或类似框架），并通过心跳维持长链接，实时更新链接信息。服务调用者通过 ZK 寻址，根据可定制算法，找到一个服务，还可以将服务信息缓存在本地以提高性能。当服务下线时，ZK 会发通知给服务客户端    

  #### 基于客户端的服务注册与发现

  ​         优点是架构简单，扩展灵活，只对服务注册器依赖。缺点是客户端要维护所有调用服务的地址，有技术难度，一般大公司都有成熟的内部框架支持，比如 Dubbo。

  ​         每次需要调用服务时需要在zookeeper中注册发现中查询，前提是每一个服务新建时需要在这个注册中心，进行ip注册和端口，服务名称。

#### 基于服务端的服务注册与发现：

​      优点是简单，所有服务对于前台调用方透明，一般在小公司在云服务上部署的应用采用的比较多。

> ​           	主要有这几个框架：
> ​                Eureka:
> ​                Consul:
> ​                Zookeeper:

​      如果服务挂了如何解决呢：

> ​            重试机制:即当没响应时他会自己再次发送请求，
>
> ​            限流：秒杀机制，直接在客户端中让百分之90的人不能直接访问到服务，而是直接提示出，
>
> ​            熔断机制：在服务端进行流量的控制
>
> ​            负载均衡：  降级（本地缓存）

#### 服务注册与发现原理：

[![dO4A81.png](https://s1.ax1x.com/2020/08/31/dO4A81.png)](https://imgchr.com/i/dO4A81)

### 单点故障：

​         在分不式锁服务中，有一种典型的应用场景，通过集群对Master进行选举，
​         即主节点不能使用的话，既不能为从节点提供服务。
​         传统的解决方案：主节点-------》备用主节点
​                        备用主节点------》》从节点 《《--------主节点  
​           这种方式网络突然有问题时会进行对回复时就会丢失包，而这样的情况就是就会把两个主节点都是在这里
​          主要使用zookeeper来解决这个问题，服务注册与发现
​          引入：为什么使用分布式锁：

   ### 微服务架构的设计模式：

####  微服务架构需要考虑的问题：

-   API Gateway
-   服务间调用
-   服务发现
-   服务容错
-   服务部署
-   数据调用 

[![DGOg4P.png](https://s3.ax1x.com/2020/11/23/DGOg4P.png)](https://imgchr.com/i/DGOg4P)

#### 微服务设计模式：

- ​            聚合器微服务形式：:由api网关进行对其聚合


  [![DGOR9f.png](https://s3.ax1x.com/2020/11/23/DGOR9f.png)](https://imgchr.com/i/DGOR9f)



  

- ​            代理微服务设计模式:代理委派请求，在这种情况下，客户端并不聚合数据，但会根据业务需求的差别调用不同的微服务。代理可以仅仅委派请求，也可以进行数据转换工作

  [![DGOW38.png](https://s3.ax1x.com/2020/11/23/DGOW38.png)](https://imgchr.com/i/DGOW38)

- ​            链式微服务设计模式：链式同步调用，这种模式在接收到请求后会产生一个经过合并的响应

  [![DGO6AI.png](https://s3.ax1x.com/2020/11/23/DGO6AI.png)](https://imgchr.com/i/DGO6AI)

- ​            分支微服务设计模式：同时允许两个微服务链，这种模式是聚合器模式的扩展，允许同时调用两个微服务链

  [![DGOcNt.png](https://s3.ax1x.com/2020/11/23/DGOcNt.png)](https://imgchr.com/i/DGOcNt)

- ​            数据共享微服务设计模式：SQL数据库反规范化可能会导致数据重复和不一致。自治是微服务的设计原则之一，就是说微服务是全栈式服务。但在重构现有的“单体应用（Monolithic Application）”时，SQL 数据库反规范化可能会导致数据重复和不一致。因此，在单体应用到微服务架构的过渡阶段，可以使用这种设计模式

  [![DGO5uQ.png](https://s3.ax1x.com/2020/11/23/DGO5uQ.png)](https://imgchr.com/i/DGO5uQ)

- ​            异步消息传递微服务设计模式：使用消息队列，虽然 REST 设计模式非常流行，但它是同步的，会造成阻塞。因此部分基于微服务的架构可能会选择使用消息队列代替 REST 请求/响应

  [![DGOhjg.png](https://s3.ax1x.com/2020/11/23/DGOhjg.png)](https://imgchr.com/i/DGOhjg)

#### 微服务架构开发建议：

>   		应用程序的核心是业务逻辑，按照业务或客户需求组织资源（这是最难的）
>  		 做有生命的产品，而不是项目
>   		全栈化
>   		后台服务贯彻 Single Responsibility Principle（单一职责原则）
>   		VM -> Docker
>   		DevOps
>	
>   		springCloud 基于springboot的技术技术框架；
>   		java原生云开发=springCloud+spring boot

### 分布式系统开发一定会遇到的四个问题以及解决方案：

​	1.服务众多，客户端如何访问。

​	2.服务众多，服务之间如何通信。

​	3.服务众多，如何治理。

​	4.服务众多，如果挂了怎么办；

这四个问题对应了解决四个问题的方式：

​	1.API网关，服务路由

​	2.Http,RPC,异步调用

​	3.服务注册与发现 -》高可用

​	4.熔断，限流，服务降级

#### 解决方案：

​		SpringCloud,spring Cloud是一套生态，是为了解决微服务架构遇到的问题，想要使用Spring Cloud必须基于Spring Boot

**1.Spring Cloud Netflix**

​		API网关，zuul组件

​		服务注册与发现，Eureka

​		Fegin -> http Client -->http通信方式，同步阻塞

   	 熔断机制 Hystrix

**2.Apache Dubbo Zookeeper**

   	Dubbo是一个高效性能的 Java RPC 通信框架，2.6.x

   	服务注册与发现，Zookeeper，

   	API网关 没有  找第三方或自己实现。

  	服务挂了，Hystrix

**3.Spring Cloud Alibaba**

 		Spring Cloud Alibaba致力于提供微服务开发的一站式解决方案。此项目包含开发分布式应用微服务的必需组件，方便开发者通过 Spring Cloud 编程模型轻松使用这些组件来开发分布式应用服务。

依托 Spring Cloud Alibaba，您只需要添加一些注解和少量配置，就可以将 Spring Cloud 应用接入阿里微服务解决方案，通过阿里中间件来迅速搭建分布式应用系统。

下一代会是什么呢，Service Mesh 服务网格化，Istio 可能是需要掌握的一套微服务解决方案。



### SpringCloud Netflix:

   	到2019目前最流行的微服务架构解决方案是：springBoot+spring cloud Netflix

   	Spring Cloud 为开发人员提供了快速构建分布式系统中一些常见模式的工具（例如配置管理，服务发现，断路器，智能路由，微代理，控制总线）。分布式系统的协调导致了样板模式, 使用 Spring Cloud 开发人员可以快速地支持实现这些模式的服务和应用程序。他们将在任何分布式环境中运行良好，包括开发人员自己的笔记本电脑，裸机数据中心，以及 Cloud Foundry 等托管平台。

​    	Spring Cloud 是基于Spring Boot进行开发，并且都是使用 Maven 做项目管理工具。   然而在2019年Spring Cloud Netflix 开始进入维护模式。所以使用的少了。      

#### 创建一个依赖管理项目：    

```xml
<?xml version="1.0" encoding="UTF-8"?>
        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
            <modelVersion>4.0.0</modelVersion>

            <parent>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-parent</artifactId>
                <version>2.0.2.RELEASE</version>
            </parent>

            <groupId>com.funtl</groupId>
            <artifactId>hello-spring-cloud-dependencies</artifactId>
            <version>1.0.0-SNAPSHOT</version>
            <packaging>pom</packaging>

            <name>hello-spring-cloud-dependencies</name>
            <url>http://www.funtl.com</url>
            <inceptionYear>2018-Now</inceptionYear>

            <properties>
                <!-- Environment Settings -->
                <java.version>1.8</java.version>
                <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
                <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>

                <!-- Spring Settings -->
                <spring-cloud.version>Finchley.RC1</spring-cloud.version>
            </properties>

            <dependencyManagement>
                <dependencies>
                    <dependency>
                        <groupId>org.springframework.cloud</groupId>
                        <artifactId>spring-cloud-dependencies</artifactId>
                        <version>${spring-cloud.version}</version>
                        <type>pom</type>
                        <scope>import</scope>
                    </dependency>
                </dependencies>
            </dependencyManagement>

            <build>
                <plugins>
                    <!-- Compiler 插件, 设定 JDK 版本 -->
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-compiler-plugin</artifactId>
                        <configuration>
                            <showWarnings>true</showWarnings>
                        </configuration>
                    </plugin>

                    <!-- 打包 jar 文件时，配置 manifest 文件，加入 lib 包的 jar 依赖 -->
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-jar-plugin</artifactId>
                        <configuration>
                            <archive>
                                <addMavenDescriptor>false</addMavenDescriptor>
                            </archive>
                        </configuration>
                        <executions>
                            <execution>
                                <configuration>
                                    <archive>
                                        <manifest>
                                            <!-- Add directory entries -->
                                            <addDefaultImplementationEntries>true</addDefaultImplementationEntries>
                                            <addDefaultSpecificationEntries>true</addDefaultSpecificationEntries>
                                            <addClasspath>true</addClasspath>
                                        </manifest>
                                    </archive>
                                </configuration>
                            </execution>
                        </executions>
                    </plugin>

                    <!-- resource -->
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-resources-plugin</artifactId>
                    </plugin>

                    <!-- install -->
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-install-plugin</artifactId>
                    </plugin>

                    <!-- clean -->
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-clean-plugin</artifactId>
                    </plugin>

                    <!-- ant -->
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-antrun-plugin</artifactId>
                    </plugin>

                    <!-- dependency -->
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-dependency-plugin</artifactId>
                    </plugin>
                </plugins>

                <pluginManagement>
                    <plugins>
                        <!-- Java Document Generate -->
                        <plugin>
                            <groupId>org.apache.maven.plugins</groupId>
                            <artifactId>maven-javadoc-plugin</artifactId>
                            <executions>
                                <execution>
                                    <phase>prepare-package</phase>
                                    <goals>
                                        <goal>jar</goal>
                                    </goals>
                                </execution>
                            </executions>
                        </plugin>

                        <!-- YUI Compressor (CSS/JS压缩) -->
                        <plugin>
                            <groupId>net.alchim31.maven</groupId>
                            <artifactId>yuicompressor-maven-plugin</artifactId>
                            <version>1.5.1</version>
                            <executions>
                                <execution>
                                    <phase>prepare-package</phase>
                                    <goals>
                                        <goal>compress</goal>
                                    </goals>
                                </execution>
                            </executions>
                            <configuration>
                                <encoding>UTF-8</encoding>
                                <jswarn>false</jswarn>
                                <nosuffix>true</nosuffix>
                                <linebreakpos>30000</linebreakpos>
                                <force>true</force>
                                <includes>
                                    <include>/*.js</include>
                                    <include>/.css</include>
                                </includes>
                                <excludes>
                                    <exclude>**/.min.js</exclude>
                                    <exclude>*/.min.css</exclude>
                                </excludes>
                            </configuration>
                        </plugin>
                    </plugins>
                </pluginManagement>

                <!-- 资源文件配置 -->
                <resources>
                    <resource>
                        <directory>src/main/java</directory>
                        <excludes>
                            <exclude>*/.java</exclude>
                        </excludes>
                    </resource>
                    <resource>
                        <directory>src/main/resources</directory>
                    </resource>
                </resources>
            </build>

            <repositories>
                <repository>
                    <id>aliyun-repos</id>
                    <name>Aliyun Repository</name>
                    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>

                <repository>
                    <id>sonatype-repos</id>
                    <name>Sonatype Repository</name>
                    <url>https://oss.sonatype.org/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
                <repository>
                    <id>sonatype-repos-s</id>
                    <name>Sonatype Repository</name>
                    <url>https://oss.sonatype.org/content/repositories/snapshots</url>
                    <releases>
                        <enabled>false</enabled>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                    </snapshots>
                </repository>

                <repository>
                    <id>spring-snapshots</id>
                    <name>Spring Snapshots</name>
                    <url>https://repo.spring.io/snapshot</url>
                    <snapshots>
                        <enabled>true</enabled>
                    </snapshots>
                </repository>
                <repository>
                    <id>spring-milestones</id>
                    <name>Spring Milestones</name>
                    <url>https://repo.spring.io/milestone</url>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
            </repositories>

            <pluginRepositories>
                <pluginRepository>
                    <id>aliyun-repos</id>
                    <name>Aliyun Repository</name>
                    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </project>

```

 #### 创建服务注册与发现：

- 用的是Eureak是一个服务注册和发现模块（与zookeeper的作用是一样的）
- 引入配置文件pom.xml
- 启动一个服务注册中心：需要注解@EnableEureakaServcer
-  配置application.yml文件：每一个实例注册之后需要向注册中心发送心跳（因此可以在内存中完成），在默认情况下 Erureka Server 也是一个 Eureka Client ,必须要指定一个 Server。
- 通过服务可以打开浏览器进行访问http://localhost:8761

#### 创建服务提供者：

​		当 Client 向 Server 注册时，它会提供一些元数据，例如主机和端口，URL，主页等。Eureka Server 从每个 Client 实例接收心跳消息。 如果心跳超时，则通常将该实例从注册 Server 中删除

**引入pom.xm文件：**



**application中的yml**

```yaml
spring:
  application:
    name: hello-spring-cloud-service-admin

server:
  port: 8762

eureka:
  client:
     serviceUrl:
        defaultZone: http://localhost:8761/eureka/   

```

#### 创建服务消费者Ribbon：

```xml
 pom.xml：
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.funtl</groupId>
        <artifactId>hello-spring-cloud-dependencies</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <relativePath>../hello-spring-cloud-dependencies/pom.xml</relativePath>
    </parent>

    <artifactId>hello-spring-cloud-web-admin-ribbon</artifactId>
    <packaging>jar</packaging>

    <name>hello-spring-cloud-web-admin-ribbon</name>
    <url>http://www.funtl.com</url>
    <inceptionYear>2018-Now</inceptionYear>

    <dependencies>
        <!-- Spring Boot Begin -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <!-- Spring Boot End -->

        <!-- Spring Cloud Begin -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
        </dependency>
        <!-- Spring Cloud End -->

        <!-- 解决 thymeleaf 模板引擎一定要执行严格的 html5 格式校验问题 -->
        <dependency>
            <groupId>net.sourceforge.nekohtml</groupId>
            <artifactId>nekohtml</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <mainClass>com.funtl.hello.spring.cloud.web.admin.ribbon.WebAdminRibbonApplication</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```

##### 在application.yml中书写配置文件：

```
spring:
  application:
    name: hello-spring-cloud-service-admin

server:
  port: 8762

eureka:
  client:
     serviceUrl:
        defaultZone: http://localhost:8761/eureka/   

```

​    创建服务feign:feign是集成了ribbon的一个服务消费者：项目中使用用Feign  可以理解为将ribbon峰会在哪个了一次

#### 熔断器防止服务雪崩：

​    	在微服务架构中，根据业务来拆分成一个个的服务，服务与服务之间可以通过 RPC 相互调用，在 Spring Cloud 中可以用 RestTemplate + Ribbon 和 Feign 来调用。为了保证其高可用，单个服务通常会集群部署。

​    	由于网络原因或者自身的原因，服务并不能保证 100% 可用，如果单个服务出现问题，调用这个服务就会出现线程阻塞，此时若有大量的请求涌入，Servlet 容器的线程资源会被消耗完毕，导致服务瘫痪。服务与服务之间的依赖性，故障会传播，会对整个微服务系统造成灾难性的严重后果，这就是服务故障的 “雪崩” 效应。

##### 1.在feign中使用：        

```java
Feign中自带熔断器：
    feign:
        hystrix:
            enabled: true
=====
service中指定fallback的类：
 @FeignClient(value = "hello-spring-cloud-service-admin", fallback = AdminServiceHystrix.class)
public interface AdminService {
   @RequestMapping(value = "hi", method = RequestMethod.GET)
   public String sayHi(@RequestParam(value = "message") String message);
 }
====
创建这个借口的实现类：
@Component
 public class AdminServiceHystrix implements AdminService {
 	@Override
 	public String sayHi(String message) {
   	 	return "Hi，your message is :"" + message + "" but request error.";
   	}
}     
```

##### 2.在ribbon中使用：

 **`pom.xml中加入：`**

```XML
<dependency>
 <groupId>org.springframework.cloud</groupId>
 <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
```

​        主函数中加入注解@EnableHystrix
**​在service中使用：**

```java
@HystrixCommand(fallbackMethod = "hiError")
public String sayHi(String message) {
    return restTemplate.getForObject("http://HELLO-SPRING-CLOUD-SERVICE-ADMIN/hi?message=" + message, String.class);
}

public String hiError(String message) {
    return "Hi，your message is :"" + message + "" but request error.";
}

```

##### 使用熔断仪器监控hystrix：

**`pom.xml中加入：`**

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
</dependency>
```

```java
@Configuration
public class HystrixDashboardConfiguration {

    @Bean
    public ServletRegistrationBean getServlet() {
        HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
        registrationBean.setLoadOnStartup(1);
        registrationBean.addUrlMappings("/hystrix.stream");
        registrationBean.setName("HystrixMetricsStreamServlet");
        return registrationBean;
    }
}

```

​    测试熔断localhost:8764/hystrix

##### hystrix触发fallback的方法：、

​        	timeout,SHORT_CIRCUITED:断路器打开不尝试执行，THREAD_POOL_REJECTED：线程池拒绝，不尝试执行，SEMAPHORE_REJECTED	信号量拒绝，不尝试执行	YES

#### 使用统一的网关访问接口zuul：

​    		在 Spring Cloud 微服务系统中，一种常见的负载均衡方式是，客户端的请求首先经过负载均衡（Zuul、Ngnix），再到达服务网关（Zuul 集群），然后再到具体的服。 服务统一注册到高可用的服务注册中心集群，服务的所有的配置文件由配置服务管理，配置服务的配置文件放在 GIT 仓库，方便开发人员随时改配置    

##### zuul：

​		 Zuul 的主要功能是路由转发和过滤器。路由功能是微服务的一部分，比如 /api/user 转发到到 User 服务，/api/shop 转发到到 Shop 服务。Zuul 默认和 Ribbon 结合实现了负载均衡的功能。

######     **pom.xml中加入：** 

```XML
<dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
 </dependency>
```

######     application.yml中书写配置文件：

```yml
spring:
  application:
    name: hello-spring-cloud-zuul

  server:
  port: 8769

  eureka:
  client:
    serviceUrl:
    defaultZone: http://localhost:8761/eureka/
  ##以/api/a/请求全部转发到ribbon服务中，以/api/b/全部转发到feign服务##
  zuul:
  routes:
    api-a:
    path: /api/a/**
    serviceId: hello-spring-cloud-web-admin-ribbon
    api-b:
    path: /api/b/**
    serviceId: hello-spring-cloud-web-admin-feign
```

​    创建回调的类：主要是创建对对其出现错误后的回调机制：

######     主函数：

```java
@SpringBootApplication
@EnableEurekaClient
@EnableZuulProxy
public class ZuulApplication {
    public static void main(String[] args) {
    	SpringApplication.run(ZuulApplication.class, args);
    }
}  
```

​    其实就是聚合其他的服务：以上聚合了ribbon和feign两个服务。
​        

#### 分布式配置中心：

​         有分布式配置中心组件 Spring Cloud Config ，它支持配置服务放在配置服务的内存中（即本地），也支持放在远程 Git 仓库中。在 Spring Cloud Config 组件中，分两个角色，一是 Config Server，二是 Config Client。

##### 分布式配置中心服务端：

###### pom.xml中加入：


```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.funtl</groupId>
        <artifactId>hello-spring-cloud-dependencies</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <relativePath>../hello-spring-cloud-dependencies/pom.xml</relativePath>
    </parent>

    <artifactId>hello-spring-cloud-config</artifactId>
    <packaging>jar</packaging>

    <name>hello-spring-cloud-config</name>
    <url>http://www.funtl.com</url>
    <inceptionYear>2018-Now</inceptionYear>

    <dependencies>
        <!-- Spring Boot Begin -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <!-- Spring Boot End -->

        <!-- Spring Cloud Begin -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
        <!-- Spring Cloud End -->
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <mainClass>com.funtl.hello.spring.cloud.config.ConfigApplication</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project> 

```

 在ConfigApplication中开启：

```java
@SpringBootApplication
@EnableConfigServer
@EnableEurekaClient
public class ConfigApplication {
   public static void main(String[] args) {
       SpringApplication.run(ConfigApplication.class, args);
  }
} 
```


###### 在application.yml中书写配置文件：

```yaml
spring:
  application:
    name: hello-spring-cloud-config
  cloud:
    config:
    label: master
    server:
      git:
      uri: https://github.com/topsale/spring-cloud-config
      search-paths: respo
      username:
      password:

  server:
  port: 8888

  eureka:
  client:
    serviceUrl:
    defaultZone: http://localhost:8761/eureka/

```

###### 配置说明：

​           `spring.cloud.config.label：配置仓库的分支`

​           `spring.cloud.config.server.git.uri：配置 Git 仓库地址（GitHub、GitLab、码云 ...）
`​  

​          `spring.cloud.config.server.git.search-paths：配置仓库路径（存放配置文件的目录）`

​           `spring.cloud.config.server.git.username：访问 Git 仓库的账号`

​           `spring.cloud.config.server.git.password：访问 Git 仓库的密码`

##### 分布式配置中心客户端配置：

###### pom.xml中配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.funtl</groupId>
        <artifactId>hello-spring-cloud-dependencies</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <relativePath>../hello-spring-cloud-dependencies/pom.xml</relativePath>
    </parent>

    <artifactId>hello-spring-cloud-config-client</artifactId>
    <packaging>jar</packaging>

    <name>hello-spring-cloud-config-client</name>
    <url>http://www.funtl.com</url>
    <inceptionYear>2018-Now</inceptionYear>

    <dependencies>
        <!-- Spring Boot Begin -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <!-- Spring Boot End -->

        <!-- Spring Cloud Begin -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
        <!-- Spring Cloud End -->
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <mainClass>com.funtl.hello.spring.cloud.config.client.ConfigClientApplication</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```

   入口类处理方式：
​    

```java
@SpringBootApplication
@EnableDiscoveryClient
public class ConfigClientApplication {
   public static void main(String[] args) {
          SpringApplication.run(ConfigClientApplication.class, args);
   }
}  
```

###### application.yml中书写配置文件： 

```yaml
spring:
  application:
    name: hello-spring-cloud-config-client
  cloud:
    config:
    uri: http://localhost:8888
    name: config-client
    label: master
    profile: dev

  server:
  port: 8889

  eureka:
  client:
    serviceUrl:
    defaultZone: http://localhost:8761/eureka/

```

  ###### 配置说明：  

>    spring.cloud.config.uri：配置服务中心的网址
> ​   spring.cloud.config.name：配置文件名称的前缀
> ​   spring.cloud.config.label：配置仓库的分支
> ​   spring.cloud.config.profile：配置文件的环境标识
> ​        dev：表示开发环境
> ​        test：表示测试环境
> ​         prod：表示生产环境  

#### 服务链路追踪：

 **`这里主要使用ZipKin：`
**​         每个服务向 ZipKin 报告计时数据，ZipKin 会根据调用关系通过 ZipKin UI 生成依赖关系图，显示了多少跟踪请求通过每个服务，该系统让开发者可通过一个 Web 前端轻松的收集和分析数据，例如用户每次请求服务的处理时间等，可方便的监测系统中存在的瓶颈 。

**​    `说明：`**

​         微服务架构是通过业务来划分服务的，使用 REST 调用。对外暴露的一个接口，可能需要很多个服务协同才能完成这个接口功能，如果链路上任何一个服务出现问题或者网络超时，都会形成导致接口调用失败。随着业务的不断扩张，服务之间互相调用会越来越复杂。

#### 服务监控：

​        admin
​    启动顺序：
​        config->eureka--->--->zipkin--->Admin--->serviceAdmin--->ZuulApplication   

### 各组件深入之Spring Cloud Eureka

通常来说服务注册与发现包括两部分，一个是服务器端，另一个是客户端。Server是一个公共服务，为Client提供服务注册和发现的功能，维护注册到自身的Client的相关信息，同时提供接口给Client获取注册表中其他服务的信息，使得动态变化的Client能够进行服务间的相互调用。Client将自己的服务信息通过一定的方式登记到Server上，并在正常范围内维护自己信息一致性，方便其他服务发现自己，同时可以通过Server获取到自己依赖的其他服务信息，完成服务调用

Eureka是一项基于REST（代表性状态转移）的服务，主要在AWS云中用于查找服务，以实现负载均衡和中间层服务器的故障转移。 我们称此服务为Eureka服务器服务注册与发现。 Eureka还带有一个基于Java的客户端组件Eureka Client，它使与服务的交互变得更加容易。 客户端还具有一个内置的负载平衡器，可以执行基本的循环负载平衡。 Netflix使用更复杂的负载均衡器将Eureka包装起来，以基于流量，资源使用，错误条件等多种因素提供加权负载均衡，以提供出色的弹性.



原理：主管服务注册与发现，也就是微服务的名称注册到Eureka，就可以通过Eureka找到微服务，而不需要修改服务调用的配置文件。



分析：Spring Cloud封装了Netflix公司开发的Eureka模块来实现服务的注册与发现，采用的c-s的设计架构，Eureka Server作为服务注册功能的服务器，他是服务注册中心。而系统的其他微服务，使用Eureka的客户端连接到Eureka Server并维持心跳。这样系统的维护人员可以通过Eureka Server来监控系统中的各个微服务是否正常运行。Spring Cloud的一些其他模块（比如Zuul）就可以通过Eureka Server来发现系统其他的微服务，并执行相关逻辑。

#### **Eureka Server**

Eureka Server提供服务注册服务，各个节点启动后，会在Eureka Server中进行注册， 这样Eureka Server中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观的看到。

Eureka Server既可以独立部署，也可以集群部署。在集群部署的情况下，Eureka Server间会进行注册表信息同步的操作，这时被同步注册表信息的Eureka Server将会被其他同步注册表信息的Eureka Server称为peer.

通常来讲，一个Eureka Server也是一个Eureka Client，它会尝试注册自己，所以需要至少一个注册中心的URL来定位对等点peer。如果不提供这样一个注册端点，注册中心也能工作，但是会在日志中打印无法向peer注册自己的信息。在独立（Standalone）Eureka Server的模式下，Eureka Server一般会关闭作为客户端注册自己的行为。Eureka Server与Eureka Client之间的联系主要通过心跳的方式实现。心跳（Heartbeat）即Eureka Client定时向Eureka Server汇报本服务实例当前的状态，维护本服务实例在注册表中租约的有效性。

Eureka Server需要随时维持最新的服务实例信息，所以在注册表中的每个服务实例都需要定期发送心跳到Server中以使自己的注册保持最新的状态（数据一般直接保存在内存中）。为了避免Eureka Client在每次服务间调用都向注册中心请求依赖服务实例的信息，Eureka Client将定时从Eureka Server中拉取注册表中的信息，并将这些信息缓存到本地，用于服务发现

#### **Eureka Client**

Eureka Client是一个Java客户端， 用于简化Eureka Server的交互，客户端同时也具备一个内置的、 使用轮询（round-robin）负载算法的负载均衡器。在应用启动后，将会向Eureka Server发送心跳（默认周期为30秒），以证明当前服务是可用状态 (30秒发送一次心跳更新租约。 如果客户端几次无法续签租约)。 如果Eureka Server在一定的时间（默认90秒）未收到客户端的心跳，Eureka Server将会从服务注册表中把这个服务节点移除。 任何区域的客户端都可以查找注册表信息（每30秒发生一次）以查找其服务（可能在任何区域）并进行远程调用。

DiscoveryClient来源于spring-cloud-client-discovery，是Spring Cloud中定义用来服务发现的公共接口，在Spring Cloud的各类服务发现组件中（如NetflixEureka或Consul）都有相应的实现。它提供从服务注册中心根据serviceId获取到对应服务实例信息的能力。当一个服务实例拥有DiscoveryClient的具体实现时，就可以从服务注册中心中发现其他的服务实例。在Eureka Client中注入DiscoveryClient，并从Eureka Server获取服务实例的信息



#### **Eureka Server的自我保护机制**

如果在15分钟内超过85%的节点都没有正常的心跳，那么Eureka就认为客户端与注册中心出现了网络故障，此时会出现以下几种情况：

- Eureka不再从注册列表中移除因为长时间没收到心跳而应该过期的服务
- Eureka仍然能够接受新服务的注册和查询请求，但是不会被同步到其它节点上（即保证当前节点依然可用）
- 当网络稳定时，当前实例新的注册信息会被同步到其它节点中

因此， Eureka可以很好的应对因网络故障导致部分节点失去联系的情况，而不会像ZooKeeper那样使整个注册服务瘫痪。



#### 服务发现原理

Eureka最初设计的目的是AWS（亚马逊网络服务系统）中用于部署分布式系统，所以首先对AWS上的区域（Regin）和可用区（Availability Zone）进行简单的介绍。

区域：AWS根据地理位置把某个地区的基础设施服务集合称为一个区域，区域之间相对独立。在架构图上，us-east-1c、us-east-1d、us-east-1e表示AWS中的三个设施服务区域，这些区域中分别部署了一个Eureka集群。

可用区：AWS的每个区域都是由多个可用区组成的，而一个可用区一般都是由多个数据中心（简单理解成一个原子服务设施）组成的。可用区与可用区之间是相互独立的，有独立的网络和供电等，保证了应用程序的高可用性。在上述的架构图中，一个可用区中可能部署了多个Eureka，一个区域中有多个可用区，这些Eureka共同组成了一个Eureka集群。

[![cmmBB8.png](https://z3.ax1x.com/2021/04/02/cmmBB8.png)](https://imgtu.com/i/cmmBB8)

□ Application Service：是一个Eureka Client，扮演服务提供者的角色，提供业务服务，向Eureka Server注册和更新自己的信息，同时能从Eureka Server注册表中获取到其他服务的信息。

□ Eureka Server：扮演服务注册中心的角色，提供服务注册和发现的功能。每个Eureka Cient向Eureka Server注册自己的信息，也可以通过EurekaServer获取到其他服务的信息达到发现和调用其他服务的目的。

□ Application Client：是一个Eureka Client，扮演了服务消费者的角色，通过Eureka Server获取注册到其上其他服务的信息，从而根据信息找到所需的服务发起远程调用。

□ Replicate:Eureka Server之间注册表信息的同步复制，使Eureka Server集群中不同注册表中服务实例信息保持一致。

□ Make Remote Call：服务之间的远程调用。

□ Register：注册服务实例，Client端向Server端注册自身的元数据以供服务发现。

□ Renew：续约，通过发送心跳到Server以维持和更新注册表中服务实例元数据的有效性。当在一定时长内，Server没有收到Client的心跳信息，将默认服务下线，会把服务实例的信息从注册表中删除。

□ Cancel：服务下线，Client在关闭时主动向Server注销服务实例元数据，这时Client的服务实例数据将从Server的注册表中删除。

□ Get Registry：获取注册表，Client向Server请求注册表信息，用于服务发现，从而发起服务间远程调用。

#### Eureka Client 源码解析

Eureka Client为了简化开发人员的开发工作，将很多与Eureka Server交互的工作隐藏起来，自主完成。主要包含以下职能：

- 读取配置信息
- 服务发现客户端
- 拉取注册表信息
- 服务注册
- 初始化定时任务
- 服务下线

[![cw6icD.png](https://z3.ax1x.com/2021/04/11/cw6icD.png)](https://imgtu.com/i/cw6icD)

Eukeka Client通过Starter的方式引入依赖，Spring Boot将会为项目使用以下的自动配置类：

□ EurekaClientAutoConfiguration:Eureke Client自动配置类，负责EurekaClient中关键Beans的配置和初始化，如ApplicationInfoManager和EurekaClientConfig等。

□ RibbonEurekaAutoConfiguration:Ribbon负载均衡相关配置。

□ EurekaDiscoveryClientConfiguration：配置自动注册和应用的健康检查器。

##### 读取自身配置信息

通过EurekaDiscoveryClientConfiguration配置类，Spring Boot帮助EurekaClient完成很多必要Bean的属性读取和配置，下表列出了EurekaDiscoveryClientConfiguration中的属性读取和配置类

[![cwcFP0.png](https://z3.ax1x.com/2021/04/11/cwcFP0.png)](https://imgtu.com/i/cwcFP0)

Spring Cloud中的服务发现客户端DiscoveryClient进行进一步的介绍，它是客户端进行服务发现的核心接口.DiscoveryClient是Spring Cloud中用来进行服务发现的顶级接口，在Netflix Eureka或者Consul中都有相应的具体实现类

```java
/**
 *
 * Netflix Eureka or consul.io
 DiscoveryClient是Spring Cloud中用来进行服务发现的顶级接口，在Netflix Eureka或者Consul中都有相应的具体实现类
 * @author Spencer Gibb
 */
public interface DiscoveryClient {

	/**
	 * 获取实现类的描述
	 * @return the description
	 */
	String description();

	/**
	 * 通过服务Id获取服务实例的信息
	 * @param serviceId the serviceId to query
	 * @return a List of ServiceInstance
	 */
	List<ServiceInstance> getInstances(String serviceId);

	/**
	 * 获取所有服务实例Id
	 * @return all known service ids
	 */
	List<String> getServices();

}
```

##### 服务发现客户端

DiscoveryClient是Eureka Client的核心类，包括与Eureka Server交互的关键逻辑具备以下职能：

- 注册服务实例到Eureka Server中；
- 发送心跳更新与Eureka Server的租约；
- 在服务关闭时从Eureka Server中取消租约，服务下线；
-  查询在Eureka Server中注册的服务实例列表

EurekaDiscoveryClient实现了DiscoveryClient接口，但是通过查看EurekaDiscoveryClient中代码，会发现它是通过组合EurekaClien类实现接口的功能，如下为getInstance方法的实现：

[![cwRyQK.png](https://z3.ax1x.com/2021/04/11/cwRyQK.png)](https://imgtu.com/i/cwRyQK)

```java
public class EurekaDiscoveryClient implements DiscoveryClient {

	public static final String DESCRIPTION = "Spring Cloud Eureka Discovery Client";

	private final EurekaInstanceConfig config;

	private final EurekaClient eurekaClient;

	public EurekaDiscoveryClient(EurekaInstanceConfig config, EurekaClient eurekaClient) {
		this.config = config;
		this.eurekaClient = eurekaClient;
	}

	@Override
	public String description() {
		return DESCRIPTION;
	}

	@Override
	public List<ServiceInstance> getInstances(String serviceId) {
		List<InstanceInfo> infos = this.eurekaClient.getInstancesByVipAddress(serviceId,
				false);
		List<ServiceInstance> instances = new ArrayList<>();
		for (InstanceInfo info : infos) {
			instances.add(new EurekaServiceInstance(info));
		}
		return instances;
	}

	public static class EurekaServiceInstance implements ServiceInstance {
		private InstanceInfo instance;

		public EurekaServiceInstance(InstanceInfo instance) {
			Assert.notNull(instance, "Service instance required");
			this.instance = instance;
		}

		public InstanceInfo getInstanceInfo() {
			return instance;
		}

		@Override
		public String getServiceId() {
			return this.instance.getAppName();
		}

		@Override
		public String getHost() {
			return this.instance.getHostName();
		}

		@Override
		public int getPort() {
			if (isSecure()) {
				return this.instance.getSecurePort();
			}
			return this.instance.getPort();
		}

		@Override
		public boolean isSecure() {
			// assume if secure is enabled, that is the default
			return this.instance.isPortEnabled(SECURE);
		}

		@Override
		public URI getUri() {
			return DefaultServiceInstance.getUri(this);
		}

		@Override
		public Map<String, String> getMetadata() {
			return this.instance.getMetadata();
		}
	}

	@Override
	public List<String> getServices() {
		Applications applications = this.eurekaClient.getApplications();
		if (applications == null) {
			return Collections.emptyList();
		}
		List<Application> registered = applications.getRegisteredApplications();
		List<String> names = new ArrayList<>();
		for (Application app : registered) {
			if (app.getInstances().isEmpty()) {
				continue;
			}
			names.add(app.getName().toLowerCase());

		}
		return names;
	}

}
```

```java
package com.netflix.discovery;

@ImplementedBy(DiscoveryClient.class)
public interface EurekaClient extends LookupService {
    Applications getApplicationsForARegion(@Nullable String var1);
	
    // 返回当前注册表中所有的服务实例信息
    Applications getApplications(String var1);
	// 
    List<InstanceInfo> getInstancesByVipAddress(String var1, boolean var2);

    List<InstanceInfo> getInstancesByVipAddress(String var1, boolean var2, @Nullable String var3);

    List<InstanceInfo> getInstancesByVipAddressAndAppName(String var1, String var2, boolean var3);

    Set<String> getAllKnownRegions();

    InstanceStatus getInstanceRemoteStatus();

    /** @deprecated */
    @Deprecated
    List<String> getDiscoveryServiceUrls(String var1);

    /** @deprecated */
    @Deprecated
    List<String> getServiceUrlsFromConfig(String var1, boolean var2);

    /** @deprecated */
    @Deprecated
    List<String> getServiceUrlsFromDNS(String var1, boolean var2);

    /** @deprecated */
    @Deprecated
    void registerHealthCheckCallback(HealthCheckCallback var1);
	// 为Eureka Client注册一个健康检查处理器
    void registerHealthCheck(HealthCheckHandler var1);
	// 为Euraka Client 注册一个事件监听器，监听client服务的信息更新
    void registerEventListener(EurekaEventListener var1);

    boolean unregisterEventListener(EurekaEventListener var1);

    HealthCheckHandler getHealthCheckHandler();

    void shutdown();

    EurekaClientConfig getEurekaClientConfig();

    ApplicationInfoManager getApplicationInfoManager();
}
```

Application持有服务实例信息列表，它可以理解成同一个服务的集群信息，这些服务实例都挂在同一个服务名appName下。InstanceInfo代表一个服务实例信息。Application部分代码如下：

```java
@Serializer("com.netflix.discovery.converters.EntityBodyConverter")
@XStreamAlias("application")
@JsonRootName("application")
public class Application {
    
    private static Random shuffleRandom = new Random();

    private String name;

    @XStreamOmitField
    private volatile boolean isDirty = false;

    @XStreamImplicit
    private final Set<InstanceInfo> instances;

    private final AtomicReference<List<InstanceInfo>> shuffledInstances;

    private final Map<String, InstanceInfo> instancesMap;

    public Application() {
        instances = new LinkedHashSet<InstanceInfo>();
        instancesMap = new ConcurrentHashMap<String, InstanceInfo>();
        shuffledInstances = new AtomicReference<List<InstanceInfo>>();
    }

    public Application(String name) {
        this();
        this.name = StringCache.intern(name);
    }

    @JsonCreator
    public Application(
            @JsonProperty("name") String name,
            @JsonProperty("instance") List<InstanceInfo> instances) {
        this(name);
        for (InstanceInfo instanceInfo : instances) {
            addInstance(instanceInfo);
        }
    }

```

为了保证原子性操作，Application中对InstanceInfo的操作都是同步操作。Applications是注册表中所有服务实例信息的集合，里面的操作大多也是同步操作

Eureka Server一般通过心跳（heartbeats）来识别一个实例的状态。Eureka Client中存在一个定时任务定时通过HealthCheckHandler检测当前Client的状态，如果Client的状态发生改变，将会触发新的注册事件，更新Eureka Server的注册表中该服务实例的相关信息。



EurekaClient来自于com.netflix.discovery包中，其默认实现为com.netflix.discovery. DiscoveryClient，属于eureka-client的源代码，它提供了Eureka Client注册到Server上、续租、下线以及获取Server中注册表信息等诸多关键功能。

- 提供了多种方式获取InstanceInfo，例如根据区域、EurekaServer地址等获取。

- 提供了本地客户端（所处的区域、可用区等）的数据，这部分与AWS密切相关。

- 提供了为客户端注册和获取健康检查处理器的能力

**DiscoveryClient 实现了EurekaClient接口，**

```java
@Singleton
public class DiscoveryClient implements EurekaClient {
    private static final Logger logger = LoggerFactory.getLogger(DiscoveryClient.class);

    // Constants
    public static final String HTTP_X_DISCOVERY_ALLOW_REDIRECT = "X-Discovery-AllowRedirect";

    private static final String VALUE_DELIMITER = ",";
    private static final String COMMA_STRING = VALUE_DELIMITER;

    /**
     * @deprecated here for legacy support as the client config has moved to be an instance variable
     */
    @Deprecated
    private static EurekaClientConfig staticClientConfig;

    // Timers
    private static final String PREFIX = "DiscoveryClient_";
    private final Counter RECONCILE_HASH_CODES_MISMATCH = Monitors.newCounter(PREFIX + "ReconcileHashCodeMismatch");
    private final com.netflix.servo.monitor.Timer FETCH_REGISTRY_TIMER = Monitors
            .newTimer(PREFIX + "FetchRegistry");
    private final Counter REREGISTER_COUNTER = Monitors.newCounter(PREFIX
            + "Reregister");
}
```

**DiscoveryClient 构造函数：**
在DiscoveryClient构造函数中，Eureka Client会执行从EurekaServer中拉取注册表信息、服务注册、初始化发送心跳、缓存刷新（重新拉取注册表信息）和按需注册定时任务等操作，可以说DiscoveryClient的构造函数贯穿了Eureka Client启动阶段的各项工作。DiscoveryClient的构造函数传入的参数如下所示

```java
@Inject
    DiscoveryClient(ApplicationInfoManager applicationInfoManager, EurekaClientConfig config, AbstractDiscoveryClientOptionalArgs args,
                    Provider<BackupRegistry> backupRegistryProvider) {
        if (args != null) {
            this.healthCheckHandlerProvider = args.healthCheckHandlerProvider;
            this.healthCheckCallbackProvider = args.healthCheckCallbackProvider;
            this.eventListeners.addAll(args.getEventListeners());
            this.preRegistrationHandler = args.preRegistrationHandler;
        } else {
            this.healthCheckCallbackProvider = null;
            this.healthCheckHandlerProvider = null;
            this.preRegistrationHandler = null;
        }
        
        this.applicationInfoManager = applicationInfoManager;
        InstanceInfo myInfo = applicationInfoManager.getInfo();

        clientConfig = config;
        staticClientConfig = clientConfig;
        transportConfig = config.getTransportConfig();
        instanceInfo = myInfo;
        if (myInfo != null) {
            appPathIdentifier = instanceInfo.getAppName() + "/" + instanceInfo.getId();
        } else {
            logger.warn("Setting instanceInfo to a passed in null value");
        }

        this.backupRegistryProvider = backupRegistryProvider;

        this.urlRandomizer = new EndpointUtils.InstanceInfoBasedUrlRandomizer(instanceInfo);
        localRegionApps.set(new Applications());

        fetchRegistryGeneration = new AtomicLong(0);

        remoteRegionsToFetch = new AtomicReference<String>(clientConfig.fetchRegistryForRemoteRegions());
        remoteRegionsRef = new AtomicReference<>(remoteRegionsToFetch.get() == null ? null : remoteRegionsToFetch.get().split(","));

        if (config.shouldFetchRegistry()) {
            this.registryStalenessMonitor = new ThresholdLevelsMetric(this, METRIC_REGISTRY_PREFIX + "lastUpdateSec_", new long[]{15L, 30L, 60L, 120L, 240L, 480L});
        } else {
            this.registryStalenessMonitor = ThresholdLevelsMetric.NO_OP_METRIC;
        }

        if (config.shouldRegisterWithEureka()) {
            this.heartbeatStalenessMonitor = new ThresholdLevelsMetric(this, METRIC_REGISTRATION_PREFIX + "lastHeartbeatSec_", new long[]{15L, 30L, 60L, 120L, 240L, 480L});
        } else {
            this.heartbeatStalenessMonitor = ThresholdLevelsMetric.NO_OP_METRIC;
        }

        logger.info("Initializing Eureka in region {}", clientConfig.getRegion());

        if (!config.shouldRegisterWithEureka() && !config.shouldFetchRegistry()) {
            logger.info("Client configured to neither register nor query for data.");
            scheduler = null;
            heartbeatExecutor = null;
            cacheRefreshExecutor = null;
            eurekaTransport = null;
            instanceRegionChecker = new InstanceRegionChecker(new PropertyBasedAzToRegionMapper(config), clientConfig.getRegion());

            // This is a bit of hack to allow for existing code using DiscoveryManager.getInstance()
            // to work with DI'd DiscoveryClient
            DiscoveryManager.getInstance().setDiscoveryClient(this);
            DiscoveryManager.getInstance().setEurekaClientConfig(config);

            initTimestampMs = System.currentTimeMillis();
            logger.info("Discovery Client initialized at timestamp {} with initial instances count: {}",
                    initTimestampMs, this.getApplications().size());

            return;  // no need to setup up an network tasks and we are done
        }

        try {
            // default size of 2 - 1 each for heartbeat and cacheRefresh
            // 定义一个基于线程池的定时器线程池ScheduledExecutorService，线程池大小为2，一个线程用于发送心跳，另一个线程用于缓存刷新，同时定义了发送心跳和缓存刷新线程池
            scheduler = Executors.newScheduledThreadPool(2,
                    new ThreadFactoryBuilder()
                            .setNameFormat("DiscoveryClient-%d")
                            .setDaemon(true)
                            .build());

            heartbeatExecutor = new ThreadPoolExecutor(
                    1, clientConfig.getHeartbeatExecutorThreadPoolSize(), 0, TimeUnit.SECONDS,
                    new SynchronousQueue<Runnable>(),
                    new ThreadFactoryBuilder()
                            .setNameFormat("DiscoveryClient-HeartbeatExecutor-%d")
                            .setDaemon(true)
                            .build()
            );  // use direct handoff

            cacheRefreshExecutor = new ThreadPoolExecutor(
                    1, clientConfig.getCacheRefreshExecutorThreadPoolSize(), 0, TimeUnit.SECONDS,
                    new SynchronousQueue<Runnable>(),
                    new ThreadFactoryBuilder()
                            .setNameFormat("DiscoveryClient-CacheRefreshExecutor-%d")
                            .setDaemon(true)
                            .build()
            );  // use direct handoff
			// 初始化Eureka Client与Eureka Server进行HTTP交互的Jersey客户端，将AbstractDiscoveryClientOptionalArgs中的属性用来构建EurekaTransport
            // EurekaTransport是DiscoveryClient中的一个内部类，其内封装了DiscoveryClient与Eureka Server进行HTTP调用的Jersey客户端
            eurekaTransport = new EurekaTransport();
            scheduleServerEndpointTask(eurekaTransport, args);

            AzToRegionMapper azToRegionMapper;
            if (clientConfig.shouldUseDnsForFetchingServiceUrls()) {
                azToRegionMapper = new DNSBasedAzToRegionMapper(clientConfig);
            } else {
                azToRegionMapper = new PropertyBasedAzToRegionMapper(clientConfig);
            }
            if (null != remoteRegionsToFetch.get()) {
                azToRegionMapper.setRegionsToFetch(remoteRegionsToFetch.get().split(","));
            }
            instanceRegionChecker = new InstanceRegionChecker(azToRegionMapper, clientConfig.getRegion());
        } catch (Throwable e) {
            throw new RuntimeException("Failed to initialize DiscoveryClient!", e);
        }
		// 从Eureka Server中(fetchRegistry方法)拉取注册表信息 下面为true时
        // 在Eureka Client向EurekaServer注册前，需要先从Eureka Server拉取注册表中的信息，这是服务发现的前提。通过将Eureka Server中的注册表信息缓存到本地，就可以就近获取其他服务的相关信息，减少与Eureka Server的网络通信
        if (clientConfig.shouldFetchRegistry() && !fetchRegistry(false)) {
            fetchRegistryFromBackup();
        }
		// 拉取完Eureka Server中的注册表信息后，将对服务实例进行注册
        // call and execute the pre registration handler before all background tasks (inc registration) is started
        if (this.preRegistrationHandler != null) {
            this.preRegistrationHandler.beforeRegistration();
        }
		
        if (clientConfig.shouldRegisterWithEureka() && clientConfig.shouldEnforceRegistrationAtInit()) {
            try {
                // 发起注册服务
                if (!register() ) {
                    // 注册失败抛出异常
                    throw new IllegalStateException("Registration error at startup. Invalid server response.");
                }
            } catch (Throwable th) {
                logger.error("Registration error at startup: {}", th.getMessage());
                throw new IllegalStateException(th);
            }
        }
		// 初始化任务定时器
        // finally, init the schedule tasks (e.g. cluster resolvers, heartbeat, instanceInfo replicator, fetch
        initScheduledTasks();

        try {
            Monitors.registerObject(this);
        } catch (Throwable e) {
            logger.warn("Cannot register timers", e);
        }

        // This is a bit of hack to allow for existing code using DiscoveryManager.getInstance()
        // to work with DI'd DiscoveryClient
        DiscoveryManager.getInstance().setDiscoveryClient(this);
        DiscoveryManager.getInstance().setEurekaClientConfig(config);

        initTimestampMs = System.currentTimeMillis();
        logger.info("Discovery Client initialized at timestamp {} with initial instances count: {}",
                initTimestampMs, this.getApplications().size());
    }
```

**最后总结一下，在DiscoveryClient的构造函数中，主要依次做了以下的事情：**

1）相关配置的赋值，类似ApplicationInfoManager、EurekaClientConfig等。

2）备份注册中心的初始化，默认没有实现。

3）拉取Eureka Server注册表中的信息。

4）注册前的预处理。

5）向Eureka Server注册自身。

6）初始化心跳定时任务、缓存刷新和按需注册等定时任务。

##### 拉取注册表信息

在DiscoveryClient的构造函数中，调用了DiscoveryClient.fetchRegistry方法从Eureka Server中拉取注册表信息

```java
private boolean fetchRegistry(boolean forceFullRegistryFetch) {
        Stopwatch tracer = FETCH_REGISTRY_TIMER.start();

        try {
            // If the delta is disabled or if it is the first time, get all
            // applications
            // 如果禁用增量，或者applications为null 这是第一次，获取所有
            Applications applications = getApplications();

            if (clientConfig.shouldDisableDelta()
                    || (!Strings.isNullOrEmpty(clientConfig.getRegistryRefreshSingleVipAddress()))
                    || forceFullRegistryFetch
                    || (applications == null)
                    || (applications.getRegisteredApplications().size() == 0)
                    || (applications.getVersion() == -1)) //Client application does not have latest library supporting delta
            {
                // 省略log信息
                // 全量拉取注册表信息
                getAndStoreFullRegistry();
            } else {
                // 增量拉取注册表信息
                getAndUpdateDelta(applications);
            }
          // 计算应用集合一致性哈希码
            applications.setAppsHashCode(applications.getReconcileHashCode());
            // 打印注册表上的所有的实例
            logTotalInstances();
        } catch (Throwable e) {
            logger.error(PREFIX + "{} - was unable to refresh its cache! status = {}", appPathIdentifier, e.getMessage(), e);
            return false;
        } finally {
            if (tracer != null) {
                tracer.stop();
            }
        }

        // Notify about cache refresh before updating the instance remote status
    	// 在更新实例远程状态之前通知有关缓存刷新的信息
        onCacheRefreshed();

        // Update remote status based on refreshed data held in the cache	
    	// 根据缓存中保存的刷新数据更新远程状态
        updateInstanceRemoteStatus();

        // registry was fetched successfully, so return true
    	// 注册表已成功拉取，返回true
        return true;
    }
```

一般来讲，在Eureka客户端，除了第一次拉取注册表信息，之后的信息拉取都会尝试只进行增量拉取（第一次拉取注册表信息为全量拉取），下面将分别介绍拉取注册表信息的两种实现getAndStoreFullRegistry() 和 getAndUpdateDelta(applications)

###### 全量拉去注册表信息

一般只有在第一次拉取的时候，才会进行注册表信息的全量拉取，主要在DiscoveryClient.getAndStoreFullRegistry方法中进行

```java
private void getAndStoreFullRegistry() throws Throwable {
    // 获取拉取注册表的版本，防止拉取版本落后    
    long currentUpdateGeneration = fetchRegistryGeneration.get();
	
        logger.info("Getting all instance registry info from the eureka server");

        Applications apps = null;
        EurekaHttpResponse<Applications> httpResponse = clientConfig.getRegistryRefreshSingleVipAddress() == null
                ? eurekaTransport.queryClient.getApplications(remoteRegionsRef.get())
                : eurekaTransport.queryClient.getVip(clientConfig.getRegistryRefreshSingleVipAddress(), remoteRegionsRef.get());
    // 获取成功
        if (httpResponse.getStatusCode() == Status.OK.getStatusCode()) {
            apps = httpResponse.getEntity();
        }
        logger.info("The response status is {}", httpResponse.getStatusCode());

        if (apps == null) {
            logger.error("The application is null for some reason. Not storing this information");
        } else if (fetchRegistryGeneration.compareAndSet(currentUpdateGeneration, currentUpdateGeneration + 1)) {
            // 从apps中筛选出状态为UP的实例，同时打乱实例的顺序，防止同一服务的不同实例在启动时接受流量
            localRegionApps.set(this.filterAndShuffle(apps));
            logger.debug("Got full registry with apps hashcode {}", apps.getAppsHashCode());
        } else {
            logger.warn("Not updating applications as another thread is updating it already");
        }
    }
```

全量拉取将从Eureka Server中拉取注册表中所有的服务实例信息（封装在Applications中），并经过处理后替换掉本地注册表缓存Applications

getAndStoreFullRegistry方法可能被多个线程同时调用，导致新拉取的注册表被旧的注册表覆盖（有可能出现先拉取注册表信息的线程在覆盖apps时被阻塞，被后拉取注册表信息的线程抢先设置了apps，被阻塞的线程恢复后再次设置了apps，导致apps数据版本落后），产生脏数据，对此，Eureka通过类型为AtomicLong的currentUpdateGeneration对apps的更新版本进行跟踪。如果更新版本不一致，说明本次拉取注册表信息已过时，不需要缓存到本地。拉取到注册表信息之后会对获取到的apps进行筛选，只保留状态为UP的服务实例信息

###### 增量拉取注册表信息

增量式的拉取方式，一般发生在第一次拉取注册表信息之后，拉取的信息定义为从某一段时间之后发生的所有变更信息，通常来讲是3分钟之内注册表的信息变化。在获取到更新的delta后，会根据delta中的增量更新对本地的数据进行更新。与getAndStoreFullRegistry方法一样，也通过fetchRegistryGeneration对更新的版本进行控制。增量式拉取是为了维护Eureka Client本地的注册表信息与Eureka Server注册表信息的一致性，防止数据过久而失效，采用增量式拉取的方式减少了拉取注册表信息的通信量。Client中有一个注册表缓存刷新定时器专门负责维护两者之间信息的同步性。但是当增量式拉取出现意外时，定时器将执行全量拉取以更新本地缓存的注册表信息。具体代码如下所示。

```java
private void getAndUpdateDelta(Applications applications) throws Throwable {
        long currentUpdateGeneration = fetchRegistryGeneration.get();

        Applications delta = null;
        EurekaHttpResponse<Applications> httpResponse = eurekaTransport.queryClient.getDelta(remoteRegionsRef.get());
        if (httpResponse.getStatusCode() == Status.OK.getStatusCode()) {
            delta = httpResponse.getEntity();
        }
		// 获取增量拉取失败
        if (delta == null) {
            logger.warn("The server does not allow the delta revision to be applied because it is not safe. "
                    + "Hence got the full registry.");
            // 进行全量拉取
            getAndStoreFullRegistry();
        } else if (fetchRegistryGeneration.compareAndSet(currentUpdateGeneration, currentUpdateGeneration + 1)) {
            logger.debug("Got delta update with apps hashcode {}", delta.getAppsHashCode());
            String reconcileHashCode = "";
            if (fetchRegistryUpdateLock.tryLock()) {
                try {
                    // 更新本地缓存
                    updateDelta(delta);
                    // 计算应用集合一致性哈希表
                    reconcileHashCode = getReconcileHashCode(applications);
                } finally {
                    fetchRegistryUpdateLock.unlock();
                }
            } else {
                logger.warn("Cannot acquire update lock, aborting getAndUpdateDelta");
            }
            // There is a diff in number of instances for some reason
            // 比较应用集合一致性哈希吗，如果不一致将认为本次增量式拉取数据胰脏，将发起全量拉取更新本地注册表信息
            if (!reconcileHashCode.equals(delta.getAppsHashCode()) || clientConfig.shouldLogDeltaDiff()) {
                reconcileAndLogDifference(delta, reconcileHashCode);  // this makes a remoteCall
            }
        } else {
            logger.warn("Not updating application delta as another thread is updating it already");
            logger.debug("Ignoring delta update with apps hashcode {}, as another thread is updating it already", delta.getAppsHashCode());
        }
    }
```

 更新本地缓存**updateDelta(delta);**

```java
private void updateDelta(Applications delta) {
        int deltaCount = 0;
        for (Application app : delta.getRegisteredApplications()) {
            for (InstanceInfo instance : app.getInstances()) {
                Applications applications = getApplications();
                String instanceRegion = instanceRegionChecker.getInstanceRegion(instance);
                if (!instanceRegionChecker.isLocalRegion(instanceRegion)) {
                    Applications remoteApps = remoteRegionVsApps.get(instanceRegion);
                    if (null == remoteApps) {
                        remoteApps = new Applications();
                        remoteRegionVsApps.put(instanceRegion, remoteApps);
                    }
                    applications = remoteApps;
                }

                ++deltaCount;
                // 变更类型为ADDED
                if (ActionType.ADDED.equals(instance.getActionType())) {
                    Application existingApp = applications.getRegisteredApplications(instance.getAppName());
                    if (existingApp == null) {
                        applications.addApplication(app);
                    }
                    logger.debug("Added instance {} to the existing apps in region {}", instance.getId(), instanceRegion);
 // 添加到本地注册表中
                 applications.getRegisteredApplications(instance.getAppName()).addInstance(instance);
                } else if (ActionType.MODIFIED.equals(instance.getActionType())) {
                    // 变更类型为 MODIFIED
                    Application existingApp = applications.getRegisteredApplications(instance.getAppName());
                    if (existingApp == null) {
                        applications.addApplication(app);
                    }
                    logger.debug("Modified instance {} to the existing apps ", instance.getId());

 // 添加到本地注册表中
                    applications.getRegisteredApplications(instance.getAppName()).addInstance(instance);

                } else if (ActionType.DELETED.equals(instance.getActionType())) {
                    // 变更类型为 DELETED
                    Application existingApp = applications.getRegisteredApplications(instance.getAppName());
                    if (existingApp == null) {
                        applications.addApplication(app);
                    }
                    logger.debug("Deleted instance {} to the existing apps ", instance.getId());
   // 从注册表中删除
                    applications.getRegisteredApplications(instance.getAppName()).removeInstance(instance);
                }
            }
        }
        logger.debug("The total number of instances fetched by the delta processor : {}", deltaCount);

        getApplications().setVersion(delta.getVersion());
        getApplications().shuffleInstances(clientConfig.shouldFilterOnlyUpInstances());

        for (Applications applications : remoteRegionVsApps.values()) {
            applications.setVersion(delta.getVersion());
            applications.shuffleInstances(clientConfig.shouldFilterOnlyUpInstances());
        }
    }
```

更新本地注册表缓存之后，Eureka Client会通过getReconcileHashCode计算合并后的Applications的appsHashCode（应用集合一致性哈希码），和Eureka Server传递的delta上的appsHashCode进行比较（delta中携带的appsHashCode通过Eureka Server的全量注册表计算得出），比对客户端和服务端上注册表的差异。如果哈希值不一致的话将再调用一次getAndStoreFullRegistry获取全量数据保证EurekaClient与Eureka Server之间注册表数据的一致

计算hash一致性 getReconcileHashCode()

```java
private String getReconcileHashCode(Applications applications) {
        TreeMap<String, AtomicInteger> instanceCountMap = new TreeMap<String, AtomicInteger>();
        if (isFetchingRemoteRegionRegistries()) {
            for (Applications remoteApp : remoteRegionVsApps.values()) {
                remoteApp.populateInstanceCountMap(instanceCountMap);
            }
        }
        applications.populateInstanceCountMap(instanceCountMap);
        return Applications.getReconcileHashCode(instanceCountMap);
    }
```

##### 服务注册

在拉取完Eureka Server中的注册表信息并将其缓存在本地后，Eureka Client将向Eureka Server注册自身服务实例元数据，主要逻辑位于DiscoveryClent.register()方法中。register方法代码如下所示：

```java
// 通过进行适当的REST调用来注册eureka服务
boolean register() throws Throwable {
        logger.info(PREFIX + "{}: registering service...", appPathIdentifier);
        EurekaHttpResponse<Void> httpResponse;
        try {
            httpResponse = eurekaTransport.registrationClient.register(instanceInfo);
        } catch (Exception e) {
            logger.warn(PREFIX + "{} - registration failed {}", appPathIdentifier, e.getMessage(), e);
            throw e;
        }
        if (logger.isInfoEnabled()) {
            logger.info(PREFIX + "{} - registration status: {}", appPathIdentifier, httpResponse.getStatusCode());
        }
        return httpResponse.getStatusCode() == 204;
    }
```

Eureka Client会将自身服务实例元数据（封装在InstanceInfo中）发送到Eureka Server中请求服务注册，当Eureka Server返回204状态码时，说明服务注册成功.

##### 初始化定时任务

很明显，服务注册应该是一个持续的过程，Eureka Client通过定时发送心跳的方式与Eureka Server进行通信，维持自己在Server注册表上的租约。同时Eureka Server注册表中的服务实例信息是动态变化的，为了保持Eureka Client与Eureka Server的注册表信息的一致性，Eureka Client需要定时向EurekaServer拉取注册表信息并更新本地缓存。为了监控EurekaClient应用信息和状态的变化，Eureka Client设置了一个按需注册定时器，定时检查应用信息或者状态的变化，并在发生变化时向Eureka Server重新注册，避免注册表中的本服务实例信息不可用。在DiscoveryClient.initScheduledTasks方法中初始化了三个定时器任务，一个用于向Eureka Server拉取注册表信息刷新本地缓存；一个用于向Eureka Server发送心跳；一个用于进行按需注册的操作。

```java
private void initScheduledTasks() {
        if (clientConfig.shouldFetchRegistry()) {
            // registry cache refresh timer
            // 注册表缓存刷新定时器，获取配置文件中刷新间隔，默认为30秒，可通过eureka.client.registry-fetch-interval-seconds 进行设置
            int registryFetchIntervalSeconds = clientConfig.getRegistryFetchIntervalSeconds();
            int expBackOffBound = clientConfig.getCacheRefreshExecutorExponentialBackOffBound();
            scheduler.schedule(
                    new TimedSupervisorTask(
                            "cacheRefresh",
                            scheduler,
                            cacheRefreshExecutor,
                            registryFetchIntervalSeconds,
                            TimeUnit.SECONDS,
                            expBackOffBound,
                            new CacheRefreshThread()
                    ),
                    registryFetchIntervalSeconds, TimeUnit.SECONDS);
        }

        if (clientConfig.shouldRegisterWithEureka()) {
            // 发送心跳定时器，默认30秒发送一次。
            /**
            * 通过InstanceInfo类中的获取LeaseInfo类
            * private volatile LeaseInfo leaseInfo;
            *
            *	@JsonRootName("leaseInfo")
                public class LeaseInfo {

                    public static final int DEFAULT_LEASE_RENEWAL_INTERVAL = 30;
                    public static final int DEFAULT_LEASE_DURATION = 90;

                    // Client settings
                    private int renewalIntervalInSecs = DEFAULT_LEASE_RENEWAL_INTERVAL;
                    private int durationInSecs = DEFAULT_LEASE_DURATION;
                    }
            */
            int renewalIntervalInSecs = instanceInfo.getLeaseInfo().getRenewalIntervalInSecs();
            int expBackOffBound = clientConfig.getHeartbeatExecutorExponentialBackOffBound();
            logger.info("Starting heartbeat executor: " + "renew interval is: {}", renewalIntervalInSecs);
			// 心跳定时器
            // Heartbeat timer
            scheduler.schedule(
                    new TimedSupervisorTask(
                            "heartbeat",
                            scheduler,
                            heartbeatExecutor,
                            renewalIntervalInSecs,
                            TimeUnit.SECONDS,
                            expBackOffBound,
                            new HeartbeatThread()
                    ),
                    renewalIntervalInSecs, TimeUnit.SECONDS);
			// 按需注册定时器
            // InstanceInfo replicator
            instanceInfoReplicator = new InstanceInfoReplicator(
                    this,
                    instanceInfo,
                    clientConfig.getInstanceInfoReplicationIntervalSeconds(),
                    2); // burstSize
	// 监控应用的status变化，发生变化即可发起重新注册	
            statusChangeListener = new ApplicationInfoManager.StatusChangeListener() {
                @Override
                public String getId() {
                    return "statusChangeListener";
                }

                @Override
                public void notify(StatusChangeEvent statusChangeEvent) {
                    if (InstanceStatus.DOWN == statusChangeEvent.getStatus() ||
                            InstanceStatus.DOWN == statusChangeEvent.getPreviousStatus()) {
                        // log at warn level if DOWN was involved
                        logger.warn("Saw local status change event {}", statusChangeEvent);
                    } else {
                        logger.info("Saw local status change event {}", statusChangeEvent);
                    }
                    instanceInfoReplicator.onDemandUpdate();
                }
            };

            if (clientConfig.shouldOnDemandUpdateStatusChange()) {
  //注册应用状态改变监控器
                applicationInfoManager.registerStatusChangeListener(statusChangeListener);
            }

// 启动定时按需注册定时任务
            instanceInfoReplicator.start(clientConfig.getInitialInstanceInfoReplicationIntervalSeconds());
        } else {
            logger.info("Not registering with Eureka server per configuration");
        }
    }
```

######  缓存刷新定时任务与发送心跳定时任务

在DiscoveryClient .initScheduledTasks()方法中，通过ScheduledExecutorService.schedule（）的方式提交缓存刷新任务和发送心跳任务，任务执行的方式为延时执行并且不循环，这两个任务的定时循环逻辑由TimedSupervisorTask提供实现。TimedSupervisorTask继承了TimerTask，提供执行定时任务的功能。它在run方法中定义执行定时任务的逻辑。具体代码如下所示

```java
public class TimedSupervisorTask extends TimerTask {
    private static final Logger logger = LoggerFactory.getLogger(TimedSupervisorTask.class);

    private final Counter timeoutCounter;
    private final Counter rejectedCounter;
    private final Counter throwableCounter;
    private final LongGauge threadPoolLevelGauge;

    private final ScheduledExecutorService scheduler;
    private final ThreadPoolExecutor executor;
    private final long timeoutMillis;
    private final Runnable task;

    private final AtomicLong delay;
    private final long maxDelay;

    public TimedSupervisorTask(String name, ScheduledExecutorService scheduler, ThreadPoolExecutor executor,
                               int timeout, TimeUnit timeUnit, int expBackOffBound, Runnable task) {
        this.scheduler = scheduler;
        this.executor = executor;
        this.timeoutMillis = timeUnit.toMillis(timeout);
        this.task = task;
        this.delay = new AtomicLong(timeoutMillis);
        this.maxDelay = timeoutMillis * expBackOffBound;

        // Initialize the counters and register.
        timeoutCounter = Monitors.newCounter("timeouts");
        rejectedCounter = Monitors.newCounter("rejectedExecutions");
        throwableCounter = Monitors.newCounter("throwables");
        threadPoolLevelGauge = new LongGauge(MonitorConfig.builder("threadPoolUsed").build());
        Monitors.registerObject(name, this);
    }

    @Override
    public void run() {
        Future<?> future = null;
        try {
            // 执行任务
            future = executor.submit(task);
            threadPoolLevelGauge.set((long) executor.getActiveCount());
            // 等待任务执行结果
            future.get(timeoutMillis, TimeUnit.MILLISECONDS);  // block until done or timeout
            // 执行完成设置下次时间间隔
            delay.set(timeoutMillis);
            threadPoolLevelGauge.set((long) executor.getActiveCount());
        } catch (TimeoutException e) {
            logger.warn("task supervisor timed out", e);
            // 任务超时
            timeoutCounter.increment();

            long currentDelay = delay.get();
            long newDelay = Math.min(maxDelay, currentDelay * 2);
            delay.compareAndSet(currentDelay, newDelay);

        } catch (RejectedExecutionException e) {
            if (executor.isShutdown() || scheduler.isShutdown()) {
                logger.warn("task supervisor shutting down, reject the task", e);
            } else {
                logger.warn("task supervisor rejected the task", e);
            }
			// 执行任务被拒绝，统计拒绝次数
            rejectedCounter.increment();
        } catch (Throwable e) {
            if (executor.isShutdown() || scheduler.isShutdown()) {
                logger.warn("task supervisor shutting down, can't accept the task");
            } else {
                logger.warn("task supervisor threw an exception", e);
            }

            throwableCounter.increment();
        } finally {
            // 取消未结束的任务
            if (future != null) {
                future.cancel(true);
            }
			// 如果定时任务服务未关闭，定义下一次任务
            if (!scheduler.isShutdown()) {
                scheduler.schedule(this, delay.get(), TimeUnit.MILLISECONDS);
            }
        }
    }
}
```

run方法中存在以下的任务调度过程：

1）scheduler初始化并延迟执行TimedSupervisorTask；

2）TimedSupervisorTask将task提交executor中执行，task和executor在初始化TimedSupervisorTask时传入：

□ 若task正常执行，TimedSupervisorTask将自己提交到scheduler，延迟delay时间后再次执行；

□ 若task执行超时，计算新的delay时间（不超过maxDelay）,TimedSupervisorTask将自己提交到scheduler，延迟delay时间后再次执行；

[![cwLRjH.png](https://z3.ax1x.com/2021/04/11/cwLRjH.png)](https://imgtu.com/i/cwLRjH)

TimedSupervisorTask通过这种不断循环提交任务的方式，完成定时执行任务的要求

###### 按需注册定时任务

按需注册定时任务的作用是当Eureka Client中的InstanceInfo或者status发生变化时，重新向Eureka Server发起注册请求，更新注册表中的服务实例信息，保证Eureka Server注册表中服务实例信息有效和可用。**initScheduledTasks()中分离出的部分代码**

```java
			// 按需注册定时器
            // InstanceInfo replicator
            instanceInfoReplicator = new InstanceInfoReplicator(
                    this,
                    instanceInfo,
                    clientConfig.getInstanceInfoReplicationIntervalSeconds(),
                    2); // burstSize
	// 监控应用的status变化，发生变化即可发起重新注册	
            statusChangeListener = new ApplicationInfoManager.StatusChangeListener() {
                @Override
                public String getId() {
                    return "statusChangeListener";
                }

                @Override
                public void notify(StatusChangeEvent statusChangeEvent) {
                    if (InstanceStatus.DOWN == statusChangeEvent.getStatus() ||
                            InstanceStatus.DOWN == statusChangeEvent.getPreviousStatus()) {
                        // log at warn level if DOWN was involved
                        logger.warn("Saw local status change event {}", statusChangeEvent);
                    } else {
                        logger.info("Saw local status change event {}", statusChangeEvent);
                    }
                    instanceInfoReplicator.onDemandUpdate();
                }
            };

            if (clientConfig.shouldOnDemandUpdateStatusChange()) {
  //注册应用状态改变监控器
                applicationInfoManager.registerStatusChangeListener(statusChangeListener);
            }

// 启动定时按需注册定时任务
            instanceInfoReplicator.start(clientConfig.getInitialInstanceInfoReplicationIntervalSeconds());
        } else {
            logger.info("Not registering with Eureka server per configuration");
        }
```

按需注册分为两部分，一部分是定义了一个定时任务，定时刷新服务实例的信息和检查应用状态的变化，在服务实例信息发生改变的情况下向Eureka Server重新发起注册操作；另一部分是注册了状态改变监控器，在应用状态发生变化时，刷新服务实例信息，在服务实例信息发生改变的情况下向Eureka Server重新发起注册操作

```java
/**
*用于将本地instanceinfo更新和复制到远程服务器的任务。 此任务的属性是：-配置有单个更新线程以确保对远程服务器进行顺序更新-可以通过onDemandUpdate（）按需计划更新任务-任务处理的速率受burstSize限制-始终计划新的更新任务自动执行较早的更新任务后。 但是，如果启动了按需任务，则计划的自动更新任务将被丢弃（并且在新的按需更新之后将安排新的任务）
*/
class InstanceInfoReplicator implements Runnable {
    private static final Logger logger = LoggerFactory.getLogger(InstanceInfoReplicator.class);

    private final DiscoveryClient discoveryClient;
    private final InstanceInfo instanceInfo;

    private final int replicationIntervalSeconds;
    private final ScheduledExecutorService scheduler;
    private final AtomicReference<Future> scheduledPeriodicRef;

    private final AtomicBoolean started;
    private final RateLimiter rateLimiter;
    private final int burstSize;
    private final int allowedRatePerMinute;

    InstanceInfoReplicator(DiscoveryClient discoveryClient, InstanceInfo instanceInfo, int replicationIntervalSeconds, int burstSize) {
        this.discoveryClient = discoveryClient;
        this.instanceInfo = instanceInfo;
        this.scheduler = Executors.newScheduledThreadPool(1,
                new ThreadFactoryBuilder()
                        .setNameFormat("DiscoveryClient-InstanceInfoReplicator-%d")
                        .setDaemon(true)
                        .build());

        this.scheduledPeriodicRef = new AtomicReference<Future>();

        this.started = new AtomicBoolean(false);
        this.rateLimiter = new RateLimiter(TimeUnit.MINUTES);
        this.replicationIntervalSeconds = replicationIntervalSeconds;
        this.burstSize = burstSize;

        this.allowedRatePerMinute = 60 * this.burstSize / this.replicationIntervalSeconds;
        logger.info("InstanceInfoReplicator onDemand update allowed rate per min is {}", allowedRatePerMinute);
    }

    public void start(int initialDelayMs) {
        if (started.compareAndSet(false, true)) {
            instanceInfo.setIsDirty();  // for initial register
            Future next = scheduler.schedule(this, initialDelayMs, TimeUnit.SECONDS);
            scheduledPeriodicRef.set(next);
        }
    }

    public void stop() {
        shutdownAndAwaitTermination(scheduler);
        started.set(false);
    }

    private void shutdownAndAwaitTermination(ExecutorService pool) {
        pool.shutdown();
        try {
            if (!pool.awaitTermination(3, TimeUnit.SECONDS)) {
                pool.shutdownNow();
            }
        } catch (InterruptedException e) {
            logger.warn("InstanceInfoReplicator stop interrupted");
        }
    }

    public boolean onDemandUpdate() {
        if (rateLimiter.acquire(burstSize, allowedRatePerMinute)) {
            if (!scheduler.isShutdown()) {
                scheduler.submit(new Runnable() {
                    @Override
                    public void run() {
                        logger.debug("Executing on-demand update of local InstanceInfo");
    
                        Future latestPeriodic = scheduledPeriodicRef.get();
                        if (latestPeriodic != null && !latestPeriodic.isDone()) {
                            logger.debug("Canceling the latest scheduled update, it will be rescheduled at the end of on demand update");
                            latestPeriodic.cancel(false);
                        }
    
                        InstanceInfoReplicator.this.run();
                    }
                });
                return true;
            } else {
                logger.warn("Ignoring onDemand update due to stopped scheduler");
                return false;
            }
        } else {
            logger.warn("Ignoring onDemand update due to rate limiter");
            return false;
        }
    }

    public void run() {
        try {
            
            // 刷新了InstanceInfo中的服务实例信息
            discoveryClient.refreshInstanceInfo();
	// 如果数据发生改变，则返回数据更新时间
            Long dirtyTimestamp = instanceInfo.isDirtyWithTime();
            if (dirtyTimestamp != null) {
                // 注册服务实例
                discoveryClient.register();
                // 重置更新状态
                instanceInfo.unsetIsDirty(dirtyTimestamp);
            }
        } catch (Throwable t) {
            logger.warn("There was a problem with the instance info replicator", t);
        } finally {
            // 执行下一个延时任务
            Future next = scheduler.schedule(this, replicationIntervalSeconds, TimeUnit.SECONDS);
            scheduledPeriodicRef.set(next);
        }
    }

}

```

DiscoveryClient中刷新本地服务实例信息和检查服务状态变化的代码如下refreshInstanceInfo()方法：

```java
void refreshInstanceInfo() {
    // 刷新服务实例信息
    applicationInfoManager.refreshDataCenterInfoIfRequired();
    // 更新租约信息    
    applicationInfoManager.refreshLeaseInfoIfRequired();

        InstanceStatus status;
        try {
            // 检查服务实例的变化
            status = getHealthCheckHandler().getStatus(instanceInfo.getStatus());
        } catch (Exception e) {
            logger.warn("Exception from healthcheckHandler.getStatus, setting status to DOWN", e);
            status = InstanceStatus.DOWN;
        }

        if (null != status) {
            applicationInfoManager.setInstanceStatus(status);
        }
    }
```

run方法首先调用了discoveryClient#refreshInstanceInfo方法刷新当前的服务实例信息，查看当前服务实例信息和服务状态是否发生变化，如果当前服务实例信息或者服务状态发生变化将向Eureka Server重新发起服务注册操作。最后声明了下一个延时任务，用于再次调用run方法，继续检查服务实例信息和服务状态的变化，在服务实例信息发生变化的情况下重新发起注册。如果Eureka Client的状态发生变化（在Spring Boot通过Actuator对服务状态进行监控，具体实现为EurekaHealthCheckHandler），注册在ApplicationInfoManager的状态改变监控器将会被触发，从而调用InstanceInfoReplicator#onDemandUpdate方法，检查服务实例信息和服务状态的变化，可能会引发按需注册任务。代码如下所示

```java
// InstanceInfoReplicator#onDemandUpdate方法
public boolean onDemandUpdate() {
    // 控制流量，当超过限制时，不能进行按需更新
        if (rateLimiter.acquire(burstSize, allowedRatePerMinute)) {
            if (!scheduler.isShutdown()) {
                scheduler.submit(new Runnable() {
                    @Override
                    public void run() {
                        logger.debug("Executing on-demand update of local InstanceInfo");
    
                        Future latestPeriodic = scheduledPeriodicRef.get();
                        // 取消上次run任务
                        if (latestPeriodic != null && !latestPeriodic.isDone()) {
                            logger.debug("Canceling the latest scheduled update, it will be rescheduled at the end of on demand update");
                            latestPeriodic.cancel(false);
                        }
    
                        InstanceInfoReplicator.this.run();
                    }
                });
                return true;
            } else {
                logger.warn("Ignoring onDemand update due to stopped scheduler");
                return false;
            }
        } else {
            logger.warn("Ignoring onDemand update due to rate limiter");
            return false;
        }
    }
```

InstanceInfoReplicator#onDemandUpdate方法调用InstanceInfoReplicator#run方法检查服务实例信息和服务状态的变化，并在服务实例信息发生变化的情况下向Eureka Server发起重新注册的请求。为了防止重复执行run方法，onDemandUpdate方法还会取消执行上次已提交且未完成的run方法，执行最新的按需注册任务



[![cwxueH.png](https://z3.ax1x.com/2021/04/11/cwxueH.png)](https://imgtu.com/i/cwxueH)

##### 服务下线

一般情况下，应用服务在关闭的时候，Eureka Client会主动向Eureka Server注销自身在注册表中的信息。DiscoveryClient中对象销毁前执行的清理方法

```java
@PreDestroy
    @Override
    public synchronized void shutdown() {
        if (isShutdown.compareAndSet(false, true)) {
            logger.info("Shutting down DiscoveryClient ...");
		// 原子操作，确保只会执行一次
            if (statusChangeListener != null && applicationInfoManager != null) {
 // 注销状态监听器
                applicationInfoManager.unregisterStatusChangeListener(statusChangeListener.getId());
            }
// 取消定时任务
            cancelScheduledTasks();

            // If APPINFO was registered
            if (applicationInfoManager != null
                    && clientConfig.shouldRegisterWithEureka()
                    && clientConfig.shouldUnregisterOnShutdown()) {
 // 服务下线
               applicationInfoManager.setInstanceStatus(InstanceStatus.DOWN);
                unregister();
            }
// 关闭Jesry客户端
            if (eurekaTransport != null) {
                eurekaTransport.shutdown();
            }
            // 关闭相关的监视器
            heartbeatStalenessMonitor.shutdown();
            registryStalenessMonitor.shutdown();

            logger.info("Completed shut down of DiscoveryClient");
        }
    }
```

#### Eureka Server源码解析

Eureka Server作为一个开箱即用的服务注册中心，提供了以下的功能，用以满足与Eureka Client交互的需求：

-  服务注册
- 接受服务心跳
- 服务剔除 
- 服务下线
- 集群同步 
- 获取注册表中服务实例信息

需要注意的是，Eureka Server同时也是一个Eureka Client，在不禁止EurekaServer的客户端行为时，它会向它配置文件中的其他Eureka Server进行拉取注册表、服务注册和发送心跳等操作

##### 服务实例注册表

InstanceRegistry是Eureka Server中注册表管理的核心接口。它的类结构如下图所示。图中出现了两个InstanceRegistry，最下面的InstanceRegistry对EurekaServer的注册表实现类PeerAwareInstanceRegistryImpl进行了继承和扩展，使其适配Spring Cloud的使用环境，主要实现由PeerAwareInstanceRegistryImpl提供

[![c0e5lQ.png](https://z3.ax1x.com/2021/04/11/c0e5lQ.png)](https://imgtu.com/i/c0e5lQ)

上层的InstanceRegistry是Eureka Server注册表的最核心接口，其职责是在内存中管理注册到Eureka Server中的服务实例信息。InstanceRegistry分别继承了LeaseManager和LookupService接口。LeaseManager接口的功能是对注册到Eureka Server中的服务实例租约进行管理。而LookupService提供对服务实例进行检索的功能，在Eureka Client的源码解析中已进行介绍，在此不对其接口进行展示。LeaseManager接口提供的方法代码如下所示

```java
public interface LeaseManager<T> {

    void register(T r, int leaseDuration, boolean isReplication);

    boolean cancel(String appName, String id, boolean isReplication);

    boolean renew(String appName, String id, boolean isReplication);

    void evict();
}
```

LeaseManager接口作用是对注册到Eureka Server中的服务实例租约进行管理，分别有服务注册、服务下线、服务租约更新以及服务剔除等操作。

LeaseManager中管理的对象是Lease, Lease代表一个Eureka Client服务实例信息的租约，它提供了对其内持有的类的时间有效性操作。Lease持有的类是代表服务实例信息的InstanceInfo。Lease中定义了租约的操作类型，分别是注册、下线、更新，同时提供了对租约中时间属性的各项操作。租约默认有效时长（duration）为90秒。InstanceRegistry在继承LeaseManager和LookupService接口的基础上，还添加了一些特有的方法，可以更为简单地管理服务实例租约和查询注册表中的服务实例信息。可以通过AbstractInstanceRegistry查看InstanceRegistry接口方法的具体实现。PeerAwareInstanceRegistry继承了InstanceRegistry接口，在其基础上添加了Eureka Server集群同步的操作，其实现类PeerAwareInstanceRegistryImpl继承了AbstractInstanceRegistry的实现，在对本地注册表操作的基础上添加了对其peer节点的同步复制操作，使得Eureka Server集群中的注册表信息保持一致

##### 服务注册

Eureka Client在发起服务注册时会将自身的服务实例元数据封装在InstanceInfo中，然后将InstanceInfo发送到Eureka Server。Eureka Server在接收到Eureka Client发送的InstanceInfo后将会尝试将其放到本地注册表中以供其他Eureka Client进行服务发现。**服务注册的主要实现位于AbstractInstanceRegistry的registry方法中**。

在register中，服务实例的InstanceInfo保存在Lease中，Lease在AbstractInstanceRegistry中统一通过ConcurrentHashMap保存在内存中。在服务注册过程中，会先获取一个读锁，防止其他线程对registry注册表进行数据操作，避免数据的不一致。然后从resgitry查询对应的InstanceInfo租约是否已经存在注册表中，根据appName划分服务集群，使用InstanceId唯一标记服务实例。如果租约存在，比较两个租约中的InstanceInfo的最后更新时间lastDirtyTimestamp，保留时间戳大的服务实例信息InstanceInfo。如果租约不存在，意味这是一次全新的服务注册，将会进行自我保护的统计，创建新的租约保存InstanceInfo。接着将租约放到resgitry注册表中。之后将进行一系列缓存操作并根据覆盖状态规则设置服务实例的状态，缓存操作包括将InstanceInfo加入用于统计EurekaClient增量式获取注册表信息的recentlyChangedQueue和失效responseCache中对应的缓存。最后设置服务实例租约的上线时间用于计算租约的有效时间，释放读锁并完成服务注册。

```java
/**
     * Registers a new instance with a given duration.
     *
     * @see com.netflix.eureka.lease.LeaseManager#register(java.lang.Object, int, boolean)
     */
    public void register(InstanceInfo registrant, int leaseDuration, boolean isReplication) {
        try {
            // 获取锁
            read.lock();
            // 这里的registry是ConConcurrentHashMap 根据appName对服务集群进行分类
            Map<String, Lease<InstanceInfo>> gMap = registry.get(registrant.getAppName());
            REGISTER.increment(isReplication);
            if (gMap == null) {
                final ConcurrentHashMap<String, Lease<InstanceInfo>> gNewMap = new ConcurrentHashMap<String, Lease<InstanceInfo>>();
               // 这里有一个比较严谨的操作，防止在添加新的服务实例集群租约时，把已有的其他线程的集群租约覆盖掉，如果存在该键值，直接返回值，否则添加该键值对，返回null 
                gMap = registry.putIfAbsent(registrant.getAppName(), gNewMap);
                if (gMap == null) {
                    gMap = gNewMap;
                }
            }
            // 根据instanceId获取实例的租约
            Lease<InstanceInfo> existingLease = gMap.get(registrant.getId());
            // Retain the last dirty timestamp without overwriting it, if there is already a lease
            if (existingLease != null && (existingLease.getHolder() != null)) {
                Long existingLastDirtyTimestamp = existingLease.getHolder().getLastDirtyTimestamp();
                Long registrationLastDirtyTimestamp = registrant.getLastDirtyTimestamp();
                logger.debug("Existing lease found (existing={}, provided={}", existingLastDirtyTimestamp, registrationLastDirtyTimestamp);

                // this is a > instead of a >= because if the timestamps are equal, we still take the remote transmitted
                // InstanceInfo instead of the server local copy.
                // 如果该实例的租约已经存在，比较最后更新时间戳的大小，取最大值的注册信息为有效
                if (existingLastDirtyTimestamp > registrationLastDirtyTimestamp) {
                    logger.warn("There is an existing lease and the existing lease's dirty timestamp {} is greater" +
                            " than the one that is being registered {}", existingLastDirtyTimestamp, registrationLastDirtyTimestamp);
                    logger.warn("Using the existing instanceInfo instead of the new instanceInfo as the registrant");
                    registrant = existingLease.getHolder();
                }
            } else {
                // The lease does not exist and hence it is a new registration
                // 如果租约不存在，这时一个新的注册实例
                synchronized (lock) {
                    if (this.expectedNumberOfRenewsPerMin > 0) {
                        // Since the client wants to cancel it, reduce the threshold
                        // 自我保护机制
                        // (1
                        // for 30 seconds, 2 for a minute)
                        this.expectedNumberOfRenewsPerMin = this.expectedNumberOfRenewsPerMin + 2;
                        this.numberOfRenewsPerMinThreshold =
                                (int) (this.expectedNumberOfRenewsPerMin * serverConfig.getRenewalPercentThreshold());
                    }
                }
                logger.debug("No previous lease information found; it is new registration");
            }
            // 创建新租约
            Lease<InstanceInfo> lease = new Lease<InstanceInfo>(registrant, leaseDuration);
            if (existingLease != null) {
 //  如果租约存在，继承租约的服务上线初始时间
                lease.setServiceUpTimestamp(existingLease.getServiceUpTimestamp());
            }
           // 保存租约
            gMap.put(registrant.getId(), lease);
            // 添加最近注册的对列，用来统计最近注册服务实例的数据
            synchronized (recentRegisteredQueue) {
                recentRegisteredQueue.add(new Pair<Long, String>(
                        System.currentTimeMillis(),
                        registrant.getAppName() + "(" + registrant.getId() + ")"));
            }
            // This is where the initial state transfer of overridden status happens
            if (!InstanceStatus.UNKNOWN.equals(registrant.getOverriddenStatus())) {
                logger.debug("Found overridden status {} for instance {}. Checking to see if needs to be add to the "
                                + "overrides", registrant.getOverriddenStatus(), registrant.getId());
                if (!overriddenInstanceStatusMap.containsKey(registrant.getId())) {
                    logger.info("Not found overridden id {} and hence adding it", registrant.getId());
                    overriddenInstanceStatusMap.put(registrant.getId(), registrant.getOverriddenStatus());
                }
            }
            // 根据覆盖状态规则得到服务实例的最终状态，并设置服务实例的当前状态
            InstanceStatus overriddenStatusFromMap = overriddenInstanceStatusMap.get(registrant.getId());
            if (overriddenStatusFromMap != null) {
                logger.info("Storing overridden status {} from map", overriddenStatusFromMap);
                registrant.setOverriddenStatus(overriddenStatusFromMap);
            }

            // Set the status based on the overridden status rules
            InstanceStatus overriddenInstanceStatus = getOverriddenInstanceStatus(registrant, existingLease, isReplication);
            registrant.setStatusWithoutDirty(overriddenInstanceStatus);

            // If the lease is registered with UP status, set lease service up timestamp
            // 如果服务实例状态为up设置租约的服务上线时间，只有第一次设置有效
            if (InstanceStatus.UP.equals(registrant.getStatus())) {
                lease.serviceUp();
            }
            registrant.setActionType(ActionType.ADDED);
           // 添加最近租约变更记录队列，标识ActionType为ADDED
            // 这将用于Eureka Client 增量式获取注册表信息
           // 
            recentlyChangedQueue.add(new RecentlyChangedItem(lease));
           // 设置服务实例信息更新时间
            registrant.setLastUpdatedTimestamp();
           // 设置response缓存过期，这将用于Eureka Client全量获取注册表信息
            invalidateCache(registrant.getAppName(), registrant.getVIPAddress(), registrant.getSecureVipAddress());
            logger.info("Registered instance {}/{} with status {} (replication={})",
                    registrant.getAppName(), registrant.getId(), registrant.getStatus(), isReplication);
        } finally {
            // 释放锁
            read.unlock();
        }
    }
```

##### 接受服务心跳

在Eureka Client完成服务注册之后，它需要定时向EurekaServer发送心跳请求（默认30秒一次），维持自己在EurekaServer中租约的有效性。Eureka Server处理心跳请求的核心逻辑位于AbstractInstanceRegistry#renew方法中。renew方法是对Eureka Client位于注册表中的租约的续租操作，不像register方法需要服务实例信息，仅根据服务实例的服务名和服务实例id即可更新对应租约的有效时间

```java
 public boolean renew(String appName, String id, boolean isReplication) {
        RENEW.increment(isReplication);
     // 根据appName获取集群的租约集合
        Map<String, Lease<InstanceInfo>> gMap = registry.get(appName);
        Lease<InstanceInfo> leaseToRenew = null;
        if (gMap != null) {
            leaseToRenew = gMap.get(id);
        }
     // 租约不存在，直接返回false
        if (leaseToRenew == null) {
            RENEW_NOT_FOUND.increment(isReplication);
            logger.warn("DS: Registry: lease doesn't exist, registering resource: {} - {}", appName, id);
            return false;
        } else {
            // 根据覆盖状态规则得到服务实例的最终状态
            InstanceInfo instanceInfo = leaseToRenew.getHolder();
            if (instanceInfo != null) {
                // touchASGCache(instanceInfo.getASGName());
                InstanceStatus overriddenInstanceStatus = this.getOverriddenInstanceStatus(
                        instanceInfo, leaseToRenew, isReplication);
                if (overriddenInstanceStatus == InstanceStatus.UNKNOWN) {
                    logger.info("Instance status UNKNOWN possibly due to deleted override for instance {}"
                            + "; re-register required", instanceInfo.getId());
                // 如果得到的服务实例最后状态是UNKNOWN，取消续约
                    RENEW_NOT_FOUND.increment(isReplication);
                    return false;
                }
                if (!instanceInfo.getStatus().equals(overriddenInstanceStatus)) {
                    logger.info(
                            "The instance status {} is different from overridden instance status {} for instance {}. "
                                    + "Hence setting the status to overridden status", instanceInfo.getStatus().name(),
                                    instanceInfo.getOverriddenStatus().name(),
                                    instanceInfo.getId());
                    instanceInfo.setStatusWithoutDirty(overriddenInstanceStatus);

                }
            }
            // 统计每分钟租约续租的次数，用于自我保护
            renewsLastMin.increment();
            // 更新租约的有效时间
            leaseToRenew.renew();
            return true;
        }
    }




    /**
     * @return The rule that will process the instance status override.
     */
    protected abstract InstanceStatusOverrideRule getInstanceInfoOverrideRule();

    protected InstanceInfo.InstanceStatus getOverriddenInstanceStatus(InstanceInfo r,
                                                                    Lease<InstanceInfo> existingLease,
                                                                    boolean isReplication) {
        InstanceStatusOverrideRule rule = getInstanceInfoOverrideRule();
        logger.debug("Processing override status using rule: {}", rule);
        return rule.apply(r, existingLease, isReplication).status();
    }
```

在#renew方法中，不关注InstanceInfo，仅关注于租约本身以及租约的服务实例状态。如果根据服务实例的appName和instanceInfoId查询出服务实例的租约，并且根据#getOverriddenInstanceStatus方法得到的instanceStatus不为InstanceStatus.UNKNOWN，那么更新租约中的有效时间，即更新租约Lease中的lastUpdateTimestamp，达到续约的目的；如果租约不存在，那么返回续租失败的结果

##### 服务剔除

如果Eureka Client在注册后，既没有续约，也没有下线（服务崩溃或者网络异常等原因），那么服务的状态就处于不可知的状态，不能保证能够从该服务实例中获取到回馈，所以需要服务剔除AbstractInstanceRegistry#evict方法定时清理这些不稳定的服务，该方法会批量将注册表中所有过期租约剔除。实现代码如下所示：

```java
	/**
     * Evicts everything in the instance registry that has expired, if expiry is enabled.
     * 如果启用了到期，则将实例注册表中已到期的所有内容逐出
     * @see com.netflix.eureka.lease.LeaseManager#evict()
     */
    @Override
    public void evict() {
        evict(0l);
    }

    public void evict(long additionalLeaseMs) {
        logger.debug("Running the evict task");
		// 检查是否启用了租约到期 
        // 自我保护机制如果出现该状态不允许剔除服务
        if (!isLeaseExpirationEnabled()) {
            logger.debug("DS: lease expiration is currently disabled.");
            return;
        }
        //我们首先收集所有过期的物品，以随机顺序将其逐出。对于大型驱逐集，
        //如果不这样做，则可能会在自我保护开始之前先清除整个应用程序。通过将其随机化，
        //影响应均匀地分布在所有应用程序中。
        // 遍历注册表register，一次性获取所有的过期租约
        List<Lease<InstanceInfo>> expiredLeases = new ArrayList<>();
        for (Entry<String, Map<String, Lease<InstanceInfo>>> groupEntry : registry.entrySet()) {
            Map<String, Lease<InstanceInfo>> leaseMap = groupEntry.getValue();
            if (leaseMap != null) {
                for (Entry<String, Lease<InstanceInfo>> leaseEntry : leaseMap.entrySet()) {
                    Lease<InstanceInfo> lease = leaseEntry.getValue();
                    if (lease.isExpired(additionalLeaseMs) && lease.getHolder() != null) {
                        expiredLeases.add(lease);
                    }
                }
            }
        }

        //为了补偿GC暂停或本地时间漂移，我们需要使用当前注册表大小作为触发自我保存的基础。否则，我们将清除完整的注册表
        // 计算最大允许剔除的租约的数量，获取注册租约总数
        int registrySize = (int) getLocalRegistrySize();
        // 计算注册表租约的阈值，与自我保护相关
        int registrySizeThreshold = (int) (registrySize * serverConfig.getRenewalPercentThreshold());
        int evictionLimit = registrySize - registrySizeThreshold;
		// 计算剔除租约的数量
        int toEvict = Math.min(expiredLeases.size(), evictionLimit);
        if (toEvict > 0) {
            logger.info("Evicting {} items (expired={}, evictionLimit={})", toEvict, expiredLeases.size(), evictionLimit);

            Random random = new Random(System.currentTimeMillis());
            // 逐个随机剔除
            for (int i = 0; i < toEvict; i++) {
                // Pick a random item (Knuth shuffle algorithm)
                int next = i + random.nextInt(expiredLeases.size() - i);
                // 在指定列表中的指定位置交换元素。 （如果指定的位置相等，则调用此方法将使列表保持不变。
                Collections.swap(expiredLeases, i, next);
                Lease<InstanceInfo> lease = expiredLeases.get(i);

                String appName = lease.getHolder().getAppName();
                String id = lease.getHolder().getId();
                // EurekaMonitors 给定统计量增加计数器
                EXPIRED.increment();
                logger.warn("DS: Registry: expired lease for {}/{}", appName, id);
                // 逐个剔除
                internalCancel(appName, id, false);
            }
        }
    }
```

服务剔除将会遍历registry注册表，找出其中所有的过期租约，然后根据配置文件中续租百分比阀值和当前注册表的租约总数量计算出最大允许的剔除租约的数量（当前注册表中租约总数量减去当前注册表租约阀值），分批次剔除过期的服务实例租约。对过期的服务实例租约调用AbstractInstanceRegistry#internalCancel服务下线的方法将其从注册表中清除掉。服务剔除#evict方法中有很多限制，都是为了保证EurekaServer的可用性：

□ 自我保护时期不能进行服务剔除操作。

□ 过期操作是分批进行。

□ 服务剔除是随机逐个剔除，剔除均匀分布在所有应用中，防止在同一时间内同一服务集群中的服务全部过期被剔除，以致大量剔除发生时，在未进行自我保护前促使了程序的崩溃。

服务剔除是一个定时的任务，所以AbstractInstanceRegistry中定义了一个EvictionTask用于定时执行服务剔除，默认为60秒一次。服务剔除的定时任务一般在AbstractInstanceRegistry初始化结束后进行，按照执行频率evictionIntervalTimerInMs的设定，定时剔除过期的服务实例租约。自我保护机制主要在Eureka Client和Eureka Server之间存在网络分区的情况下发挥保护作用，在服务器端和客户端都有对应实现。假设在某种特定的情况下（如网络故障）, Eureka Client和Eureka Server无法进行通信，此时Eureka Client无法向EurekaServer发起注册和续约请求，Eureka Server中就可能因注册表中的服务实例租约出现大量过期而面临被剔除的危险，然而此时的Eureka Client可能是处于健康状态的（可接受服务访问），如果直接将注册表中大量过期的服务实例租约剔除显然是不合理的。针对这种情况，Eureka设计了“自我保护机制”。在EurekaServer处，如果出现大量的服务实例过期被剔除的现象，那么该Server节点将进入自我保护模式，保护注册表中的信息不再被剔除，在通信稳定后再退出该模式；在Eureka Client处，如果向Eureka Server注册失败，将快速超时并尝试与其他的EurekaServer进行通信。“自我保护机制”的设计大大提高了Eureka的可用性。

##### 服务下线

Eureka Client在应用销毁时，会向Eureka Server发送服务下线请求，清除注册表中关于本应用的租约，避免无效的服务调用。在服务剔除的过程中，也是通过服务下线的逻辑完成对单个服务实例过期租约的清除工作。服务下线的主要实现代码位于AbstractInstanceRegistry#internalCancel方法中，仅需要服务实例的服务名和服务实例id即可完成服务下线

```java
	/**
     * 取消实例的注册。通常，它在关闭客户端以通知服务器从流量中删除实例时由客户端调用。
     * </p>
     *
     * @param appName the application name of the application.
     * @param id the unique identifier of the instance.
     * @param isReplication true if this is a replication event from other nodes, false
     *                      otherwise.
     * @return true if the instance was removed from the {@link AbstractInstanceRegistry} successfully, false otherwise.
     */
    @Override
    public boolean cancel(String appName, String id, boolean isReplication) {
        return internalCancel(appName, id, isReplication);
    }

    /**
     * cancel(String, String, boolean)方法被PeerAwareInstanceRegistry覆盖，因此每个取消请求都复制到对等方。 但是，对于在远程对等方中视为有效取消的过期，这是不希望的，因此不会启动自我保存模式
     */
    protected boolean internalCancel(String appName, String id, boolean isReplication) {
        try {
            // 获取锁，防止被其他线程进行修改
            read.lock();
            // 根据这是由于从其他eureka服务器进行复制还是由于eureka客户端启动的操作而增加给定统计信息的计数器
            // 调用EurekaMonitors
            CANCEL.increment(isReplication);
            // 根据appName获取服务实例集群
            Map<String, Lease<InstanceInfo>> gMap = registry.get(appName);
            Lease<InstanceInfo> leaseToCancel = null;
            // 移除服务实例的租约
            if (gMap != null) {
                leaseToCancel = gMap.remove(id);
            }
            // 将服务实例信息添加到最近下线服务实例统计队列
            synchronized (recentCanceledQueue) {
                recentCanceledQueue.add(new Pair<Long, String>(System.currentTimeMillis(), appName + "(" + id + ")"));
            }
            InstanceStatus instanceStatus = overriddenInstanceStatusMap.remove(id);
            if (instanceStatus != null) {
                logger.debug("Removed instance id {} from the overridden map which has value {}", id, instanceStatus.name());
            }
             // 如果租约不存在返回false
            if (leaseToCancel == null) {
                CANCEL_NOT_FOUND.increment(isReplication);
                logger.warn("DS: Registry: cancel failed because Lease is not registered for: {}/{}", appName, id);
                return false;
            } else {
                // 设置租约的下线时间
                leaseToCancel.cancel();
                InstanceInfo instanceInfo = leaseToCancel.getHolder();
                String vip = null;
                String svip = null;
                if (instanceInfo != null) {
                    instanceInfo.setActionType(ActionType.DELETED);
                    // 添加最近租约变更记录的队列，标识ActionType为DELETED
                    // 这将用于Eureka Client 增量式获取注册表信息
                    recentlyChangedQueue.add(new RecentlyChangedItem(leaseToCancel));
                    instanceInfo.setLastUpdatedTimestamp();
                    vip = instanceInfo.getVIPAddress();
                    svip = instanceInfo.getSecureVipAddress();
                }
                // 设置response缓存过期
                invalidateCache(appName, vip, svip);
                logger.info("Cancelled instance {}/{} (replication={})", appName, id, isReplication);
                // 下线成功
                return true;
            }
        } finally {
            // 释放锁
            read.unlock();
        }
    }
```

internalCancel方法与register方法的行为过程很类似，首先通过registry根据服务名和服务实例id查询关于服务实例的租约Lease是否存在，统计最近请求下线的服务实例用于EurekaServer主页展示。如果租约不存在，返回下线失败；如果租约存在，从registry注册表中移除，设置租约的下线时间，同时在最近租约变更记录队列中添加新的下线记录，以用于EurekaClient的增量式获取注册表信息，最后设置repsonse缓存过期。internalCancel方法中同样通过读锁保证registry注册表中数据的一致性，避免脏读

##### 集群同步 

如果Eureka Server是通过集群的方式进行部署，那么为了维护整个集群中Eureka Server注册表数据的一致性，势必需要一个机制同步Eureka Server集群中的注册表数据。Eureka Server集群同步包含两个部分，一部分是EurekaServer在启动过程中从它的peer节点中拉取注册表信息，并将这些服务实例的信息注册到本地注册表中；另一部分是EurekaServer每次对本地注册表进行操作时，同时会将操作同步到它的peer节点中，达到集群注册表数据统一的目的

###### Eureka Server初始化本地注册表信息

在Eureka Server启动的过程中，会从它的peer节点中拉取注册表来初始化本地注册表，这部分主要通过PeerAwareInstanceRegistryImpl#syncUp方法完成，它将从可能存在的peer节点中，拉取peer节点中的注册表信息，并将其中的服务实例信息注册到本地注册表中

```java
/**
     * Populates the registry information from a peer eureka node. This
     * operation fails over to other nodes until the list is exhausted if the
     * communication fails.
     */
    @Override
    public int syncUp() {
        // 从临近的Peer中复制整个注册表
        // Copy entire entry from neighboring DS node
        int count = 0;
		// 如果获取不到，线程等待
        for (int i = 0; ((i < serverConfig.getRegistrySyncRetries()) && (count == 0)); i++) {
            if (i > 0) {
                try {
                    Thread.sleep(serverConfig.getRegistrySyncRetryWaitMs());
                } catch (InterruptedException e) {
                    logger.warn("Interrupted during registry transfer..");
                    break;
                }
            }
            // 获取所有的服务实例
            Applications apps = eurekaClient.getApplications();
            for (Application app : apps.getRegisteredApplications()) {
                for (InstanceInfo instance : app.getInstances()) {
                    try {
                        // 判断是否可注册，主要用于AWS环境下进行，若部署在其他的环境，直接返回true
                        if (isRegisterable(instance)) {
                            // 注册到自身的注册表中
                            register(instance, instance.getLeaseInfo().getDurationInSecs(), true);
                            count++;
                        }
                    } catch (Throwable t) {
                        logger.error("During DS init copy", t);
                    }
                }
            }
        }
        return count;
    }
```



Eureka Server也是一个Eureka Client，在启动的时候也会进行DiscoveryClient的初始化，会从其对应的Eureka Server中拉取全量的注册表信息。在Eureka Server集群部署的情况下，Eureka Server从它的peer节点中拉取到注册表信息后，将遍历这个Applications，将所有的服务实例通过AbstractRegistry#register方法注册到自身注册表中。在初始化本地注册表时，Eureka Server并不会接受来自EurekaClient的通信请求（如注册、或者获取注册表信息等请求）。在同步注册表信息结束后会通过PeerAwareInstanceRegistryImpl#openForTraffic方法允许该Server接受流量

```java
@Override
    public void openForTraffic(ApplicationInfoManager applicationInfoManager, int count) {
        // Renewals happen every 30 seconds and for a minute it should be a factor of 2.
        // 初始化自我保护机制统计参数
        this.expectedNumberOfRenewsPerMin = count * 2;
        this.numberOfRenewsPerMinThreshold =
                (int) (this.expectedNumberOfRenewsPerMin * serverConfig.getRenewalPercentThreshold());
        logger.info("Got {} instances from neighboring DS node", count);
        logger.info("Renew threshold is: {}", numberOfRenewsPerMinThreshold);
        this.startupTime = System.currentTimeMillis();
        // 如果同步的应用实例数量为0，将在一段时间内拒绝Client获取注册信息
        if (count > 0) {
            this.peerInstancesTransferEmptyOnStartup = false;
        }
        DataCenterInfo.Name selfName = applicationInfoManager.getInfo().getDataCenterInfo().getName();
        boolean isAws = Name.Amazon == selfName;
        // 判断是否是AWS运行环境
        if (isAws && serverConfig.shouldPrimeAwsReplicaConnections()) {
            logger.info("Priming AWS connections for all replicas..");
            primeAwsReplicas(applicationInfoManager);
        }
        logger.info("Changing status to UP");
        // 修改服务实例的状态为健康上线，可以接受流量
        applicationInfoManager.setInstanceStatus(InstanceStatus.UP);
        super.postInit();
    }
```

在Eureka Server中有一个StatusFilter过滤器，用于检查Eureka Server的状态，当Server的状态不为UP时，将拒绝所有的请求。在Client请求获取注册表信息时，Server会判断此时是否允许获取注册表中的信息。上述做法是为了避免EurekaServer在#syncUp方法中没有获取到任何服务实例信息时（Eureka Server集群部署的情况下）, Eureka Server注册表中的信息影响到Eureka Client缓存的注册表中的信息。如果Eureka Server在#syncUp方法中没有获得任何的服务实例信息，它将把peerInstancesTransferEmptyOnStartup设置为true，这时该Eureka Server在WaitTimeInMsWhenSyncEmpty（可以通过eureka.server.wait-time-in-ms-when-sync-empty设置，默认是5分钟）时间后才能被Eureka Client访问获取注册表信息

###### Eureka Server之间注册表信息的同步复制

为了保证Eureka Server集群运行时注册表信息的一致性，每个Eureka Server在对本地注册表进行管理操作时，会将相应的操作同步到所有peer节点中。在**PeerAwareInstanceRegistryImpl**中，对Abstractinstanceregistry中的#register、#cancel和#renew等方法都添加了同步到peer节点的操作，使Server集群中注册表信息保持最终一致性，如下所示

```java
@Override
    public boolean cancel(final String appName, final String id,
                          final boolean isReplication) {
        if (super.cancel(appName, id, isReplication)) {
            // 同步下线状态
            replicateToPeers(Action.Cancel, appName, id, null, null, isReplication);
            synchronized (lock) {
                if (this.expectedNumberOfRenewsPerMin > 0) {
                    // Since the client wants to cancel it, reduce the threshold (1 for 30 seconds, 2 for a minute)
                    this.expectedNumberOfRenewsPerMin = this.expectedNumberOfRenewsPerMin - 2;
                    this.numberOfRenewsPerMinThreshold =
                            (int) (this.expectedNumberOfRenewsPerMin * serverConfig.getRenewalPercentThreshold());
                }
            }
            return true;
        }
        return false;
    }

    /**
     * Registers the information about the {@link InstanceInfo} and replicates
     * this information to all peer eureka nodes. If this is replication event
     * from other replica nodes then it is not replicated.
     *
     * @param info
     *            the {@link InstanceInfo} to be registered and replicated.
     * @param isReplication
     *            true if this is a replication event from other replica nodes,
     *            false otherwise.
     */
    @Override
    public void register(final InstanceInfo info, final boolean isReplication) {
        int leaseDuration = Lease.DEFAULT_DURATION_IN_SECS;
        if (info.getLeaseInfo() != null && info.getLeaseInfo().getDurationInSecs() > 0) {
            leaseDuration = info.getLeaseInfo().getDurationInSecs();
        }
        super.register(info, leaseDuration, isReplication);
        // 同步注册状态
        replicateToPeers(Action.Register, info.getAppName(), info.getId(), info, null, isReplication);
    }

    /*
     * (non-Javadoc)
     *
     * @see com.netflix.eureka.registry.InstanceRegistry#renew(java.lang.String,
     * java.lang.String, long, boolean)
     */
    public boolean renew(final String appName, final String id, final boolean isReplication) {
        if (super.renew(appName, id, isReplication)) {
            // 同步续约状态
            replicateToPeers(Action.Heartbeat, appName, id, null, null, isReplication);
            return true;
        }
        return false;
    }
    
    
private void replicateToPeers(Action action, String appName, String id,
                                  InstanceInfo info /* optional */,
                                  InstanceStatus newStatus /* optional */, boolean isReplication) {
        Stopwatch tracer = action.getTimer().start();
        try {
            if (isReplication) {
                numberOfReplicationsLastMin.increment();
            }
            // If it is a replication already, do not replicate again as this will create a poison replication
            // 如果peer集群为空，或者这本来就是复制操作，那么就不再复制，防止造成循环复制
            if (peerEurekaNodes == Collections.EMPTY_LIST || isReplication) {
                return;
            }
			// 向peer 集群的每一个peer进行同步
            for (final PeerEurekaNode node : peerEurekaNodes.getPeerEurekaNodes()) {
                // If the url represents this host, do not replicate to yourself.
                // 如果peer节点是自身的话，不进行同步复制
                if (peerEurekaNodes.isThisMyUrl(node.getServiceUrl())) {
                    continue;
                }
                // 根据Action调用不同的同步请求
                replicateInstanceActionsToPeers(action, appName, id, info, newStatus, node);
            }
        } finally {
            tracer.stop();
        }
    }

    /**
     * Replicates all instance changes to peer eureka nodes except for
     * replication traffic to this node.
     * 根据action的不同，调用PeerEurekaNode的不同方法进行同步复制
     *  将所有实例更改复制到对等eureka节点，但复制到该节点的流量除外
     * Action是一个枚举内部类 包含 Heartbeat,Refister,Cancel,StatusUpdate,DeleteStatusOverride
     */
    private void replicateInstanceActionsToPeers(Action action, String appName,
                                                 String id, InstanceInfo info, InstanceStatus newStatus,
                                                 PeerEurekaNode node) {
        try {
            InstanceInfo infoFromRegistry = null;
            CurrentRequestVersion.set(Version.V2);
            switch (action) {
                case Cancel:
                    // 同步下线
                    node.cancel(appName, id);
                    break;
                case Heartbeat:
                    InstanceStatus overriddenStatus = overriddenInstanceStatusMap.get(id);
                    infoFromRegistry = getInstanceByAppAndId(appName, id, false);
                    // 同步心跳
                    node.heartbeat(appName, id, infoFromRegistry, overriddenStatus, false);
                    break;
                case Register:
                    // 同步注册
                    node.register(info);
                    break;
                case StatusUpdate:
                    infoFromRegistry = getInstanceByAppAndId(appName, id, false);			// 同步状态更新
                    node.statusUpdate(appName, id, newStatus, infoFromRegistry);
                    break;
                case DeleteStatusOverride:
                    infoFromRegistry = getInstanceByAppAndId(appName, id, false);
                    node.deleteStatusOverride(appName, id, infoFromRegistry);
                    break;
            }
        } catch (Throwable t) {
            logger.error("Cannot replicate information to {} for action {}", node.getServiceUrl(), action.name(), t);
        }
    }    
```

PeerEurekaNode中的每一个同步复制都是通过批任务流的方式进行操作，同一时间段内相同服务实例的相同操作将使用相同的任务编号，在进行同步复制的时候根据任务编号合并操作，减少同步操作的数量和网络消耗，但是同时也造成同步复制的延时性，不满足CAP中的C（强一致性）。通过Eureka Server在启动过程中初始化本地注册表信息和Eureka Server集群间的同步复制操作，最终达到了集群中Eureka Server注册表信息一致的目的

##### 获取注册表中服务实例信息

Eureka Server中获取注册表的服务实例信息主要通过两个方法实现：AbstractInstanceRegistry .getApplicationsFromMultipleRegions()从多地区获取全量注册表数据，AbstractInstanceRegistry.getApplicationDeltasFromMultipleRegions()从多地区获取增量式注册表数据。

###### getApplicationsFromMultipleRegions获取全量注册表数据

```java
    public Applications getApplicationsFromMultipleRegions(String[] remoteRegions) {

        boolean includeRemoteRegion = null != remoteRegions && remoteRegions.length != 0;

        logger.debug("Fetching applications registry with remote regions: {}, Regions argument {}",
                includeRemoteRegion, remoteRegions);

        if (includeRemoteRegion) {
            GET_ALL_WITH_REMOTE_REGIONS_CACHE_MISS.increment();
        } else {
            GET_ALL_CACHE_MISS.increment();
        }
        Applications apps = new Applications();
        apps.setVersion(1L);
        // 从本地registry获取所有的服务实例信息InstanceInfo
        for (Entry<String, Map<String, Lease<InstanceInfo>>> entry : registry.entrySet()) {
            Application app = null;

            if (entry.getValue() != null) {
                for (Entry<String, Lease<InstanceInfo>> stringLeaseEntry : entry.getValue().entrySet()) {
                    Lease<InstanceInfo> lease = stringLeaseEntry.getValue();
                    if (app == null) {
                        app = new Application(lease.getHolder().getAppName());
                    }
                    app.addInstance(decorateInstanceInfo(lease));
                }
            }
            if (app != null) {
                apps.addApplication(app);
            }
        }
        if (includeRemoteRegion) {
            // 获取远程Region中的Eureka Server中的注册表信息
            for (String remoteRegion : remoteRegions) {
                RemoteRegionRegistry remoteRegistry = regionNameVSRemoteRegistry.get(remoteRegion);
                if (null != remoteRegistry) {
                    Applications remoteApps = remoteRegistry.getApplications();
                    for (Application application : remoteApps.getRegisteredApplications()) {
                        if (shouldFetchFromRemoteRegistry(application.getName(), remoteRegion)) {
                            logger.info("Application {}  fetched from the remote region {}",
                                    application.getName(), remoteRegion);

                            Application appInstanceTillNow = apps.getRegisteredApplications(application.getName());
                            if (appInstanceTillNow == null) {
                                appInstanceTillNow = new Application(application.getName());
                                apps.addApplication(appInstanceTillNow);
                            }
                            for (InstanceInfo instanceInfo : application.getInstances()) {
                                appInstanceTillNow.addInstance(instanceInfo);
                            }
                        } else {
                            logger.debug("Application {} not fetched from the remote region {} as there exists a "
                                            + "whitelist and this app is not in the whitelist.",
                                    application.getName(), remoteRegion);
                        }
                    }
                } else {
                    logger.warn("No remote registry available for the remote region {}", remoteRegion);
                }
            }
        }
        apps.setAppsHashCode(apps.getReconcileHashCode());
        return apps;
    }

```

它首先会将本地注册表registry中的所有服务实例信息提取出来封装到Applications中，再根据是否需要拉取远程Region中的注册表信息，将远程Region的Eureka Server注册表中的服务实例信息添加到Applications中。最后将封装了全量注册表信息的Applications返回给Client

###### getApplicationDeltasFromMultipleRegions从多地区获取增量式注册表数据。

getApplicationDeltasFromMultipleRegions方法将会从多个地区中获取增量式注册表信息，并封装成Applications返回，实现代码如下所示

```java
public Applications getApplicationDeltasFromMultipleRegions(String[] remoteRegions) {
        if (null == remoteRegions) {
            remoteRegions = allKnownRemoteRegions; // null means all remote regions.
        }

        boolean includeRemoteRegion = remoteRegions.length != 0;

        if (includeRemoteRegion) {
            GET_ALL_WITH_REMOTE_REGIONS_CACHE_MISS_DELTA.increment();
        } else {
            GET_ALL_CACHE_MISS_DELTA.increment();
        }

        Applications apps = new Applications();
        apps.setVersion(responseCache.getVersionDeltaWithRegions().get());
        Map<String, Application> applicationInstancesMap = new HashMap<String, Application>();
        try {
            // 开启写锁
            write.lock();
            // 遍历recentlyChangedQueue队列获取最近变化的服务实例信息InstanceInfo
            Iterator<RecentlyChangedItem> iter = this.recentlyChangedQueue.iterator();
            logger.debug("The number of elements in the delta queue is :{}", this.recentlyChangedQueue.size());
            while (iter.hasNext()) {
                Lease<InstanceInfo> lease = iter.next().getLeaseInfo();
                InstanceInfo instanceInfo = lease.getHolder();
                logger.debug("The instance id {} is found with status {} and actiontype {}",
                        instanceInfo.getId(), instanceInfo.getStatus().name(), instanceInfo.getActionType().name());
                Application app = applicationInstancesMap.get(instanceInfo.getAppName());
                if (app == null) {
                    app = new Application(instanceInfo.getAppName());
                    applicationInstancesMap.put(instanceInfo.getAppName(), app);
                    apps.addApplication(app);
                }
                app.addInstance(decorateInstanceInfo(lease));
            }

            if (includeRemoteRegion) {
                // 获取远程Region中的Eureka Server的增量式注册表信息
                for (String remoteRegion : remoteRegions) {
                    RemoteRegionRegistry remoteRegistry = regionNameVSRemoteRegistry.get(remoteRegion);
                    if (null != remoteRegistry) {
                        Applications remoteAppsDelta = remoteRegistry.getApplicationDeltas();
                        if (null != remoteAppsDelta) {
                            for (Application application : remoteAppsDelta.getRegisteredApplications()) {
                                if (shouldFetchFromRemoteRegistry(application.getName(), remoteRegion)) {
                                    Application appInstanceTillNow =
                                            apps.getRegisteredApplications(application.getName());
                                    if (appInstanceTillNow == null) {
                                        appInstanceTillNow = new Application(application.getName());
                                        apps.addApplication(appInstanceTillNow);
                                    }
                                    for (InstanceInfo instanceInfo : application.getInstances()) {
                                        appInstanceTillNow.addInstance(instanceInfo);
                                    }
                                }
                            }
                        }
                    }
                }
            }

            Applications allApps = getApplicationsFromMultipleRegions(remoteRegions);
            // 计算应用集合一致性哈希码，用以在Eureka Client拉取时进行对比
            apps.setAppsHashCode(allApps.getReconcileHashCode());
            return apps;
        } finally {
            write.unlock();
        }
    }
```

获取增量式注册表信息将会从recentlyChangedQueue队列中获取最近变化的服务实例信息。recentlyChangedQueue队列中统计了近3分钟内进行注册、修改和剔除的服务实例信息，在服务注册AbstractInstanceRegistry.registry()、接受心跳请求AbstractInstanceRegistry.renew()和服务下线AbstractInstanceRegistry.internalCancel()等方法中均可见到recentlyChangedQueue队列对这些服务实例进行登记，用于记录增量式注册表信息。getApplicationsFromMultipleRegions()方法同样提供了从远程Region的Eureka Server获取增量式注册表信息的能力

#### **Eureka和ZooKeeper**

著名的CAP理论指出，一个分布式系统不可能同时满足C（一致性）、A（可用性）和P（分区容错性）。由于分区容错性在是分布式系统中必须要保证的，因此我们只能在A和C之间进行权衡。

##### **ZooKeeper保证CP**

当向注册中心查询服务列表时，我们可以容忍注册中心返回的是几分钟以前的注册信息，但不能接受服务直接down掉不可用。也就是说，服务注册功能对可用性的要求要高于一致性。但是ZooKeeper会出现这样一种情况，当Master节点因为网络故障与其他节点失去联系时，剩余节点会重新进行leader选举。问题在于，选举leader的时间太长，30 ~ 120s，且选举期间整个ZooKeeper集群都是不可用的，这就导致在选举期间注册服务瘫痪。在云部署的环境下，因网络问题使得ZooKeeper集群失去Master节点是较大概率会发生的事，虽然服务能够最终恢复，但是漫长的选举时间导致的注册长期不可用是不能容忍的。

##### **Eureka保证AP**

Eureka在设计时就优先保证可用性。Eureka各个节点都是平等的，几个节点挂掉不会影响正常节点的工作，剩余的节点依然可以提供注册和查询服务。而Eureka的客户端在向某个Eureka注册或时如果发现连接失败，则会自动切换至其它节点，只要有一台Eureka还在，就能保证注册服务可用（保证可用性），只不过查到的信息可能不是最新的（不保证强一致性）。

参考：https://www.jianshu.com/p/6a3db6939fb0

### 各组件深入之Spring Cloud openFeign（远程调用）

在微服务架构中，业务都会被拆分成一个独立的服务，服务与服务的通讯是基于HTTP RESTful的。Spring Cloud有两种服务调用方式，一种是Ribbon+RestTemplate，另一种是Feign。

Feign是声明性Web服务客户端。 它使编写Web服务客户端更加容易。 要使用Feign，请创建一个接口并对其进行注释。 它具有可插入注释支持，包括Feign注释和JAX-RS注释。 Feign还支持可插拔编码器和解码器。 Spring Cloud添加了对Spring MVC注释的支持，并支持使用Spring Web中默认使用的相同HttpMessageConverters。 Spring Cloud集成了Eureka和Spring Cloud LoadBalancer，以在使用Feign时提供负载平衡的http客户端。 就是通过把http请求封装到了注解中。

使用OpenFeign的Spring应用架构一般分为三个部分，分别为服务注册中心、服务提供者和服务消费者。服务提供者向服务注册中心注册自己，然后服务消费者通过OpenFeign发送请求时，OpenFeign会向服务注册中心获取关于服务提供者的信息，然后再向服务提供者发送网络请求

[![c5isUS.png](https://z3.ax1x.com/2021/04/17/c5isUS.png)](https://imgtu.com/i/c5isUS)



Spring Cloud 的 Feign 支持中的一个核心概念是命名客户机。每个佯装的客户机都是一个组件集合的一部分，这些组件一起工作，根据需要联系一个远程服务器，这个集合有一个名称，作为一个使用@feignclient 注释的应用程序开发人员，你可以给它一个名称。Spring Cloud 使用 FeignClientsConfiguration 根据需要为每个命名客户机创建一个新的集合，作为 ApplicationContext。其中包括一个假动作。解码器，一个假装。编码器，和一个假装。合约。可以使用@feignclient 注释的 contextId 属性覆盖集合的名称。

Hystrix 支持熔断(fallback)的概念: 一个默认的代码路径，在熔断或出现错误时执行。要为给定的@feignclient 启用熔断，请将熔断属性设置为实现熔断的类名。您还需要将实现声明为 springbean。

```java
/**
 * 去请求feign服务端itoken-service-admin中的服务接口
 * @Author kay三石
 * @date:2019/6/22
 */
// value 是声明的方式指向了 服务提供者
@FeignClient(value="itoken-service-admin",fallback = AdminServiceFallback.class)
public interface AdminService  extends BaseClientService {

    /**
     * 根据 ID 获取管理员
     *
     * @return
     */
    @RequestMapping(value = "v1/admins", method = RequestMethod.GET)
    public String get(
            @RequestParam(required = true, value = "userCode") String userCode
    );
}
```

如果需要访问制造回退触发器的原因，可以在@feignclient 中使用 fallbackFactory 属性。

```java
// name 调用服务的名称和value等
@FeignClient(name=ServiceNameConstants.DEMOB_SERVICE,fallbackFactory = DemobServiceClientFallbackFactory.class)
public interface DemobServiceClient {
    
    @GetMapping(value = "/demob/test/getDemobById")
    DemobDTO getDemobById(@RequestParam("id")String id);
    
}   
@Component
public class DemobServiceClientFallbackFactory implements FallbackFactory<DemobServiceClient> {

    @Override
    public DemobServiceClient create(Throwable cause) {
        DemobServiceClientFallback demobServiceClientFallback = new DemobServiceClientFallback();
        demobServiceClientFallback.setCause(cause);
        return demobServiceClientFallback;
    }

}
@Slf4j
public class DemobServiceClientFallback implements DemobServiceClient {
    @Setter
    private Throwable cause;
    
    @Override
    public DemobDTO getDemobById(String id) {
        log.error("根据id获取demob信息失败",cause);
        throw new FirstException();
    }

}
```

| 注解           | 接口Target     | 使用说明                                                     |
| -------------- | -------------- | ------------------------------------------------------------ |
| `@RequestLine` | 方法上         | 定义HttpMethod 和 UriTemplate. UriTemplate 中使用`{}` 包裹的表达式，可以通过在方法参数上使用@Param 自动注入 |
| `@Param`       | 方法参数       | 定义模板变量，模板变量的值可以使用名称的方式使用模板注入解析 |
| `@Headers`     | 类上或者方法上 | 定义头部模板变量，使用@Param 注解提供参数值的注入。如果该注解添加在接口类上，则所有的请求都会携带对应的Header信息；如果在方法上，则只会添加到对应的方法请求上 |
| `@QueryMap`    | 方法上         | 定义一个键值对或者 pojo，参数值将会被转换成URL上的 query 字符串上 |
| `@HeaderMap`   | 方法上         | 定义一个HeaderMap, 与 UrlTemplate 和HeaderTemplate 类型，可以使用@Param 注解提供参数值 |



#### FeignClient 的配置参数

| 属性名        | 默认值     | 作用                                                         | 备注                                        |
| ------------- | ---------- | ------------------------------------------------------------ | ------------------------------------------- |
| value         | 空字符串   | 调用服务名称，和name属性相同                                 |                                             |
| serviceId     | 空字符串   | 服务id，作用和name属性相同                                   | 已过期                                      |
| name          | 空字符串   | 调用服务名称，和value属性相同                                |                                             |
| url           | 空字符串   | 全路径地址或hostname，http或https可选                        |                                             |
| decode404     | false      | 配置响应状态码为404时是否应该抛出FeignExceptions             |                                             |
| configuration | {}         | 自定义当前feign client的一些配置                             | 参考FeignClientsConfiguration               |
| fallback      | void.class | 熔断机制，调用失败时，走的一些回退方法，可以用来抛出异常或给出默认返回数据。 | 底层依赖hystrix，启动类要加上@EnableHystrix |
| path          | 空字符串   | 自动给所有方法的requestMapping前加上前缀，类似与controller类上的requestMapping |                                             |
| primary       | true       |                                                              |                                             |

#### Feign原理：

- 启动时，程序会进行包扫描，扫描所有包下所有@FeignClient注解的类，并将这些类注入到spring的IOC容器中。当定义的Feign中的接口被调用时，通过JDK的动态代理来生成RequestTemplate。
- RequestTemplate中包含请求的所有信息，如请求参数，请求URL等。
- RequestTemplate声场Request，然后将Request交给client处理，这个client默认是JDK的HTTPUrlConnection，也可以是OKhttp、Apache的HTTPClient等。
- 最后client封装成LoadBaLanceClient，结合ribbon负载均衡地发起调用。

#### Feign源码解析

##### 核心组件

在阅读OpenFeign源码时，可以沿着两条路线进行，一是FeignServiceClient这样的被@FeignClient注解修饰的接口类（后续简称为FeignClient接口类）如何创建，也就是其Bean实例是如何被创建的；二是调用FeignServiceClient对象的网络请求相关的函数时，OpenFeign是如何发送网络请求的。而OpenFeign相关的类也可以以此来进行分类，一部分是用来初始化相应的Bean实例的，一部分是用来在调用方法时发送网络请求。

下图是关于Fegin的相关的关键类图。其中比较重要的类为FeignClientFactoryBean、FeignContext和SynchronousMethodHandler。FeignClientFactoryBean是创建@FeignClient修饰的接口类Bean实例的工厂类；FeignContext是配置组件的上下文环境，保存着相关组件的不同实例，这些实例由不同的FeignConfiguration配置类构造出来；SynchronousMethodHandler是MethodHandler的子类，可以在FeignClient相应方法被调用时发送网络请求，然后再将请求响应转化为函数返回值进行输出。

[![cIDO0J.png](https://z3.ax1x.com/2021/04/18/cIDO0J.png)](https://imgtu.com/i/cIDO0J)

OpenFeign会首先进行相关BeanDefinition的动态注册，然后当Spring容器注入相关实例时会进行实例的初始化，最后当FeignClient接口类实例的函数被调用时会发送网络请求。

##### 动态注册BeanDefinition

OpenFeign可以通过多种方式进行自定义配置，配置的变化会导致接口类初始化时使用不同的Bean实例，从而控制OpenFeign的相关行为，比如说网络请求的编解码、压缩和日志处理

###### FeignClentsRegister

@EnableFeignClients有三个作用，一是引入FeignClientsRegistrar；二是指定扫描FeignClient的包信息，就是指定FeignClient接口类所在的包名；三是指定FeignClient接口类的自定义配置类。@EnableFeignClients注解的定义如下所示.

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
@Import(FeignClientsRegistrar.class)
public @interface EnableFeignClients {

	/**
	 * basePackages()属性的别名。 允许使用更简洁的注释声明，例如： @ComponentScan("org.my.pkg")而不是@ComponentScan(basePackages="org.my.pkg") 。
	 * @return the array of 'basePackages'.
	 */
	String[] value() default {};

	/**
	 * 基本软件包以扫描带注释的组件。
value()是此属性的别名（并与该属性互斥）。
使用basePackageClasses()作为基于字符串的软件包名称的类型安全替代方法
	 *
	 * @return the array of 'basePackages'.
	 */
	String[] basePackages() default {};

	/**
	 * basePackages()类型安全替代方法，用于指定要扫描的组件以扫描带注释的组件。 指定类别的包装将被扫描。
考虑在每个程序包中创建一个特殊的无操作标记类或接口，该类或接口除了被该属性引用外没有其他用途。.
	 *
	 * @return the array of 'basePackageClasses'.
	 */
	Class<?>[] basePackageClasses() default {};

	/**
	 * 自定义feign client的自定义配置，例如feign.codec.Decoder ， feign.codec.Encoder和feign.Contract 组件。
	 */
	Class<?>[] defaultConfiguration() default {};

	/**
	 * 用@FeignClient注释的类的列表。 如果不为空，则禁用类路径扫描
	 * @return
	 */
	Class<?>[] clients() default {};
}

```

FeignClientsRegistrar是ImportBeanDefinitionRegistrar的子类，Spring用ImportBeanDefinitionRegistrar来动态注册BeanDefinition。OpenFeign通过FeignClientsRegistrar来处理@FeignClient修饰的FeignClient接口类，将这些接口类的BeanDefinition注册到Spring容器中，这样就可以使用@Autowired等方式来自动装载这些FeignClient接口类的Bean实例。FeignClientsRegistrar的部分代码如下所示。

```java
class FeignClientsRegistrar implements ImportBeanDefinitionRegistrar,
		ResourceLoaderAware, EnvironmentAware {

	// patterned after Spring Integration IntegrationComponentScanRegistrar
	// and RibbonClientsConfigurationRegistgrar

	private ResourceLoader resourceLoader;

	private Environment environment;

	public FeignClientsRegistrar() {
	}

	@Override
	public void setResourceLoader(ResourceLoader resourceLoader) {
		this.resourceLoader = resourceLoader;
	}

	@Override
	public void registerBeanDefinitions(AnnotationMetadata metadata,
			BeanDefinitionRegistry registry) {
        // 从EnableFeignClients的属性值来构建Feign的自定义Configuration进行注册
		registerDefaultConfiguration(metadata, registry);
        // 扫描package，注册被@FeignClient修饰的接口类的Bean信息
		registerFeignClients(metadata, registry);
	}
            
```

FeignClientsRegistrar的registerBeanDefinitions方法主要做了两个事情，一是注册@EnableFeignClients提供的自定义配置类中的相关Bean实例，二是根据@EnableFeignClients提供的包信息扫描@FeignClient注解修饰的FeignCleint接口类，然后进行Bean实例注册。@EnableFeignClients的自定义配置类是被@Configuration注解修饰的配置类，它会提供一系列组装FeignClient的各类组件实例。这些组件包括：Client、Targeter、Decoder、Encoder和Contract等。接下来看看registerDefaultConfiguration的代码实现，如下所示

```java
private void registerDefaultConfiguration(AnnotationMetadata metadata,
			BeanDefinitionRegistry registry) {
    // 获取到metadata中关于enableFeignClients键值队
		Map<String, Object> defaultAttrs = metadata
				.getAnnotationAttributes(EnableFeignClients.class.getName(), true);
// 如果EnableFeignClients配置了defaultConfiguration那么进行下一步操作，如果没有使用默认的FeignConfiguration
		if (defaultAttrs != null && defaultAttrs.containsKey("defaultConfiguration")) {
			String name;
			if (metadata.hasEnclosingClass()) {
				name = "default." + metadata.getEnclosingClassName();
			}
			else {
				name = "default." + metadata.getClassName();
			}
			registerClientConfiguration(registry, name,
					defaultAttrs.get("defaultConfiguration"));
		}
	}
// ，registerDefaultConfiguration方法会判断@EnableFeignClients注解是否设置了defaultConfiguration属性。如果有，则将调用registerClientConfiguration方法，进行BeanDefinitionRegistry的注册
private void registerClientConfiguration(BeanDefinitionRegistry registry, Object name,
			Object configuration) {
    // 使用BeanDefinitionBuilder来生成BeanDedinition，并注册到registry
		BeanDefinitionBuilder builder = BeanDefinitionBuilder
				.genericBeanDefinition(FeignClientSpecification.class);
		builder.addConstructorArgValue(name);
		builder.addConstructorArgValue(configuration);
		registry.registerBeanDefinition(
				name + "." + FeignClientSpecification.class.getSimpleName(),
				builder.getBeanDefinition());
	}
```

BeanDefinitionRegistry是Spring框架中用于动态注册BeanDefinition信息的接口，调用其registerBeanDefinition方法可以将BeanDefinition注册到Spring容器中，其中name属性就是注册BeanDefinition的名称.

FeignClientSpecification类实现了NamedContextFactory.Specification接口，它是OpenFeign组件实例化的重要一环，它持有自定义配置类提供的组件实例，供OpenFeign使用。Spring Cloud框架使用NamedContextFactory创建一系列的运行上下文（ApplicationContext），来让对应的Specification在这些上下文中创建实例对象。这样使得各个子上下文中的实例对象相互独立，互不影响，可以方便地通过子上下文管理一系列不同的实例对象。NamedContextFactory有三个功能，一是创建AnnotationConfigApplicationContext子上下文；二是在子上下文中创建并获取Bean实例；三是当子上下文消亡时清除其中的Bean实例。在OpenFeign中，FeignContext继承了NamedContextFactory，用于存储各类OpenFeign的组件实例。

[![cIyDYR.png](https://z3.ax1x.com/2021/04/18/cIyDYR.png)](https://imgtu.com/i/cIyDYR)

FeignAutoConfiguration是OpenFeign的自动配置类，它会提供FeignContext实例。并且将之前注册的FeignClientSpecification通过setConfigurations方法设置给FeignContext实例。这里处理了默认配置类FeignClientsConfiguration和自定义配置类的替换问题。如果FeignClientsRegistrar没有注册自定义配置类，那么configurations将不包含FeignClientSpecification对象，否则会在setConfigurations方法中进行默认配置类的替换。FeignAutoConfiguration的相关代码如下所示

```java
@Configuration
@ConditionalOnClass(Feign.class)
@EnableConfigurationProperties({FeignClientProperties.class, FeignHttpClientProperties.class})
public class FeignAutoConfiguration {

	@Autowired(required = false)
	private List<FeignClientSpecification> configurations = new ArrayList<>();

	@Bean
	public HasFeatures feignFeature() {
		return HasFeatures.namedFeature("Feign", Feign.class);
	}

	@Bean
	public FeignContext feignContext() {
		FeignContext context = new FeignContext();
		context.setConfigurations(this.configurations);
		return context;
	}
}
/////////////////FeignContext.java////////
/**
 创建伪装类实例的工厂。 它为每个客户端名称创建一个Spring ApplicationContext，并从那里提取所需的bean。
 * @author Spencer Gibb
 * @author Dave Syer
 */
public class FeignContext extends NamedContextFactory<FeignClientSpecification> {

	public FeignContext() {
        // 将默认的FeignClientConfiguration作为参数传递给构造函数
		super(FeignClientsConfiguration.class, "feign", "feign.client.name");
	}

}
```

NamedContextFactory是FeignContext的父类，其createContext方法会创建具有名称的Spring的AnnotationConfigApplicationContext实例作为当前上下文的子上下文。这些AnnotationConfigApplicationContext实例可以管理OpenFeign组件的不同实例。NamedContextFactory的实现如下代码所示

```java
public abstract class NamedContextFactory<C extends NamedContextFactory.Specification> implements DisposableBean, ApplicationContextAware {
    private Map<String, AnnotationConfigApplicationContext> contexts = new ConcurrentHashMap();
    private Map<String, C> configurations = new ConcurrentHashMap();
    private ApplicationContext parent;
    private Class<?> defaultConfigType;
    private final String propertySourceName;
    private final String propertyName;

    public NamedContextFactory(Class<?> defaultConfigType, String propertySourceName, String propertyName) {
        this.defaultConfigType = defaultConfigType;
        this.propertySourceName = propertySourceName;
        this.propertyName = propertyName;
    }

    public void setApplicationContext(ApplicationContext parent) throws BeansException {
        this.parent = parent;
    }

    public void setConfigurations(List<C> configurations) {
        Iterator var2 = configurations.iterator();

        while(var2.hasNext()) {
            C client = (NamedContextFactory.Specification)var2.next();
            this.configurations.put(client.getName(), client);
        }

    }

    public Set<String> getContextNames() {
        return new HashSet(this.contexts.keySet());
    }
	// 由于NamedContextFactory实现了DisposableBean接口，当NamedContextFactory实例消亡时，Spring框架会调用其destroy方法，清除掉自己创建的所有子上下文和自身包含的所有组件实例
    public void destroy() {
        Collection<AnnotationConfigApplicationContext> values = this.contexts.values();
        Iterator var2 = values.iterator();

        while(var2.hasNext()) {
            AnnotationConfigApplicationContext context = (AnnotationConfigApplicationContext)var2.next();
            context.close();
        }

        this.contexts.clear();
    }

    protected AnnotationConfigApplicationContext getContext(String name) {
        if (!this.contexts.containsKey(name)) {
            synchronized(this.contexts) {
                if (!this.contexts.containsKey(name)) {
                    this.contexts.put(name, this.createContext(name));
                }
            }
        }

        return (AnnotationConfigApplicationContext)this.contexts.get(name);
    }

    protected AnnotationConfigApplicationContext createContext(String name) {
        // 通过注解的形式获取上下文对象
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
        // 获取该name所对应的configuration，如果有的话就注册到Context中
        if (this.configurations.containsKey(name)) {
            Class[] var3 = ((NamedContextFactory.Specification)this.configurations.get(name)).getConfiguration();
            int var4 = var3.length;

            for(int var5 = 0; var5 < var4; ++var5) {
                Class<?> configuration = var3[var5];
                context.register(new Class[]{configuration});
            }
        }
		// 注册default的Configuration，也就是FeignClientRegister类的registerDefaultConfiguration方法中的注册的Configuration
        Iterator var9 = this.configurations.entrySet().iterator();

        while(true) {
            Entry entry;
            do {
                if (!var9.hasNext()) {
                    // 注册PropertyPlaceholderAutoConfiguration和FeignClientsConfiguration配置；类
                    context.register(new Class[]{PropertyPlaceholderAutoConfiguration.class, this.defaultConfigType});
                    // 设置子Context的Environment的propertySource属性源
                    context.getEnvironment().getPropertySources().addFirst(new MapPropertySource(this.propertySourceName, Collections.singletonMap(this.propertyName, name)));
                    // 所有的context的parent都相同，这样的话，一些相同的Bean可以通过parent context来获取
                    if (this.parent != null) {
                        context.setParent(this.parent);
                    }

                    context.setDisplayName(this.generateDisplayName(name));
                    context.refresh();
                    return context;
                }

                entry = (Entry)var9.next();
            } while(!((String)entry.getKey()).startsWith("default."));

            Class[] var11 = ((NamedContextFactory.Specification)entry.getValue()).getConfiguration();
            int var12 = var11.length;

            for(int var7 = 0; var7 < var12; ++var7) {
                Class<?> configuration = var11[var7];
                context.register(new Class[]{configuration});
            }
        }
    }
    ....
        .....
}
```

NamedContextFactory会创建出AnnotationConfigApplicationContext实例，并以name作为唯一标识，然后每个AnnotationConfigApplicationContext实例都会注册部分配置类，从而可以给出一系列的基于配置类生成的组件实例，这样就可以基于name来管理一系列的组件实例，为不同的FeignClient准备不同配置组件实例，比如说Decoder、Encoder等

###### 扫描类信息

FeignClientsRegistrar做的第二件事情是扫描指定包下的类文件，注册@FeignClient注解修饰的接口类信息。

```java
public void registerFeignClients(AnnotationMetadata metadata,
			BeanDefinitionRegistry registry) {
    // 生产自定义的ClassPathScanningCandidateComponentProvider
		ClassPathScanningCandidateComponentProvider scanner = getScanner();
		scanner.setResourceLoader(this.resourceLoader);

		Set<String> basePackages;
	// 获取EnableFeignClients所有属性的键值对
		Map<String, Object> attrs = metadata
				.getAnnotationAttributes(EnableFeignClients.class.getName());
    // 依照注解进行TypeFilter，只会扫描出被FeignClient修饰的类
		AnnotationTypeFilter annotationTypeFilter = new AnnotationTypeFilter(
				FeignClient.class);
		final Class<?>[] clients = attrs == null ? null
				: (Class<?>[]) attrs.get("clients");
    // 如果没有设置clients属性，那么需要扫描basePackage，所以设置了AnnotationTypeFilter并且去获取basePackage
		if (clients == null || clients.length == 0) {
			scanner.addIncludeFilter(annotationTypeFilter);
			basePackages = getBasePackages(metadata);
		}
		else {
            //设置了AnnotationTypeFilter并且去获取basePackage
			final Set<String> clientClasses = new HashSet<>();
			basePackages = new HashSet<>();
			for (Class<?> clazz : clients) {
				basePackages.add(ClassUtils.getPackageName(clazz));
				clientClasses.add(clazz.getCanonicalName());
			}
			AbstractClassTestingTypeFilter filter = new AbstractClassTestingTypeFilter() {
				@Override
				protected boolean match(ClassMetadata metadata) {
					String cleaned = metadata.getClassName().replaceAll("\\$", ".");
					return clientClasses.contains(cleaned);
				}
			};
			scanner.addIncludeFilter(
					new AllTypeFilter(Arrays.asList(filter, annotationTypeFilter)));
		}
		// 遍历获取到的basepackage列表
		for (String basePackage : basePackages) {
            // 获取basepackage列表下所有的BeanDefinition
			Set<BeanDefinition> candidateComponents = scanner
					.findCandidateComponents(basePackage);
			for (BeanDefinition candidateComponent : candidateComponents) {
				if (candidateComponent instanceof AnnotatedBeanDefinition) {
					// verify annotated class is an interface
					AnnotatedBeanDefinition beanDefinition = (AnnotatedBeanDefinition) candidateComponent;
					AnnotationMetadata annotationMetadata = beanDefinition.getMetadata();
					Assert.isTrue(annotationMetadata.isInterface(),
							"@FeignClient can only be specified on an interface");
					// 从BeanDefinition中获取FeignClient的属性值
					Map<String, Object> attributes = annotationMetadata
							.getAnnotationAttributes(
									FeignClient.class.getCanonicalName());

					String name = getClientName(attributes);
                    // 对于单独某个Feignclient的configuration进行配置
					registerClientConfiguration(registry, name,
							attributes.get("configuration"));
					// 注册FeignClient的BeanDefinition
					registerFeignClient(registry, annotationMetadata, attributes);
				}
			}
		}
	}

	private void registerFeignClient(BeanDefinitionRegistry registry,
			AnnotationMetadata annotationMetadata, Map<String, Object> attributes) {
		String className = annotationMetadata.getClassName();
		BeanDefinitionBuilder definition = BeanDefinitionBuilder
				.genericBeanDefinition(FeignClientFactoryBean.class);
		validate(attributes);
		definition.addPropertyValue("url", getUrl(attributes));
		definition.addPropertyValue("path", getPath(attributes));
		String name = getName(attributes);
		definition.addPropertyValue("name", name);
		definition.addPropertyValue("type", className);
		definition.addPropertyValue("decode404", attributes.get("decode404"));
		definition.addPropertyValue("fallback", attributes.get("fallback"));
		definition.addPropertyValue("fallbackFactory", attributes.get("fallbackFactory"));
		definition.setAutowireMode(AbstractBeanDefinition.AUTOWIRE_BY_TYPE);

		String alias = name + "FeignClient";
		AbstractBeanDefinition beanDefinition = definition.getBeanDefinition();

		boolean primary = (Boolean)attributes.get("primary"); // has a default, won't be null

		beanDefinition.setPrimary(primary);

		String qualifier = getQualifier(attributes);
		if (StringUtils.hasText(qualifier)) {
			alias = qualifier;
		}

		BeanDefinitionHolder holder = new BeanDefinitionHolder(beanDefinition, className,
				new String[] { alias });
		BeanDefinitionReaderUtils.registerBeanDefinition(holder, registry);
	}
```

FeignClientsRegistrar的registerFeignClients方法依据@EnableFeignClients的属性获取要扫描的包路径信息，然后获取这些包下所有被@FeignClient注解修饰的接口类的BeanDefinition，最后调用registerFeignClient动态注册BeanDefinition。registerFeignClients方法中有一些细节值得认真学习，有利于加深了解Spring框架。首先是如何自定义Spring类扫描器，即如何使用ClassPathScanningCandidateComponentProvider和各类TypeFilter。OpenFeign使用了AnnotationTypeFilter，来过滤出被@FeignClient修饰的类，getScanner方法的具体实现如下所示.

```java
// FeignClientsRegistrar.java
protected ClassPathScanningCandidateComponentProvider getScanner() {
		return new ClassPathScanningCandidateComponentProvider(false, this.environment) {
			@Override
			protected boolean isCandidateComponent(AnnotatedBeanDefinition beanDefinition) {
				boolean isCandidate = false;
                // 判断beanDefinition是否为内部类，否则直接返回false
				if (beanDefinition.getMetadata().isIndependent()) {
                    // 判断是否为接口类，所实现的接口只有一个，并且该接口是Annotation否则返回true
					if (!beanDefinition.getMetadata().isAnnotation()) {
						isCandidate = true;
					}
				}
				return isCandidate;
			}
		}
	}
```

ClassPathScanningCandidateComponentProvider的作用是遍历指定路径的包下的所有类。比如指定包路径为com/test/openfeign，它会找出com.test.openfeign包下所有的类，将所有的类封装成Resource接口集合。Resource接口是Spring对资源的封装，有FileSystemResource、ClassPathResource、UrlResource等多种实现。接着ClassPathScanning CandidateComponentProvider类会遍历Resource集合，通过includeFilters和excludeFilters两种过滤器进行过滤操作。includeFilters和excludeFilters是TypeFilter接口类型实例的集合，TypeFilter接口是一个用于判断类型是否满足要求的类型过滤器。excludeFilters中只要有一个TypeFilter满足条件，这个Resource就会被过滤掉；而includeFilters中只要有一个TypeFilter满足条件，这个Resource就不会被过滤。如果一个Resource没有被过滤，它会被转换成ScannedGenericBeanDefinition添加到BeanDefinition集合中

##### 实例初始化

FeignClientFactoryBean是工厂类，Spring容器通过调用它的getObject方法来获取对应的Bean实例。被@FeignClient修饰的接口类都是通过FeignClientFactoryBean的getObject方法来进行实例化的，具体实现如下代码所示：

```java

```

###### 扫描函数信息

######  生成Proxy接口类

##### 函数调用和网络请求





### 各组件深入之Spring Cloud Ribbon（负载均衡）

Ribbon 是 Netflix 公司的一个开源的负载均衡 项目，是一个客户端/进程内负载均衡器，运行在消费者端 。

其工作原理就是 Consumer 端获取到了所有的服务列表之后，在其内部 使用负载均衡算法 ，进行对多个系统的调用。



比如有一个项目部署了3个系统中如果不用负载均衡策略，那么没次请求都会达到第一个系统中，那么其余的系统就是摆设了。所以需要负载均衡策略，减少服务器的压力。

####  Ribbon 的几种负载均衡算法

负载均衡，不管 Nginx 还是 Ribbon 都需要其算法的支持，如果我没记错的话 Nginx 使用的是 轮询和加权轮询算法。而在 Ribbon 中有更多的负载均衡调度算法，其默认是使用的 RoundRobinRule 轮询策略。

- RoundRobinRule ：轮询策略。Ribbon 默认采用的策略。若经过一轮轮询没有找到可用的 provider，其最多轮询 10 轮。若最终还没有找到，则返回 null。
- RandomRule : 随机策略，从所有可用的 provider 中随机选择一个。
- RetryRule : 重试策略。先按照 RoundRobinRule 策略获取 provider，若获取失败，则在指定的时限内重试。默认的时限为 500 毫秒。

还有很多，这里不一一举了，你最需要知道的是默认轮询算法，并且可以更换默认的负载均衡算法，只需要在配置文件中做出修改就行。

```
providerName:  
  ribbon:  
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule  
```

当然，在 Ribbon 中你还可以自定义负载均衡算法 ，你只需要实现 IRule 接口，然后修改配置文件或者自定义 Java Config 类。

### 各组件深入之Spring Cloud Hystrix（断路器）

Hystrix是从Netflix API团队2011年开始的弹性工程工作中发展而来的。2012年，Hystrix继续发展和成熟，Netflix的许多团队都采用了它。今天，每天都有数百亿线程孤立的调用和数千亿信号量孤立的调用通过Hystrix在Netflix上执行。这极大地提高了正常运行时间和弹性

Netflix的创造了一个调用的库[Hystrix](https://github.com/Netflix/Hystrix)实现了[断路器图案](http://martinfowler.com/bliki/CircuitBreaker.html)。在微服务架构中，通常有多层服务调用

较低级别的服务中的服务故障可能导致用户级联故障。当对特定服务的呼叫达到一定阈值时（Hystrix中的默认值为5秒，20次故障），电路打开，不进行通话。在错误和开路的情况下，开发人员可以提供后备。

**Feign 是自带熔断器的，但默认是关闭的。需要在配置文件中配置打开它**

```
feign:
  hystrix:
    enabled: true
```

#### Hystrix工作原理

- 防止任何单个依赖项耗尽所有容器（例如Tomcat）用户线程。
- 减少负载并快速失败，而不是排队。
- 在可行的情况下提供备用，以保护用户免受故障的影响。
- 使用隔离技术（例如隔板，泳道和断路器模式）来限制任何一种依赖关系的影响。
- 通过近实时指标，监视和警报优化发现时间
- 通过在Hystrix的大多数方面中以低延迟传播配置更改来优化恢复时间，并支持动态属性更改，这使您可以通过低延迟反馈回路进行实时操作修改。
- 防止整个依赖客户端执行失败，而不仅仅是网络通信失败



讲到hystrix必须谈论到几个问题。**服务降级，服务容错**



#### Hystrix容错

Hystrix的容错主要是通过添加容许延迟和容错的方法，帮助控制这些分布式服务之间的交互。主要有以下几种容错方式：

资源隔离，熔断，降级

##### 资源隔离

资源隔离主要指对线程的隔离。Hystrix提供了两种线程隔离方式：线程池和信号量

**线程池**

通过自己线程池中的线程进行隔离的好处是：

- 该应用程序受到完全保护，不受客户端库的攻击。给定依赖库的池可以填满，而不会影响应用程序的其余部分。
- 该应用程序可以接受风险更低的新客户端库。如果发生问题，它将隔离到库中并且不会影响其他所有内容。
- 当出现故障的客户端再次恢复正常运行时，线程池将被清除，应用程序将立即恢复运行正常的性能，而整个Tomcat容器不堪重负的情况下，恢复时间很长。
- 如果客户端库配置错误，线程池的运行状况将迅速证明这一点（通过增加错误，延迟，超时，拒绝等），您可以在不影响应用程序功能的情况下进行处理（通常是通过动态属性实时进行）。 。
- 如果客户端服务更改了性能特征（通常会经常出现问题），从而又导致需要调整属性（增加/减少超时，更改重试次数等），则可以通过线程池指标（错误，延迟）再次看到该特征，超时，拒绝），并且可以在不影响其他客户端，请求或用户的情况下进行处理。
- 除了隔离优势之外，拥有专用的线程池还可以提供内置的并发性，可以利用这些并发性在同步客户端库之上构建异步外观（类似于Netflix API如何在Hystrix命令之上构建反应性，完全异步的Java API）。 。

简而言之，线程池提供的隔离允许客户端库和子系统性能特征的不断变化和动态组合得到优雅处理，而不会造成中断。

**注意：**尽管有单独的线程提供了隔离，但是您的基础客户端代码也应具有超时和/或对线程中断的响应，因此它不会无限期地阻塞并使Hystrix线程池饱和。

**线程池的缺点**

线程池的主要缺点是它们增加了计算开销。每个命令执行都涉及在单独线程上运行命令所涉及的队列，调度和上下文切换。

Netflix在设计此系统时，决定接受此间接费用，以换取其提供的收益，并认为它很小，不会对成本或性能造成重大影响。

**信号量**

您可以使用信号量（或计数器）将并发调用的数量限制为任何给定的依赖项，而不是使用线程池/队列大小。这使Hystrix无需使用线程池就可以减轻负载，但它不允许超时和退出。如果您信任客户端，并且只想减少负载，则可以使用这种方法。

`HystrixCommand`并`HystrixObservableCommand`在2个地方支持信号灯：

- **后备：**当Hystrix检索后备时，它总是在调用Tomcat线程上进行。
- **执行：**如果将该属性设置为`execution.isolation.strategy`，`SEMAPHORE`则Hystrix将使用信号量而不是线程来限制调用该命令的并发父线程的数量。

您可以通过定义可以执行多少个并发线程的动态属性来配置信号灯的这两种用法。您应该使用与调整线程池大小时类似的计算来确定它们的大小（在不到毫秒的时间内返回的内存中调用可以执行超过5000rps的操作，信号量仅为1或2…但默认值为10）。

**注意：**如果依赖关系被信号量隔离，然后变为潜在状态，则父线程将保持阻塞状态，直到基础网络调用超时为止。

一旦达到限制，信号灯拒绝将开始，但是填充信号灯的线程无法走开。

##### 熔断

Hystrix中的熔断器(Circuit Breaker)也是起类似作用，Hystrix在运行过程中会向每个commandKey对应的熔断器报告成功、失败、超时和拒绝的状态，熔断器维护并统计这些数据，并根据这些统计信息来决策熔断开关是否打开。如果打开，熔断后续请求，快速返回。隔一段时间（默认是5s）之后熔断器尝试半开，放入一部分流量请求进来，相当于对依赖服务进行一次健康检查，如果请求成功，熔断器关闭。

熔断机制是应对雪崩效应的一种微服务链路保户机制，当扇出链路的某个微服务不可用或者响应时间太长时，会进行服务的降级，进而熔断该节点微服务的调用，快速返回错误的相应信息。当检测当该节点微服务调用响应正常后恢复调用链路，熔断机制的注解是@HystrixCommand。

“熔断器”本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控，**某个异常，服务故障 被触发，直接熔断整个服务**。向调用方法返回一个符合预期的、可处理的备选响应(FallBack),而不是长时间的等待或者抛出吊牌用方法无法处理的异常，就保证了服务调用方的线程不会被长时间占用，避免故障在分布式系统中蔓延，乃至雪崩

使用时需开启服务熔断机制，启动类中加入@EnableCircuitBreaker。在调用方法中加入@HystrixCommand(fallbackMethod="test") // 这个指定了备选的响应，进行服务降级。 

```java
 	//正常的功能方法
    @GetMapping("/test/{id}")
    @HystrixCommand(fallbackMethod ="hystrix_test" )  //去找备选响应，进行服务降级
    public User findById(@PathVariable("id") Integer id) {
        User user = userService.findById(id);
        if (null == user){
            throw  new  RuntimeException("该"+id+"没有对应信息");
        }
        return user;
    }
    //备选响应，服务降级
    public User hystrix_test(@PathVariable("id") Integer id){
        return  new User(id,"该ID："+id+"没有对应的数据","Hystrix服务降级");
    }
```

现在在我所见到使用这种熔断的方式很少，一般都是直接使用的服务降级。

##### 降级

降级，通常指务高峰期，为了保证核心服务正常运行，需要停掉一些不太重要的业务，或者某些服务不可用时，执行备用逻辑从故障服务中快速失败或快速返回，以保障主体业务不受影响。Hystrix提供的降级主要是为了容错，保证当前服务不受依赖服务故障的影响，从而提高服务的健壮性。要支持回退或降级处理，可以重写HystrixCommand的getFallBack方法或HystrixObservableCommand的resumeWithFallback方法。

Hystrix在以下几种情况下会走降级逻辑：

- 执行construct()或run()抛出异常
- 熔断器打开导致命令短路
- 命令的线程池和队列或信号量的容量超额，命令被拒绝
- 命令执行超时

**自动降级**：超时、失败次数、故障、限流

 （1）配置好超时时间(异步机制探测回复情况)；

 （2）不稳的的api调用次数达到一定数量进行降级(异步机制探测回复情况)；

 （3）调用的远程服务出现故障(dns、http服务错误状态码、网络故障、Rpc服务异常)，直接进行降级。

**降级的回退方式**

**Fail Fast 快速失败**

快速失败是最普通的命令执行方法，命令没有重写降级逻辑。 如果命令执行发生任何类型的故障，它将直接抛出异常。

**Fail Silent 无声失败**

指在降级方法中通过返回null，空Map，空List或其他类似的响应来完成。

[hystrix详解](https://blog.csdn.net/loushuiyifan/article/details/82702522)

[hystrix的服务熔断和服务降级](https://www.cnblogs.com/guanyuehao0107/p/11848286.html)

[hystrix降级理解](https://www.cnblogs.com/qdhxhz/p/9581440.html)

### 各组件深入之Spring Cloud Zuul（服务网关）

Zuul 的主要功能是路由转发和过滤器。路由功能是微服务的一部分，比如 `/api/user` 转发到到 User 服务，`/api/shop` 转发到到 Shop 服务。Zuul 默认和 Ribbon 结合实现了负载均衡的功能。

Zuul是spring cloud中的微服务网关。网关： 是一个网络整体系统中的前置门户入口。请求首先通过网关，进行路径的路由，定位到具体的服务节点上。

　　Zuul是一个微服务网关，首先是一个微服务。也是会在Eureka注册中心中进行服务的注册和发现。也是一个网关，请求应该通过Zuul来进行路由。

　　Zuul网关不是必要的。是推荐使用的。

　　使用Zuul，一般在微服务数量较多（多于10个）的时候推荐使用，对服务的管理有严格要求的时候推荐使用，当微服务权限要求严格的时候推荐使用。



Zuul 是开源的微服务网关，可与 Eureka、Ribbon、Hystrix 等组件配合使用，**Zuul 它的核心是一系列过滤器**，这些过滤器可完成下面功能：

- 身份认证与安全：识别每个资源的验证要求，并拒绝那些要求不符合的请求
- 审核与监控：在边缘位置追踪有意义的数据和统计结果，从而带来精确的生产视图
- 动态路由：动态的将请求路由到不同的后端集群
- 压力测试：逐渐增加指向集群的流量，以了解性能
- 负载分配：为每一种负载类型分配对应容量，并弃用超出限定值的请求
- 静态响应处理：在边缘位置直接建立部分响应，从而避免转发到内部集群
- 多区域弹性：跨越 AWS Region 进行请求路由，实现 ELB 使用多样化，让系统边缘更贴近使用者

Spring Cloud 对 Zuul 进行了整合和增强，Zuul 默认使用的 HTTP 客户端是 Apache Http Client，也可使用 RestClient 或 okHttpClient

**服务网关基本功能**

智能路由：接收外部一切请求，并转发到后端的对外服务open-service上去；

​      注意：我们只转发外部请求，服务之间的请求不走网关，这就表示全链路追踪、内部服务API监控、内部服务之间调用的容错、智能路由不能在网关完成；

​       当然，也可以将所有的服务调用都走网关，那么几乎所有的功能都可以集成到网关中，但是这样的话，网关的压力会很大，不堪重负。

权限校验：可在微服务网关上进行认证，然后在将请求转发给微服务，无须每个微服务都进行认证，不校验服务内部的请求。服务内部的请求有必要校验吗？

 API监控：只监控经过网关的请求，以及网关本身的一些性能指标（例如，gc等）；

   限流：与监控配合，进行限流操作；

API日志统一收集：类似于一个aspect切面，记录接口的进入和出去时的相关日志

**zuul过滤器**

[参考：zuul过滤器](https://www.jianshu.com/p/29e9c91e3f3e)

### 各组件深入之Spring Cloud Config

对于分布式系统而言我们就不应该去每个应用下去分别修改配置文件，再者对于重启应用来说，服务无法访问所以直接抛弃了可用性，这是我们更不愿见到的。

那么有没有一种方法既能对配置文件统一地进行管理，又能在项目运行时动态修改配置文件呢？

那就是 Spring Cloud Config 。

Spring Cloud Config 为分布式系统中的外部化配置提供服务器和客户端支持。使用Config 服务器，可以在中心位置管理所有环境中应用程序的外部属性。

简单来说，Spring Cloud Config 就是能将各个 应用/系统/模块 的配置文件存放到 统一的地方然后进行管理



​      