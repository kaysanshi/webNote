## Redis

NoSQL : not only sql。非关系性数据库；解决大规模的数据集合带来的难题，尤其是大数据应用难题。<br/>

nosql的产品：key-value 形式；列储存数据库，文档性的数据库，图形数据库，易扩展，大数据流量，高性能，灵活的数据模型，高可用。<br/>

Redis:是一个高性能的key-value的数据库，支持数据的持久化，提供不同类型的数据结构存储，支持数据备份.支持以下类型：字符型，散列型，列表型list，集合型set，有序集合型sort set。

Redis有着更为复杂的数据结构并且提供对他们的原子性操作，这是一个不同于其他数据库的进化路径。Redis的数据类型都是基于基本数据结构的同时对程序员透明，无需进行额外的抽象。

Redis运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时需要权衡内存，因为数据量不能大于硬件内存。在内存数据库方面的另一个优点是，相比在磁盘上相同的复杂的数据结构，在内存中操作起来非常简单，这样Redis可以做很多内部复杂性很强的事情。同时，在磁盘格式方面他们是紧凑的以追加的方式产生的，因为他们并不需要进行随机访问。



### 使用redis的目的：	

​	在项目中主要用来用作数据的缓存，将数据缓存在redis中，减轻对底层数据库的访问压力，获得更高的并发和更快的请求响应速度

使用缓存之后，大量的查询语句就从原来的数据库服务器中，转移到了高效的Redis服务器中执行，这将在很大程度上减轻原来数据库服务器的压力，并且提升查询的反应速度和效率。所以在很大的程度上，系统性能就得到了很好的改善。

（1）高性能

​		比如说有一个很复杂的sql数据查询，这个查询要耗费大量的时间，如果每次都直接取数据查询，那必然会对请求响应时间造成很大的影响，如果能在第一次查询完毕之后，将其直接保存在缓存当中，下次查询的时候，直接在缓存中拿走现成的数据，这样就会大大缩短请求的响应时间。

（2）高并发

​		我们知道数据库能承受的并发是有限的，那么在流量高峰期(比如，抢购、打折、秒杀等等)，会有大量的请求进入我们的系统，比如查询某个商品的详情，如果我们没有缓冲，那么给次查询都要走数据库，假如我们的数据库每秒只能接受2000个请求，结果一秒钟进来了5000个请求，那么数据库就直接崩掉了，毫无高并发可言，而如果我们中间具有缓存服务，那么在第一个用户查询商品详情时(或者提前将放好)我们可以直接将商品的详情信息数据放到缓存里面，这样在后续用户查询时就可以直接走缓存，不走数据库，缓存是基于内存的，它的访问速度快，并发高；因此就可以提供一个高并发的支持。

### linux下安装redis:

```
$ wget http://download.redis.io/releases/redis-3.0.0.tar.gz
$ tar xzf redis-3.0.0.tar.gz
$ cd redis-3.0.0
$ make
```

make完后 redis-2.8.17目录下会出现编译后的redis服务程序redis-server,还有用于测试的客户端程序redis-cli,两个程序位于安装目录 src 目录下：

**启动redis服务：**

```
下面启动redis服务.进入redis的安装目录
方式一：
$ cd src
$ ./redis-server
注意这种方式启动redis 使用的是默认配置。也可以通过启动参数告诉redis使用指定配置文件使用下面命令启动。
方式二：
$ cd src
$ ./redis-server ../redis.conf
redis.conf 是一个默认的配置文件。我们可以根据需要使用自己的配置文件。
```

启动redis服务进程后，就可以使用测试客户端程序redis-cli和redis服务交互了。 比如：

```livescript
$ cd src
$ ./redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```

#### ubuntu下安装

在 Ubuntu 系统安装 Redis 可以使用以下命令:

> $sudo apt-get update
>
> $sudo apt-get install redis-server

**启动 Redis**

> $ redis-server
>
> ​		查看 redis 是否启动
>
> $ redis-cli
>
> ​		以上命令将打开以下终端：
>
> redis 127.0.0.1:6379>
> 			127.0.0.1 是本机 IP ，6379 是 redis 服务端口。现在我们输入 PING 命令。
>
> redis 127.0.0.1:6379> ping
> 		PONG
> 		以上说明我们已经成功安装了redis。

**查看Redis是否启动**

打开redis的客户端使用：

​				redis-cli:

​				redis 127.0.0.1:6379>ping ,检测是否启动redis



如果需要在远程 redis 服务上执行命令，同样我们使用的也是 redis-cli 命令。

​	语法
​		`$ redis-cli -h host -p port -a password`

​	 例如：

​		`$redis-cli -h 127.0.0.1 -p 6379 -a "mypass"`

​		`redis 127.0.0.1:6379>`

​		`redis 127.0.0.1:6379> PING`

**查看redis的配置项**

获取全部的配置项`CONFIG get *`

**redis 后台启动**

复制一份redis.conf 文件到 `usr/local/bin`中。然后修改其中的：**daemonize yes**  这个

然后到redis目录下进行启动

```
[root@localhost src]# ./redis-server /usr/local/bin/redis.conf

[root@localhost src]# ps aux|grep redis
root 97629  0.0  0.7 140912 7396 ? Ssl  11:13   0:00 ./redis-server *:6379
root  97915  0.0  0.0 112824 976 pts/0  R+ 11:13 0:00 grep --color=auto redis
[root@localhost src]# 
```

**redis 开机自启**

1.编写脚本 `vim /etc/init.d/redis`

[参考](https://www.cnblogs.com/-zhuang/articles/10599276.html)

2.设置redis权限  `chmod 755 /etc/init.d/redis`

3.启动redis   `/etc/init.d/redis start`

4.设置开机自启：

```
cd /etc/init.d/
chkconfig redis on
```



### Linux下删除Redis

1. **卸载软件**
   					apt-get remove redis

   

2. **清除配置**

   ​					apt-get remove --purge redis

3. **删除残留文件**

   ​					find / -name redis

   

   > ​	一般设置如下
   > ​					rm -rf var/lib/redis/
   >
   > ​				    rm -rf /var/log/redis
   >
   > ​					rm -rf /etc/redis/
   >
   > ​					rm -rf /usr/bin/redis-*



### **Redis.conf 配置项说明如下：**

**语法Redis CONFIG 命令格式如下：`redis 127.0.0.1:6379> CONFIG GET CONFIG_SETTING_NAME`**

**实例**

```
redis 127.0.0.1:6379> CONFIG GET loglevel

1) "loglevel"
2) "notice"
```

#### 编辑配置

你可以通过修改 redis.conf 文件或使用 **CONFIG set** 命令来修改配置。

**语法**

**CONFIG SET** 命令基本语法：

```
redis 127.0.0.1:6379> CONFIG SET CONFIG_SETTING_NAME NEW_CONFIG_VALUE
```

**实例**

```
redis 127.0.0.1:6379> CONFIG SET loglevel "notice"
OK
redis 127.0.0.1:6379> CONFIG GET loglevel

1) "loglevel"
2) "notice"
```

| daemonize no                 | Redis 默认不是以守护进程的方式运行，可以通过该配置项修改，使用 yes 启用守护进程（Windows 不支持守护线程的配置为 no ） |
| ---------------------------- | ------------------------------------------------------------ |
| `pidfile /var/run/redis.pid` | 当 Redis 以守护进程方式运行时，Redis 默认会把 pid 写入 /var/run/redis.pid 文件，可以通过 pidfile 指定 |
| `port 6379`                  | 指定 Redis 监听端口，默认端口为 6379，作者在自己的一篇博文中解释了为什么选用 6379 作为默认端口，因为 6379 在手机按键上 MERZ 对应的号码，而 MERZ 取自意大利歌女 Alessia Merz 的名字 |
| `bind 127.0.0.1`             | 绑定的主机地址                                               |
| `timeout 300`                | 当客户端闲置多长秒后关闭连接，如果指定为 0 ，表示关闭该功能  |
| `loglevel notice`            | 指定日志记录级别，Redis 总共支持四个级别：debug、verbose、notice、warning，默认为 notice |
| `logfile stdout`             | 日志记录方式，默认为标准输出，如果配置 Redis 为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会发送给 /dev/null |
| `databases 16`               | 设置数据库的数量，默认数据库为0，可以使用SELECT 命令在连接上指定数据库id |

### Redis作为服务进行开机自启

进入redis目录 我的redis是在/usr/local/src/redis-2.8.17/

```
[root@localhost redis-2.8.17]# mkdir /etc/redis
[root@localhost redis-2.8.17]# mv redis.conf /etc/redis/6379.conf
[root@localhost utils]# cp redis_init_script /etc/init.d/redis
[root@localhost utils]# vim /etc/init.d/redis
[root@localhost redis-2.8.17]# service redis start
```

修改vim /etc/init.d/redis

```
REDISPORT=6379
EXEC=/usr/local/src/redis-2.8.17/src/redis-server
CLIEXEC=/usr/local/src/redis-2.8.17/src/redis-cli

PIDFILE=/var/run/redis_${REDISPORT}.pid
CONF="/etc/redis/${REDISPORT}.conf"

```

然后启动。

### Redis基本数据类型：

​		**Redis的数据类型包括五种基本的数据类型。`String字符串--String`, `hash字典---HashMap`，`list列表---LinkedList`，`set集合---HashSet`，`zset(sorted_set)有序集合---TreeSeet` 下面依次介绍使用**

`redis自身是一个Map其中这里说的数据类型均表示value部分的类型。因为key部分永远都是字符串。`

####  String字符串：

​		redis的String可以包含任何的数据类型如jpg,序列化队形

> ​				 set name "lkl"  //添加修改数据
>
> ​				 get name   // 可以获得刚输入的值
>
> ​				 del name  // 删除key数据
>
> 添加修改多个数据：
>
> ​	mset key1 value1 key2 value2
>
> 获取多个数据
>
> ​	mget key1 key2
>
> 获取数据字符长度
>
> ​	strlen key
>
> 追加信息到原始信息后部分（如果原始信息存在就追加，否则新建）
>
> ​	append key value

##### 使用场景：		

- ​	<font color="blue" size="4px">redis用于控制数据库表主键id，为数据库表主键提供生成策略，保障数据库表的主键唯一性  此方案适用于所有数据库，且支持数据库集群</font>

  - **大型企业级应用中，分表操作是基本操作，使用多张表存储同类型数据，但是对应的主键 id 必须保证统一性 ，不能重复。Oracle 数据库具有 sequence 设定，可以解决该问题，但是 MySQL数据库并不具有类似的机 制，那么如何解决？** 

    - 设置数值数据增加指定范围的值

      ```
      incr key
      incrby key increment
      incrbyfloat key increment
      ```

    - 设置数值数据减少指定范围的值

      ```
      decr key
      decrby key increment
      ```

      string在redis的内部存储默认是一个字符串，当遇到增减类操作incr,descr时会自动转为数值性计算

      redis所有的操作都是原子性的，采用单线程的处理，无需考虑并发带来数据的影响。

-    <font color="blue" size="4px"> redis 控制数据的生命周期，通过数据是否失效控制业务行为，适用于所有具有时效性限定控制的操作</font>

  - **电商商家开启热门商品推荐，热门商品不能一直处于热门期，每种商品热门期维持3天，3天后自动取消热门。 新闻网站会出现热点新闻，热点新闻最大的特征是时效性，如何自动控制热点新闻的时效性？**

    - 设置数据类型具有指定的生命周期

      ```
      setex key seconds value
      psetex key milliseconds value
      ```

-    <font color="blue" size="4px">redis应用于各种结构型和非结构型高热度数据访问加速</font>

  - **主页高频访问信息显示控制，例如新浪微博大V主页显示粉丝数与微博数量**

    - 在redis中为大V用户设定用户信息，以用户主键和属性值作为key，后台设定定时刷新策略即可 

      ```
      eg: user:id:3506728370:fans → 12210947 
      eg: user:id:3506728370:blogs → 6164 
      eg: user:id:3506728370:focuss → 83 
      ```

    - 在redis中以json格式存储大V用户信息，定时刷新（也可以使用hash类型）

      ```
       eg: user:id:3506728370 → 
       {"id":3506728370,"name":"春晚","fans":12210862,"blogs":6164, "focus":83}
      ```

##### 注意事项

- 数据操作不成功的反馈与数据正常操作之间的差异 string 类型数据操作的注意事项 

  ① 表示运行结果是否成功 

  >  (integer) 0 → false 失败 
  >
  >  (integer) 1 → true 成功 

  ② 表示运行结果值 

  > (integer) 3 → 3 3个 
  >
  > (integer) 1 → 1 1个 

- 数据未获取到 （nil）等同于null 

- 数据最大存储量 512MB 

- 数值计算最大范围（java中的long最大值)

