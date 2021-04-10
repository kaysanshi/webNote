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



#### **Eureka和ZooKeeper**

著名的CAP理论指出，一个分布式系统不可能同时满足C（一致性）、A（可用性）和P（分区容错性）。由于分区容错性在是分布式系统中必须要保证的，因此我们只能在A和C之间进行权衡。

##### **ZooKeeper保证CP**

当向注册中心查询服务列表时，我们可以容忍注册中心返回的是几分钟以前的注册信息，但不能接受服务直接down掉不可用。也就是说，服务注册功能对可用性的要求要高于一致性。但是ZooKeeper会出现这样一种情况，当Master节点因为网络故障与其他节点失去联系时，剩余节点会重新进行leader选举。问题在于，选举leader的时间太长，30 ~ 120s，且选举期间整个ZooKeeper集群都是不可用的，这就导致在选举期间注册服务瘫痪。在云部署的环境下，因网络问题使得ZooKeeper集群失去Master节点是较大概率会发生的事，虽然服务能够最终恢复，但是漫长的选举时间导致的注册长期不可用是不能容忍的。

##### **Eureka保证AP**

Eureka在设计时就优先保证可用性。Eureka各个节点都是平等的，几个节点挂掉不会影响正常节点的工作，剩余的节点依然可以提供注册和查询服务。而Eureka的客户端在向某个Eureka注册或时如果发现连接失败，则会自动切换至其它节点，只要有一台Eureka还在，就能保证注册服务可用（保证可用性），只不过查到的信息可能不是最新的（不保证强一致性）。

参考：https://www.jianshu.com/p/6a3db6939fb0

### 各组件深入之Spring Cloud openFeign（远程调用）

​			在微服务架构中，业务都会被拆分成一个独立的服务，服务与服务的通讯是基于HTTP RESTful的。Spring Cloud有两种服务调用方式，一种是Ribbon+RestTemplate，另一种是Feign。

​			Feign是声明性Web服务客户端。 它使编写Web服务客户端更加容易。 要使用Feign，请创建一个接口并对其进行注释。 它具有可插入注释支持，包括Feign注释和JAX-RS注释。 Feign还支持可插拔编码器和解码器。 Spring Cloud添加了对Spring MVC注释的支持，并支持使用Spring Web中默认使用的相同HttpMessageConverters。 Spring Cloud集成了Eureka和Spring Cloud LoadBalancer，以在使用Feign时提供负载平衡的http客户端。 就是通过把http请求封装到了注解中。

​			Spring Cloud 的 Feign 支持中的一个核心概念是命名客户机。每个佯装的客户机都是一个组件集合的一部分，这些组件一起工作，根据需要联系一个远程服务器，这个集合有一个名称，作为一个使用@feignclient 注释的应用程序开发人员，你可以给它一个名称。Spring Cloud 使用 FeignClientsConfiguration 根据需要为每个命名客户机创建一个新的集合，作为 ApplicationContext。其中包括一个假动作。解码器，一个假装。编码器，和一个假装。合约。可以使用@feignclient 注释的 contextId 属性覆盖集合的名称。

​			Hystrix 支持熔断(fallback)的概念: 一个默认的代码路径，在熔断或出现错误时执行。要为给定的@feignclient 启用熔断，请将熔断属性设置为实现熔断的类名。您还需要将实现声明为 springbean。

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