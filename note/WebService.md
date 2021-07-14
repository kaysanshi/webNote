## Web Service

### 概念

Web service是一个平台独立的，低耦合的，自包含的、基于可编程的web的应用程序，可使用开放的XML（标准通用标记语言下的一个子集）标准来描述、发布、发现、协调和配置这些应用程序，用于开发分布式的互操作的应用程序。

- Web Service技术， 能使得运行在不同机器上的不同应用无须借助附加的、专门的第三方软件或硬件， 就可相互交换数据或集成。依据Web Service规范实施的应用之间， 无论它们所使用的语言、 平台或内部协议是什么， 都可以相互交换数据。
- Web Service是自描述、 自包含的可用网络模块， 可以执行具体的业务功能。Web Service也很容易部署， 因为它们基于一些常规的产业标准以及已有的一些技术，诸如标准通用标记语言下的子集XML、HTTP。
- Web Service减少了应用接口的花费。Web Service为整个企业甚至多个组织之间的业务流程的集成提供了一个通用机制。
- Web Service是一种新的Web应用程序分支，其可以执行从简单的请求到复杂商务处理的任何功能。一旦部署以后，其他Web Service应用程序可以发现并调用它部署的服务。因此，Web Service是构造分布式、模块化应用程序和面向服务应用集成的最新技术和发展趋势。用于沟通不同平台，编程语言和组件模型中的不同的类型系统。webservcie 的请求都是post的