#### Hash:是一个键值对key->value 集合

对一系列存储的数据进行编组，方便管理，典型应用存储对象信息.

- 如果field数量较少，存储结构优化为类数组结构 
- 如果field数量较多，存储结构使用HashMap结构

> ​				 hset key  field value  //添加修改数据
>
> ​				 hget key  field // 可以获得刚输入的值
>
> ​				 hdel key  field1 [field2] // 删除key数据
>
> 添加修改多个数据：
>
> ​	hmset key field1 value1 field2 value2 … 
>
> 获取多个数据
>
> ​	hmget key field1 field2 … 
>
> 获取hash表字段的数量
>
> ​	hlen key
>
> 获取hash表中是否存在指定的字段
>
> ​	hexists key field

​	获取设置的值：HGET myhash field1    输出的结果为hello

##### 注意事项

- hash类型下的value只能存储字符串，不允许存储其他数据类型，不存在嵌套现象。如果数据未获取到， 对应的值为（nil）
- 每个 hash 可以存储 2 32 - 1 个键值对 
- hash类型十分贴近对象的数据存储形式，并且可以灵活添加删除对象属性。但hash设计初衷不是为了存 储大量对象而设计的，切记不可滥用，更不可以将hash作为对象列表使用 
- hgetall 操作可以获取全部属性，如果内部field过多，遍历整体数据效率就很会低，有可能成为数据访问 瓶颈

##### 使用场景：		

​	场景：`存储，读取，修改，用户属性`，

- <font color="blue" size="4px">redis 应用于购物车数据存储设计</font>

  - 电商网站购物车设计与实现:

    > 以客户id作为key，每位客户创建一个hash存储结构存储对应的购物车信息 
    >
    > 将商品编号作为field，购买数量作为value进行存储 
    >
    > 添加商品：追加全新的field与value 
    >
    > 浏览：遍历hash 
    >
    > 更改数量：自增/自减，设置value值 
    >
    > 删除商品：删除field 
    >
    > 清空：删除key 
    >
    > 此处仅讨论购物车中的模型设计 
    >
    > 购物车与数据库间持久化同步、购物车与订单间关系、未登录用户购物车信息存储不进行讨论

    当前仅仅是将数据存储到了redis中，并没有起到加速的作用，商品信息还需要二次查询数据库 每条购物车中的商品记录保存成两条field  field1专用于保存购买数量 

    > 命名格式：商品 id : nums 
    >
    > 保存数据：数值 

    field2专用于保存购物车中显示的信息，包含文字描述，图片地址，所属商家信息等 

    > 命名格式：商品 id : info 
    >
    > 保存数据：json

- <font color="blue" size="4px">redis 应用于抢购，限购类、限量发放优惠卷、激活码等业务的数据存储设计</font>

  - 双11活动日，销售手机充值卡的商家对移动、联通、电信的30元、50元、100元商品推出抢购活动，每种商 品抢购上限1000张

    > 以商家id作为key 
    >
    > 将参与抢购的商品id作为field  
    >
    > 将参与抢购的商品数量作为对应的value 
    >
    > 抢购时使用降值的方式控制产品数量 
    >
    > 实际业务中还有超卖等实际问题，这里不做讨论

#### List（列表）

​			Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。 列表最多可存储 232 - 1 元素 (4294967295, 每个列表可存储40多亿)。<br/>

> 添加/修改数据
>
>    lpush key value1 [value2] …… 
>
>    rpush key value1 [value2] ……
>
> 获取数据
>
>  lrange key start stop
>
>  lindex key index 
>
>  llen key 
>
> 获取并移除数据
>
>  lpop key 
>
>  rpop key
>
> 规定时间内获取并移除数据
>
>   blpop key1 [key2] timeout 
>
>   brpop key1 [key2] timeout 
>
>   brpoplpush source destination timeout

##### 使用场景：

- <font color="blue" size="4px">redis 应用于具有操作先后顺序的数据控制  最新消息排行等功能(比如朋友圈的时间线)</font>  `移除指定的数据：lrem key count value`

  - twitter、新浪微博、腾讯微博中个人用户的关注列表需要按照用户的关注顺序进行展示，粉丝列表需要将最 近关注的粉丝列在前面

  > 依赖list的数据具有顺序的特征对信息进行管理 
  >
  > 使用队列模型解决多路信息汇总合并的问题 
  >
  > 使用栈模型解决最新消息的问题

-  <font color="blue" size="4px">消息队列</font>

​		

##### 注意事项

- list中保存的数据都是string类型的，数据总容量是有限的，最多2 32 - 1 个元素 (4294967295)。 
-  list具有索引的概念，但是操作数据时通常以队列的形式进行入队出队操作，或以栈的形式进行入栈出栈操作 
-  获取全部数据操作结束索引设置为-1 
- list可以对数据进行分页操作，通常第一页的信息来自于list，第2页及更多的信息通过数据库的形式加载

#### Set（集合）Redis的Set是string类型的无序集合。

​		集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。 能够保存大量的数据，高效的内部存储机制，便于查询。

​		`sadd 命令`

​		添加一个 string 元素到 key 对应的 set 集合中，成功返回1，如果元素已经在集合中返回 0，如果 key 对应的 set 不存在则返回错误。
​		`sadd key member`

​		集合中最大的成员数为 232 - 1(4294967295, 每个集合可存储40多亿个成员)。

##### 使用场景：

共同好友 

利用唯一性,统计访问网站的所有独立ip 

好友推荐时,根据tag求交集,大于某个阈值就可以推荐

- <font color="blue" size="4px">redis 应用于随机推荐类信息检索，例如热点歌单推荐，热点新闻推荐，热卖旅游线路，应用APP推荐， 大V推荐等</font>

  - 每位用户首次使用今日头条时会设置3项爱好的内容，但是后期为了增加用户的活跃度、兴趣点，必须让用户 对其他信息类别逐渐产生兴趣，增加客户留存度，如何实现？

    系统分析出各个分类的最新或最热点信息条目并组织成set集合 ， 随机挑选其中部分信息 、配合用户关注信息分类中的热点信息组织成展示的全信息集合

    > 随机获取集合中指定数量的数据
    >
    > srandmember key [count]
    >
    > 随机获取集合中某个数据并将该数据移出集合
    >
    > spop key [count]

