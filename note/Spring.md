## Spring 

### 为什么需要Spring FrameWork

当已经存在许多开放源代码（和专有）J2EEframework时，我们为什么还需要Spring Framework？

​	`因为诸多原因让Spring变得独特：它定位的领域是许多其他流行的framework没有的。``Spring致力于提供一种方法管理你的业务对象。``Spring是全面的和模块化的。`

​	`Spring有分层的体系结构`，这意味着你能选择使用它孤立的任何部分，它的架构仍然是内在稳定的。因此从你的学习中，你可得到最大的价值。例如，你可能选择仅仅使用Spring来简单化JDBC的使用，或用来管理所有的业务对象。它的设计从底部帮助你编写易于测试的代码。   

​     Spring是用于测试驱动工程的理想的framework。

​	 Spring对你的工程来说，它不需要一个以上的framework。Spring是潜在地一站式解决方案，定位于与典型应用相关的大部分基础结构。它也涉及到其他framework没有考虑到的内容。可以降低开发企业应用的复杂程度，以IoC(控制反转)和AOP(面向切面编程)两种技术为基础简化了企业开发的复杂性，方便解耦，简化开发 Spring 就是一个大工厂，可以将所有对象创建和依赖关系维护，交给 Spring 管理 AOP 编程的支持 Spring 提供面向切面编程，可以方便的实现对程序进行权限拦截、运行监控等功能 声明式事务的支持 只需要通过配置就可以完成对事务的管理，而无需手动编程 方便程序的测试 Spring 对 Junit4 支持，可以通过注解方便的测试 Spring 程序 方便集成各种优秀框架 Spring 不排斥各种优秀的开源框架，其内部提供了对各种优秀框架（如：Struts、Hibernate、 MyBatis、Quartz 等）的直接支持 降低 JavaEE API 的使用难度 Spring 对 JavaEE 开发中非常难用的一些 API（JDBC、JavaMail、远程调用等），都提供了封装， 使这些 API 应用难度大大降

### 体系结构

Spring 有可能成为所有企业应用程序的一站式服务点，然而，Spring 是模块化的，允许你挑选和选择适用于你的模块，不必要把剩余部分也引入。Spring 框架提供约 20 个模块，可以根据应用程序的要求来使用。

