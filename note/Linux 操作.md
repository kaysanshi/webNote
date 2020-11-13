## Linux 操作

### Linux常用的命令：

####  文件操作

目录切换：

> ​			cd usr		切换到该目录下usr目录 	
> ​			cd ../		切换到上一层目录
> ​			cd /		切换到系统根目录
> ​			cd ~		切换到用户主目录
> ​			cd -		切换到上一个所在目录

​	查看目录： ls

​	查看所有文件及目录(包括隐藏的)： ls -a

> 该目录下所有目录和文件的详细信息:ls -l  可以显示出文件的权限：
>
> ​			如：drwxr-xr-x   对应的是：文件类型，读取权，写入权，执行权
>
> ​			d：代表目录
>
> ​			-：代表文件
>
> ​			l：代表链接（可以认为是window中的快捷方式）
>
> ​			后面的9位分为3组，每3位置一组，分别代表属主的权限，与当前用户同组的	用户的权限，其他用户的权限
>
> ​			r：代表权限是可读，r也可以用数字4表示
>
> ​			w：代表权限是可写，w也可以用数字2表示
>
> ​			x：代表权限是可执行，x也可以用数字1表示
>
> 

- 查找目录：find 目录 参数

-  查找文件： find / -name file1 从“/” 开始进入根文件系统查找文件和目录

- 修改目录名称(包括压缩包)：mv 目录名称 新目录名称

- 

- 移动目录：mv 目录名称 目录的新位置

- 创建一个目录 ：mkdir 目录名       

- 创建目录：mkdir -pv  ./abc/123:创建层级目录

- 删除目录：rm  目录 都可直接使用 rm -rf 目录/文件/压缩包

- 强制删除文件：rm -f ./xxx.x

- 文件创建： touch 文件名称 touch aaa.txt

- 文件查看：cat/more/less/tail 文件

- 编辑文件：vim  /etc/profile     底行输入:wq退出并保存

- 解压缩：

  - > ​		tar命令  -c 是压缩
    > ​		-x是解压
    > ​		-z是否需要解压gzip压缩
    > ​		-v压缩过程中显示文件
    > ​		-f 使用档名在f 之后立即接档名

  - ​		压缩：tar -zcvf xxx.tar ./目标文件目录

  - ​		解压：tar -zxvf 文件名

  - ​		解压到指定目录： tar -zxvf xx.tar.gz -C /目录


#### 网络操作

**查看ip:**  `ifconfig`    `ip addr`

**查看日志** ： `tail -f xxx.log`

### Ubuntu下操作

#### 安装jdk:

当使用这个命令安装时：

`wget  http://download.oracle.com/otn-pub/java/jdk/8u181-b13/96a7b8442fe848ef90c96a2fad6ed6d1/jdk-8u181-linux-x64.tar.gz`

​	会报错：gzip: stdin: not in gzip format 
​	tar: Child returned status 1 
​	tar: Error is not recoverable: exiting now 

​	需要加上： --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie"因为oracle公司要你接受同意没如果直接不带头格式下载后不能解压：

所以用以下方式就可以了：

```
wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u181-b13/96a7b8442fe848ef90c96a2fad6ed6d1/jdk-8u181-linux-x64.tar.gz
```

**解压**：`tar -zxvf jdk-8u181-linux-x64.tar.gz`

##### **jdk环境配置：**

​		`vim /etc/profile`

	    export JAVA_HOME=/java/jdk1.8.0_181
	    export CLASSPATH=JAVA_HOME/lib/
	    export PATH=PATH:$JAVA_HOME/bin
	    export PATH JAVA_HOME CLASSPATH
	// 方式二
		export JAVA_HOME=/root/java/jdk1.8.0_181
		export JRE_HOME=$JAVA_HOME/jre
		exportCLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
		export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
	// 方式三
		export JAVA_HOME=/usr/local/src/java/jdk1.7.0_71
		export CLASSPATH=.:$JAVA_HOME/lib.tools.jar
		export PATH=$JAVA_HOME/bin:$PATH
		export export JAVA_HOME CLASSPATH PATH
点击esc等下面的insert发生变化输入:qw保存并退出

`保存生效：source /etc/profile`

`重启机器：sudo shutdown -r now`

`输入查看是否生效：javac -version`

`卸载.gz：rm -rf /java/ jdk-8u181-linux-x64.tar.gz`

#### 安装tomcat：

地址去官网看Tomcat的liunx版本的下载的链接直接使用命令

```
wget http://mirrors.shu.edu.cn/apache/tomcat/tomcat-9/v9.0.12/bin/apache-tomcat-9.0.12.tar.gz
```

解压：`tar zxvf  apache-tomcat-9.0.12.tar.gz;`

配置Tomcat环境：

​		`编辑文件：vim  /etc/profile` 

> ​		export CATALINA_HOME=/root/java/apache-tomcat-9.0.16
> ​		export CATALINA_HOME

​	点击esc等下面的insert发生变化输入:qw保存并退出