- <font color="blue" size="4px">redis 应用于同类信息的关联搜索，二度关联搜索，深度关联搜索(显示共同关注。共同好友）</font>

  - 脉脉为了促进用户间的交流，保障业务成单率的提升，需要让每位用户拥有大量的好友，事实上职场新人不 具有更多的职场好友，如何快速为用户积累更多的好友？

  -  新浪微博为了增加用户热度，提高用户留存性，需要微博用户在关注更多的人，以此获得更多的信息或热门 话题，如何提高用户关注他人的总量？

  -  QQ新用户入网年龄越来越低，这些用户的朋友圈交际圈非常小，往往集中在一所学校甚至一个班级中，如何 帮助用户快速积累好友用户带来更多的活跃度？

  -  微信公众号是微信信息流通的渠道之一，增加用户关注的公众号成为提高用户活跃度的一种方式，如何帮助 用户积累更多关注的公众号？ 美团外卖为了提升成单量，必须帮助用户挖掘美食需求，如何推荐给用户最适合自己的美食？

    > 求两个集合的交、并、差集 解决方案 
    >
    > sinter key1 [key2]
    >
    > sunion key1 [key2] 
    >
    > sdiff key1 [key2]
    >
    > 求两个集合的交、并、差集并存储到指定集合中 
    >
    > sinterstore destination key1 [key2] 
    >
    > sunionstore destination key1 [key2]
    >
    > sdiffstore destination key1 [key2] 
    >
    >  将指定数据从原始集合中移动到目
    >
    > smove source destination member 

- <font color="blue" size="4px">redis 应用于同类型数据的快速去重</font>

  - 公司对旗下新的网站做推广，统计网站的PV（访问量）,UV（独立访客）,IP（独立IP）。 PV：网站被访问次数，可通过刷新页面提高访问量 UV：网站被不同用户访问的次数，可通过cookie统计访问量，相同用户切换IP地址，UV不变 IP：网站被不同IP地址访问的总次数，可通过IP地址统计访问量，相同IP不同用户访问，IP不变

    >  利用set集合的数据去重特征，记录各种访问数据 
    >
    >  建立string类型数据，利用incr统计日访问量（PV） 
    >
    >  建立set模型，记录不同cookie数量（UV）
    >
    >  建立set模型，记录不同IP数量（IP）

- <font color="blue" size="4px">redis 应用于基于黑名单与白名单设定的服务控制</font>

  - 资讯类信息类网站追求高访问量，但是由于其信息的价值，往往容易被不法分子利用，通过爬虫技术， 快速获取信息，个别特种行业网站信息通过爬虫获取分析后，可以转换成商业机密进行出售。例如第三方火 车票、机票、酒店刷票代购软件，电商刷评论、刷好评。 同时爬虫带来的伪流量也会给经营者带来错觉，产生错误的决策，有效避免网站被爬虫反复爬取成为每 个网站都要考虑的基本问题。在基于技术层面区分出爬虫用户后，需要将此类用户进行有效的屏蔽，这就是 黑名单的典型应用

    > 基于经营战略设定问题用户发现、鉴别规则 
    >
    > 周期性更新满足规则的用户黑名单，加入set集合 
    >
    > 用户行为信息达到后与黑名单进行比对，确认行为去向 
    >
    > 黑名单过滤IP地址：应用于开放游客访问权限的信息源 
    >
    > 黑名单过滤设备信息：应用于限定访问设备的信息源 
    >
    > 黑名单过滤用户：应用于基于访问权限的信息源

```
实例
redis 127.0.0.1:6379> sadd runoob redis
(integer) 1
redis 127.0.0.1:6379> sadd runoob mongodb
(integer) 1
redis 127.0.0.1:6379> sadd runoob rabitmq
(integer) 1
redis 127.0.0.1:6379> sadd runoob rabitmq
(integer) 0
redis 127.0.0.1:6379> smembers runoob

1) "redis"
2) "rabitmq"
3) "mongodb"
注意：以上实例中 rabitmq 添加了两次，但根据集合内元素的唯一性，第二次插入的元素将被忽略。
```

##### 注意事项

- set 类型不允许数据重复，如果添加的数据在 set 中已经存在，将只保留一份 set 类型数据操作的
- set 虽然与hash的存储结构相同，但是无法启用hash中存储值的空间

#### zset(sorted set：有序集合)

​		Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。zset的成员是唯一的,但分数(score)却可以重复。 在set的存储结构基础上添加可排序字段

​		`zadd 命令`  添加元素到集合，元素在集合中存在则更新对应score

> 添加数据
>
> ​	zadd key score1 member1 [score2 member2]
>
> 获取全部数据 
>
> ​	zrange key start stop [WITHSCORES] 
>
> ​	zrevrange key start stop [WITHSCORES]
>
> 删除数据
>
>  	zrem key member [member ...] 
>
> 按条件获取数据
>
> ​	zrangebyscore key min max [WITHSCORES] [LIMIT] 
>
> ​	zrevrangebyscore key max min [WITHSCORES] 
>
> 条件删除数据 
>
> ​	zremrangebyrank key start stop 
>
> ​	zremrangebyscore key min max 
>
>  min与max用于限定搜索查询的条件 
>
>  start与stop用于限定查询范围，作用于索引，表示开始和结束索引 
>
>  offset与count用于限定查询范围，作用于查询结果，表示开始位置和数据总量
>
> 获取集合数据总量
>
> zcard key 
>
> zcount key min max
>
> 集合交、并操作
>
>  zinterstore destination numkeys key [key ...] 
>
> zunionstore destination numkeys key [key ...]  

##### 使用场景：

- <font color="blue" size="4px">排行榜 redis 应用于计数器组合排序功能对应的排名</font>

  - 为所有参与排名的资源建立排序依据

    > 获取数据对应的索引（排名）
    >
    > ​	zrank key member 
    >
    > ​	zrevrank key member
    >
    > score值获取与修改
    >
    > ​	zscore key member 
    >
    > ​	zincrby key increment member

- <font color="blue" size="4px">redis 应用于即时任务/消息队列执行管理</font>

  - 当任务或者消息待处理，形成了任务队列或消息队列时，对于高优先级的任务要保障对其优先处理，如 何实现任务权重管理。

    >  对于带有权重的任务，优先处理权重高的任务，采用score记录权重即可 多条件任务权重设定 
    >
    > 如果权重条件过多时，需要对排序score值进行处理，保障score值能够兼容2条件或者多条件，例如外贸 订单优先于国内订单，总裁订单优先于员工订单，经理订单优先于员工订单 
    >
    >  因score长度受限，需要对数据进行截断处理，尤其是时间设置为小时或分钟级即可（折算后）
    >
    >   先设定订单类别，后设定订单发起角色类别，整体score长度必须是统一的，不足位补0。第一排序规则首 位不得是0 
    >
    > ​       例如外贸101，国内102，经理004，员工008。
    >
    > ​       员工下的外贸单score值为101008（优先） 
    >
    >  经理下的国内单score值为102004

- <font color="blue" size="4px">redis 应用于定时任务执行顺序管理或任务过期管理</font>

  - 网站会定期开启投票、讨论，限时进行，逾期作废。如何有效管理此类过期信息

    >  对于基于时间线限定的任务处理，将处理时间记录为score值，利用排序功能区分处理的先后顺序 
    >
    >  记录下一个要处理的时间，当到期后处理对应任务，移除redis中的记录，并记录下一个要处理的时间 
    >
    >  当新任务加入时，判定并更新当前下一个要处理的任务时间  为提升sorted_set的性能，通常将任务根据特征存储成若干个sorted_set。例如1小时内，1天内，周内， 月内，季内，年度等，操作时逐级提升，将即将操作的若干个任务纳入到1小时内处理的队列中
    >
    >  获取当前系统时间 time

##### 注意事项

-  score保存的数据存储空间是64位，如果是整数范围是-9007199254740992~9007199254740992 
- score保存的数据也可以是一个双精度的double值，基于双精度浮点数的特征，可能会丢失精度，使用时 候要慎重 
- sorted_set 底层存储还是基于set结构的，因此数据不能重复，如果重复添加相同的数据，score值将被反 复覆盖，保留最后一次修改的结果

<font color="blue">**以上redis使用场景可以总结如下几个点：**</font>

1. redis用于控制数据库表主键id，为数据库表主键提供生成策略，保障数据库表的主键唯一性 
2. redis 控制数据的生命周期，通过数据是否失效控制业务行为，适用于所有具有时效性限定控制的操作 
3. redis应用于各种结构型和非结构型高热度数据访问加速 
4. redis 应用于购物车数据存储设计 
5. redis 应用于抢购，限购类、限量发放优惠卷、激活码等业务的数据存储设计 
6. redis 应用于具有操作先后顺序的数据控制 
7. redis 应用于最新消息展示  
8. redis 应用于随机推荐类信息检索，例如热点歌单推荐，热点新闻推荐，热卖旅游线路，应用APP推荐，大V推荐等 
9. redis 应用于同类信息的关联搜索，二度关联搜索，深度关联搜索 
10. redis 应用于同类型不重复数据的合并、取交集操作 
11. redis 应用于同类型数据的快速去重 
12. redis 应用于基于黑名单与白名单设定的服务控制 
13. redis 应用于计数器组合排序功能对应的排名 
14. redis 应用于定时任务执行顺序管理或任务过期管理 
15. redis 应用于及时任务/消息队列执行管理 
16. redis 应用于按次结算的服务控制 
17. redis 应用于基于时间顺序的数据操作，而不关注具体时间

### Redis高级数据类型

- Bitmaps
- HyperLogLog
- GEO

### Redis命令

#### Redis key操作的命令

- ​     `keys *`   查询所有 
- ​      `keys it*`   查询所有以it开头
- ​      `keys *heima`  查询所有以heima结尾
- ​       `keys ??heima`  查询所有前面两个字符任意，后面以heima结尾 
- ​       `keys user:?`  查询所有以user:开头，最后一个字符任意 
- ​        `keys u[st]er:1`  查询所有以u开头，以er:1结尾，中间包含一

- ​		`Del key`：用于删除key
- ​		`DUMP key`：序列化key
- ​		`EXISTS  key` :检查key是否存在
- ​		`EXPIRE key seconds:` 为key设置过期时间，以秒计
- ​		`EXPIRE key timestamp`: 为key设置过期时间  接受的unix的时间戳
- ​		`EXPIREAT  key millseconds`:设置key的过期时间
- ​		`PEXPIREAT key milliseconds-timestamp`  ：设置 key 过期时间的时间戳(unix timestamp) 以毫秒计KEYS pattern 查找所有符合给定模式( pattern)的 key 。
- ​		`MOVE key db`  ：将当前数据库的 key 移动到给定的数据库 db 当中。
- ​		`PERSIST key`  ：移除 key 的过期时间，key 将持久保持。
- ​		`PTTL key`  ：以毫秒为单位返回 key 的剩余的过期时间。
- ​		`TTL key`  ：以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)。
- ​		`RANDOMKEY`  ：从当前数据库中随机返回一个 key 。
- ​		`RENAME key newkey`  :修改 key 的名称
- ​		`RENAMENX key newkey`  :仅当 newkey 不存在时，将 key 改名为 newkey 。
- ​		`TYPE key`  :返回 key 所储存的值的类型。

