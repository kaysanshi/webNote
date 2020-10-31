# **python**

[![BUCZdA.png](https://s1.ax1x.com/2020/10/31/BUCZdA.png)](https://imgchr.com/i/BUCZdA)

## python读取配置文件：

项目中使用的常量，我们把它收集在一个文件中，这就是配置文件。配置文件在项目中是非常必要的，它避免了项目中文件对常量的分散使用，让常量可以统一修改，避免造成修改不全面的问题。常用的配置文件后缀是.ini、.conf、.py，当然还有使用.json、.txt的，推荐使用常用的.ini、.py，配置文件的名字一般是config便于理解和使用。.ini 文件是Initialization File的缩写，即初始化文件，是windows的系统配置文件所采用的存储格式，统管windows的各项配置；.py的配置文件，在python项目中是作为一个包导入，严格来说不是配置文件，而是扩展包。

下面将介绍两类配置文件的使用，一类是.ini、.txt，另一类是.py。

.ini、.txt配置文件使用方法是一致的，只是一个后缀的区别，这里以ini配置文件来介绍，这类配置文件我们使用内置configparser库来使用，它可以实现配置文件的写入、更新、删除、读取等操作非常方便，建议使用这种方式。

## Configparser介绍：

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

1.  基本的读取配置文件
        -read(filename) 直接读取ini文件内容
        -sections() 得到所有的section，并以列表的形式返回
        -options(section) 得到该section的所有option
        -items(section) 得到该section的所有键值对
        -get(section,option) 得到section中option的值，返回为string类型
        -getint(section,option) 得到section中option的值，返回为int类型，还有相应的getboolean()和getfloat() 函数。

2) 基本的写入配置文件
     -add_section(section) 添加一个新的section
     -set( section, option, value) 对section中的option进行设置，需要调用write将内容写入配置文件。



## 实例

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

## global语句使用：

def spam():

​	global wggd

​	wggd = 'ajs'

wggd= 'gloabl'

print (wggd) #输出为ajs

## os.path模块

os.path包含了许多文件名和文件路径的操作有关的函数，使用需要 import os

```
os.path.abspath(path)将换回参数的绝对路径

os.path.isabs(path)如果是一个绝对的路径则返回true

os.path.relpath(path,start) 键返回从start路径到path路径的相对路径的字符串

os.path.dirname(path) 将返回一个字符串包含path参数的中最后一个斜杠的前的所有内容

os.path.basename(path)将返回一个字符串，包含path参数字最后的一个斜杠的所有内容


```

### 读取文件：

file=open("路径") 打开文件

file.read()读取这个路径返回这个字符串

file.readlines()读取一个字符串列表

### 写入文件：

file=open("路径") 打开文件

file.write("paths -strings")

### 保存变量shelve模块：

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

### 用pprint.pformat()函数保存变量

可以将列表或者字典中的内容加载到屏幕，pprint.pformat()返回同样的文本字符串

## 使用selenium模块控制浏览器

selenium模块是让python控制浏览器的，实际点击链接，填写登录信息，就像一个用户操作界面一样

### 一、[Selenium](http://www.ltesting.net/ceshi/open/kygncsgj/selenium/)的[Java](http://www.ltesting.net/ceshi/ruanjianceshikaifajishu/rjcskf)[编程]基本语法

　　在Selenium的基本语法中首先是需要定位到页面元素的，Selenium通过找寻到界面元素然后触发相应的时间，通过页面元素变化判断是否执行了相应的操作。

　　可以通过页面元素的属性获取相应的页面元素。

#### 　　1.通过Identifier(id)定位元素

　　所给出的定位的HTML元素必须要有identifier，如果没有就会报告未找到相应元素的错误。通过ID来标识可以确定唯一性。

　　[Web](http://www.ltesting.net/ceshi/ceshijishu/webcs/)Element ele = By.id("idName");

　　只有当你明确知道元素的id属性，才能使用。

#### 　　2.通过Name定位元素

　　使用id定位元素固然方面，但是id并不是html元素必须的，我们可以使用Name定位元素位置。

　　使用Name定位元素的位置，会匹配第一个与Name匹配的元素。如果页面中有多个相同的Name，可以使用更多的筛选器进行元素筛选的细化。

　　WebElement ele = By.Name("name");

　　只有明确知道原属的那么，才能使用。

#### 　　3.通过Xpath定位元素

　　在一些情况下无法得知页面元素的id和name，还可以使用xpath从已知节点开始定位相应的元素。

　　绝对路径进行定位 xpath=/html/body/form[1]

　　通过元素的相应属性定位例如xpath=//form[@id='loginForm'] 表示从根节点开始查找一个form元素她包含一个属性是id并且里面的值为loginForm

　　找到某一个元素下相应的子元素 xpath=//form[input/@name='username'] 找到Form表单下面的input元素并且该元素有一个属性name且该name的值是username。

　　找到第一个指定的元素 xpath=//input[@name='username'] 扎到第一个input元素里面有一个name属性并且该属性值为username

　　还可以使用更加细分的查找 xpath = //input[@name='continue'][@type='button'] 找到input标签，1.该标签有一个name=continue属性和type=button属性的元素。

#### 　　4.通过连接定位元素

　　WebElement ele = By.LinkText("xxxx");

　　当知晓相应的链接类容就可以定位到相应的元素。

#### 　　5.通过DOM元素定位元素

　　DOM元素是HTML的基本元素，而且只有dom定位可以直接通过document

　　dom=document.getElementById('loginForm')

　　dom=document.forms['loginForm']

　　dom=document.forms[0]

　　document.forms[0].username

　　document.forms[0].elements['username']

　　document.forms[0].elements[0]

　　document.forms[0].elements[3]

#### 　　6.通过tagName，通过标签名称进行定位

　　List eles = By.TagName("tagname");

　　在一个页面中相同的tag标签太多了，如果仅适用单个tagname查询会找到一个列表。

#### 　　7.通过Css进行定位(Cascading Style Sheets)

　　css=form#loginForm

　　css=input[name="username"]

　　css=input.required[type="text"]

　　css=input.passfield

　　css=#loginForm input[type="button"]

　　css=#loginForm input:nth-child(2)

### 二、Selenium错误现场保存方法(截屏和记录日志)

　　知晓了如何定位元素，我们就可以触发页面上相应的BOM事件了。

　　那么当执行页面响应的[自动化](http://www.ltesting.net/ceshi/ceshijishu/zdcs/)操作的时候发生了错误我们应该怎样才能更好的记录相应的错误呢?记录Log日志和保存相应的屏幕错误信息。

　　1.首先记录log日志，可使用为[java](http://www.ltesting.net/ceshi/ruanjianceshikaifajishu/rjcskf)量身定做的log4j进行日志记录(我的另外一篇Log4j如何实现日志分模块，分天，分错误级别进行记录)

　　2.截取错误发生时的屏幕，这样就可以很快的定位错误发生前的操作和错误发生时的错误信息了。

　　分享一段执行截屏的java代码。

　　

```java
/*截屏操作,遇到错误自动截屏存储到指定位置。

　　* 指定保存的路径，然后通过 TakesScreenshot 的 getScreenshotAs进行截屏操作。

　　* WebElement 继承 TakesScreenshot 这个最大能耐焊好的基于了浏览器，返回当前的状态

　　* ——整个当前的HTML元素内容

　　* ——可视化部分的HTML元素

　　* */

　　public static void captureScreenshot(String arg0,WebDriver driver){

　　if(PrivateDataSource.Debug){

　　logger.debug("调试截图功能，并把截图存储到："+PrivateDataSource.screenshotsResultsPath);

　　}

　　String screenshotsResultsPath=PrivateDataSource.screenshotsResultsPath;

　String imagePath = screenshotsResultsPath + File.separator+arg0+"_"

　　+arg0+".png";

　　File screenShotFile = ((TakesScreenshot)driver).getScreenshotAs(OutputType.FILE);

　　try {

　　org.apache.commons.io.FileUtils.copyFile(screenShotFile, new File(imagePath));

　　} catch (IOException e) {

　　e.printStackTrace();

　　if(PrivateDataSource.DebugError){

　　logger.info( logger.getClass().getName()+" selenium输出截图功能失败。以下是错误信息!");

　　logger.error(e.getStackTrace());

　　}

　　}

　　}

```

### 三、Selenium浏览器[兼容性](http://www.ltesting.net/ceshi/ceshijishu/gncs/jrxcs/)[测试]

#### 　　1.向浏览器注入一段可执行的JavaScript脚本

　　在很多情况下我们需要程序触发一段JavaScript脚本，首先我们需要注册相应的JavaScript到浏览器中然后触发执行。

　　WebElement ele = driver.findElement(By.id("SubMenu7").xpath("//table/tbody/tr[9]"));//定位Web页面的元素

　　//((JavascriptExecutor)driver).executeScript("arguments[0].onclick=function(){alert('js has been execute!');}", ele);

　　//为这个元素添加可执行的js

　　((JavascriptExecutor)driver).executeScript("arguments[0].onclick=function()

　　{SelectMenu(this,'AirLineSeasonManage.aspx?TimeStamp=' + TimeStamp());}", ele);//为这个元素添加可执行的js

　　ele.click();

　　以上代码首先找到相应的元素

　　然后向相应的元素里面注入一段可执行脚本

　　最后点击该元素执行这个JavaScript脚本。

#### 　　2.IE浏览器运行注意点

　　首先需要[下载](http://www.ltesting.net/ceshi/down)一个IEDriverServer.exe工具然后放置在任意位置，记录相应的存放path

　　设置浏览器启动路径System.setProperty("[web](http://www.ltesting.net/ceshi/ceshijishu/webcs/)driver.ie.driver", "存放IEDriverServer.exe的path");

　　启动程序开始执行

#### 　　3.Chrome浏览器运行注意点

　　首先需要下载一个chromedriver.exe模拟Chrome浏览器的工具，放在任意位置，记录相应的存放path

　　设置浏览器启动路径System.setProperty("webdriver.chrome.driver", "存放chromedriver.exe的path");

　　启动程序开始执行

#### 　　4.FireFox浏览器中运行注意点

　　直接安装了FireFox浏览器，并且Selenium对FireFox有非常好的支持，所以不需要下载其余的模拟器进行执行验证。

　　如果你的FireFox没有默认安装，即改变了默认的安装路径需要设定运行变量，不然FireFox不会被正常启动的。

　　System.setProperty("webdriver.firefox.bin", "FireFox的安装路径");

　　启动程序开始执行。

## Xpath

**XPath 是一门在 XML 文档中查找信息的语言。XPath 可用来在 XML 文档中对元素和属性进行遍历。**XPath 是 W3C XSLT 标准的主要元素，并且 XQuery 和 XPointer 都构建于 XPath 表达之上。因此，对 XPath 的理解是很多高级 XML 应用的基础

http://www.w3school.com.cn/xpath/xpath_syntax.asp



