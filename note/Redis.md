## Redis

​	NoSQL : not only sql。非关系性数据库；解决大规模的数据集合带来的难题，尤其是大数据应用难题。<br/>

​			nosql的产品：key-value 形式；列储存数据库，文档性的数据库，图形数据库，易扩展，大数据流量，高性能，灵活的数据模型，高可用。<br/>

​	Redis:是一个高性能的key-value的数据库，支持数据的持久化，提供不同类型的数据结构存储，支持数据备份.支持以下类型：字符型，散列型，列表型list，集合型set，有序集合型sort set。

​	Redis有着更为复杂的数据结构并且提供对他们的原子性操作，这是一个不同于其他数据库的进化路径。Redis的数据类型都是基于基本数据结构的同时对程序员透明，无需进行额外的抽象。

​	Redis运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时需要权衡内存，因为数据量不能大于硬件内存。在内存数据库方面的另一个优点是，相比在磁盘上相同的复杂的数据结构，在内存中操作起来非常简单，这样Redis可以做很多内部复杂性很强的事情。同时，在磁盘格式方面他们是紧凑的以追加的方式产生的，因为他们并不需要进行随机访问。



### 使用redis的目的：	

​	在项目中主要用来用作数据的缓存，将数据缓存在redis中，减轻对底层数据库的访问压力，获得更高的并发和更快的请求响应速度

使用缓存之后，大量的查询语句就从原来的数据库服务器中，转移到了高效的Redis服务器中执行，这将在很大程度上减轻原来数据库服务器的压力，并且提升查询的反应速度和效率。所以在很大的程度上，系统性能就得到了很好的改善。

（1）高性能

​		比如说有一个很复杂的sql数据查询，这个查询要耗费大量的时间，如果每次都直接取数据查询，那必然会对请求响应时间造成很大的影响，如果能在第一次查询完毕之后，将其直接保存在缓存当中，下次查询的时候，直接在缓存中拿走现成的数据，这样就会大大缩短请求的响应时间。

（2）高并发

​		我们知道数据库能承受的并发是有限的，那么在流量高峰期(比如，抢购、打折、秒杀等等)，会有大量的请求进入我们的系统，比如查询某个商品的详情，如果我们没有缓冲，那么给次查询都要走数据库，假如我们的数据库每秒只能接受2000个请求，结果一秒钟进来了5000个请求，那么数据库就直接崩掉了，毫无高并发可言，而如果我们中间具有缓存服务，那么在第一个用户查询商品详情时(或者提前将放好)我们可以直接将商品的详情信息数据放到缓存里面，这样在后续用户查询时就可以直接走缓存，不走数据库，缓存是基于内存的，它的访问速度快，并发高；因此就可以提供一个高并发的支持。

### linux下安装redis:

```
$ wget http://download.redis.io/releases/redis-2.8.17.tar.gz
$ tar xzf redis-2.8.17.tar.gz
$ cd redis-2.8.17
$ make
```

make完后 redis-2.8.17目录下会出现编译后的redis服务程序redis-server,还有用于测试的客户端程序redis-cli,两个程序位于安装目录 src 目录下：

**启动redis服务：**

```
下面启动redis服务.
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



### Redis数据类型：

​		**Redis的数据类型包括五种基本的数据类型。`String字符串`, `hash字典`，`list列表`，`set集合`，`zset有序集合` 下面依次介绍使用**

####  String字符串：

​		redis的String可以包含任何的数据类型如jpg,序列化队形

​				 set name "lkl" 

​				 get name 可以获得刚输入的值

#### Hash:是一个键值对key->value 集合

​			HMSet myhash field1 "hello" filed2 "redis" 

​			获取设置的值：HGET myhash field1    输出的结果为hello

##### 使用场景：		

​	场景：`存储，读取，修改，用户属性`，

#### List（列表）

​			Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。 列表最多可存储 232 - 1 元素 (4294967295, 每个列表可存储40多亿)。<br/>

##### 使用场景：

- 最新消息排行等功能(比如朋友圈的时间线)
-  消息队列

```
实例
redis 127.0.0.1:6379> lpush runoob redis
(integer) 1
redis 127.0.0.1:6379> lpush runoob mongodb
(integer) 2
redis 127.0.0.1:6379> lpush runoob rabitmq
(integer) 3
redis 127.0.0.1:6379> lrange runoob 0 10
1) "rabitmq"
2) "mongodb"
3) "redis"
redis 127.0.0.1:6379>
				