#### Redis 字符串常用命名

- ​		`set key value` :设置指定的key的值
- ​		`get key`：获得指定的key
- ​		`getRANGE key start end` :返回key中的字符串值
- ​		`GETSET key value` :将指定的key设置value 返回key的旧值
- ​		`MGET key1[key2]`  :获取一个或多个给定的key值
- ​		`SETNX key vlue`  :只有key不存在时在时设置 key 的值。
- ​		`SETRANGE key offset value` :用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始。
- ​		`STRLEN key`：返回 key 所储存的字符串值的长度。
- ​		`MSET key value [key value ...]`：同时设置一个或多个 key-value 对。
- ​		`MSETNX key value [key value ...]` ：同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。
- ​		`PSETEX key milliseconds value`：这个命令和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，而不是像 SETEX 命令那样，以秒为单位。
- ​		`INCR key`：将 key 中储存的数字值增一。
- ​		`INCRBY key increment` ：将 key 所储存的值加上给定的增量值（increment） 。
- ​		`INCRBYFLOAT key increment`：将 key 所储存的值加上给定的浮点增量值（increment） 。
- ​		`DECR key` ：将 key 中储存的数字值减一。
- ​		`DECRBY key decrement`  ： key 所储存的值减去给定的减量值（decrement） 。
- ​		`APPEND key value`：如果 key 已经存在并且是一个字符串， APPEND 命令将指定的 value 追加到该 key 原来值（value）的末尾。

#### Redis hash字典操作常用的命令

- ​			`HDEL key field1 [field2]` 删除一个或多个哈希表字段
- ​			`HEXISTS key field` 查看哈希表 key 中，指定的字段是否存在。
- ​			`HGET key field` 获取存储在哈希表中指定字段的值。
- ​			`HGETALL key` 获取在哈希表中指定 key 的所有字段和值
- ​			`HINCRBY key field increment` 为哈希表 key 中的指定字段的整数值加上增量 increment 。
- ​			`HINCRBYFLOAT key field increment` 为哈希表 key 中的指定字段的浮点数值加上增量 increment 。
- ​			`HKEYS key` 获取所有哈希表中的字段
- ​			`HLEN key` 获取哈希表中字段的数量
- ​			`HMGET key field1 [field2]` 获取所有给定字段的值
- ​		    `HMSET key field1 value1 [field2 value2 ]` 同时将多个 field-value (域-值)对设置到哈希表 key 中。
- ​			`HSET key field value` 将哈希表 key 中的字段 field 的值设为 value 。
- ​			`HSETNX key field value` 只有在字段 field 不存在时，设置哈希表字段的值。
- ​			`HVALS key` 获取哈希表中所有值
- ​			`HSCAN key cursor [MATCH pattern] [COUNT count]` 迭代哈希表中的键值对。

#### Redis List(列表)操作常用的命令

#### Redis Set(集合)操作常用的命名

#### Redis Zset(有序集合)操作常用的命令

```
zadd key score member 
实例
redis 127.0.0.1:6379> zadd runoob 0 redis
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 mongodb
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 rabitmq
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 rabitmq
(integer) 0
redis 127.0.0.1:6379> > ZRANGEBYSCORE runoob 0 1000
1) "mongodb"
2) "rabitmq"
3) "redis"
```

上面三个可以去参考菜鸟教程



### Redis 持久化方案

Redis的所有数据都是保存到内存中的。

> Rdb：快照形式，定期把内存中当前时刻的数据保存到磁盘。Redis默认支持的持久化方案。
>
> AOF形式：把所有对数据库操作的命令，增删改操作的命令。保存到文件中。数据库恢复时把所有的命令执行一遍即可

- RDB持久化方式能够在指定的时间间隔能对你的数据进行快照存储.
- AOF持久化方式记录每次对服务器写的操作,当服务器重启的时候会重新执行这些命令来恢复原始的数据,AOF命令以redis协议追加保存每次写的操作到文件末尾.Redis还能对AOF文件进行后台重写,使得AOF文件的体积不至于过大.
- 如果你只希望你的数据在服务器运行的时候存在,你也可以不使用任何持久化方式.
- 你也可以同时开启两种持久化方式, 在这种情况下, 当redis重启的时候会优先载入AOF文件来恢复原始的数据,因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整.

#### RDB

快照形式，定期把内存中当前时刻的数据保存到磁盘。Redis默认支持的持久化方案。