![img](https://img-blog.csdn.net/20180825212519786?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3MjU2ODk2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![Click and drag to move](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 核心容器

**核心容器由spring-core，spring-beans，spring-context，spring-context-support和spring-expression（SpEL，Spring表达式语言，Spring Expression Language）等模块组成，它们的细节如下：**

- spring-core模块提供了框架的基本组成部分，包括 IoC 和依赖注入功能。
- spring-beans 模块提供 BeanFactory，工厂模式的微妙实现，它移除了编码式单例的需要，并且可以把配置和依赖从实际编码逻辑中解耦。
- context模块建立在由core和 beans 模块的基础上建立起来的，它以一种类似于JNDI注册的方式访问对象。Context模块继承自Bean模块，并且添加了国际化（比如，使用资源束）、事件传播、资源加载和透明地创建上下文（比如，通过Servelet容器）等功能。Context模块也支持Java EE的功能，比如EJB、JMX和远程调用等。ApplicationContext接口是Context模块的焦点。spring-context-support提供了对第三方库集成到Spring上下文的支持，比如缓存（EhCache, Guava, JCache）、邮件（JavaMail）、调度（CommonJ, Quartz）、模板引擎（FreeMarker, JasperReports, Velocity）等。
- spring-expression模块提供了强大的表达式语言，用于在运行时查询和操作对象图。它是JSP2.1规范中定义的统一表达式语言的扩展，支持set和get属性值、属性赋值、方法调用、访问数组集合及索引的内容、逻辑算术运算、命名变量、通过名字从Spring IoC容器检索对象，还支持列表的投影、选择以及聚合等。。

### 数据访问/集成

**数据访问/集成层包括 JDBC，ORM，OXM，JMS 和事务处理模块，它们的细节如下：（注：JDBC=Java Data Base Connectivity，ORM=Object Relational Mapping，OXM=Object XML Mapping，JMS=Java Message Service）**

- JDBC 模块提供了JDBC抽象层，它消除了冗长的JDBC编码和对数据库供应商特定错误代码的解析。
-  ORM 模块提供了对流行的对象关系映射API的集成，包括JPA、JDO和Hibernate等。通过此模块可以让这些ORM框架和spring的其它功能整合，比如前面提及的事务管理。
- OXM 模块提供了对OXM实现的支持，比如JAXB、Castor、XML Beans、JiBX、XStream等。
- JMS 模块包含生产（produce）和消费（consume）消息的功能。从Spring 4.1开始，集成了spring-messaging模块。
- 事务模块为实现特殊接口类及所有的 POJO 支持编程式和声明式事务管理。（注：编程式事务需要自己写beginTransaction()、commit()、rollback()等事务管理方法，声明式事务是通过注解或配置由spring自动处理，编程式事务粒度更细）

### Web

**Web 层由 Web，Web-MVC，Web-Socket 和 Web-Portlet 组成，它们的细节如下：**

- Web 模块提供面向web的基本功能和面向web的应用上下文，比如多部分（multipart）文件上传功能、使用Servlet监听器初始化IoC容器等。它还包括HTTP客户端以及Spring远程调用中与web相关的部分。
- Web-MVC 模块为web应用提供了模型视图控制（MVC）和REST Web服务的实现。Spring的MVC框架可以使领域模型代码和web表单完全地分离，且可以与Spring框架的其它所有功能进行集成。
- Web-Socket 模块为 WebSocket-based 提供了支持，而且在 web 应用程序中提供了客户端和服务器端之间通信的两种方式。
- Web-Portlet 模块提供了用于Portlet环境的MVC实现，并反映了spring-webmvc模块的功能。

### 其他

**还有其他一些重要的模块，像 AOP，Aspects，Instrumentation，Web 和测试模块，它们的细节如下：**

- AOP 模块提供了面向方面的编程实现，允许你定义方法拦截器和切入点对代码进行干净地解耦，从而使实现功能的代码彻底的解耦出来。使用源码级的元数据，可以用类似于.Net属性的方式合并行为信息到代码中。
- Aspects 模块提供了与 AspectJ 的集成，这是一个功能强大且成熟的面向切面编程（AOP）框架。
- Instrumentation 模块在一定的应用服务器中提供了类 instrumentation 的支持和类加载器的实现。
- Messaging 模块为 STOMP 提供了支持作为在应用程序中 WebSocket 子协议的使用。它也支持一个注解编程模型，它是为了选路和处理来自 WebSocket 客户端的 STOMP 信息。
- 测试模块支持对具有 JUnit 或 TestNG 框架的 Spring 组件的测试。

### 主要有七大模块

`每个模块可以单独使用也可以多模块组合使用，`

`核心模块：` spring core是核心容器实现了IoC模式，提供了框架的基础功能，在模块中包含BeanFactory类，负责对JavaBean配置与管理采用Factory模式实现loC容器即依赖注入。

`Context模块：` 继承了BeanFactory并且添加了处理事件，国际化,资源装载，数据校验等，JNDI访问，ejb,远程调用，集成模块框架，Email,定时任务。

`AOP模块`：通过事务管理使得任意Spring管理的对象AOP化。

`DAO模块`：JDBC的抽象层，简化数据库的厂商的异常错误，减少了代码的书写，并且提供了声明式的任务，和编程式任务。

`O/R映射模块`：直接用Hibernate。

`Web模块`：建立在Spring Context 模块的基础，提供servlet监听器的Context和web应用上下文

`mvc模块`：建立在Spring 核心功能之上，使得拥有Spring框架的所有特性适应于多种的视图模块技术

```
配置：
Spring模块中是根据每一个模块对应的一个jar包
spring,jar//整个Spring模块
spring-core.jar//核心模块包含ioc容器
spring-aop.jar//Aop模块
spring-context.jar//Spring上下文包含ApplicationContext容器
spring-dao.jar//dao层与jdbc的支持
spring-orm.jar
spring-web.jar
spring-webmvc.jar//
Spring项目：<---Spring配置{jar包----->tlb标签库--->applicationContext.xml}
```

### IOC 容器

IoC即控制反转，他使得组件或类之间尽量的形成一种松的耦合结构，创建类都是Ioc容器来干，Spring 容器是 Spring 框架的核心。容器将创建对象，把它们连接在一起，配置它们，并管理他们的整个生命周期从创建到销毁。把对象的创建、初始化、销毁交给 spring 来管理，而不是由开发者控制，实现控制反转。

所谓IoC，就是通过容器来控制业务对象之间的依赖关系，而非传统实现中，由代码直接操控。这也就是“控制反转”概念的所在：控制权由应用代码中转到了外部容器，控制权的转移，就是反转。控制权转移带来的好处就是降低了业务对象之间的依赖程度 Spring 容器使用依赖注入（DI）来管理组成一个应用程序的组件。这些对象被称为 Spring Beans，Spring IoC 容器利用 Java 的 POJO 类和配置元数据来生成完全配置和可执行的系统或应用程序

Spring通过一个配置文件描述了Bean及Bean之间的依赖关系，利用Java语言的反射功能实例化Bean并建立Bean之间的依赖关系。

Spring的IoC容器在完成这些底层工作的基础上，还提供了Bean实例缓存、生命周期管理、Bean实例代理、事件发布、资源装载等高级服务

**谈及IOC容器我们应该从BeanFactory和ApplicationContext接口分析。**

#### BeanFactory

BeanFactory是Spring最核心的接口，他提供了高级的IoC配置机制。BeanFactory使管理不同类型的java对象成为了可能，应用上下文(ApplicationContext)建立在BeanFactory的基础之上。它还提供了国际化支持和框架事件体系，更易于创建实际应用。一般称BeanFactory为IoC容器，而称ApplicationContext为应用上下文。但有时为了行文方便，也将ApplicationContext称为Spring容器。

它主要的功能是为依赖注入 （DI） 提供支持，这个容器接口在 org.springframework.beans.factory.BeanFactor 中被定义。
`BeanFactory 和相关的接口，比如BeanFactoryAware、 DisposableBean、InitializingBean，仍旧保留在 Spring 中，主要目的是向后兼容已经存在的和那些 Spring 整合在一起的第三方框架实现了IoC控制，可以称为IoC容器通过xml配置文件或.properties中读取Javabean的定义，来实现Javabean配置和管理创建。`

可以通过BeanFactory接口方法getBean来使用Bean名字，从而当获取Bean时，如果需要获取的Bean是prototype类型的，用户还可以为这个prototype类型的Bean生成指定构造函数的对应参数。这使得在一定程度上可以控制生成prototype类型的Bean。有了BeanFactory的定义，用户可以执行以下操作：

❑ 通过接口方法containsBean让用户能够判断容器是否含有指定名字的Bean。

❑ 通过接口方法isSingleton来查询指定了名字的Bean是否是Singleton类型的Bean。对于Singleton属性，用户可以在BeanDefinition中指定。

❑ 通过接口方法isPrototype来查询指定了名字的Bean是否是prototype类型的。与Singleton属性一样，这个属性也可以由用户在BeanDefinition中指定。

❑ 通过接口方法isTypeMatch来查询指定了名字的Bean的Class类型是否是特定的Class类型。这个Class类型可以由用户来指定。

❑ 通过接口方法getType来查询指定了名字的Bean的Class类型。

❑ 通过接口方法getAliases来查询指定了名字的Bean的所有别名，这些别名都是用户在BeanDefinition中定义的。这些定义的接口方法勾画出了IoC容器的基本特性



**XmlBeanFactory可以通过xml读取装配JavaBean**在调用getBean()方法时不会实例化任何对象只有在JavaBean需要创建时才会分配资源空间，

- 第一步利用框架提供的 XmlBeanFactory() API 去生成工厂 bean 以及利用 ClassPathResource() API 去加载在路径 CLASSPATH 下可用的 bean 配置文件。
  	XmlBeanFactory() API 负责创建并初始化所有的对象，即在配置文件中提到的 bean。
  
- 第二步利用第一步生成的 bean 工厂对象的 getBean() 方法得到所需要的 bean。 这个方法通过配置文件中的 bean ID 来返回一个真正的对象，该对象最后可以用于实际的对象。一旦得到这个对象，就可以利用这个对象来调用任何方法

**例如通过BeanFactory装载配置文件，启动Spring IoC容器：**
	
```java
Resource re=new ClassPathResource("applicationContext.xml");
BeanFactory factory=new XmlBeanFactory(re);
Test test =factory.getBean("test");
> 在xml文件中配置如下：
> <！引入beans.dtd>
> <beans>
> < bean id="test" class="com.test.Test">
> </beans>
```

XmlBeanFactory通过Resource装载Spring配置信息并启动IoC容器，然后就可以通过BeanFactory#getBean(beanName)方法从IoC容器中获取Bean了。通过BeanFactory启动IoC容器时，并不会初始化配置文件中定义的Bean。初始化动作发生在第一个调用时，对于单实例（singleton）的Bean来说，BeanFactory会缓存Bean实例，所以第二次使用getBean()获取Bean时，将直接从IoC容器的缓存中获取Bean实例。



Spring在DefaultSingletonBeanRegistry类中提供了一个用于缓存单实例Bean的缓存器，*它是一个用HashMap实现的缓存器*，单实例的Bean以beanName为键保存在这个HashMap中。值得一提的是，在初始化BeanFactory时，必须为其提供一种日志框架，这里使用Log4J，即在类路径下提供Log4J配置文件，这样启动Spring容器才不会报错

#### ApplicationContext:

ApplicationContext是Spring中较高级的容器和beanFactory类似，他可以加载配置文件定义的bean，将所有的bean集中在一起，当请求时分配bean，**扩展了BeanFactory容器并添加了国际化，生命周期，事件，监听，提供了BeanFactory的所有特性而且允许用户使用更多的声明方式.** ApplicationContext由BeanFactory派生而来，提供了更多面向实际应用的功能。在BeanFactory中，很多功能需要以编程的方式实现，而在ApplicationContext中则可以通过配置的方式实现.

有三个实现的类：	`ClassPathXmlApplicationContext,FileSystemXmlApplicationContext,WebApplicationContext`

##### ClassPathXmlApplicationContext:

​	从当前类路径中检索配置文件并装载他来创建容器的实例

​	`ApplicationContext context=new  ClassPathXmlApplicationContext(String configLocation);`

##### FileSystemXmlApplicationContext:

​	如果配置文件放置在文件系统的路径下，则可以优先考虑使用这个类。不是从类路径中获取配置信息，而是通过参数指定配置文件的位置，可以获取类路径之外的资源，该容器从 XML 文件中加载已被定义的 bean。

在这里，你需要提供给构造器 XML 文件的完整路径

​	`ApplicationContext context=new  FileSystemXmlApplicationContext(String configLocation);`

##### WebApplicationContext:

WebApplicationContext是专门为Web应用准备的，它允许从相对于Web根目录的路径中装载配置文件，完成初始化工作。从WebApplicationContext中可以获得ServletContext的引用，整个Web应用上下文对象将作为属性放置到ServletContext中，以便Web应用环境可以访问Spring应用上下文。Spring专门为此提供一个工具类WebApplicationContextUtils，通过该类的getWebApplicationContext(ServletContext sc) 方 法，即 可 以 从ServletContext中 获 取WebApplicationContext实例

​	有两种方法在servlet中使用

- 1.在servlet中的web.xml配置Spring 的 ContextLoaderListener的监听器，
- 2.修改web.xml在配置文件中添加一个servlet定义使用Spring的ContextLoaderServlert类

**ApplicationContext的初始化和BeanFactory的初始化有一个重大的区别：BeanFactory在初始化容器时，并未实例化Bean，直到第一次访问某个Bean时才实例化目标Bean；而ApplicationContext在初始化应用上下文时就实例化所有单实例的Bean。因此ApplicationContext的初始化时间会比BeanFactory稍长一些，不过稍后的调用则没有“第一次惩罚”的问题**

#### IOC容器的初始化（以Xml形式的Ioc初始化）

**在看具体的容器初始化先看下重要的BeanDefinition的类图**

[![6eD13Q.png](https://s3.ax1x.com/2021/03/05/6eD13Q.png)](https://imgtu.com/i/6eD13Q)

BeanDefinition是配置文件<bean>元素标签在容器中内部表示形式。

- `RootBeanDefinition`可以单独作为一个`BeanDefinition`，也可以作为其他`BeanDefinition`的父类。但是他不能作为其他BeanDefinition的子类
- `ChildBeanDefinition`相当于一个子类，不可以单独存在，必须要依赖一个父`BeanDetintion`。
- `GenericBeanDefinition `可以替代`RootBeanDefinition`和`ChildBeanDefinition`
- `AnnotatedGenericBeanDefinition`处理`@``Configuration`注解
- `ConfigurationClassBeanDefinition`处理`@Bean`注解
- `ScannedGenericBeanDefinition`处理`@Component`注解

IoC容器的初始化包括**BeanDefinition的Resouce定位**、**载入**和**注册**这三个基本的过程

**BeanDefinition**描述和定义了创建一个Bean需要的所有信息，属性，构造函数参数以及访问它们的方法。还有其他一些信息，比如这些定义来源自哪个类等等信息

**Resouce定位：** BeanDefinition的资源定位由ResourceLoader通过统一的Resource接口来完成，这个Resource对各种形式的BeanDefinition的使用提供了统一接口。比如说，在文件系统中的Bean定义信息可以使用FileSystemResource来进行抽象；在类路径中可以使用前面提到的ClassPathResource来使用，等等。这个过程类似于容器寻找数据的过程，就像用水桶装水先要把水找到一样

**BeanDefinition的载入：**第二个关键的部分是BeanDefinition的载入，该载入过程把用户定义好的Bean表示成IoC容器内部的数据结构，而这个容器内部的数据结构就是BeanDefinition，总地说来，这个BeanDefinition定义了一系列的数据来使得IoC容器能够方便地对POJO对象也就是Spring的Bean进行管理。即BeanDefinition就是Spring的领域对象模型

**BeanDefinition的注册：**第三个过程是向IoC容器注册这些BeanDefinition的过程。这个过程是通过调用BeanDefinitionRegistry接口的实现来完成的，这个注册过程把载入过程中解析得到的BeanDefinition向IoC容器进行注册。可以看到，在IoC容器内部，是通过使用一个HashMap来持有这些BeanDefinition数据的。

##### BeanDefinition的Resource定位

Resource定位这个过程就是我们所看到的寻找bean定义的资源配置文件，找到”applicationContext.xml“以及其他的配置文件信息。

这时我们可以使用的是ClassPathResource，意味着Spring会在类路径中寻找以文件形式存在的BeanDefinition信息。

`ClassPathResource res = new ClassPathResource("beans.xml")` 使用这个代码不能让DefaultListableBeanFactory.

这个过程是使用的ApplicationContext的实现类`ClassPathXmlApplicationContext,FileSystemXmlApplicationContext,WebApplicationContext` 通过这些类去定位到Resource对象。

###### **以ClassPathXmlApplicationContext获取bean为例深入源码分析**

```java
ClassPathXmlApplicationContext  context = new ClassPathXmlApplicationContext("bean.xml");
```

当程序通过new的时候会进行调用其构造方法，在构造方法内进行资源加载，主要看构造方法内的**setConfigLocations(configLocations)** 进行资源定位

```java
public ClassPathXmlApplicationContext(
			String[] configLocations, boolean refresh, @Nullable ApplicationContext parent)
			throws BeansException {

		super(parent);
    	// 资源定位
		setConfigLocations(configLocations);
    	// 载入BeanDefinition的入口
		if (refresh) {
			refresh();
		}
	}
```

资源定位集中在抽象类AbstractRefreshableConfigApplicationContext的setConfigurations()方法内我们具体的看下：在这个方法内是先进行断言这个位置是否为null.在这类中初始了一个String[] configLocations数组.将解析后的路径填充到这个数组中。

```java
/**
* 设置上下文的配置，如果未配置则 可以进行默认配置
*
**/
public void setConfigLocations(@Nullable String... locations) {
		if (locations != null) {
            // 断言是否为空路径
			Assert.noNullElements(locations, "Config locations must not be null");
			this.configLocations = new String[locations.length];
			for (int i = 0; i < locations.length; i++) {
                // resolvePath为同一类中将字符串解析路径的方法
				this.configLocations[i] = resolvePath(locations[i]).trim();
			}
		}
		else {
			this.configLocations = null;
		}
	}
/**
* 路径的解析
*/
protected String resolvePath(String path) {
	return getEnvironment().resolveRequiredPlaceholders(path);
}
/**
* 获取路径
*/
@Override
public ConfigurableEnvironment getEnvironment() {
    // 环境为null则进行创建环境
    if (this.environment == null) {
        this.environment = createEnvironment();
    }
    // 否者返回档期那的环境
    return this.environment;
}
/**
* 创建并返回一个环境
**/
protected ConfigurableEnvironment createEnvironment() {
    return new StandardEnvironment();
}
/**
*这个时候创建了一个标准的环境。可以看到resolvePath()方法来自AbstractEnvironment类中
*
**/
public class StandardEnvironment extends org.springframework.core.env.AbstractEnvironment {
    public static final java.lang.String SYSTEM_ENVIRONMENT_PROPERTY_SOURCE_NAME = "systemEnvironment";
    public static final java.lang.String SYSTEM_PROPERTIES_PROPERTY_SOURCE_NAME = "systemProperties";

    public StandardEnvironment() { /* compiled code */ }

    protected void customizePropertySources(org.springframework.core.env.MutablePropertySources propertySources) { /* compiled code */ }
}
```

经过上面的步骤这样就完成了Resource的定位。

##### BeanDefinition的载入

BeanDefinition的载入过程相当于把我们定义的BeanDefinition在IoC容器中转化成一个Spring内部表示的数据结构的过程。IoC容器对Bean的管理和依赖注入功能的实现，是通过对其持有的BeanDefinition进行各种相关的操作来完成的。这些BeanDefinition数据在IoC容器里通过一个HashMap来保持和维护，当然这只是一种比较简单的维护方式。refresh()方法是载入BeanDefinition的入口。

```java
public ClassPathXmlApplicationContext(
			String[] configLocations, boolean refresh, @Nullable ApplicationContext parent)
			throws BeansException {

		super(parent);
    	// 资源定位
		setConfigLocations(configLocations);
    	// 载入BeanDefinition的入口
		if (refresh) {
			refresh();
		}
	}
```

###### BeanDefinition中的refresh()方法进行载入

在AbstractApplicationContext类中找到这个方法，它详细地描述了整个ApplicationContext的初始化过程，比如BeanFactory的更新，messagesource和postprocessor的注册。具体看下refresh()方法

```java
@Override
	public void refresh() throws BeansException, IllegalStateException {
		synchronized (this.startupShutdownMonitor) {
			// Prepare this context for refreshing. 刷新前的准备
            // 调用容器准备刷新方法，获取容器的当前时间，同时给容器设置同步标识
			prepareRefresh();

			// Tell the subclass to refresh the internal bean factory. 
            // 关键方法构建beanFactory——>接下来会详解本方法
            // 告诉子类启动refershBeanFactory()方法，Bean定义资源文件的载入从子类的refreshBeanFactory()方法启动
			ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

			// Prepare the bean factory for use in this context. 为在这个上下文中使用beanFactory做准备
			prepareBeanFactory(beanFactory);

			try {
                // 设置后置处理器
				// Allows post-processing of the bean factory in context subclasses.
				postProcessBeanFactory(beanFactory);
                
				// 调用bean的后置处理器，这些处理器在上下文中被注册为bean的形式
				// Invoke factory processors registered as beans in the context.
				invokeBeanFactoryPostProcessors(beanFactory);
                
				// 注册拦截bean创建的处理器
				// Register bean processors that intercept bean creation.
				registerBeanPostProcessors(beanFactory);
                
				// 为上下文初始化消息源，国际化功能
				// Initialize message source for this context.
				initMessageSource();
                
				// 初始化上下文的时间机制
				// Initialize event multicaster for this context.
				initApplicationEventMulticaster();
                
				// 初始化其他特殊bean在特殊上下文子类中
				// Initialize other special beans in specific context subclasses.
				onRefresh();
				
                // 检查监听的bean，并将他们注册到容器中
				// Check for listener beans and register them.
				registerListeners();
				
                // 初始化所有的非懒加载单件
				// Instantiate all remaining (non-lazy-init) singletons.
				finishBeanFactoryInitialization(beanFactory);
				
                // 发布相关事件，结束refresh
				// Last step: publish corresponding event.
				finishRefresh();
			}catch (BeansException ex) {
				if (logger.isWarnEnabled()) {
					logger.warn("Exception encountered during context initialization - " +
							"cancelling refresh attempt: " + ex);
				}
				// 出现异常销毁bean
				// Destroy already created singletons to avoid dangling resources.
				destroyBeans();
				
                // 这个active在上面的prepare中被设置为了true
				// Reset 'active' flag.
				cancelRefresh(ex);
				
				// Propagate exception to caller.
				throw ex;
			}finally {
				// Reset common introspection caches in Spring's core, since we
				// might not ever need metadata for singleton beans anymore...
                // 重置缓存
				resetCommonCaches();
			}
		}
	}
```

refresh()方法主要为IOC容器Bean的生命周期管理提供条件，Spring IOC容器载入Bean配置信息从其子类容器的refreshBeanFactory()方法启动，所以整个refresh()方法中obtainFreshBeanFactory()以后的代码都是再注册容器信息源和生命周期事件。

refresh()方法主要作用是：在创建IOC容器前，如果已经存在容器，需要把已有的容器销毁和关闭，以保证在refresh()方法之后使用的是最新创建的IOC容器，它类似于对Ioc容器的重启，在新创建的容器中进行初始化对Bean配置资源进行载入。

在这个refresh()方法中我们主要看下AbstractApplicationContext类下的obtainFreshBeanFactory()相关方法

```java
protected ConfigurableListableBeanFactory obtainFreshBeanFactory() {
    // 使用委派模式，父类定义了抽象的refreshBeanFactory()的方法
    refreshBeanFactory();
    return getBeanFactory();
}
/**
* AbstractApplicationContext的子类
* AbstractRefreshableApplicationContext类下的refreshBeanFactory()
**/
@Override
protected final void refreshBeanFactory() throws BeansException {
    // 如果已经有容器，销毁容器的Bean，关闭容器
    if (hasBeanFactory()) {
        destroyBeans();
        closeBeanFactory();
    }
    try {
        // 创建IOC容器，使用DefaultListableBeanFactory
        DefaultListableBeanFactory beanFactory = createBeanFactory();
        // 设置序列化Id, map 中 serializableFactories put 序列化id,弱引用 
        beanFactory.setSerializationId(getId());
        // 定制此上下文使用的内部bean工厂，主要分析是否允许Bean定义覆盖，和允许循环引用是否设置为null
        customizeBeanFactory(beanFactory);
        // 启动对BeanDefiniton的载入，这里又使用了委派模式，在当前类中只定义了抽象的loadBeanDefinitions()方法，调用子类容器实现
        loadBeanDefinitions(beanFactory);
        this.beanFactory = beanFactory;
    }
    catch (IOException ex) {
        throw new ApplicationContextException("I/O error parsing bean definition source for " + getDisplayName(), ex);
    }
}
/**
* 创建IOC容器，使用DefaultListableBeanFactory
*/
protected DefaultListableBeanFactory createBeanFactory() {
    return new DefaultListableBeanFactory(getInternalParentBeanFactory());
}
```

下面再进行深入看到AbstractXmlApplicationContext类中 loadBeanDefinitions() 

```java
@Override
protected void loadBeanDefinitions(DefaultListableBeanFactory beanFactory) throws BeansException, IOException {
    // 创建XmlBeanDefinitionReader,并通过回调设置到BeanFactory中取。创建BeanFactory的过程
    // Create a new XmlBeanDefinitionReader for the given BeanFactory.
    XmlBeanDefinitionReader beanDefinitionReader = new XmlBeanDefinitionReader(beanFactory);
	
    // 为Bean读取器设置Spring资源加载器
    // Configure the bean definition reader with this context's
    // resource loading environment.
    beanDefinitionReader.setEnvironment(this.getEnvironment());
    // 设定ResourceLoader
    // 容器本身也是个资源加载器
    beanDefinitionReader.setResourceLoader(this);
    // 为Bean读取器设置SAX xml解析器
    beanDefinitionReader.setEntityResolver(new ResourceEntityResolver(this));

    // Allow a subclass to provide custom initialization of the reader,
    // then proceed with actually loading the bean definitions.
    // 启动Bean定义信息载入的过程，启用xml的校验机制
    initBeanDefinitionReader(beanDefinitionReader);
    // 加载Bean定义（Bean读取器真正实现加载的方法）
    loadBeanDefinitions(beanDefinitionReader);
}
/**
* AbstractXmlApplicationContext中内部私有的loadBeanDefinitions(XmlBeanDefinitionReader reader)
* 实际上是调用的XmlBeanDefinitionReader的loadBeanDefinitions()
*/
protected void loadBeanDefinitions(XmlBeanDefinitionReader reader) throws BeansException, IOException {
    // 获取Bean配置的资源定位
    Resource[] configResources = getConfigResources();
    if (configResources != null) {
        // xml Bean读取器调用其父类XmlBeanDefinitionReader读取Bean的配置信息
        reader.loadBeanDefinitions(configResources);
    }
    // 如果子类中获取的Bean资源定位为空，则获取ClassPathXmlApplicationContext构造方法的setConfigLocation方法设置的资源
    String[] configLocations = getConfigLocations();
    if (configLocations != null) {
        // xml Bean 读取器调用其父类AbstractBeanDefinitionReader读取定位
        reader.loadBeanDefinitions(configLocations);
    }
}
///////////////分配路径处理策略//////////

// 程序会走进reader.loadBeanDefinitions(configLocations);我们看下载入过程
@Override
	public int loadBeanDefinitions(String location) throws BeanDefinitionStoreException {
		return loadBeanDefinitions(location, null);
	}

	/**
	 * 从指定的资源位置加载bean定义。该位置也可以是位置模式，前提是此bean定义读取器的ResourceLoader是ResourcePatternResolver
	 */
	public int loadBeanDefinitions(String location, @Nullable Set<Resource> actualResources) throws BeanDefinitionStoreException {
        // 获取IOC容器初始化过程中设置的资源加载器
		ResourceLoader resourceLoader = getResourceLoader();
		if (resourceLoader == null) {
			throw new BeanDefinitionStoreException(
					"Cannot import bean definitions from location [" + location + "]: no ResourceLoader available");
		}

		if (resourceLoader instanceof ResourcePatternResolver) {
			// Resource pattern matching available.
			try {
               // 将指定位置的Bean配置信息解析为Spring IOC容器封装的资源，加载多个指定位置的Bean配置信息
				Resource[] resources = ((ResourcePatternResolver) resourceLoader).getResources(location);
                // 在这里loadBeanDefinitions会进行调用BeanDefinitionReader的实现类就是下面的
            // XmlBeanDefinitionReader的loadBeanDefinitions()。这里又一次用了委派模型
				int loadCount = loadBeanDefinitions(resources);
				if (actualResources != null) {
					for (Resource resource : resources) {
						actualResources.add(resource);
					}
				}
				if (logger.isDebugEnabled()) {
					logger.debug("Loaded " + loadCount + " bean definitions from location pattern [" + location + "]");
				}
				return loadCount;
			}
			catch (IOException ex) {
				throw new BeanDefinitionStoreException(
						"Could not resolve bean definition resource pattern [" + location + "]", ex);
			}
		}
		else {
			// Can only load single resources by absolute URL.
            // 将指定的位置的Bean位置信息解析为Spring IOC容器封装的资源，加载单个指定的位置Bean配置信息
			Resource resource = resourceLoader.getResource(location);
            // 在这里loadBeanDefinitions会进行调用BeanDefinitionReader的实现类就是下面的
            // XmlBeanDefinitionReader的loadBeanDefinitions()这里又一次用了委派模型
			int loadCount = loadBeanDefinitions(resource);
			if (actualResources != null) {
				actualResources.add(resource);
			}
			if (logger.isDebugEnabled()) {
				logger.debug("Loaded " + loadCount + " bean definitions from location [" + location + "]");
			}
			return loadCount;
		}
	}



// 实际上是调用的XmlBeanDefinitionReader的loadBeanDefinitions()

	/**
	 * Load bean definitions from the specified XML file.
	 * @param encodedResource the resource descriptor for the XML file,
	 * allowing to specify an encoding to use for parsing the file
	 * @return the number of bean definitions found
	 * @throws BeanDefinitionStoreException in case of loading or parsing errors
	 */
public int loadBeanDefinitions(EncodedResource encodedResource) throws BeanDefinitionStoreException {
    Assert.notNull(encodedResource, "EncodedResource must not be null");
    if (logger.isTraceEnabled()) {
        logger.trace("Loading XML bean definitions from " + encodedResource);
    }

    Set<EncodedResource> currentResources = this.resourcesCurrentlyBeingLoaded.get();

    if (!currentResources.add(encodedResource)) {
        throw new BeanDefinitionStoreException(
            "Detected cyclic loading of " + encodedResource + " - check your import definitions!");
    }
	// 得到Xml 文件并用InputStream准备读取
    try (InputStream inputStream = encodedResource.getResource().getInputStream()) {
        InputSource inputSource = new InputSource(inputStream);
        if (encodedResource.getEncoding() != null) {
            // 设定字符编码
            inputSource.setEncoding(encodedResource.getEncoding());
        }
        // 进行实际载入
        return doLoadBeanDefinitions(inputSource, encodedResource.getResource());
    }
    catch (IOException ex) {
        throw new BeanDefinitionStoreException(
            "IOException parsing XML document from " + encodedResource.getResource(), ex);
    }
    finally {
        currentResources.remove(encodedResource);
        if (currentResources.isEmpty()) {
            this.resourcesCurrentlyBeingLoaded.remove();
        }
    }
}
// 从特定的XML文件读取，实际载入BeanDefinition的地方
protected int doLoadBeanDefinitions(InputSource inputSource, Resource resource)
			throws BeanDefinitionStoreException {

		try {
            // 取得XML的Document对象，由DefaultDocumentLoader在定义时创建的documentLoader
            // 将bean配置转换成Document对象
			Document doc = doLoadDocument(inputSource, resource);
            // 启动对Beandefinition解析的详细过程，会将bean转变为IOC容器里的内部的数据结构，这个过程会用到spring的Bean
			int count = registerBeanDefinitions(doc, resource);
			if (logger.isDebugEnabled()) {
				logger.debug("Loaded " + count + " bean definitions from " + resource);
			}
			return count;
		}
		...
	}
// Spring的BeanDefinion是怎样按照Spring的Bean语义要求进行解析并转化为容器内部数据结构的，这个过程是在registerBeanDefinitions (doc, resource)中完成的
public int registerBeanDefinitions(Document doc, Resource resource) throws BeanDefinitionStoreException {
	// 创建BeanDefinitionDocumentReader来对xml进行解析
    BeanDefinitionDocumentReader documentReader = createBeanDefinitionDocumentReader();
    // 获取容器注册的Bean的数量
		int countBefore = getRegistry().getBeanDefinitionCount();
    	// 具体解析会放到了BeanDefinitionDocumentReader中的 registerBeanDefinitions
    	// 使用了委派将委派到DefaultBeanDefinitionDocumentReader完成
		documentReader.registerBeanDefinitions(doc, createReaderContext(resource));
    	// 统计解析Bean的数量
		return getRegistry().getBeanDefinitionCount() - countBefore;
	}

	/**
	 * Create the {@link BeanDefinitionDocumentReader} to use for actually
	 * reading bean definitions from an XML document.
	 * <p>The default implementation instantiates the specified "documentReaderClass".
	 * @see #setDocumentReaderClass
	 */
	protected BeanDefinitionDocumentReader createBeanDefinitionDocumentReader() {
		return BeanUtils.instantiateClass(this.documentReaderClass);
	}
/**
* BeanDefinitionDocumentReader中的 registerBeanDefinitions()调用其实现类DefaultBeanDefinitionDocumentReader对文档进行解析
*/
	/**
	 *此实现根据“ spring-beans” XSD（历史上称为DTD）解析bean定义。
打开一个DOM文档； 然后初始化在<beans/>级别指定的默认设置； 然后解析包含的bean定义
	 */
	@Override
	public void registerBeanDefinitions(Document doc, XmlReaderContext readerContext) {
        // 获得XML描述
		this.readerContext = readerContext;
		logger.debug("Loading bean definitions");
        // 获得Document根元素
		Element root = doc.getDocumentElement();
        // 
		doRegisterBeanDefinitions(root);
	}
	/**
	 * Register each bean definition within the given root {@code <beans/>} element.
	 */
	protected void doRegisterBeanDefinitions(Element root) {
		// Any nested <beans> elements will cause recursion in this method. In
		// order to propagate and preserve <beans> default-* attributes correctly,
		// keep track of the current (parent) delegate, which may be null. Create
		// the new (child) delegate with a reference to the parent for fallback purposes,
		// then ultimately reset this.delegate back to its original (parent) reference.
		// this behavior emulates a stack of delegates without actually necessitating one.	 
        // 具体解析过程由BeanDefinitionParserDelegate实现
        // BeanDefinitionParserDelegate定义了XML文件各种元素
		BeanDefinitionParserDelegate parent = this.delegate;
		this.delegate = createDelegate(getReaderContext(), root, parent);

		if (this.delegate.isDefaultNamespace(root)) {
			String profileSpec = root.getAttribute(PROFILE_ATTRIBUTE);
			if (StringUtils.hasText(profileSpec)) {
				String[] specifiedProfiles = StringUtils.tokenizeToStringArray(
						profileSpec, BeanDefinitionParserDelegate.MULTI_VALUE_ATTRIBUTE_DELIMITERS);
				if (!getReaderContext().getEnvironment().acceptsProfiles(specifiedProfiles)) {
					if (logger.isInfoEnabled()) {
						logger.info("Skipped XML bean definition file due to specified profiles [" + profileSpec +
								"] not matching: " + getReaderContext().getResource());
					}
					return;
				}
			}
		}
		// 解析前进行自定义解析，增强解析过程的可扩展性
		preProcessXml(root);
        //从文档的根元素，开始进行Bean定义的文档对象的解析
		parseBeanDefinitions(root, this.delegate);
        // 在解析Bean定义之后进行自定义解析，增加解析过程的可扩展性
		postProcessXml(root);

		this.delegate = parent;
	}
... 省略了createDelegate()方法的解析。整个过程包含parseBeanDefinitionElement()
    parsePropertyElements(Element beanEle,BeanDefinition bd)
    parsePropertySubElement(Element ele,@Nullable BeanDefinition bd, ...)
    parseListElement(Element collections,@Nullable BeanDefinition bd)
    
```

##### BeanDefinition的注册

在这些动作完成以后，用户定义的BeanDefinition信息已经在IoC容器内建立起了自己的数据结构以及相应的数据表示，但此时这些数据还不能让IoC容器直接使用，需要在IoC容器中对这些BeanDefinition数据进行注册。这个注册为IoC容器了提供更友好的使用方式，在DefaultListableBeanFactory中，是通过一个HashMap来持有载入的BeanDefinition的，这个HashMap的定义在DefaultListableBeanFactory可以看到.

在DefaultListableBeanFactory中实现了BeanDefinitionRegistry的接口，这个接口的实现完成BeanDefinition向容器的注册。这个注册过程不复杂，就是把解析得到的BeanDefinition设置到hashMap中去。需要注意的是，如果遇到同名的BeanDefinition的情况，进行处理的时候需要依据allowBeanDefinitionOverriding的配置来完成

**分配注册策略**

```java
	// BeanDefinitionReaderUtils的registerBeanDefinition()方法向Spring IOC容器注册解析Bean，
	/**
	 * Register the given bean definition with the given bean factory.
	 * @param definitionHolder the bean definition including name and aliases
	 * @param registry the bean factory to register with
	 * @throws BeanDefinitionStoreException if registration failed
	 */
	public static void registerBeanDefinition(
			BeanDefinitionHolder definitionHolder, BeanDefinitionRegistry registry)
			throws BeanDefinitionStoreException {
		// 获取解析的BeanDefinition的名称
		// Register bean definition under primary name.
		String beanName = definitionHolder.getBeanName();
        // 向IOC容器注册BeanDefinition
		registry.registerBeanDefinition(beanName, definitionHolder.getBeanDefinition());
		// 如果解析的BeanDefinition有别名向Spring Ioc注册别名
		// Register aliases for bean name, if any.
		String[] aliases = definitionHolder.getAliases();
		if (aliases != null) {
			for (String alias : aliases) {
				registry.registerAlias(beanName, alias);
			}
		}
	}
```

**像容器中进行注册**

```java
	
	// DefaultListableBeanFactory.java	
	/---------------------------------------------------------------------
	// Implementation of BeanDefinitionRegistry interface
	//---------------------------------------------------------------------

	@Override
	public void registerBeanDefinition(String beanName, BeanDefinition beanDefinition)
			throws BeanDefinitionStoreException {
		
		Assert.hasText(beanName, "Bean name must not be empty");
		Assert.notNull(beanDefinition, "BeanDefinition must not be null");
		// 是否是AbstractBeanDefinition的实例
		if (beanDefinition instanceof AbstractBeanDefinition) {
			try {
				((AbstractBeanDefinition) beanDefinition).validate();
			}
			catch (BeanDefinitionValidationException ex) {
				throw new BeanDefinitionStoreException(beanDefinition.getResourceDescription(), beanName,
						"Validation of bean definition failed", ex);
			}
		}
		// 从map中获取BeanDefinition
		BeanDefinition existingDefinition = this.beanDefinitionMap.get(beanName);
		if (existingDefinition != null) {
			if (!isAllowBeanDefinitionOverriding()) {
				throw new BeanDefinitionOverrideException(beanName, beanDefinition, existingDefinition);
			}
			else if (existingDefinition.getRole() < beanDefinition.getRole()) {
				// e.g. was ROLE_APPLICATION, now overriding with ROLE_SUPPORT or ROLE_INFRASTRUCTURE
				if (logger.isInfoEnabled()) {
					logger.info("Overriding user-defined bean definition for bean '" + beanName +
							"' with a framework-generated bean definition: replacing [" +
							existingDefinition + "] with [" + beanDefinition + "]");
				}
			}
			else if (!beanDefinition.equals(existingDefinition)) {
				if (logger.isDebugEnabled()) {
					logger.debug("Overriding bean definition for bean '" + beanName +
							"' with a different definition: replacing [" + existingDefinition +
							"] with [" + beanDefinition + "]");
				}
			}
			else {
				if (logger.isTraceEnabled()) {
					logger.trace("Overriding bean definition for bean '" + beanName +
							"' with an equivalent definition: replacing [" + existingDefinition +
							"] with [" + beanDefinition + "]");
				}
			}
			this.beanDefinitionMap.put(beanName, beanDefinition);
		}
		else {
            // 检查该工厂的Bean创建阶段是否已经开始，即在此期间是否有任何Bean被标记为已创建
			if (hasBeanCreationStarted()) {
				// Cannot modify startup-time collection elements anymore (for stable iteration)不能再修改启动时收集元素（用于稳定的迭代）
				synchronized (this.beanDefinitionMap) {
					this.beanDefinitionMap.put(beanName, beanDefinition);
					List<String> updatedDefinitions = new ArrayList<>(this.beanDefinitionNames.size() + 1);
					updatedDefinitions.addAll(this.beanDefinitionNames);
					updatedDefinitions.add(beanName);
					this.beanDefinitionNames = updatedDefinitions;
					removeManualSingletonName(beanName);
				}
			}
			else {
				// Still in startup registration phase
                // 仍处于启动注册阶段
				this.beanDefinitionMap.put(beanName, beanDefinition);
				this.beanDefinitionNames.add(beanName);
				removeManualSingletonName(beanName);
			}
			this.frozenBeanDefinitionNames = null;
		}

		if (existingDefinition != null || containsSingleton(beanName)) {
            // 重置给定bean的所有bean定义缓存，包括从其派生的bean的缓存
			resetBeanDefinition(beanName);
		}
		else if (isConfigurationFrozen()) {
			clearByTypeCache();
		}
	}
```



**备注：** IoC容器和上下文的初始化一般不包含Bean依赖注入的实现。一般而言，依赖注入发生在应用第一次向容器通过getBean索取Bean时。但有一个例外值得注意，在使用IoC容器时有一个预实例化的配置，这个预实例化是可以配置的，具体来说可以通过在Bean定义信息中的lazyinit属性来设定；有了这个预实例化的特性，用户可以对容器初始化过程作一个微小的控制；从而改变这个被设置了lazyinit属性的Bean的依赖注入的发生，使得这个Bean的依赖注入在IoC容器初始化时就预先完成了

经过上面的步骤IOC容器已经初始化完成了。下面就是IOC容器的依赖注入的实现了。

### IOC容器初始化（基于注解形式的初始化）



### Spring Bean

**什么是bean:**

在 Spring 中，构成应用程序**主干**并由**Spring IoC容器**管理的**对象**称为**bean**。bean是一个由Spring IoC容器实例化、组装和管理的对象。

要使应用程序中的Spring容器成功启动，需要同时具备以下3方面的条件。

`· Spring框架的类包都已经放到应用程序的类路径下。`

`· 应用程序为Spring提供完备的Bean配置信息。`

`· Bean的类都已经放到应用程序的类路径下。`

Spring启动时读取应用程序提供的Bean配置信息，并在Spring容器中生成一份相应的Bean配置注册表，然后根据这张注册表实例化Bean，装配好Bean之间的依赖关系，为上层应用提供准备就绪的运行环境



[![6ikXbn.png](https://s3.ax1x.com/2021/03/01/6ikXbn.png)](https://imgtu.com/i/6ikXbn)

bean 对象也是由Spring IoC容器管理，bean 是一个被实例化，组装，并通过 Spring IoC 容器所管理的对象。这些 bean 是由用容器提供的配置元数据创建的。

Bean配置信息定义了Bean的实现及依赖关系，Spring容器根据各种形式的Bean配置信息在容器内部建立Bean定义注册表，然后根据注册表加载、实例化Bean，并建立Bean和Bean的依赖关系，最后将这些准备就绪的Bean放到Bean缓存池中，以供外层的应用程序调用。

​	`<bean>元素有以下属性：`	id，name,class,singleton,autowire,init-method,destroy-method,depends-on

```xml
<bean id="id" class="创建的bean类" scope="bean的作用域">
<!-- collaborators and configuration for this bean go here -->
</bean>
<bean id="..." class="..." lazy-init="true">
<!-- collaborators and configuration for this bean go here -->
</bean>
<bean id="..." class="..." destroy-method="...">
<!-- collaborators and configuration for this bean go here -->
</bean>
```

#### Bean元素

使用Spring注册的对象 

​			name属性：给被管理对象起名字，根据改名字获取对象。名字可以重复，可以使用特殊字符

​			class属性：被管理对象完整的类名

​			id属性：与name属性一样，名字不可重复，不能使用特殊字符

​			尽量使用name属性

#### Bean作用域

##### singleton	

在spring IoC容器仅存在一个Bean实例，Bean以单例方式存在，默认值

当一个bean的作用域为Singleton，那么Spring IoC容器中只会存在一个共享的bean实例，并且所有对bean的请求，只要id与该bean定义相匹配，则只会返回bean的同一实例。

Singleton是单例类型，就是在创建起容器时就同时自动创建了一个bean的对象，不管你是否使用他都存在了每次获取到的对象都是同一个对象。

注意，Singleton作用域是Spring中的缺省作用域
		`<bean id="..." class="..." scope="singleton"></bean>`	

##### prototype	

​		每次从容器中调用Bean时，都返回一个新的实例，即每次调用getBean()时，相当于执行newXxxBean()这就是平时使用new创建对象的默认方式；

​		表示一个bean定义对应多个对象实例。Prototype作用域的bean会导致在每次对该bean请求（将其注入到另一个bean中，或者以程序的方式调用容器的getBean()方法）时都会创建一个新的bean实例。

​		Prototype是原型类型，它在我们创建容器的时候并没有实例化，而是当我们获取bean的时候才会去创建一个对象，而且我们每次获取到的对象都不是同一个对象。根据经验，对有状态的bean应该使用prototype作用域，而对无状态的bean则应该使用singleton作用域。

​		通常DAO不会被配置成prototype,因为一个Dao不会支持任何会话状态，

##### request	

每次HTTP请求都会创建一个新的Bean，该作用域仅适用于WebApplicationContext环境			

##### session	

同一个HTTP Session共享一个Bean，不同Session使用不同的Bean，仅适用于WebApplicationContext环境

##### global-session	

一般用于Portlet应用环境，改作用于仅适用于WebApplicationContext环境								   

#### Bean的生命周期

[![g432ff.jpg](https://z3.ax1x.com/2021/05/19/g432ff.jpg)](https://imgtu.com/i/g432ff)

声明带有 init-method 和/或 destroy-method 参数的 。init-method 属性指定一个方法，在实例化 bean 时，立即调用该方法。同样，destroy-method 指定一个方法，只有从容器中移除 bean 之后，才能调用该方法
				

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="helloWorld" class="com.tutorialspoint.HelloWorld" init-method="init" destroy-method="destroy">
    	<property name="message" value="Hello World!"/>
    </bean>
</beans>
```

​	如果你有太多具有相同名称的初始化或者销毁方法的 Bean，那么你不需要在每一个 bean 上声明初始化方法和销毁方法。框架使用 元素中的 default-init-method 和 default-destroy-method 属性提供了灵活地配置这种情况，

##### Bean的初始化

**Bean初始化 Bean工厂实现应尽可能支持标准Bean生命周期接口。 全套BeanFactory初始化方法及其标准顺序为：**

- BeanNameAware的setBeanName()
  - 在创建此bean的 `BeanFactory` 中设置bean的名称。
  - 在填充常规bean属性之后 但在 `InitializingBean.afterPropertiesSet（）`类的init回调 或 `自定义 init-method`之前调用
- BeanClassLoaderAware的setBeanClassLoader()
  - 允许bean知道 bean 的回调 `ClassLoader class loader` ;
  - 当前 bean 工厂使用的类加载器来加载bean类。
- BeanFactoryAware的setBeanFactory()
  - 将拥有的工厂提供给 Bean 实例的回调。
  - 在填充常规 bean 属性之后但在初始化回调之前调用，例如 `InitializingBean.afterPropertiesSet()` 或 `自定义的 init-method`。
  - bean 可以立即在工厂中调用方法。
- EnvironmentAware的setEnvironment()
- EmbeddedValueResolverAware的setEmbeddedValueResolver()
  - 通过 `ApplicationContextAware` / `BeanFactoryAware` 接口，这可以替代完整的`ConfigurableBeanFactory` 依赖项。
  - 设置 `StringValueResolver` 以用于解析嵌入式定义值。
- ResourceLoaderAware的setResourceLoade()r （仅在在应用程序上下文中运行时适用）
  - 设置运行该对象的 ResourceLoader。
  - 这可能是 ResourcePatternResolver，可以通过 `instanceof ResourcePatternResolver 进行检查。
- ApplicationEventPublisherAware的setApplicationEventPublisher ()（仅适用于在应用程序上下文中运行的情况）
  - 设置此对象在其中运行的 ApplicationEventPublisher。
  - 在填充正常的 bean 属性之后,但在 InitializingBean.afterPropertiesSet 或 自定义init-method 之类的 init 回调之前调用。
  - 在 ApplicationContextAware.setApplicationContext 之前调用
- MessageSourceAware的setMessageSource ()（仅适用于在应用程序上下文中运行的情况）
- ApplicationContextAware的setApplicationContext() （仅适用于在应用程序上下文中运行的情况）
- ServletContextAware的setServletContext() （仅适用于在Web应用程序上下文中运行的情况）
- BeanPostProcessors的postProcessBeforeInitialization方法
- InitializingBean的afterPropertiesSet()
- 自定义的初始化方法(init-method)定义(a custom init-method Definition)
- BeanPostProcessors的postProcessAfterInitialization()方法

**简版：**

```
1.<bean>标签使用autowire属性，会进行自动装配，

2.通过get(),set()方法。

3.如果实现BeanNameAware接口容器会将调用bean的setBeanName()方法传递bean的ID

4.如果实现BeanFactoryAware接口，容器会将调用的bean得setBeanfactory()方法注入bean，

5.如果注册了BeanPostProcessor接口的实现类，将调用这个实现类的postProcessBeforeInitialization()方法；完成bean的预处理

6.如果是实现了InitializingBean接口容器会调用JavaBean的afterPropertiesSet()方法修改JavaBean的属性。

7.在XML中配置Bean时如果用init-method属性指定来了初始化方法容器会执行指定的方法

8.如果注册了BeanPostProcessor的实现类，将调用实现类的postProcessAfterInitialization()方法完成后置处理方法
```

##### Bean的销毁

1.在销毁bean之前如果Bean实现了DisposableBean接口，容器会调用bean的destroy()方法来完成销毁工作，

2.如果在bean定义了指定的销毁方法了在bean被销毁前会先执行指定的方法，在同时指定的时候DisposableBean接口时有优先权

#### Bean的后置处理

BeanPostProcessor 接口定义回调方法，你可以实现该方法来提供自己的实例化逻辑，依赖解析逻辑等。你也可以在 Spring 容器通过插入一个或多个 BeanPostProcessor 的实现来完成实例化，配置和初始化一个bean之后实现一些自定义逻辑回调方法。

​		你可以配置多个 BeanPostProcesso r接口，通过设置 BeanPostProcessor 实现的 Ordered 接口提供的 order 属性来控制这些 BeanPostProcessor 接口的执行顺序。

​		BeanPostProcessor 可以对 bean（或对象）实例进行操作，这意味着 Spring IoC 容器实例化一个 bean 实例，然后 BeanPostProcessor 接口进行它们的工作。

​		ApplicationContext 会自动检测由 BeanPostProcessor 接口的实现定义的 bean，注册这些 bean 为后置处理器，然后通过在容器中创建 bean，在适当的时候调用它。调用的时候只需要实现BeanPostProcessor接口

#### Bean定义继承

bean 定义可以包含很多的配置信息，包括构造函数的参数，属性值，容器的具体信息例如初始化方法，静态工厂方法名，等等。

子 bean 的定义继承父定义的配置数据。子定义可以根据需要重写一些值，或者添加其他值。

Spring Bean 定义的继承与 Java 类的继承无关，但是继承的概念是一样的。你可以定义一个父 bean 的定义作为模板和其他子 bean 就可以从父 bean 中继承所需的配置。当你使用基于 XML 的配置元数据时，通过使用父属性，指定父 bean 作为该属性的值来表明子 bean 的定义。

```xml
xml配置信息：
<?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="helloWorld" class="xxxparent">
        <property name="message1" value="Hello World!"/>
        <property name="message2" value="Hello Second World!"/>
    </bean>
    <bean id="helloIndia" class="xxx" parent="helloWorld">
        <property name="message1" value="Hello India!"/>
        <property name="message3" value="Namaste India!"/>
    </bean>
</beans>
## 使用的时候可以直接定义模板，这样就可以方便让其他子bean定义使用：需要指定抽象属性为true;
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="beanTeamplate" abstract="true">
        <property name="message1" value="Hello World!"/>
        <property name="message2" value="Hello Second World!"/>
        <property name="message3" value="Namaste India!"/>
    </bean>
    <bean id="helloIndia" class="com.tutorialspoint.HelloIndia" parent="beanTeamplate">
        <property name="message1" value="Hello India!"/>
        <property name="message3" value="Namaste India!"/>
    </bean>
</beans>
```
### 依赖注入（DI）

**依赖注入(DI)的设计模式是用来定义对象彼此间的依赖** 主要有以下方式注入：

- Setter方法注入
- 构造器注入（构造函数注入）
- 接口注入
- 设值函数注入

#### 接口注入

​		基于接口将调用与实现分离，必须实现容器所规定的接口使程序代码和容器的API绑定在一起，不是理想的依赖注入

#### Setter注入

​	基于Java的setter方法的属性赋值最为广泛应用

​	可以混合这两种方法，基于构造函数和基于 setter 方法的 DI，然而使用有强制性依存关系的构造函数和有可选依赖关系的 setter是一个好的做法。

​	代码是 DI 原理的清洗机，当对象与它们的依赖关系被提供时，解耦效果更明显。对象不查找它的依赖关系，也不知道依赖关系的位置或类，而这一切都由 Spring 框架控制的。	

​	例如：

​		一个简单的Javabean就是有一个私有的属性对应getter() setter()方法，来实现对属性的封装；

```java
class User {
    private  String name;
    private  String  sex;
    private  int age;
    //setter();
}
## applicationContext.xml配置：
<bean name="user" class="xxx.User">
   <property name="name">
    	<value>sssd</value>
   </property>
   <property name="age">
        <value>12</value>
   </property>
   <property name="sex">
        <value>男</value>
   </property>
</bean>
直接获取到bean用User对象接收，然后即可以调用对象的属性
```
#### 构造函数注入

> 当容器调用带有一组参数的类构造函数时，基于构造函数的 DI 就完成了，其中每个参数代表一个对其他类的依赖。基于构造方法为属性赋值，容器通过调用类的构造方法将其进行依赖注入
> 		<constructor-arg>是<bean>元素的子元素，通过 <constructor-arg>的子元素<value>可以传参
> 		<ref>元素用于引入其他的Javabean对象

```java
public class TextEditor {
    private SpellChecker spellChecker;
    public TextEditor(SpellChecker spellChecker) {
        System.out.println("Inside TextEditor constructor." );
        this.spellChecker = spellChecker;
    }
    public void spellCheck() {
        spellChecker.checkSpelling();
    }
}
public class SpellChecker {
    public SpellChecker(){
        System.out.println("Inside SpellChecker constructor." );
    }
    public void checkSpelling() {
        System.out.println("Inside checkSpelling." );
    } 
}
public static void main(String[] args) {
    ApplicationContext context = 
        new ClassPathXmlApplicationContext("Beans.xml");
    TextEditor te = (TextEditor) context.getBean("textEditor");
    te.spellCheck();
}
beans.xml文件：
<!-- Definition for textEditor bean -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor">
   <constructor-arg ref="spellChecker"/>
</bean>
<!-- Definition for spellChecker bean -->
<bean id="spellChecker" class="com.tutorialspoint.SpellChecker">
</bean>
这样的好处是实例化对象的同时完成属性的初始化
class User {
	private  String name;
	private  String  sex;
	private  int age;
	//setter();
	User(String name,int age,string sex){
		this.name=name;
		this.age=age;
		this.sex=sex;
	}
}
在applicationContext.xml为其赋值
    <bean name="user" class="com.xxx.User">
        <constructor-arg>
        	<value>小强</value>
        </contructor-arg>
        <constructor-arg>
        	<value>12</value>
        </contructor-arg>
        <constructor-arg>
        <value>男</value>
        </contructor-arg>
        构造函数注入设置执行哪一个构造函数，主要用 index属性进行测试：
        <!-- 构造函数注入
        constructor-age 用于设置属性的 
        name 用于设置构造函数的参数名
        index 用于设置参数的索引
        type 用于设置参数类型
        ref 用于引入其他对象，也需要先把其他对象给实例出来
        -->
        <bean name="user4" class="com.leo.demo.User">
            <constructor-arg name="name" index="0" type="String" value="黑市"></constructor-arg>
            <constructor-arg name="car" index="1" ref="car"></constructor-arg>
        </bean>
        <bean name="user5" class="com.leo.demo.User">
             <constructor-arg name="name" index="1" type="String" value="黑市"></constructor-arg>
             <constructor-arg name="car" index="0" ref="car"></constructor-arg>
          </bean>
构造函数如下：
/**
 * 指定到这个构造：
 * @param car
 * @param name
 */
public User(Car car,String name) {
     this.car=car;
     this.name=name;
     System.out.println("car,name,");
}
/**
 * 
 * @param name
 * @param car
*/
public User(String name,Car car) {
    this.car=car;
    this.name=name;
    System.out.println("name,car");
}
使用的时候：
 @Test
public void test5() {
    ClassPathXmlApplicationContext context =new ClassPathXmlApplicationContext("Beans.xml");
    User bean=(User)context.getBean("user4");
    System.out.println(bean.getCar().getName());
    context.close();
}
/**
 * 构造函数注入指定走哪一个构造函数配置走car nam
@Test
public void test6() {
    ClassPathXmlApplicationContext context =new ClassPathXmlApplicationContext("Beans.xml");
    User bean=(User)context.getBean("user5");
    System.out.println(bean.getCar().getName());
    context.close();
}					
```

#### 设值函数注入：

当容器调用一个无参的构造函数或一个无参的静态 factory 方法来初始化你的 bean 后，通过容器在你的 bean 上调用设值函数，基于设值函数的 DI 就完成了。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

    <!-- Definition for textEditor bean -->
    <bean id="textEditor" class="com.tutorialspoint.TextEditor">
    <property name="spellChecker" ref="spellChecker"/>
    </bean>

    <!-- Definition for spellChecker bean -->
    <bean id="spellChecker" class="com.tutorialspoint.SpellChecker">
    </bean>

</beans>
应该注意定义在基于构造函数注入和基于设值函数注入中的 Beans.xml 文件的区别。
唯一的区别就是在基于构造函数注入中，我们使用的是〈bean〉标签中的〈constructor-arg〉元素，而在基于设值函数的注入中，我们使用的是〈bean〉标签中的〈property〉元素。
如果你要把一个引用传递给一个对象，那么你需要使用 标签的 ref 属性，而如果你要直接传递一个值，那么你应该使用 value 属性。
```
#### 注入其他


```xml
注入内部beans:
匿名内部类的与xml
<bean id="outerBean" class="...">
	<property name="target">
		<bean id="innerBean" class="..."/>
	</property>
</bean>
注入集合：
Java Collection 类型 List、Set、Map 和 Properties，为了处理这种情况，Spring提供了四种类型的集合：
<list><set><map><props>
例如：
public class JavaCollection {
List addressList;
Set  addressSet;
Map  addressMap;
Properties addressProp;
....
}
配置形式：
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

    <!-- Definition for javaCollection -->
    <bean id="javaCollection" class="com.tutorialspoint.JavaCollection">

    <!-- results in a setAddressList(java.util.List) call -->
    <property name="addressList">
        <list>
            <value>INDIA</value>
            <value>Pakistan</value>
            <value>USA</value>
            <value>USA</value>
        </list>
    </property>

    <!-- results in a setAddressSet(java.util.Set) call -->
    <property name="addressSet">
        <set>
            <value>INDIA</value>
            <value>Pakistan</value>
            <value>USA</value>
            <value>USA</value>
        </set>
    </property>

    <!-- results in a setAddressMap(java.util.Map) call -->
    <property name="addressMap">
        <map>
            <entry key="1" value="INDIA"/>
            <entry key="2" value="Pakistan"/>
            <entry key="3" value="USA"/>
            <entry key="4" value="USA"/>
        </map>
    </property>

    <!-- results in a setAddressProp(java.util.Properties) call -->
    <property name="addressProp">
        <props>
            <prop key="one">INDIA</prop>
            <prop key="two">Pakistan</prop>
            <prop key="three">USA</prop>
            <prop key="four">USA</prop>
        </props>
    </property>
    </bean>
</beans>
```
### IOC容器依赖注入原理探索

由IOC容器已经初始化完毕，IoC容器初始化的过程,主要完成的工作是在IoC容器中建立 BeanDefinition 数据映射,并没有看到IoC容器对Bean依赖关系进行注入,假设当前IoC容器已经载入用户定义的Bean信息,依赖注入主要发生在两个阶段正常情况下,由用户第一次向IoC容器索要Bean时触发但我们可以在 BeanDefinition 信息中通过控制 lazy-init 属性来让容器完成对Bean的预实例化,即在初始化的过程中就完成某些Bean的依赖注入的过程.

```java
	   AnnotationConfigApplicationContext configApplicationContext = new AnnotationConfigApplicationContext(ApplicationContextConfig.class);

        // 从spring ioc 容器获取 person
        Person person = (Person) configApplicationContext.getBean("person");
        System.out.println(person.toString());
```

从AbstractBeanFactory#getBean()进行探索,这个方法将bean定义转换为了对象 

```java
	@Override
	public Object getBean(String name) throws BeansException {
		return doGetBean(name, null, null, false);
	}
```

```java
/**
*	返回一个实例，该实例可以是指定bean的共享或独立的。 
    参数：
        名称–要检索的bean的名称
        requiredType –要检索的bean的必需类型
        args –使用显式参数创建bean实例时要使用的参数（仅在创建新实例而不是检索现有实例时才应用）
        typeCheckOnly –是否为类型检查而不是实际使用获取实例
    返回值：Bean的一个实例
    抛出：BeansException如果无法创建Bea
*/
protected <T> T doGetBean(final String name, @Nullable final Class<T> requiredType,
			@Nullable final Object[] args, boolean typeCheckOnly) throws BeansException {
		// 返回Bean名称，必要时去除工厂取消引用前缀，并将别名解析为规范名称
		final String beanName = transformedBeanName(name);
		Object bean;
		/**
		* 检查缓存中或者实例工厂中是否由对应的实例
		* 在创建单例bean时会存在依赖注入的情况，而在创建依赖的时候为了避免循环依赖
		* spring创建bean的原则是不等bean创建完成就会将创建bean的ObjectFactory提早曝光。也就是将ObjectFactory加入缓存，一旦下个bean创建时候需要依赖上个bean直接使用ObjectFactory
		* 
		**/
		// Eagerly check singleton cache for manually registered singletons.
    	// 检查单例缓存中是否有手动注册的单例
    	// getSingleton返回以给定名称注册的（原始）单例对象。检查已经实例化的单例，并且还允许对当前创建的单例的早期引用（解析循环引用）
		Object sharedInstance = getSingleton(beanName);
		if (sharedInstance != null && args == null) {
			if (logger.isDebugEnabled()) {
                // 返回指定的单例bean当前是否正在创建（在整个工厂内）
				if (isSingletonCurrentlyInCreation(beanName)) {
					logger.debug("Returning eagerly cached instance of singleton bean '" + beanName +
							"' that is not fully initialized yet - a consequence of a circular reference");
				}
				else {
					logger.debug("Returning cached instance of singleton bean '" + beanName + "'");
				}
			}
            // 获取给定bean实例的对象，如果是FactoryBean，则可以是bean实例本身或其创建的对象,就是返回指定方法返回的实例
			bean = getObjectForBeanInstance(sharedInstance, name, beanName, null);
		}

		else {
			// Fail if we're already creating this bean instance:
			// We're assumably within a circular reference.
            // 如果我们已经在创建这个bean实例，则失败：
            //我们大概在循环引用中。isPrototypeCurrentlyInCreation()方法返回指定的原型bean是否当前正在创建中（在当前线程内）
            // 只有在单例情况下才会尝试解决循环依赖，原型模式如果存在A中又B的属性，B中有A的属性，那么当依赖注入的时候就会产生当A还未创建完的时候，因为对B的再次返回创建A造成循环依赖的情况
			if (isPrototypeCurrentlyInCreation(beanName)) {
				throw new BeanCurrentlyInCreationException(beanName);
			}

			// Check if bean definition exists in this factory.
            // 检查该工厂中是否存在bean定义
			BeanFactory parentBeanFactory = getParentBeanFactory();
            // 如果beanDefinitionMap中不包含beanName则尝试从parentBeanFactory检查
			if (parentBeanFactory != null && !containsBeanDefinition(beanName)) {
				// Not found -> check parent.
                // 确定原始bean名称，将本地定义的别名解析为规范名称
				String nameToLookup = originalBeanName(name);
				if (parentBeanFactory instanceof AbstractBeanFactory) {
					return ((AbstractBeanFactory) parentBeanFactory).doGetBean(
							nameToLookup, requiredType, args, typeCheckOnly);
				}
                // 递归到BeanFactory中找
				else if (args != null) {
					// Delegation to parent with explicit args.
                    // 使用显式参数委派给父级.返回一个实例，该实例可以是指定bean的共享或独立的。允许指定显式构造函数自变量/工厂方法自变量，覆盖Bean定义中指定的默认自变量（如果有
					return (T) parentBeanFactory.getBean(nameToLookup, args);
				}
				else {
                    // 没有参数->委托给标准的getBean方法
					// No args -> delegate to standard getBean method.
                    // 返回一个实例，方法getBean(name,type)该实例可以是指定bean的共享或独立的。行为与getBean(String)相同，但是如果bean不是必需的类型，则通过抛出BeanNotOfRequiredTypeException来提供类型安全性的度量。 这意味着如getBean(String)那样，不能正确地强制转换结果时抛出ClassCastException。将别名转换回相应的规范bean名称。 将询问父工厂是否在该工厂实例中找不到该bean
					return parentBeanFactory.getBean(nameToLookup, requiredType);
				}
			}
			// 如果不是仅仅做类型检查则是创建bean,这里要进行记录
			if (!typeCheckOnly) {
                // 将指定的bean标记为已经创建（或将要创建）。这允许bean工厂优化其缓存以重复创建指定的bean
				markBeanAsCreated(beanName);
			}

			try {
                // 将存储XMl配置文件的GernericBeanDefinition转换为RootBeanDefinition,如何指定BeanName是子Bean的话同时会合并父类的相关属性
                // 返回合并的RootBeanDefinition，如果指定的bean对应于子bean定义，则遍历父bean定义
				final RootBeanDefinition mbd = getMergedLocalBeanDefinition(beanName);
                // 检查给定的合并bean定义，可能会引发验证异常
				checkMergedBeanDefinition(mbd, beanName, args);
				// 保证当前bean依赖的bean的初始化
				// Guarantee initialization of beans that the current bean depends on.
                // 返回该bean依赖的bean名称
				String[] dependsOn = mbd.getDependsOn();
                // 若存在依赖则需要递归实例化依赖的bean
				if (dependsOn != null) {
					for (String dep : dependsOn) {
						if (isDependent(beanName, dep)) {
							throw new BeanCreationException(mbd.getResourceDescription(), beanName,
									"Circular depends-on relationship between '" + beanName + "' and '" + dep + "'");
						}
                        // 为给定的bean注册一个从属bean，要在给定的bean被销毁之前将其销毁
                        // 缓存依赖调用
						registerDependentBean(dep, beanName);
						getBean(dep);
					}
				}
				// 创建bean实例
				// Create bean instance.
				if (mbd.isSingleton()) {
					sharedInstance = getSingleton(beanName, () -> {
						try {
							return createBean(beanName, mbd, args);
						}
						catch (BeansException ex) {
							// Explicitly remove instance from singleton cache: It might have been put there
							// eagerly by the creation process, to allow for circular reference resolution.
							// Also remove any beans that received a temporary reference to the bean.
                            //从单例缓存中显式删除实例：创建过程可能将它
                            //急切地放置在那里，以允许循环引用解析。
                            //还删除所有收到对bean的临时引用的bean
                            // 销毁给定的bean。 如果找到了相应的一次性Bean实例，则委托destroyBean
							destroySingleton(beanName);
							throw ex;
						}
					});
                    // 获取给定bean实例的对象，如果是FactoryBean，则可以是bean实例本身或其创建的对象
					bean = getObjectForBeanInstance(sharedInstance, name, beanName, mbd);
				}

				else if (mbd.isPrototype()) {
					// It's a prototype -> create a new instance.
					Object prototypeInstance = null;
					try {
                        // 创建原型之前进行回调。默认实现将原型注册为当前正在创建中
						beforePrototypeCreation(beanName);
                        // 为给定的合并bean定义（和参数）创建一个bean实例。 如果是子定义，则Bean定义将已经与父定义合并。所有bean检索方法都委托该方法进行实际的bean创建
						prototypeInstance = createBean(beanName, mbd, args);
					}
					finally {
                        //创建原型后进行回调。默认实现将原型标记为不再创建
						afterPrototypeCreation(beanName);
					}
					bean = getObjectForBeanInstance(prototypeInstance, name, beanName, mbd);
				}

				else {
                    // 获取scope域
					String scopeName = mbd.getScope();
					final Scope scope = this.scopes.get(scopeName);
					if (scope == null) {
						throw new IllegalStateException("No Scope registered for scope name '" + scopeName + "'");
					}
					try {
						Object scopedInstance = scope.get(beanName, () -> {
                            // 创建原型之前进行回调。默认实现将原型注册为当前正在创建中
							beforePrototypeCreation(beanName);
							try {
								return createBean(beanName, mbd, args);
							}
							finally {
                                // 创建原型后进回调
								afterPrototypeCreation(beanName);
							}
						});
                        // 获取给定bean实例的对象，如果是FactoryBean，则可以是bean实例本身或其创建的对象
						bean = getObjectForBeanInstance(scopedInstance, name, beanName, mbd);
					}
					catch (IllegalStateException ex) {
						throw new BeanCreationException(beanName,
								"Scope '" + scopeName + "' is not active for the current thread; consider " +
								"defining a scoped proxy for this bean if you intend to refer to it from a singleton",
								ex);
					}
				}
			}
			catch (BeansException ex) {
                // Bean创建失败后，对缓存的元数据执行适当的清理
				cleanupAfterBeanCreationFailure(beanName);
				throw ex;
			}
		}
		// 检查所需的类型是否与实际bean实例的类型匹配
		// Check if required type matches the type of the actual bean instance.
		if (requiredType != null && !requiredType.isInstance(bean)) {
			try {
                // 获取此BeanFactory使用的类型转换器。 这可能是每个调用的新实例，因为TypeConverters通常不是线程安全的。如果默认的PropertyEditor机制处于活动状态，则返回的TypeConverter将知道所有已注册的自定义编辑器
                // 将值转换为所需的类型（如果需要，则从字符串）。从String到任何类型的转换通常将使用PropertyEditor类的setAsText方法或ConversionService中的Spring Converter
				T convertedBean = getTypeConverter().convertIfNecessary(bean, requiredType);
				if (convertedBean == null) {
					throw new BeanNotOfRequiredTypeException(name, requiredType, bean.getClass());
				}
				return convertedBean;
			}
			catch (TypeMismatchException ex) {
				if (logger.isDebugEnabled()) {
					logger.debug("Failed to convert bean '" + name + "' to required type '" +
							ClassUtils.getQualifiedName(requiredType) + "'", ex);
				}
				throw new BeanNotOfRequiredTypeException(name, requiredType, bean.getClass());
			}
		}
		return (T) bean;
	}
```

仅从代码量上就能看出来bean的加载经历了一个相当复杂的过程，其中涉及各种各样的考虑。相信读者细心阅读上面的代码，并参照部分代码注释，是可以粗略地了解整个Spring加载bean的过程。对于加载过程中所涉及的步骤大致如下。

1. **转换对应beanName**。或许很多人不理解转换对应beanName是什么意思，传入的参数name不就是beanName吗？其实不是，这里传入的参数可能是别名，也可能是FactoryBean，所以需要进行一系列的解析，这些解析内容包括如下内容。

   ​    ● 去除FactoryBean的修饰符，也就是如果name="&aa"，那么会首先去除&而使name="aa"。

   ​	● 取指定alias所表示的最终beanName，例如别名A指向名称为B的bean则返回B；若别名A指向别名B，别名B又指向名称为C的bean则返回C。

2. **尝试从缓存中加载单例**。单例在Spring的同一个容器内只会被创建一次，后续再获取bean，就直接从单例缓存中获取了。当然这里也只是尝试加载，首先尝试从缓存中加载，如果加载不成功则再次尝试从singletonFactories中加载。因为在创建单例bean的时候会存在依赖注入的情况，而在创建依赖的时候为了避免循环依赖，在Spring中创建bean的原则是不等bean创建完成就会将创建bean的ObjectFactory提早曝光加入到缓存中，一旦下一个bean创建时候需要依赖上一个bean则直接使用ObjectFactory（后面章节会对循环依赖重点讲解）。

3. **bean的实例化**。如果从缓存中得到了bean的原始状态，则需要对bean进行实例化。这里有必要强调一下，缓存中记录的只是最原始的bean状态，并不一定是我们最终想要的bean。举个例子，假如我们需要对工厂bean进行处理，那么这里得到的其实是工厂bean的初始状态，但是我们真正需要的是工厂bean中定义的factory-method方法中返回的bean，而getObjectForBeanInstance就是完成这个工作的，后续会详细讲解。

4. **原型模式的依赖检查**。只有在单例情况下才会尝试解决循环依赖，如果存在A中有B的属性，B中有A的属性，那么当依赖注入的时候，就会产生当A还未创建完的时候因为对于B的创建再次返回创建A，造成循环依赖，也就是情况：isPrototypeCurrentlyInCreation(beanName)判断true。

5. **检测parentBeanFactory**。从代码上看，如果缓存没有数据的话直接转到父类工厂上去加载了，这是为什么呢？可能读者忽略了一个很重要的判断条件：parentBeanFactory != null &&!containsBean Definition (beanName)，parentBeanFactory != null。parentBeanFactory如果为空，则其他一切都是浮云，这个没什么说的，但是!containsBeanDefinition(beanName)就比较重要了，它是在检测如果当前加载的XML配置文件中不包含beanName所对应的配置，就只能到parentBeanFactory去尝试下了，然后再去递归的调用getBean方法

6. **将存储XML配置文件的GernericBeanDefinition转换为RootBeanDefinition**。因为从XML配置文件中读取到的Bean信息是存储在GernericBeanDefinition中的，但是所有的Bean后续处理都是针对于RootBeanDefinition的，所以这里需要进行一个转换，转换的同时如果父类bean不为空的话，则会一并合并父类的属性。

7. **寻找依赖**。因为bean的初始化过程中很可能会用到某些属性，而某些属性很可能是动态配置的，并且配置成依赖于其他的bean，那么这个时候就有必要先加载依赖的bean，所以，在Spring的加载顺寻中，在初始化某一个bean的时候首先会初始化这个bean所对应的依赖。

8. **针对不同的scope进行bean的创建**。我们都知道，在Spring中存在着不同的scope，其中默认的是singleton，但是还有些其他的配置诸如prototype、request之类的。在这个步骤中，Spring会根据不同的配置进行不同的初始化策略。

9. **类型转换**。程序到这里返回bean后已经基本结束了，通常对该方法的调用参数requiredType是为空的，但是可能会存在这样的情况，返回的bean其实是个String，但是requiredType却传入Integer类型，那么这时候本步骤就会起作用了，它的功能是将返回的bean转换为requiredType所指定的类型。当然，String转换为Integer是最简单的一种转换，在Spring中提供了各种各样的转换器，用户也可以自己扩展转换器来满足需求

#### FactoryBean

Spring通过反射机制利用bean的class属性指定实现类来实例化bean.实例化bean过程比较复杂，如果按照传统的方式，则需要在<bean>中提供大量的配置信息，配置方式的灵活性是受限的，这时采用编码的方式可能会得到一个简单的方案。Spring为此提供了一个org.Springframework.bean.factory.FactoryBean的工厂类接口，用户可以通过实现该接口定制实例化bean的逻辑.看下FactoryBean的源码：

```java
package org.Springframework.beans.factory;
public interface FactoryBean<T>{
	// 返回由factoryBean创建的bean实例，如果是isSingleton()返回true,将该实例放到spring容器中的单例缓冲池中。
	T getObjec() throw Exception;
	// 返回由FactoryBean创建的bean实例的作用域是single还是propertype.
	Class<?> getObjectType();
	// 返回factoryBean创建的bean类型
	boolean isSingleton()
}
```

#### 缓存中获取单例bean

单例在Spring的同一个容器内只会被创建一次，后续再获取bean直接从单例缓存中获取，当然这里也只是尝试加载，首先尝试从缓存中加载，然后再次尝试尝试从singletonFactories中加载。因为在创建单例bean的时候会存在依赖注入的情况，而在创建依赖的时候为了避免循环依赖， Spring创建bean的原则是不等bean创建完成就会将创建bean的ObjectFactory提早曝光加入到缓存中，一旦下一个bean创建时需要依赖上个bean，则直接使用ObjectFactory. DefaultSingletonBeanRegistry#getSingleton()

```java
	@Override
	@Nullable
	public Object getSingleton(String beanName) {
        // 参数true设置标识允许早期依赖
		return getSingleton(beanName, true);
	}
	/**
	 * 返回以给定名称注册的（原始）单例对象。
检查已经实例化的单例，并且还允许对当前创建的单例的早期引用（解析循环引用
	 * @param beanName the name of the bean to look for
	 * @param allowEarlyReference whether early references should be created or not
	 * @return the registered singleton object, or {@code null} if none found
	 */
	@Nullable
	protected Object getSingleton(String beanName, boolean allowEarlyReference) {
        // 检查缓存中是否存在实例
		Object singletonObject = this.singletonObjects.get(beanName);
		if (singletonObject == null && isSingletonCurrentlyInCreation(beanName)) {
            // 如果为空，则锁定全局变量进行处理
			synchronized (this.singletonObjects) {
                // 如果bean正在加载则不处理
				singletonObject = this.earlySingletonObjects.get(beanName);
				if (singletonObject == null && allowEarlyReference) {
                    // 当某些方法需要提前初始化的时候则会调用addSingletonFactory方法将对应的ObjectFactory初始化策略，存储在singletonFactories
					ObjectFactory<?> singletonFactory = this.singletonFactories.get(beanName);
					if (singletonFactory != null) {
                        // 调用预先设定的getObject方法
						singletonObject = singletonFactory.getObject();
						// 记录在缓存中 earlySingletonObjects和singletonFactories互斥
                        // 早期的单例对象bean的缓存：bean名称-> bean实例
                        // 与singletonObjects不同之处在于，当一个单例bean被放到这里后那么bean还在创建过程中，就可以通过getBean方法获取到了，其目的检查循环引用
                        this.earlySingletonObjects.put(beanName, singletonObject);	
                        // 单例工厂的缓存就是个map：Bean名称-> ObjectFactory
						this.singletonFactories.remove(beanName);
					}
				}
			}
		}
		return singletonObject;
	}
```

这个方法因为涉及循环依赖的检测，以及涉及很多变量的记录存取，所以让很多读者摸不着头脑。这个方法首先尝试从singletonObjects里面获取实例，如果获取不到再从earlySingleton Objects里面获取，如果还获取不到，再尝试从singletonFactories里面获取beanName对应的ObjectFactory，然后调用这个ObjectFactory的getObject来创建bean，并放到earlySingleton Objects里面去，并且从singletonFacotories里面remove掉这个ObjectFactory，而对于后续的所有内存操作都只为了循环依赖检测时候使用，也就是在allowEarlyReference为true的情况下才会使用

#### 从bean的实例中获取对象

主要是针对的这一段代码进行分析：AbstractBeanFactory#getObjectForBeanInstance()

```java
// AbstractBeanFactory.java中
protected Object getObjectForBeanInstance(
			Object beanInstance, String name, String beanName, @Nullable RootBeanDefinition mbd) {
		// 如果指定的name是相关的工厂（以&前缀）且beanInstance又不是FactoryBean类型则验证不通过
		// Don't let calling code try to dereference the factory if the bean isn't a factory.
		if (BeanFactoryUtils.isFactoryDereference(name) && !(beanInstance instanceof FactoryBean)) {
			throw new BeanIsNotAFactoryException(transformedBeanName(name), beanInstance.getClass());
		}
		// 现在我们有了bean实例，它可以是普通bean或FactoryBean。 
    	//如果它是一个FactoryBean，我们将使用它创建一个bean实例，但如果用户想要直接获取工厂实例而不是工厂的getObject方法对应的实例那么传入的name应该加前缀&
		// Now we have the bean instance, which may be a normal bean or a FactoryBean.
		// If it's a FactoryBean, we use it to create a bean instance, unless the
		// caller actually wants a reference to the factory.
		if (!(beanInstance instanceof FactoryBean) || BeanFactoryUtils.isFactoryDereference(name)) {
			return beanInstance;
		}
		// 加载FactoryBean
		Object object = null;
		if (mbd == null) {
            // 尝试从缓存获取bean
			object = getCachedObjectForFactoryBean(beanName);
		}
		if (object == null) {
			// Return bean instance from factory.
			FactoryBean<?> factory = (FactoryBean<?>) beanInstance;
			// Caches object obtained from FactoryBean if it is a singleton.
			if (mbd == null && containsBeanDefinition(beanName)) {
				mbd = getMergedLocalBeanDefinition(beanName);
			}
			boolean synthetic = (mbd != null && mbd.isSynthetic());
			object = getObjectFromFactoryBean(factory, beanName, !synthetic);
		}
		return object;
	}
```

从上面的代码来看，其实这个方法并没有什么重要的信息，大多是些辅助代码以及一些功能性的判断，而真正的核心代码却委托给了getObjectFromFactoryBean，

我们来看看getObjectForBeanInstance中的所做的工作。


（1）对FactoryBean正确性的验证。

（2）对非FactoryBean不做任何处理。

（3）对bean进行转换。

（4）将从Factory中解析bean的工作委托给getObjectFromFactoryBean

#### 获取单例（当缓存中没有bean的实例）

在AbstractBeanFactory#doGetBean()方法中有判断在bean的加载过程如果缓存中不存已经加载的单例bean就需要从头开始bean的加载过程。以下就是获取单例bean的实现

```java
public Object getSingleton(String beanName, ObjectFactory<?> singletonFactory) {
		Assert.notNull(beanName, "Bean name must not be null");
    	// 同步变量map
		synchronized (this.singletonObjects) {
            // 从map中获取指定name的bean对象，判断适度加载过，因为singleton模式其实就是复用以前创建的bean
			Object singletonObject = this.singletonObjects.get(beanName);
            // 如果bean为空可以进行singletonObject的bean的初始化
			if (singletonObject == null) {
				if (this.singletonsCurrentlyInDestruction) {
					throw new BeanCreationNotAllowedException(beanName,
							"Singleton bean creation not allowed while singletons of this factory are in destruction " +
							"(Do not request a bean from a BeanFactory in a destroy method implementation!)");
				}
				if (logger.isDebugEnabled()) {
					logger.debug("Creating shared instance of singleton bean '" + beanName + "'");
				}
                // 创建单例之前的回调。默认实现将单例注册为当前正在创建中   其实就是做标记
				beforeSingletonCreation(beanName);
				boolean newSingleton = false;
				boolean recordSuppressedExceptions = (this.suppressedExceptions == null);
				if (recordSuppressedExceptions) {
					this.suppressedExceptions = new LinkedHashSet<>();
				}
				try {
                    // 初始化bean
					singletonObject = singletonFactory.getObject();
					newSingleton = true;
				}
				catch (IllegalStateException ex) {
					// Has the singleton object implicitly appeared in the meantime ->
					// if yes, proceed with it since the exception indicates that state.
					singletonObject = this.singletonObjects.get(beanName);
					if (singletonObject == null) {
						throw ex;
					}
				}
				catch (BeanCreationException ex) {
					if (recordSuppressedExceptions) {
						for (Exception suppressedException : this.suppressedExceptions) {
							ex.addRelatedCause(suppressedException);
						}
					}
					throw ex;
				}
				finally {
					if (recordSuppressedExceptions) {
						this.suppressedExceptions = null;
					}
                    // 创建单例后的回调。默认实现将单例标记为不在创建中   其实就是做标记
					afterSingletonCreation(beanName);
				}
                // 新的singleton为true
				if (newSingleton) {
                    // 加入缓存
					addSingleton(beanName, singletonObject);
				}
			}
			return singletonObject;
		}
	}
```

上述代码中其实是使用了回调方法，使得程序可以在单例创建的前后做一些准备及处理操作，而真正的获取单例bean的方法其实并不是在此方法中实现的，其实现逻辑是在ObjectFactory类型的实例singletonFactory中实现的。而这些准备及处理操作包括如下内容。

（1）检查缓存是否已经加载过。

（2）若没有加载，则记录beanName的正在加载状态。

（3）加载单例前记录加载状态。

```java
/**
	 * Callback before singleton creation.
	 * <p>The default implementation register the singleton as currently in creation.
	 * @param beanName the name of the singleton about to be created
	 * @see #isSingletonCurrentlyInCreation
	 */
	protected void beforeSingletonCreation(String beanName) {
		if (!this.inCreationCheckExclusions.contains(beanName) && !this.singletonsCurrentlyInCreation.add(beanName)) {
			throw new BeanCurrentlyInCreationException(beanName);
		}
	}

	/**
	 * Callback after singleton creation.
	 * <p>The default implementation marks the singleton as not in creation anymore.
	 * @param beanName the name of the singleton that has been created
	 * @see #isSingletonCurrentlyInCreation
	 */
	protected void afterSingletonCreation(String beanName) {
		if (!this.inCreationCheckExclusions.contains(beanName) && !this.singletonsCurrentlyInCreation.remove(beanName)) {
			throw new IllegalStateException("Singleton '" + beanName + "' isn't currently in creation");
		}
	}
```

（4）通过调用参数传入的ObjectFactory的个体Object方法实例化bean。

（5）加载单例后的处理方法调用。

（6）将结果记录至缓存并删除加载bean过程中所记录的各种辅助状态。

```java
	/**
	 * Add the given singleton object to the singleton cache of this factory.
	 * <p>To be called for eager registration of singletons.
	 * @param beanName the name of the bean
	 * @param singletonObject the singleton object
	 */
	protected void addSingleton(String beanName, Object singletonObject) {
		synchronized (this.singletonObjects) {
			this.singletonObjects.put(beanName, singletonObject);
			this.singletonFactories.remove(beanName);
			this.earlySingletonObjects.remove(beanName);
			this.registeredSingletons.add(beanName);
		}
	}
```

（7）返回处理结果

虽然我们已经从外部了解了加载bean的逻辑架构，但现在我们还并没有开始对bean加载功能的探索，之前提到过，bean的加载逻辑其实是在传入的ObjectFactory类型的参数singletonFactory中定义的，我们反推参数的获取，其实是去执行的doGetBean中的createBean：

```java
sharedInstance = getSingleton(beanName, () -> {
						try {
							return createBean(beanName, mbd, args);
						}
						catch (BeansException ex) {
							// Explicitly remove instance from singleton cache: It might have been put there
							// eagerly by the creation process, to allow for circular reference resolution.
							// Also remove any beans that received a temporary reference to the bean.
							destroySingleton(beanName);
							throw ex;
						}
					});
```

#### 准备创建bean

在AbstractautowireCapableBeanFactory#createBean()

```java
	@Override
	protected Object createBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args)
			throws BeanCreationException {

		if (logger.isDebugEnabled()) {
			logger.debug("Creating instance of bean '" + beanName + "'");
		}
		RootBeanDefinition mbdToUse = mbd;

		// Make sure bean class is actually resolved at this point, and
		// clone the bean definition in case of a dynamically resolved Class
		// which cannot be stored in the shared merged bean definition.
        // 锁定class，根据设置的class属性或者根据className来解析Class
		Class<?> resolvedClass = resolveBeanClass(mbd, beanName);
		if (resolvedClass != null && !mbd.hasBeanClass() && mbd.getBeanClassName() != null) {
			mbdToUse = new RootBeanDefinition(mbd);
			mbdToUse.setBeanClass(resolvedClass);
		}

		// Prepare method overrides. 验证及准备覆盖的方法 处理overrides属性
		try {
			mbdToUse.prepareMethodOverrides();
		}
		catch (BeanDefinitionValidationException ex) {
			throw new BeanDefinitionStoreException(mbdToUse.getResourceDescription(),
					beanName, "Validation of method overrides failed", ex);
		}

		try {
           	// 给BeanPostProcessors一个机会返回代理来替代真正的实例 Aop的功能基于这个判断
			// Give BeanPostProcessors a chance to return a proxy instead of the target bean instance.
			Object bean = resolveBeforeInstantiation(beanName, mbdToUse);
			if (bean != null) {
				return bean;
			}
		}
		catch (Throwable ex) {
			throw new BeanCreationException(mbdToUse.getResourceDescription(), beanName,
					"BeanPostProcessor before instantiation of bean failed", ex);
		}

		try {
			Object beanInstance = doCreateBean(beanName, mbdToUse, args);
			if (logger.isDebugEnabled()) {
				logger.debug("Finished creating instance of bean '" + beanName + "'");
			}
			return beanInstance;
		}
		catch (BeanCreationException ex) {
			// A previously detected exception with proper bean creation context already...
			throw ex;
		}
		catch (ImplicitlyAppearedSingletonException ex) {
			// An IllegalStateException to be communicated up to DefaultSingletonBeanRegistry...
			throw ex;
		}
		catch (Throwable ex) {
			throw new BeanCreationException(
					mbdToUse.getResourceDescription(), beanName, "Unexpected exception during bean creation", ex);
		}
	}
```

从代码中我们可以总结出函数完成的具体步骤及功能。

（1）根据设置的class属性或者根据className来解析Class。

（2）对override属性进行标记及验证。很多读者可能会不知道这个方法的作用，因为在Spring的配置里面根本就没有诸如override-method之类的配置，那么这个方法到底是干什么用的呢？其实在Spring中确实没有override-method这样的配置，但是如果读过前面的部分，可能会有所发现，在Spring配置中是存在lookup-method和replace-method的，而这两个配置的加载其实就是将配置统一存放在BeanDefinition中的methodOverrides属性里，而这个函数的操作其实也就是针对于这两个配置的。

（3）应用初始化前的后处理器，解析指定bean是否存在初始化前的短路操作。

（4）创建bean。我们首先查看下对override属性标记及验证的逻辑实现

##### 处理override属性

查看源码中AbstractBeanDefinition类的prepareMethodOverrides方法：

```java
/**
	 * 验证并准备为此bean定义的方法替代。 检查是否存在具有指定名称的方法
	 */
	public void prepareMethodOverrides() throws BeanDefinitionValidationException {
		// Check that lookup methods exists.
		MethodOverrides methodOverrides = getMethodOverrides();
		if (!methodOverrides.isEmpty()) {
			Set<MethodOverride> overrides = methodOverrides.getOverrides();
			synchronized (overrides) {
				for (MethodOverride mo : overrides) {
					prepareMethodOverride(mo);
				}
			}
		}
	}

	/**
	 * 验证并准备给定的方法重写。 检查是否存在具有指定名称的方法，如果找不到该方法，则将其标记为未重载
	 */
	protected void prepareMethodOverride(MethodOverride mo) throws BeanDefinitionValidationException {
        // 获取对应类中对应方法名的个数
		int count = ClassUtils.getMethodCountForName(getBeanClass(), mo.getMethodName());
		if (count == 0) {
			throw new BeanDefinitionValidationException(
					"Invalid method override: no method with name '" + mo.getMethodName() +
					"' on class [" + getBeanClassName() + "]");
		}
		else if (count == 1) {
			// Mark override as not overloaded, to avoid the overhead of arg type checking.
            // 标记MethodOverride暂未被覆盖，避免参数类型检查的开销
			mo.setOverloaded(false);
		}
	}
```

通过以上两个函数的代码你能体会到它所要实现的功能吗？之前反复提到过，在Spring配置中存在lookup-method和replace-method两个配置功能，而这两个配置的加载其实就是将配置统一存放在BeanDefinition中的methodOverrides属性里，这两个功能实现原理其实是在bean实例化的时候如果检测到存在methodOverrides属性，会动态地为当前bean生成代理并使用对应的拦截器为bean做增强处理，相关逻辑实现在bean的实例化部分详细介绍。但是，这里要提到的是，对于方法的匹配来讲，如果一个类中存在若干个重载方法，那么，在函数调用及增强的时候还需要根据参数类型进行匹配，来最终确认当前调用的到底是哪个函数。但是，Spring将一部分匹配工作在这里完成了，如果当前类中的方法只有一个，那么就设置重载该方法没有被重载，这样在后续调用的时候便可以直接使用找到的方法，而不需要进行方法的参数匹配验证了，而且还可以提前对方法存在性进行验证，正可谓一箭双雕

##### 实例化的前置处理

在真正调用doCreate方法创建bean的实例前使用了这样一个方法resolveBeforeInstantiation (beanName, mbd)对BeanDefinigiton中的属性做些前置处理。当然，无论其中是否有相应的逻辑实现我们都可以理解，因为真正逻辑实现前后留有处理函数也是可扩展的一种体现，但是，这并不是最重要的，在函数中还提供了一个短路判断，这才是最为关键的部分

```java
if(bean !=null){
	return bean;
}
```

当经过前置处理后返回的结果如果不为空，那么会直接略过后续的Bean的创建而直接返回结果。这一特性虽然很容易被忽略，但是却起着至关重要的作用，我们熟知的AOP功能就是基于这里的判断的。

```java
@Nullable
	protected Object resolveBeforeInstantiation(String beanName, RootBeanDefinition mbd) {
		Object bean = null;
        // 如果尚未被解析
		if (!Boolean.FALSE.equals(mbd.beforeInstantiationResolved)) {
			// Make sure bean class is actually resolved at this point.
            // 确保此时确实解决了bean类
			if (!mbd.isSynthetic() && hasInstantiationAwareBeanPostProcessors()) {
				Class<?> targetType = determineTargetType(beanName, mbd);
				if (targetType != null) {
                    // 实例化前的后处理器应用
					bean = applyBeanPostProcessorsBeforeInstantiation(targetType, beanName);
					if (bean != null) {
                        // 实例化后的后置处理器
						bean = applyBeanPostProcessorsAfterInitialization(bean, beanName);
					}
				}
			}
			mbd.beforeInstantiationResolved = (bean != null);
		}
		return bean;
	}
```

**实例化前的后处理器应用**

bean的实例化前调用，也就是将AbsractBeanDefinition转换为BeanWrapper前的处理。给子类一个修改BeanDefinition的机会，也就是说当程序经过这个方法后，bean可能已经不是我们认为的bean了，而是或许成为了一个经过处理的代理bean，可能是通过cglib生成的，也可能是通过其它技术生成的。 这个是Aop的技术相关所以这里不进入仔细探究

```java
@Nullable
	protected Object applyBeanPostProcessorsBeforeInstantiation(Class<?> beanClass, String beanName) {
		for (BeanPostProcessor bp : getBeanPostProcessors()) {
			if (bp instanceof InstantiationAwareBeanPostProcessor) {
				InstantiationAwareBeanPostProcessor ibp = (InstantiationAwareBeanPostProcessor) bp;
				Object result = ibp.postProcessBeforeInstantiation(beanClass, beanName);
				if (result != null) {
					return result;
				}
			}
		}
		return null;
	}
```



**实例化后的后置处理器**

在讲解从缓存中获取单例bean的时候就提到过，Spring中的规则是在bean的初始化后尽可能保证将注册的后处理器的postProcessAfterInitialization方法应用到该bean中，因为如果返回的bean不为空，那么便不会再次经历普通bean的创建过程，所以只能在这里应用后处理器的postProcessAfterInitialization方法

```java
@Override
	public Object applyBeanPostProcessorsAfterInitialization(Object existingBean, String beanName) throws BeansException {

		Object result = existingBean;
		for (BeanPostProcessor beanProcessor : getBeanPostProcessors()) {
			Object current = beanProcessor.postProcessAfterInitialization(result, beanName);
			if (current == null) {
				return result;
			}
			result = current;
		}
		return result;
	}
```

#### 循环依赖

下面就是一个循环依赖的例子：

[![gDP9PI.png](https://z3.ax1x.com/2021/05/13/gDP9PI.png)](https://imgtu.com/i/gDP9PI)



#### 创建bean

当经历过resolveBeforeInstantiation方法后，程序有两个选择，如果创建了代理或者说重写了InstantiationAwareBeanPostProcessor的postProcessBeforeInstantiation方法并在方法postProcess BeforeInstantiation中改变了bean，则直接返回就可以了，否则需要进行常规bean的创建。而这常规bean的创建就是在doCreateBean中完成的

```java
// AbstractAutowireCapableBeanFactory.java
protected Object doCreateBean(final String beanName, final RootBeanDefinition mbd, final @Nullable Object[] args)
			throws BeanCreationException {

		// Instantiate the bean.
		BeanWrapper instanceWrapper = null;
		if (mbd.isSingleton()) {
			instanceWrapper = this.factoryBeanInstanceCache.remove(beanName);
		}
		if (instanceWrapper == null) {
            // 根据指定bean使用对应的策略，创建新的实例，如：工厂方法，构造函数自动注入，简单初始化
			instanceWrapper = createBeanInstance(beanName, mbd, args);
		}
		final Object bean = instanceWrapper.getWrappedInstance();
		Class<?> beanType = instanceWrapper.getWrappedClass();
		if (beanType != NullBean.class) {
			mbd.resolvedTargetType = beanType;
		}

		// Allow post-processors to modify the merged bean definition.
		synchronized (mbd.postProcessingLock) {
			if (!mbd.postProcessed) {
				try {
					applyMergedBeanDefinitionPostProcessors(mbd, beanType, beanName);
				}
				catch (Throwable ex) {
					throw new BeanCreationException(mbd.getResourceDescription(), beanName,
							"Post-processing of merged bean definition failed", ex);
				}
				mbd.postProcessed = true;
			}
		}
		// 需要提早曝光，单例&允许循环依赖&当前bean正在创建中，检查循环依赖
		// Eagerly cache singletons to be able to resolve circular references
		// even when triggered by lifecycle interfaces like BeanFactoryAware.
		boolean earlySingletonExposure = (mbd.isSingleton() && this.allowCircularReferences &&
				isSingletonCurrentlyInCreation(beanName));
		if (earlySingletonExposure) {
			if (logger.isDebugEnabled()) {
				logger.debug("Eagerly caching bean '" + beanName +
						"' to allow for resolving potential circular references");
			}
            // 为避免后期循环依赖，可以在bean初始化完成前将创建实例的ObjectFactory加入工厂
			addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));
		}

		// Initialize the bean instance.
		Object exposedObject = bean;
		try {
            // 对bean进行填充，将各个属性值注入，其中可能存在依赖于其他bean的属性，则会递归初始化依赖bean
			populateBean(beanName, mbd, instanceWrapper);
            // 调用初始化方法没比如init-method
			exposedObject = initializeBean(beanName, exposedObject, mbd);
		}
		catch (Throwable ex) {
			if (ex instanceof BeanCreationException && beanName.equals(((BeanCreationException) ex).getBeanName())) {
				throw (BeanCreationException) ex;
			}
			else {
				throw new BeanCreationException(
						mbd.getResourceDescription(), beanName, "Initialization of bean failed", ex);
			}
		}

		if (earlySingletonExposure) {
			Object earlySingletonReference = getSingleton(beanName, false);
            // earlySingletonReference 只有在检查到用循环依赖的情况才会不为null
			if (earlySingletonReference != null) {
                // 如果exposedObject没有初始化方法中被改变，也就是没有被增强
				if (exposedObject == bean) {
					exposedObject = earlySingletonReference;
				}
				else if (!this.allowRawInjectionDespiteWrapping && hasDependentBean(beanName)) {
					String[] dependentBeans = getDependentBeans(beanName);
					Set<String> actualDependentBeans = new LinkedHashSet<>(dependentBeans.length);
					for (String dependentBean : dependentBeans) {
                        // 检查依赖
						if (!removeSingletonIfCreatedForTypeCheckOnly(dependentBean)) {
							actualDependentBeans.add(dependentBean);
						}
					}
					if (!actualDependentBeans.isEmpty()) {
						throw new BeanCurrentlyInCreationException(beanName,
								"Bean with name '" + beanName + "' has been injected into other beans [" +
								StringUtils.collectionToCommaDelimitedString(actualDependentBeans) +
								"] in its raw version as part of a circular reference, but has eventually been " +
								"wrapped. This means that said other beans do not use the final version of the " +
								"bean. This is often the result of over-eager type matching - consider using " +
								"'getBeanNamesOfType' with the 'allowEagerInit' flag turned off, for example.");
					}
				}
			}
		}

		// Register bean as disposable.
		try {
            // 根据scope注册bean
			registerDisposableBeanIfNecessary(beanName, bean, mbd);
		}
		catch (BeanDefinitionValidationException ex) {
			throw new BeanCreationException(
					mbd.getResourceDescription(), beanName, "Invalid destruction signature", ex);
		}

		return exposedObject;
	}
```



（1）如果是单例则需要首先清除缓存。

（2）实例化bean，将BeanDefinition转换为BeanWrapper。转换是一个复杂的过程，但是我们可以尝试概括大致的功能，如下所示。

● 如果存在工厂方法则使用工厂方法进行初始化。

● 一个类有多个构造函数，每个构造函数都有不同的参数，所以需要根据参数锁定构造函数并进行初始化。

● 如果既不存在工厂方法也不存在带有参数的构造函数，则使用默认的构造函数进行bean的实例化。

（3）MergedBeanDefinitionPostProcessor的应用。bean合并后的处理，Autowired注解正是通过此方法实现诸如类型的预解析。

（4）依赖处理。在Spring中会有循环依赖的情况，例如，当A中含有B的属性，而B中又含有A的属性时就会构成一个循环依赖，此时如果A和B都是单例，那么在Spring中的处理方式就是当创建B的时候，涉及自动注入A的步骤时，并不是直接去再次创建A，而是通过放入缓存中的ObjectFactory来创建实例，这样就解决了循环依赖的问题。

（5）属性填充。将所有属性填充至bean的实例中。

（6）循环依赖检查。之前有提到过，在Sping中解决循环依赖只对单例有效，而对于prototype的bean，Spring没有好的解决办法，唯一要做的就是抛出异常。在这个步骤里面会检测已经加载的bean是否已经出现了依赖循环，并判断是否需要抛出异常。

（7）注册DisposableBean。如果配置了destroy-method，这里需要注册以便于在销毁时候调用。

（8）完成创建并返回。可以看到上面的步骤非常的繁琐，每一步骤都使用了大量的代码来完成其功能，最复杂也是最难以理解的当属循环依赖的处理，在真正进入doCreateBean前我们有必要先了解下循环依赖

##### 创建bean的实例 createBeanInstance(beanName, mbd, args)

```java
//// AbstractAutowireCapableBeanFactory.java
protected BeanWrapper createBeanInstance(String beanName, RootBeanDefinition mbd, @Nullable Object[] args) {
		// Make sure bean class is actually resolved at this point.
		Class<?> beanClass = resolveBeanClass(mbd, beanName);

		if (beanClass != null && !Modifier.isPublic(beanClass.getModifiers()) && !mbd.isNonPublicAccessAllowed()) {
			throw new BeanCreationException(mbd.getResourceDescription(), beanName,
					"Bean class isn't public, and non-public access not allowed: " + beanClass.getName());
		}

		Supplier<?> instanceSupplier = mbd.getInstanceSupplier();
		if (instanceSupplier != null) {
			return obtainFromSupplier(instanceSupplier, beanName);
		}

		if (mbd.getFactoryMethodName() != null)  {
			return instantiateUsingFactoryMethod(beanName, mbd, args);
		}

		// Shortcut when re-creating the same bean...
		boolean resolved = false;
		boolean autowireNecessary = false;
		if (args == null) {
			synchronized (mbd.constructorArgumentLock) {
				if (mbd.resolvedConstructorOrFactoryMethod != null) {
					resolved = true;
					autowireNecessary = mbd.constructorArgumentsResolved;
				}
			}
		}
		if (resolved) {
			if (autowireNecessary) {
				return autowireConstructor(beanName, mbd, null, null);
			}
			else {
				return instantiateBean(beanName, mbd);
			}
		}

		// Need to determine the constructor...
		Constructor<?>[] ctors = determineConstructorsFromBeanPostProcessors(beanClass, beanName);
		if (ctors != null ||
				mbd.getResolvedAutowireMode() == RootBeanDefinition.AUTOWIRE_CONSTRUCTOR ||
				mbd.hasConstructorArgumentValues() || !ObjectUtils.isEmpty(args))  {
			return autowireConstructor(beanName, mbd, ctors, args);
		}

		// No special handling: simply use no-arg constructor.
		return instantiateBean(beanName, mbd);
	}
```

虽然代码中实例化的细节非常复杂，但是在createBeanIntance方法中我们还是可以清晰地看到实例化的逻辑的。（1）如果在RootBeanDefinition中存在factoryMethodName属性，或者说在配置文件中配置了factory-method，那么Spring会尝试使用instantiateUsingFactoryMethod(beanName, mbd, args)方法根据RootBeanDefinition中的配置生成bean的实例。

（2）解析构造函数并进行构造函数的实例化。因为一个bean对应的类中可能会有多个构造函数，而每个构造函数的参数不同，Spring在根据参数及类型去判断最终会使用哪个构造函数进行实例化。但是，判断的过程是个比较消耗性能的步骤，所以采用缓存机制，如果已经解析过则不需要重复解析而是直接从RootBeanDefinition中的属性resolvedConstructorOrFactoryMethod缓存的值去取，否则需要再次解析，并将解析的结果添加至RootBeanDefinition中的属性resolvedConstructorOrFactoryMethod中。

**autowireConstructor**

对于实例的创建Spring中分成了两种情况，一种是通用的实例化，另一种是带有参数的实例化。带有参数的实例化过程相当复杂，因为存在着不确定性，所以在判断对应参数上做了大量工作 

```java
// “自动装配构造函数”（按类型带有构造函数参数）的行为。 如果指定了显式构造函数参数值，则将所有剩余参数与Bean工厂中的Bean进行匹配时也适用。
// 这对应于构造函数注入：在这种模式下，Spring Bean工厂能够托管需要基于构造函数的依赖关系解析的组件。
protected BeanWrapper autowireConstructor(
			String beanName, RootBeanDefinition mbd, @Nullable Constructor<?>[] ctors, @Nullable Object[] explicitArgs) {

		return new ConstructorResolver(this).autowireConstructor(beanName, mbd, ctors, explicitArgs);
	}
```

ConstructorResolver#autowireConstructor()

```java
// “自动装配构造函数”（按类型带有构造函数参数）的行为。 如果指定了显式构造函数参数值，则将所有剩余参数与Bean工厂中的Bean进行匹配时也适用。
// 这对应于构造函数注入：在这种模式下，Spring Bean工厂能够托管需要基于构造函数的依赖关系解析的组件。
public BeanWrapper autowireConstructor(final String beanName, final RootBeanDefinition mbd,
			@Nullable Constructor<?>[] chosenCtors, @Nullable final Object[] explicitArgs) {
		
		BeanWrapperImpl bw = new BeanWrapperImpl();
    // 使用在此工厂注册的自定义编辑器初始化给定的BeanWrapper。 被BeanWrappers调用，它将创建并填充Bean实例.默认实现将委托委托registerCustomEditors 。 可以在子类中覆盖
		this.beanFactory.initBeanWrapper(bw);

		Constructor<?> constructorToUse = null;
		ArgumentsHolder argsHolderToUse = null;
		Object[] argsToUse = null;
		// explicitArgs通过getBean方法传入，如果getBean方法调用指定方法参数那么直接使用
		if (explicitArgs != null) {
			argsToUse = explicitArgs;
		}
		else {
            // 如果getbean方法没有指定则尝试从配置文件中解析
			Object[] argsToResolve = null;
            // 尝试从缓存中获取
			synchronized (mbd.constructorArgumentLock) {
				constructorToUse = (Constructor<?>) mbd.resolvedConstructorOrFactoryMethod;
				if (constructorToUse != null && mbd.constructorArgumentsResolved) {
					// Found a cached constructor...
                    // 从缓存中获取
					argsToUse = mbd.resolvedConstructorArguments;
					if (argsToUse == null) {
                        // 配置的构造函数参数
						argsToResolve = mbd.preparedConstructorArguments;
					}
				}
			}
            // 如果缓存中存在
			if (argsToResolve != null) {
                // 解析参数类型，如给定方法的够着函数A(int,int)则通过这个方法把配置中的("1","1") 转为(1,1) 缓存中的值可能是原始值也可能是最终值
				argsToUse = resolvePreparedArguments(beanName, mbd, bw, constructorToUse, argsToResolve);
			}
		}
		// 没有被缓存
		if (constructorToUse == null) {
			// Need to resolve the constructor.
			boolean autowiring = (chosenCtors != null ||
					mbd.getResolvedAutowireMode() == RootBeanDefinition.AUTOWIRE_CONSTRUCTOR);
			ConstructorArgumentValues resolvedValues = null;

			int minNrOfArgs;
			if (explicitArgs != null) {
				minNrOfArgs = explicitArgs.length;
			}
			else {
                // 提取配置文件中的配置的构造函数参数
				ConstructorArgumentValues cargs = mbd.getConstructorArgumentValues();
                // 用于承载解析后的函数参数的值
				resolvedValues = new ConstructorArgumentValues();
                // 能解析到的参数个数
				minNrOfArgs = resolveConstructorArguments(beanName, mbd, bw, cargs, resolvedValues);
			}

			// Take specified constructors, if any.
			Constructor<?>[] candidates = chosenCtors;
			if (candidates == null) {
				Class<?> beanClass = mbd.getBeanClass();
				try {
					candidates = (mbd.isNonPublicAccessAllowed() ?
							beanClass.getDeclaredConstructors() : beanClass.getConstructors());
				}
				catch (Throwable ex) {
					throw new BeanCreationException(mbd.getResourceDescription(), beanName,
							"Resolution of declared constructors on bean Class [" + beanClass.getName() +
							"] from ClassLoader [" + beanClass.getClassLoader() + "] failed", ex);
				}
			}
            // 排序给定的构造函数，public构造函数优先，非public构造函数参数数量降序
			AutowireUtils.sortConstructors(candidates);
			int minTypeDiffWeight = Integer.MAX_VALUE;
			Set<Constructor<?>> ambiguousConstructors = null;
			LinkedList<UnsatisfiedDependencyException> causes = null;

			for (Constructor<?> candidate : candidates) {
				Class<?>[] paramTypes = candidate.getParameterTypes();

				if (constructorToUse != null && argsToUse.length > paramTypes.length) {
					// Already found greedy constructor that can be satisfied ->
					// do not look any further, there are only less greedy constructors left.
                    // 如果已经找到选用的构造函数，或者需要的参数个数小于当前的构造函数参数个数则终止，因为已经按照参数个数降序排列
					break;
				}
				if (paramTypes.length < minNrOfArgs) {
					continue;
				}

				ArgumentsHolder argsHolder;
				if (resolvedValues != null) {
                    // 有参数则根据数据值构造对应参数类型的参数
					try {
                        // 注释上获取参数名称
						String[] paramNames = ConstructorPropertiesChecker.evaluate(candidate, paramTypes.length);
						if (paramNames == null) {
                            // 获取参数名称探索器
							ParameterNameDiscoverer pnd = this.beanFactory.getParameterNameDiscoverer();
							if (pnd != null) {
                                // 获取指定构造函数的参数名称
								paramNames = pnd.getParameterNames(candidate);
							}
						}
                        // 根据名称和数据类型创建参数持有者
						argsHolder = createArgumentArray(beanName, mbd, resolvedValues, bw, paramTypes, paramNames,
								getUserDeclaredConstructor(candidate), autowiring);
					}
					catch (UnsatisfiedDependencyException ex) {
						if (this.beanFactory.logger.isTraceEnabled()) {
							this.beanFactory.logger.trace(
									"Ignoring constructor [" + candidate + "] of bean '" + beanName + "': " + ex);
						}
						// Swallow and try next constructor.
						if (causes == null) {
							causes = new LinkedList<>();
						}
						causes.add(ex);
						continue;
					}
				}
				else {
					// Explicit arguments given -> arguments length must match exactly.
					if (paramTypes.length != explicitArgs.length) {
						continue;
					}
                    // 构造函数没有参数的情况
					argsHolder = new ArgumentsHolder(explicitArgs);
				}
// 探测是否有不确定性的构造函数存在，例如不同的构造函数为父子关系。
				int typeDiffWeight = (mbd.isLenientConstructorResolution() ?
						argsHolder.getTypeDifferenceWeight(paramTypes) : argsHolder.getAssignabilityWeight(paramTypes));
				// Choose this constructor if it represents the closest match.
                // 如果它代表着当前最接近的匹配则选择作为构造函数
				if (typeDiffWeight < minTypeDiffWeight) {
					constructorToUse = candidate;
					argsHolderToUse = argsHolder;
					argsToUse = argsHolder.arguments;
					minTypeDiffWeight = typeDiffWeight;
					ambiguousConstructors = null;
				}
				else if (constructorToUse != null && typeDiffWeight == minTypeDiffWeight) {
					if (ambiguousConstructors == null) {
						ambiguousConstructors = new LinkedHashSet<>();
						ambiguousConstructors.add(constructorToUse);
					}
					ambiguousConstructors.add(candidate);
				}
			}

			if (constructorToUse == null) {
				if (causes != null) {
					UnsatisfiedDependencyException ex = causes.removeLast();
					for (Exception cause : causes) {
						this.beanFactory.onSuppressedException(cause);
					}
					throw ex;
				}
				throw new BeanCreationException(mbd.getResourceDescription(), beanName,
						"Could not resolve matching constructor " +
						"(hint: specify index/type/name arguments for simple parameters to avoid type ambiguities)");
			}
			else if (ambiguousConstructors != null && !mbd.isLenientConstructorResolution()) {
				throw new BeanCreationException(mbd.getResourceDescription(), beanName,
						"Ambiguous constructor matches found in bean '" + beanName + "' " +
						"(hint: specify index/type/name arguments for simple parameters to avoid type ambiguities): " +
						ambiguousConstructors);
			}

			if (explicitArgs == null) {
                // 将解析的构造函数加入缓存
				argsHolderToUse.storeCache(mbd, constructorToUse);
			}
		}

		try {
			final InstantiationStrategy strategy = beanFactory.getInstantiationStrategy();
			Object beanInstance;

			if (System.getSecurityManager() != null) {
				final Constructor<?> ctorToUse = constructorToUse;
				final Object[] argumentsToUse = argsToUse;
				beanInstance = AccessController.doPrivileged((PrivilegedAction<Object>) () ->
						strategy.instantiate(mbd, beanName, beanFactory, ctorToUse, argumentsToUse),
						beanFactory.getAccessControlContext());
			}
			else {
				beanInstance = strategy.instantiate(mbd, beanName, this.beanFactory, constructorToUse, argsToUse);
			}

			bw.setBeanInstance(beanInstance);
			return bw;
		}
		catch (Throwable ex) {
			throw new BeanCreationException(mbd.getResourceDescription(), beanName,
					"Bean instantiation via constructor failed", ex);
		}
	}
```

包含：属性注入，

### 自动装配Bean

**自动装配：**

​			`<bean>元素来声明 bean 和通过使用 XML 配置文件中的<constructor-arg>和<property>元素来注入 。`

​			`Spring 容器可以在不使用<constructor-arg>和<property> 元素的情况下自动装配相互协作的 bean 之间的关系，这有助于减少编写一个大的基于 Spring 的应用程序的 XML 配置的数量`

​			使用自动装配无法从配置文件中读懂JavaBean需要哪些属性

​			当自动装配始终在同一个项目中使用时，它的效果最好。如果通常不使用自动装配，它可能会使开发人员混淆的使用它来连接只有一个或两个 bean 定义。

​			不过，自动装配可以显著减少需要指定的属性或构造器参数，但你应该在使用它们之前考虑到自动装配的局限性和缺点。

​			你可以使用<bean>元素的 autowire 属性为一个 bean 定义指定自动装配模式;

**`<bean id="customer" class="com.yiibai.common.Customer" autowire="byName" />`**

**autowire属性有以下取值：**

- `no`	这是默认的设置，它意味着没有自动装配，你应该使用显式的bean引用来连线。你不用为了连线做特殊的事。在依赖注入章节你已经看到这个了。
- `byName`	由属性名自动装配。Spring 容器看到在 XML 配置文件中 bean 的自动装配的属性设置为 byName。然后尝试匹配，并且将它的属性与在配置文件中被定义为相同名称的 beans 的属性进行连接。
- `byType`	由属性数据类型自动装配。Spring 容器看到在 XML 配置文件中 bean 的自动装配的属性设置为 byType。然后如果它的类型匹配配置文件中的一个确切的 bean 名称，它将尝试匹配和连接属性的类型。如果存在不止一个这样的 bean，则一个致命的异常将会被抛出。
- `constructor`	类似于 `byType`，但该类型适用于构造函数参数类型。如果在容器中没有一个构造函数参数类型的 bean，则一个致命错误将会发生。
- `autodetect`	Spring首先尝试通过 constructor 使用自动装配来连接，如果它不执行，Spring 尝试通过 byType 来自动装配。

> ​	<bean>元素byname装配：
> ​					<bean id="textEditor" class="com.tutorialspoint.TextEditor" 
> ​					autowire="byName">
> ​				它尝试将它的属性与配置文件中定义为相同名称的 beans 进行匹配和连接。如果找到匹配项，它将注入这些 beans，否则，它将抛出异常
> ​	<bean>元素bytype装配：
> ​					<bean id="textEditor" class="com.tutorialspoint.TextEditor" autowire="byType">
> ​					如果它的 type 恰好与配置文件中 beans 名称中的一个相匹配，它将尝试匹配和连接它的属性。如果找到匹配项，它将注入这些 beans，否则，它将抛出异常

#### 使用注解进行装配

##### @Autowired注解

​	@Autowired注解是通过匹配数据类型自动装配Bean。默认byType

##### @Qualifier注解

​	@Qualifier注解我们用来控制bean应在字段上自动装配,使用 @Quanlifier 告诉Spring哪些bean应当自动装配。

```java
public class Customer {

	@Autowired
	@Qualifier("personA")
	private Person person;
	//...
}
```



### 注解代替xml

<!-- 指定扫描哪些注解，扫描包时会扫描指定包下的所有的子包 -->

```java
<context:component-scan base-package="com.leo.demo"></context:component-scan>
使用时：@Component("user");
    @Component("user")
    @Service("user")//service层
    @Controller("user")//web层
    @Repository("user")//dao层
    @Scope(scopeName="singleton|protptype")//指定对象的作用域
注入值：
使用反射的Field赋值               不建议使用破坏了封装性
    @Value("lll")
    private String name;
    @Value("12")
    private Integer age;
    另一种是在：
    通过set()方法赋值 推荐使用 
    @Value("lll")
    public void setName(String name) {
    this.name = name;
    }	
    @Resouce(name="car1")//手动设置注入哪一个对象类型
    private Car car;
需要在xml中配置这个car对象的不同的实例化
@PostConStruct 用于创建对象调用===》init-method的属性形式
@PreDestory用于销毁对象时调用-=》destory-method=“方法名”的属性形式
测试的方式：
这样就不需要在测试时每次都创建容器
在类名中用注解：
    @Runnwith(SpringJunit4ClassRunner.class)//帮我们创建容器
    @ContextConfiguration("xxx.xml")//指定读取的配置文件
    @Test
    public void fun(){
    }
```

### AOP

​		横向重复，纵向抽取
​		Aop基于代理的机制
​		Spring产生代理对象，

#### 实现的AOP的原理：

##### 动态代理：
​			被代理对象必须实现接口，如果没有接口将不能使用对某一个目标中的方法进行增强

#### cglib代理：

​			可以对任何类生成代理，他可以目标对象进行继承代理。若目标对象被final修饰则该类不可以生成代理

​				`Spring两者混合使用。`

#### SpringAop开发：

​		Spring封装了动态代理代码，不需受用书写  可以对任何类进行d代理的增强

##### Aop术语：

​	`切面(aspect)：`对象操作过程中的截面，一段程序代码被植入到程序的流程中，(切入点+通知)

​	`连接点(JoinPoint)：`对象的操作过程中的某个阶段点，目标对象中所有可以增强的方法

​    `切入点(Pointcut)：`是连接点的集合，目标对象中已经增强的方法

​    `通知(Advice)：`某个切入点被横切后所取得处理逻辑，增强的代码

​    `目标对象(Target)：`所有被通知的对象

​	`织入(Weaving)：`将切面功能应用到目标对象的过程。织入时期:(编译时期，类加载时期，执行期，

   `引入：`已编译的类在运行期动态加载属性和方法。

##### Spring切入点：

​	他表示注入切面的位置有以下三种切入点：`静态切入点，动态切入点，其他切入点`

##### 静态切入点：

​	 静态往往意味着不变，只能应用在相对不变的位置上静态切入点在某个方法名上是织入切面，在织入代码前，进象进行方法的匹配，判断当前的正在调用的方法是不是已经定义了静态切入点.若定义过说明匹配成功，织入切面，如没有定义为静态的切入点这匹配失败，不进行织入切面。

​		Pointcut接口是切入点的定义接口，用它来规定可切入的链接点的属性，通过对该接口的来扩展处理其他类型的链接点
​					

```java
public interface Pointcut{
                                                  						ClassFilter getClassFilter();                                				MethodMatcher getMethodMatcher();
}
//使用ClassFilter接口匹配目标类
public interface ClassFilter{
   //与目标类相匹配  
    boolean matches(Class class);
}
```



##### 动态切入点：

​			可以应用在相对变化的位置上，

##### Aspect：

​		就是Spring的切面，他是对象操作过程的截面，是对系统中的对象操作过程中的截面的逻辑进行模块化的封装的Aop概念实体
​		

##### Aop事务：

​		Spring 事务应用的方法上的策略的描述，传播行为，隔离级别，只读，超时属性，

###### 编程式事务管理：

​		在Spring中主要使用PlatformTransactionManager接口的事务管理器或者是TransactionTemplate,后者符合模板形式

######  声明式事务管理：

​			在声明的事务中不涉及组建依赖关系，通过AOP来实现事务管理，无需编写任何代码就可以实现基于容器的事务管理，推荐使用

​			常用TransactionProxyFactoryBean完成声明式事务管理，设置代理的目标对象，代理对象生成的方法和事务的生成方式和事务属性，代理对象是在目标对象上生成的包含事务和AOP切面的新的对象，可以付给目标的引用代替目标对象，


​				


```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.springframework.org/schema/beans" xmlns:context="http://www.springframework.org/schema/context" xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd ">	
    <!-- 指定spring读取db.properties配置 -->
    <context:property-placeholder location="classpath:db.properties"  />
    <!-- 事务核心管理器,封装了所有事务操作. 依赖于连接池 -->
    <bean name="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager" >
        <property name="dataSource" ref="dataSource" ></property>
    </bean>
    <!-- 事务模板对象 -->
    <bean name="transactionTemplate" class="org.springframework.transaction.support.TransactionTemplate" >
        <property name="transactionManager" ref="transactionManager" ></property>
    </bean>

    <!-- 配置事务通知 -->
    <tx:advice id="txAdvice" transaction-manager="transactionManager" >
        <tx:attributes>
            <!-- 以方法为单位,指定方法应用什么事务属性
     isolation:隔离级别
     propagation:传播行为
     read-only:是否只读
     -->
            <tx:method name="save*" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="false" />
            <tx:method name="persist*" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="false" />
            <tx:method name="update*" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="false" />
            <tx:method name="modify*" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="false" />
            <tx:method name="delete*" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="false" />
            <tx:method name="remove*" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="false" />
            <tx:method name="get*" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="true" />
            <tx:method name="find*" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="true" />
            <tx:method name="transfer" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="false" />
        </tx:attributes>
    </tx:advice>
    <!-- 配置织入 -->
    <aop:config  >
        <!-- 配置切点表达式      *号代表的任意的参数， -->
        <aop:pointcut expression="execution(* service.*ServiceImpl.*(..))" id="txPc"/>
        <!-- 配置切面 : 通知+切点
      advice-ref:通知的名称
      pointcut-ref:切点的名称
     -->
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPc" />
    </aop:config>
    <!-- 1.将连接池 -->
    <bean name="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource" >
        <property name="jdbcUrl" value="${jdbc.jdbcUrl}" ></property>
        <property name="driverClass" value="${jdbc.driverClass}" ></property>
        <property name="user" value="${jdbc.user}" ></property>
        <property name="password" value="${jdbc.password}" ></property>
    </bean>
    <!-- 2.Dao-->
    <bean name="accountDao" class="dao.AccountDaoImpl" >
        <property name="dataSource" ref="dataSource" ></property>
    </bean>
    <!-- 3.Service-->
    <bean name="accountService" class="service.AccountServiceImpl" >
        <property name="ad" ref="accountDao" ></property>
        <property name="tt" ref="transactionTemplate" ></property>
    </bean>  

</beans>
```
### Spring最核心的两个类：

#### DefaultListableBeanFactory

XmlBeanFactory继承自DefaultListableBeanFactory，而DefaultListableBeanFactory是整个bean加载的核心部分，是Spring注册及加载bean的默认实现，而对于XmlBeanFactory与DefaultListableBeanFactory不同的地方其实是在XmlBeanFactory中使用了自定义的XML读取器XmlBeanDefinitionReader，实现了个性化的BeanDefinitionReader读取，DefaultListableBeanFactory继承了AbstractAutowireCapableBeanFactory并实现了ConfigurableListableBeanFactory以及BeanDefinitionRegistry接口。以下ConfigurableListableBeanFactory的层次结构图：

![https://note.youdao.com/web/#/file/12fc37e835ef1f10c1a898069ce83c85/note/4d66dbf676d025f8148f4085f4b11519/](https://static.dingtalk.com/media/lALPDgfLOmTZhSvNAsDNBqo_1706_704.png_720x720q90g.jpg?bizType=im)

● AliasRegistry：定义对alias的简单增删改等操作。

● SimpleAliasRegistry：主要使用map作为alias的缓存，并对接口AliasRegistry进行实现。

● SingletonBeanRegistry：定义对单例的注册及获取。

● BeanFactory：定义获取bean及bean的各种属性。

● DefaultSingletonBeanRegistry：对接口SingletonBeanRegistry各函数的实现。

● HierarchicalBeanFactory：继承BeanFactory，也就是在BeanFactory定义的功能的基础上增加了对parentFactory的支持。

● BeanDefinitionRegistry：定义对BeanDefinition的各种增删改操作。

● FactoryBeanRegistrySupport：在DefaultSingletonBeanRegistry基础上增加了对FactoryBean的特殊处理功能。

● ConfigurableBeanFactory：提供配置Factory的各种方法。

● ListableBeanFactory：根据各种条件获取bean的配置清单。

● AbstractBeanFactory：综合FactoryBeanRegistrySupport和ConfigurableBeanFactory的功能。

● AutowireCapableBeanFactory：提供创建bean、自动注入、初始化以及应用bean的后处理器。

● AbstractAutowireCapableBeanFactory：综合AbstractBeanFactory并对接口Autowire Capable BeanFactory进行实现。

● ConfigurableListableBeanFactory：BeanFactory配置清单，指定忽略类型及接口等。

● DefaultListableBeanFactory：综合上面所有功能，主要是对Bean注册后的处理。

XmlBeanFactory对DefaultListableBeanFactory类进行了扩展，主要用于从XML文档中读取BeanDefinition，对于注册及获取Bean都是使用从父类DefaultListableBeanFactory继承的方法去实现，而唯独与父类不同的个性化实现就是增加了XmlBeanDefinitionReader类型的reader属性。在XmlBeanFactory中主要使用reader属性对资源文件进行读取和注册。

```java

/** @deprecated */
@Deprecated
public class XmlBeanFactory extends DefaultListableBeanFactory {
    private final XmlBeanDefinitionReader reader;

    public XmlBeanFactory(Resource resource) throws BeansException {
        this(resource, (BeanFactory)null);
    }

    public XmlBeanFactory(Resource resource, BeanFactory parentBeanFactory) throws BeansException {
        super(parentBeanFactory);
        this.reader = new XmlBeanDefinitionReader(this);
        this.reader.loadBeanDefinitions(resource);
    }
}
```

#### XmlBeanDefinitionReader

配置文件的读取是Spring中重要的功能，因为Spring的大部分功能都是以配置作为切入点的，那么我们可以从XmlBeanDefinitionReader中梳理一下资源文件读取、解析及注册的大致脉络，首先我们看看各个类的功能。

● ResourceLoader：定义资源加载器，主要应用于根据给定的资源文件地址返回对应的Resource。

● BeanDefinitionReader：主要定义资源文件读取并转换为BeanDefinition的各个功能

● EnvironmentCapable：定义获取Environment方法。

● DocumentLoader：定义从资源文件加载到转换为Document的功能。

● AbstractBeanDefinitionReader：对EnvironmentCapable、BeanDefinitionReader类定义的功能进行实现。

● BeanDefinitionDocumentReader：定义读取Docuemnt并注册BeanDefinition功能。

● BeanDefinitionParserDelegate：定义解析Element的各种方法。经过以上分析，我们可以梳理出整个XML配置文件读取的大致流程如下所示：

![](https://static.dingtalk.com/media/lALPDg7mN66My63NARTNA8s_971_276.png_720x720q90g.jpg?bizType=im)

在XmlBeanDifinitionReader中主要包含以下几步的处理。

（1）通过继承自AbstractBeanDefinitionReader中的方法，来使用ResourLoader将资源文件路径转换为对应的Resource文件。

（2）通过DocumentLoader对Resource文件进行转换，将Resource文件转换为Document文件。

（3）通过实现接口BeanDefinitionDocumentReader的DefaultBeanDefinitionDocumentReader类对Document进行解析，并使用BeanDefinitionParserDelegate对Element进行解析。

### Spring启动流程



### Spring 整合JDBC

JdbcTemplate操作数据库：
			在这个类中的内部已经处理完了数据库资源的建立和释放并且可以避免一些常见的错误，可以直接实例化，也可以通过依赖注入的方式在ApplicationContext中参生作为Javabean的引用。运行核心的jdbc的工作流程提供很多重载方法提高程序的灵活性。
		

```java
//public class UserDaoImpl extends JdbcDaoSupport implements UserDao {
//super.getJdbcTemplate().
//可以直接将DataSource直接在这个类中注入时直接作为参数把数据源给注入就可以了
public class UserDaoImpl implements UserDao {
    private JdbcTemplate Jt;
    public JdbcTemplate getJt() {
        return Jt;
    }

    public void setJt(JdbcTemplate jt) {
        Jt = jt;
    }

    @Override
    public void addUser(User user) {
        // TODO Auto-generated method stub
        String sql="insert into user values(null,'kkk','123','kkka222.COM')";
        Jt.update(sql);
    }

    @Override
    public void deleteUser(int id) {
        // TODO Auto-generated method stub
        String sql="delete from user where id=?";
        Jt.update(sql,id);
        System.out.println("删除成功");
    }

    @Override
    public void updateUser(User user) {
        // TODO Auto-generated method stub
        String sql="update user set name='?',password='?',email='?' where id=?";
        Jt.update(sql, user.getName(),user.getPassword(),user.getEmail(),user.getId());
    }

    @Override
    public int getTotalCount() {
        // TODO Auto-generated method stub
        String sql="select count(*) from user";
        return Jt.queryForObject(sql, Integer.class);

    }

    @Override
    public List<User> getAllUser() {
        // TODO Auto-generated method stub
        String sql="select * from user ";

        List<User> list=Jt.query(sql, new RowMapper<User>(){

            @Override
            public User mapRow(ResultSet rs, int arg1) throws SQLException {
                // TODO Auto-generated method stub
                User user=new User();
                user.setId(rs.getInt("id"));
                user.setName(rs.getString("name"));
                user.setPassword(rs.getString("password"));
                user.setEmail(rs.getString("email"));
                return user;
            }

        });
        return list;
    }

    @Override
    public User getUserbyId(Integer id) {
        // TODO Auto-generated method stub
        String sql="select * from user where id=?";
        return Jt.queryForObject(sql, new RowMapper<User>(){

            @Override
            public User mapRow(ResultSet rs, int arg1) throws SQLException {
                // TODO Auto-generated method stub
                User user=new User();
                user.setId(rs.getInt("id"));
                user.setName(rs.getString("name"));
                user.setPassword(rs.getString("password"));
                user.setEmail(rs.getString("email"));
                return user;
            }

        },id);
    }
    
配置：
    
<!--Spring 读取指定的db.property配置  -->
<context:property-placeholder location="classpath:db.properties"></context:property-placeholder>
<!-- 将连接池放到spring 容器 -->
<bean name ="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
<!-- 直接在这设置：<property name="jdbcUrl" value="jdbc:mysql:///hibernate"></property>
	<property name="driverClass" value="com.mysql.jdbc.Driver"></property>
	<property name="user" value="root"></property>
	<property name="password" value="123"></property> -->
	<!-- 这是通过Spring进行读取配置文件然后进行读取各个属性 -->
	<property name="jdbcUrl" value="${jdbc.jdbcUrl}"></property>
	<property name="driverClass" value="${jdbc.driverClass}"></property>
	<property name="user" value="${jdbc.user}"></property>
	<property name="password" value="${jdbc.password}"></property>
</bean>
<!-- 将JdbcTemplate放入Spring容器 -->
<bean name="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
	<property name="dataSource" ref="dataSource"></property>
</bean>
<!-- 将UserDao放入到Spring容器中 -->
<bean name="userDao" class="com.leo.jdbc.UserDaoImpl">
	<property name="Jt" ref="jdbcTemplate"></property>
</bean>

</beans>
```
### Spring整合其他两大框架

#### web层单独整合

Spring整合其他两大框架原理：

​		web层:用struts2+jsp然后Action对象交给Spring管理

​		service层：JavaBean直接交给Spring 管理

​		dao :hibernate中的sessionfactory和Session获得，aop事务都交给Spring管理都由Spring容器来创建和维护

> 导包：
>
> struts2:基本包+    struts2-spring-plugin-2.5.16是struts把Action对象交给Spring的插件如果没有Spring容器则会报错
>
> Spring：基础包：core|bean.context,expression,logging,log4j.   web:-web,    aop:aop,aspect,aopweaving,aop联盟，事务：jdbc,tx,c3p0,orm,
>
> ​		测试：-test,
> hibernate：操作数据库的规范-entitymanager;
>
> 导入约束：
> web应用单独配置Spring容器：
>
> 在web 的xml配置如下：
>
> ```xml
> <!--将 web 引入Spring容器中 -->
> <context-param>
>     <param-name>contextConfigLocation</param-name>
>     <param-value>classpath*:/applicationContext3.xml</param-value>
> </context-param>
> 
> web应用单独整合struts2:
> 在web 的xml配置如下：
> <!-- 配置struts -->
> <filter>
>     <filter-name>struts2</filter-name>
>     <filter-class>
>     org.apache.struts2.dispatcher.filter.StrutsPrepareAndExecuteFilter
>     </filter-class>
> </filter>
> <filter-mapping>
>     <filter-name>struts2</filter-name>
>     <url-pattern>/*</url-pattern>
> </filter-mapping>
> ```
>
> web单独整合hibernate :
>
> ​			1.配置实体映射文件：

```java
<?xml version='1.0' encoding='UTF-8'?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
<!-- 映射文件 -->
<hibernate-mapping>
<!-- <hibernate-mapping> 为根元素的 XML 文件，里面包含所有<class>标签。
    <class> 标签是用来定义从一个 Java 类到数据库表的特定映射。
        Java 的类名使用 name 属性来表示，数据库表明用 table 属性来表示。 -->
<class name="com.leo.domain.User" table="user">
    <!-- <meta> 标签是一个可选元素，可以被用来修饰类。 -->
    <meta attribute="class-description">
        This class contains the employee detail.
    </meta>
    <!--主键标签   -->
    <id name="id" type="int" column="id">
        <!--generator用来自动生成主键 ,class有以下属性native，使用算法创建主键 -->
        <generator class="assigned"></generator>
    </id>
    <!-- property用来使属性与数据库表的列匹配 标签中 name 属性引用的是类的性质，column 属性引用的是数据库表的列。
    type 属性保存 Hibernate 映射的类型，这个类型会将从 Java 转换成 SQL 数据类型。-->
    <property name="name" column="name" type="string"/>
    <property name="password" column="password" type="string"/>
    <property name="email" column="email" type="string"/>
</class>
</hibernate-mapping>
    
2.配置hibernate 配置文件：
    
<hibernate-configuration>
<session-factory>
    <!--数据库的驱动，URL，用户名，密码，hibernate方言，打印sql,映射文件  -->
    <property name="connection.driver_class">com.mysql.jdbc.Driver</property>                 
    <property name="connection.url">jdbc:mysql://localhost:3306/hibernate</property>
    <property name="connection.username">root</property>
    <property name="connection.password">123</property>
    <property name="dialect">org.hibernate.dialect.MySQL5InnoDBDialect</property>
    <property name="show_sql">true</property>
    <mapping resource="com/leo/domain/user.hbm.xml"/>
</session-factory>
</hibernate-configuration>
```

#### 完全整合Struts2,hibernate	

> 	Spring与struts2整合：
> 	导包：struts2-spring-plugin.jar是struts中的Action交于Spring容器
> 	在struts.xml配置：
> 	配置常量：struts.objectFactory=spring   :把action创建交给Spring容器
> 	struts.objectFactory.spring.autowise=name   ，Spring负者依赖注入属性


```xml
   
整合方案一：用原来的class属性来使用
    由struts创建对象，Spring用来组装和管理	
<package name="" namespace="/" extends="struts-default">
    <!-- 整合方案一：用原来的class属性来使用
    由struts创建对象，Spring用来组装和管理	
    -->
    <action name="userAction_" class="com/leo/struts2/UserAction.java" method="{1}">
    	<result name="suceesss">/index.jsp</result>
    </action>
</package>
    自动装配时其实就是属性的注入：必须提供setget方法，然后属性名与<bean>下的name一致，这样就可以交给Spring容器来创建管理对象
        
整合方式二（推荐使用）：
    在applicationContext.xml配置如下：
    class属性填写Spring中action对象的beanName，就是spring管理的xml中配置的bean的名字。完全有Spring来创建管理action的周期
    注意；Spring不能自动组装，只能手动注入依赖属性
<beans>
<!-- action对象的作用范围一定为多例 这样才符合struts2架构 -->
<!-- 这是有Spring来创建和管理 注意；Spring不能自动组装，只能手动注入依赖属性 -->
<bean name="userAction" class="com.leo.struts2.UserAction" scope="prototype">
	<property name="userservice" ref="userService"></property>
</bean>
<bean name="userservice" class="com.leo.service.impl.UserServiceImpl"></bean>
</beans>
在struts.xml配置如下：
<struts>
    <!-- 配置常量意思是否把action对象交给Spring容器来管理和创建 -->
    <constant name="struts.objectFactory" value="spring"></constant>
    <!-- 用来配置Action的的依赖注入属性 -->
    <constant name="struts.objectFactory.spring.autoWire" value="name"></constant>

    <package name="" namespace="/" extends="struts-default">	
    <!--方案二：	class属性填写Spring中action对象的beanName，就是spring管理的xml中配置的bean的名字。完全有Spring来创建管理action的周期
    注意；Spring不能自动组装，只能手动注入依赖属性 -->
        <action name="userAction_" class="userAction" method="{1}">
        <result name="suceesss">/index.jsp</result>
        </action>
    </package>
</struts>
引入C3p0连接池：
创建c3p0配置文件：
在applicationContext.com读取到这个然后交给Spring容器注入到SessionFactory对象中
<!-- 配置c3p0连接池 -->
<!-- 指定spring读取db.properties配置 -->
<context:property-placeholder location="classpath:db.properties"  />
<!-- 1.将连接池 -->
<bean name="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource" >
    <property name="jdbcUrl" value="${jdbc.jdbcUrl}" ></property>
    <property name="driverClass" value="${jdbc.driverClass}" ></property>
    <property name="user" value="${jdbc.user}" ></property>
    <property name="password" value="${jdbc.password}" ></property>
</bean>
<bean name="sFactory" class="org.springframework.orm.hibernate5.LocalSessionFactotyBean">
<!-- 将连接池注入到sessionfactory ,hibernate 获得连接 -->
    <property name="dataSource" ref="dataSource"></property>
    <property name="hibernateProperties">
    <props>
        <!-- <prop key="hibernate.hbm2ddl.auto" >update</prop>
        <prop key="hibernate.connection.driver_class" >com.mysql.jdbc.Driver</prop>
        <prop key="hibernate.connection.username" >root</prop>
        <prop key="hibernate.connection.password" >123</prop> -->
        <prop key="hibernate.dialect" >org.hibernate.dialect.MySQL5InnoDBDialect</prop>
        <prop key="show_sql" >true</prop>
        <prop key="hibernate.format_sql" >true</prop>	
    </props>
</property>
Spring 整合hibernate:
Spring
<!-- 将sessionFactory配置到文件中
1仍然外部的hibernate.cfg.xml
<bean name="sFactory" class="org.springframework.orm.hibernate5.LocalSessionFactotyBean">
<property name="configLocation" value="class:hibernate.cfg.xml"></property>
</bean>
-->
<!-- 方式二：在Spring中配置 hibernate.cfg.xml-->
<bean name="sFactory" class="org.springframework.orm.hibernate5.LocalSessionFactotyBean">
    <property name="hibernateProperties">
        <props>
            <prop key="hibernate.hbm2ddl.auto" >update</prop>
            <prop key="hibernate.connection.driver_class" >com.mysql.jdbc.Driver</prop>
            <prop key="hibernate.connection.username" >root</prop>
            <prop key="hibernate.connection.password" >123</prop>
            <prop key="hibernate.dialect" >org.hibernate.dialect.MySQL5InnoDBDialect</prop>
            <prop key="show_sql" >true</prop>
            <prop key="hibernate.format_sql" >true</prop>
        </props>
    </property>
    <!-- 引入元数据 方式一:这是通过在列表中指定相应的实体-->
    <property name="mappingResource">
        <list> <value>com/leo/domain/user.hbm.xml</value></list>
    </property>
    <!-- 引入元数据方式二：直接可以读取这个包下面的所有的映射文件-->
    <property name="mappingDirectoryLocations">
        <value>classpath:com/leo/domain</value>
    </property>
</bean>
扩大Session的作用域：
在web.xml中配置扩大session的作用域：
<!--配置session的作用域 
注意 openSessionInView一定要在struts中的filter的之前
-->
<filter>
    <filter-name>openSessionInView</filter-name>
    <filter-class>
    org.springframework.orm.hibernate5.support.OpenSessionInViewFilter
    </filter-class>
</filter>
<filter-mapping>
    <filter-name>openSessionInView</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

