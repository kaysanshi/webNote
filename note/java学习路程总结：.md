### javase：

2016年冬12月我大一上学期的枯燥的生活结束了。放寒假回家后买了台电脑，我书顺便给我哥要了套视频，拿到视频后有个java学习路线，我就是按照那个进行学习的不过在后来我也是有所改变。具体的学习方式可以去我上篇的文章。下面我将每个学习的章节技术点以黑体加粗展示

先是从**认识面向对象**开始，因为我学过了C语言但是不是特别的熟练，而C语言没有对象的概念，他是面向过程的语言，所以就是认识什么是对象对象，java编程思想中有这样一句 “一切皆对象”，也就是在使用语言时可以把每一个部分当做对象。其实主要了解什么是对象，自己心中有这个概念就可以。



**开发环境，工具** 是学习任何一门语言的必备的技能，你要想搞明白这个语言的使用，“工欲善必先利器”  你就必须先把环境搭建起来，好多人工作了几年都不能把jdk给正确的安装下来，还需要baidu,这个是基础的技能，所以当我们学习的时候一定熟练掌握，比如最简单的就是把jdk的bin目录直接配置到path目录下。开发工具，你可能看的视频很多开始都是在记事本上给你演示的，其实在真正的开发中是不可能这样使用的，你想想一个个项目有时沉淀了四五年，那代码量是多么的多，记事本能搞定吗，所以先是推荐Eclipse 因为开源免费，后序可以搞Idea进行使用，notePad++进行查看代码，Hbuilder用于写html，js这些，这样你的环境和工具都有了那么你是不是就能够进行学习了。



**java基本数据类型**  基本数据类型是这个语言的基础，java有八大基本数据类型，这些基本数据类型修饰变量后让其变量赋予真正的意义。我的前辈给我说他面试到一位工作三年的 java基本数据类型 都不能答得上来，所以我们学校时一定把基础给扎实。java八大基本数据类以及他的包装类这些都要熟练记忆，`char ,boolean,byte,short,int,double,float,long`. 这些给记牢以及对应的包装类是啥。有了这些基本数据类型看下他们默认值为什么，这个要牢记。学完这些后就可以去看运算符了。



**操作符（运算符，关键字，修饰符，循环，条件语句）** 基本类型掌握了我们就可以根据这些进行操作，比如 int 类型的+,-，*，/ ,%,的操作（加，减，乘，除，模） 然后循环语句，条件判断语句， 类中字段的修改符，private,public ,protect,默认。 关键字也是用于修饰变量的，比如 `public static void main(){}` 这个方法是开放的 ，静态的，返回值为空的。关键字就是 static ,还有很多关键字 比如下方，现阶段不要求全会，至少要保证以后要回，因为这些以后都要用到。