![img](http://img1.tuicool.com/NjYjYvF.png!web?_=6182478)



##### **优点**

- RDB是一个非常紧凑的文件,它保存了某个时间点得数据集,非常适用于数据集的备份,比如你可以在每个小时报保存一下过去24小时内的数据,同时每天保存过去30天的数据,这样即使出了问题你也可以根据需求恢复到不同版本的数据集.
- RDB是一个紧凑的单一文件,很方便传送到另一个远端数据中心或者亚马逊的S3（可能加密），<font color="red">非常适用于灾难恢复.</font>
- RDB在保存RDB文件时父进程唯一需要做的就是fork出一个子进程,接下来的工作全部由子进程来做，父进程不需要再做其他IO操作，所以RDB持久化方式可以最大化redis的性能.
- 与AOF相比,在恢复大的数据集的时候，RDB方式会更快一些.

##### 缺点弊端

- 如果你希望在redis意外停止工作（例如电源中断）的情况下丢失的数据最少的话，那么RDB不适合你.虽然你可以配置不同的save时间点(例如每隔5分钟并且对数据集有100个写的操作),是Redis要完整的保存整个数据集是一个比较繁重的工作,你通常会每隔5分钟或者更久做一次完整的保存,万一在Redis意外宕机,你可能会丢失几分钟的数据.
- RDB 需要经常fork子进程来保存数据集到硬盘上,当数据集比较大的时候,fork的过程是非常耗时的,可能会导致Redis在一些毫秒级内不能响应客户端的请求.如果数据集巨大并且CPU性能不是很好的情况下,这种情况会持续1秒,AOF也需要fork,但是你可以调节重写日志文件的频率来提高数据集的耐久度.
- 基于快照每次读写都是全部数据，当数据量巨大时，效率低下。

##### 解决弊端思路：

- 不写全数据，仅记录部分数据 
- 降低区分数据是否改变的难度，改记录数据为记录操作过程 
- 对所有操作均进行记录，排除丢失数据的风险



##### 启动方式 —— save指令

save : 用于手动执行一次保存操作

<font color="red">save指令的执行会阻塞当前Redis服务器，直到当前RDB过程完成为止，有可能会造成长时间阻塞，线上环境不建议使用。</font>

数据量过大，单线程执行方式造成效率过低时使用：

bgsave : 手动启动后台保存操作。但不是立即执行。



#### AOF

![img](http://img2.tuicool.com/YrqaY3f.png!web?_=6182478)

以独立日志的方式记录每次写命令，重启时再重新执行AOF文件中命令 达到恢复数据的目的。与RDB相比可以简单描述为改记录数据为记录数据产生的过程 （主流方式：具有实时性）

##### 优点

- 使用AOF 会让你的Redis更加耐久: 你可以使用不同的fsync策略：无fsync,每秒fsync,每次写的时候fsync.使用默认的每秒fsync策略,Redis的性能依然很好(fsync是由后台线程进行处理的,主线程会尽力处理客户端请求),一旦出现故障，你最多丢失1秒的数据.
- AOF文件是一个只进行追加的日志文件,所以不需要写入seek,即使由于某些原因(磁盘空间已满，写的过程中宕机等等)未执行完整的写入命令,你也也可使用redis-check-aof工具修复这些问题.
- Redis 可以在 AOF 文件体积变得过大时，自动地在后台对 AOF 进行重写： 重写后的新 AOF 文件包含了恢复当前数据集所需的最小命令集合。 整个重写操作是绝对安全的，因为 Redis 在创建新 AOF 文件的过程中，会继续将命令追加到现有的 AOF 文件里面，即使重写过程中发生停机，现有的 AOF 文件也不会丢失。 而一旦新 AOF 文件创建完毕，Redis 就会从旧 AOF 文件切换到新 AOF 文件，并开始对新 AOF 文件进行追加操作。
- AOF 文件有序地保存了对数据库执行的所有写入操作， 这些写入操作以 Redis 协议的格式保存， 因此 AOF 文件的内容非常容易被人读懂， 对文件进行分析（parse）也很轻松。 导出（export） AOF 文件也非常简单： 举个例子， 如果你不小心执行了 FLUSHALL 命令， 但只要 AOF 文件未被重写， 那么只要停止服务器， 移除 AOF 文件末尾的 FLUSHALL 命令， 并重启 Redis ， 就可以将数据集恢复到 FLUSHALL 执行之前的状态。

##### 缺点弊端

- 对于相同的数据集来说，AOF 文件的体积通常要大于 RDB 文件的体积。
- 根据所使用的 fsync 策略，AOF 的速度可能会慢于 RDB 。 在一般情况下， 每秒 fsync 的性能依然非常高， 而关闭 fsync 可以让 AOF 的速度和 RDB 一样快， 即使在高负荷之下也是如此。 不过在处理巨大的写入载入时，RDB 可以提供更有保证的最大延迟时间（latency）。

##### 解决弊端思路：

- 不写全数据，仅记录部分数据 
- 降低区分数据是否改变的难度，改记录数据为记录操作过程 
- 对所有操作均进行记录，排除丢失数据的风险

##### AOF重写

- always(每次） 每次写入操作均同步到AOF文件中，数据零误差，性能较低 

- everysec（每秒） 每秒将缓冲区中的指令同步到AOF文件中，数据准确性较高，性能较高 在系统突然宕机的情况下丢失1秒内的数据 

- no（系统控制） 由操作系统控制每次同步到AOF文件的周期，整体过程不可控	

  [![Bqz6iV.png](https://s1.ax1x.com/2020/11/10/Bqz6iV.png)](https://imgchr.com/i/Bqz6iV)

**AOF缓冲区同步文件策略，由参数appendfsync控制 系统调用write和fsync说明：**

- write操作会触发延迟写（delayed write）机制，Linux在内核提供页缓冲区用来提高硬盘IO性能。write操作在写入系统缓冲区后直接返回。同步硬盘操作依赖于系统调度机制，列如：缓冲区页空间写满或达到特定时间周期。同步文件之前，如果此时系统故障宕机，缓冲区内数据将丢失。
- fsync针对单个文件操作（比如AOF文件），做强制硬盘同步，fsync将阻塞知道写入硬盘完成后返回，保证了数据持久化。

#### RDB与AOF常用配置

##### RDB持久化配置

Redis会将数据集的快照dump到dump.rdb文件中。此外，我们也可以通过配置文件来修改Redis服务器dump快照的频率，在打开6379.conf文件之后，我们搜索save，可以看到下面的配置信息：

> save 900 1              #在900秒(15分钟)之后，如果至少有1个key发生变化，则dump内存快照。
>
> save 300 10            #在300秒(5分钟)之后，如果至少有10个key发生变化，则dump内存快照。
>
> save 60 10000        #在60秒(1分钟)之后，如果至少有10000个key发生变化，则dump内存快照。

##### AOF持久化配置

在Redis的配置文件中存在三种同步方式，它们分别是：

> appendfsync always     #每次有数据修改发生时都会写入AOF文件。
>
> appendfsync everysec  #每秒钟同步一次，该策略为AOF的缺省策略。
>
> appendfsync no          #从不同步。高效但是数据不会被持久化

#### RDB与AOF的选择

一般来说， 如果想达到足以媲美 PostgreSQL 的数据安全性， 你应该同时使用两种持久化功能。

**如果你非常关心你的数据， 但仍然可以承受数分钟以内的数据丢失， 那么你可以只使用 RDB 持久化。**

**有很多用户都只使用 AOF 持久化， 但我们并不推荐这种方式： 因为定时生成 RDB 快照（snapshot）非常便于进行数据库备份， 并且 RDB 恢复数据集的速度也要比 AOF 恢复的速度要快， 除此之外， 使用 RDB 还可以避免之前提到的 AOF 程序的 bug 。**

- RDB与AOF的选择实际上是在做一种权衡，每种都有利有弊
- 如不能承受数分钟以内的数据丢失，对业务数据非常敏感，选用AOF 
- 如能承受数分钟以内的数据丢失，且追求大数据集的恢复速度，选用RDB 
- 灾难恢复选用RDB 
- 双保险策略，同时开启 RDB 和 AOF，重启后，Redis优先使用 AOF 来恢复数据，降低丢失数据的量

##### 如何从RDB方式切换为AOF方式：

在 Redis 2.2 或以上版本，可以在不重启的情况下，从 RDB 切换到 AOF ：

- 为最新的 dump.rdb 文件创建一个备份。
- 将备份放到一个安全的地方。
- 执行以下两条命令:
- redis-cli config set appendonly yes
- redis-cli config set save “”
- 确保写命令会被正确地追加到 AOF 文件的末尾。
- 执行的第一条命令开启了 AOF 功能： Redis 会阻塞直到初始 AOF 文件创建完成为止， 之后 Redis 会继续处理命令请求， 并开始将写入命令追加到 AOF 文件末尾。

执行的第二条命令用于关闭 RDB 功能。 这一步是可选的， 如果你愿意的话， 也可以同时使用 RDB 和 AOF 这两种持久化功能。

<font color="red">重要:别忘了在 redis.conf 中打开 AOF 功能！ 否则的话， 服务器重启之后， 之前通过 CONFIG SET 设置的配置就会被遗忘， 程序会按原来的配置来启动服务器。</font>

[参考RDB与AOF的区别](https://www.cnblogs.com/zxs117/p/11242026.html)

### Redis数据删除测略

在内存占用与CPU占用之间寻找一种平衡，顾此失彼都会造成整体redis性能的下降，甚至引发服务器宕机或内存泄露

#### 1.定时删除

- 创建一个定时器，当key设置有过期时间，且过期时间到达时，由定时器任务立即执行对键的删除操作
- 优点：节约内存，到时就删除，快速释放掉不必要的内存占用
- 缺点：CPU压力很大，无论CPU此时负载量多高，均占用CPU，会影响redis服务器响应时间和指令吞吐量
- 总结：用处理器性能换取存储空间（拿时间换空间）

#### 2.惰性删除

- 数据到达过期时间，不做处理。等下次访问该数据时
- 如果未过期，返回数据
- 发现已过期，删除，返回不存在
- 优点：节约CPU性能，发现必须删除的时候才删除
- 缺点：内存压力很大，出现长期占用内存的数据
- 总结：用存储空间换取处理器性能（拿时间换空间）

#### 3.定期删除

- Redis启动服务器初始化时，读取配置server.hz的值，默认为10
- 每秒钟执行server.hz次serverCron() -->databaseCron()--->activeExpireCycle()
- activeExpireCycle()对每个expires[*]逐一进行检测，每次执行250ms/server.hz
- 对某个expires[*]检测时，随机挑选W个key检测*
  - 如果key超时，删除key
  - *如果一轮中删除的key的数量>W*25%，循环该过程
  - 如果一轮中删除的key的数量≤W*25%，检查下一个expires[*]，0-15循环
  - W取值=ACTIVE_EXPIRE_CYCLE_LOOKUPS_PER_LOOP属性值
- 参数current_db用于记录activeExpireCycle() 进入哪个expires[*] 执行
- 如果activeExpireCycle()执行时间到期，下次从current_db继续向下执行
  - ​		

#### 删除测略对比

| 定时删除 | 节约内存，无暂用 | 不分时段占用CPU资源，频度高   | 拿时间换空间       |
| -------- | ---------------- | ----------------------------- | ------------------ |
| 惰性删除 | 内存占用严重     | 延时执行，CPU利用率高         | 拿空间换时间       |
| 定期删除 | 内存定期随机清理 | 每秒花费固定的CPU资源维护内存 | 随机抽查，重点抽查 |

### Redis数据逐出测略

**当新数据进入Redis时如果内存不足怎么办？**

Redis使用内存存储数据，在执行每一个命令前，会调用freeMemoryIfNeeded()检测内存是否充足。如果内存不满足新加入数据的最低存储要求，redis要临时删除一些数据为当前指令清理存储空间。清理数据的策略称为逐出算法。

注意：逐出数据的过程不是100%能够清理出足够的可使用的内存空间，如果不成功则反复执行。当对所有数据尝试完毕后，如果不能达到内存清理的要求，将出现错误信息。

##### 相关配置

- 检测易失数据（可能会过期的数据集server.db[i].expires ）
  - ① volatile-lru：挑选最近最少使用的数据淘汰
  - ② volatile-lfu：挑选最近使用次数最少的数据淘汰
  - ③ volatile-ttl：挑选将要过期的数据淘汰
  - ④ volatile-random：任意选择数据淘汰
- 检测全库数据（所有数据集server.db[i].dict ）
  - ⑤ allkeys-lru：挑选最近最少使用的数据淘汰
  - ⑥ allkeys-lfu：挑选最近使用次数最少的数据淘汰
  - ⑦ allkeys-random：任意选择数据淘汰
- 放弃数据驱逐
  - ⑧ no-enviction（驱逐）：禁止驱逐数据（redis4.0中默认策略），会引发错误OOM（Out Of Memory）

### <font color="red"> 使用Redis缓存出现的常见问题：</font>

#### 缓存的处理流程

[![BoQiwD.png](https://s1.ax1x.com/2020/11/08/BoQiwD.png)](https://imgchr.com/i/BoQiwD)

#### 缓存雪崩

​		目前电商首页以及热点数据都会去做缓存 ，一般缓存都是定时任务去刷新，或者是查不到之后去更新的，定时任务刷新就有一个问题。

 		当缓存服务器重启或者大量缓存集中在某一个时间段失效，这样在失效的时候，会给后端系统带来很大压 力。导致系统崩溃。

**如何避免?**

 1：在缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量。比如对某个 key 只允许一个 线程查询数据和写缓存，其他线程等待。

 2：做二级缓存，A1 为原始缓存，A2 为拷贝缓存，A1 失效时，可以访问 A2，A1 缓存失效时间设置 为短期，A2 设置为长期 

 3：不同的 key，设置不同的过期时间，让缓存失效的时间点尽量均匀 

#### 缓存穿透

 一般的缓存系统，都是按照 key 去缓存查询，如果不存在对应的 value，就应该去后端系统查找（比如  DB）。一些恶意的请求会故意查询不存在的 key,请求量很大，就会对后端系统造成很大的压力。这就叫  做缓存穿透。 

**如何避免?**

 1：对查询结果为空的情况也进行缓存，缓存时间设置短一点，或者该key 对应的数据 insert 了之后清 理缓存。 

 2：对一定不存在的 key 进行过滤。可以把所有的可能存在的 key 放到一个大的 Bitmap 中，查询时通过 该 bitmap 过滤。 

`缓存穿透`我会在接口层增加校验，比如用户鉴权校验，参数做校验，不合法的参数直接代码Return，比如：id 做基础校验，id <=0的直接拦截等。

#### 缓存击穿

 缓存击穿是指缓存中没有但数据库中有的数据（一般是缓存时间到期），这时由于并发用户特别多，同时读缓存没读到数据，又同时去数据库去取数据，引起数据库压力瞬间增大，造成过大压力

​      **解决方案：**

1. 设置热点数据永远不过期。<br/>
2. 加互斥锁，互斥锁参考代码如下：

##### 使用布隆过滤器解决缓存击穿

`布隆过滤器（Bloom Filter）`这个也能很好的防止缓存穿透的发生，他的原理也很简单就是利用高效的数据结构和算法快速判断出你这个Key是否在数据库中存在，不存在你return就好了，存在你就去查了DB刷新KV再return。

`缓存击穿`的话，设置热点数据永远不过期。或者加上互斥锁就能搞定了

#### 缓存与数据库双写不一致

一般来说，如果允许缓存可以稍微的跟数据库偶尔有不一致的情况，也就是说如果你的系统不是严格要求 “缓存+数据库” 必须保持一致性的话，最好不要做这个方案，即：读请求和写请求串行化，串到一个内存队列里去。

串行化可以保证一定不会出现不一致的情况，但是它也会导致系统的吞吐量大幅度降低，用比正常情况下多几倍的机器去支撑线上的一个请求。

Cache Aside Pattern

最经典的缓存+数据库读写的模式，就是 Cache Aside Pattern。

- 读的时候，先读缓存，缓存没有的话，就读数据库，然后取出数据后放入缓存，同时返回响应。
- 更新的时候，先更新数据库，然后再删除缓存。

 

为什么是删除缓存，而不是更新缓存？

原因很简单，很多时候，在复杂点的缓存场景，缓存不单单是数据库中直接取出来的值。

比如可能更新了某个表的一个字段，然后其对应的缓存，是需要查询另外两个表的数据并进行运算，才能计算出缓存最新的值的。

**1、最初级的缓存不一致问题以及解决方案**

**问题：**

​	先修改数据库，再删除缓存，如果删除缓存失败了，那么会导致数据库中是新数据，缓存中是旧数据，数据出现不一致。

**解决思路：**

　　先删除缓存，再修改数据库，如果删除缓存成功了修改数据库失败了，那么数据库中是旧数据，缓存中是空的，那么数据不会不一致，因为读的时候缓存没有，则读数据库中旧数据，然后更新到缓存中。

　　[![BoYXxH.md.png](https://s1.ax1x.com/2020/11/08/BoYXxH.md.png)](https://imgchr.com/i/BoYXxH)

其实还有以下解决方案：

1. 先删缓存，再更新数据库
2. 先更新数据库，再删缓存
3. 缓存延时双删，更新前先删除缓存，然后更新数据，再延时删除缓存
4. 监听MySQL binlog进行缓存更新

[可参考：缓存与数据库双写一致性最佳解决方案](https://www.jianshu.com/p/dc1e5091a0d8)

**2、并发下数据缓存不一致问题分析**

**问题**：

　　第一个请求数据发生变更，先删除了缓存，然后要去修改数据库，此时还没来得及去修改；

　　第二个请求过来去读缓存，发现缓存空了，去查询数据库，查到了修改前的旧数据，放到了缓存中；

　　第三个请求读取缓存中的数据 (此时第一个请求已经完成了数据库修改的操作)。

　　完了，数据库和缓存中的数据不一样了。。。。

**问题分析：**

　　只有在对同一条数据并发读写的时候，才可能会出现这种问题。其实如果说你的并发量很低的话，特别是读并发很低，每天访问量就1万次，那么很少的情况下，会出现刚才描述的那种不一致的场景;但如果每天的是上亿的流量，每秒并发读是几万，每秒只要有数据更新的请求，就可能会出现上述的数据库+缓存不一致的情况。

**解决思路**

　　数据库的缓存更新与读取操作进行串行化，一个队列对应一个工作线程，每个工作线程串行拿到对应的操作，然后一条一条的执行。

1. 首先我们的项目里维护一组线程池和内存队列。
2. 更新数据的时候，根据数据的唯一标识将请求路由到一个jvm队列中，去更新数据库,然后请求结束。
3. 读取数据的时候，先查缓存，如果发现数据不在缓存中，那么将根据唯一标识路由之后，也发送同一个jvm内部的队列中，重新读取数据库后更新缓存,最后请求结束。

[![BoYLGD.md.png](https://s1.ax1x.com/2020/11/08/BoYLGD.md.png)　　

​		这里有一个需要优化的点，比如一个队列中，连续存在多个更新缓存请求串在一起是没意义的，这样重复的查询数据库并更新缓存的操作应该优化：如果发现队列中已经有一个更新缓存的请求了，那么就不用再放个更新请求操作进去了，直接让后面的读请求阻塞个200ms左右(这里只是举个例子，实际值可以根据服务的响应时间和机器的处理能力来计算)，然后再次查询缓存，如果缓存没有值就查数据库，拿到结果后不用更新缓存，直接返回给页面即可。

[参考：缓存与数据库双写不一致](https://www.cnblogs.com/wlwl/p/11601632.html)

#### 缓存并发竞争key

其实这个问题就是<font color="red">多个客户端同时并发写一个key可能本来应该先到的数据后到了，导致数据版本错了</font>；或者是多客户端同时获取一个 key，修改值之后再写回去，只要顺序错了，数据就错了。

而且 redis 自己就有天然解决这个问题的 CAS 类的乐观锁方案。

**解决方案**

你要写入缓存的数据，都是从 mysql 里查出来的，都得写入 mysql 中，写入 mysql 中的时候必须保存一个时间戳，从 mysql 查出来的时候，时间戳也查出来。

每次要**写之前，先判断一下**当前这个 value 的时间戳是否比缓存里的 value 的时间戳要新。如果是的话，那么可以写，否则，就不能用旧的数据覆盖新的数据。

##### 解决方案(一) 分布式锁+时间戳

**1.整体技术方案**

这种情况，主要是准备一个分布式锁，大家去抢锁，抢到锁就做set操作。

加锁的目的实际上就是把并行读写改成串行读写的方式，从而来避免资源竞争。

**2.Redis分布式锁的实现**

主要用到的redis函数是setnx()

用SETNX实现分布式锁 使用setnx设置一个公共锁

​	`setnx lock-key value`

利用SETNX非常简单地实现分布式锁。例如：某客户端要获得一个名字youzhi的锁，客户端使用下面的命令进行获取：

SETNX lock.youzhi<current Unix time + lock timeout + 1>

如返回1，则该客户端获得锁，把lock.youzhi的键值设置为时间值表示该键已被锁定，该客户端最后可以通过DEL lock.foo来释放该锁。

如返回0，表明该锁已被其他客户端取得，这时我们可以先返回或进行重试等对方完成或等待锁超时。

**3.时间戳**

由于上面举的例子，要求key的操作需要顺序执行，所以需要保存一个时间戳判断set顺序。

系统A key 1 {ValueA 7:00}

系统B key 1 { ValueB 7:05}

假设系统B先抢到锁，将key1设置为{ValueB 7:05}。接下来系统A抢到锁，发现自己的key1的时间戳早于缓存中的时间戳（7:00<7:05），那就不做set操作了。

**4.什么是分布式锁**

因为传统的加锁的做法（如java的synchronized和Lock）这里没用，只适合单点。因为这是分布式环境，需要的是分布式锁。

当然，分布式锁可以基于很多种方式实现，比如zookeeper、redis等，不管哪种方式实现，基本原理是不变的：用一个状态值表示锁，对锁的占用和释放通过状态值来标识。

##### 解决方案(二) 利用消息队列

在并发量过大的情况下,可以通过消息中间件进行处理,把并行读写进行串行化。

把Redis.set操作放在队列中使其串行化,必须的一个一个执行。

这种方式在一些高并发的场景中算是一种通用的解决方案。

#### Redis持久化方案

AOF 和RDB上面已经陈述

#### 数据删除策略

**数据删除策略的目标：**
在内存占用与CPU之间寻找一种平衡，顾此失彼都会造成整体Redis性能的下降，甚至引发服务器宕机或者内存泄漏。

**定时删除**

- 创建一个定时器，当key设置有过期时间，且过期时间到达时，由定时器任务立即执行对键的删除操作
- 优点：节约内存，到期就删除，快速释放掉不必要的内存占用
- 缺点：CPU压力很大，无论CPU此时负载量多高，均占用CPU,会影响redis服务器响应时间和指令吞吐量
- 总结：用处理器性能换取存储空间(拿时间换空间)

**惰性删除**

- 数据到达过期时间，不做处理。等下次访问该数据时
  - 如果未过期，返回数据
  - 发现已过期，删除，返回不存在
- 优点：节约CPU性能，发现必须删除的时候才删除
- 缺点：内存压力很大，出现长期占用内存的数据
- 总结：用存储空间换取处理器性能

**定期删除**

- 周期性轮训redis库中的时效性数据，采用随机抽取的策略，利用过期数据占比的方式控制删除频度
- 特点1：CPU性能占用设置有峰值，检测频度可自定义设置
- 特点2：内存压力不是很大，长期占用内存的冷数据会被持续清理
- 总结：周期性抽查存储空间（随机抽查，重点抽查）

#### 数据逐出策略

**当新数据进入Redis时如果内存不足怎么办？**

Redis使用内存存储数据，在执行每一个命令前，会调用freeMemoryIfNeeded()检测内存是否充足。如果内存不满足新加入数据的最低存储要求，redis要临时删除一些数据为当前指令清理存储空间。清理数据的策略称为逐出算法。

注意：逐出数据的过程不是100%能够清理出足够的可使用的内存空间，如果不成功则反复执行。当对所有数据尝试完毕后，如果不能达到内存清理的要求，将出现错误信息。

### Jedis

Jedis是一个集成redis的一些操作的命令，封装了redis的 Java客户端，提供连接池的使用一般不直接使用jedis，而是在其上在封装一层，作为业务的使用。如果用spring的话，可以看看spring 封装的 redis Spring Data Redis。

[Jedis](https://link.jianshu.com/?t=https://github.com/xetorthio/jedis)是Redis官方推荐的Java连接开发工具。要在Java开发中使用好Redis中间件，必须对Jedis熟悉才能写成漂亮的代码

#### 基本使用

Jedis的基本使用非常简单，只需要创建Jedis对象的时候指定host，port, password即可。当然，Jedis对象又很多构造方法，都大同小异，只是对应和Redis连接的socket的参数不一样而已。

```java
Jedis jedis = new Jedis("localhost", 6379);  //指定Redis服务Host和port
jedis.auth("xxxx"); //如果Redis服务连接需要密码，制定密码
String value = jedis.get("key"); //访问Redis服务
 //设置 redis 字符串数据
jedis.set("runoobkey", "www.runoob.com");
// 获取存储的数据并输出
System.out.println("redis 存储的字符串为: "+ jedis.get("runoobkey"));
/**
*获取列表
*/
//存储数据到列表中
jedis.lpush("site-list", "Runoob");
jedis.lpush("site-list", "Google");
jedis.lpush("site-list", "Taobao");
// 获取存储的数据并输出
List<String> list = jedis.lrange("site-list", 0 ,2);
for(int i=0; i<list.size(); i++) {
    System.out.println("列表项为: "+list.get(i));
}
 // 获取数据并输出
Set<String> keys = jedis.keys("*"); 
Iterator<String> it=keys.iterator() ;   
while(it.hasNext()){   
    String key = it.next();   
    System.out.println(key);   
}
jedis.close(); //使用完关闭连接
```

在Jedis对象构建好之后，Jedis底层会打开一条Socket通道和Redis服务进行连接。所以在使用完Jedis对象之后，需要调用Jedis.close()方法把连接关闭，不如会占用系统资源。当然，如果应用非常平凡的创建和销毁Jedis对象，对应用的性能是很大影响的，因为构建Socket的通道是很耗时的(类似数据库连接)。我们应该使用连接池来减少Socket对象的创建和销毁过程。

#### jedis连接池使用

Jedis连接池是基于apache-commons pool2实现的。在构建连接池对象的时候，需要提供池对象的配置对象，及JedisPoolConfig(继承自GenericObjectPoolConfig)。我们可以通过这个配置对象对连接池进行相关参数的配置(如最大连接数，最大空数等)。

```csharp
JedisPoolConfig config = new JedisPoolConfig();
config.setMaxIdle(8);
config.setMaxTotal(18);
JedisPool pool = new JedisPool(config, "127.0.0.1", 6379, 2000, "password");
Jedis jedis = pool.getResource();
String value = jedis.get("key");
......
jedis.close();
pool.close();
```

使用Jedis连接池之后，在每次用完连接对象后一定要记得把连接归还给连接池。Jedis对close方法进行了改造，如果是连接池中的连接对象，调用Close方法将会是把连接对象返回到对象池，若不是则关闭连接。可以查看如下代码

```kotlin
@Override
public void close() { //Jedis的close方法
    if (dataSource != null) {
        if (client.isBroken()) {
            this.dataSource.returnBrokenResource(this);
        } else {
            this.dataSource.returnResource(this);
        }
    } else {
        client.close();
    }
}

//另外从对象池中获取Jedis链接时，将会对dataSource进行设置
// JedisPool.getResource()方法
public Jedis getResource() {
    Jedis jedis = super.getResource();   
    jedis.setDataSource(this);
    return jedis;
}
```



#### Redis在spring中使用：

```java
public interface JedisClient {
	String set(String key, String value);
	String get(String key);
	Boolean exists(String key);
	Long expire(String key, int seconds);
	Long ttl(String key);
	Long incr(String key);
	Long hset(String key, String field, String value);
	String hget(String key, String field);
	Long hdel(String key, String... field);
	Boolean hexists(String key, String field);
	List<String> hvals(String key);
	Long del(String key);
}

/**

- 集群版
- @author leoill
*TODO
 *2019年1月11日
 */
public class JedisClientCluster implements JedisClient {
  private JedisCluster jedisCluster;
  public JedisCluster getJedisCluster() {
	return jedisCluster;
}
  public void setJedisCluster(JedisCluster jedisCluster) {
	this.jedisCluster = jedisCluster;
}
  @Override
public String set(String key, String value) {
	return jedisCluster.set(key, value);
}
  @Override
public String get(String key) {
	return jedisCluster.get(key);
}
  @Override
public Boolean exists(String key) {
	return jedisCluster.exists(key);
}
  @Override
public Long expire(String key, int seconds) {
	return jedisCluster.expire(key, seconds);
}
  @Override
public Long ttl(String key) {
	return jedisCluster.ttl(key);
}
  @Override
public Long incr(String key) {
	return jedisCluster.incr(key);
}
  @Override
public Long hset(String key, String field, String value) {
	return jedisCluster.hset(key, field, value);
}
  @Override
public String hget(String key, String field) {
	return jedisCluster.hget(key, field);
}
  @Override
public Long hdel(String key, String... field) {
	return jedisCluster.hdel(key, field);
}
  @Override
public Boolean hexists(String key, String field) {
	return jedisCluster.hexists(key, field);
}
  @Override
public List<String> hvals(String key) {
	return jedisCluster.hvals(key);
}
  @Override
public Long del(String key) {
	return jedisCluster.del(key);
}

}
package com.leo.e3mall.common.jedis;

import java.util.List;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
/**

- 单机版
- @author leoill
*TODO
 *2019年1月11日
 */
public class JedisClientPool implements JedisClient {
  private JedisPool jedisPool;
  public JedisPool getJedisPool() {
	return jedisPool;
}
  public void setJedisPool(JedisPool jedisPool) {
	this.jedisPool = jedisPool;
}
  @Override
public String set(String key, String value) {
	Jedis jedis = jedisPool.getResource();
	String result = jedis.set(key, value);
	jedis.close();
	return result;
}
  @Override
public String get(String key) {
	Jedis jedis = jedisPool.getResource();
	String result = jedis.get(key);
	jedis.close();
	return result;
}
  @Override
public Boolean exists(String key) {
	Jedis jedis = jedisPool.getResource();
	Boolean result = jedis.exists(key);
	jedis.close();
	return result;
}
  @Override
public Long expire(String key, int seconds) {
	Jedis jedis = jedisPool.getResource();
	Long result = jedis.expire(key, seconds);
	jedis.close();
	return result;
}
  @Override
public Long ttl(String key) {
	Jedis jedis = jedisPool.getResource();
	Long result = jedis.ttl(key);
	jedis.close();
	return result;
}
  @Override
public Long incr(String key) {
	Jedis jedis = jedisPool.getResource();
	Long result = jedis.incr(key);
	jedis.close();
	return result;
}
  @Override
public Long hset(String key, String field, String value) {
	Jedis jedis = jedisPool.getResource();
	Long result = jedis.hset(key, field, value);
	jedis.close();
	return result;
}
  @Override
public String hget(String key, String field) {
	Jedis jedis = jedisPool.getResource();
	String result = jedis.hget(key, field);
	jedis.close();
	return result;
}
  @Override
public Long hdel(String key, String... field) {
	Jedis jedis = jedisPool.getResource();
	Long result = jedis.hdel(key, field);
	jedis.close();
	return result;
}
  @Override
public Boolean hexists(String key, String field) {
	Jedis jedis = jedisPool.getResource();
	Boolean result = jedis.hexists(key, field);
	jedis.close();
	return result;
}
  @Override
public List<String> hvals(String key) {
	Jedis jedis = jedisPool.getResource();
	List<String> result = jedis.hvals(key);
	jedis.close();
	return result;
}
  @Override
public Long del(String key) {
	Jedis jedis = jedisPool.getResource();
	Long result = jedis.del(key);
	jedis.close();
	return result;
}

}

```


**通过spring的bean的注入然后加载redis的配置文件：**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
		http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
<!-- 连接redis单机版 -->
<bean id="jedisClientPool" class="com.leo.e3mall.common.jedis.JedisClientPool">
	<property name="jedisPool" ref="jedisPool"></property>
</bean>
<bean id="jedisPool" class="redis.clients.jedis.JedisPool">
	<constructor-arg name="host" value="192.168.25.162"></constructor-arg>
	<constructor-arg name="port" value="6379"></constructor-arg>
</bean>
<!-- 连接redis集群版 -->
<bean id="jedisClientCluster" class="com.leo.e3mall.common.jedis.JedisClientCluster">
	<property name="jedisCluster" ref="jedisCluster"></property>
</bean>
<bean id="jedisCluster" class="redis.clients.jedis.JedisCluster">
	<constructor-arg name="nodes">
		<set>
			<bean class="redis.clients.jedis.HostAndPort">
				<constructor-arg name="host" value="192.168.25.162"></constructor-arg>
				<constructor-arg name="port" value="7001"></constructor-arg>
			</bean>
			<bean class="redis.clients.jedis.HostAndPort">
				<constructor-arg name="host" value="192.168.25.162"></constructor-arg>
				<constructor-arg name="port" value="7002"></constructor-arg>
			</bean>
			<bean class="redis.clients.jedis.HostAndPort">
				<constructor-arg name="host" value="192.168.25.162"></constructor-arg>
				<constructor-arg name="port" value="7003"></constructor-arg>
			</bean>
			<bean class="redis.clients.jedis.HostAndPort">
				<constructor-arg name="host" value="192.168.25.162"></constructor-arg>
				<constructor-arg name="port" value="7004"></constructor-arg>
			</bean>
			<bean class="redis.clients.jedis.HostAndPort">
				<constructor-arg name="host" value="192.168.25.162"></constructor-arg>
				<constructor-arg name="port" value="7005"></constructor-arg>
			</bean>
			<bean class="redis.clients.jedis.HostAndPort">
				<constructor-arg name="host" value="192.168.25.162"></constructor-arg>
				<constructor-arg name="port" value="7006"></constructor-arg>
			</bean>
		</set>
	</constructor-arg>
</bean>
</beans>
```

#### Redis在springboot中的使用

需要使用Redis，可在工程的Maven配置中加入spring-boot-starter-redis依赖，其中gson是用来转换Json数据格式的工具，mysql是引用了上一节的模块，演示在Redis中的存取操作。

##### 引入依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-redis</artifactId>
    </dependency>
    <dependency>
        <groupId>com.google.code.gson</groupId>
        <artifactId>gson</artifactId>
        <version>2.2.4</version>
    </dependency>
    <dependency>
        <groupId>springboot.db</groupId>
        <artifactId>mysql</artifactId>
        <version>${project.version}</version>
    </dependency>
</dependencies>
```

##### 创建Redis服务类

```java
@Repository
public class UserRedis {
    @Autowired
    private RedisTemplate<String, String> redisTemplate;
    public void add(String key, Long time,User user) {
        Gson gson = new Gson();
        redisTemplate.opsForValue().set(key, gson.toJson(user), time, TimeUnit.
MINUTES);
    }
    public void add(String key, Long time, List<User> users) {
        Gson gson = new Gson();
        redisTemplate.opsForValue().set(key, gson.toJson(users), time, TimeUnit.
MINUTES);
    }
    public User get(String key) {
        Gson gson = new Gson();
        User user = null;
        String userJson = redisTemplate.opsForValue().get(key);
        if(!StringUtils.isEmpty(userJson))
            user = gson.fromJson(userJson, User.class);
        return user;
    }
    public List<User> getList(String key) {
        Gson gson = new Gson();
        List<User> ts = null;
        String listJson = redisTemplate.opsForValue().get(key);
        if(!StringUtils.isEmpty(listJson))
            ts = gson.fromJson(listJson, new TypeToken<List<User>>(){}.getType());
        return ts;
    }
    public void delete(String key){
        redisTemplate.opsForValue().getOperations().delete(key);
    }
}
```

Redis没有表结构的概念，所以要实现MySQL数据库中表的数据（即普通Java对象映射的实体数据）在Redis中存取，必须做一些转换，使用JSON格式的文本作为Redis与Java普通对象互相交换数据的存储格式。这里使用Gson工具将类对象转换为JSON格式的文本进行存储，要取出数据时，再将JSON文本数据转化为Java对象。<br/>
因为Redis使用了key-value的方式存储数据，所以存入时要生成一个唯一的key，而要查询或者删除数据时，就可以使用这个唯一的key进行相应的操作。<br/>
保存在Redis数据库中的数据默认是永久存储的，可以指定一个时限来确定数据的生命周期，超过指定时限的数据将被Redis自动清除。<br/>
另外，为了能正确调用RedisTemplate，必须对其进行一些初始化工作，即主要对它存取的字符串进行一个JSON格式的系列化初始配置。

##### RedisTemplate初始化

```java
@Configuration
public class RedisConfig {
    @Bean
    public RedisTemplate<String, String> redisTemplate(
            RedisConnectionFactory factory) {
        StringRedisTemplate template = new StringRedisTemplate(factory);
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        template.setValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();
        return template;
    }
}
```

##### 封装使用缓存RedisTemplate:

RedisTemplate实现了对Redis的调用。这种方式可以很方便地对列表对象进行系列化，在数据存取时使用Json进行格式转换。这里使用分钟作为时间单位来设定数据在Redis中保存的有效期限。

```java
@Repository
public class UserRedis {
    @Autowired
    private RedisTemplate<String, String> redisTemplate;
    public void add(String key, Long time, User user) {
        Gson gson = new Gson();
        redisTemplate.opsForValue().set(key, gson.toJson(user), time, TimeUnit.
MINUTES);
    }
    public void add(String key, Long time, List<User> users) {
        Gson gson = new Gson();
        redisTemplate.opsForValue().set(key, gson.toJson(users), time, TimeUnit.
MINUTES);
    }
    public User get(String key) {
        Gson gson = new Gson();
        User user = null;
        String json = redisTemplate.opsForValue().get(key);
        if(!StringUtils.isEmpty(json))
            user = gson.fromJson(json, User.class);
        return user;
    }
    public List<User> getList(String key) {
        Gson gson = new Gson();
        List<User> ts = null;
        String listJson = redisTemplate.opsForValue().get(key);
        if(!StringUtils.isEmpty(listJson))
            ts = gson.fromJson(listJson, new TypeToken<List<User>>(){}.getType());
        return ts;
    }
    public void delete(String key){
        redisTemplate.opsForValue().getOperations().delete(key);
    }
}
```

然后编写使用Redis缓存。即在使用原来数据库的增删查改过程中，同时使用Redis进行辅助存取，以达到提升访问速度的目的，从而缓解对原来数据库的访问压力。这样，访问一条数据时，首先从Redis读取，如果存在则不再到MySQL中读取，如果不存在再到MySQL读取，并将读取的结果暂时保存在Redis中。

##### 在数据服务中使用Redis作为辅助缓存

```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    @Autowired
    private UserRedis userRedis;
    private static final String keyHead = "mysql:get:user:";
    public User findById(Long id) {
        User user = userRedis.get(keyHead + id);
        if(user == null){
            user = userRepository.findOne(id);
            if(user != null)
                userRedis.add(keyHead + id, 30L, user);
        }
        return user;
    }
    public User create(User user) {
        User newUser = userRepository.save(user);
        if(newUser != null)
            userRedis.add(keyHead + newUser.getId(), 30L, newUser);
        return newUser;
    }
    public User update(User user) {
        if(user != null) {
            userRedis.delete(keyHead + user.getId());
            userRedis.add(keyHead + user.getId(), 30L, user);
        }
        return userRepository.save(user);
    }
    public void delete(Long id) {
        userRedis.delete(keyHead + id);
        userRepository.delete(id);
    }
```

上面使用Redis缓存的两种方法，可以在一个应用中混合使用。但不管怎么使用，对于控制器来说都是完全透明的，控制器对数据接口的调用还是像以前一样，它并不清楚数据接口后端是否启用了缓存，如代码清单4-16所示。

##### 控制器使用数据接口service实现

```java
@Autowired
private UserService userService;
 @RequestMapping(value="/{id}")
public String show(ModelMap model,@PathVariable Long id) {
User user = userService.findById(id);
model.addAttribute("user",user);
return "user/show";
}

```

使用缓存之后，大量的查询语句就从原来的数据库服务器中，转移到了高效的Redis服务器中执行，这将在很大程度上减轻原来数据库服务器的压力，并且提升查询的反应速度和效率。所以在很大的程度上，系统性能就得到了很好的改善。

