## MQ

## RabbitMQ:

### RabbitMQ 的优点

- 基于 ErLang 语言开发具有高可用高并发的优点，适合辑群服务器
- 健壮、稳定、易用、跨平台、支持多种语言、文档齐全
- 有消息确认机制和持久化机制，可靠性高
- 开源

### RabbitMQ 的概念

#### 生产者和消费者

- Producer：消息的生产者
- Consumer：消息的消费者

#### Queue

- 消息队列，提供了 FIFO 的处理机制，具有缓存消息的能力。RabbitMQ 中，队列消息可以设置为持久化，临时或者自动删除。
- 设置为持久化的队列，Queue 中的消息会在 Server 本地硬盘存储一份，防止系统 Crash，数据丢失
- 设置为临时队列，Queue 中的数据在系统重启之后就会丢失
- 设置为自动删除的队列，当不存在用户连接到 Server，队列中的数据会被自动删除

#### ExChange

Exchange 类似于数据通信网络中的交换机，提供消息路由策略。RabbitMQ 中，Producer 不是通过信道直接将消息发送给 Queue，而是先发送给 ExChange。一个 ExChange 可以和多个 Queue 进行绑定，Producer 在传递消息的时候，会传递一个 ROUTING_KEY，ExChange 会根据这个 ROUTING_KEY 按照特定的路由算法，将消息路由给指定的 Queue。和 Queue 一样，ExChange 也可设置为持久化，临时或者自动删除

##### ExChange 的 4 种类型

- direct（默认）：直接交换器，工作方式类似于单播，ExChange 会将消息发送完全匹配 ROUTING_KEY 的 Queue（key 就等于 queue）
- fanout：广播是式交换器，不管消息的 ROUTING_KEY 设置为什么，ExChange 都会将消息转发给所有绑定的 Queue（无视 key，给所有的 queue 都来一份）
- topic：主题交换器，工作方式类似于组播，ExChange 会将消息转发和 ROUTING_KEY 匹配模式相同的所有队列（key 可以用“宽字符”模糊匹配 queue），比如，ROUTING_KEY 为 `user.stock` 的 Message 会转发给绑定匹配模式为 `* .stock,user.stock`， `* . *` 和 `#.user.stock.#` 的队列。（ * 表是匹配一个任意词组，# 表示匹配 0 个或多个词组）
- headers：消息体的 header 匹配，无视 key，通过查看消息的头部元数据来决定发给那个 queue（AMQP 头部元数据非常丰富而且可以自定义）

#### Binding

所谓绑定就是将一个特定的 ExChange 和一个特定的 Queue 绑定起来。ExChange 和 Queue 的绑定可以是多对多的关系

#### Virtual Host

在 RabbitMQ Server 上可以创建多个虚拟的 Message Broker，又叫做 Virtual Hosts (vhosts)。每一个 vhost 本质上是一个 mini-rabbitmq server，分别管理各自的 ExChange，和 bindings。vhost 相当于物理的 Server，可以为不同 app 提供边界隔离，使得应用安全的运行在不同的 vhost 实例上，相互之间不会干扰。Producer 和 Consumer 连接 rabbit server 需要指定一个 vhost

### RabbitMQ 的使用过程

- 客户端连接到消息队列服务器，打开一个 Channel。
- 客户端声明一个 ExChange，并设置相关属性。
- 客户端声明一个 Queue，并设置相关属性。
- 客户端使用 Routing Key，在 ExChange 和 Queue 之间建立好绑定关系。
- 客户端投递消息到 ExChange。
- ExChange 接收到消息后，就根据消息的 key 和已经设置的 binding，进行消息路由，将消息投递到一个或多个队列里

### RabbitMQ安装：

#### docker-compose.yml

```yml
version: '3.1'
services:
  rabbitmq:
    restart: always
    image: rabbitmq:management
    container_name: rabbitmq
    ports:
      - 5672:5672
      - 15672:15672
    environment:
      TZ: Asia/Shanghai
      RABBITMQ_DEFAULT_USER: rabbit
      RABBITMQ_DEFAULT_PASS: 123456
    volumes:
      - ./data:/var/lib/rabbitmq
```

#### 访问地址

http://ip:15672

