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

## HTTPS

HTTPS （全称：Hyper Text Transfer Protocol over SecureSocket Layer），是以安全为目标的 HTTP 通道，在HTTP的基础上通过传输加密和[身份认证](https://baike.baidu.com/item/身份认证/5294713)保证了传输过程的安全性 [1]  。HTTPS 在HTTP 的基础下加入[SSL](https://baike.baidu.com/item/SSL/320778)，HTTPS 的安全基础是 SSL，因此加密的详细内容就需要 SSL。 HTTPS 存在不同于 HTTP 的默认端口及一个加密/身份验证层（在 HTTP与 [TCP](https://baike.baidu.com/item/TCP/33012) 之间）。这个系统提供了身份验证与加密通讯方法。它被广泛用于[万维网](https://baike.baidu.com/item/万维网/215515)上安全敏感的通讯，例如交易支付等方面 （来源于：百度百科）

### **为什么出现https呢？**

在http请求都是通过明文请求的而不法分子很容易通过抓包工具获取到我们在网络间传输的信息。想象我们在支付时银行卡密码通过明文在网络间传输那是不是风险很大。为了解决这个问题，Netscape 公司制定了HTTPS协议，HTTPS可以将数据加密传输，也就是传输的是密文，即便黑客在传输过程中拦截到数据也无法破译，这就保证了网络通信的安全。

**要想了解 HTTPS 为何安全，还得继续了解一下这些概念：明文，密文，密钥 ，加密算法、摘要算法、数字签名和数字证书。**

**明文**： 明文指的是未被加密过的原始数据。
**密文**：明文被某种加密算法加密之后，会变成密文，从而确保原始数据的安全。密文也可以被解密，得到原始的明文。
**密钥**：密钥是一种参数，它是在明文转换为密文或将密文转换为明文的算法中输入的参数。密钥分为对称密钥与非对称密钥，分别应用在对称加密和非对称加密上。

### 加密算法

#### **对称密钥密码体制**

对称密钥密码体制，即加密密钥和解密密钥是使用相同的密码体制。对称密钥加密技术的缺点之一就是发送者和接收者在对话之前，一定要有一个共享的密钥，所以不太安全。双方必须协商一个保密的密钥。

其加密过程如下：**明文 + 加密算法 + 私钥 => 密文**
 解密过程如下：   **密文 + 解密算法 + 私钥 => 明文**

对称加密中用到的密钥叫做私钥，私钥表示个人私有的密钥，即该密钥不能被泄露。
 其加密过程中的私钥与解密过程中用到的私钥是同一个密钥，这也是称加密之所以称之为“对称”的原因。由于对称加密的算法是公开的，所以一旦私钥被泄露，那么密文就很容易被破解，所以对称加密的缺点是密钥安全管理困难。加密解密算法需要密钥双方必须知道，但密钥无法通过网络进行发送。这样又有问题了。

[![cnzJEt.png](https://z3.ax1x.com/2021/04/03/cnzJEt.png)](https://imgtu.com/i/cnzJEt)

#### **公钥密码体制（非对称加密） RSA**

公钥密码体制非对称加密）使用不同的加密密钥与解密密钥。公钥密码体制产生的主要原因有两个：一是对称密钥密码体制的密钥分配问题，二是对数字签名的需求。

在公钥密码体制（非对称加密）中不必双方协商一个保密的密钥，而是有一对钥匙，一个是保密的，称为私钥；另一个是公开的，称为公钥。

在公钥密码体制非对称加密）中，加密密钥是公开的，解密密钥(私钥)是需要保密的，加密算法和解密算法也是公开的。

公钥密码体制非对称加密）的加密和解密有如下特点：

**用私钥加密的数据，只有对应的公钥才能解密；用公钥加密的数据，只有对应的私钥才能解密。**

[![cuC0J0.png](https://z3.ax1x.com/2021/04/03/cuC0J0.png)](https://imgtu.com/i/cuC0J0)

**密钥对产生器**产生出接收者 B 的一对密钥，即加密密钥 PK 和解密密钥 SK。发送者 A 用 B 的公钥 PK 作为加密密钥来加密信息，B 接收后用解密密钥 SK 解密。

使用对称密钥时，由于双方使用同样的密钥，因此在通信信道上可以进行一对一的双向保密通信，双方都可以用同一个密钥加密解密。

使用公开密钥时，在通信信道上可以是多对一的单向保密信道。即可以有多人持有 B 的公钥，但只有 B 才能解密。

[![cuiW26.png](https://z3.ax1x.com/2021/04/03/cuiW26.png)](https://imgtu.com/i/cuiW26)

**摘要算法**

摘要算法的主要特征是加密过程不需要密钥，并且经过加密的数据无法被解密，目前可以被解密逆向的只有CRC32算法，只有输入相同的明文数据经过相同的消息摘要算法才能得到相同的密文。

[![cuFCIs.png](https://z3.ax1x.com/2021/04/03/cuFCIs.png)](https://imgtu.com/i/cuFCIs)

**数字签名**

用加密系统对报文进行签名，以说明是谁编写的报文，同时证明报文未被篡改过，这种技术称为**数字签名**。

数字签名是附加在报文上的特殊加密校验码。使用数字签名的好处有：

签名可以证明是作者编写了这条报文。只有作者才会有最机密的私有密钥，因此，只有作者才能计算出这些校验和。签名可以防止报文被篡改，如果有人在报文传输过程中对其进行了修改，校验和就不再匹配了。数字签名通常是用非对称公开密钥技术产生的。

![img](https://pics4.baidu.com/feed/279759ee3d6d55fb091dbbe00771574222a4ddfd.jpeg?token=e7c500b51f53e9430fa5fb8e5b81aec4&s=1AAA7A23BBAC6C035C5DB0D20000C0B1)

看上图，任何人都能用 A 的公钥 PK 对密文进行 E 运算后得到 A 发送的明文。可见这种通信并非为了保密，而是为了进行签名和核实签名，即确认此信息是 A 发送的（使用 A 的密钥进行加密的报文，只有使用 A 的公钥才能正确解密）。 但上述过程仅对报文进行了签名，对报文 X 本身却未保密，所以要采用下图的方法，同时实现秘密通信和数字签名。

![img](https://pics4.baidu.com/feed/7acb0a46f21fbe09cbc6d2da0233143b8644ad3b.jpeg?token=1367b3bce22351dd015ff3c6828abe73&s=1AA87A228FD844CA1C5590C60000E0B1)

**数字证书**

假如你想访问一个网站，怎么确保对方给你的公钥是你想访问的网站的公钥，而不是被中间人篡改过的？

数字证书的出现就是为了解决这个问题，它是由数字证书认证机构颁发的，用来证明公钥拥有者的身份。换句话说，数字证书的作用就相当于人的身份证，身份证证明了张三就是张三，而不是别人。

**数字证书一般包含以下内容**：

对象的名称（人、服务器、组织等）；过期时间；证书发布者（由谁为证书担保）；来自证书发布者的数字签名；对象的公钥；对象和所用签名算法的描述性信息。任何人都可以创建一个数字证书，但由谁来担保才是重点。

**数字证书的数字签名计算过程**：

用摘要算法对数字证书的内容计算出摘要；用数字证书的私钥对摘要进行加密得到数字签名。

![img](https://pics0.baidu.com/feed/267f9e2f07082838be9b1f2bd2cab1094d08f1d3.jpeg?token=470c61ae6abdb997e56bcca1ae028cc5&s=780C34720BC840494CF561DE0000C0B1)

当浏览器收到证书时，会对签名颁发机构进行验证，如果颁发机构是个很有权威的公共签名机构，浏览器可能就知道其公开密钥了（浏览器会预装很多签名颁发机构的证书）。如果对签名颁发机构一无所知，浏览器通常会向用户显示一个对话框，看看他是否相信这个签名发布者。

因为数字证书的公钥是公开的，任何人都可以用公钥解密出数字证书的数字签名的摘要，然后再用同样的摘要算法对证书内容进行摘要计算，将得出的摘要和解密后的摘要作对比，如果内容一致则说明这个证书没有被篡改过，可以信任。

这个过程是建立在被大家所认可的证书机构之上得到的公钥，所以这是一种安全的方式。

![img](https://pics3.baidu.com/feed/2cf5e0fe9925bc31cf2fb32c368c95b9ca1370eb.jpeg?token=e7790f915cdf3d8db287d67ad70db3f6&s=7E28346313CF614B4AFC40DA0000C0B1)

### Https请求的整个流程

**一个HTTPS请求实际上包含了两次HTTP传输，可以细分为8步。**

[![cuEneS.png](https://z3.ax1x.com/2021/04/03/cuEneS.png)](https://imgtu.com/i/cuEneS)

1.客户端向服务器发起HTTPS请求，连接到服务器的443端口

2.服务器端有一个密钥对，即公钥和私钥，是用来进行非对称加密使用的，服务器端保存着私钥，不能将其泄露，公钥可以发送给任何人。

3.服务器将自己的公钥发送给客户端。

4.客户端收到服务器端的证书之后，会对证书进行检查，验证其合法性，如果发现发现证书有问题，那么HTTPS传输就无法继续。严格的说，这里应该是验证服务器发送的数字证书的合法性，关于客户端如何验证数字证书的合法性，下文会进行说明。如果公钥合格，那么客户端会生成一个随机值，这个随机值就是用于进行对称加密的密钥，我们将该密钥称之为client key，即客户端密钥，这样在概念上和服务器端的密钥容易进行区分。然后用服务器的公钥对客户端密钥进行非对称加密，这样客户端密钥就变成密文了，至此，HTTPS中的第一次HTTP请求结束。

5.客户端会发起HTTPS中的第二个HTTP请求，将加密之后的客户端密钥发送给服务器。

6.服务器接收到客户端发来的密文之后，会用自己的私钥对其进行非对称解密，解密之后的明文就是客户端密钥，然后用客户端密钥对数据进行对称加密，这样数据就变成了密文。

7.然后服务器将加密后的密文发送给客户端。

8.客户端收到服务器发送来的密文，用客户端密钥对其进行对称解密，得到服务器发送的数据。这样HTTPS中的第二个HTTP请求结束，整个HTTPS传输完成。

参考：《码农翻身》

参考：《菜鸟教程》

## TCP与UDP

在介绍TCP与UDP之前我们先看下计算机网络分层模型。OSI七层网络模型和TCP/IP四层概念模型，以及对应的网络协议

TCP: 传输协议TCP

UDP：用户数据协议UDP

[![cKkgQP.png](https://z3.ax1x.com/2021/04/04/cKkgQP.png)](https://imgtu.com/i/cKkgQP)

### TCP

TCP服务模型包括面向连接服务和可靠数据传输服务。当某个应用程序调用TCP作为其运输协议时，该应用程序就能获得来自TCP的这两种服务。

- 面向连接服务：在应用层数据报文开始流动之前，TCP让客户和服务器相互交换运输层控制的信息。这个握手过程提示客户端和服务端做好准备。在握手阶段一个TCP连接就是两个进程的套接字之间建立了，这个过程是双工的。即连接双方的进程可以在此连接上同时进行报文的收发。当应用程序结束报文发送时，必须拆除该连接。
- 可靠的数据传输服务：通信进程能够依靠TCP，无差错，按适当的顺序交互所有的数据，当应用程序的一端将字节流传进套接字时。他能够依靠TCP将相同的字节流交互到接受方的套接字，而没有字节的丢失和冗余。

#### TCP连接

[![cQ9OXT.png](https://z3.ax1x.com/2021/04/05/cQ9OXT.png)](https://imgtu.com/i/cQ9OXT)

**TCP连接的建立过程主要解决以下3个问题：**

1. 要使每一方都能够确定对方的存在
2. 要允许双方协商一些参数（如：最大的窗口值，）
3. 能够对运输实体资源进行分配。

### TCP 三次握手四次挥手原理

在谢希仁著《计算机网络》第四版中讲“三次握手”的目的是“为了防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误”。在另一部经典的《计算机网络》（Andrew S.Tanenbaum著，第四版）一书中讲“三次握手”的目的是为了解决“网络中存在延迟的重复分组”的问题。这两种不同的表述其实阐明的是同一个问题。

[![cQP1i9.png](https://z3.ax1x.com/2021/04/05/cQP1i9.png)](https://imgtu.com/i/cQP1i9)

- 第一次握手：建立连接，客户端发送sync包（sync=x）到服务器，并进入SYN_SENT状态等待服务器确认;SYN：同步序列编号（Synchronize Sequence Numbers)   。。 首部的同步位SYN=1，初始序号seq=x，SYN=1的报文段不能携带数据，但要消耗掉一个序号。
- 第二次握手：服务器收到客户端的Syn包，必须确认客户的SYN(ack=x+1) 同时自己也发送一个SYN包(syn=y),即SYN+ACK包，此时服务器进入SYN_RECV状态； 在确认报文段中SYN=1，ACK=1，确认号ack=x+1，初始序号seq=y
- 第三次握手：客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=y+1)发送完毕后，客户端和服务端进入ESTABLISHED(TCP连接成功)状态，完成三次握手。 确认报文段ACK=1，确认号ack=y+1，序号seq=x+1（初始为seq=x，第二个报文段所以要+1），ACK报文段可以携带数据，不携带数据则不消耗序号。

#### TCP为何采用三次握手而不是采用两次握手呢

**现在这个有两种解释：第一种是网上流传的：**

3次握手完成两个重要的功能，既要双方做好发送数据的准备工作(双方都知道彼此已准备好)，也要允许双方就初始序列号进行协商，这个序列号在握手过程中被发送和确认。

​       现在把三次握手改成仅需要两次握手，死锁是可能发生的。作为例子，考虑计算机S和C之间的通信，假定C给S发送一个连接请求分组，S收到了这个分组，并发 送了确认应答分组。按照两次握手的协定，S认为连接已经成功地建立了，可以开始发送数据分组。可是，C在S的应答分组在传输中被丢失的情况下，将不知道S 是否已准备好，不知道S建立什么样的序列号，C甚至怀疑S是否收到自己的连接请求分组。在这种情况下，C认为连接还未建立成功，将忽略S发来的任何数据分 组，只等待连接确认应答分组。而S在发出的分组超时后，重复发送同样的分组。这样就形成了死锁

**第二种是谢希仁版的《计算机网络》中的例子：**

[![cQiMOf.png](https://z3.ax1x.com/2021/04/05/cQiMOf.png)](https://imgtu.com/i/cQiMOf)

**注：上图黄色部分说明了为什么不可以使用两次握手的问题。**

#### TCP为什么连接的时候是三次握手，关闭的时候却是四次握手？

先看下四次挥手的释放连接的过程。以下来源于谢希仁编著的《计算机网络》(第七版)

[![cQFu4J.png](https://z3.ax1x.com/2021/04/05/cQFu4J.png)](https://imgtu.com/i/cQFu4J)

[![cQFQ3R.png](https://z3.ax1x.com/2021/04/05/cQFQ3R.png)](https://imgtu.com/i/cQFQ3R)

**那么就来解答为何关闭时需要四次握手？**

因为当Server端收到Client端的SYN连接请求报文后，可以直接发送SYN+ACK报文。其中ACK报文是用来应答的，SYN报文是用来同步的。但是关闭连接时，当Server端收到FIN报文时，很可能并不会立即关闭SOCKET，所以只能先回复一个ACK报文，告诉Client端，"你发的FIN报文我收到了"。只有等到我Server端所有的报文都发送完了，我才能发送FIN报文，因此不能一起发送。故需要四步握手。

#### 如果建立了连接，但客户端突然出现故障怎么办？

TCP还设有一个保活计时器，客户端如果出现故障，服务器不能一直等下去，白白浪费资源。服务器每收到一次客户端的请求后都会重新复位这个计时器，时间通常是设置为2小时，若两小时还没有收到客户端的任何数据，服务器就会发送一个探测报文段，以后每隔75秒发送一次。若一连发送10个探测报文仍然没反应，服务器就认为客户端出了故障，接着就关闭连接。

### UDP

UDP是一个不提供不必要服务的轻量级运输协议，他仅提供最小服务，**UDP是无连接的**，因此在两个进程通信没有握手过程中。UDP协议提供一种不可靠数据传输服务，也就是说，当进程将一个报文发送进UDP套接字时，UDP协议并不保证该报文将到达进程，同时也不保证报文的顺序。**UDP使用尽最大努力交付**，**UDP是面向报文的。**

**UDP没有包含拥塞控制机制**，所以UDP的发送端是可以用它选定的任何速率向下层（网络层）注入数据，

**UDP支持一对一，一对多，多对一和多对多的交互通信**

**UDP的首部开销小，只有8字节，比TCP的20字节的首部要短**

[![cMokdS.png](https://z3.ax1x.com/2021/04/05/cMokdS.png)](https://imgtu.com/i/cMokdS)



### TCP与UDP区别

- TCP是面向连接的；UDP是无连接的，即发送数据之前不需要建立连接。
- TCP提供可靠的服务。通过TCP连接传输的数据，无差错，不丢失，不重复，且按序到达；UDP尽量保证到达，不保证可靠交互。
- TCP是面向字节流，实际tcp连接传送的数据可看成一连串的无结构的字节流；UDP是面向报文的，UDP没有阻塞控制，因此网络出现拥堵不会使得源主机发送效率降低（实时应用有用）
- 每条TCP连接只能点对点，UDP支持一对一，一对多，多对多的交互通信。
- TCP首部开销20字节；UDP的首部开销小，只有8个字节
- TCP的逻辑通信信道是双全工的可靠的信道，UDP则是不可靠信道。

## Request与Response

在前面Http请求的文章中我们看到一个http请求包含请求头，请求体，响应头和响应体，那么对于这些请求方面和响应方面我们与浏览器交互标准是有了，我们后端是不是需要一个关于请求和响应的API,我们为了方便描述一个http请求出现了Request和Response概念。

[![cdkc11.png](https://z3.ax1x.com/2021/04/10/cdkc11.png)](https://imgtu.com/i/cdkc11)

### Request

request这个对象不用事先声明，就可以在JSP网页中使用，在编译为Servlet之后，它会转换为javax.servlet.http.HttpServletRequest形态的对象，HttpServletRequest对象是有关于客户端所发出的请求的对象，只要是有关于客户端请求的信息，都可以由它来取得，例如请求标头、请求方法、请求参数、客户端IP，客户端浏览器等等信息

**ServletRequest -- 通用request，提供一个request应该具有的最基本的方法.HttpSerletRequest是Rquest的子类针对http协议进行了进一步的增强**

#### Request的操作

##### 获取客户机信息

  getRequestURL()   方法返回客户端发出请求完整URL
  getRequestURI()    方法返回请求行中的资源名部分
  getQueryString()   方法返回请求行中的参数部分
  getRemoteAddr()  方法返回发出请求的客户机的IP地址
  getMethod()     得到客户机请求方式
  getContextPath()   获得当前web应用虚拟目录名称

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		//1.获取客户端请求的完整的url
		String url=request.getRequestURL().toString();
		System.out.println(url);
		//2.获取客户端请求的资源的部分名称 
		String uri=request.getRequestURI();
		System.out.println(uri);
		//3.获取请求行中的参数部分
		String pram=request.getQueryString();
		System.out.println(pram);
		//4.返回客户端的ip地址(*)
		String ip=request.getRemoteAddr();
		System.out.println(ip);
		//5.获取客户机的请求方式
		String method=request.getMethod();
		System.out.println(method);
		//6.获取当前web的应用的名称
		String name=request.getContextPath();
		System.out.println(name);
		
		//请求转发时以后有这个方法
		response.sendRedirect(request.getContextPath()+"/index.jsp");
	}
```

##### 获取请求头信息

获得客户机请求头
      getHeader(name)方法 --- String 
      getHeaders(String name)方法 --- Enumeration<String>枚举变量<string类型的>
      getHeaderNames方法 --- Enumeration<String>
获得具体类型客户机请求头
       getIntHeader(name)方法  --- int
       getDateHeader(name)方法 --- long(日期对应毫秒)

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		//获取客户机的请求头
		//String value=request.getHeader("Host");
		//System.out.println(value);
		//遍历所有的请求头
		Enumeration<String> enument=request.getHeaderNames();
		while(enument.hasMoreElements()){
			String name=enument.nextElement();
			String values=request.getHeader(name);
			System.out.println(name+":"+values);
		}
	}
```

##### 获取请求参数

request.getParameter()

​       浏览器以什么编码来发送请求参数? 浏览器以什么编码打开的表单页面,就用什么编码发送这个页面提交的数据。服务器以什么编码来打开呢?如果不指定,则使用ISO8859-1,这样如果请求参数中有中文必然就乱码了
​      对于POST提交,可以设置request.setCharacterEncoding("utf-8");明确的通知服务器以浏览器发送过来的编码来打开数据就可以解决乱码但是上面的方法只对请求中实体内容部分起作用,所以GET提交的乱码并不能解决. 对于GET提交的乱码,只能手动的进行编解码从而解决乱码问题:
​     ``String username = request.getParameter("username");``
​	 `username = new String(username.getBytes("iso8859-1"),"utf-8");`

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		/*
		 * post提交乱码解决
		 */
		//此处也要进行对服务器编码进行设置(通知服务器以什么编码解码http请求中的实体内容)
		request.setCharacterEncoding("utf-8");
		//获取请求参数的值但是传中文字符是会转义到其他的
		String name=request.getParameter("username");
		//System.out.println(name);
		/*
		 * get提交乱码解决方式(同样也适合post提交方式)
		 * 现对提交的参数按照iso8859-1进行编码,然后在解码到其他码表转回
		 * 
		 */
			String username=new String(name.getBytes("iso8859-1"),"utf-8");
		/////
		//获取到用一个枚举变量的类型；
		Enumeration<String> enumeration=request.getParameterNames();
		while(enumeration.hasMoreElements()){
			String names=enumeration.nextElement();
			String values=request.getParameter(names);
			System.out.println(names+":"+values);
		}
		
		
	}
```

##### 利用请求域传递对象

  	  作用范围: 整个请求链上
        生命周期: 当服务器收到一个请求,创建出代表请求的request对象,request开始.当请求结束,服务器销毁代表请求的request对象,request域结束.
        作用: 在整个请求链范围内共享数据,通常我们在Servlet中处理好的数据会存入request域后请求转发到jsp页面来进行展示

```java
 protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		//request作用域 全局域    getRequestDispater() 返回一个作为位于给定路径的资源资源的封装器的 RequestDispatcher 对象。
		request.setAttribute("banana", "color:yellow");
		this.getServletContext().getRequestDispatcher("/Demo2").forward(request, response);
	
		//转发到xxxjsp中
	//先要获取其中的数据ru
		String result="xxxx";
		request.setAttribute("xxx", result);
		request.getRequestDispatcher("xxx.jsp");
	}
```
##### 实现请求转发和请求包含

  **(1)请求转发(.forward()):**
            this.getServletContext().getRequestDispatcher("").forward(request,response);
            request.getRequestDispatcher("").forward(request,response); 

    请求转发是希望将请求交给另外一个资源执行,所以应该保证只有最后真正要执行的资源才能够输出数据,所以:
    请求转发时,如果已经有数据被写入到了response的缓冲区,但是这些数据还没有被发送到客户端,则请求转发时,这些数据将会被清空.但是清空的只是响应中的实体内容部分,头信息并不会被清空.
    而请求转发时已经有数据被打给了浏览器,那么再进行请求转发,不能成功,会抛出异常,原因是响应已经结束了,再转发交给其他人没意义了
    在最终输出数据的Servlet执行完成后,response实体内容中的数据将会被设置为已提交的状态,再往里写数据也不会起作用
   **(2)请求包含(.include()):将两个资源的输出进行合并后输出多个资源同是输出**
            this.getServletContext().getRequestDispatcher("").include(request,response);
            request.getRequestDispatcher("").include(request,response);

    被包含的Servlet程序不能改变响应消息的状态码和响应头，如果它里面存在这样的语句，这些语句的执行结果将被忽略常被用来进行页面布局
  **(3)三种资源处理方式的区别**
            请求重定向
                response.sendRedirect();
            请求转发
                request.getRequestDispatcher().forward();
            请求包含
                request.getRequestDispatcher().include();

**请求重定向和请求转发的区别:**

    请求重定向地址栏会发生变化.请求转发地址栏不发生变化.
    请求重定向两次请求两次响应.请求转发一次请求一次响应.
                
    如果需要在资源跳转时利用request域传递域属性则必须使用请求转发 request.getRequestDispatcher().forward();
    如果希望资源跳转后修改用户的地址栏则使用请求重定向response.sendRedirect();
    如果使用请求转发也可以重定向也可以,则优先使用请求转发,减少浏览器对服务器的访问次数减轻服务器的压力.
### Response

response是Servlet.service方法的一个参数，类型为javax.servlet.http.HttpServletResponse。在客户端发出每个请求时，服务器都会创建一个response对象，并传入给Servlet.service()方法。response对象是用来对客户端进行响应的，这说明在service()方法中使用response对象可以完成对客户端的响应工作。

#### Response操作

##### 设置编码方式

​		response.setHeader("Content-Type", "text/html;charset=utf-8");
​		response.setCharacterEncoding("utf-8");
​		response.getWriter().write("中国");

```java
public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		//这里是一个编码过程,用的是操作系统的编码GBK;
		//浏览器打开时也是GBK的打开方式所以没有乱码
		//response.getOutputStream().write("English very so easy".getBytes());
		//这是会出现乱码,需要让浏览器也使用Utf-8编码打开才不会乱码或则用下面的方法
		//response.setHeader("Content-Type", "text/html;charset=utf-8");
		//response.getOutputStream().write("中国".getBytes("utf-8"));
		/*这时用中文又会乱码这时是只能是服务器把汉字转换为010101然后去查iso8859-1码表
		这个码表中没有中文,如果在iso8859-1找不到的话会被转换为?,然而浏览器又会用GBK打开这个编码所以会显示??
		这时要指定服务器查的码表
		*/
		//指定服务器查的码表
		response.setCharacterEncoding("gbk");
		response.getWriter().write("beijiang");
		response.getWriter().write("中国");
		//或者这样;
		response.setHeader("Content-Type", "text/html;charset=utf-8");
		response.setCharacterEncoding("utf-8");
		response.getWriter().write("中国");
		//或者这样;setContentType可以直接指定浏览器和服务器的编码方式
		response.setContentType("text/html,charset=utf-8");
		response.getWriter().write("中国");
		//或者这样SetCharacterEnconding指定服务器的编码
		//setContentType指定浏览器的编码
		response.setCharacterEncoding("utf-8");
		response.setContentType("text/html,charset=utf-8");
		response.getWriter().write("中国");
	}
```

##### 设置是否缓存(缓存时间)

​		不进行缓存的设置形式
​		response.setIntHeader("Expires", -1);
​		response.setHeader("Cache-control","no-cache");
​		response.setHeader("Pragma","no-cache");
​		设置缓冲并设置缓存的时间
​		response.setDateHeader("Expires", System.currentTimeMillis()+1000L*3600*24*30);

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//设置缓冲的时间
		response.setDateHeader("Expires", System.currentTimeMillis()+1000L*3600*24*30);
		//这样只是读取到这样的文件但是并没有实现下载功能
		InputStream in=new  FileInputStream(this.getServletContext().getRealPath("1.jpg"));
		OutputStream out=response.getOutputStream();
		byte[]bs=new byte[1024];
		int i=0;
		i=in.read(bs);
		while(i!=-1){
			out.write(bs,0,i);
			i=in.read(bs);
		}
		in.close();
		///下载的形式应该用这个方式
	
	}

public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		//设置响应头信息在浏览器中不进行缓存
		response.setIntHeader("Expires", -1);
		response.setHeader("Cache-control","no-cache");
		response.setHeader("Pragma","no-cache");
		//同时设置服务器和浏览器的编码方式
		response.setContentType("text/html;charset=utf-8");
		response.getWriter().write("当前时间是:"+new Date().toLocaleString());
	}
```

##### 设置资源下载

​		文件名中包含中文，则文件名要进行URL编码，URLEncoding.encode('啊啊.jpg','utf-8');如果不进行编码则文件名显示错误并且不可下载

​		///下载的形式应该用这个方式  翻译:Disposition:配置

​		`response.setHeader("Content-Disposition", "attachment;filename=1.jpg");`

​		//这样只是读取到这样的文件但是并没有实现下载功能
​		`InputStream in=new  FileInputStream(this.getServletContext().getRealPath("1.jpg"));
`	         

​    	`OutputStream out=response.getOutputStream();`

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		///下载的形式应该用这个方式(Y)
		//setHeader头信息不支持中文格式所以命名不能用中文命名这样可以指定下载是所显示的名字
		//response.setHeader("Content-Disposition", "attachment;filename=美女.jpg");
		//方式一:狭隘
		//response.setHeader("Content-Disposition", "attachment;filename=butiful.jpg");
		//解决方式二:url编码可以用ascII码中的转换为url编码然后再转换为指定的编码
		response.setHeader("Content-Disposition", "attachment;filename="+URLEncoder.encode("美女.jpg","utf-8"));
		//这样只是读取到这样的文件但是并没有实现下载功能(N)
		InputStream in=new  FileInputStream(this.getServletContext().getRealPath("1.jpg"));
		OutputStream out=response.getOutputStream();
		byte[]bs=new byte[1024];
		int i=0;
		i=in.read(bs);
		while(i!=-1){
			out.write(bs,0,i);
			i=in.read(bs);
		}
		in.close();
	}
```

##### 请求从定向

​	response.sendRedirect("/Test/index.jsp");

##### 设置刷新跳转

​	`response.setHeader("refresh", "3;url=/Test/index.jsp");`
​	转发`//request.getRequestDispatcher("/index.jsp").forward(request, response);`
​	包含`/request.getRequestDispatcher("/index.jsp").include(request, response);`
​	重定向`response.sendRedirect("/Test/index.jsp");`

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		//隔几秒刷新页面
		//response.getWriter().write(new Date().toString());
		//response.setHeader("Refresh", "1");
		//隔几秒会到主页
		//response.setCharacterEncoding("utf-8");
		//response.setHeader("Content-Type", "text/html;charset=utf-8");
		response.setContentType("text/html;charset=utf-8");
		response.getWriter().write("恭喜你注册成功 3秒后跳转页面....");
		response.setHeader("refresh", "3;url=/Test/index.jsp");
		//但是一般不会这样写会把写出的话放到html页面中
		//向newFilehtml那样进行操作可以在html中用<meta http-equiv="" content="">来模拟响应头信息
	}
```



## Session与Cookie

浏览器开始访问网站到访问网站结束期间产生的多次请求响应组合在一起叫做一次会话，会话的过程中会产生会话相关的数据，我们需要将这些数据保存起来。

### Cookie

是客户端的技术，程序把每个用户的数据以cookie的形式写给用户的各自的浏览器，当用户使用浏览器再去访问服务器中的web资源时，这样，web资源处理的就是用户各自的数据了。
Cookie是基于set-Cookie响应头和Cookie请求头工作的,服务器可以发送set-Cookie请求头命令浏览器保存一个cookie信息,浏览器会在访问服务器时以Cookie请求头的方式带回之前保存的信息cookie在浏览器中的存放只允许存300个cookie，每个站点最多有20个cookie在浏览器的存放cookie是不安全的，很有很能被丢失；
**删除cookie必须设置maxAge path 一致性才可以覆盖**

**cookie是客户端技术**

- 数据保存在客户端,这个信息可以保存很长时间
- 数据随时有可能被清空,所以cookie保存的数据是不太靠谱的
- 数据被保存在了客户端,随时有可能被人看走,如果将一些敏感信息比如用户名密码等信息存在cookie中,可能有安全问题

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.setContentType("text/html;charset=utf-8");
		
		//创建cookie数组
		Cookie[] cookies=request.getCookies();
		Cookie findC=null;
		if(cookies!=null){
		for(Cookie c :cookies){
			if("lasttime".equals(c.getName())){
				findC=c;
			}
		}
		}
		if(findC==null){
			response.getWriter().write("你是第一次访问这个网站");
		}else{
			//cookies是返回一个long行的值
			Long lastTime=Long.parseLong(findC.getValue());
			response.getWriter().write("你上次访问的时间是："+new Date(lastTime).toLocaleString());
		}
		
		Date date=new Date();
		Cookie cookie=new Cookie("lasttime", date.getTime()+"");
		//设置cookies保存的最多时间
		//相当于response中添加了一个Set-cookie的响应头
		cookie.setMaxAge(36000);
		//设置整个web应用的cookie信息都可以带过去；
		cookie.setPath(request.getContextPath());
		response.addCookie(cookie);
	}
```

**setMaxAge与getMaxAge方法** 

-  一个Cookie如果没有设置过MaxAge则这个Cookie是一个会话级别的Cookie,这个Cookie信息打给浏览器后浏览器会将它保存在浏览器的内存中,这意味着只要浏览器已关闭随着浏览器内存的销毁Cookie信息也就消失了.
-  一个Cookie也可以设置MaxAge,浏览一一旦发现收到的Cookie被设置了MaxAge,则会将这个Cookie信息以文件的形式保存在浏览器的临时文件夹中,保存到指定的时间到来为止.这样一来即使多次开关浏览器,由于这些浏览器都能在临时文件夹中看到cookie文件,所以在cookie失效之前cookie信息都存在.
- 想要命令浏览器删除一个Cookie,发送一个同名同path的cookie,maxage设置为0,浏览器以名字+path识别cookie,发现同名同path,cookie覆盖后立即超时被删除,从而就删除了cookie.就是一个覆盖.

**setPath与getPath方法**
     用来通知浏览器在访问服务器中的哪个路径及其子路径时带着当前cookie信息过来如果不明确设置,则默认的路径是发送Cookie的Servlet所在的路径.

**setDomain与getDomain方法**
       用来通知浏览器在访问哪个域名的时候带着当前的cookie信息.但是要注意,现代的浏览器一旦发现cookie设置过domain信息则会拒绝接受这个Cookie.我们平常不要设置这个方法.

Cookie是不可跨域名的。域名www.google.com颁发的Cookie不会被提交到域名www.baidu.com去。这是由Cookie的隐私安全机制决定的。隐私安全机制能够禁止网站非法获取其他网站的Cookie。

正常情况下，同一个一级域名下的两个二级域名如www.baidu.com和www.images.baidu.com也不能交互使用Cookie，因为二者的域名并不严格相同。如果想所有www.baidu.com名下的二级域名都可以使用该Cookie，需要设置Cookie的domain参数

```java
Cookie cookie = new Cookie("time","20080808"); // 新建Cookie
cookie.setDomain("www.baidu.com"); // 设置域名
cookie.setPath("/"); // 设置路径
cookie.setMaxAge(Integer.MAX_VALUE); // 设置有效期
response.addCookie(cookie);              
```

```java
/**
* 显示之前看的书从cookie中获取信息
**/
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.setContentType("text/html;charset=utf-8");
		//查询数据库中的书的展示：
		Map<String, Book> map=BookDao.getbooks();
		for (Map.Entry<String, Book>entry :map.entrySet()) {
			Book book =entry.getValue();
			response.getWriter().write("<a href='"+request.getContextPath()+"/BookInfoServlet?id="+book.getId()+"'>"+book.getName()+"</a><br>");
		}
		response.getWriter().write("<hr>");
		//2`显示之前看过的书
		Cookie[] cookies=request.getCookies();
		Cookie findC=null;
		if(cookies!=null){
            for(Cookie c :cookies){
                if("last".equals(c.getName())){
                    findC=c;
                }
            }
		}
		if(findC==null){
			response.getWriter().write("你未浏览过");
		}else{
			response.getWriter().write("你浏览过书有："+"<br/>");
			//cookies是返回一个long行的值
			String[] ids=findC.getValue().split(",");
			for(String id :ids){
				Book book=BookDao.getbook(id);
				//response.getWriter().write(book.getName()+"<br/>");
			}
		}
	
	}
```

[![cabF3j.png](https://z3.ax1x.com/2021/04/10/cabF3j.png)](https://imgtu.com/i/cabF3j)

### Session

由于HTTP协议是无状态的协议，所以服务端需要记录用户的状态时，就需要用某种机制来识具体的用户，这个机制就是Session.典型的场景比如购物车，当你点击下单按钮时，由于HTTP协议无状态，所以并不知道是哪个用户操作的，所以服务端要为特定的用户创建了特定的Session，用用于标识这个用户，并且跟踪用户，这样才知道购物车里面有几本书。这个Session是保存在服务端的，有一个唯一标识。

session是服务器端技术，数据保存在服务区端,相对来说比较稳定和安全，占用服务器内存,所以一般存活的时间不会太长,超过超时时间就会被销毁.我们要根据服务器的压力和session 的使用情况合理设置session的超时时间,既能保证session的存活时间够用,同时不用的session可以及时销毁减少对服务器内存的占用.

#### **作用范围:**

当前会话范围

 #### **生命周期:**            

当程序第一次调用到request.getSession()方法时说明客户端明确的需要用到session此时创建出对应客户端的Session对象.

当session超过30分钟(这个时间是可以在web.xml文件中进行修改的)没有人使用则认为session超时销毁这个session.

程序中明确的调用session.invalidate()方法可以立即杀死session.

当服务器被非正常关闭时,随着虚拟机的死亡而死亡.如果服务器是正常关闭,还未超时的session会被以文件的形式保存在服务器的work目录下,这个过程叫做session的钝化.下次再正常启动服务器时,钝化着的session会被恢复到内存中,这个过程叫做session的活化.

 **作用**:在会话范围内共享数据

  session时间的配置：在配置的时是以分钟为单位的；
​在web.xml中用配置`<session-config><session-timeout>30</></>`

#### **session 的原理:**

request.getSession()方法会检查请求中有没有JSESSIONID 如果没有则检查请求的URL后有没有以参数的形式带着JSESSIONID过来,如果有则找到对应的Session， 服务器如果找不到则认为这个浏览器没有对应的Session,创建一个Session然后再在响应中添加JSESSIONID

cookie的值就是这个Session 的id

默认情况下,JSESSIONID 的path为当前web应用的名称,并且没有设置过MaxAge,是一个会话级别的cookie.

这意味着一旦关闭浏览器再新开浏览器时,由于JSESSIONID丢失,会找不到之前的Session我们可以手动的发送JSESSIONID cookie,名字和path设置的和自动发送时一样,但是设置一下MaxAge,使浏览器除了在内存中保存JSESSIONID信息以外还在临时文件夹中以文件的形式保存,这样即使重开浏览器仍然可以使用之前的session

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		String prod=request.getParameter("prod");
		prod=new String(prod.getBytes("iso8859-1"),"UTF-8");
		
		HttpSession session=request.getSession();
		Cookie jc=new Cookie("JSESSIONID", session.getId());
		jc.setPath(request.getContextPath());
		jc.setMaxAge(1800);
		response.addCookie(jc);
		session.setAttribute("prod", prod);
	
	}
```

```java
/**
*
*登录后将用户信息存到session中
*/
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		//1.获取用户名密码
		String username = request.getParameter("username");
		String password = request.getParameter("password");
		//2.查询数据库检查用户名密码
		if(UserDao.valiNamePsw(username, password)){
			//3.如果正确登录后重定向到主页
			request.getSession().setAttribute("user", username);
			response.sendRedirect(request.getContextPath()+"/loginout/index.jsp");
			return;
		}else{
			//4.如果错误提示
			response.getWriter().write("用户名密码不正确!");
		}	
	}
/**
* 退出时把session杀死
*/
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		//1.杀死session
		//request.getsession();如果没有session会创建一个
    if(request.getSession(false)!=null 
       && request.getSession().getAttribute("user")!=null){
        request.getSession().invalidate();
    }
    //2.重定向到主页
    response.sendRedirect(request.getContextPath()+"/loginout/index.jsp");
}

/**
* 防止form表单重复提交
*/
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		request.setCharacterEncoding("utf-8");
		try {
			Thread.sleep(4*1000);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		String username = request.getParameter("username");
		String valinum = request.getParameter("valinum");
		String valinum2 = (String) request.getSession().getAttribute("valinum");
		if(valinum2!=null && !"".equals(valinum2) && valinum.equals(valinum2)){
			request.getSession().removeAttribute("valinum");
			System.out.println("向数据库中注册一次:"+username);
		}else{
			response.getWriter().write("from web:不要重复提交!!");
		}	
}
```

### URL重写:

 如果浏览器禁用了Cookie,浏览器就没有办法JSESSIONID cookie,这样就用不了Session了.我们可以使用URL重写的机制,在所有的超链接后都以参数的形式拼接JSESSIONID信息,从而在点击超链接时可以使用URL参数的方式待会JSESSIONID,从而使用Session将URL进行重写拼接上JSESSIONID的过程就叫做URL重写

```
request.getSession() --在URL重写之前一定要先创建出Session,才有Session id,才能进行重写
response.encodeURL()--- 一般的地址都用这个方法重写
response.encodeRedirectURL() --- 如果地址是用来进行重定向的则使用这个方法
```

url重写的方法一旦发现浏览器带回了任意cookie信息,则认为客户端没有禁用cookie,就不会再进行重写操作


## Servlet

Java Servlet 是运行在 Web 服务器或应用服务器上的程序，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层。

使用 Servlet，您可以收集来自网页表单的用户输入，呈现来自数据库或者其他源的记录，还可以动态创建网页。

Java Servlet 通常情况下与使用 CGI（Common Gateway Interface，公共网关接口）实现的程序可以达到异曲同工的效果。但是相比于 CGI，Servlet 有以下几点优势：

- 性能明显更好。
- Servlet 在 Web 服务器的地址空间内执行。这样它就没有必要再创建一个单独的进程来处理每个客户端请求。
- Servlet 是独立于平台的，因为它们是用 Java 编写的。
- 服务器上的 Java 安全管理器执行了一系列限制，以保护服务器计算机上的资源。因此，Servlet 是可信的。
- Java 类库的全部功能对 Servlet 来说都是可用的。它可以通过 sockets 和 RMI 机制与 applets、数据库或其他软件进行交互。

### Servlet生命周期

Servlet 生命周期可被定义为从创建直到毁灭的整个过程。以下是 Servlet 遵循的过程：

- Servlet 初始化后调用 **init ()** 方法。
- Servlet 调用 **service()** 方法来处理客户端的请求。
- Servlet 销毁前调用 **destroy()** 方法。
- 最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的。

现在让我们详细讨论生命周期的方法。

### Servlet调用过程

![Servlet的调用过程图](E:\Java\learning-note\note_old_jsp_servlert\Servlet的调用过程图.gif)

### Servlet过滤器与监听器

#### 过滤器

Servlet 过滤器可以动态地拦截请求和响应，以变换或使用包含在请求或响应中的信息。

可以将一个或多个 Servlet 过滤器附加到一个 Servlet 或一组 Servlet。Servlet 过滤器也可以附加到 JavaServer Pages (JSP) 文件和 HTML 页面。调用 Servlet 前调用所有附加的 Servlet 过滤器。

Servlet 过滤器是可用于 Servlet 编程的 Java 类，可以实现以下目的：

- 在客户端的请求访问后端资源之前，拦截这些请求。
- 在服务器的响应发送回客户端之前，处理这些响应。

例如：统一字符编码，字符的压缩，加密，实施安全控制等；

与过滤器有关的有三个包：Filter FilterChain和FilterConfig;




	Filter:所有过滤器都必须实现这个接口；
			生命周期：web应用加载后立即创建这个web应用的所有过滤器，创建后是驻留在内存中init();过滤器初始化，容器会创建实例后调用这个方法
	FilterConfig：代表web.xml中对filter的配置信息
			获取servletContext对象
			获取初始信息	
	FilterChain:doFilter();用于调用过滤器链中的下一个过滤器，如果是最后一个则将请求提交给处理程序或响应到客户端上；filterChain代表一个连对象，一个资源可以用多个过滤器进行拦截，拦截顺序和filtermapping的顺序决定链的最后一各节点就是访问的资源；
	FilterConfig:用于过滤器初始化阶段提供过滤器的名字，初始化参数，servlet上下文的信息；
		String getFilterName();返回web.xml文件定义的名称
		ServletContext getServletContext()方法,返回调用者所处的Servlet的上下文
		String getInitParameter(String name):返回配置过滤器名是name的初始值；‘
		Enumeration getgetInitParameterNames()以Enumeration形式返回过滤器所有初始化参数的名称
		
	出现servlet3.0后在eclipes中就不需要配置web.xml了
	如何进行创建出filter中的参数，写在web.xml中是不能实现的：
	范式：创建filter过滤器，然后在其中的参数列表中选择是否创建参数，然后在改下对应的url-parttern参数
	让他对应你的jsp文件就可以解决这个问题；
	@WebFilter(
		urlPatterns = { "/jsp/index.jsp" }, 
		initParams = { 
				@WebInitParam(name = "count", value = "5000")
		})
		用filterConfig来获取属性的值是多少
		filterConfig.getInitParameter(String name);
	@WebFilter(asyncSupported = true, description = "filterdemo", urlPatterns = { "/*" })
	
	在myeclipse中就必须在web.xml逐一配置出来
	有：
		<filter>
			<filter-name>Filter1</filter-name>
	  		<filter-class>cn.itcast.filter.Filter1</filter-class>
		</filter>	
		<!-- 配置过滤器去拦截哪个资源 -->
		<filter-mapping>
	  		<filter-name>Filter1</filter-name>
	  		<url-pattern>/hello.jsp</url-pattern>
			<dispatcher>REQUEST</dispatcher>--用来配置以哪种方式对资源的访问(request/forward/include/error)
		可以配置多个dispatcher如果不配置默认为request请求
		</filter-mapping>	
```java
@WebFilter(asyncSupported = true, description = "filterdemo", urlPatterns = { "/demo1Filr" })
public class Demo1Filter implements Filter {

    /**
     * Default constructor. 
     */
    public Demo1Filter() {
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see Filter#destroy()
	 */
	public void destroy() {
		// TODO Auto-generated method stub
	System.out.println("filter销毁了");
	}

	/**
	 * @see Filter#doFilter(ServletRequest, ServletResponse, FilterChain)
	 */
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		// TODO Auto-generated method stub
		// place your code here
		System.out.println("demo1前");
		// pass the request along the filter chain
		//意思是执行下一个节点可以为过滤器可以为资源
		chain.doFilter(request, response);
		System.out.println("demo1后");
	}

	/**
	 * @see Filter#init(FilterConfig)
	 */
	public void init(FilterConfig fConfig) throws ServletException {
		// TODO Auto-generated method stub
	System.out.println("filter创建了");
	}

}
```

### Servlert经典实例

#### 文件上传

```java
package com.itheima.upload;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.lang.reflect.InvocationTargetException;
import java.sql.SQLException;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.UUID;

import javax.servlet.ServletException;
import javax.servlet.annotation.MultipartConfig;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.commons.beanutils.BeanUtils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.fileupload.FileItem;
import org.apache.commons.fileupload.FileUploadException;
import org.apache.commons.fileupload.disk.DiskFileItemFactory;
import org.apache.commons.fileupload.servlet.ServletFileUpload;

import com.itheima.domain.Resource;
import com.itheima.util.DaoUtils;
import com.itheima.util.IOUtils;

/**
 * Servlet implementation class UploaddisckServlet
 */
@WebServlet("/UploaddisckServlet")
@MultipartConfig
public class UploaddisckServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public UploaddisckServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		//1.上传文件
		String upload=this.getServletContext().getRealPath("WEB-INF/upload");
		String temp=this.getServletContext().getRealPath("WEB_INF/temp");
		//创建工厂设置缓冲大小和穿就缓冲区路径
		DiskFileItemFactory factory=new DiskFileItemFactory();
		factory.setSizeThreshold(1024*100);
		factory.setRepository(new File(temp));
		//2.生产文件上传核心类
		ServletFileUpload fileUpload=new ServletFileUpload(factory);
		//设置编码
		fileUpload.setHeaderEncoding("UTF-8");
		//设置文件大小的上传限制
		fileUpload.setFileSizeMax(1024*1024*10);
		fileUpload.setSizeMax(1024*1024*100);
		//检查当前是项目是否为上传文件
		//if (fileUpload.isMultipartContent(request)) {
			//throw new RuntimeException("请用正确的表单上传");
		//}
		//解析request
		//3.利用文件上传核心类来解析request
		try {
			List<FileItem> list=fileUpload.parseRequest(request);
			Map<String,String> map=new HashMap<>();
			//循环遍历
			for(FileItem item :list){
				if (item.isFormField()) {
					//普通的字段获得的是一个表单
					String name=item.getFieldName();
					String value=item.getString("utf-8");
					map.put(name, value);
					System.out.println(name+":"+value);
				}else{
					//当前一个文件上传项
					String filename=item.getName();//文件名
					//设置一个独一无二的文件名
					String uuidfilename=UUID.randomUUID().toString()+"_"+filename;
					map.put("realname", filename);
					map.put("uuidname",uuidfilename);
					map.put("ip", request.getRemoteAddr());
					String savepath="/WEB-INF/upload";
					//转换为hash值
					int hash=uuidfilename.hashCode();
					//转化为hash字符串
					String hashstr=Integer.toHexString(hash);
					char[] hss=hashstr.toCharArray();
					
					for(char c:hss){
						upload+="/"+c;
						savepath+="/"+c;
					}
					new File(upload).mkdirs();
					map.put("savepath", savepath);
					InputStream inputStream=item.getInputStream();
					
					OutputStream outputStream=new FileOutputStream(new File(upload,uuidfilename)); 
					IOUtils.In2Out(inputStream, outputStream);
					IOUtils.close(inputStream, outputStream);
					//删除临时文件
					item.delete();
				}
			}
			//像数据库中插入
			Resource resource=new Resource();
			BeanUtils.populate(resource, map);
			String sql="insert into netdisk values(null,?,?,?,?,null,?)";
			QueryRunner runner=new QueryRunner(DaoUtils.getSource());
			runner.update(sql,resource.getUuidname(),resource.getRealname(),resource.getSavepath(),resource.getIp(),resource.getDescription() );
			//3.重定向回主页
			response.sendRedirect(request.getContextPath()+"/disk/index.jsp");
		} catch (FileUploadException | IllegalAccessException | InvocationTargetException | SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	
		
	
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}

```

版本二：

```java
package com.itheima.upload;

import java.io.File;
import java.io.FileOutputStream;
import java.io.FilePermission;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.math.BigDecimal;
import java.util.List;
import java.util.UUID;

import javax.servlet.ServletException;
import javax.servlet.annotation.MultipartConfig;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.commons.fileupload.FileItem;
import org.apache.commons.fileupload.FileUploadException;
import org.apache.commons.fileupload.ProgressListener;
import org.apache.commons.fileupload.disk.DiskFileItemFactory;
import org.apache.commons.fileupload.servlet.ServletFileUpload;

import com.itheima.util.IOUtils;

import sun.nio.ch.IOUtil;



/**
 * Servlet implementation class UploadServlet
 */
@WebServlet(description = "文件上传", urlPatterns = { "/UploadServlet" })
public class UploadServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpSep=rvlet()
     */
    public UploadServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		//通过工厂类来实现
		DiskFileItemFactory factory=new DiskFileItemFactory();
		//设置缓冲区大小
		factory.setSizeThreshold(100*1024);
		//设置临时文件夹
		factory.setRepository(new File(this.getServletContext().getRealPath("WEB-INF/temp")));
		//2.生产文件上传核心类
		ServletFileUpload fileUpload=new ServletFileUpload(factory);
		try {
		//判断是否为真正的表单上传文件
		//if (fileUpload.isMultipartContent(request)) {
			//throw new RuntimeException("请用正确的表单上传");
		//}
		//设置文件大小的上传限制
		fileUpload.setFileSizeMax(1024*1024*10);
		fileUpload.setSizeMax(1024*1024*100);
		//设置编码
		fileUpload.setHeaderEncoding("UTF-8");
		//设置上传监听进度条：
		fileUpload.setProgressListener(new ProgressListener() {
			//
			Long beginTime = System.currentTimeMillis();
			//已经读了，总共多少 ，读到第几个了
			@Override
			public void update(long pBytesRead, long pContentLength, int pItems) {
				// TODO Auto-generated method stub
				//转换为kb
				//double br=pBytesRead*1.0/1024;
				//double cl=pContentLength*1.0/1024;
				//为了保留字节用以下方法
				BigDecimal br = new BigDecimal(pBytesRead).divide(new BigDecimal(1024),2,BigDecimal.ROUND_HALF_UP);
				BigDecimal cl = new BigDecimal(pContentLength).divide(new BigDecimal(1024),2,BigDecimal.ROUND_HALF_UP);
				System.out.print("当前读取的是第"+pItems+"个上传项,总大小"+cl+"KB,已经读取"+br+"KB");
				//剩余字节数
				BigDecimal ll = cl.subtract(br);
				System.out.print("剩余"+ll+"KB");
				//上传百分比
				BigDecimal per = br.multiply(new BigDecimal(100)).divide(cl,2,BigDecimal.ROUND_HALF_UP);
				System.out.print("已经完成"+per+"%");
				//上传用时
				Long nowTime = System.currentTimeMillis();
				Long useTime = (nowTime - beginTime)/1000;
				System.out.print("已经用时"+useTime+"秒");
				//上传速度
				BigDecimal speed = new BigDecimal(0);
				if(useTime!=0){
					//四舍五入模式向“最近邻居”转弯，除非两个邻居都是等距的，在这种情况下是圆括弧的。
					speed = br.divide(new BigDecimal(useTime),2,BigDecimal.ROUND_HALF_UP);
				}
				System.out.print("上传速度为"+speed+"KB/S");
				//大致剩余时间
				BigDecimal ltime = new BigDecimal(0);
				if(!speed.equals(new BigDecimal(0))){
					//返回一个 BigDecimal ，其值为 (this / divisor) ，其比例为指定。
					ltime = ll.divide(speed,0,BigDecimal.ROUND_HALF_UP);
				}
				System.out.print("大致剩余时间为"+ltime+"秒");
				
				System.out.println();
				}
		});
		
		//3.利用文件上传核心类来解析request
			List<FileItem> list=fileUpload.parseRequest(request);
			//循环遍历
			for(FileItem item :list){
				if (item.isFormField()) {
					//普通的字段获得的是一个表单？？
					String name=item.getFieldName();
					String value=item.getString("utf-8");
					System.out.println(name+":"+value);
				}else{
					//当前一个文件上传项
					String filename=item.getName();//文件名
					//设置一个独一无二的文件名
					String uuidfilename=UUID.randomUUID().toString()+"_"+filename;
					//转换为hash值
					int hash=uuidfilename.hashCode();
					//转化为hash字符串
					String hashstr=Integer.toHexString(hash);
					char[] hss=hashstr.toCharArray();
					String path=this.getServletContext().getRealPath("upload");
					for(char c:hss){
						path+="/"+c;
					}
					new File(path).mkdirs();
					InputStream inputStream=item.getInputStream();
					
					OutputStream outputStream=new FileOutputStream(new File(path,uuidfilename)); 
					IOUtils.In2Out(inputStream, outputStream);
					IOUtils.close(inputStream, outputStream);
					//删除临时文件
					item.delete();
				}
			}
		} catch (FileUploadException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
			throw new RuntimeException();
		}
		
	
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}


```

util

```java
public class IOUtils {
	private IOUtils() {
	}
	
	public static void In2Out(InputStream in,OutputStream out) throws IOException{
		byte [] bs = new byte[1024];
		int i = 0;
		while((i=in.read(bs))!=-1){
			out.write(bs,0,i);
		}
	}
	
	public static void close(InputStream in,OutputStream out){
		if(in!=null){
			try {
				in.close();
			} catch (IOException e) {
				e.printStackTrace();
			}finally{
				in = null;
			}
		}
		if(out!=null){
			try {
				out.close();
			} catch (IOException e) {
				e.printStackTrace();
			}finally{
				out = null;
			}
		}
	}
}
```

#### 自动登录拦截器

```java
package com.itheima.filter;

import java.io.IOException;
import java.sql.SQLException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;
import javax.servlet.annotation.WebInitParam;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;

import com.itheima.domain.User;
import com.itheima.util.DaoUtils;

/**
 * Servlet Filter implementation class AutologinFilter
 */
@WebFilter(
		description = "自动登录过滤器", 
		urlPatterns = { "/*" }, 
		initParams = { 
				@WebInitParam(name = "encode", value = "utf-8", description = "编码过滤")
		})
public class AutologinFilter implements Filter {

    /**
     * Default constructor. 
     */
    public AutologinFilter() {
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see Filter#destroy()
	 */
	public void destroy() {
		// TODO Auto-generated method stub
	}

	/**
	 * @see Filter#doFilter(ServletRequest, ServletResponse, FilterChain)
	 */
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		// TODO Auto-generated method stub
		// place your code here
		//1.只有未登录的才能自动登录
		HttpServletRequest req=(HttpServletRequest)request;
		HttpServletResponse rsp=(HttpServletResponse)response;
		if (req.getSession(false)==null|| req.getSession().getAttribute("user")==null) {
			//2.只有带自动登录的的cookie才能自动登录
			Cookie[]cs=req.getCookies();
			Cookie findC=null;
			if (cs!=null) {
				for(Cookie c:cs) {
					if ("autologin".equals(c.getName())) {
						findC=c;
						break;
					}
				}
			}
			if (findC!=null) {
				//3.自动登录cookie中保存的用户名密码正确才能登录
				String name=findC.getValue().split(":")[0];
				String password=findC.getValue().split(":")[1];
				User user=null;
				String sql="select *from user where name=?and password=?";
				//数据库的操作
			
				try {
					QueryRunner runner=new QueryRunner(DaoUtils.getSource());
					user=runner.query(sql,new BeanHandler<User>(User.class),name,password);
				} catch (SQLException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
				if (user!=null) {
					req.getSession().setAttribute("user", user);
				}
			}
		}
		// pass the request along the filter chain
		chain.doFilter(request, response);
	}

	/**
	 * @see Filter#init(FilterConfig)
	 */
	public void init(FilterConfig fConfig) throws ServletException {
		// TODO Auto-generated method stub
	}

}

```

#### 全站乱码过滤器

```java
package com.itheima.filter;

import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.util.Map;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;
import javax.servlet.annotation.WebInitParam;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;

/**
 * Servlet Filter implementation class EncodingFilter
 */
@WebFilter(description = "全站乱码过滤器", urlPatterns = { "/*" },
initParams = { 
		@WebInitParam(name ="encode", value = "UTF-8")
})
public class EncodingFilter implements Filter {
	private FilterConfig config=null;
	private String encode=null;
    /**
     * Default constructor. 
     */
    public EncodingFilter() {
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see Filter#destroy()
	 */
	public void destroy() {
		// TODO Auto-generated method stub
	}

	/**
	 * @see Filter#doFilter(ServletRequest, ServletResponse, FilterChain)
	 */
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		// TODO Auto-generated method stub
		// place your code here
		//处理get请求的乱码
		response.setCharacterEncoding(encode);
		response.setContentType("text/html;charset="+encode);
		//处理post请求乱码
		//request.setCharacterEncoding(encode);
		// pass the request along the filter chain
		chain.doFilter(new MyHttpServletRequest((HttpServletRequest)request), response);
	}

	/**
	 * @see Filter#init(FilterConfig)
	 */
	public void init(FilterConfig fConfig) throws ServletException {
		// TODO Auto-generated method stub
		this.config=fConfig;
		//获取参数看参数是哪一个然后把他设置给新的编码方式
		encode=config.getInitParameter(encode)==null?"utf-8":config.getInitParameter(encode);
	}

	//装饰：写一个类实现被装饰的一个接口在构造方法传入被装饰者，不想改造的方法调用原有的方法，想改造
	//的自己来写逻辑
	
class MyHttpServletRequest extends HttpServletRequestWrapper{
		private HttpServletRequest request=null;
		private boolean isNotEncode=true;
		public MyHttpServletRequest(HttpServletRequest request) {
			super(request);
			// TODO Auto-generated constructor stub
			this.request=request;
		}
		@Override
		public String getParameter(String name) {
			// TODO Auto-generated method stub
			return getParameterValues(name)==null?null:getParameterValues(name)[0];
		
		
		}
		@Override
		public Map<String, String[]> getParameterMap() {
			// TODO Auto-generated method stub
			try {
				//post提交处理方式
			if (request.getMethod().equalsIgnoreCase("post")) {
				
					request.setCharacterEncoding(encode);
					return request.getParameterMap();
					
				}else if (request.getMethod().equalsIgnoreCase("get")) {
					//get处理方式
					Map<String, String[]> map=request.getParameterMap();
					if (isNotEncode) {
						for(Map.Entry<String, String[]> entry:map.entrySet()){
							String[] vsString=entry.getValue();
							for(int i=0;i<vsString.length;i++){
							vsString[i]=new String(vsString[i].getBytes("iso8859-1"),encode);
							}
						}
						isNotEncode=false;
					}
					
					return map;
				}else{
					return super.getParameterMap();
					}
				} catch (UnsupportedEncodingException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
					throw new RuntimeException();
				}
			
			
			
				
		}
		@Override
		public String[] getParameterValues(String name) {
			// TODO Auto-generated method stub
			return getParameterMap().get(name);
			
		}	
	}
}

```

#### 邮件发送

```java
public class MailUtils {

	//email:邮件发给谁  subject:主题  emailMsg：邮件的内容
	public static void sendMail(String email, String subject, String emailMsg)
			throws AddressException, MessagingException {
		
		// 1.创建一个程序与邮件服务器会话对象 Session
		Properties props = new Properties();
		props.setProperty("mail.transport.protocol", "SMTP");//发邮件的协议
		props.setProperty("mail.host", "localhost");//发送邮件的服务器地址
		props.setProperty("mail.smtp.auth", "true");// 指定验证为true

		// 创建验证器
		Authenticator auth = new Authenticator() {
			public PasswordAuthentication getPasswordAuthentication() {
				return new PasswordAuthentication("tom", "12345");//发邮件的账号的验证
			}
		};

		Session session = Session.getInstance(props, auth);

		// 2.创建一个Message，它相当于是邮件内容
		Message message = new MimeMessage(session);

		message.setFrom(new InternetAddress("leokay555@163.com")); // 设置发送者

		message.setRecipient(RecipientType.TO, new InternetAddress(email)); // 设置发送方式与接收者

		message.setSubject(subject);//邮件的主题

		message.setContent(emailMsg, "text/html;charset=utf-8");

		// 3.创建 Transport用于将邮件发送
		Transport.send(message);
	}
}
```

```
(1) javax.mail.Properties类   
		　　JavaMail需要Properties来创建一个session对象。它将寻找字符串"mail.smtp.host"，属性值就是发送邮件的主机.   
		用法:   
		　   Properties props = new Properties ();  //Properties props = System.getProperties();  
		　	props.put("mail.smtp.host", "smtp.163.com");//可以换上你的smtp主机名。   
(2) javax.mail.Session类   
		　　这个Session类代表JavaMail 中的一个邮件session. 每一个基于 JavaMail的应用程序至少有一个session但是可以有任意多的session。 在这个例子中, Session对象需要知道用来处理邮件的SMTP 服务器。   
		用法:   
		　　 Session sendMailSession = Session.getInstance(props, null);   //不须认证  
(3) javax.mail.Transport类   
		　　邮件是既可以被发送也可以被受到。JavaMail使用了两个不同的类来完成这两个功能：Transport 和Store. Transport 是用来发送信息的，而Store用来收邮件。在这发送邮件我们只需要用到Transport对象。   
			用法：   
			    Transport  transport = sendMailSession.getTransport("smtp");   
			　　用JavaMail Session对象的getTransport 方法来初始化Transport。传过去的字符串申明了对象所要使用的协议，如"smtp"。这将为我们省了很多时间。因为JavaMail以经内置了很多协议的实现方法。   
			　注意: JavaMail并不是绝对支持每一个协议，目前支持IMAP、 SMTP和 POP3.   
(4) javax.mail.MimeMessage类   
	　　Message对象将存储我们实际发送的电子邮件信息，Message对象被作为一个MimeMessage对象来创建并且需要知道应当选择哪一个JavaMail session。   
	　　用法：   
			 Message newMessage = new MimeMessage(sendMailSession);   
(5) javax.mail.InternetAddress类   
		一旦您创建了 Session 和 Message，并将内容填入消息后，就可以用Address确定信件地址了。和 Message 一样，Address 也是个抽象类。您用的是Javax.mail.internet.InternetAddress 类.   
		用法:   
		    InternetAddress from=new InternetAddress("xxf@cafe.com");   //收件人邮箱地址  
(6) javax.mail.Store类   
		Store类实现特定邮件协议上的读、写、监视、查找等操作。通过Javax.mail.Store类可以访问Javax.mail.Folder类。   
		用法:   
		    Store store=s.getSorte("pop3");  //s为一个邮件会话   
		    store.connect(popserver,username,password);//通过你提供的popserver地址(邮箱服务器),用户名和密码登录你的邮箱    
(7) javax.mail.Folder类   
		Folder类用于分级组织邮件，并提供照Javax.mail.Message格式访问email的能力。   
			用法:   
			    Folder folder=store.getFolder("INBOX");   
			    folder.open(Folder.READ_ONLY);   
(8) javax.mail.Internet.MimeMultipart   
		一般保存电子邮件内容的容器是Multipart抽象类,它定义了增加和删除及获得电子邮件不同部分内容的方法.由于Multipart是抽象类,我们必须为它使用一个具体的子类,JavaMail API提供javax.mail.Internet.MimeMultpart类来使用MimeMessage对象.   
			用法:   
			    MimeMultipart multipart=new MimeMultipart();   
		注:我们使用MimeMultipart对象的一个方法是addBodyPart(),它在我们的电子邮件内容里添加BodyPart(BodyPart类在下面紧接着要介绍)对象.消息可以有很多部分,一个BodyPart可以代表一个部分.   
(9) javax.mail.Internet.MimeBodyPart类   
		MimeBodyPart是BodyPart具体用于mimeMessage的一个子类.   
		MimeBodyPart对象代表一个MimeMessage对象内容的一部分.每个MimeBodyPart被认为有两部分:   
		⊙一个MIME类型   
		⊙匹配这个类型的内容   
		用法:   
		    MimeBodyPart mdp=new MimeBodyPart();   
		    String text="Hello JavaMail!";   
		//定义MIME类型为text/plain,并设置MimeBodyPart的内容.   
		    mdp.setContent(text,"text/plain");    
(10) javax.activation.DataHandler类(包含在JAF中)   
		JavaMail API不限制信息只为文本,任何形式的信息都可能作茧自缚MimeMessage的一部分.除了文本信息,作为文件附件包含在电子邮件信息的一部分是很普遍的.JavaMail API通过使用DataHandler对象,提供一个允许我们包含非文本BodyPart对象的简便方法.   
		用法:   
		    DataHandler dh=new DataHandler(text,type);   
		    mdp.setDatahandler(dh);  //mdp是一个MimeBodyPart对象   
(11) javax.activation.FileDataSource类(包含在JAF中)   
		一个FileDataSource对象可以表示本地文件和服务器可以直接访问的资源.一个本地文件可以通过创建一个新的MimeBodyPart对象附在一个mimeMessage对象上.   
		用法:   
		    MimeMultipart mm=new MimeMultipart();   
		    MimeBodyPart mdp=new MimeBodyPart();   
		    FileDataSource fds=new FileDataSource("c:/exam.txt");   
		    mdp.setDataHandler(new DataHandler(fds));   //设置数据源   
		    mm.addBodyPart(mdp);  //为当前消息MimeMultipart对象增加MimeBodyPart   
(12) javax.activation.URLDataSource类(包含在JAF中)   
远程资源,URL不会指向它们,由一个URLDataSource对象表示.一个远程资源可以通过创建一个新mimeBodyPart对象附在一个mimeMessage对象上(同FileDataSource差不多).   
```

#### 文件下载

```java
package com.leo.crazy;

import java.io.InputStream;
import java.io.RandomAccessFile;
import java.net.HttpURLConnection;
import java.net.URL;

/**
 *下载
 *download()方法负责按如下步骤来实现多线程下载。
（1）创建URL对象。
（2）获取指定URL对象所指向资源的大小（通过getContentLength()方法获得），此处用到了URLConnection类，该类代表Java应用程序和URL之间的通信链接。后面还有关于URLConnection更详细的介绍。
（3）在本地磁盘上创建一个与网络资源具有相同大小的空文件。
（4）计算每个线程应该下载网络资源的哪个部分（从哪个字节开始，到哪个字节结束）。
（5）依次创建、启动多个线程来下载网络资源的指定部分。
 * @author leoi555
 *@date 2018年10月20日
 */
public class DownUtil {
	    // 定义下载资源的路径
	    private String path;
	    // 指定所下载的文件的保存位置
	    private String targetFile;
	    // 定义需要使用多少个线程下载资源
	    private int threadNum;
	    // 定义下载的线程对象
	    private DownThread[] threads;
	    // 定义下载的文件的总大小
	    private int fileSize;
	    public DownUtil(String path, String targetFile, int threadNum)
	    {
	          this.path=path;
	          this.threadNum=threadNum;
	          // 初始化threads数组
	          threads=new DownThread[threadNum];
	          this.targetFile=targetFile;
	    }
	    public void download() throws Exception
	    {
	          URL url=new URL(path);
	          HttpURLConnection conn=(HttpURLConnection) url.openConnection();
	          conn.setConnectTimeout(5*1000);
	          conn.setRequestMethod("GET");
	          conn.setRequestProperty(
	                "Accept",
	                "image/gif, image/jpeg, image/pjpeg, image/pjpeg, "
	                + "application/x-shockwave-flash, application/xaml+xml, "
	                + "application/vnd.ms-xpsdocument, application/x-ms-xbap, "
	                + "application/x-ms-application, application/vnd.ms-excel, "
	                + "application/vnd.ms-powerpoint, application/msword, ＊/＊");
	          conn.setRequestProperty("Accept-Language", "zh-CN");
	          conn.setRequestProperty("Charset", "UTF-8");
	          conn.setRequestProperty("Connection", "Keep-Alive");
	          // 得到文件大小
	          fileSize=conn.getContentLength();
	          conn.disconnect();
	          int currentPartSize=fileSize / threadNum + 1;
	          RandomAccessFile file=new RandomAccessFile(targetFile, "rw");
	          // 设置本地文件的大小
	          file.setLength(fileSize);
	          file.close();
	          for (int i=0; i < threadNum; i++)
	          {
	                // 计算每个线程下载的开始位置
	                int startPos=i*currentPartSize;
	                // 每个线程使用一个RandomAccessFile进行下载
	                RandomAccessFile currentPart=new RandomAccessFile(targetFile,
	                    "rw");
	                // 定位该线程的下载位置
	                currentPart.seek(startPos);
	                // 创建下载线程
	                threads[i]=new DownThread(startPos, currentPartSize,
	                    currentPart);
	                // 启动下载线程
	                threads[i].start();
	          }
	    }
	    // 获取下载的完成百分比
	    public double getCompleteRate()
	    {
	          // 统计多个线程已经下载的总大小
	          int sumSize=0;
	          for (int i=0; i < threadNum; i++)
	          {
	                sumSize +=threads[i].length;
	          }
	          // 返回已经完成的百分比
	          return sumSize*1.0 / fileSize;
	    }
	    private class DownThread extends Thread
	    {
	          // 当前线程的下载位置
	          private int startPos;
	          // 定义当前线程负责下载的文件大小
	          private int currentPartSize;
	          // 当前线程需要下载的文件块
	          private RandomAccessFile currentPart;
	          // 定义该线程已下载的字节数
	          public int length;
	          public DownThread(int startPos, int currentPartSize,
	                RandomAccessFile currentPart)
	          {
	                this.startPos=startPos;
	                this.currentPartSize=currentPartSize;
	                this.currentPart=currentPart;
	          }
	          public void run()
	          {
	                try
	                {
	                    URL url=new URL(path);
	                    HttpURLConnection conn=(HttpURLConnection)url
	                          .openConnection();
	                    conn.setConnectTimeout(5*1000);
	                    conn.setRequestMethod("GET");
	                    conn.setRequestProperty(
	                          "Accept",
	                          "image/gif, image/jpeg, image/pjpeg, image/pjpeg, "
	                          + "application/x-shockwave-flash, application/xaml+xml, "
	                          + "application/vnd.ms-xpsdocument, application/x-ms-xbap, "
	                          + "application/x-ms-application, application/vnd.ms-excel, "
	                          + "application/vnd.ms-powerpoint, application/msword, ＊/＊");
	                    conn.setRequestProperty("Accept-Language", "zh-CN");
	                    conn.setRequestProperty("Charset", "UTF-8");
	                    InputStream inStream=conn.getInputStream();
	                    // 跳过startPos个字节，表明该线程只下载自己负责的那部分文件
	                    inStream.skip(this.startPos);
	                    byte[] buffer=new byte[1024];
	                    int hasRead=0;
	                    // 读取网络数据，并写入本地文件
	                    while (length < currentPartSize
	                          && (hasRead=inStream.read(buffer)) !=-1)
	                    {
	                          currentPart.write(buffer, 0, hasRead);
	                          // 累计该线程下载的总大小
	                          length +=hasRead;
	                    }
	                    currentPart.close();
	                    inStream.close();
	                }
	                catch (Exception e)
	                {
	                    e.printStackTrace();
	                }
	          }
	    }
	
}

```

## 面试题：

**jsp 和 servlet 有什么区别？**

-  jsp经编译后就变成了Servlet.（JSP的本质就是Servlet，JVM只能识别java的类，不能识别JSP的代码，Web容器将JSP的代码编译成JVM能够识别的java类）
- jsp更擅长表现于页面显示，servlet更擅长于逻辑控制。
- Servlet中没有内置对象，Jsp中的内置对象都是必须通过HttpServletRequest对象，HttpServletResponse对象以及HttpServlet对象得到。
- Jsp是Servlet的一种简化，使用Jsp只需要完成程序员需要输出到客户端的内容，Jsp中的Java脚本如何镶嵌到一个类中，由Jsp容器完成。而Servlet则是个完整的Java类，这个类的Service方法用于生成对客户端的响应

**jsp 有哪些内置对象？作用分别是什么？**

 JSP有9个内置对象：

- request：封装客户端的请求，其中包含来自GET或POST请求的参数；
- response：封装服务器对客户端的响应；
- pageContext：通过该对象可以获取其他对象；
- session：封装用户会话的对象；
- application：封装服务器运行环境的对象；
- out：输出服务器响应的输出流对象；
- config：Web应用的配置对象；
- page：JSP页面本身（相当于Java程序中的this）；
- exception：封装页面抛出异常的对象。

**说一下 jsp 的 4 种作用域？**

 JSP中的四种作用域包括page、request、session和application，

- **page**代表与一个页面相关的对象和属性
- **request**代表与Web客户机发出的一个请求相关的对象和属性。一个请求可能跨越多个页面，涉及多个Web组件；需要在页面显示的临时数据可以置于此作用域。
- **session**代表与某个用户与服务器建立的一次会话相关的对象和属性。跟某个用户相关的数据应该放在用户自己的session中
- **application**代表与整个Web应用程序相关的对象和属性，它实质上是跨越整个Web应用程序，包括多个页面、请求和会话的一个全局作用域

**session 和 cookie 有什么区别？**

-  cookie数据存放在客户的浏览器上，session数据放在服务器上。
- cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗,考虑到安全应当使用session。
- session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能,考虑到减轻服务器性能方面，应当使用cookie。
- 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie，而session则存储与服务端，浏览器对其没有限制。

**说一下 session 的工作原理？**

 其实session是一个存在服务器上的类似于一个散列表格的文件。里面存有我们需要的信息，在我们需要用的时候可以从里面取出来。类似于一个大号的map吧，里面的键存储的是用户的sessionid，用户向服务器发送请求的时候会带上这个sessionid。这时就可以从中取出对应的值了

**如果客户端禁止 cookie 能实现 session 还能用吗？**

 Cookie与 Session，一般认为是两个独立的东西，Session采用的是在服务器端保持状态的方案，而Cookie采用的是在客户端保持状态的方案。但为什么禁用Cookie就不能得到Session呢？因为Session是用Session ID来确定当前对话所对应的服务器Session，而Session ID是通过Cookie来传递的，禁用Cookie相当于失去了Session ID，也就得不到Session了

**假定用户关闭Cookie的情况下使用Session，其实现途径有以下几种：**

-  设置php.ini配置文件中的“session.use_trans_sid = 1”，或者编译时打开打开了“--enable-trans-sid”选项，让PHP自动跨页传递Session ID。
- 手动通过URL传值、隐藏表单传递Session ID
- 用文件、数据库等形式保存Session ID，在跨页过程中手动调用

**Tomcat的优化经验**

去掉对web.xml的监视，把jsp提前编辑成Servlet。有富余物理内存的情况，加大tomcat使用的jvm的内存,关于Tomcat我会单独做一个关于Tomcat服务器的文章，毕竟我们无论如何都离不开运行的容器

**解释一下什么是servlet;**

servlet有良好的生存期的定义，包括加载和实例化、初始化、处理请求以及服务结束。这个生存期由javax.servlet.Servlet接口的init,service和destroy方法表达。

**4、说一说Servlet的生命周期?**

Servlet被服务器实例化后，容器运行其init方法，请求到达时运行其service方法，service方法自动派遣运行与请求对应的doXXX方法（doGet，doPost）等，当服务器决定将实例销毁的时候调用其destroy方法。

web容器加载servlet，生命周期开始。通过调用servlet的init()方法进行servlet的初始化。通过调用service()方法实现，根据请求的不同调用不同的do***()方法。结束服务，web容器调用servlet的destroy()方法。

**Servlet的基本架构**

```java
public class ServletName extends HttpServlet {

public void doPost(HttpServletRequest request,HttpServletResponse response) throws

ServletException, IOException {

}

public void doGet(HttpServletRequest request,HttpServletResponse response) throws

ServletException, IOException {

}

}
```

**SERVLET API中forward()与redirect()的区别？**

forward是服务器请求资源，服务器直接访问目标地址的URL，把那个URL的响应内容读取过来，然后把这些内容再发给浏览器，浏览器根本不知道服务器发送的内容是从哪儿来的，所以它的地址栏中还是原来的地址。

redirect就是服务端根据逻辑,发送一个状态码,告诉浏览器重新去请求那个地址，一般来说浏览器会用刚才请求的所有参数重新请求，所以session,request参数都可以获取。

**什么情况下调用doGet()和doPost()？**

Jsp页面中的FORM标签里的method属性为get时调用doGet()，为post时调用doPost()。

**Request对象的主要方法：**

```java
setAttribute(String name,Object)：设置名字为name的request的参数值

getAttribute(String name)：返回由name指定的属性值

getAttributeNames()：返回request对象所有属性的名字集合，结果是一个枚举的实例

getCookies()：返回客户端的所有Cookie对象，结果是一个Cookie数组

getCharacterEncoding()：返回请求中的字符编码方式

getContentLength()：返回请求的Body的长度

getHeader(String name)：获得HTTP协议定义的文件头信息

getHeaders(String name)：返回指定名字的request Header的所有值，结果是一个枚举的实例

getHeaderNames()：返回所以request Header的名字，结果是一个枚举的实例

getInputStream()：返回请求的输入流，用于获得请求中的数据

getMethod()：获得客户端向服务器端传送数据的方法

getParameter(String name)：获得客户端传送给服务器端的有name指定的参数值

getParameterNames()：获得客户端传送给服务器端的所有参数的名字，结果是一个枚举的实例

getParametervalues(String name)：获得有name指定的参数的所有值

getProtocol()：获取客户端向服务器端传送数据所依据的协议名称

getQueryString()：获得查询字符串

getRequestURI()：获取发出请求字符串的客户端地址

getRemoteAddr()：获取客户端的IP地址

getRemoteHost()：获取客户端的名字

getSession([Boolean create])：返回和请求相关Session

getServerName()：获取服务器的名字

getServletPath()：获取客户端所请求的脚本文件的路径

getServerPort()：获取服务器的端口号

removeAttribute(String name)：删除请求中的一个属性
```

**jsp页面间对象传递的方法**

request，session，application，cookie

**MVC的各个部分都有那些技术来实现?如何实现?**

MVC是Model－View－Controller的简写。Model代表的是应用的业务逻辑（通过JavaBean，EJB组件实现），View是应用的表示面（由JSP页面产生），Controller是提供应用的处理过程控制（一般是一个Servlet），通过这种设计模型把应用逻辑，处理过程和显示逻辑分成不同的组件实现。这些组件可以进行交互和重用。

**我们在web应用开发过程中经常遇到输出某种编码的字符，如iso8859-1等，如何输出一个某种编码的字符串？**

```java
String tempStr  = newString(str.getBytes("ISO-8859-1"), "GBK");
```