```

#### Set（集合）Redis的Set是string类型的无序集合。

​		集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。

​		`sadd 命令`

​		添加一个 string 元素到 key 对应的 set 集合中，成功返回1，如果元素已经在集合中返回 0，如果 key 对应的 set 不存在则返回错误。
​		`sadd key member`

​		集合中最大的成员数为 232 - 1(4294967295, 每个集合可存储40多亿个成员)。

##### 使用场景：

- 共同好友 
- 利用唯一性,统计访问网站的所有独立ip 
- 好友推荐时,根据tag求交集,大于某个阈值就可以推荐

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

#### zset(sorted set：有序集合)

​		Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。zset的成员是唯一的,但分数(score)却可以重复。

​		`zadd 命令`  添加元素到集合，元素在集合中存在则更新对应score

##### 使用场景：

- 1、排行榜 
- 2、带权重的消息队列

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



### Redis命令

#### Redis key操作的命令

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

#### RDB![img](http://img1.tuicool.com/NjYjYvF.png!web?_=6182478)



##### **优点**

- RDB是一个非常紧凑的文件,它保存了某个时间点得数据集,非常适用于数据集的备份,比如你可以在每个小时报保存一下过去24小时内的数据,同时每天保存过去30天的数据,这样即使出了问题你也可以根据需求恢复到不同版本的数据集.
- RDB是一个紧凑的单一文件,很方便传送到另一个远端数据中心或者亚马逊的S3（可能加密），<font color="red">非常适用于灾难恢复.</font>
- RDB在保存RDB文件时父进程唯一需要做的就是fork出一个子进程,接下来的工作全部由子进程来做，父进程不需要再做其他IO操作，所以RDB持久化方式可以最大化redis的性能.
- 与AOF相比,在恢复大的数据集的时候，RDB方式会更快一些.

##### 缺点

- 如果你希望在redis意外停止工作（例如电源中断）的情况下丢失的数据最少的话，那么RDB不适合你.虽然你可以配置不同的save时间点(例如每隔5分钟并且对数据集有100个写的操作),是Redis要完整的保存整个数据集是一个比较繁重的工作,你通常会每隔5分钟或者更久做一次完整的保存,万一在Redis意外宕机,你可能会丢失几分钟的数据.
- RDB 需要经常fork子进程来保存数据集到硬盘上,当数据集比较大的时候,fork的过程是非常耗时的,可能会导致Redis在一些毫秒级内不能响应客户端的请求.如果数据集巨大并且CPU性能不是很好的情况下,这种情况会持续1秒,AOF也需要fork,但是你可以调节重写日志文件的频率来提高数据集的耐久度.

#### AOF

![img](http://img2.tuicool.com/YrqaY3f.png!web?_=6182478)

##### 优点

- 使用AOF 会让你的Redis更加耐久: 你可以使用不同的fsync策略：无fsync,每秒fsync,每次写的时候fsync.使用默认的每秒fsync策略,Redis的性能依然很好(fsync是由后台线程进行处理的,主线程会尽力处理客户端请求),一旦出现故障，你最多丢失1秒的数据.
- AOF文件是一个只进行追加的日志文件,所以不需要写入seek,即使由于某些原因(磁盘空间已满，写的过程中宕机等等)未执行完整的写入命令,你也也可使用redis-check-aof工具修复这些问题.
- Redis 可以在 AOF 文件体积变得过大时，自动地在后台对 AOF 进行重写： 重写后的新 AOF 文件包含了恢复当前数据集所需的最小命令集合。 整个重写操作是绝对安全的，因为 Redis 在创建新 AOF 文件的过程中，会继续将命令追加到现有的 AOF 文件里面，即使重写过程中发生停机，现有的 AOF 文件也不会丢失。 而一旦新 AOF 文件创建完毕，Redis 就会从旧 AOF 文件切换到新 AOF 文件，并开始对新 AOF 文件进行追加操作。
- AOF 文件有序地保存了对数据库执行的所有写入操作， 这些写入操作以 Redis 协议的格式保存， 因此 AOF 文件的内容非常容易被人读懂， 对文件进行分析（parse）也很轻松。 导出（export） AOF 文件也非常简单： 举个例子， 如果你不小心执行了 FLUSHALL 命令， 但只要 AOF 文件未被重写， 那么只要停止服务器， 移除 AOF 文件末尾的 FLUSHALL 命令， 并重启 Redis ， 就可以将数据集恢复到 FLUSHALL 执行之前的状态。

##### 缺点

- 对于相同的数据集来说，AOF 文件的体积通常要大于 RDB 文件的体积。
- 根据所使用的 fsync 策略，AOF 的速度可能会慢于 RDB 。 在一般情况下， 每秒 fsync 的性能依然非常高， 而关闭 fsync 可以让 AOF 的速度和 RDB 一样快， 即使在高负荷之下也是如此。 不过在处理巨大的写入载入时，RDB 可以提供更有保证的最大延迟时间（latency）。

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

用SETNX实现分布式锁

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