[![BbiNPx.png](https://s1.ax1x.com/2020/11/09/BbiNPx.png)](https://imgchr.com/i/BbiNPx)

### RabbitMQ 使用

#### 生产者

创建一个名为 `spring-boot-amqp-provider` 的生产者项目

##### application.yml

```yml
spring:
  application:
    name: spring-boot-amqp
  rabbitmq:
    host: 192.168.75.133
    port: 5672
    username: rabbit
    password: 123456
```

##### 创建队列配置

```java
package com.lusifer.spring.boot.amqp.config;

import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class RabbitMQConfiguration {
    @Bean
    public Queue queue() {
        return new Queue("helloRabbit");
    }
}
```

##### 创建消息提供者

```java
package com.lusifer.spring.boot.amqp.provider;

import org.springframework.amqp.core.AmqpTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.util.Date;

@Component
public class HelloRabbitProvider {
    @Autowired
    private AmqpTemplate amqpTemplate;

    public void send() {
        String context = "hello" + new Date();
        System.out.println("Provider: " + context);
        amqpTemplate.convertAndSend("helloRabbit", context);
    }
}
```

##### 创建测试用例

```java
package com.lusifer.spring.boot.amqp.test;

import com.lusifer.spring.boot.amqp.Application;
import com.lusifer.spring.boot.amqp.provider.HelloRabbitProvider;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest(classes = Application.class)
public class AmqpTest {
    @Autowired
    private HelloRabbitProvider helloRabbitProvider;

    @Test
    public void testSender() {
        for (int i = 0; i < 10; i++) {
            helloRabbitProvider.send();
        }
    }
}
```

#### 消费者

创建一个名为 `spring-boot-amqp-consumer` 的消费者项目

##### application.yml

```xml
spring:
  application:
    name: spring-boot-amqp-consumer
  rabbitmq:
    host: 192.168.75.133
    port: 5672
    username: rabbit
    password: 123456
```

##### 使用.html#创建消息消费者)创建消息消费者

```java
package com.lusifer.spring.boot.amqp.consumer;

import org.springframework.amqp.rabbit.annotation.RabbitHandler;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

@Component
@RabbitListener(queues = "helloRabbit")
public class HelloRabbitConsumer {
    @RabbitHandler
    public void process(String message) {
        System.out.println("Consumer: " + message);
    }
}
```



### ActiveMQ:

ActiveMQ 是Apache出品，最流行的，能力强劲的开源消息总线。ActiveMQ 是一个完全支持JMS1.1和J2EE 1.4规范的 JMS Provider实现,尽管JMS规范出台已经是很久的事情了,但是JMS在当今的J2EE应用中间仍然扮演着特殊的地位。

####  主要特点：

1. 多种语言和协议编写客户端。语言: Java, C, C++, C#, Ruby, Perl, Python, PHP。应用协议: OpenWire,Stomp REST,WS Notification,XMPP,AMQP
2. 完全支持JMS1.1和J2EE 1.4规范 (持久化,XA消息,事务)
3. 对Spring的支持,ActiveMQ可以很容易内嵌到使用Spring的系统里面去,而且也支持Spring2.0的特性
4. 通过了常见J2EE服务器(如 Geronimo,JBoss 4, GlassFish,WebLogic)的测试,其中通过JCA 1.5 resource adaptors的配置,可以让ActiveMQ可以自动的部署到任何兼容J2EE 1.4 商业服务器上
5. 支持多种传送协议:in-VM,TCP,SSL,NIO,UDP,JGroups,JXTA
6. 支持通过JDBC和journal提供高速的消息持久化
7. 从设计上保证了高性能的集群,客户端-服务器,点对点
8. 支持Ajax
9. 支持与Axis的整合
10. 可以很容易得调用内嵌JMS provider,进行测试

#### ActiveMQ的消息形式

JMS定义了五种不同的消息正文格式，以及调用的消息类型，允许你发送并接收以一些不同形式的数据，提供现有消息格式的一些级别的兼容性。

- StreamMessage -- Java原始值的数据流
- MapMessage--一套名称-值对
- TextMessage--一个字符串对象
- ObjectMessage--一个序列化的 Java对象
-  BytesMessage--一个字节的数据流

一种是点对点的，即一个生产者和一个消费者一一对应；

另一种是发布/订阅模式，即一个生产者产生消息并进行发送后，可以由多个消费者进行接收。

#### 安装

> 1、需要jdk
> 2、安装Linux系统。生产环境都是Linux系统。
> 			第一步： 把ActiveMQ 的压缩包上传到Linux系统。<br/>
> 			第二步：解压缩。<br/>
> 			第三步：启动。<br/>
> 			使用bin目录下的activemq命令启动：<br/>
> 			[root@localhost bin]# ./activemq start
> 			关闭：<br/>
> 			[root@localhost bin]# ./activemq stop
> 			查看状态：<br/>
> 			[root@localhost bin]# ./activemq status
>
> ​			注意：如果ActiveMQ整合spring使用不要使用activemq-all-5.12.0.jar包。建议使用5.11.2
>
> ​			进入管理后台：<br/>
> ​			http://192.168.25.168:8161/admin<br/>
> ​			用户名：admin <br/>
> ​			密码：admin<br/>

#### 使用方法：

- ​		Producer--->Topic--->consumer
- ​		Producer---->queue--->Consumer

##### PQC之Producer

​		生产者：生产消息，发送端。
​		把jar包添加到工程中。使用5.11.2版本的jar包。

> ​	第一步：创建ConnectionFactory对象，需要指定服务端ip及端口号。
> ​	第二步：使用ConnectionFactory对象创建一个Connection对象。
> ​	第三步：开启连接，调用Connection对象的start方法。
> ​	第四步：使用Connection对象创建一个Session对象。
> ​	第五步：使用Session对象创建一个Destination对象（topic、queue），此处创建一个Queue对象。
> ​	第六步：使用Session对象创建一个Producer对象。
> ​	第七步：创建一个Message对象，创建一个TextMessage对象。
> ​	第八步：使用Producer对象发送消息。
> ​	第九步：关闭资源。

```java
@Test
public void testQueueProducer() throws Exception {
	// 第一步：创建ConnectionFactory对象，需要指定服务端ip及端口号。
	//brokerURL服务器的ip及端口号
	ConnectionFactory connectionFactory = new ActiveMQConnectionFactory("tcp://192.168.25.168:61616");
	// 第二步：使用ConnectionFactory对象创建一个Connection对象。
	Connection connection = connectionFactory.createConnection();
	// 第三步：开启连接，调用Connection对象的start方法。
	connection.start();
	// 第四步：使用Connection对象创建一个Session对象。
	//第一个参数：是否开启事务。true：开启事务，第二个参数忽略。
	//第二个参数：当第一个参数为false时，才有意义。消息的应答模式。1、自动应答2、手动应答。一般是自动应答。
	Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
	// 第五步：使用Session对象创建一个Destination对象（topic、queue），此处创建一个Queue对象。
	//参数：队列的名称。
	Queue queue = session.createQueue("test-queue");
	// 第六步：使用Session对象创建一个Producer对象。
	MessageProducer producer = session.createProducer(queue);
	// 第七步：创建一个Message对象，创建一个TextMessage对象。
	/*TextMessage message = new ActiveMQTextMessage();
	message.setText("hello activeMq,this is my first test.");*/
	TextMessage textMessage = session.createTextMessage("hello activeMq,this is my first test.");
	// 第八步：使用Producer对象发送消息。
	producer.send(textMessage);
	// 第九步：关闭资源。
	producer.close();
	session.close();
	connection.close();
}
```

##### PQC之Consumer

> 消费者：接收消息。
> 		第一步：创建一个ConnectionFactory对象。
> 		
> 		第二步：从ConnectionFactory对象中获得一个Connection对象。
> 		
> 		第三步：开启连接。调用Connection对象的start方法。
> 		
> 		第四步：使用Connection对象创建一个Session对象。
> 		
> 		第五步：使用Session对象创建一个Destination对象。和发送端保持一致queue，并且队列的名称一致。
> 		
> 		第六步：使用Session对象创建一个Consumer对象。
> 		
> 		第七步：接收消息。
> 		
> 		第八步：打印消息。
> 		
> 		第九步：关闭资源

```JAVA
@Test
public void testQueueConsumer() throws Exception {
	// 第一步：创建一个ConnectionFactory对象。
	ConnectionFactory connectionFactory = new ActiveMQConnectionFactory("tcp://192.168.25.168:61616");
	// 第二步：从ConnectionFactory对象中获得一个Connection对象。
	Connection connection = connectionFactory.createConnection();
	// 第三步：开启连接。调用Connection对象的start方法。
	connection.start();
	// 第四步：使用Connection对象创建一个Session对象。
	Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
	// 第五步：使用Session对象创建一个Destination对象。和发送端保持一致queue，并且队列的名称一致。
	Queue queue = session.createQueue("test-queue");
	// 第六步：使用Session对象创建一个Consumer对象。
	MessageConsumer consumer = session.createConsumer(queue);
	// 第七步：接收消息。
	consumer.setMessageListener(new MessageListener() {
		
		@Override
		public void onMessage(Message message) {
			try {
				TextMessage textMessage = (TextMessage) message;
				String text = null;
				//取消息的内容
				text = textMessage.getText();
				// 第八步：打印消息。
				System.out.println(text);
			} catch (JMSException e) {
				e.printStackTrace();
			}
		}
	});
	//等待键盘输入
	System.in.read();
	// 第九步：关闭资源
	consumer.close();
	session.close();
	connection.close();
}
```

##### PTC之Producer

> 使用步骤：
> 			第一步：创建ConnectionFactory对象，需要指定服务端ip及端口号。
> 			
> 			第二步：使用ConnectionFactory对象创建一个Connection对象。
> 			
> 			第三步：开启连接，调用Connection对象的start方法。
> 			
> 			第四步：使用Connection对象创建一个Session对象。
> 			
> 			第五步：使用Session对象创建一个Destination对象（topic、queue），此处创建一个Topic对象。
> 			
> 			第六步：使用Session对象创建一个Producer对象。
> 			
> 			第七步：创建一个Message对象，创建一个TextMessage对象。
> 			
> 			第八步：使用Producer对象发送消息。
> 			
> 			第九步：关闭资源。

```java
@Test
public void testTopicProducer() throws Exception {
	// 第一步：创建ConnectionFactory对象，需要指定服务端ip及端口号。
	// brokerURL服务器的ip及端口号
	ConnectionFactory connectionFactory = new ActiveMQConnectionFactory("tcp://192.168.25.168:61616");
	// 第二步：使用ConnectionFactory对象创建一个Connection对象。
	Connection connection = connectionFactory.createConnection();
	// 第三步：开启连接，调用Connection对象的start方法。
	connection.start();
	// 第四步：使用Connection对象创建一个Session对象。
	// 第一个参数：是否开启事务。true：开启事务，第二个参数忽略。
	// 第二个参数：当第一个参数为false时，才有意义。消息的应答模式。1、自动应答2、手动应答。一般是自动应答。
	Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
	// 第五步：使用Session对象创建一个Destination对象（topic、queue），此处创建一个topic对象。
	// 参数：话题的名称。
	Topic topic = session.createTopic("test-topic");
	// 第六步：使用Session对象创建一个Producer对象。
	MessageProducer producer = session.createProducer(topic);
	// 第七步：创建一个Message对象，创建一个TextMessage对象。
	/*
		* TextMessage message = new ActiveMQTextMessage(); message.setText(
		* "hello activeMq,this is my first test.");
		*/
	TextMessage textMessage = session.createTextMessage("hello activeMq,this is my topic test");
	// 第八步：使用Producer对象发送消息。
	producer.send(textMessage);
	// 第九步：关闭资源。
	producer.close();
	session.close();
	connection.close();
}
```



##### PTC之Consumer

> 消费者：接收消息。
> 			第一步：创建一个ConnectionFactory对象。
> 			
> 			第二步：从ConnectionFactory对象中获得一个Connection对象。
> 			
> 			第三步：开启连接。调用Connection对象的start方法。
> 			
> 			第四步：使用Connection对象创建一个Session对象。
> 			
> 			第五步：使用Session对象创建一个Destination对象。和发送端保持一致topic，并且话题的名称一致。
> 			
> 			第六步：使用Session对象创建一个Consumer对象。
> 			
> 			第七步：接收消息。
> 			
> 			第八步：打印消息。
> 			
> 			第九步：关闭资源

```java
@Test
public void testTopicConsumer() throws Exception {
	// 第一步：创建一个ConnectionFactory对象。
	ConnectionFactory connectionFactory = new ActiveMQConnectionFactory("tcp://192.168.25.168:61616");
	// 第二步：从ConnectionFactory对象中获得一个Connection对象。
	Connection connection = connectionFactory.createConnection();
	// 第三步：开启连接。调用Connection对象的start方法。
	connection.start();
	// 第四步：使用Connection对象创建一个Session对象。
	Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
	// 第五步：使用Session对象创建一个Destination对象。和发送端保持一致topic，并且话题的名称一致。
	Topic topic = session.createTopic("test-topic");
	// 第六步：使用Session对象创建一个Consumer对象。
	MessageConsumer consumer = session.createConsumer(topic);
	// 第七步：接收消息。
	consumer.setMessageListener(new MessageListener() {

		@Override
		public void onMessage(Message message) {
			try {
				TextMessage textMessage = (TextMessage) message;
				String text = null;
				// 取消息的内容
				text = textMessage.getText();
				// 第八步：打印消息。
				System.out.println(text);
			} catch (JMSException e) {
				e.printStackTrace();
			}
		}
	});
	System.out.println("topic的消费端03。。。。。");
	// 等待键盘输入
	System.in.read();
	// 第九步：关闭资源
	consumer.close();
	session.close();
	connection.close();
}
```