| abstract     | 表明类或者成员方法具有抽象属性                               |
| ------------ | ------------------------------------------------------------ |
| assert       | 断言，用来进行程序调试                                       |
| boolean      | 基本数据类型之一，声明布尔类型的关键字                       |
| break        | 提前跳出一个块                                               |
| byte         | 基本数据类型之一，字节类型                                   |
| case         | 用在switch语句之中，表示其中的一个分支                       |
| catch        | 用在异常处理中，用来捕捉异常                                 |
| char         | 基本数据类型之一，字符类型                                   |
| class        | 声明一个类                                                   |
| const        | 保留关键字，没有具体含义                                     |
| continue     | 回到一个块的开始处                                           |
| default      | 默认，例如，用在switch语句中，表明一个默认的分支。Java8 中也作用于声明接口函数的默认实现 |
| do           | 用在do-while循环结构中                                       |
| double       | 基本数据类型之一，双精度浮点数类型                           |
| else         | 用在条件语句中，表明当条件不成立时的分支                     |
| enum         | 枚举                                                         |
| extends      | 表明一个类型是另一个类型的子类型。对于类，可以是另一个类或者抽象类；对于接口，可以是另一个接口 |
| final        | 用来说明最终属性，表明一个类不能派生出子类，或者成员方法不能被覆盖，或者成员域的值不能被改变，用来定义常量 |
| finally      | 用于处理异常情况，用来声明一个基本肯定会被执行到的语句块     |
| float        | 基本数据类型之一，单精度浮点数类型                           |
| for          | 一种循环结构的引导词                                         |
| goto         | 保留关键字，没有具体含义                                     |
| if           | 条件语句的引导词                                             |
| implements   | 表明一个类实现了给定的接口                                   |
| import       | 表明要访问指定的类或包                                       |
| instanceof   | 用来测试一个对象是否是指定类型的实例对象                     |
| int          | 基本数据类型之一，整数类型                                   |
| interface    | 接口                                                         |
| long         | 基本数据类型之一，长整数类型                                 |
| native       | 用来声明一个方法是由与计算机相关的语言（如C/C++/FORTRAN语言）实现的 |
| new          | 用来创建新实例对象                                           |
| package      | 包                                                           |
| private      | 一种访问控制方式：私用模式                                   |
| protected    | 一种访问控制方式：保护模式                                   |
| public       | 一种访问控制方式：共用模式                                   |
| return       | 从成员方法中返回数据                                         |
| short        | 基本数据类型之一,短整数类型                                  |
| static       | 表明具有静态属性                                             |
| strictfp     | 用来声明FP_strict（单精度或双精度浮点数）表达式遵循[IEEE 754](https://baike.baidu.com/item/IEEE 754)算术规范 |
| super        | 表明当前对象的父类型的引用或者父类型的构造方法               |
| switch       | 分支语句结构的引导词                                         |
| synchronized | 表明一段代码需要同步执行                                     |
| this         | 指向当前实例对象的引用                                       |
| throw        | 抛出一个异常                                                 |
| throws       | 声明在当前定义的成员方法中所有需要抛出的异常                 |
| transient    | 声明不用序列化的成员域                                       |
| try          | 尝试一个可能抛出异常的程序块                                 |
| void         | 声明当前成员方法没有返回值                                   |
| volatile     | 表明两个或者多个变量必须同步地发生变化                       |
| while        | 用在循环结构中                                               |



**面向对象：**真正的了解到面向对象是从认识类开始的，类是什么 类是封装了对象的行为和属性。从上面一句是不是你要去了解 什么是行为，什么是属性。明白这个后就会继续学习 抽象类，静态类，内部类，接口，三大特性（封装，继承，多态）， 到这个时候你就会了解 重载与重写的区别，为什么重载构造函数， 为什么重写  顺便稍微了解一下内存结构，对象存储位置 ，这样其实是数据结构中的，所以要去看数据结构的知识。

[![D1Skp8.png](https://s3.ax1x.com/2020/11/21/D1Skp8.png)](https://imgchr.com/i/D1Skp8)

**自动拆箱与装箱：**  自动装箱就是Java自动将原始类型值转换成对应的对象，比如将int的变量转换成Integer对象，这个过程叫做装箱，反之将Integer对象转换成int类型值，这个过程叫做拆箱。因为这里的装箱和拆箱是自动进行的非人为转换，所以就称作为自动装箱和拆箱。原始类型byte, short, char, int, long, float, double 和 boolean 对应的封装类为Byte, Short, Character, Integer, Long, Float, Double, Boolean。 这样是不是你会发现我在学习基本数据 类型时与这个包装类却别了



**数组:** 数组的特性 ，数组的创建，数组元素的查找，更新，删除。明白数组的存储。下面去了解java中的常用类。



**java常用类：** `Object`,`String,StringBuffer,StringBuild,System,Math,Redom,Date,Date Format,BigDecimal,Pattern与Matcher类, Properties`   去逐个了解上面的基本类的方法和特性，比如 String,StringBuffer,StringBuild的使用场景，不要了解太深去会用就可以。



**异常**：在上面的类使用时是不是idea有时会同时try catch包裹 这就是编译器告诉你程序可能会有异常 ，你去了解什么是异常， 异常的关键字，throw,throws,Exception类 运行时异常，非运行时异常 

[![D1SMt0.png](https://s3.ax1x.com/2020/11/21/D1SMt0.png)](https://imgchr.com/i/D1SMt0)

**集合**：了解完异常后来学习集合，明白集合的特性，两个顶级的接口Collection，Map,以及他们的不同的实现类List（ArrayList,LinkedList,Vector）,Set（HashSet－LinkedHashSet，TreeSet），集合遍历Iterator,Hash操作Map（HashMap-LinkedHashMap,TreeMap）,各个的特性  

[![D19owj.png](https://s3.ax1x.com/2020/11/21/D19owj.png)](https://imgchr.com/i/D19owj)

**IO操作**: 去学习流的两大顶级接口  输入流InputStream，Read,输出流OutputStream，Write,字符流（Read,Write），字节流（inputStream,OutputStream），File类，Chanel类  的操作。自己实现文件的读取和文件的写入。

[![D19qf0.jpg](https://s3.ax1x.com/2020/11/21/D19qf0.jpg)](https://imgchr.com/i/D19qf0)

**多线程**：去了解进程与线程的区别，并且牢记于心。多线程特性，实现多线程的方式（Thread,Runnable）,线程的生命周期，应用 ，自己手写最有名的余票代码。 



**Swing**:  这个选学，不过我当时学了好多，因为我用swing搞了个坦克大战看人家的视频，现在还可以使用的，项目地址在github上 javase 中。



**网络编程**：Socket,端口，协议，  这个我跟着视频做了qq聊天。简易的根据socket



**注解：**  学习完后去了解注解 元数据，基本注解，注解实现 等。这样你的javaSE基本上就可以学习完毕了 ，不过需要你大量的练习，练习以后才能够出效果。

我这里贴上我的se学习的代码在git上  https://github.com/kaysanshi/javase

学习完以上经过大量的练习去牢记代码，去明白其中的实现原理

### javaEE

在进行java EE的学习之前我是做了一些准备，先把html css js 基础这些给学习了，因为在webapp的数据额呈现的方式都是通过html解析展示到用户的客户端，在我们后面接触的东西99%的都是B/S架构所以先把这个些技术点给学会，会用标签技术即可。我还记的在大二的时候我们的web前端课程上我仿照梅赛德斯-奔驰网站做了个静态页面，可以看下效果图，里面主要用了html+CSS+JS这些。

[![D3FceU.png](https://s3.ax1x.com/2020/11/21/D3FceU.png)](https://imgchr.com/i/D3FceU)

[![D3FrQ0.png](https://s3.ax1x.com/2020/11/21/D3FrQ0.png)](https://imgchr.com/i/D3FrQ0)

搞定了那些开始去了解后端是如何进行构建web项目的，依次是从下面这些内容进行学习的。同样 黑色字体是需要学习的技术栈，里面包含了一些小的技术点



**XML 解析**   先从XML解析 因为XML也是一门标记性语言，在webapp项目中使用的是最多的。从XML 文档声明，元素，属性，实体引用，xml约束，dtd约束语法，xml解析(包含 dom 解析，sax解析)  以及是如何使用的在webapp项目中。



比如下面的一个简单地XML结构：

```xml
<? xml version="1.0" encoding="gbk"?>
<? xml-stylesheet type="text/css"href="1.css"?>
<country>
	<a>中国</a>
	<b>美国</b>
	<c>法国</c>
	<d>日本</d>
</country>
```

**web应用概念** 主要去了解，静态web资源，动态web资源，常见的动态web资源开发技术，C/S B/S相关概念。web应用的目录结构，web.xml作用。



**Tomcat**  有上面的web应用的概念，那我们的应用应该在哪个地方运行呢，这样一思考是不是我们需要一个容器进行加载这些所需要的东西呢，tomcat 安装 ，tomcat配置。下图显示的是我在服务器中的搭建的tomcat.



[![D3ZoZV.png](https://s3.ax1x.com/2020/11/21/D3ZoZV.png)](https://imgchr.com/i/D3ZoZV)

**http协议** http请求(请求行，请求头，请求实体)，http响应 （状态行，响应头）学习这个的时候我们好像还没学习计算机网络，因为计算机网络是大二下学期才开始学的，当时还没有太多的概念只是跟着视频进行学习。学习完后就去百度这个东西到底是干啥么。后面慢慢明白技术的本质了，他解决了网络之间的通信的问题，后面你可能还要去了解https 为什么http协议不安全，以及CxSF。



**response** 什么是response,repose输出数据，实现以下几个功能，文件下载，定时刷新页面，控制是否缓存资源，请求重定向。设置编码格式

**request** 什么是request ,实现以下几个功能，获取客户机信息，获取请求头信息，获取请求参数，获取请求域传递对象，实现请求转发与请求包含。

[![D3Zxqx.png](https://s3.ax1x.com/2020/11/21/D3Zxqx.png)](https://imgchr.com/i/D3Zxqx)

**jsp**  java server pages 概念，语法，指令，jsp标签，九大隐式对象（request,response,config,application,page,session,pageContent,exception,out） 四大作用域 （PageContext.APPLICATION_SCOPE，PageContext.SESSION_SCOPE，PageContext.REQUEST_SCOPE，PageContext.PAGE_SCOPE ） 

**会话技术-Session,Cookie**  需要学习 会话的概念，Session 服务器技术，作用域，声明周期，原理，URL重写技术 ，Session 案例（用户登录注销，防止表单重复提交，实现一次性验证码） cookie的概念，cookie案例. 比如下面两幅图片就是浏览器中cookie和session的显示。 

[![D3eiJe.png](https://s3.ax1x.com/2020/11/21/D3eiJe.png)](https://imgchr.com/i/D3eiJe)

[![D3eEQA.png](https://s3.ax1x.com/2020/11/21/D3eEQA.png)](https://imgchr.com/i/D3eEQA)

学完这些可以做个小的项目了我记得当时我们做了一个学生管理系统具体部分代码文件如下图所示。

[![D3uteI.png](https://s3.ax1x.com/2020/11/21/D3uteI.png)](https://imgchr.com/i/D3uteI)

**javaBean** :javabean用于把大量的java代码提取出来，jsp页面负责接受页面的请求 javaBean负责装处理的数据 ,了解即可

**MVC模式**  三层架构的概念。 软件分层。

**Servlet** ：为什么出现Servlet, Servlet声明周期（init(),service(),destory()），Servlet调用过程，

**Filter**：Filter概念，FilterChain,FilterConfig,过滤器的应用场景

**listener**：监听器概念，我还记得的学习这个的时候是大二寒假了，当时寒假在家我学习的这个，还有一个就是那个EStore项目：当时还总结了都学习了哪些东西。

[![D3KZ9S.png](https://s3.ax1x.com/2020/11/21/D3KZ9S.png)](https://imgchr.com/i/D3KZ9S)

这个也是在寒假学习的，这样算下来写代码的时间刚好一年，这个时间节点，我视乎迷茫了我不知道后面要学习什么了，当时听人家说框架不用学习，然而在一次和其他人交流时说出来可能不用你会框架但是你必须要会用，面试会问的。好家伙当时我听到这里我就准备准备去找些资源看看学习路线的需求，等到开学我就在闲暇时间学习框架去了。

**数据库学习** ：在学习框架之前我把数据库给从新过了一遍，因为当时正好我们要学习这个数据库原理。所以顺便把mysql和sqlserver进行学习了，我记得当时数据库课设用C#做了个图书管理系统。小的项目，具体如下图效果图。我翻了好久才找到课设的报告截图：

[![D3MAbR.png](https://s3.ax1x.com/2020/11/21/D3MAbR.png)](https://imgchr.com/i/D3MAbR)

**jdbc** ：当时做课程的时候使用的C# 进行做的这个也就是当时图快就直接去书籍上看实践直接使用了。jdbc是学习的贯穿了整个javaee应该说，因为在jsp做课设时就是用到jdbc连接，当时是自己封装的操作类，后来使用了queryrunner，发现了用人已经将数据库操作的已经封装成了对象，只需要拿来用即可，当时自己还在使用自己封装的，感觉不是那么的丝滑，当使用这个感觉原来还可以这么操作。当时的认知还是不多的，因为没有太多的人去指引你去学习什么，只能自己摸索。转而再回顾想想坚持下来不容易，因为自学过程遇到了很多的坑，是你意想不到的。代码环境问题让你头疼，代码bug让你头疼，配置让你头疼。等等等，这是我遇到了个益友。他在我们大一上学期的时候就已经开始学习java了，还好才大二下开始，时间节点不是特别的晚。在接下来的学习我们常常一起在机房进行熬夜学习。我常常跟随他的脚步进行学习。以上就是我没有学习框架前所学的一些东西。



### 框架学习

对于框架实在是太多我现在还正在通过不断的学习来丰富一些框架知识，但是以下是基本的框架知识，是有必要熟悉和熟练使用的。

**maven**  maven是apache下的纯java开发项目管理工具，maven常用标签，`<groupId>` : 的值就是项目的包名，`<artifactld>`  模块名，一般使用项目名，``<version>`` 版本 packaging :打包方式， 以及maven的打包命令。

**Struct2** 当时我学习struct2时候还有用的，现在感觉是不是用的很少了，都用SpringMVC给替代了，反正现在不是很流行了。当时学习的时候主要从以下方面进行学习了，依赖项，struts 动作，stucts.xml，action动作，模型，拦截器，结果类型，OGNL/值栈，标签 API 这些方式学习，这个时可以做些小的demo

**Hibernate** 学习了struct2这个可以搞定了请求的封装接受参数的问题了，但是对数据库的操作还是不是框架，所以就开始学习Hibernate，如何操作数据库的，包含hibernate概念，(SqlSessionFacctory,session), HQL查询，标准的查询，原生SQL, O/R 映射。主键策略，对象状态，事务，关系模型，管理级别，以及 一对多，对对一查询。

**Spring** 学习了之后开始学习spring了，包含spring概念，优缺点，七大模块，IOC容器（BeanFactory, ApplicationContext），Bean，Bean的自动装配， DI, AOP(反向代理)

**SSh项目练习** 主要是structs+spring+hibernate项目，一个小的人员管理，增删改查操作

***SpringMVc***  springmvc概念，springmvc请求流程，springmvc参数绑定(形参绑定，对象类型，包装类型) requestMapping访问路径映射，

**Mybatis** ：mybaits概念，mybatis功能架构，为什么使用mybatis,(SqlSessionFactory,sqlsession,SqlMapConfig) mybatis映射 ，mapper动态代理，输入参数类型parameterType(基本数据类型，pojo类型，包装类型), 输出参数类型 （resultType ,resultMap） 标签，Spring整合mybatis

**SSM项目**  一个小的增删改查项目，用springmvc+spring+Mybatis架构进行构建

**Linux** linux目录文件，linux,常见的的命令，文件操作，jdk环境搭建，tomcat ,maven,mysql的环境的构建

**redis**  redis的基本类型，redis的应用场景。缓存击穿，穿透，雪崩，缓存在spring中的使用

**Spring注解开发** ：spring中常用的注解的学习。

**bos项目** ：spring，struts2,hibernate,maven

​    hibernate:由数据库生成pojo和映射文件
​	log4j：日志输入输出
​	pinyin4j：汉语转拼音，及其缩写
​	json：数据格式的转换
​	webservice(cxf,)：对接口的调用，一个项目调用另一个项目中的接口模块代码，远程开发调用
​	quartz：任务调度，相当于一个定时器功能
​	,javamail：邮件的发送
​	Apachepoi：excel的操作，导入导出功能
​	ehcache：缓存的使用，对其中的数据可以缓存一部分到浏览器中以及本地
​	,shiro：权限管理控制，认证
​	前端：easyUI：window(弹出窗口)，messager(消息提示)，menubutton(菜单)，combox(下拉框)
​	ztree：菜单节点树
​	hightcharts：图表定制,饼图，折线图，柱状图
​	ocupload：一键上传，
​	ExtJs：控件
​	jfreechart,

**商城项目** : SOA,分布式架构，dubbo,nginx,FastDFS,Redis,Redis持久化方案，Redis集群搭建，solr,ActiveMQ, 以及总结，一定要多总结，总结才能够收获，没有总结是收获不到什么东西的。

**SpringBoot** ：springboot概念，原理，应用，整合

**SpringCloud**：springcloud,组件，服务注册与发现机制，分布式配置中心，远程调用，分布式事务，Quartz任务调度,

**itoken项目** ：docker ,sso,redis,MQ,nginx,Fastdfs,spring cloud netflix.git,

**SpringBoot 整合组件**  spring boot与缓存（redis） ,Springboot与检索（ElsaticSearch）,Springboot与jdbc(mybatis,jpa,jdbctempate), Springboot 与thymeleaf.

**Spring Security Outh2** 这个是用于认证和授权的，我们需要在实际的项目中进行使用。

其实我们学习很多只是停留在会用层面上，并不深入，当我们在面试中只是了解皮毛肯定不行，所以需要我们花大量的时间去研究其实现过程，中间练习的过程中我们需要不断的去踩坑，不断的解决问题，才能够真正的实现其中的问题，我们需要不断的去探索，不过在技术学习的过程中我们也应该去学习业务知识因为我们的技术始终是个工具是为业务去提供更好的解决方案。