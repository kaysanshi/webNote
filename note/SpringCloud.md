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


#### 模式：

​            聚合器微服务形式：:由api网关进行对其聚合

​            代理微服务设计模式:代理委派请求

​            链式微服务设计模式：链式同步调用，

​            分支微服务设计模式：同时允许两个微服务链，

​            数据共享微服务设计模式：SQL数据库反规范化可能会导致数据重复和不一致。

​            异步消息传递微服务设计模式：使用消息队列

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

1.Spring Cloud Netflix

​		API网关，zuul组件

​		服务注册与发现，Eureka

​		Fegin -> http Client -->http通信方式，同步阻塞

   	 熔断机制 Hystrix

2.Apache Dubbo Zookeeper

   	Dubbo是一个高效性能的 Java RPC 通信框架，2.6.x

   	服务注册与发现，Zookeeper，

   	API网关 没有  找第三方或自己实现。

  	服务挂了，Hystrix

3.Spring Cloud Alibaba

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

Eureka是一项基于REST（代表性状态转移）的服务，主要在AWS云中用于查找服务，以实现负载均衡和中间层服务器的故障转移。 我们称此服务为Eureka服务器服务注册与发现。 Eureka还带有一个基于Java的客户端组件Eureka Client，它使与服务的交互变得更加容易。 客户端还具有一个内置的负载平衡器，可以执行基本的循环负载平衡。 Netflix使用更复杂的负载均衡器将Eureka包装起来，以基于流量，资源使用，错误条件等多种因素提供加权负载均衡，以提供出色的弹性.

原理：主管服务注册与发现，也就是微服务的名称注册到Eureka，就可以通过Eureka找到微服务，而不需要修改服务调用的配置文件。



分析：Spring Cloud封装了Netflix公司开发的Eureka模块来实现服务的注册与发现，采用的c-s的设计架构，Eureka Server作为服务注册功能的服务器，他是服务注册中心。而系统的其他微服务，使用Eureka的客户端连接到Eureka Server并维持心跳。这样系统的维护人员可以通过Eureka Server来监控系统中的各个微服务是否正常运行。Spring Cloud的一些其他模块（比如Zuul）就可以通过Eureka Server来发现系统其他的微服务，并执行相关逻辑。

#### **Eureka Server**

Eureka Server提供服务注册服务，各个节点启动后，会在Eureka Server中进行注册， 这样Eureka Server中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观的看到。

#### **Eureka Client**

Eureka Client是一个Java客户端， 用于简化Eureka Server的交互，客户端同时也具备一个内置的、 使用轮询（round-robin）负载算法的负载均衡器。在应用启动后，将会向Eureka Server发送心跳（默认周期为30秒），以证明当前服务是可用状态 (30秒发送一次心跳更新租约。 如果客户端几次无法续签租约)。 如果Eureka Server在一定的时间（默认90秒）未收到客户端的心跳，Eureka Server将会从服务注册表中把这个服务节点移除。 任何区域的客户端都可以查找注册表信息（每30秒发生一次）以查找其服务（可能在任何区域）并进行远程调用。

#### **Eureka Server的自我保护机制**

如果在15分钟内超过85%的节点都没有正常的心跳，那么Eureka就认为客户端与注册中心出现了网络故障，此时会出现以下几种情况：

- Eureka不再从注册列表中移除因为长时间没收到心跳而应该过期的服务
- Eureka仍然能够接受新服务的注册和查询请求，但是不会被同步到其它节点上（即保证当前节点依然可用）
- 当网络稳定时，当前实例新的注册信息会被同步到其它节点中

因此， Eureka可以很好的应对因网络故障导致部分节点失去联系的情况，而不会像ZooKeeper那样使整个注册服务瘫痪。

#### **Eureka和ZooKeeper**

著名的CAP理论指出，一个分布式系统不可能同时满足C（一致性）、A（可用性）和P（分区容错性）。由于分区容错性在是分布式系统中必须要保证的，因此我们只能在A和C之间进行权衡。

##### **ZooKeeper保证CP**

当向注册中心查询服务列表时，我们可以容忍注册中心返回的是几分钟以前的注册信息，但不能接受服务直接down掉不可用。也就是说，服务注册功能对可用性的要求要高于一致性。但是ZooKeeper会出现这样一种情况，当Master节点因为网络故障与其他节点失去联系时，剩余节点会重新进行leader选举。问题在于，选举leader的时间太长，30 ~ 120s，且选举期间整个ZooKeeper集群都是不可用的，这就导致在选举期间注册服务瘫痪。在云部署的环境下，因网络问题使得ZooKeeper集群失去Master节点是较大概率会发生的事，虽然服务能够最终恢复，但是漫长的选举时间导致的注册长期不可用是不能容忍的。

##### **Eureka保证AP**

Eureka在设计时就优先保证可用性。Eureka各个节点都是平等的，几个节点挂掉不会影响正常节点的工作，剩余的节点依然可以提供注册和查询服务。而Eureka的客户端在向某个Eureka注册或时如果发现连接失败，则会自动切换至其它节点，只要有一台Eureka还在，就能保证注册服务可用（保证可用性），只不过查到的信息可能不是最新的（不保证强一致性）。

参考：https://www.jianshu.com/p/6a3db6939fb0

### 各组件深入之Spring Cloud openFeign

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

### 各组件深入之Spring Cloud Hystrix

### 各组件深入之Spring Cloud Zuul

### 各组件深入之Spring Cloud Config





​      