[![RTVXdA.png](https://z3.ax1x.com/2021/07/06/RTVXdA.png)](https://imgtu.com/i/RTVXdA)

### 核心技术

#### XML

- 可扩展的标记语言，是webService平台中表示数据的基本格式，易于建立和易于解析，与平台无关
- XML是由万维网协会(W3C)创建，W3C制定的XML SchemaXSD　定义了一套标准的数据类型，并给出了一种语言来扩展这套数据类型
- XML是由万维网协会(W3C)创建，W3C制定的XML SchemaXSD　定义了一套标准的数据类型，并给出了一种语言来扩展这套数据类型
- XML可以使web services十分方便的处理数据，它的内容与表示的分离十分理想

#### WSDL

Web Service描述语言WSDL　就是用机器能阅读的方式提供的一个正式描述文档而基于XML（标准通用标记语言下的一个子集）的语言，用于描述Web Service及其函数、参数和返回值。因为是基于XML的，所以WSDL既是机器可阅读的，又是人可阅读的。

一个 WSDL 文档的主要结构是类似这样的：

```xml
<definitions>
    <types>
    data type definitions........
    </types>

    <message>
    definition of the data being communicated....
    </message>

    <portType>
    set of operations......
    </portType>

    <binding>
    protocol and data format specification....
    </binding>

</definitions>
```

#### UUDI

UDDI 的目的是为电子商务建立标准；UDDI是一套基于Web的、分布式的、为Web Service提供的、信息注册中心的实现标准规范，同时也包含一组使企业能将自身提供的Web Service注册，以使别的企业能够发现的访问协议的实现标准。

#### SOAP协议

1998 年 XML 1.0 发布，被 W3C (World Wide Web Consortium) 推荐为标准的描述语言。同年，SOAP 也完成了初版设计。SOAP (Simple Object Access Protocol) 简单对象访问协议，在 1998 年因为微软 XML-RPC 的原因，还没有公之于众，一直到 2003 年 6 月的 SOAP 1.2 版本发布，才被 W3C 推荐。

SOAP即**简单对象访问协议**(Simple Object Access Protocol)，它是用于交换XML（标准通用标记语言下的一个子集）编码信息的轻量级协议。

SOAP使用XML消息调用远程方法，这样web services可以通过HTTP协议的post和get方法与远程机器交互，而且，SOAP更加健壮和灵活易用对于应用程序开发来说，使程序之间进行因特网通信是很重要的。目前的应用程序通过使用远程过程调用（RPC）在诸如 DCOM 与 CORBA 等对象之间进行通信，但是 HTTP 不是为此设计的。
RPC 会产生兼容性以及安全问题；防火墙和代理服务器通常会阻止此类流量。通过 HTTP 在应用程序间通信是更好的方法，因为 HTTP 得到了所有的因特网浏览器及服务器的支持。SOAP 就是被创造出来完成这个任务的。

SOAP 提供了一种标准的方法，使得运行在不同的操作系统并使用不同的技术和编程语言的应用程序可以互相进行通信。

##### 核心技术

SOAP采用了已经广泛使用的两个协议:HTTP 和[XML](https://baike.baidu.com/item/XML)（[标准通用标记语言](https://baike.baidu.com/item/标准通用标记语言)下的一个子集）。HTTP用于实现 SOAP 的RPC 风格的传输, 而XML 是它的编码模式。采用几行代码和一个XML 解析器, HTTP 服务器( MS 的 IIS 或 Apache) 立刻成为SOAP 的 ORBS。SOAP 通讯协议使用 HTTP 来发送XML 格式的信息。HTTP与RPC 的协议很相似,它简单、 配置广泛,并且对防火墙比其它协议更容易发挥作用。HTTP 请求一般由 Web 服务器软件(如 IIS 和Apache)来处理, 但越来越多的应用服务器产品正在支持HTTP。XML 作为一个更好的[网络数据](https://baike.baidu.com/item/网络数据)表达方式( NDR)。SOAP 把 XML 的使用代码化为请求和响应参数编码模式, 并用HTTP 作传输。具体地讲, 一个SOAP 方法可以简单地看作遵循SOAP编码规则的[HTTP请求](https://baike.baidu.com/item/HTTP请求)和响应, 一个 SOAP终端则可以看作一个基于HTTP 的URL, 它用来识别方法调用的目标。像CORBA/ IIOP一样, SOAP不需要具体的对象绑定到一个给定的终端, 而是由具体实现程序来决定怎样把对象终端标识符映像到服务器端的对象。

##### SOAP语法

一条 SOAP 消息就是一个普通的 XML 文档，包含下列元素：

- 必需的 Envelope 元素，可把此 XML 文档标识为一条 SOAP 消息
- 可选的 Header 元素，包含头部信息
- 必需的 Body 元素，包含所有的调用和响应信息
- 可选的 Fault 元素，提供有关在处理此消息所发生错误的信息

```xml
Http协议：请求头和请求体之间必须有空行
    post /XXX.action HTTP/1.1
    HOST www.kay555.cn
    contenType taxt/html;charset=utf8
    id=111&name=xxx&....
SOAP协议：
    <envelope>
        <body>
        </body>
    </envelope>
<SOAP-ENV:Envelope 各种属性>
<!--百度百科示例-->
　<SOAP:HEADER>
　</SOAP:HEADER>
　<SOAP:Body>
　</SOAP:Body>
</SOAP-ENV:Envelope>
```

##### 协议约定

SOAP 的协议约定用的是 WSDL (Web Service Description Language) ，这是一种 Web 服务描述语言，在服务的客户端和服务端开发者不用面对面交流，只要用的是 WSDL 定义的格式，客户端知道了 WSDL 文件，就知道怎么去封装请求，调用服务。

```xml
<wsdl:types>
 <xsd:schema targetNamespace="http://www.task.io/management">
  <xsd:complexType name="task">
   <xsd:element name="name" type="xsd:string"></xsd:element>
  <xsd:element name="type" type="xsd:string"></xsd:element>
   <xsd:element name="priority" type="xsd:int"></xsd:element>
  </xsd:complexType>
 </xsd:schema>
</wsdl:types>
```

一套完整的 WSDL 还会有消息结构体的定义，然后将信息绑定到 SOAP 请求的 body 里，然后编写成 service，具体这里就不展开了。

WSDL 有可以自动生成 Stub 的工具，客户端可以直接通过自动生成的 Stub 去调用服务端

##### 传输协议

SOAP 是用 HTTP 进行传输的，有个信封的概念，信息就像是一封信，有 Header 和 Body，SOAP 的请求和回复都放在信封里，进行传递。

```
    post /XXX.action HTTP/1.1
    HOST www.kay555.cn
    contenType taxt/html;charset=utf8
```

```xml
<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://www.w3.org/2001/12/soap-envelope"
soap:encodingStyle="http://www.w3.org/2001/12/soap-encoding">
    <soap:Header>
        <m:Trans xmlns:m="http://www.w3schools.com/transaction/"
          soap:mustUnderstand="1">1234
        </m:Trans>
    </soap:Header>
    <soap:Body xmlns:m="http://www.task.io/management">
        <m:addTask">
            <task>
                <name>Write a blog article</name>
                <type>Writing</type>
                <priority>1</priority>
            </task>
        </m:addTask>
    </soap:Body>
</soap:Envelope>
```

##### 服务发现

SOAP 的服务发现用的是 **UDDI**（Universal Description, Discovery, Integration) 统一描述发现集成，相当于一个注册中心，服务提供方将 WSDL 文件发布到注册中心，使用方可以到这个注册中心查找。

##### 优缺点

###### 优点

1. 协议约定面向对象，更贴合业务逻辑的应用场景。
2. 服务定义清楚，在 WSDL 能清楚了解到所有服务。
3. 格式不用完全一致，比如上面那个请求里 name, type, priority 的顺序不用完全跟服务端的 WSDL 对应。版本更新上，客户端可以先增加新的项，服务端可以之后再更新。
4. 使用 WS Security 所为安全标准，安全性较高。
5. SOAP 是 **面向动作** 的，支持比较复杂的动作，可以在 XML 里放热和动作，比如 ADD，MINUS

###### 缺点

1. 远程调用速度慢，效率低。因为以 XML 作为数据格式，除了主要传输的数据之外，有较多冗余用在定义格式上，占用带宽，并且对于 XML 的序列化和解析的速度也比较慢
2. 协议约定 WSDL 比较复杂，要经过好几个环节才能搞定。
3. SOAP 多数用的是 POST，而 HTTP 有 GET， DELETE，PUT 等很多别的方法，通常是 POST 加上动作，比如 POST CreateTask, POST DeleteTask。而多数用 POST 的原因是 GET 请求最大长度限制较多，而 SOAP 需要把数据加上 SOAP 标准化的格式，请求数据比较大，超过 GET 的限制。
4. SOAP 的业务状态大多是维护在服务端的，比如说分页，服务端会记住用户在哪个页面上，在企业软件中，客户端和服务端比较平衡的情况下是没有问题的，但是在失衡情况下，比如说客户端请求大大超过服务端时，服务端维护所有状态的成本太高，影响并发量。

针对 SOAP 速度慢的缺点，直接的有两种解决方案，一是使用简单一点的文本方式，比如 JSON，二是采用二进制调用

### JAX-WS & JAX-RS

WebService提供了两大类解决方案JAX-WS(基于SOAP协议的RPC-style,和基于SOAP协议的文档style),JAX-RS(基于ReastFul的Jersey和restTeasy)

#### JAX-WS（JX-RPC）

**Java API for XML Web Services（JAX-WS）**是Java程序设计语言一个用来创建Web服务的API。

JAX-WS是sun的Java企业平台一部分。和其它Java EE的API一样，JAX-WS使用了Java SE5引入的Java标注机制来简化Web服务客户端和服务端的开发和部署用于SOAP Web服务。 编写JAX-WS应用程序代码的方法有两种：**RPC样式**和**Document样式**。

JAX-RPC可以被看作是Java RMI在Web服务协议上的实现允许Java应用程序可以通过已知的描述信息调用一个基于Java的Web服务，描述信息与Web服务的WSDL（Web服务描述语言）描述相一致. **JAX-RPC 2.0被更名为JAX-WS 2.0.**JAX-WS2.0 (JSR 224)是Sun新的web services协议栈，是一个完全基于标准的实现。在binding层，使用的是the Java Architecture for XML Binding (JAXB, JSR 222)，在parsing层，使用的是the Streaming API for XML (StAX, JSR 173)，同时它还完全支持schema规范。

在服务器端，用户只需要通过Java语言定义远程调用所需要实现的接口SEI (service endpoint interface)，并提供相关的实现，通过调用JAX-WS的服务发布接口就可以将其发布为WebService接口。

在客户端，用户可以通过JAX-WS的API创建一个代理（用本地对象来替代远程的服务）来实现对于远程服务器端的调用。

当然 JAX-WS 也提供了一组针对底层消息进行操作的API调用，你可以通过Dispatch 直接使用SOAP消息或XML消息发送请求或者使用Provider处理SOAP或XML消息。

通过web service所提供的互操作环境，我们可以用JAX-WS轻松实现JAVA平台与其他编程环境(.net等)的互操作。

##### JAX-WS(RPC样式)

- PC样式的Web服务使用方法名称和参数来生成XML结构。
- RPC样式生成的WSDL很难针对模式进行验证。
- 在RPC样式中，SOAP消息被发送为多个元素。
- RPC样式消息紧密耦合。
- 在RPC样式中，SOAP消息保留操作名称。在RPC样式中，参数作为离散值发送

使用示例：JAX-WS API内置于JDK中，因此无需为其加载任何额外的jar文件

```java
import javax.jws.WebMethod;
import javax.jws.WebService;
import javax.jws.soap.SOAPBinding;

/**
* @user:
* @date:2021/7/6
* @Description:
*/            
@WebService
@SOAPBinding(style = SOAPBinding.Style.RPC)
public interface HelloWord {
    @WebMethod
    String getHelloWorldAsString(String name);

}
@WebService(endpointInterface = "com.blueearth.bewemp.doc.test.HelloWord")
public class HelloWordImpl implements HelloWord{
    @Override
    public String getHelloWorldAsString(String name) {
        return "hello word jax-ws"+name;
    }
}
public class Publisher {
    public static void main(String[] args) {
        Endpoint.publish("http://localhost:1212/ws/hello",new HelloWordImpl());
    }
}
```

 **运行之后可以看到下图**

运行访问localhost:1212/ws/hello?wsdl

[![RoSi8A.png](https://z3.ax1x.com/2021/07/06/RoSi8A.png)](https://imgtu.com/i/RoSi8A)

```java
/**
* @user:
* @date:2021/7/6
* @Description:
*/		
public class Client {
    public static void main(String[] args) throws MalformedURLException {
        URL url = new URL("http://localhost:1212/ws/hello?wsdl");

        // 1st argument service URI, refer to wsdl document above
        // 2nd argument is service name, refer to wsdl document above
        QName qname = new QName("http://test.doc.bewemp.blueearth.com/", "HelloWorldImplService");
        Service service = Service.create(url, qname);
        HelloWord hello = service.getPort(HelloWord.class);
        System.out.println(hello.getHelloWorldAsString("jk rpc"));
    }
}
/***
* output - hello word jax-wsjk rpc
*/
```

##### JAX-WS(Document样式)

- 可以根据预定义模式验证文档样式Web服务。
- 在文档样式中，SOAP消息作为单个文档发送。
- 文档样式消息松散耦合。
- 在文档样式中，SOAP消息中不需要操作名称。
- 在文档样式中，参数以XML格式发送。

看代码：只需要将style 改为Doument就可以

```java
@WebService
@SOAPBinding(style = SOAPBinding.Style.DOCUMENT)
public interface HelloWord {
    @WebMethod
    String getHelloWorldAsString(String name);
}
```

运行访问localhost:1212/ws/hello?wsdl

[![RonCcT.png](https://z3.ax1x.com/2021/07/06/RonCcT.png)](https://imgtu.com/i/RonCcT)

```java
// 进行测试：
public class Client {
    public static void main(String[] args) throws MalformedURLException {
        URL url = new URL("http://localhost:1212/ws/hello?wsdl");

        // 1st argument service URI, refer to wsdl document above
        // 2nd argument is service name, refer to wsdl document above
        QName qname = new QName("http://test.doc.bewemp.blueearth.com/", "HelloWordImplService");
        Service service = Service.create(url, qname);
        HelloWord hello = service.getPort(HelloWord.class);
        System.out.println(hello.getHelloWorldAsString(" web service document"));
    }
}
// out-put: hello word jax-ws web service document
```

#### JAX-RS

JAX-RS是JAVA EE6 引入的一个新技术。 JAX-RS即Java API for RESTful Web Services，是一个Java 编程语言的应用程序接口，支持按照表述性状态转移（REST）架构风格创建Web服务。

JAX-RS使用了Java SE5引入的Java注解来简化Web服务的客户端和服务端的开发和部署。用于SOAP Web服务。 编写JAX-WS应用程序代码的方法有两种：RPC样式和Document样式。

JAX-RS提供了一些注解将一个资源类，一个POJO Java类，封装为Web资源。

@Path，标注资源类或者方法的相对路径

@GET，@PUT，@POST，@DELETE，标注方法是HTTP请求的类型。

@Produces，标注返回的MIME媒体类型

```
@Produces("text/plain"): 用于下载文本文件。
@Produces("image/png"): 用于下载图片文件。
@Produces("application/pdf"): 用于下载PDF文件。
@Produces("application/vnd.ms-excel"): 用于下载Excel文件。
@Produces("application/msword"): 用于下载Word文件
```


@Consumes，标注可接受请求的MIME媒体类型

@PathParam，@QueryParam，@HeaderParam，@CookieParam，@MatrixParam，@FormParam,分别标注方法的参数来自于HTTP请求的不同位置，例如@PathParam来自于URL的路径，@QueryParam来自于URL的查询参数，@HeaderParam来自于HTTP请求的头信息，@CookieParam来自于HTTP请求的Cookie。

基于JAX-RS实现的框架有Jersey，RESTEasy等。这两个框架创建的应用可以很方便地部署到Servlet 容器中.

```java
	@Path("/hello")  
	public class HelloService{  
		@GET  
		@Path("/{param}")  
		public Response getMsg(@PathParam("param") String msg) {  
			String output = "Jersey say : " + msg;  
			return Response.status(200).entity(output).build();  
		}  
	}
	@GET  
    @Path("{year}/{month}/{day}")  
    public Response getDate(  
            @PathParam("year") int year,  
            @PathParam("month") int month,   
            @PathParam("day") int day) {  

       String date = year + "/" + month + "/" + day;  

       return Response.status(200)  
        .entity("getDate is called, year/month/day : " + date)  
        .build();  
    }  
```

配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
	<servlet>
		<servlet-name>Jersey REST Service</servlet-name>
		<servlet-class>org.glassfish.jersey.servlet.ServletContainer</servlet-class>
		<init-param>
			<param-name>jersey.config.server.provider.packages</param-name>
			<param-value>com.kaysanshi</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>Jersey REST Service</servlet-name>
		<url-pattern>/rest/*</url-pattern>
	</servlet-mapping>
	</web-app>
```

测试代码：

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>JAX-RS Server示例</title>
	</head>
	<body>
		<a href="rest/hello/aa">JAX-RS注解参数示例</a>
		<a href="rest/hello/2020/07/01">JAX-RS注解多参数日期示例</a>
	</body>
</html>
```

参考：

[Building Web Services with JAX-WS](https://docs.oracle.com/javaee/6/tutorial/doc/bnayl.html)

[Building RESTful Web Services with JAX-RS](https://docs.oracle.com/javaee/6/tutorial/doc/giepu.html)

### Apach CXF

Apache CXF = Celtix + XFire，开始叫 Apache CeltiXfire，后来更名为 Apache CXF 了，以下简称为 CXF。
CXF 继承了 Celtix 和 XFire 两大开源项目的精华，提供了对 JAX-WS 全面的支持，并且提供了多种 Binding 、DataBinding、Transport 以及各种 Format 的支持，并且可以根据实际项目的需要，采用代码优先（Code First）或者 WSDL 优先（WSDL First）来轻松地实现 Web Services 的发布和使用。Apache CXF已经是一个正式的Apache顶级项目。

#### cxf服务端：

导包：配置cxf.xml，书写接口和实现类。在web.xml配置：

```xml

		 <!-- 配置CXF提供的servlet -->
		  <servlet>
				<servlet-name>cxf</servlet-name>
				<servlet-class>org.apache.cxf.transport.servlet.CXFServlet</servlet-class>
				<!-- 通过初始化参数指定CXF配置文件位置 -->
				<init-param>
					<param-name>config-location</param-name>
					<param-value>classpath:cxf.xml</param-value>
				</init-param>
		  </servlet>
		  <servlet-mapping>
				<servlet-name>cxf</servlet-name>
				<url-pattern>/service/*</url-pattern>
		  </servlet-mapping>
在接口上添加注解@WebService然后提供对外访问的方法：
在cxf.xml配置：
		 <!--配置bean-->
			<bean id="helloService" class="com.leo.service.HelloServiceImpl"></bean>
			<!-- 注册服务  
			<url-pattern>/service/*</url-pattern>与下面访问的路径有关
			http://ip:port/projectName/service/address
			address对应的是下面的address的配置
			-->
			<jaxws:server id="myService" address="/hello">
				<jaxws:serviceBean>
					<ref bean="helloService"></ref>
				</jaxws:serviceBean>
			</jaxws:server>
```

#### CXF客户端

通过wsimport -s . http://10.33.8.112:8080/hello?wsdl 解析出来生成java代码，然后创建代理对象，通过代理对象实现远程调用把生成的代码中接口java文件引入：
**配置cxf.xml:**

```xml
<!-- 注册客户端代理对象，通过spring框架创建代理对象，使用代理对象实现远程调用 -->
		<jaxws:client id="myclient"
			address="http://10.33.8.112:8080/cxf_service/service/hello"
			serviceClass="com.leo.client.HelloService"
		> </jaxws:client>
```

**测试代码**

```java
/**
* 测试
* @author leoi555
* 
*/
public class App {
public static void main(String[] args) {
    	ApplicationContext context=new ClassPathXmlApplicationContext("cxf.xml");
        HelloService service=(HelloService) context.getBean("myclient");
        String ret=service.sayHello("cxf:::");
        System.out.println(ret);
    }
}
```

#### CXF服务端web版：

```xml
只需要在maven项目中加入以下依赖：
			<!-- https://mvnrepository.com/artifact/org.apache.cxf/cxf-rt-frontend-jaxws -->
					<dependency>
					    <groupId>org.apache.cxf</groupId>
					    <artifactId>cxf-rt-frontend-jaxws</artifactId>
					    <version>3.2.6</version>
					</dependency>

					<!-- https://mvnrepository.com/artifact/org.apache.cxf/cxf-rt-transports-http -->
					<dependency>
					    <groupId>org.apache.cxf</groupId>
					    <artifactId>cxf-rt-transports-http</artifactId>
					    <version>3.2.0</version>
					</dependency>
					
					<!-- https://mvnrepository.com/artifact/org.apache.cxf/cxf-core -->
					<dependency>
					    <groupId>org.apache.cxf</groupId>
					    <artifactId>cxf-core</artifactId>
					    <version>3.2.5</version>
					</dependency>
					<dependency>
				<groupId>wsdl4j</groupId>
				<artifactId>wsdl4j</artifactId>
				<version>1.6.2</version>
				</dependency>
					<!-- https://mvnrepository.com/artifact/org.apache.cxf/cxf-api -->
					<dependency>
					    <groupId>org.apache.cxf</groupId>
					    <artifactId>cxf-api</artifactId>
					    <version>2.7.18</version>
					</dependency>
```

```xml
 <!-- 配置CXF提供的servlet -->
		  <servlet>
				<servlet-name>cxf</servlet-name>
				<servlet-class>org.apache.cxf.transport.servlet.CXFServlet</servlet-class>
				<!-- 这里直接配置到spring的上下文中不再进行对cxf配置的读取 -->
				<!-- 通过初始化参数指定CXF配置文件位置
				<init-param>
					<param-name>config-location</param-name>
					<param-value>classpath:cxf.xml</param-value>
				</init-param> -->
		  </servlet>
		  <!-- 配置映射文件 -->
		  <servlet-mapping>
				<servlet-name>cxf</servlet-name>
				<url-pattern>/service/*</url-pattern>
		  </servlet-mapping>
		<filter>
		    <filter-name>struts2</filter-name>
		    <filter-class>
			 org.apache.struts2.dispatcher.filter.StrutsPrepareAndExecuteFilter
		      </filter-class>
		  </filter>
		<filter-mapping>
		    <filter-name>struts2</filter-name>
		    <url-pattern>/struts/*</url-pattern>
			<url-pattern>/frame/*</url-pattern>
			<url-pattern>*.action</url-pattern>
			<url-pattern>*.jsp</url-pattern>
		  </filter-mapping>

		  然后把cxf的配置放到struts配置上面
		  服务端注册服务：
			<!-- 注册服务  
			<url-pattern>/service/*</url-pattern>与下面访问的路径有关
			http://ip:port/projectName/service/address
			address对应的是下面的address的配置-->
			
			<jaxws:server id="customerServer" address="/customer">
				<jaxws:serviceBean>
					<ref bean="customerService"></ref>
				</jaxws:serviceBean>
			</jaxws:server>
然后访问这个路径即可以得到 vmimport -s . http://10.33.8.112:8080/ssh_Crm/service/customer?wsdl
		 D:\test>wsimport  -s  .  http://10.33.8.112:8080/hello?wsdl 
		 然后生成java代码在客户端引入其接口：
		注册客户端代理对象：
		<!-- 注册crm客户端的代理对象 -->
			<jaxws:client id="crmClient" serviceClass="com.leo.crm.CustomerService" address="http://localhost:8080/ssh_Crm/service/customer">
				
			</jaxws:client>
```

### RPC

在网络世界里，不同机器要怎么互相通信？最基础的方法是基于 TCP/IP 通过 Socket 编程去实现调用方和被调用方。但是 Socket 编程的难度大，需要比较强的专业性，实现又复杂，如果每一次机器之间要通信时，程序员都要手动去处理这么多，这就让新手能做的，变成了要精通网络的老师傅才能完成。有没有什么更好的办法呢？

在 1984 年，Bruce Jay Nelson 发表了奠定基础性的论文 [Implementing Remote Procedure Call](https://link.zhihu.com/?target=http%3A//www.cs.cmu.edu/~dga/15-712/F07/papers/birrell842.pdf)，定义了机器之间互通这种远程调用的标准。RPC (Remote Procedure Call) 即远程过程调用，有了它，客户端可以像调用本地接口一样调用远程的服务端

#### 什么是RPC

RPC（Remote Procedure Call）—远程过程调用，它是一种通过网络从远程计算机程序上请求服务，而不需要了解底层网络技术的协议。比如两个不同的服务 A、B 部署在两台不同的机器上，那么服务 A 如果想要调用服务 B 中的某个方法该怎么办呢？使用 *HTTP请求* 当然可以，但是可能会比较慢而且一些优化做的并不好。 RPC 的出现就是为了解决这个问题。

最终解决的问题：**让分布式或者微服务系统中不同服务之间的调用像本地调用一样简单**

#### RPC核心功能

一个 RPC 的核心功能主要有 5 个部分组成，分别是：客户端、客户端 Stub、网络传输模块、服务端 Stub、服务端等。

[![Ro05LQ.jpg](https://z3.ax1x.com/2021/07/06/Ro05LQ.jpg)](https://imgtu.com/i/Ro05LQ)

下面分别介绍核心 RPC 框架的重要组成：

- 客户端(Client)：服务调用方。
- 客户端存根(Client Stub)：存放服务端地址信息，将客户端的请求参数数据信息打包成网络消息，再通过网络传输发送给服务端。
- 服务端存根(Server Stub)：接收客户端发送过来的请求消息并进行解包，然后再调用本地服务进行处理。
- 服务端(Server)：服务的真正提供者。
- Network Service：底层传输，可以是 TCP 或 HTTP。

#### RPC调用过程

1. 服务消费方（client）调用以本地调用方式调用服务；
2. client stub接收到调用后负责将方法、参数等组装成能够进行网络传输的消息体；
3. client stub找到服务地址，并将消息发送到服务端；
4. server stub收到消息后进行解码；
5. server stub根据解码结果调用本地的服务；
6. 本地服务执行并将结果返回给server stub；
7. server stub将返回结果打包成消息并发送至消费方；
8. client stub接收到消息，并进行解码；
9. 服务消费方得到最终结果。

#### RPC解决的问题

一个 RPC 框架基本需要解决 协议约定、网络传输、服务发现 这三个问题。

**协议约定问题** 指的是怎么规定远程调用的语法，怎么传参数等。

**传输协议问题** 指的是在网络发生错误、重传、丢包或者有性能问题时怎么办？

**服务发现问题** 指的是如果知道服务端有哪些服务可以调用，从哪个端口访问？服务端可能实现多个远程调用，在不同的进程上，随机监听端口，客户端要怎么才能知道这些端口呢

#### 常见的RPC框架

RPC 是一种技术思想而非一种规范或协议，常见 RPC 技术和框架有：

- 应用级的服务框架：阿里的 Dubbo/Dubbox、Google gRPC、Spring Boot/Spring Cloud。
- 远程通信协议：RMI、Socket、SOAP(HTTP XML)、REST(HTTP JSON)。
- 通信框架：MINA 和 Netty。

##### ONC RPC

ONC RPC 算是最早 RPC 的一种实现方式，由 Sun 公司开发，在 NFS (Network File System) 中使用。

###### 协议约定

ONC RPC 协议用的是 XDR (External Data Representation)，是一个标准的**数据压缩格式**，所有的请求和回复都有严格的格式要求，并且不能随意更改。

###### 传输协议

ONC RPC 传输方面主要是通过一个状态机来实现。

###### 服务发现

ONC RPC 通过服务端的一个 portmapper 来实现服务发现。服务端在启动时向 portmapper 注册，portmapper 的端口是大家都知道的，所以客户端可以通过 portmapper 找到服务端。

ONC RPC 作为最早的 RPC 框架，还是有很多问题的

1. 协议格式要求严格：需要客户端和服务端的压缩格式**完全一致**。
2. 协议修改不灵活：客户端和服务端都要做修改，如果只有一方做了修改， 那 RPC 就会有错误。这导致版本更新的问题，每一次的版本更新，客户端和服务端基本是耦合的，必须同时作出更改，这就要求开发客户端和服务端的需要是同一批人，或者至少要有密切的交流。
3. 面向函数：随着发展，面向对象是业务逻辑设计的主流，面向函数的 RPC 模式难用。

##### SOAP

soap可以看上面的

##### gRPC

像 SOAP 这类基于文本类的 RPC 框架，速度上都是有先天不足的。为了有比较好的性能，还是得用二进制的方式进行远程调用。gRPC 是现在最流行的二进制 RPC 框架之一。2015 年由 Google 开源，在发布后迅速得到广泛关注。

###### 协议约定

gRPC 的协议是 Protocol Buffers，是一种压缩率极高的序列化协议，效率比 XML，JSON 更快。Google 在 2008 年开源了 Protocol Buffers，支持多种编程语言，所以 gRPC 支持客户端与服务端可以用不同语言实现。

###### 传输协议

在 JAVA 技术栈中，gRPC 的数据传输用的是 Netty Channel， **Netty** 是一个高效的基于异步 IO 的网络传输架构。Netty Channel 中，每个 gRPC 请求封装成 HTTP 2.0 的 Stream。

基于 HTTP 2.0 是 gRPC 一个很大的优势，可以定义四种不同的服务方法：单向 RPC，客户端流式 RPC，服务端流式 RPC，双向流式 RPC。

###### 服务发现

gRPC 本身没有提供服务发现的机制，需要通过其他组件。一个比较高性能的服务发现和负载均衡器是 **Envoy**，可以灵活配置转发规则，有兴趣的可以去了解下。

gRPC 是分布式微服务之间通信很好的一个选择，总结起来是因为有这些优点：

1. Protocol Buffers 压缩性高，速度快
2. HTTP 2.0 流传输
3. 支持多语言

gRPC 更多的是用在微服务集群内部，服务与服务之间的通信，服务与客户端之间的通信，REST 可以说是现在的主流。

### XML-RPC

**XML-RPC**是一个[远程过程调用](https://baike.baidu.com/item/远程过程调用)（[远端程序呼叫](https://baike.baidu.com/item/远端程序呼叫)）（remote procedure call，RPC)的[分布式计算](https://baike.baidu.com/item/分布式计算)[协议](https://baike.baidu.com/item/协议)，通过[XML](https://baike.baidu.com/item/XML)将调用函数封装，并使用[HTTP](https://baike.baidu.com/item/HTTP)协议作为传送机制。

**XML-RPC是最简单，最简单的Web服务方法之一，它使计算机可以轻松地调用其他计算机上的程序。**

- XML-RPC允许程序通过网络进行函数或过程调用。
- XML-RPC使用HTTP协议将信息从客户端计算机传递到服务器计算机。
- XML-RPC使用一个小的XML词汇表来描述请求和响应的性质。
- XML-RPC客户端在XML请求中指定过程名称和参数，服务器在XML响应中返回错误或响应。
- XML-RPC参数是一个简单的类型和内容列表 - 结构和数组是最复杂的类型。
- XML-RPC没有对象的概念，也没有包含使用其他XML词汇表的信息的机制。
- 使用XML-RPC和Web服务，Web成为程序连接的集合，计算机在紧密绑定的路径上交换信息。
- XML-RPC于1998年初开发，它由UserLand Software发布，最初在他们的Frontier产品中实现。

**XML-RPC由三个相对较小的部分组成：**

- XML-RPC数据模型：用于传递参数，返回值和错误(错误消息)的一组类型。
- XML-RPC请求结构：包含方法和参数信息的HTTP POST请求。
- XML-RPC响应结构：包含返回值或故障信息的HTTP响应。

#### 请求格式

XML-RPC请求是XML内容和HTTP标头的组合。 XML内容使用数据类型结构来传递参数，并包含标识指定要调用哪个过程的附加信息，而HTTP标头提供了一个用于通过Web传递请求的包装器。

每个请求都包含一个XML文档，其根元素是`methodCall`元素。 每个`methodCall`元素都包含一个`methodName`元素和一个`params`元素。 `methodName`元素标识要调用的过程的名称，而`params`元素包含参数列表和值。 每个`params`元素包括一个`param`元素列表，而`param`元素又包含值元素。

例如，要将请求传递给名称为`circleArea`的方法，该方法采用`double`类型参数(对于半径)，XML-RPC请求格式将如下所示：

```xml
<?xml version="1.0"?>
<methodCall>
   <methodName>circleArea</methodName>
      <params>
         <param>
            <value><double>2.41</double></value>
         </param>
      </params>
</methodCall>
```

例如，如果`circleArea`方法可在`/xmlrpc`侦听XML-RPC服务器获得返回结果，则请求如下所示：

```shell
POST /xmlrpc HTTP 1.0
User-Agent: myXMLRPCClient/1.0
Host: 192.168.124.2
Content-Type: text/xml
Content-Length: 169
Shell
```

组装信息后，整个请求看起来如下：

```xml
POST /xmlrpc HTTP 1.0
User-Agent: myXMLRPCClient/1.0
Host: 192.168.124.2
Content-Type: text/xml
Content-Length: 169
<?xml version="1.0"?>
<methodCall>
   <methodName>circleArea</methodName>
      <params>
         <param>
            <value><double>2.41</double></value>
         </param>
      </params>
</methodCall>
```

#### 响应格式

响应与请求非常类似，但又有一些不一样。 如果响应成功 - 找到过程，正确执行并返回结果 - 那么XML-RPC响应看起来很像请求 - 但是请求中`methodCall`元素被响应中`methodResponse`元素替换，并且没有`methodName`元素：

```xml
<?xml version="1.0"?>
<methodResponse>
   <params>
      <param>
         <value><double>18.24668429131</double></value>
      </param>
   </params>
</methodResponse>
```

在上面响应返回结果中 - 

- XML-RPC响应只能包含一个参数。
- 参数可以是数组或结构，因此可以返回多个值。
- 始终需要有返回值作为响应。 “成功值” - 是布尔值`true(1)`。

#### XML-RPC在java中使用

Java端使用Apache XML Project的Apache XML-RPC，可从 http://xml.apache.org/xmlrpc/ 获得。

将所有`.jar`文件放在适当的路径(如：构建路径)中，然后使用JAVA创建一个客户端和一个小型XML-RPC服务器。

```java
import java.util.*;
import org.apache.xmlrpc.*;

public class JavaClient {
   public static void main (String [] args) {

      try {
         XmlRpcClient client = new XmlRpcClient("http://localhost/RPC2"); 
         Vector params = new Vector();

         params.addElement(new Integer(17));
         params.addElement(new Integer(13));

         Object result = server.execute("sample.sum", params);

         int sum = ((Integer) result).intValue();
         System.out.println("The sum is: "+ sum);

      } catch (Exception exception) {
         System.err.println("JavaClient: " + exception);
      }
   }
}

public class JavaServer { 

   public Integer sum(int x, int y){
      return new Integer(x+y);
   }

   public static void main (String [] args){

      try {

         System.out.println("Attempting to start XML-RPC Server...");

         WebServer server = new WebServer(80);
         server.addHandler("sample", new JavaServer());
         server.start();

         System.out.println("Started successfully.");
         System.out.println("Accepting requests. (Halt program to stop.)");

      } catch (Exception exception){
         System.err.println("JavaServer: " + exception);
      }
   }
}
```

运行服务端，然后进行运行客户端这里会进行输入总和为30

### JSON-RPC

JSON-RPC是一种无状态、轻量级远程过程调用(RPC)协议。该规范主要定义了几种数据结构及其处理规则。它与传输无关，因为这些概念可以在相同的进程中、通过套接字、通过http或在许多不同的消息传递环境中使用。它使用JSON (RFC 4627)作为数据格式.JSON-RPC 直接在内容中定义了欲调用的函数名称（如 {"method": "getUser"}），这也令开发者不会陷于该使用 PUT 或者 PATCH 的问题之中.

JSON-RPC 2.0 Request 对象和 Response 对象可能无法与现有的 JSON-RPC 1.0客户机或服务器一起工作。但是，很容易区分这两个版本，因为2.0总是有一个名为“ jsonrpc”的成员，其 String 值为“2.0”，而1.0没有。大多数2.0实现应该考虑处理1.0对象，即使不考虑1.0的对等和类暗示方面。

 **带有位置参数的rpc调用**

```
--> {"jsonrpc": "2.0", "method": "subtract", "params": [42, 23], "id": 1}
<-- {"jsonrpc": "2.0", "result": 19, "id": 1}

--> {"jsonrpc": "2.0", "method": "subtract", "params": [23, 42], "id": 2}
<-- {"jsonrpc": "2.0", "result": -19, "id": 2}
```

**带有命名参数的rpc调用**

```
--> {"jsonrpc": "2.0", "method": "subtract", "params": {"subtrahend": 23, "minuend": 42}, "id": 3}
<-- {"jsonrpc": "2.0", "result": 19, "id": 3}

--> {"jsonrpc": "2.0", "method": "subtract", "params": {"minuend": 42, "subtrahend": 23}, "id": 4}
<-- {"jsonrpc": "2.0", "result": 19, "id": 4}
```

[更多示例具体可以看](https://www.jsonrpc.org/specification)

### Rest

#### 什么是Rest:

Rest即 Representational State Transfer。（资源）表现层状态转化。是目前最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便，所以正得到越来越多网站的采用.无论是服务间的调用RPC方式还是cs中都能体现到Rest（注：本文大约3000字）

> 以下文章来源：博客原文https://blog.igevin.info/posts/restful-architecture-in-general/,以第一人称进行讲述，其中里面有批注是我的理解

在移动互联网的大潮下，随着docker等技术的兴起，『微服务』的概念也越来越被大家接受并应用于实践，日益增多的web service逐渐统一于RESTful 架构风格，如果开发者对RESTful 架构风格不甚了解，则开发出的所谓RESTful API总会貌合神离，不够规范。  

![](https://oscimg.oschina.net/oscnet/2a4e450c-64c8-4952-9958-83e93a6d18d4.png "RESTful Style")

#### RESTful Style

##### RESTful架构风格

RESTful架构风格最初由Roy T. Fielding（HTTP/1.1协议专家组负责人）在其2000年的博士学位论文中提出。HTTP就是该架构风格的一个典型应用。从其诞生之日开始，它就因其可扩展性和简单性受到越来越多的架构师和开发者们的青睐。一方面，随着云计算和移动计算的兴起，许多企业愿意在互联网上共享自己的数据、功能；另一方面，在企业中，RESTful API（也称RESTful Web服务）也逐渐超越SOAP成为实现SOA的重要手段之一。时至今日，RESTful架构风格已成为企业级服务的标配。

REST即Representational State Transfer的缩写，可译为"表现层状态转化”。REST最大的几个特点为：资源、统一接口、URI和无状态。

##### RESTful架构风格的特点

###### 资源

所谓"资源"，就是网络上的一个实体，或者说是网络上的一个具体信息。它可以是一段文本、一张图片、一首歌曲、一种服务，总之就是一个具体的实在。资源总要通过某种载体反应其内容，文本可以用txt格式表现，也可以用HTML格式、XML格式表现，甚至可以采用二进制格式；图片可以用JPG格式表现，也可以用PNG格式表现；JSON是现在最常用的资源表示格式(批注：在我们现在的数据传送都是以json形式进行传输)。

资源是以json(或其他Representation)为载体的、面向用户的一组数据集，资源对信息的表达倾向于概念模型中的数据：资源总是以某种Representation为载体显示的，即序列化的信息 常用的Representation是json(推荐)或者xml（不推荐）等 Representation 是REST架构的表现层

相对而言，数据（尤其是数据库）是一种更加抽象的、对计算机更高效和友好的数据表现形式，更多的存在于逻辑模型中

> 资源和数据关系如下：数据又是资源，资源又是数据

![](https://oscimg.oschina.net/oscnet/cad63b7f-d5b3-4de2-aec8-dd3a1379813d.png "resource vs data")

###### 统一接口

RESTful架构风格规定，数据的元操作，即CRUD(create, read, update和delete,即数据的增删查改)操作，分别对应于HTTP方法：GET用来获取资源，POST用来新建资源（也可以用于更新资源），PUT用来更新资源，DELETE用来删除资源，这样就统一了数据操作的接口，仅通过HTTP方法，就可以完成对数据的所有增删查改工作。(ps：关于http可以去看我原来的文章[http](http://mp.weixin.qq.com/s?__biz=MzU5MjY4NDYxNQ==&mid=2247484072&idx=1&sn=10a0540729094cb03d83d552d8b8740a&chksm=fe1ab180c96d3896ac9b391d5b07b1a0d557e18663930e26ce455249700f4302eb61fe416e02&scene=21#wechat_redirect)![](https://oscimg.oschina.net/oscnet/d6ba65ff-2977-4199-8f3f-d4579e4a94b9.png))

即：

> GET（SELECT）：从服务器取出资源（一项或多项）。

> POST（CREATE）：在服务器新建一个资源。

> PUT（UPDATE）：在服务器更新资源（客户端提供完整资源数据）。

> PATCH（UPDATE）：在服务器更新资源（客户端提供需要修改的资源数据）。

> DELETE（DELETE）：从服务器删除资源。

###### URI

可以用一个URI（统一资源定位符）指向资源，即每个URI都对应一个特定的资源。要获取这个资源，访问它的URI就可以，因此URI就成了每一个资源的地址或识别符。(ps:我们想访问资源必须知道他所在的地址，否者计算机是无法找到的。)

一般的，每个资源至少有一个URI与之对应，最典型的URI即URL。

###### 无状态

所谓无状态的，即所有的资源，都可以通过URI定位，而且这个定位与其他资源无关，也不会因为其他资源的变化而改变。有状态和无状态的区别，

举个简单的例子说明一下。如查询员工的工资，如果查询工资是需要登录系统，进入查询工资的页面，执行相关操作后，获取工资的多少，则这种情况是有状态的，因为查询工资的每一步操作都依赖于前一步操作，只要前置操作不成功，后续操作就无法执行；

![](https://oscimg.oschina.net/oscnet/6c244ad1-85d1-43d7-af33-abdfe899bf76.png "state")

如果输入一个url即可得到指定员工的工资，则这种情况是无状态的，因为获取工资不依赖于其他资源或状态，且这种情况下，员工工资是一个资源，由一个url与之对应，可以通过HTTP中的GET方法得到资源，这是典型的RESTful风格。

![](https://oscimg.oschina.net/oscnet/a1580e06-5995-4abb-b64d-c99345756072.png "stateless")

##### ROA、SOA、REST与RPC

> ROA即Resource Oriented Architecture，RESTful 架构风格的服务是围绕资源展开的，是典型的ROA架构（虽然“A”和“架构”存在重复，但说无妨），虽然ROA与SOA并不冲突，甚至把ROA看做SOA的一种也未尝不可，但由于RPC也是SOA，比较久远一点点论文、博客或图书也常把SOA与RPC混在一起讨论，因此，RESTful 架构风格的服务通常被称之为ROA架构，很少提及SOA架构，以便更加显式的与RPC区分。

RPC风格曾是Web Service的主流，最初是基于XML-RPC协议（一个远程过程调用（remote procedure call，RPC)的分布式计算协议），后来渐渐被SOAP协议（简单对象访问协议（Simple Object Access Protocol））取代；RPC风格的服务，不仅可以用HTTP，还可以用TCP或其他通信协议。但RPC风格的服务，受开发服务采用语言的束缚比较大，如.NET框架中，开发web service的传统方式是使用WCF，基于WCF开发的服务即RPC风格的服务，使用该服务的客户端通常要用C#来实现，如果使用python或其他语言，很难实现可以直接与服务通信客户端；进入移动互联网时代后，RPC风格的服务很难在移动终端使用，而RESTful风格的服务，由于可以直接以json或xml为载体承载数据，以HTTP方法为统一接口完成数据操作，客户端的开发不依赖于服务实现的技术，移动终端也可以轻松使用服务，这也加剧了REST取代RPC成为web service的主导。(ps:关于RPC将会在后面具体写篇文章当都介绍，RPC可以理解直接调用的另一个服务的方法，比如dubbo就是一个RPC框架)

RPC与RESTful的区别如下面两个图所示：

![](https://oscimg.oschina.net/oscnet/c42d04bf-6610-4554-a67c-c1f9222e9739.png "blog-post-REST-vs-RPC1")



**ps:rpc服务的请求数据传输**  

-   把发送的数据封装到一个对象object中
    
-   object经过序列化得到json
    
-   json再经过base64编码，得到base64Json
    
-   base64Json的md5值，通过私钥（SHA1withRSA签名算法），计算出签名，并进行base64编码得到sign
    
-   base64Json经过url编码，得到urlEncodeJson；sign经过url编码得到urlEncodeSign。把urlEncodeSign和base64EncodeJson当做HTTP参数发出请求。
    
-   接受响应，把response报文的数据实体得到responseJson（响应的Conten-Type=application/json）
    
-   把responseJson经过反序列化得到jsonObject
    
-   从jsonObject中得到responseSign和responceMsg，利用公钥验签。
    
-   responseMsg经过base64解码得到msgJson
    
-   把msgJson反序列化为dataJsonObject，开始使用里面的字段

Restful风格：

![](https://oscimg.oschina.net/oscnet/4249b532-019d-4151-800a-d5774cebcc3b.png "blog-post-REST-vs-RPC2")

ps:rest就是我们常用的，不会因为客户端与服务器所采用的技术不同影响通信，他就是一个http请求。  