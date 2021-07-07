

## Appium+python自动化测试 

本文用于android和python自动化测试学习使用其中有的部分是博客收集，感谢博主，同时感谢b站中的up组

文章由：

[kay三石]: https://me.csdn.net/qq_37256896	"blog"

## **python：**



```
Python 是一种解释型语言： 这意味着开发过程中没有了编译这个环节。类似于PHP和Perl语言。
Python 是交互式语言： 这意味着，您可以在一个 Python 提示符 >>> 后直接执行代码。
Python 是面向对象语言: 这意味着Python支持面向对象的风格或代码封装在对象的编程技术。
Python 是初学者的语言：Python 对初级程序员而言，是一种伟大的语言，它支持广泛的应用程序开发，从简单的文字处理到 WWW 浏览器再到游戏。
paython下载安装即可
```

![img](https://www.pythontab.com/uploadfile/2017/0512/20170512112808642.png)

### **python基本使用形式**

python的使用中文编码：	 

```
#-*- coding: UTF-8 -*-
```

引号：

Python 可以使用引号( **'** )、双引号( **"** )、三引号( **'''** 或 **"""** ) 来表示字符串，引号的开始与结束必须的相同类型的。

其中三引号可以由多行组成，编写多行文本的快捷语法，常用于文档字符串，在文件的特定地点，被当做注释。

```
word = 'word'
sentence = "这是一个句子。"
paragraph = """这是一个段落。
包含了多个语句"""
```

注释：

```
# 第一个注释
print "Hello, Python!"  # 第二个注释

'''
这是多行注释，使用单引号。
这是多行注释，使用单引号。
这是多行注释，使用单引号。
'''

"""
这是多行注释，使用双引号。
这是多行注释，使用双引号。
这是多行注释，使用双引号。
"""
```

使用print输出  

```
print('hello python')
```

元组：

Python的元组与列表类似，不同之处在于元组的元素不能修改。元组使用小括号，列表使用方括号。元组创建很简单，只需要在括号中添加元素，并使用逗号隔开即可。

如下实例：

`tup1 = ('physics', 'chemistry', 1997, 2000)`

`tup2 = (1, 2, 3, 4, 5 )`

`tup3 = "a", "b", "c", "d"`

```
使用这个来输出 
print "tup1[0]: ", tup1[0]
print "tup2[1:5]: ", tup2[1:5] 输出1到4（下标从0开始）
元组中的元素值是不允许修改的
删除
tup = ('physics', 'chemistry', 1997, 2000)
print tup
del tup
print "After deleting tup : "
print tup

```

| Python 表达式                   | 结果                           | 描述     |
| ---------------------------- | ---------------------------- | ------ |
| len((1, 2, 3))               | 3                            | 计算元素个数 |
| (1, 2, 3) + (4, 5, 6)        | (1, 2, 3, 4, 5, 6)           | 连接     |
| ('Hi!',) * 4                 | ('Hi!', 'Hi!', 'Hi!', 'Hi!') | 复制     |
| 3 in (1, 2, 3)               | True                         | 元素是否存在 |
| for x in (1, 2, 3): print x, | 1 2 3                        | 迭代     |

### 元组索引截取

因为元组也是一个序列，所以我们可以访问元组中的指定位置的元素，也可以截取索引中的一段元素，如下所示：

元组：

```
L = ('spam', 'Spam', 'SPAM!')
```

| Python 表达式 | 结果                | 描述             |
| ---------- | ----------------- | -------------- |
| L[2]       | 'SPAM!'           | 读取第三个元素        |
| L[-2]      | 'Spam'            | 反向读取，读取倒数第二个元素 |
| L[1:]      | ('Spam', 'SPAM!') | 截取元素           |

### 元组内置函数

Python元组包含了以下内置函数

| 序号   | 方法及描述                                    |
| ---- | ---------------------------------------- |
| 1    | [cmp(tuple1, tuple2)](https://www.runoob.com/python/att-tuple-cmp.html)比较两个元组元素。 |
| 2    | [len(tuple)](https://www.runoob.com/python/att-tuple-len.html)计算元组元素个数。 |
| 3    | [max(tuple)](https://www.runoob.com/python/att-tuple-max.html)返回元组中元素最大值。 |
| 4    | [min(tuple)](https://www.runoob.com/python/att-tuple-min.html)返回元组中元素最小值。 |
| 5    | [tuple(seq)](https://www.runoob.com/python/att-tuple-tuple.html)将列表转换为元组。 |

### python字典：

字典是另一种可变容器模型，且可存储任意类型对象。

字典的每个键值 key=>value 对用冒号 : 分割，每个键值对之间用逗号 , 分割，整个字典包括在花括号 {} 中 ,格式如下所示：

d = {key1 : value1, key2 : value2 }

键一般是唯一的，如果重复最后的一个键值对会替换前面的，值不需要唯一。

\>>>dict = {'a': 1, 'b': 2, 'b': '3'}>>> dict['b']'3'>>> dict{'a': 1, 'b': '3'}

值可以取任何数据类型，但键必须是不可变的，如字符串，数字或元组。

一个简单的字典实例：

dict = {'Alice': '2341', 'Beth': '9102', 'Cecil': '3258'}

也可如此创建字典：

dict1 = { 'abc': 456 }dict2 = { 'abc': 123, 98.6: 37 }

这和map基本上一样 取字典的值为 print dirct1['abc']，

修改字典的值：dict['Age'] = 8 # 更新
dict['School'] = "RUNOOB" # 添加 

del dict['Name']  # 删除键是'Name'的条目

dict.clear()      # 清空字典所有条目
del dict          # 删除字典

#### 字典键的特性：

 1）不允许同一个键出现两次。创建时如果同一个键被赋值两次，后一个值会被记住，如下实例：

2）键必须不可变，所以可以用数字，字符串或元组充当，所以用列表就不行，如下实例：

其他的方法去看实例：

其他的不在介绍具体的可以去查看<https://www.runoob.com/python>

### python读取配置文件：

项目中使用的常量，我们把它收集在一个文件中，这就是配置文件。配置文件在项目中是非常必要的，它避免了项目中文件对常量的分散使用，让常量可以统一修改，避免造成修改不全面的问题。常用的配置文件后缀是.ini、.conf、.py，当然还有使用.json、.txt的，推荐使用常用的.ini、.py，配置文件的名字一般是config便于理解和使用。.ini 文件是Initialization File的缩写，即初始化文件，是windows的系统配置文件所采用的存储格式，统管windows的各项配置；.py的配置文件，在python项目中是作为一个包导入，严格来说不是配置文件，而是扩展包。

下面将介绍两类配置文件的使用，一类是.ini、.txt，另一类是.py。

.ini、.txt配置文件使用方法是一致的，只是一个后缀的区别，这里以ini配置文件来介绍，这类配置文件我们使用内置configparser库来使用，它可以实现配置文件的写入、更新、删除、读取等操作非常方便，建议使用这种方式。

### Configparser介绍：

此模块提供[`ConfigParser`](https://docs.python.org/3/library/configparser.html#configparser.ConfigParser)实现基本配置语言的类，该语言提供类似于Microsoft Windows INI文件中的结构。您可以使用它来编写可由最终用户轻松定制的Python程序。

注意：这个库并*没有*解释或写在INI语法的Windows注册表扩展版本中使用的值类型的前缀。

*class* `configparser.``ConfigParser`(*defaults=None*, *dict_type=collections.OrderedDict*, *allow_no_value=False*, *delimiters=('='*, *':')*, *comment_prefixes=('#'*, *';')*, *inline_comment_prefixes=None*, *strict=True*, *empty_lines_in_values=True*, *default_section=configparser.DEFAULTSECT*, *interpolation=BasicInterpolation()*, *converters={}*)

主要配置解析器。当给出*默认值时*，它将初始化为内部默认值字典。当给出*dict_type时*，它将用于为节的列表，节中的选项和默认值创建字典对象。

当给定*分隔符时*，它被用作从值中划分键的子串集合。当给出*comment_prefixes时*，它将被用作在其他空行中为注释添加前缀的子字符串集。评论可以缩进。当给出*inline_comment_prefixes时*，它将用作在非空行中为注释添加前缀的子字符串集。

当*strict*是`True`（默认值）时，解析器在从单个源（文件，字符串或字典）读取时，不允许任何部分或选项重复，引发[`DuplicateSectionError`](https://docs.python.org/3/library/configparser.html#configparser.DuplicateSectionError)或 [`DuplicateOptionError`](https://docs.python.org/3/library/configparser.html#configparser.DuplicateOptionError)。当*empty_lines_in_values*为`False` （默认值*:)时*`True`，每个空行标记选项的结尾。否则，多行选项的内部空行将保留为值的一部分。当*allow_no_value*为`True`（默认值*:)时*`False`，接受没有值的选项; 为这些保留的值是`None`，并且序列化没有尾随分隔符。

当给出*default_section时*，它指定特殊部分的名称，其中包含其他部分的默认值和插值目的（通常命名`"DEFAULT"`）。可以使用`default_section`instance属性在运行时检索和更改此值。

可以通过*插值*参数提供自定义处理程序来定制插值行为。`None`可用于完全关闭插值，`ExtendedInterpolation()`提供更高级的变体灵感`zc.buildout`。有关[专题文档部分中](https://docs.python.org/3/library/configparser.html#interpolation-of-values)有关此主题的更多信息 。

插值中使用的所有选项名称都将[`optionxform()`](https://docs.python.org/3/library/configparser.html#configparser.ConfigParser.optionxform)像任何其他选项名称引用一样通过该方法传递 。例如，使用默认实现[`optionxform()`](https://docs.python.org/3/library/configparser.html#configparser.ConfigParser.optionxform)（将选项名称转换为小写），值和等价。`foo %(bar)s``foo %(BAR)s`

当*转换器*给出，它应该是一个字典，其中每个键表示一个类型的转换器的名称和每一个值是一个可调用的执行从字符串中的转化为期望的数据类型。每个转换器都`get*()`在解析器对象和节代理上获得自己的相应方法。

常用的方法：

1. 基本的读取配置文件

   ```
     -read(filename) 直接读取ini文件内容
     -sections() 得到所有的section，并以列表的形式返回
     -options(section) 得到该section的所有option
     -items(section) 得到该section的所有键值对
     -get(section,option) 得到section中option的值，返回为string类型
     -getint(section,option) 得到section中option的值，返回为int类型，还有相应的getboolean()和getfloat() 函数。
   ```

2) 基本的写入配置文件

```
 -add_section(section) 添加一个新的section
 -set( section, option, value) 对section中的option进行设置，需要调用write将内容写入配置文件。
```



#### 实例

新建一个config.ini的配置文件内容如下：

```
[mysql]
name = admin
host = 255.255.255.0
proxy = 6037
password = 123456
pool = true
time = 3
```

其中[]中的是section节点，该节点下的等式是option即键=值

```
config.sections()  # 获取section节点
['mysql']

config.options('mysql')  # 获取指定section 的options即该节点的所有键
['name', 'host', 'proxy', 'password', 'pool', 'time']

config.get("mysql", "name")  # 获取指定section下的options
'admin'

config.getint("mysql", "proxy")  # 将获取到值转换为int型
6037

config.getboolean("mysql", "pool")  # 将获取到值转换为bool型
True

config.getfloat("mysql", "time")  # 将获取到值转换为浮点型
3.0

config.items("mysql")  # 获取section的所用配置信息
[('name', 'admin'), ('host', '255.255.255.0'), ('proxy', '6037'), ('password', '123456'), ('pool', 'true'), ('time', '3')]

config.set("mysql", "name", "root")  # 修改db_port的值为69
config.get("mysql", "name") 
'root'

config.has_section("mysql")  # 是否存在该section
True

config.has_option("mysql", "password")  # 是否存在该option
True

config.add_section("redis")  # 添加section节点
config.set("redis", "name", "redis_admin")  # 设置指定section 的options
config.items('redis')
[('name', 'redis_admin')]
#######################################################################
# -*- coding: utf-8 -*-

import configparser

config = configparser.ConfigParser() #获取这实例
config.read("Config.ini", encoding="utf-8")

config.sections()  # 获取section节点

config.options('mysql')  # 获取指定section 的options即该节点的所有键

config.get("mysql", "name")  # 获取指定section下的options
config.getint("mysql", "proxy")  # 将获取到值转换为int型
config.getboolean("mysql", "pool")  # 将获取到值转换为bool型
config.getfloat("mysql", "time")  # 将获取到值转换为浮点型

config.items("mysql")  # 获取section的所用配置信息

config.set("mysql", "name", "root")  # 修改db_port的值为69

config.has_section("mysql")  # 是否存在该section
config.has_option("mysql", "password")  # 是否存在该option

config.add_section("redis")  # 添加section节点
config.set("redis", "name", "redis_admin")  # 设置指定section 的options

config.remove_section("redis")  # 整个section下的所有内容都将删除
config.remove_option("mysql", 'time')  # 删除section下的指定options

config.write(open("Config", "w"))  # 保存config
```

共享的配置信息可以用

### global语句使用：

def spam():

​	global wggd

​	wggd = 'ajs'

wggd= 'gloabl'

print (wggd) #输出为ajs

### os.path模块

os.path包含了许多文件名和文件路径的操作有关的函数，使用需要 import os

```
os.path.abspath(path)将换回参数的绝对路径

os.path.isabs(path)如果是一个绝对的路径则返回true

os.path.relpath(path,start) 键返回从start路径到path路径的相对路径的字符串

os.path.dirname(path) 将返回一个字符串包含path参数的中最后一个斜杠的前的所有内容

os.path.basename(path)将返回一个字符串，包含path参数字最后的一个斜杠的所有内容


```

#### 读取文件：

file=open("路径") 打开文件

file.read()读取这个路径返回这个字符串

file.readlines()读取一个字符串列表

#### 写入文件：

file=open("路径") 打开文件

file.write("paths -strings")

#### 保存变量shelve模块：

可以将程序中的变量保存到二进制文件shelf文件中，可以然你的程序中添加，保存和打开功能，读写数据如下

import shelve

shelfile=shelve.open('mydata')

cats=['asd','pooks','simon']

shelfile['cats']=cats

shelfile.close()

以上会自行在本目录中创建mydata.bak,mydata.data,mydata.dir

shelfile=shelve.open('mydata')

type(shelfile)

shelfile['cats']

shelfile.close()

#### 用pprint.pformat()函数保存变量

可以将列表或者字典中的内容加载到屏幕，pprint.pformat()返回同样的文本字符串

## Andriod的基本操作

### android App类型

##### 原生APP

Native App因为位于平台层上方，向下访问和兼容的能力会比较好一些，可以支持在线或离线，消息推送或本地资源访问，摄像拨号功能的调取。但是由于设备碎片化，App的开发成本要高很多，维持多个版本的更新升级比较麻烦，用户的安装门槛也比较高。但是比较乐观的是，[App](https://baike.baidu.com/item/App)Store培养了一种比较好的用户付费模式，所以在Apple的生态圈里，开发者的盈利模式是一种明朗状态，其他market也在往这条路上靠拢。

1、提供最佳的用户体验，最优质的用户界面，最华丽的交互

2、针对不同平台提供不同体验

3、可节省带宽成本

4、可访问本地资源

5、盈利模式明朗

6、打开速度更快

##### 混合应用APP（Hybrid APP）

Hybrid App是指介于web-app、native-app这两者之间的[app](https://baike.baidu.com/item/app/33589),它虽然看上去是一个Native App，但只有一个UI WebView，里面访问的是一个Web App，比如街旁网最开始的应用就是包了个客户端的壳，其实里面是HTML5的网页，后来才推出真正的原生应用。再彻底一点的，如掌上百度和淘宝客户端Android版，走的也是Hybrid App的路线，不过掌上百度里面封装的不是WebView，而是自己的浏览内核，所以体验上更像客户端，更高效。

##### Web版App

本质就是浏览器功能的叠加，用普通web开发语言开发，通过浏览器运行

优势：支持范围广，开发成本低

##### 对比



![img](https://img-blog.csdn.net/20180917200853746?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2tlZXA3ODk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



![20180917205145229](C:\Users\leoill\Desktop\20180917205145229.png)

技术特性对比

![20180917211647501](C:\Users\leoill\Desktop\20180917211647501.png)

#### android SDK命令

sdk中文网:http://www.android-studio.org/

下载：https://dl.google.com/android/repository/sdk-tools-windows-4333796.zip

配置环境变量：

​	androidHome:"sdk路径"

​	path中加入：%androidHome%\tools;%androidHome%\platfom-tools;以及bulid-tools中的api路径

目录：

- add-ons: 附加库，第三方公司为平台附加的功能系统

- doc:sdkAPI,

- extras:存放android supportv4,v7,v13,和usb驱动，以及intel硬件加速

- platform: 是每个平台的sdk工具真正的文件，存放不同版本的android系统

- samples:是android中的默认实例工程


adb工作原理：客户端监听在5037服务端口，与adb的服务器进行通信

常用命令：
adb version :查看版本号

adb connect ip地址：连接没模拟器端口

adb shell : 是进入到android系统中的可以查看文件架构,进入后就是linux的命令的执行''#'是代表root权限，‘’“$”是没有root权限，

adb install | -r <apkName> -r是覆盖元安装文件 -s可以指定设备命令

失败后进入 adb remount,adb shell, cd /data, chmod 777 local 赋予读写权限即可

####package与Activity

package:是唯一的包名，可以识别这个组件是在哪个位置

activity:是所有的流程运行的基础，activity可以包含多个组件

其他activity中的布局元素和android学的一样不在赘述

### Monkey

####简介

Monkey就是猴子，  Monkey测试，就像一只猴子， 在电脑面前，乱敲键盘在测试。  猴子什么都不懂， 只知道乱敲通过Monkey程序模拟用户触摸屏幕、滑动Trackball、 按键等操作来对设备上的程序进行压力测试，检测程序多久的时间会发生异常

Monkey 主要用于Android 的压力测试  自动的一个压力测试小工具， 主要目的就是为了测试app 是否会Crash.

####程序介绍（启动步骤）

（1） Monkey程序由Android系统自带，使用Java诧言写成，在Android文件系统中的存放路径是： /system/framework/monkey.jar；   
（2） Monkey.jar程序是由一个名为“monkey”的Shell脚本来启动执行，shell脚本在Android文件系统中 的存放路径是：/system/bin/monkey；  
（3）Monkey 命令启动方式：    
           a）可以通过PC机CMD窗口中执行: adb shell monkey ｛+命令参数｝来进行Monkey测试          

​			b）在PC上adb shell 进入Android系统，通过执行 monkey {+命令参数} 来进行Monkey 测试          

​			c )  在Android机或者模拟器上直接执行monkey 命令，可以在Android机上安装Android终端模拟器  

#### monkey架构

Monkey 运行在设备或模拟器上面，可以脱离PC运行（普遍做法是将monkey作为一个像待测应用发送 随机按键消息的测试工具。验证待测应用在这些随机性的输入面前是否会闪退或者崩溃）

activity:是

![img](https://images2015.cnblogs.com/blog/263119/201605/263119-20160505223944482-1516389266.png)

####monkey参数



![img](https://images2015.cnblogs.com/blog/263119/201605/263119-20160505230123154-1071480837.png)

-p <允许的包名列表>         

用此参数指定一个或多个包。指定包之后，monkey将只允许系统启动指定的app。如果丌指定包， monkey将允许系统启动设备中的所有app。  

指定一个包：adb shell monkey -p com.shjt.map 100   

指定多个包：adb shell monkey -p fishjoy.control.menu  –p com.shjt.map  100  

-v        

用亍指定反馈信息级别（信息级别就是日志的详细程度），总共分3个级别，分别对应的参数如下 表所示： 

Level 0  :  adb shell monkey -p com.shjt.map -v 100               // 缺省值，仅提供启动提示、测试完成和最终结果等少量信息   

Level 1  :  adb shell monkey -p com.shjt.map -v  -v 100          // 提供较为详细的日志，包括每个发送到Activity的事件信息

Level 2  :  adb shell monkey -p com.shjt.map -v  -v  -v 100     // 最详细的日志，包括了测试中选中/未选中的Activity信息
-s（随机数种子）            

用亍指定伪随机数生成器的seed值，如果seed相同，则两次Monkey测试所产生的事件序列也相同的。  示例：

monkey测试1：adb shell monkey -p com.shjt.map –s 10 100                

monkey测试2：adb shell monkey -p com.shjt.map –s 10 100  
--throttle <毫秒>            

用亍指定用户操作（即事件）间的时延，单位是毫秒；如果丌指定这个参数，monkey会尽可能快的 生成和发送消息。 示

例：adb shell monkey -p com.shjt.map --throttle 3000 100   

#### 命令综合实践

##### monkeyrunner

monkeyrunner即android SDK中自带的工具之一，此工具提供API可按制android设备或模拟器。

monkeyrunner提供了一个API，使用此API写出的程序可以在Android代码之外控制Android设备和模拟器。通过monkeyrunner，您可以写出一个Python程序去安装一个Android应用程序或测试包，运行它，向它发送模拟击键，截取它的用户界面图片，并将截图存储于工作站上。

monkeyrunner工具的主要目的是用于测试功能/框架水平上的应用程序和设备，或用于运行单元测试套件，但您当然也可以将其用于其它目的。

http://developer.android.com/tools/help/monkeyrunner_concepts.html

Monkey：Monkey工具直接运行在设备或模拟器的adb shell中，生成用户或系统的伪随机事件流。

Monkeyrunner：Monkeyrunner工具是在工作站上通过API定义的特定命令和事件控制设备或模拟器。

###### 环境搭建

1.python搭建

2.jdk

3.sdk

4.配置platform-tools路径

######Monkeyrunner API

主要包括三个模块

1、MonkeyRunner:这个类提供了用于连接monkeyrunner和设备或模拟器的方法，它还提供了用于创建用户界面显示提供了方法。

2、MonkeyDevice:代表一个设备或模拟器。这个类为安装和卸载包、开启Activity、发送按键和触摸事件、运行测试包等提供了方法。

3、MonkeyImage:这个类提供了捕捉屏幕的方法。这个类为截图、将位图转换成各种格式、对比两个MonkeyImage对象、将image保存到文件等提供了方法。

**MokeyRunnerApI**

- ​	1.waitForConnection():等待设备连接

  - 2.choice()：选项列表框

  - 3.help():帮助文档

  - 4.input():输入

  - 5.sleep()暂停


**MonkeyDevice:**

- installPackage(string path)

- removePackage(string package)

- startActivity(string uri,string action,string data....)

- touch(integer x,integer y,integer type)

- drag(tuple start,tuple end,float duration,integer steps) 起始位置，总点，持续事件 默认1.0s，步数默认 10


**MonkeyImageAPI**

- convertToBytes() 转换为图像格式
- getRawPixel()获取当前坐标像素元组
- getRawPixelint()获取当前坐标的像素值
- saneAs()图像对比
- writeToFile()保存图像文件
- getSubImage()截取子图像
- takeSnapshot()进行屏幕截图

引用导入API
from com.android.monkeyrunner import <module> 

运行monkeyrunner

命令语法为：

monkeyrunner -plugin <plugin_jar> <program_filename> <program_options>

 方式一：在CMD命令窗口直接运行monkeyrunner

方式二：使用Python编写测试代码文件，在CMD中执行monkeyrunner Findyou.py运行

不论使用哪种方式，您都需要调用SDK目录的tools子目录下的monkeyrunner命令。

 注意：在运行monkeyrunner之前必须先运行相应的模拟器或连接真机，否则monkeyrunner无法连接到设备

运行模拟器有两种方法：1、通过eclipse中执行模拟器 2、在CMD中通过命令调用模拟器

这里介绍通过命令，在CMD中执行模拟器的方法

emulator -avd test

上面命令中test是指模拟器的名称。

 附：问题：CMD运行提示monkeyrunner不是内部或外部命令，也不是可运行的程序或批处理文件。

解决：电脑环境变量未配置，将monkeyrunner所在目录配在环境变量里。

变量名：Path

变量值：D:\android\android-sdk-windows\tools;D:\android\android-sdk-windows\platform-tools

```py
#使用monkeyRunner进行连接
from com.android.monkeyruuner import MonkeyRunner as mr
from com.android.monkeyrunner import Monkeydevice as md
from com.android.monkeyrunner import MonkeyImage as mi

device=mr.waitForConnection()
print("innstall app")
device.installPackage('路径')
package=""
activity=""
runnerComponect=package+'/'+activity
device.startActivity(component=runnerComponect)
#直接启动那个monkeyrunner直接就可以启动
mr.sleep(3) #设置时间间隔
print("touch cancel button")
device.touch(618,895,'DOWN_AND_UP')
mr.sleep(1)
print("touch skip button")
device.touch(618,895,'DOWN_AND_UP')
mr.sleep(1)
print("input usernmae andpassword")
device.touch(618,895,'DOWN_AND_UP')
mr.sleep(1)
device.type('zxxx')
device.touch(618,995,'DOWN_AND_UP')
mr.sleep(1)
device.type('zddd') #发送字符串到键盘
mr.sleep(2)
print("touch loginbutton")
device.touch(918,895,'DOWN_AND_UP')
mr.sleep(1)
#截图
screeshot=device.takeSnapshot()
screeshot.writeToFile("路径",png)
#字符串发送到键盘 
#device.type('字符串')
device.type('Findyou')
 

#唤醒设备屏幕
#锁屏后,屏幕关闭，可以用下命令唤醒
device.wake()
 

#重起手机

device.reboot()
 

#模拟滑动

#device.drag(X,Y,D,S)
#X 开始坐标
#Y 结束坐标
#D 拖动持续时间(以秒为单位)，默认1.0秒
#S 插值点时要采取的步骤。默认值是10
device.drag((100,1053),(520,1053),0.1,10)
 

#在指定位置发送触摸事件 
#device.touch(x,y,触摸事件类型)
#x,y的单位为像素
#触摸事件类型，请见下文中Findyou对device.press描述

device.touch(520,520,'DOWN_AND_UP')
 

#发送指定类型指定键码的事件

复制代码
#device.press(参数1:键码,参数2:触摸事件类型)
#参数1：见android.view.KeyEvent
#参数2，如有TouchPressType()返回的类型－触摸事件类型，有三种。
#1、DOWN 发送一个DOWN事件。指定DOWN事件类型发送到设备，对应的按一个键或触摸屏幕上。
#2、UP 发送一个UP事件。指定UP事件类型发送到设备，对应释放一个键或从屏幕上抬起。
#3、DOWN_AND_UP 发送一个DOWN事件，然后一个UP事件。对应于输入键或点击屏幕。
以上三种事件做为press()参数或touch()参数

#按下HOME键
device.press('KEYCODE_HOME',MonkeyDevice.DOWN_AND_UP) 
#按下BACK键
device.press('KEYCODE_BACK',MonkeyDevice.DOWN_AND_UP) 
#按下下导航键
device.press('KEYCODE_DPAD_DOWN',MonkeyDevice.DOWN_AND_UP) 
#按下上导航键
device.press('KEYCODE_DPAD_UP',MonkeyDevice.DOWN_AND_UP) 
#按下OK键
device.press('KEYCODE_DPAD_CENTER',MonkeyDevice.DOWN_AND_UP) 

```

### Appium

Appium是一个开源、跨平台的测试框架，可以用来测试原生及混合的移动端应用。Appium支持IOS、Android及FirefoxOS平台。Appium使用WebDriver的json wire协议，来驱动Apple系统的UIAutomation库、Android系统的UIAutomator框架。Appium对IOS系统的支持得益于Dan Cuellar’s对于IOS自动化的研究。Appium也集成了Selendroid，来支持老android版本。

Appium支持Selenium WebDriver支持的所有语言，如java、Object-C、JavaScript、Php、Python、Ruby、C#、Clojure，或者Perl语言，更可以使用Selenium WebDriver的Api。Appium支持任何一种测试框架。如果只使用Apple的UIAutomation，我们只能用javascript来编写测试用例，而且只能用Instruction来运行测试用例。同样，如果只使用Google的UIAutomation，我们就只能用java来编写测试用例。Appium实现了真正的跨平台自动化测试。

#### 技术架构

![](https://cmlanche-1251406926.cos.ap-guangzhou.myqcloud.com/blog/4y4w5.png)

| 功能\Driver      | appium-android-driver                    | appium-uiautomator2-driver               | appium-espresso-driver               |
| :------------- | :--------------------------------------- | :--------------------------------------- | :----------------------------------- |
| 用途             | 驱动UIAutomator1                           | 驱动UIAutomator2                           | 驱动espresso                           |
| automationName | UiAutomator1                             | UiAutomator2                             | Espresso                             |
| 包形式            | AppiumBootstrap.jar                      | appium-uiautomator2-server-v${version}.apk | TODO待研究                              |
| 包依赖地址          | bootstrap/bin/                           | appium-uiautomator2-server/apks/         | TODO                                 |
| 优点             | jar包形式，免安装，一个命令直接启动，权限级别是shell级别         | 官方推荐使用2，对高版本兼容性好                         | 控件识别能力强                              |
| 缺点             | 对高版本兼容性差，容易无法识别控件                        | apk形式，需要安装                               | apk形式，需求安装，并且是侵入式的，可能带来风险            |
| Server模块       | 在相同工程中，Bootstrap目录，maven工程，主要目标是在bin目录下输出AppiumBootstrap.jar | 不同工程，单独的另外一个Nodejs工程：appium-uiautomator2-server | 相同Nodejs工程，espress-server目录，gradle工程 |

Appium组件：

Appium Server:是appium的服务器。一个web接口服务，使用nodejs实现

Appium Desktop是一款适用与mac和windowslinunx的开源的应用程序，他以美观而灵活的用户界面提供Appium自动化服务器的强大工具，

#### Appium环境搭建

1.安装node.js  node -v :查看版本 npm -v 是node.js中的包管理的工具，

2.安装appium  可以通过淘宝镜（npm.taobao.org）像 npm install -g cnpm --register=https://reistry.npm.taobao.arg	使用cmd:cnpm install -g appium1.7.2-g

where appium可查看安装路径，运行 cmd: appium

3.安装python

4.安装pycharm

#### Appium会话—— Capability

desired Capabilitys的功能是配置Appium的会话，是一组的键值对的集合，其中键值对设置对应的名称而值设置对应的值，它们将作为JSON对象发送到Appium。

```
{
"platformName": "iOS",
"platformVersion": "11.0",
"deviceName": "iPhone 7",
"automationName": "XCUITest",
"app": "/path/to/my.app"
}
```

这组期望的能力的表示为Appium到在iPhone 7模拟器与iOS 11开始自动化会议，使用的愿望[XCUITest驱动](http://appium.io/docs/en/drivers/ios-xcuitest/index.html)，以`/path/to/my.app`作为待测应用程序。

#####session:

Appium的客户端和服务端之间的通信必须在session中的上下文中进行客户端发起通信色时候首先技术逆行json对象给服务器，然后创建session的ID返回客户端，之后客户端可以用session的Id来操作

#####capability配置讲解：

###### 公用的Capability

![img](https://yuedu.baidu.com/bookeditor/interface/imageview?book_id=ee049f357275a417866fb84ae45c3b3567ecdd1b&file=c68cd18390ae28f4fa21221586988f0b.png)

###### android独有的capability

![img](https://yuedu.baidu.com/bookeditor/interface/imageview?book_id=ee049f357275a417866fb84ae45c3b3567ecdd1b&file=6d6b92e23ae105aba3dbac5ff328fb1e.png)

###### ios独有的capability

| 键               | 描述                                       | 实例                    |
| --------------- | ---------------------------------------- | --------------------- |
| bundleId        | 捆绑被测试应用的ID。用于在真实设备上启动应用程序或在测试启动期间使用需要捆绑ID的其他大写字母。要使用捆绑包ID在真实设备上运行测试，您可以省略“app”功能，但必须提供“udid”。 | 例如 io.appium.TestApp` |
| `udid`          | 连接的物理设备的唯一设备标识符                          | 例如 `1ae203187fc012g`  |
| `launchTimeout` | 在假设挂起并且会话失败之前等待仪器的时间量（以ms为单位）            | 例如 `20000`            |
| `appName`       | 被测应用程序的显示名称。用于在iOS 9+中自动化应用程序的后台处理。      | 例如， `UICatalog`       |

更多参看http://appium.io/docs/en/writing-running-appium/caps/

#### Capability实现自动登录

启动App进入登录界面，自动输入用户名和密码

`descried_caps['unicodeKeyboard']="ture"`设置这个编码支持中文

`descried_caps['resetKeyBoard']`="true" 设置重置键盘

```
capability.py
from appium import webdriver
descried_caps={}
descried_caps['platformName']='Android'
descried_caps['deviceName']='127.0.0.1:62025'
descried_caps['platformVersion']='5.1.1'
dedcried_caps['uuid']=''#真机中需要使用，这个是adb中列出的
dedcried_caps['noRest']='true' #保留上次的
descried_caps['unicodeKeyboard']="ture" #设置后需要把输入法设置为本机的
descried_caps['resetKeyBoard']="true"
descried_caps['app']=''
descried_caps['appPackage']=''
descried_caps['appActivity']=''
driver=webdriver.Romote('http://127.0.0.1:4723',descried_caps)
```

```python
from capability import driver
def login():
	driver.find_elemet_by_id('').click()
	driver.find_elemet_by_id('').click()
try:
	driver.find_elemet_by_id('')
except NoSuchElement:
	login()
else:
	driver.find_elment_by_id()
	
```



#### 第一个appium的脚本

```python
from appium import webdriver
descried_caps={}
descried_caps['platformName']='Android'
descried_caps['deviceName']='127.0.0.1:62025'
descried_caps['platformVersion']='5.1.1'
dedcried_caps['uuid']=''#真机中需要使用，这个是adb中列出的
dedcried_caps['noRest']='true' #保留上次的
descried_caps['app']=''
descried_caps['appPackage']=''
descried_caps['appActivity']=''
driver=webdriver.Romote('http://127.0.0.1:4723',descried_caps)
driver.find_element_by_id("id名称").click() #点击

执行即可
```

学过unittest的都知道里面用前置和后置setup和teardown非常好用，在每次用例开始前和结束后都去执行一次

当然还有更高级一点的setupClass和teardownClass，需配合@classmethod装饰器一起使用

#### Appium元素定位

##### id

使用id定位不会重复，可以用 find_element_by_id()方法查找

find_elements_by_id().click()

find_elements_by_id().clear() 清除这些

###### 检测元素检测是否存在

<u>用if(){}else{}判断元素是否存在时还是会出现异常的不可以用这个来判断，不可以使用这个方法</u>

应该用try catch方法：

```python
def check_thebuton():  
	print("check")
	try:
		cancelbutton=driver.find_element_by_id('android:id/button1')
	except NoSuchElementException:
        print('hhh')
	else:
		cancelbutton.click()
check_thebuton()	
```

##### name定位:现在已经废弃了

find_element_by_name('请输入用那个户名').send('xx')

##### className定位

classname是根据元素的类型进行定位，但是实际情况中的很多的都是相同的，如果有id则不建议这样使用，重复时只能定义到第一个这样的

find_element_by_name('请输入用那个户名').send('xx')

##### 相对定位

找到元素的父元素的节点然后在找到这个，如果父元素中下面有多个相同的只能调用功第一个

parent=find_element_by_id()

parent.find_element_byclassName("").click()

##### xpath定位

xpath是一种路径的定位方式，主要用于依赖元素绝对路径经定位

1.xpath路径表达式：

​	

| 表达式      | 描述                        |
| -------- | ------------------------- |
| /        | 从根节点选取                    |
| //       | 从匹配选择的当前节点选择文档中的节点，而不考虑位置 |
| nodename | 选取节点的所有子节点                |
| .        | 当前节点                      |
| ..       | 选取当前节点的父节点                |
| @        | 选取属性                      |

XPath 通配符可用来选取未知的 XML 元素。

| 通配符    | 描述         |
| :----- | :--------- |
| *      | 匹配任何元素节点。  |
| @*     | 匹配任何属性节点。  |
| node() | 匹配任何类型的节点。 |

driver.find_element_by_path()

##### list定位

相同的classname不能够区分定位，所以就需要获取一个数组下标来获取区分不同的标记元素，

lists_imageviws=driver.find_elements_by_id()

list_imageviews[0].click()

#### Appium元素等待

元素等待的作用，增强脚本的健壮性，提高执行效率

##### 强制等待

设置固定的等待时间，使用sleep()方法即可实现

```
   from time import sleep #强制等待5秒
   sleep(5)
```

##### **隐式等待**

隐式等待是针对全部元素设置的等待时间

```
driver.implicitly_wait(20)
```

##### **显式等待**

显式等待是针对某个元素来设置的等待时间。
法WebDriverWait格式参数如下

```
from selenium.webdriver.support.ui import WebDriverWait

WebDriverWait(driver, timeout, poll_frequency=0.5, ignored_exceptions=None)
driver : WebDriver
timeout : 最长超时时间，默认以秒为单位
poll_frequency : 休眠时间的间隔时间，默认为0.5秒
ignored_exceptions : 超时后的异常信息，默认情况下抛NoSuchElementException异常。
```

WebDriverWait()一般和until()或until_not()方法配合使用，另外，lambda提供了一个运行时动态创建函数的方法。

WebDriverWait()一般和until()或until_not()方法配合使用，另外，lambda提供了一个运行时动态创建函数的方法。

```
from selenium.webdriver.support.ui import WebDriverWait
WebDriverWait(driver,10).until(lambda x:x.find_element_by_id("elementID"))
12
```

#### Toast元素识别

toast弹窗识别：需要安装selenuim,使用WebDriverWait()一般和until()或until_not()方法配合使用，另外，lambda提供了一个运行时动态创建函数的方法。显示的等待超时后的异常信息，默认情况下抛NoSuchElementException异常。

```python
#coding=utf-8
from selenuim.webdriver,support.ui import WebDriverWait

driver.find_element_by_id("com.tal.kaoyan:id/login_email_edittext").clear()
driver.find_element_by_id("com.tal.kaoyan:id/login_email_edittext").send_keys("zxcx")

driver.find_element_by_id("com.tal.kaoyan:id/login_password_edittext").send_keys("zxcx")
driver.find_element_by_id("com.tal.kaoyan:id/login_btn").click()

error_message='用户名密码错误'
limit_message='验证失败'
message='//*[@text=\text'{}']'.fromat(error_meaasage)
#message='//*[@text=\text'{}']'.fromat(limit_meaasage)
#下面的是超时报错
toast_element=WebDriverWait(driver,5).until(
WebDriverWait(driver,10).until(lambda x:x.find_element_by_id(message)) 
print(toast_element.text)


```

#### 屏幕截图

##### 方法1

```
driver.save_screenshot()该方法直接保存当前截图到脚本所在的文件的位置 文件为截图
driver.save_screenshot("screen.jpg")
```

##### 方法2

```
driver.screenshot_as_file(self,filename)将截图保留到指定的文件路径
```

#### Appium隐藏键盘

使用driver.hide_keyboard()来隐藏

```python
# 源码中使用这样的
def hide_keyboard(self, key_name=None, key=None, strategy=None):
    """Hides the software keyboard on the device.

    In iOS, use `key_name` to press
    a particular key, or `strategy`. In Android, no parameters are used.

    Args:
        key_name (:obj:`str`, optional): key to press
        key (:obj:`str`, optional):
        strategy (:obj:`str`, optional): strategy for closing the keyboard (e.g., `tapOutside`)
    """
    data = {}
    if key_name is not None:
        data['keyName'] = key_name
    elif key is not None:
        data['key'] = key
    elif strategy is None:
        strategy = 'tapOutside'
    data['strategy'] = strategy
    self.execute(Command.HIDE_KEYBOARD, data)
    return self
```

### UiAutomator定位

当appium中不能识别的可以用这个进行补充使用。

优点：1.可以对所有操作进行自动化，操作简单；

   　　2.不需要对被测程序进行重签名，且，可以测试所有设备上的程序，比如~某APP，比如~拨号，比如~发信息等等

　　   3.对于控件定位，要比robotium简单一点点

 

缺点：1.uiautomator需要android level 16以上才可以使用，因为在level 16及以上的API里面才带有uiautomator工具

   　　2.如果想要使用resource-id定位控件，则需要level 18及以上才可以

　　   3.对中文支持不好（不代表不支持，第三方jar可以实现）

　　   4.个人感觉，控件定位不如robotium那样层级分明，仅仅个人感觉，用户行为注入还是和插桩有点点区别的



```python
import uiautomator2 as u2
class SimpleTestCase(unittest.TestCase):
    @classmethod
    def setUpClass(cls):
        cls.d = u2.connect()
        cls.d.set_orientation('natural')
        cls.d.implicitly_wait(10)

    def setUp(self):
        self.sess = self.d.session("io.appium.android.apis")
        self.sess.watchers.remove()

    def tearDown(self):
        self.sess.watchers.remove()
        def test_toast_get_message(self):
        d = self.sess
        d.toast.reset()
        assert d.toast.get_message(0) is None
        assert d.toast.get_message(0, default="d") == "d"
        d(text="App").click()
        d(text="Notification").click()
        d(text="NotifyWithText").click()
        try:
            d(text="Show Short Notification").click()
        except u2.UiObjectNotFoundError:
            d(text="SHOW SHORT NOTIFICATION").click()
        self.assertEqual(d.toast.get_message(2, 5, ""), "Short notification")
        time.sleep(.5)
        self.assertIsNone(d.toast.get_message(0, 0.4))
        # d.toast.reset()
        # d.toast.show("Hello world")
        # self.assertEqual(d.toast.get_message(5, 5), "Hello world")

    def test_scroll(self):
        d = self.sess
        d(text="App").click()
        d(scrollable=True).scroll.to(text="Voice Recognition")
```



1. ResourceId定位: `d(resourceId="com.smartisanos.clock:id/text_stopwatch").click()`
2. Text定位 `d(text="秒表").click()`
3. Description定位 `d(description="..").click()`
4. ClassName定位 `d(className="android.widget.TextView").click()`

xpath定位并不支持，一开始打算做支持的，但是发现不用也能搞定。就是代码写的长一点而已。

#### 操作控件

```
# clickd(text="Settings").click()# long clickd(text="Settings").long_click()# 等待元素的出现d(text="Settings").wait(timeout=10.0)
```

```
user =new Uiobject(new UiSelector().text(``"请输入手机号"``))
user.setText(``"test"``)
```

```
首先他会根据你的定位信息new一个对象出来，然后再在你new的对象之上进行相关操作。下面我们看一下在appium中的实现方法:
ele =self.driver.find_element_by_android_uiautomator('new UiSelector().text("请输入手机号")')
ele.send_keys("123")
```

两个代码进行比较我们可以发现他们的相同之处只是最后的定位信息是一样的，即：new UiSelector().text("请输入手机号")

##### text模糊定位

```python
ele =self.driver.find_element_by_android_uiautomator('new UiSelector().textContains("请输入手")')
ele.send_keys("123")
```

##### 正则匹配查找

```pthon
ele =self.driver.find_element_by_android_uiautomator('new UiSelector().textMatches("^请输入手.*")')
ele.send_keys("123")
```

##### resourceID定位

```
resourceId定位和appium封装好的id定位是一样的，只是这里将写法变成了uiautomator的写法而已，
ele =self.driver.find_element_by_android_uiautomator('new UiSelector().resourceId("cn.com.open.mooc:id/et_phone_edit")')
ele.send_keys('234')
```

##### resourceIDMatches定位

通过id进行正则匹配定位

```
ele = self.driver.find_element_by_android_uiautomator('new UiSelector().resourceIdMatches(".+et_phone_edit")')
ele.send_keys('234')
```

通过className进行定位

```python
ele =self.driver.find_element_by_android_uiautomator('new UiSelector().className("android.widget.EditText")')
ele.send_keys('234')
```

##### classNameMatches定位

通过className正则匹配进行定位

```python
ele = self.driver.find_element_by_android_uiautomator('new UiSelector().classNameMatches (".*EditText")')
ele.send_keys('234')
```

**中文字符的输入**
如果可以定位到元素，直接通过set_text就可以输入中文

```
d(text="Settings").set_text("你好")
```

如果定位不到元素需要使用`send_keys`方法，以及切换输入法

```python
d.set_fastinput_ime(True)d.send_keys("你好 Hello")d.set_fastinput_ime(False) # 输入法用完关掉
```

```python
driver.find_element_by_android_uiautomator\

('new UiSelector().className("android.widget.EditText")').send_keys('1234')
driver.find_element_by_android_uiautomator\
('new UiSelector().resourceId("com.tal.kaoyan:id/login_password_editorText")').sendkey("dsds")
driver.find_element_by_android_uiautomator\
('new UiSelector().resourceId("com.tal.kaoyan:id/button")').click()
```

### H5元素定位

针对这种场景直接使用前面所讲的方法来进行定位是行不通的，因为前面的都是基于Andriod原生控件进行元素定位，而Web网页是单独的B/S架构，两者的运行环境不同因此需要进行上下文（context）切换，然后对H5页面元素进行定位操作。

#### context

##### **简介**

Context的中文翻译为：语境; 上下文; 背景; 环境，在开发中我们经常说“上下文”，那么这个“上下文”到底是指什么意思呢？

Android源码中的注释是这么来解释Context的：

Interface to global information about an application environment. This is an abstract class whose implementation is provided by the Android system. It allows access to application-specific resources and classes, as well as up-calls for application-level operations such as launching activities, broadcasting and receiving intents, etc.

在程序中context我们可以理解为当前对象在程序中所处的一个环境。 比如前面提到的App一个界面是属于Activity类型，也就是Android界面环境，但是当访问内嵌的网页是属于另外一个环境（网页环境），两者处于不同的一个环境。

1. 先进入到H5页面，然后切换到context,再进行相关元素定位操作。

2. conetext切换：可以通过contexts()方法来获取到页面的所有context,然后切换到H5页面的context

3. 在H5页面进行元素定位操作

   获取方法实践

   contexts=driver.contexts

   **print**(contexts)   

   #### 实例：

   ```python
   from  appium import webdriver
   
   from selenium.webdriver.support.ui import WebDriverWait
   ```




```python
desired_caps={}

   desired_caps[‘platformName‘]=‘Android‘

   desired_caps[‘platformVersion‘]=‘5.1.1‘

   desired_caps[‘deviceName‘]=‘127.0.0.1:21503‘

   desired_caps[‘app‘]=r‘C:\Users\Shuqing\Desktop\dr.fone3.2.0.apk‘

   desired_caps[‘appPackage‘]=‘com.wondershare.drfone‘

   desired_caps[‘appActivity‘]=‘com.wondershare.drfone.ui.activity.WelcomeActivity‘

   driver = webdriver.Remote(‘http://localhost:4723/wd/hub‘, desired_caps)

   driver.implicitly_wait(5)

   print(‘click BackupBtn‘)

   driver.find_element_by_id(‘com.wondershare.drfone:id/btnBackup‘).click()

显示等待

   WebDriverWait(driver,8).until(lambda x:x.find_element_by_id(‘com.wondershare.drfone:id/btnRecoverData‘))

   print(‘click NextBtn‘)

   driver.find_element_by_id(‘com.wondershare.drfone:id/btnRecoverData‘).click()

   WebDriverWait(driver,8).until(lambda x:x.find_element_by_class_name(‘android.webkit.WebView‘))

   contexts=driver.contexts

   print(contexts)

需android4.4及以上版本的系统中才会输出更多的webview

   print(‘switch conetext‘)

   driver.switch_to.context(‘WEBVIEW_com.wondershare.drfone‘)

   print(‘edit email‘)

   driver.find_element_by_id(‘email‘).send_keys(‘shuqing@wondershare.cn‘)

   print(‘click sendBtn‘)

   driver.find_element_by_class_name(‘btn_send‘).click()

切换context 点击返回

   driver.switch_to.context(‘NATIVE_APP‘)

   driver.find_element_by_class_name(‘android.widget.ImageButton‘).click()

```



   ```

### Appuim滑动操作

​```python
swip.py
#滑动的演示
from time import sleep
from appium import webdriver
descried_caps={}
descried_caps['platformName']='Android'
descried_caps['deviceName']='127.0.0.1:62025'
descried_caps['platformVersion']='5.1.1'
dedcried_caps['uuid']=''#真机中需要使用，这个是adb中列出的
dedcried_caps['noRest']='true' #保留上次的
descried_caps['unicodeKeyboard']="ture" #设置后需要把输入法设置为本机的
descried_caps['resetKeyBoard']="true"
descried_caps['app']=''
descried_caps['appPackage']=''
descried_caps['appActivity']=''
driver=webdriver.Romote('http://127.0.0.1:4723',descried_caps)
def get_size():
    x=driver.get_window_size()['width']
    y=driver.get_window_size()['height']
    return x,y
l=get_size()
print(l)

def swip_left():
    l=get_size()
    x1=int(l[0]*0.9)
    y1=int(l[1]*0.5)
    x2=int(l[0]*0.1)
    driver.swipe(x1,y1,x2,y1,1000)
#上，上滑操作就是从屏幕的下端点击一个坐标然后往上滑动，x坐标可以不变。Y的开始和结束坐标改进即可。
def swipe_up():
    l=get_size()
    x1=int(l[0]*0.5)
    y1=int(l[1]*0.75)
    x2=int(l[0]*0.5)
    y2=int(l[1]*0.25)
    driver.swipe(x1,y1,x2,y2,1000)
#下滑就是从屏幕的上端点击一个坐标然后往下滑动到另外一个坐标，x坐标可以不变。Y的开始和结束坐标改变即可。
def swipe_down():
     l=get_size()
    x1=int(l[0]*0.5)
    y1=int(l[1]*0.25)
    x2=int(l[0]*0.5)
    y2=int(l[1]*0.75)
    driver.swipe(x1,y1,x2,y2,1000)
def swipe_right():
    swipe_down():
     l=get_size()
    x1=int(l[0]*0.25)
    y1=int(l[1]*0.5)
    x2=int(l[0]*0.75)
    y2=int(l[1]*0.5)
    driver.swipe(x1,y1,x2,y2,1000)
for i in rang(2):
    swipleft()
    sleep(5)
#然后操作其他的即可
   ```

### Appium连续滑动-TouchAction

滑动操作是两点之间的滑动，而实际上用户可通过多点连续划动的操作需要使用TouchAction,TouchAction包含的一些操作：按压，长按，点击，移动，暂停，使用前需导入 from appium.webdriver.common.touch_action import TouchAction

#### 1、按压控件

------

方法：press()

开始按压一个元素或坐标点（x,y）。通过手指按压手机屏幕的某个位置。

**press(WebElement el, int x, int y)** press也可以接收屏幕的坐标（x,y）。

例：

```
TouchAction(driver).press(x=0,y=308).release().perform()
```

除了press()方法之外，本例中还用到了别外两个新方法。

- release() 结束的行动取消屏幕上的指针。
- Perform() 执行的操作发送到服务器的命令操作。

#### 2、长按控件

------

方法：longPress()开始按压一个元素或坐标点（x,y）。 相比press()方法，longPress()多了一个入参，既然长按，得有按的时间吧。duration以毫秒为单位。1000表示按一秒钟。其用法与press()方法相同。

**longPress(WebElement el, int x, int y, Duration duration)**

例： `java TouchAction action = new TouchAction(driver); action.longPress(names.get(1),1000).perform().release(); action.longPress(1 ,302,1000).perform().release();`

#### 3、点击控件

------

方法：tap()对一个元素或控件执行点击操作。用法参考press()。

**tap(WebElement el, int x, int y)**

例： `java TouchAction action = new TouchAction(driver); action.tap(names.get(1)).perform().release(); action.tap(1 ,302).perform().release();`

#### 4、移动

------

方法：moveTo()将指针（光标）从过去指向指定的元素或点。

**movTo(WebElement el, int x, int y)**

其用法参考press()方法。

例： `Java TouchAction action = new TouchAction(driver); action.moveTo(names.get(1)).perform().release(); action.moveTo(1 ,302).perform().release();`

#### 5、暂停

------

方法：wait()暂停脚本的执行，单位为毫秒。 `Java action.wait(1000);`

### Appium多点操作-MulitAction



### 断言（assert)

Python assert（断言）用于判断一个表达式，在表达式条件为 false 的时候触发异常。

`pytest`允许您使用标准python `assert`来验证Python测试中的期望值和值

语法格式如下：

```
assert expression
```

等价于：

```
if not expression:
    raise AssertionError
```

assert 后面也可以紧跟参数:

```
assert expression [, arguments]
```

等价于：

```
if not expression:
    raise AssertionError(arguments)
```

```python
def test_updatet_other(begin):
    engagement_page.click_update_items_first()  # 选择第一条
    engagement_page.click_btn1_all()
    edit_other_page.click_other_input('other update')  # 更新
    edit_other_page.click_submit_btn()
    engagement_page.should_no_red_icon()
    other_name = engagement_page.get_first_item_content_text()
    assert other_name == "Other update"
    engagement_page.click_delete_first_note()

```



### pytest

`ytest`是一个框架，使构建简单和可伸缩的测试变得容易。测试具有表现力和可读性 - 无需样板代码。通过针对您的应用程序或库的小型单元测试或复杂功能测试，在几分钟内开始使用。

#### 安装`pytest`

1. 在命令行中运行以下命令：

```
pip install -U pytest

```

1. 检查您是否安装了正确的版本：

```
$ pytest --version
This is pytest version 5.x.y, imported from $PYTHON_PREFIX/lib/python3.6/site-packages/pytest.py

```

#### 创建你的第一个测试

使用四行代码创建一个简单的测试函数：

```
# content of test_sample.py
def func(x):
    return x + 1

def test_answer():
    assert func(3) == 5
```

一旦开发了多个测试，您可能希望将它们分组到一个类中。pytest可以很容易地创建一个包含多个测试的类：

```
# content of test_class.py
class TestClass(object):
    def test_one(self):
        x = "this"
        assert 'h' in x

    def test_two(self):
        x = "hello"
        assert hasattr(x, 'check')
```

```python
# coding=utf-8
import pytest
from page.LoginPage import LoginPage
from util.ConfigUtil import ConfigUtil
from util.ApiUtil import ApiUtil


@pytest.fixture(scope="session", autouse=True)
def begin():
    api_util = ApiUtil()

    account = api_util.create_account()
    username = account["agencyOwner"]["userName"]
    password = account["agencyOwner"]["password"]
    print username
    login_page = LoginPage()
    login_page.login(username, password)


"""执行当前目录下面的可以进行全部测试"""
if __name__ == '__main__':
    pytest.main()
#调用pytest.main()将导致导入测试及其导入的任何模块。由于python导入系统的缓存机制，pytest.main()从同一进程进行后续调用不会反映调用之间对这些文件的更改。因此，pytest.main()不建议从同一进程进行多次调用（例如，为了重新运行测试）
```

####对于@pytest.fixture（）的用法有：

对于scope可能的值`scope`有：`function`，`class`，`module`，`package`或`session`。

直接说一下吧：

| session  | 在一次Run或Debug中执行的所有case共享一个session,第一个case开始执行的时候session开始，最后一个case执行结束的时候session结束，这些case可能分布在不同的class或module中。 |
| -------- | ---------------------------------------- |
| module   | 一个.py文件可以看作一个module，其表示的范围指该文件中第一个case到最后一个case之间的范围 |
| class    | 表示的范围即class的范围	一个类中执行一次                  |
| function | 表示的范围即function的范围 每次都会执行一次               |

下一个示例将fixture函数放入单独的`conftest.py`文件中，以便来自目录中多个测试模块的测试可以访问fixture函数：

```
# content of conftest.py
import pytest
import smtplib

@pytest.fixture(scope="module")
def smtp_connection():
    return smtplib.SMTP("smtp.gmail.com", 587, timeout=5)
```

源码：

```python
def fixture(scope="function", params=None, autouse=False, ids=None, name=None):

    :arg scope: the scope for which this fixture is shared, one of
                ``"function"`` (default), ``"class"``, ``"module"``,
                ``"package"`` or ``"session"``.

                ``"package"`` is considered **experimental** at this time.

    :arg params: an optional list of parameters which will cause multiple
                invocations of the fixture function and all of the tests
                using it.
                The current parameter is available in ``request.param``.

    :arg autouse: if True, the fixture func is activated for all tests that
                can see it.  If False (the default) then an explicit
                reference is needed to activate the fixture.

    :arg ids: list of string ids each corresponding to the params
                so that they are part of the test id. If no ids are provided
                they will be generated automatically from the params.

    :arg name: the name of the fixture. This defaults to the name of the
                decorated function. If a fixture is used in the same module in
                which it is defined, the function name of the fixture will be
                shadowed by the function arg that requests the fixture; one way
                to resolve this is to name the decorated function
                ``fixture_<fixturename>`` and then use
                ``@pytest.fixture(name='<fixturename>')``.
    """
    if callable(scope) and params is None and autouse is False:
        # direct decoration
        return FixtureFunctionMarker("function", params, autouse, name=name)(scope)
    if params is not None and not isinstance(params, (list, tuple)):
        params = list(params)
    return FixtureFunctionMarker(scope, params, autouse, ids=ids, name=name)

```

具体其他的用法请看官网吧<https://docs.pytest.org/en/latest/usage.html#cmdline>

#### 前置条件，后置条件

##### 模块级别设置 /拆卸

如果在单个模块中有多个测试函数和测试类，则可以选择实现以下fixture方法，这些方法通常会针对所有函数调用一次：

```
def setup_module(module):
    """ setup any state specific to the execution of the given module."""

def teardown_module(module):
    """ teardown any state that was previously setup with a setup_module
    method.
    """

```

从pytest-3.0开始，`module`参数是可选的。

##### 类级别设置 /拆解

类似地，在调用类的所有测试方法之前和之后，在类级别调用以下方法：

```
@classmethod
def setup_class(cls):
    """ setup any state specific to the execution of the given class (which
    usually contains tests).
    """

@classmethod
def teardown_class(cls):
    """ teardown any state that was previously setup with a call to
    setup_class.
    """

```

##### 方法和功能级别

同样，围绕每个方法调用调用以下方法：

```
def setup_method(self, method):
    """ setup any state tied to the execution of the given method in a
    class.  setup_method is invoked for every test method of a class.
    """

def teardown_method(self, method):
    """ teardown any state that was previously setup with a setup_method
    call.
    """

```

从pytest-3.0开始，`method`参数是可选的。

如果您希望直接在模块级别定义测试函数，还可以使用以下函数来实现fixture：

```
def setup_function(function):
    """ setup any state tied to the execution of the given function.
    Invoked for every test function in the module.
    """

def teardown_function(function):
    """ teardown any state that was previously setup with a setup_function
    call.
    """

```

从pytest-3.0开始，`function`参数是可选的。

### YML语言

#### 简介

YAML 是专门用来写配置文件的语言，非常简洁和强大，远比 JSON 格式方便。

YAML在python语言中有PyYAML安装包。

YAML 语言（发音 /ˈjæməl/ ）的设计目标，就是方便人类读写。它实质上是一种通用的数据串行化格式。

#### **它的基本语法规则如下：**

　1、大小写敏感

　2、使用缩进表示层级关系

　3、缩进时不允许使用Tab键，只允许使用空格。

　4、缩进的空格数目不重要，只要相同层级的元素左侧对齐即可

　5、`#` 表示注释，从这个字符一直到行尾，都会被解析器忽略，这个和python的注释一样

####  **YAML 支持的数据结构有三种：**

　1、对象：键值对的集合，又称为映射（mapping）/ 哈希（hashes） / 字典（dictionary）

　2、数组：一组按次序排列的值，又称为序列（sequence） / 列表（list）

　3、纯量（scalars）：单个的、不可再分的值。字符串、布尔值、整数、浮点数、Null、时间、日期

#### Python中读取例子

```python
config.yml
platforName:Andriod
platformVersion:5.1.1
deviceName:127.0.0.1
appPackage:com.lg.
appActivity:com.example.startActivity
ip:127.0.0.1
from appium import webDriver
import yml
file=open('config.yml','r') # r代表读
data=yaml.load(file)
descried_caps={}
descried_caps['platformName']=data['platformName']
descried_caps['deviceName']=data['deviceName']
descried_caps['platformVersion']=data['platformVersion']
dedcried_caps['uuid']=''#真机中需要使用，这个是adb中列出的
dedcried_caps['noRest']='true' #保留上次的
descried_caps['unicodeKeyboard']="ture" #设置后需要把输入法设置为本机的
descried_caps['resetKeyBoard']="true"
descried_caps['app']=data[]''
descried_caps['appPackage']=data[]''
descried_caps['appActivity']=data[]''
driver=webdriver.Romote('http://127.0.0.1:4723',descried_caps)

```

### 日志

#### 日志作用

不管是在项目中开发还是调试都需要用到日志，可以帮助开发人员定位到一定的信息

#### 日志级别

logging模块默认定义了以下几个日志等级，它允许开发人员自定义其他日志级别，但是这是不被推荐的，尤其是在开发供别人使用的库时，因为这会导致日志级别的混乱。

| 日志等级（level） | 描述                                       |
| ----------- | ---------------------------------------- |
| DEBUG       | 最详细的日志信息，典型应用场景是 问题诊断                    |
| INFO        | 信息详细程度仅次于DEBUG，通常只记录关键节点信息，用于确认一切都是按照我们预期的那样进行工作 |
| WARNING     | 当某些不期望的事情发生时记录的信息（如，磁盘可用空间较低），但是此时应用程序还是正常运行的 |
| ERROR       | 由于一个更严重的问题导致某些功能不能正常运行时记录的信息             |
| CRITICAL    | 当发生严重错误，导致应用程序不能继续运行时记录的信息               |

开发应用程序或部署开发环境时，可以使用DEBUG或INFO级别的日志获取尽可能详细的日志信息来进行开发或部署调试；应用上线或部署生产环境时，应该使用WARNING或ERROR或CRITICAL级别的日志来降低机器的I/O压力和提高获取错误日志信息的效率。日志级别的指定通常都是在应用程序的配置文件中进行指定的。

> **说明：**
>
> - 上面列表中的日志等级是从上到下依次升高的，即：DEBUG < INFO < WARNING < ERROR < CRITICAL，而日志的信息量是依次减少的；
> - 当为某个应用程序指定一个日志级别后，应用程序会记录所有日志级别大于或等于指定日志级别的日志信息，而不是仅仅记录指定级别的日志信息，nginx、php等应用程序以及这里要提高的python的logging模块都是这样的。同样，logging模块也可以指定日志记录器的日志级别，只有级别大于或等于该指定日志级别的日志记录才会被输出，小于该等级的日志记录将会被丢弃。

#### logging模块的使用方式介绍

logging模块提供了两种记录日志的方式：

- 第一种方式是使用logging提供的模块级别的函数
- 第二种方式是使用Logging日志系统的四大组件

其实，logging所提供的模块级别的日志记录函数也是对logging日志系统相关类的封装而已。

##### logging模块定义的模块级别的常用函数

| 函数                                     | 说明                     |
| -------------------------------------- | ---------------------- |
| logging.debug(msg, *args, **kwargs)    | 创建一条严重级别为DEBUG的日志记录    |
| logging.info(msg, *args, **kwargs)     | 创建一条严重级别为INFO的日志记录     |
| logging.warning(msg, *args, **kwargs)  | 创建一条严重级别为WARNING的日志记录  |
| logging.error(msg, *args, **kwargs)    | 创建一条严重级别为ERROR的日志记录    |
| logging.critical(msg, *args, **kwargs) | 创建一条严重级别为CRITICAL的日志记录 |
| logging.log(level, *args, **kwargs)    | 创建一条严重级别为level的日志记录    |
| logging.basicConfig(**kwargs)          | 对root logger进行一次性配置    |

其中`logging.basicConfig(**kwargs)`函数用于指定“要记录的日志级别”、“日志格式”、“日志输出位置”、“日志文件的打开模式”等信息，其他几个都是用于记录各个级别日志的函数。

##### logging模块的四大组件

| 组件         | 说明                                       |
| ---------- | ---------------------------------------- |
| loggers    | 提供应用程序代码直接使用的接口                          |
| handlers   | 用于将日志记录发送到指定的目的位置                        |
| filters    | 提供更细粒度的日志过滤功能，用于决定哪些日志记录将会被输出（其它的日志记录将会被忽略） |
| formatters | 用于控制日志信息的最终输出格式                          |

> **说明：** logging模块提供的模块级别的那些函数实际上也是通过这几个组件的相关实现类来记录日志的，只是在创建这些类的实例时设置了一些默认值。

```
import logging
LOG_FORMAT = "%(asctime)s - %(levelname)s - %(message)s"
DATE_FORMAT = "%m/%d/%Y %H:%M:%S %p"

logging.basicConfig(filename='my.log', level=logging.DEBUG, format=LOG_FORMAT, datefmt=DATE_FORMAT)

logging.debug("This is a debug log.")
logging.info("This is a info log.")
logging.warning("This is a warning log.")
logging.error("This is a error log.")
logging.critical("This is a critical log.")
```

#### logging日志模块相关类及其常用方法介绍

下面介绍下与logging四大组件相关的类：Logger, Handler, Filter, Formatter。

##### Logger类

Logger对象有3个任务要做：

- 1）向应用程序代码暴露几个方法，使应用程序可以在运行时记录日志消息；
- 2）基于日志严重等级（默认的过滤设施）或filter对象来决定要对哪些日志进行后续处理；
- 3）将日志消息传送给所有感兴趣的日志handlers。

Logger对象最常用的方法分为两类：配置方法 和 消息发送方法

最常用的配置方法如下：

| 方法                                       | 描述                           |
| ---------------------------------------- | ---------------------------- |
| Logger.setLevel()                        | 设置日志器将会处理的日志消息的最低严重级别        |
| Logger.addHandler() 和 Logger.removeHandler() | 为该logger对象添加 和 移除一个handler对象 |
| Logger.addFilter() 和 Logger.removeFilter() | 为该logger对象添加 和 移除一个filter对象  |

> **关于Logger.setLevel()方法的说明：**
>
> 内建等级中，级别最低的是DEBUG，级别最高的是CRITICAL。例如setLevel(logging.INFO)，此时函数参数为INFO，那么该logger将只会处理INFO、WARNING、ERROR和CRITICAL级别的日志，而DEBUG级别的消息将会被忽略/丢弃。

logger对象配置完成后，可以使用下面的方法来创建日志记录：

| 方法                                       | 描述                          |
| ---------------------------------------- | --------------------------- |
| Logger.debug(), Logger.info(), Logger.warning(), Logger.error(), Logger.critical() | 创建一个与它们的方法名对应等级的日志记录        |
| Logger.exception()                       | 创建一个类似于Logger.error()的日志消息  |
| Logger.log()                             | 需要获取一个明确的日志level参数来创建一个日志记录 |

> **说明：**
>
> - Logger.exception()与Logger.error()的区别在于：Logger.exception()将会输出堆栈追踪信息，另外通常只是在一个exception handler中调用该方法。
> - Logger.log()与Logger.debug()、Logger.info()等方法相比，虽然需要多传一个level参数，显得不是那么方便，但是当需要记录自定义level的日志时还是需要该方法来完成。

那么，怎样得到一个Logger对象呢？一种方式是通过Logger类的实例化方法创建一个Logger类的实例，但是我们通常都是用第二种方式--logging.getLogger()方法。

logging.getLogger()方法有一个可选参数name，该参数表示将要返回的日志器的名称标识，如果不提供该参数，则其值为'root'。若以相同的name参数值多次调用getLogger()方法，将会返回指向同一个logger对象的引用。

> **关于logger的层级结构与有效等级的说明：**
>
> - logger的名称是一个以'.'分割的层级结构，每个'.'后面的logger都是'.'前面的logger的children，例如，有一个名称为 foo 的logger，其它名称分别为 foo.bar, foo.bar.baz 和 foo.bam都是 foo 的后代。
> - logger有一个"有效等级（effective level）"的概念。如果一个logger上没有被明确设置一个level，那么该logger就是使用它parent的level;如果它的parent也没有明确设置level则继续向上查找parent的parent的有效level，依次类推，直到找到个一个明确设置了level的祖先为止。需要说明的是，root logger总是会有一个明确的level设置（默认为 WARNING）。当决定是否去处理一个已发生的事件时，logger的有效等级将会被用来决定是否将该事件传递给该logger的handlers进行处理。
> - child loggers在完成对日志消息的处理后，默认会将日志消息传递给与它们的祖先loggers相关的handlers。因此，我们不必为一个应用程序中所使用的所有loggers定义和配置handlers，只需要为一个顶层的logger配置handlers，然后按照需要创建child loggers就可足够了。我们也可以通过将一个logger的propagate属性设置为False来关闭这种传递机制。

##### Handler类

Handler对象的作用是（基于日志消息的level）将消息分发到handler指定的位置（文件、网络、邮件等）。Logger对象可以通过addHandler()方法为自己添加0个或者更多个handler对象。比如，一个应用程序可能想要实现以下几个日志需求：

- 1）把所有日志都发送到一个日志文件中；
- 2）把所有严重级别大于等于error的日志发送到stdout（标准输出）；
- 3）把所有严重级别为critical的日志发送到一个email邮件地址。
  这种场景就需要3个不同的handlers，每个handler复杂发送一个特定严重级别的日志到一个特定的位置。

一个handler中只有非常少数的方法是需要应用开发人员去关心的。对于使用内建handler对象的应用开发人员来说，似乎唯一相关的handler方法就是下面这几个配置方法：

| 方法                                       | 描述                        |
| ---------------------------------------- | ------------------------- |
| Handler.setLevel()                       | 设置handler将会处理的日志消息的最低严重级别 |
| Handler.setFormatter()                   | 为handler设置一个格式器对象         |
| Handler.addFilter() 和 Handler.removeFilter() | 为handler添加 和 删除一个过滤器对象    |

需要说明的是，应用程序代码不应该直接实例化和使用Handler实例。因为Handler是一个基类，它只定义了素有handlers都应该有的接口，同时提供了一些子类可以直接使用或覆盖的默认行为。下面是一些常用的Handler：

| Handler                                  | 描述                                       |
| ---------------------------------------- | ---------------------------------------- |
| logging.StreamHandler                    | 将日志消息发送到输出到Stream，如std.out, std.err或任何file-like对象。 |
| logging.FileHandler                      | 将日志消息发送到磁盘文件，默认情况下文件大小会无限增长              |
| logging.handlers.RotatingFileHandler     | 将日志消息发送到磁盘文件，并支持日志文件按大小切割                |
| logging.hanlders.TimedRotatingFileHandler | 将日志消息发送到磁盘文件，并支持日志文件按时间切割                |
| logging.handlers.HTTPHandler             | 将日志消息以GET或POST的方式发送给一个HTTP服务器            |
| logging.handlers.SMTPHandler             | 将日志消息发送给一个指定的email地址                     |
| logging.NullHandler                      | 该Handler实例会忽略error messages，通常被想使用logging的library开发者使用来避免'No handlers could be found for logger XXX'信息的出现。 |

##### Formater类

Formater对象用于配置日志信息的最终顺序、结构和内容。与logging.Handler基类不同的是，应用代码可以直接实例化Formatter类。另外，如果你的应用程序需要一些特殊的处理行为，也可以实现一个Formatter的子类来完成。

Formatter类的构造方法定义如下：

```
logging.Formatter.__init__(fmt=None, datefmt=None, style='%')
```

可见，该构造方法接收3个可选参数：

- fmt：指定消息格式化字符串，如果不指定该参数则默认使用message的原始值
- datefmt：指定日期格式字符串，如果不指定该参数则默认使用"%Y-%m-%d %H:%M:%S"
- style：Python 3.2新增的参数，可取值为 '%', '{'和 '$'，如果不指定该参数则默认使用'%'

##### Filter类

Filter可以被Handler和Logger用来做比level更细粒度的、更复杂的过滤功能。Filter是一个过滤器基类，它只允许某个logger层级下的日志事件通过过滤。该类定义如下：

```
class logging.Filter(name='')
    filter(record)
```

比如，一个filter实例化时传递的name参数值为'A.B'，那么该filter实例将只允许名称为类似如下规则的loggers产生的日志记录通过过滤：'A.B'，'A.B,C'，'A.B.C.D'，'A.B.D'，而名称为'A.BB', 'B.A.B'的loggers产生的日志则会被过滤掉。如果name的值为空字符串，则允许所有的日志事件通过过滤。

filter方法用于具体控制传递的record记录是否能通过过滤，如果该方法返回值为0表示不能通过过滤，返回值为非0表示可以通过过滤。

> **说明：**
>
> - 如果有需要，也可以在filter(record)方法内部改变该record，比如添加、删除或修改一些属性。
> - 我们还可以通过filter做一些统计工作，比如可以计算下被一个特殊的logger或handler所处理的record数量等。

现在，我们对logging模块的重要组件及整个日志流处理流程都应该有了一个比较全面的了解，下面我们来看一个例子。

#### 1. 需求

现在有以下几个日志记录的需求：

- 1）要求将所有级别的所有日志都写入磁盘文件中
- 2）all.log文件中记录所有的日志信息，日志格式为：日期和时间 - 日志级别 - 日志信息
- 3）error.log文件中单独记录error及以上级别的日志信息，日志格式为：日期和时间 - 日志级别 - 文件名[:行号] - 日志信息
- 4）要求all.log在每天凌晨进行日志切割

#### 2. 分析

- 1）要记录所有级别的日志，因此日志器的有效level需要设置为最低级别--DEBUG;
- 2）日志需要被发送到两个不同的目的地，因此需要为日志器设置两个handler；另外，两个目的地都是磁盘文件，因此这两个handler都是与FileHandler相关的；
- 3）all.log要求按照时间进行日志切割，因此他需要用logging.handlers.TimedRotatingFileHandler; 而error.log没有要求日志切割，因此可以使用FileHandler;
- 4）两个日志文件的格式不同，因此需要对这两个handler分别设置格式器；

#### 3. 代码实现

```
import logging
import logging.handlers
import datetime

logger = logging.getLogger('mylogger')
logger.setLevel(logging.DEBUG)

rf_handler = logging.handlers.TimedRotatingFileHandler('all.log', when='midnight', interval=1, backupCount=7, atTime=datetime.time(0, 0, 0, 0))
rf_handler.setFormatter(logging.Formatter("%(asctime)s - %(levelname)s - %(message)s"))

f_handler = logging.FileHandler('error.log')
f_handler.setLevel(logging.ERROR)
f_handler.setFormatter(logging.Formatter("%(asctime)s - %(levelname)s - %(filename)s[:%(lineno)d] - %(message)s"))

logger.addHandler(rf_handler)
logger.addHandler(f_handler)

logger.debug('debug message')
logger.info('info message')
logger.warning('warning message')
logger.error('error message')
logger.critical('critical message')
```

all.log文件输出

```
2017-05-13 16:12:40,612 - DEBUG - debug message
2017-05-13 16:12:40,612 - INFO - info message
2017-05-13 16:12:40,612 - WARNING - warning message
2017-05-13 16:12:40,612 - ERROR - error message
2017-05-13 16:12:40,613 - CRITICAL - critical message
```

error.log文件输出

```
2017-05-13 16:12:40,612 - ERROR - log.py[:81] - error message
2017-05-13 16:12:40,613 - CRITICAL - log.py[:82] - critical message
```

#### 配置logging的几种方式

------

作为开发者，我们可以通过以下3中方式来配置logging:

- 1）使用Python代码显式的创建loggers, handlers和formatters并分别调用它们的配置函数；
- 2）创建一个日志配置文件，然后使用`fileConfig()`函数来读取该文件的内容；
- 3）创建一个包含配置信息的dict，然后把它传递个`dictConfig()`函数；

#### 向日志输出中添加上下文信息

------

除了传递给日志记录函数的参数外，有时候我们还想在日志输出中包含一些额外的上下文信息。比如，在一个网络应用中，可能希望在日志中记录客户端的特定信息，如：远程客户端的IP地址和用户名。这里我们来介绍以下几种实现方式：

- 通过向日志记录函数传递一个`extra`参数引入上下文信息
- 使用LoggerAdapters引入上下文信息
- 使用Filters引入上下文信息

#### 实战

```
[loggers]
keys=root,fileLogger,rotatingFileLogger
 
[handlers]
keys=consoleHandler,fileHandler,rotatingFileHandler
 
[formatters]
keys=simpleFormatter
 
[logger_root]
level=DEBUG
handlers=consoleHandler
 
[logger_fileLogger]
level=DEBUG
# 该logger中配置的handler
handlers=fileHandler
# logger 的名称
qualname=fileLogger
propagate=0
 
[logger_rotatingFileLogger]
level=DEBUG
# 这样配置，rotatingFileLogger中就同时配置了consoleHandler,rotatingFileHandler
# consoleHandler 负责将日志输出到控制台
# rotatingFileHandler 负责将日志输出保存到文件中
handlers=consoleHandler,rotatingFileHandler
qualname=rotatingFileLogger
propagate=0
 
[handler_consoleHandler]
class=StreamHandler
level=DEBUG
formatter=simpleFormatter
args=(sys.stdout,)
 
[handler_fileHandler]
class=FileHandler
level=DEBUG
formatter=simpleFormatter
args=('logs/logging.log', 'a')
 
[handler_rotatingFileHandler]
class=handlers.RotatingFileHandler
level=WARNING
formatter=simpleFormatter
args=("logs/rotating_logging.log", "a", 1*1024*1024, 5)
 
[formatter_simpleFormatter]
#format=%(asctime)s - %(name)s - %(levelname)s - %(message)s
format=%(asctime)s - %(module)s - %(thread)d - %(levelname)s : %(message)s
datefmt=%Y-%m-%d %H:%M:%S
```

loggers : 配置logger信息。必须包含一个名字叫做root的logger，当使用无参函数logging.getLogger()时，默认返回root这个logger，其他自定义logger可以通过 logging.getLogger("fileLogger") 方式进行调用
handlers:定义声明handlers信息。常用的handlers包括 StreamHandler（仅将日志输出到kong控制台）、FileHandler（将日志信息输出保存到文件）、RotaRotatingFileHandler（将日志输出保存到文件中，并设置单个日志wenj文件的大小和日志文件个数）
formatter : 设置日志格式
logger_xxx : 对loggers中声明的logger进行逐个配置，且要一一对应
handler_xxx : 对handlers中声明的handler进行逐个配置，且要一一对应

formatter_xxx : 对声明的formatterjinx进行配置

```
logging.config.fileConfig(“logging.conf”)
 
# 输出日志到控制台,获取的是root对应的logger
console_logger = logging.getLogger()
 
# 输出日志到单个文件
file_logger = logging.getLogger(name="fileLogger")
 
# rotatingFileLogger中额consoleHandler输出到控制台，rotatingHandler输出日志到文件
rotating_logger = logging.getLogger(name="rotatingFileLogger")
```

### PageObject+unittest

pageObject是Selenium自动化测试项目的最佳设计模式之一，通过对界面的元素的封装，减少冗余代码，同时在后期维护中，若元素发生定位只需要页面调整封装元素的代码

#### 重构思路

- 将一些公共的内容抽出来
- 元素定位方法和元素属性值与业务代码分离
- 登录功能封装为一个独立的模块
- 使用unittest进行综合管理

```python
config.yml
platforName:Andriod
platformVersion:5.1.1
deviceName:127.0.0.1
appPackage:com.lg.
appActivity:com.example.startActivity
ip:127.0.0.1
#####################
from appium import webDriver
import yml
def appium_desired():
    file=open('config.yml','r') # r代表读
    data=yaml.load(file)
    logger = logging.getLogger('mylogger')
	logger.setLevel(logging.DEBUG)
    descried_caps={}
    descried_caps['platformName']=data['platformName']
    descried_caps['deviceName']=data['deviceName']
    descried_caps['platformVersion']=data['platformVersion']
    dedcried_caps['uuid']=''#真机中需要使用，这个是adb中列出的
    dedcried_caps['noRest']=data['noRest'] #保留上次的
    descried_caps['unicodeKeyboard']=data['unicodeKeyboard'] #设置后需要把输入法设置为本机的
    descried_caps['resetKeyBoard']=data[]
    descried_caps['app']=data[]''
    descried_caps['appPackage']=data[]''
    descried_caps['appActivity']=data[]''
    driver=webdriver.Romote('http://127.0.0.1:4723',descried_caps)
    return driver
if __name__ == '__ main__'
	appium_desired()
```

公共的封装的base.py

```
class BaseView(object):
	def _init_(self,driver):
		self.driver=driver
	def find_element(self,*loc)
		return self.driver.find_element(*loc)
```

封装通用的公共类common.py继承baseView

```
import logging
class Common(BaseView):
    canselbtn=(By,ID,"android:id/button")
	def check_cancelBtn(self):
		logging.info("========check_cancel=========")
		try:
			element =self.driver.find_element(*self.cancelBtn)
		except NoSuchElementExcption:
			logging.info("not found elments")
		else:
			logging.info("click_cancel")
			element.click()
	def check_skipBtn(self,*loc)
		logging.info("========check_cancel=========")
		try:
			element =self.driver.find_element(*self.skipBtn)
		except NoSuchElementExcption:
			logging.info("not found elments")
		else:
			logging.info("click_cancel")
			element.click()
if __name__ == '__main__':
	driver=apium_desired()
	com=Common(driver)
	com.click_updateBtn()
	com.click_skipBtn()
```

封装登录loginview

```python
#导入common,appium_desrie,Common
class LoginView(Common):
	username_type=(By.ID,'')
	passWord_type=(By,ID,'')
	loginBtn=(By.ID,'')
	def login_action(self,username.password)
		self.check_cancelBtn()
		self.check_skipBtn()
		
		logging.info('=============login_action=============')
		logging,info('username is:%s' %username )
		self.driver.find_element(*.self.username_type).send_keys(username)
		logging.info('password is:%s' %password)
		self.driver.find_element(*.self.password_type).send_keys(password)
		logging.info('click loginBtn')
		self.driver.find_element(*.self.loginbtn).click()
if __name__ == '__main__':
	driver=appium_desired()
	l=LoginView(driver)
	l.login_action('用户名','密码')
```

unittest用例封装myunit.py

```python
import unittest
import logging
import appium_desired

class StartEnd(unittest,TestCase):
	def setup(self):
		logging.info('=====setup====')
		self.driver=appium_desired()
	# 执行结束之后
	def tearDown(self):
		logging.info('=====setup====')
		sleep(5)
		self.driver.close_app()
```

用例封装test_login.py

```python
class TestLogin(StartEnd):

	def test_login_zxw2018(self):
		logging.info('------test_login_zow2018------')
		l=LoginView(self,driver)
		l.login_action('自学','123')
	def test_login_zxw2017(self):
		logging.info('------test_login_zow2017------')
		l=LoginView(self,driver)
		l.login_action('账号2','密码2')
	def test_login_error(self):
		logging.info('------test_login_error------')
		l=LoginView(self,driver)
		l.login_action('自学','123')
if __name__ == '__main__':
	unitest,main()
```

### Appium自动化测试框架综合实践

#### 文件夹结构

- **app：存放app的文件夹**
- **baseView: baseViewl类（基类）**
- **common:公共的方法**
- **businessView：业务逻辑**
- **config:配置文件**
- **data:数据文件，数据驱动**
- **log:日志文件输出**
- **report:测试报告文件**
- **screenshots:存放截图**
- **test_case：测试类的模块**
- **test_run：执行测试化用例的入口**

#### driver封装

##### config.yml，log.conf配置

在config文件夹中配置这些信息config.yml

```
platforName:Andriod
platformVersion:5.1.1
deviceName:127.0.0.1
appPackage:com.lg.
appActivity:com.example.startActivity
ip:127.0.0.1
```

在config文件夹中配置日志信息log.conf

```
[loggers]
keys=root,fileLogger,rotatingFileLogger
 
[handlers]
keys=consoleHandler,fileHandler,rotatingFileHandler
 
[formatters]
keys=simpleFormatter
 
[logger_root]
level=DEBUG
handlers=consoleHandler
 
[logger_fileLogger]
level=DEBUG
# 该logger中配置的handler
handlers=fileHandler
# logger 的名称
qualname=fileLogger
propagate=0
 
[logger_rotatingFileLogger]
level=DEBUG
# 这样配置，rotatingFileLogger中就同时配置了consoleHandler,rotatingFileHandler
# consoleHandler 负责将日志输出到控制台
# rotatingFileHandler 负责将日志输出保存到文件中
handlers=consoleHandler,rotatingFileHandler
qualname=rotatingFileLogger
propagate=0
 
[handler_consoleHandler]
class=StreamHandler
level=DEBUG
formatter=simpleFormatter
args=(sys.stdout,)
 
[handler_fileHandler]
class=FileHandler
level=DEBUG
formatter=simpleFormatter
args=('logs/logging.log', 'a')
 
[handler_rotatingFileHandler]
class=handlers.RotatingFileHandler
level=WARNING
formatter=simpleFormatter
args=("logs/rotating_logging.log", "a", 1*1024*1024, 5)
 
[formatter_simpleFormatter]
#format=%(asctime)s - %(name)s - %(levelname)s - %(message)s
format=%(asctime)s - %(module)s - %(thread)d - %(levelname)s : %(message)s
datefmt=%Y-%m-%d %H:%M:%S
```

#### 公共模块的封装

##### common下的desire_caps.py

```python
from appium import webDriver
import yml

CON_LOG='../config/log.conf'
logging.config.fileConfig(CON_LOG)
logging=loging.getLogger()
def appium_desired():
    #file=open('config.yml','r') # r代表读
    #data=yaml.load(file)
    #file.close()
    with open('../config/config.yml','r',encoding='utf-8') as file
    	data=yaml.load()
    descried_caps={}
    descried_caps['platformName']=data['platformName']
    descried_caps['deviceName']=data['deviceName']
    descried_caps['platformVersion']=data['platformVersion']
    dedcried_caps['uuid']=''#真机中需要使用，这个是adb中列出的
    dedcried_caps['noRest']=data['noRest'] #保留上次的
    descried_caps['unicodeKeyboard']=data['unicodeKeyboard'] #设置后需要把输入法设置为本机的
    descried_caps['resetKeyBoard']=data[]
    base_dir = os.path.dirname(os.path.dirname(__file__))
    app_path= os.path.join(base_dir,'app',data[appname])
    descried_caps['app']=app_path
    descried_caps['appPackage']=data[]
    descried_caps['appActivity']=data[]
    driver=webdriver.Romote('http://127.0.0.1:4723',descried_caps)
    return driver
if __name__ == '__ main__'
	appium_desired()
```

##### baseView文件夹下新建BaseView.py

```python
class BaseView(object):
	def _init_(self,driver):
		self.driver=driver
	def find_element(self,*loc)
		return self.driver.find_element(*loc)
    def find_elements(self,*loc)
    	return self.driver.find_elements(*loc)
    def get_window_size(self):
        return self.driver.get_window_size()
    def swipe(self,start_x,start_y,end_x,end_y,duration)
    	return self.driver.swipe(start_x,start_y,end_x,end_y,duration)
```

##### common文件夹下新建common_fun.py

```python
from baseView.baseView import BaseView
from selennium.common.exceptions import NosuchElementException
import logging
from selenuim.webdriver.common.by import By
class Common(BaseView):
    canselbtn=(By.ID,"android:id/button")
    skipBtn=(By.ID,"com.tal.kaoyan:id/tv_skip")
    viewcacel=(By.ID,"com.tal.kaoyan:id/viewcacel")
	#登录后取消按钮
    def check_cancelBtn(self):
		logging.info("========check_cancel=========")
		try:
			element =self.driver.find_element(*self.cancelBtn)
		except NoSuchElementExcption:
			logging.info("not found elments")
		else:
			logging.info("click_cancel")
			element.click()
	def check_skipBtn(self,*loc)
		logging.info("========check_cancel=========")
		try:
			element =self.driver.find_element(*self.skipBtn)
		except NoSuchElementExcption:
			logging.info("not found elments")
		else:
			logging.info("click_cancel")
			element.click()
	def get_size(self):
        x=self.driver.get_window_size()['width']
        y=self.driver.get_window_size()['height']
        return x,y
        
    def swip_left():
        l=get_size()
        x1=int(l[0]*0.9)
        y1=int(l[1]*0.5)
        x2=int(l[0]*0.1)
       	self.swipe(x1,y1,x2,y1,1000)
    #上，上滑操作就是从屏幕的下端点击一个坐标然后往上滑动，x坐标可以不变。Y的开始和结束坐标改进即可。
    def swipe_up():
        l=get_size()
        x1=int(l[0]*0.5)
        y1=int(l[1]*0.75)
        x2=int(l[0]*0.5)
        y2=int(l[1]*0.25)
        self.swipe(x1,y1,x2,y2,1000)
    #下滑就是从屏幕的上端点击一个坐标然后往下滑动到另外一个坐标，x坐标可以不变。Y的开始和结束坐标改变即可。
    def swipe_down():
         l=get_size()
        x1=int(l[0]*0.5)
        y1=int(l[1]*0.25)
        x2=int(l[0]*0.5)
        y2=int(l[1]*0.75)
        self.swipe(x1,y1,x2,y2,1000)
    def swipe_right():
        swipe_down():
         l=get_size()
        x1=int(l[0]*0.25)
        y1=int(l[1]*0.5)
        x2=int(l[0]*0.75)
        y2=int(l[1]*0.5)
        self.swipe(x1,y1,x2,y2,1000)
    # 获得时间
    def getTime(self):
        self.now=time.strftime("%Y-%m-%d %H_%M_%s")
        return self.now
    def getScreenShot(self,module):
        time=self.getTime()    				image_file=os.path.dirname(os.path.dirname(__file__))+'/screenshots/%s_%s.png' %(module,time)
        logging.info('get %s sceenshot' %module)
        self.driver.get_screenshot_as_file(image_file)
   	#广告弹出
	def check_market_ad(self):
        logging.info('========检查登录广告=========')
        try:
            element=self.driver.find_elment(*self.viewcacel)
        except NoSuchElementException:
            pass
        else:
            logging.info('class viewcacel')
            element.click()
     # 数据读取封装    
     def get_cvs_data(self,csv_file,line):
        #获取csv文件制定的数据，cvs_file:cvs路径，line：数据行数
        csv_file = csv.reader(open(csv_file,'r',encoding='UTF-8'))
        #with open(csv_file,'r',encoding='utf-8') as file:
            #reader=csv.reader(file)
        for i,row in enumerate(csv_file,1):
            if i == line:
				return row
            
        
  	
if __name__ == '__main__':
	driver=apium_desired()
	com=Common(driver)
	com.click_updateBtn()
	com.click_skipBtn()
```

#####在businessView文件中实现业务逻辑 loginView.py

```python
class LoginView(Common):
	username_type=(By.ID,'com.tal.kaoyan:id/usernameinput')
	passWord_type=(By,ID,'com.tal.kaoyan:id/passwordinput')
	loginBtn=(By.ID,'')
	def login_action(self,username.password)
		self.check_cancelBtn()
		self.check_skipBtn()
		
		logging.info('=============login_action=============')
		logging,info('username is:%s' %username )
		self.driver.find_element(*.self.username_type).send_keys(username)
		logging.info('password is:%s' %password)
		self.driver.find_element(*.self.password_type).send_keys(password)
		logging.info('click loginBtn')
		self.driver.find_element(*.self.loginbtn).click()
	# 是否已经登录
    def checke_account_alert(self):
        logging.info('=============chect_account_action=============')
        try:
            element=self.driver.find_element(*self.tip_commit)
        except NoSuchElementException:
          	pass
        else:
            logging.info('close 关闭弹窗')
            element.click()
     def check_login_status(self):
        logging.info('=============click_loginstatus==============')
			try:
                self.driver.find_element(*self.button_myself).click()
                self.driver.find_element(*self.username)
            except NoSuchEelementException:
                logging.error('login error')
                self.getScreenShot('login fail') 
                return false #为了断言使用
            else:
                logging.info('login succeess')
                return true
     def logout_action(self):
        logging.info('===============退出==================')
        self.driver.find_element(*self.RigthButton).click()
        self.driver.find_element(*self.logoutBtn).click()
        self.driver.find_element(*self.tip_commit).click()
        
if __name__ == '__main__':
	driver=appium_desired()
	l=LoginView(driver)
	l.login_action('用户名','密码')
    l.check_login_status()
```

##### bussinessView文件中实现业务逻辑registerView.py

```python
class RegisterView(BaseView):
	#定义所有的操作的元素变量
	username=(By.ID,'')
	item_image=(By.ID,'')
	
	def reister_action(self,register_username,register_password,register_email)
	self.check_cancelBtn()
	self.check_skipBtn()
	logging.info('=====================')
	self.driver.find_element(*self.register_text).click()
	
	#头像设置 *self:表示可变参数
	logging.info('set_userheader')
	self.driver.find_element(*self.username").click()
	self.driver.find_element(*self.item_image")[0].click()
	
if __name__=='__main__':
    driver=appium_desire()
    register=RegisterView(driver)
                             
```

##### myunit.py封装：

```
import unittest
import logging
import appium_desired

class StartEnd(unittest,TestCase):
	def setup(self):
		logging.info('=====setup====')
		self.driver=appium_desired()
	# 执行结束之后
	def tearDown(self):
		logging.info('=====setup====')
		sleep(5)
		self.driver.close_app()
```



##### 测试用例注册

```
class RegisterTest(StartEnd):
	
	def test_user_register(self):
		logging.info('======test_user_register======')
		r=RegisterView(self,driver)
		username= 'zxzxc'+'FLY'+str(random.randint(1000,9000))
		password ='ssss'+str(random.randint(1000,9000))
		email ='ssss'+str(random.randint(1000,9000))+'@163.com'
		self.assertTrue(r.register_action(username,password,email))
if __name__ == '__main__':
	unittest.main()
```

##### 测试用例登录

```python
class TestLogin(StartEnd):
	csv_file='../data/account.csv'
	def test_login_zxw2018(self):
		logging.info('------test_login_zow2018------')
		l=LoginView(self,driver)
		data=l.get_csv_data(self,csv_file,2)
		l.login_action(data[0],data[1])
		self.assertTrue(l.check_login_status)
	def test_login_zxw2017(self):
		logging.info('------test_login_zow2018------')
		l=LoginView(self,driver)
		data=l.get_csv_data(self,csv_file,1)
		l.login_action(data[0],data[1])
		self.assertTrue(l.check_login_status)
	def test_login_error(self):
		logging.info('------test_login_error------')
		l=LoginView(self,driver)
		data=l.get_csv_data(self,csv_file,3)
		l.login_action(data[0],data[1])
		self.assertTrue(l.check_login_status,msg='login fail')
if __name__ == '__main__':
	unitest,main()
```

#### 执行测试用例&报告生成

测试全部以test开头的模块

```
test_dir='../test_case'
report_dir='../reports'
discover= unittest.defaultTestloader.discover(test_dir,pattern='test*.py')
now=time.strftime('%Y-%m-%d %H_%M_%S')
report_name=report_dir+'/'+now+'test_report.html'
with open(report_name,'wb') as f:
	runner=BSTestRunner(stream=f,title='',description='')
	runner.runner(discover)
```

#### Bat批处理执行



#### data数据封装

### appium+pytest最佳实践

#### 目录结构

- ​	----apk  放置Apk的文件
  - ----client 获的会话
  - -----common  公共模块
  - -----config
    - ---android
    - ---ios
    - env.ini
  - -----data
  - -----page
    - ----pag1
  - -----report
  - -----test
  - -----util

#### driver封装

```python
# coding=UTF-8
import os

from appium import webdriver
from util.ConfigUtil import ConfigUtil
from util.AppUtil import AppUtil


class DriverClient(object):
    driver = None

    def __init__(self):
        self.app_util = AppUtil()
        self.config_util = ConfigUtil()

    def init_driver(self):
        app_package = self.config_util.get_value('app_package')
        app_activity = self.config_util.get_value('app_activity')
        platform_name = self.config_util.get_value('platform_name')
        platform_version = self.app_util.get_device_version()
        device_name = self.config_util.get_value('device_name')

        desired_caps = {
            'appPackage': app_package,
            'appActivity': app_activity,
            'platformName': platform_name,
            'platformVersion': platform_version,
            'deviceName': device_name
        }

        if self.config_util.get_env("debug") == "true":
            apk_name = self.config_util.get_value('apk_name')
            app_path = os.path.abspath(os.path.join(os.path.dirname(__file__), "..", "apk", apk_name))
            desired_caps['app'] = app_path

        if int(platform_version[0]) > 5:
            desired_caps['automationName'] = "UiAutomator2"

        DriverClient.driver = webdriver.Remote('http://127.0.0.1:4723/wd/hub', desired_caps)
        DriverClient.driver.implicitly_wait(60)

    def get_driver(self):
        if DriverClient.driver is None:
            self.init_driver()
        return DriverClient.driver

```

#### common封装

```python
# coding=UTF-8
from selenium.webdriver.common.by import By

from page.BasePage import BasePage


#
class NoteCommon(BasePage):
    def __init__(self):
        super(NoteCommon, self).__init__()

    # 删除第一条数据
    def delete_first_note(self):
        lists = self.driver.find_elements_by_id("com.lg.newbackend:id/report_item_content")  # 点第一个
        lists[0].click()
        self.driver.find_element_by_id("android:id/button3").click()  # 点删除
        els = self.driver.find_elements_by_class_name(
            "android.widget.Button")  # 按钮数组,单小孩 note 只有取消、删除当前，多小孩 note 有取消、删除所有、删除当前
        els[1].click()  # 点击第二个按钮
        self.should_no_red_icon()

    def delete_first_report(self):
        self.driver.find_elements_by_id("com.lg.newbackend:id/notes_item_content")[0].click()
        self.wait_element_is_enabled("id", "android:id/button3", 5)  # 等待删除按钮
        self.driver.find_element_by_id("android:id/button3").click()
        if self.is_element_exist_id("android:id/button1"):
            self.driver.find_element_by_id(By.ID,"android:id/button1").click()
        else:
            pass
        if self.is_element_exist_id("android:id/button3"):
            self.driver.find_element_by_id("android:id/button3").click()
        else:
            pass
```

#### Page中的Basepage封装

```python
# coding:utf-8
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions as ec
from selenium.webdriver.common.by import By
from appium.webdriver.common.touch_action import TouchAction
from client.DriverClient import DriverClient
from util.ConfigUtil import ConfigUtil


class BasePage(object):
    def __init__(self):
        self.driver = DriverClient().get_driver()
        self.config_util = ConfigUtil()

        ''' 系统控件 '''
        self.sys_button1 = (By.ID, "android:id/button1")
        self.sys_button2 = (By.ID, "android:id/button2")
        self.sys_button3 = (By.ID, "android:id/button3")
        self.sys_message_modal = (By.ID, "android:id/message")

        ''' 界面控件 '''
        self.roster_tab = (By.ID, "homescreen_radiogroup_students")
        self.engagement_tab = (By.ID, "homescreen_radiogroup_report")
        self.portfolio_tab = (By.ID, "homescreen_radiogroup_notes")

    ''' 系统操作 '''

    # 点击 button1
    def click_sys_button1(self):
        self.find_element(self.sys_button1).click()

    # 点击 button1
    def click_sys_button2(self):
        self.find_element(self.sys_button2).click()

    # 点击 button3
    def click_sys_button3(self):
        self.find_element(self.sys_button3).click()

    # 隐藏键盘
    def hide_keyboard(self):
        self.driver.hide_keyboard()

    # 系统返回
    def back(self):
        self.driver.press_keycode(4)

    # 长按控件
    def touch(self, element):
        TouchAction(self.driver).long_press(element).perform()

    # 等待元素消失
    def wait_element_disappear(self, *loc):
        app_package = self.config_util.get_value('app_package')
        by = loc[0][0]
        locator = loc[0][1]

        # try:
        if by == By.ID and not locator.startswith("android:id/"):
            locator = app_package + ":id/" + locator
        loc = (by, locator)

        WebDriverWait(self.driver, 20).until(ec.invisibility_of_element_located(loc))

    # 等待消息框消失
    def wait_modal_disappear(self):
        self.wait_element_disappear(self.sys_message_modal)

    # 获取屏幕大小
    def get_window_size(self):
        size = self.driver.get_window_size()
        x = size['width']
        y = size['height']
        return x, y

    # 屏幕向上滑动
    def swipe_up(self, t=1000):
        s = self.get_window_size()
        print s
        x1 = int(s[0] * 0.35)
        y1 = int(s[1] * 0.85)
        y2 = int(s[1] * 0.25)
        self.driver.implicitly_wait(10)
        self.driver.swipe(x1, y1, x1, y2, t)

    # 屏幕向下滚动
    def swipe_down(self, t=1000):
        s = self.get_window_size()
        x1 = int(s[0] * 0.1)
        y1 = int(s[1] * 0.75)
        y2 = int(s[1] * 0.25)
        self.driver.swipe(x1, y2, x1, y1, t)

    ''' 界面操作 '''

    def click_roster_tab(self):
        self.find_element(self.roster_tab).click()

    def click_engagement_tab(self):
        self.find_element(self.engagement_tab).click()

    def click_portfolio_tab(self):
        self.find_element(self.portfolio_tab).click()

    def find_element(self, *loc):
        app_package = self.config_util.get_value('app_package')
        by = loc[0][0]
        locator = loc[0][1]

        # try:
        if by == By.ID and not locator.startswith("android:id/"):
            locator = app_package + ":id/" + locator
        loc = (by, locator)
        # for a in loc:
        #     print a
        # WebDriverWait(self.driver, 20).until(ec.visibility_of_element_located(loc))
        return self.driver.find_element(*loc)
        # except:
        #     print('Element not found: ' % locator)

    def find_elements(self, *loc):
        app_package = self.config_util.get_value('app_package')
        by = loc[0][0]
        locator = loc[0][1]

        # try:
        if by == By.ID and not by.startswith("android:id/"):
            locator = app_package + ":id/" + locator
        loc = (by, locator)

        WebDriverWait(self.driver, 20).until(ec.visibility_of_element_located(loc))
        return self.driver.find_elements(*loc)
        # except:
        #     print('Elements not found: ' % locator)

    def allow_permission(self, times=2):
        exist = self.exist_element((By.XPATH, "//android.widget.Button[2]"))
        if exist:
            i = 1
            while i <= times:
                allow_el = self.driver.find_element_by_xpath("//android.widget.Button[2]")  # app 权限允许按钮
                allow_el.click()
                i += 1

    # 判断元素是否存在
    def is_element_exist(self, xpath):
        flag = False
        try:
            self.driver.find_element_by_xpath(xpath)
            flag = True
        except:
            flag = False
        finally:
            return flag

    def is_element_exist_id(self, id_value):
        flag = False
        try:
            self.driver.find_element_by_id(id_value)
            flag = True
        except:
            flag = False
        finally:
            return flag

    # 用于判断元素是否存在
    def exist_element(self, *loc):
        try:
            self.find_element(*loc)
            return True
        except:
            return False

    # 用于等待元素可用（元素肯定会出现，只是时间问题）
    def wait_element_is_enabled(self, by, value, timeout):
        self.driver.implicitly_wait(timeout)
        if by == "id":
            try:
                self.driver.find_element_by_id(value).is_enabled()
            except:
                pass
        elif by == "xpath":
            try:
                self.driver.find_element_by_xpath(value).is_enabled()
            except:
                pass
        self.driver.implicitly_wait(8)

    def should_no_red_icon(self):
        # 判断是否显示红色叹号
        self.driver.implicitly_wait(40)
        self.driver.find_element_by_id("com.lg.newbackend:id/menu_refresh").is_enabled()
        self.driver.implicitly_wait(1)
        # time.sleep(3)   # 这里需要动态等待，因为手机、网络等会导致超过4s
        red_icon = "//android.widget.ImageView[@resource-id='com.lg.newbackend:id/unsend_sign']"
        flag = self.is_element_exist(red_icon)  # 判断红色叹号是否存在
        assert flag is False
        self.driver.implicitly_wait(8)

```

#### util中的ConfigUtil封装

```python
# coding=UTF-8
import os
import configparser


class ConfigUtil(object):
    def __init__(self):
        self.config = configparser.ConfigParser()

    def get_env(self, name):
        env_path = os.path.abspath(os.path.join(os.path.dirname(__file__), "..", "config", "env.ini"))
        self.config.read(env_path, "UTF-8")
        return self.config.get('env', name)

    def get_value(self, name):
        env = self.get_env('env')
        platform = self.get_env('platform')

        config_path = os.path.abspath(
            os.path.join(os.path.dirname(__file__), "..", "config", platform + "/" + env + "/config.ini"))
        self.config.read(config_path, "UTF-8")
        return self.config.get('all', name)

```

