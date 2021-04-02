## HTTP

### Http简介

HTTP协议用于定义客户端与web服务器通迅的格式。
HTTP是hypertext transfer protocol（超文本传输协议）的简写，它是TCP/IP协议的一个应用层协议

### Http版本

HTTP协议的版本：HTTP/1.0、HTTP/1.1，其中1.0一次请求发起一次连接，响应过后连接自动断开。1.1里每次请求响应后连接将保持一段时间，这段时间内可以再执行请求响应。

### Http工作原理

HTTP使用请求-响应的方式进行传输，一个请求对应一个响应，并且请求只能是由客户端发起的。

HTTP默认端口号为80，但是你也可以改为8080或者其他端口。

**HTTP三点注意事项：**

- HTTP是无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
- HTTP是媒体独立的：这意味着，只要客户端和服务器知道如何处理的数据内容，任何类型的数据都可以通过HTTP发送。客户端以及服务器指定使用适合的MIME-type内容类型。
- HTTP是无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快

### Http请求

一个http请求包含一个请求行，若干请求头（消息头），若干请求体，**下面是一个请求报文的格式：**

![](https://www.runoob.com/wp-content/uploads/2013/11/2012072810301161.png)

#### 请求行

GET /books/java.html HTTP/1.1  请求方式 请求的资源名 所遵循的协议
请求行中的GET称之为请求方式，请求方式有：POST、GET、HEAD、OPTIONS、DELETE、TRACE、PUT，常用的有： GET、 POST

用户如果没有设置，默认情况下浏览器向服务器发送的都是get请求，例如在浏览器直接输地址访问，点超链接访问等都是get，用户如想把请求方式改为post，可通过更改表单的提交方式实现。	

其中GET方式在请求资源的URL后跟“？参数名=参数值&参数名=。。。”方式传递参数，传输的数据内容最大为1K
其中POST方式在请求实体中传输数据
除了用Form表单明确用method指定用post方式提交数据以外，其他的方式都是GET提交方式

####  请求头((request)

	Accept: text/html,image/*    客户端可以接受的数据类型
	Accept-Charset: ISO-8859-1	客户端接受数据需要使用的字符集编码
	Accept-Encoding: gzip,compress 客户端可以接受的数据压缩格式
	Accept-Language: en-us,zh-cn  可接受的语言环境
	Host: www.it315.org:80 想要访问的虚拟主机名
	If-Modified-Since: Tue, 11 Jul 2000 18:23:51 GMT 这是和缓存相关的一个头，带着缓存资源的最后获取时间
	Referer: http://www.it315.org/index.jsp 这个头表示当前的请求来自哪个链接，这个头和防盗链的功能相关
	User-Agent: Mozilla/4.0 (compatible; MSIE 5.5; Windows NT 5.0) 客户端的一些基本信息
	Cookie 会在后面讲会话技术的时候单讲
	Connection: close/Keep-Alive 指定是否继续保持连接
	Date: Tue, 11 Jul 2000 18:23:51 GMT 当前时间
### Http响应

**一个HTTP响应代表服务器向客户端回送的数据**，它包括： 一个状态行、若干消息头、以及实体内容 。

####  状态行

HTTP/1.1 200 OK
格式： HTTP版本号　状态码　原因叙述
状态码：
			200：请求处理成功
			302：请求重定向
			304、307：服务器通知浏览器使用缓存
			404：资源未找到
			500：服务器端错误

| 状态码  | 含义                                                         |
| ------- | ------------------------------------------------------------ |
| 100~199 | 表示成功接收请求，要求客户端继续提交下一次请求才能完成整个处理过程 |
| 200~299 | 表示成功接收请求已完成整个处理，常用200                      |
| 300~399 | 未完成请求客户需要进一步细化请求，例如请求重定向，301        |
| 400~499 | 客户端的请求有错误，常用404                                  |
| 500~599 | 服务器端出现错误，常用500                                    |

#### 若干响应头(response)

	Location: http://www.it315.org/index.jsp  配合302实现请求重定向
	Server:apache tomcat 服务器的基本信息
	Content-Encoding: gzip 服务器发送数据时使用的压缩格式
	Content-Length: 80 发送数据的大小
	Content-Language: zh-cn 发送的数据使用的语言环境
	Content-Type: text/html; charset=GB2312 当前所发送的数据的基本信息，（数据的类型，所使用的编码，用于定义网络文件的类型和网页的编码，决定浏览器将以什么形式、什么编码读取这个文件，这就是经常看到一些网页点击的结果却是下载一个文件或一张图片的原因）
	Last-Modified: Tue, 11 Jul 2000 18:23:51 GMT 缓存相关的头
	Refresh: 1;url=http://www.it315.org 通知浏览器进行定时刷新，此值可以是一个数字指定多长时间以后刷新当前页面，这个数字之后也可以接一个分号后跟一个URL地址指定多长时间后刷新到哪个URL
	Content-Disposition: attachment;filename=aaa.zip 与下载相关的头
	Transfer-Encoding: chunked 传输类型，如果是此值是一个chunked说明当前的数据是一块一块传输的
	Set-Cookie:SS=Q0=5Lb_nQ; path=/search 和cookie相关的头，后面课程单讲
	ETag: W/"83794-1208174400000" 和缓存机制相关的头
	Expires: -1 指定资源缓存的时间，如果取值为0或-1浏览就不缓存资源
	Cache-Control: no-cache  缓存相关的头，如果为no-cache则通知浏览器不缓存
	Pragma: no-cache   缓存相关的头，如果为no-cache则不缓存
	以上三个头都是用来控制缓存的，是因为历史原因造成的，不同的浏览器认识不同的头，我们通常三个一起使用保证通用性。
	Connection: close/Keep-Alive   是否保持连接
	Date: Tue, 11 Jul 2000 18:23:51 GMT 当前时间
[![cetf4P.png](https://z3.ax1x.com/2021/04/02/cetf4P.png)](https://imgtu.com/i/cetf4P)

## Request与Response

### Request

request这个对象不用事先声明，就可以在JSP网页中使用，在编译为Servlet之后，它会转换为javax.servlet.http.HttpServletRequest形态的对象，HttpServletRequest对象是有关于客户端所发出的请求的对象，只要是有关于客户端请求的信息，都可以藉由它来取得，例如请求标头、请求方法、请求参数、客户端IP，客户端浏览器等等信息

**ServletRequest -- 通用request，提供一个request应该具有的最基本的方法.HttpSerletRequest是Rquest的子类针对http协议进行了进一步的增强**

#### Request的操作

##### 获取客户机信息

  getRequestURL方法返回客户端发出请求完整URL
  getRequestURI方法返回请求行中的资源名部分
  getQueryString 方法返回请求行中的参数部分
  getRemoteAddr方法返回发出请求的客户机的IP地址
   getMethod得到客户机请求方式
   getContextPath 获得当前web应用虚拟目录名称

##### 获取请求头信息

获得客户机请求头
      getHeader(name)方法 --- String 
      getHeaders(String name)方法 --- Enumeration<String>枚举变量<string类型的>
      getHeaderNames方法 --- Enumeration<String>
获得具体类型客户机请求头
       getIntHeader(name)方法  --- int
       getDateHeader(name)方法 --- long(日期对应毫秒)

##### 获取请求参数

request.getParameter()

 浏览器以什么编码来发送请求参数? 浏览器以什么编码打开的表单页面,就用什么编码发送这个页面提交的数据。服务器以什么编码来打开呢?如果不指定,则使用ISO8859-1,这样如果请求参数中有中文必然就乱码了
对于POST提交,可以设置request.setCharacterEncoding("utf-8");明确的通知服务器以浏览器发送过来的编码来打开数据就可以解决乱码但是上面的方法只对请求中实体内容部分起作用,所以GET提交的乱码并不能解决.
 对于GET提交的乱码,只能手动的进行编解码从而解决乱码问题:
            String username = request.getParameter("username");
	 username = new String(username.getBytes("iso8859-1"),"utf-8");

##### 利用请求域传递对象

  作用范围:整个请求链上
        生命周期:当服务器收到一个请求,创建出代表请求的request对象,request开始.当请求结束,服务器销毁代表请求的request对象,request域结束.
        作用:在整个请求链范围内共享数据,通常我们在Servlet中处理好的数据会存入request域后请求转发到jsp页面来进行展示

        setAttribute
        getAttribute
        removeAttribute
##### 实现请求转发和请求包含

  (1)请求转发(.forward()):
            this.getServletContext().getRequestDispatcher("").forward(request,response);
            request.getRequestDispatcher("").forward(request,response); 

    请求转发是希望将请求交给另外一个资源执行,所以应该保证只有最后真正要执行的资源才能够输出数据,所以:
    请求转发时,如果已经有数据被写入到了response的缓冲区,但是这些数据还没有被发送到客户端,则请求转发时,这些数据将会被清空.但是清空的只是响应中的实体内容部分,头信息并不会被清空.
    而请求转发时已经有数据被打给了浏览器,那么再进行请求转发,不能成功,会抛出异常,原因是响应已经结束了,再转发交给其他人没意义了
    在最终输出数据的Servlet执行完成后,response实体内容中的数据将会被设置为已提交的状态,再往里写数据也不会起作用
   (2)请求包含(.include()):将两个资源的输出进行合并后输出多个资源同是输出
            this.getServletContext().getRequestDispatcher("").include(request,response);
            request.getRequestDispatcher("").include(request,response);

    被包含的Servlet程序不能改变响应消息的状态码和响应头，如果它里面存在这样的语句，这些语句的执行结果将被忽略常被用来进行页面布局
  (3)三种资源处理方式的区别
            请求重定向
                response.sendRedirect();
            请求转发
                request.getRequestDispatcher().forward();
            请求包含
                request.getRequestDispatcher().include();

    请求重定向和请求转发的区别:
        请求重定向地址栏会发生变化.请求转发地址栏不发生变化.
        请求重定向两次请求两次响应.请求转发一次请求一次响应.
                
        如果需要在资源跳转时利用request域传递域属性则必须使用请求转发 request.getRequestDispatcher().forward();
        如果希望资源跳转后修改用户的地址栏则使用请求重定向response.sendRedirect();
        如果使用请求转发也可以重定向也可以,则优先使用请求转发,减少浏览器对服务器的访问次数减轻服务器的压力.
## Session与Cookie



## Servlet

### Servlet生命周期

### Servlet调用过程

### Servlet过滤器与监听器

### Servlert经典实例