​		`启用配置文件：source /etc/profile`

​	查看Tomcat的环境 ：

```
echo  CATALINA_HOME
	启动tomcat ：cd /java/ apache-tomcat-9.0.12/bin
				执行常用命令：./startup.sh启动
	或者使用： sudo service apache-tomcat-9.0.12 start    //启动  
				 sudo service apache-tomcat-9.0.12 restart  //重启  
				 sudo service apache-tomcat-9.0.12 stop     //停止  
	卸载：sudo apt-get autoremove  apache-tomcat-9.0.12 
```

这样就可以直接访问了8080断就就可以直接访问了

#### 安装mysql:

查看自带mysql输入 `rpm -qa | grep mysql`

或者 ： `sudo netstat -tap | grep mysql`

在 Ubuntu 16.04 中，默认情况下，只有最新版本的 MySQL 包含在 APT 软件包存储库中。在撰写本文时，那是 MySQL 5.7要安装它，只需更新服务器上的包索引并安装默认包 apt-get。

##### 安装执行以下命令：



> sudo apt-get update
> sudo apt-get install mysql-server

然后等待下载。配置密码

[![DS0PaR.png](https://s3.ax1x.com/2020/11/13/DS0PaR.png)](https://imgchr.com/i/DS0PaR)

再次验证：

[![DS0kPx.png](https://s3.ax1x.com/2020/11/13/DS0kPx.png)](https://imgchr.com/i/DS0kPx)

这样的等待完成安装即可。

##### 配置远程连接



​	编辑 ： `/etc/mysql/mysql.conf.d/mysqld.cnf`

​	将bind-address = 127.0.0.1修改为bind-address = 0.0.0.0

​	加入：character-set-server=utf8使其使用utf-8编码

[![DS0VxO.png](https://s3.ax1x.com/2020/11/13/DS0VxO.png)](https://imgchr.com/i/DS0VxO)

​	

   重新启动并登陆mysql

​	sudo service mysql restart

​	mysql -uroot -p

​	修改权限：

​	``GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'yourpassword' WITH GRANT OPTION;`

​	FLUSH PRIVILEGES;

[![DS0KZd.png](https://s3.ax1x.com/2020/11/13/DS0KZd.png)](https://imgchr.com/i/DS0KZd)

​	重新启动mysql

​	sudo service mysql restart

​	注意：有时重启服务器后mysql未启动导致远程无法连接只需启动mysql

​	sudo service mysql start

​	这样就可以直接连接了



**卸载mysql:**

​			sudo apt purge mysql-*
​			sudo rm -rf /etc/mysql/ /var/lib/mysql
​			sudo apt autoremove
​			sudo apt autoreclean

安装mysql5.5：wget http://dev.mysql.com/get/Downloads/MySQL-5.5/mysql-5.5.46-linux2.6-x86_64.tar.gz

配置和上面的一致即可。

### CentOS下操作：

#### 安装jdk

```
wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u181-b13/96a7b8442fe848ef90c96a2fad6ed6d1/jdk-8u181-linux-x64.tar.gz
```

但是这个方式总是被限制不如直接去上传一个tar包。解压后配置环境变量。

`vim /etc/profile`

```
	export JAVA_HOME=/usr/local/src/java/jdk1.8.0_181
	export CLASSPATH=.:$JAVA_HOME/lib.tools.jar
	export PATH=$JAVA_HOME/bin:$PATH
	export export JAVA_HOME CLASSPATH PATH
```



#### 安装mysql

一般网上给出的资料都是

```
#yum install mysql
#yum install mysql-server
#yum install mysql-devel
```

安装mysql和mysql-devel都成功，但是安装mysql-server失败，如下：

[root@localhost src]# yum install mysql-server
已加载插件：fastestmirror, product-id, search-disabled-repos, subscription-manager

This system is not registered with an entitlement server. You can use subscription-manager to register.

Loading mirror speeds from cached hostfile
 * base: mirrors.163.com
 * extras: mirrors.163.com
 * updates: mirrors.163.com
  没有可用软件包 mysql-server。
  错误：无须任何处

处理方式 

##### 安装方式一：安装MaraiDB

MariaDB数据库管理系统是MySQL的一个分支，主要由开源社区在维护，采用GPL授权许可。开发这个分支的原因之一是：甲骨文公司收购了MySQL后，有将MySQL闭源的潜在风险，因此社区采用分支的方式来避开这个风险。MariaDB的目的是完全兼容MySQL，包括API和命令行，使之能轻松成为MySQL的代替品。

安装mariadb，大小59 M。

```
yum install mariadb-server mariadb 
```

mariadb数据库的相关命令是：

systemctl start mariadb  #启动MariaDB

systemctl stop mariadb  #停止MariaDB

systemctl restart mariadb  #重启MariaDB

systemctl enable mariadb  #设置开机启动

所以先启动数据库

```
systemctl start mariadb
```

这时候是没有密码的：

[![DS68Wn.png](https://s3.ax1x.com/2020/11/13/DS68Wn.png)](https://imgchr.com/i/DS68Wn)

##### 安装方式二直接下载：

使用wget命令直接下载

```
wget http://dev.mysql.com/get/Downloads/MySQL-5.6/MySQL-5.6.22-1.el6.i686.rpm-bundle.tar
```

1、 mkdir /usr/local/src/mysql

2、 cd /usr/local/src/mysql

3    tar -xvf MySQL-5.6.22-1.el6.i686.rpm-bundle.tar

4.安装server: `rpm -ivh MySQL-server-5.6.22-1.el6.i686.rpm`

​	出现问题，需要安装依赖：

​	`yum -y install libaio.so.1 libgcc_s.so.1 libstdc++.so.6`

[![DSffTf.png](https://s3.ax1x.com/2020/11/13/DSffTf.png)](https://imgchr.com/i/DSffTf)

​	再安装： yum install net-tools 

​	[![DSWnUO.png](https://s3.ax1x.com/2020/11/13/DSWnUO.png)](https://imgchr.com/i/DSWnUO)

​	升级： `yum  update libstdc++-4.4.7-4.el6.x86_64`

再进行对其执行安装命令：

[![DSfH6s.png](https://s3.ax1x.com/2020/11/13/DSfH6s.png)](https://imgchr.com/i/DSfH6s)

5.安装client   rpm -ivh MySQL-client-5.6.22-1.el6.i686.rpm 

出现问题：

​	先安装： yum -y install libncurses.so.5 libtinfo.so.5

[![DSWi8J.png](https://s3.ax1x.com/2020/11/13/DSWi8J.png)](https://imgchr.com/i/DSWi8J)

​	

再进行对其执行安装命令：

这样就结束了安装：

1、 使用密码登录mysql账号：mysql -uroot -p

2、 修改root密码：SET PASSWORD = PASSWORD('123456');

##### 配置mysql

- 加入到系统服务：`chkconfig --add mysql`

  

- 自动启动：`chkconfig mysql on`

  

- 查询列表：`chkconfig`



设置远程访问（使用root密码）：

> grant all privileges on *.* to 'root' @'%' identified by '123456'; 
>
> flush privileges;

防火墙打开3306端口

> /sbin/iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
>
> /etc/rc.d/init.d/iptables save
>
> /etc/init.d/iptables status

#### 安装tomcat

直接上传 然后解压

```
export CATALINA_HOME=/usr/local/src/java/apache-tomcat-9.0.16
export CATALINA_HOME
```

​	点击esc等下面的insert发生变化输入:qw保存并退出

​		`启用配置文件：source /etc/profile`

​	查看Tomcat的环境 ：

```
echo  CATALINA_HOME
	启动tomcat ：cd /java/ apache-tomcat-9.0.12/bin
				执行常用命令：./startup.sh启动
	或者使用： sudo service apache-tomcat-9.0.12 start    //启动  
				 sudo service apache-tomcat-9.0.12 restart  //重启  
				 sudo service apache-tomcat-9.0.12 stop     //停止  
	卸载：sudo apt-get autoremove  apache-tomcat-9.0.12 
```



### 利用putty来链接服务器传输文件和操作：

​	去官网直接下载：https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

​    下载后：点击putty.exe 然后把你的实例的公网IP地址输入就可以直接连上去操作了：

	#### 打开psftp:

​	首先需要登陆远程服务器：open hostname  然后输入用户名和密码login，进入之后就是类似于 linux的shell命令 比如 `pwd ls cd put get`

​	由于是远程登陆， 涉及到remote 和local两个工作路径

> cd ls pwd 这一套都是remote的
> local的就在前面加l前缀，比如 lcd lls lpwd都是local的
> put filename 用于把本地当前路劲的filename文件上传到remote的当前路径
> get filename 用于把remote的当前路径的filename文件下载到local的当前路径

#### 文件操作

用命令行的形式上传到服务器：本地目录--->目标目录

上传单个：put E:/node/chrunlee/app.js /home/ubuntu/app.js 

上传目录：put -r E:/源数据代码/love  /java/love直接上传到 /java/apache-tomcat-9.0.12/webapps/love这样就可以直接访问了

移动文件的命令：mv love /java/apache-tomcat-9.0.12/webapps;这样就可以直接访问：

上传文件或者文件夹时要把上传到目标文件中带上你需要上传的文件名：put -r E:/源数据代码/love  /java/love;如果不带的话就会把文件夹中的所有文件放到java目录下了

​	你可以通过命令来查看你的webapps下面有多少文件及目录: cd /java/apache-tomcat-9.0.12/webapps --->然后命令 ls 就可以看到目录下有多少文件了：同样可以直接用putty把软件给传输到你的服务器中

apache存储目录：

​		var/www/execise
​		var/www/stuResource

spring boot 后台运行命令：

​			 nohup java -jar portal.jar > /root/java/log/portal_log.log 2>&1 &

下载的所有的应用程序安装必须放在：usr/local目录下
