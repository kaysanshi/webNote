## Nginx

 ### 代理： 

 代理服务器，客户机在发送请求时，不会直接发送给目的主机，而是先发送给代理服务器，代理服务接受客户机请求之后，再向主机发出，并接收目的主机返回的数据，存放在代理服务器的硬盘中，再发送给客户机。

#### 正向代理：

正向代理，架设在客户机与目标主机之间，只用于代理内部网络对 Internet 的连接请求，客户机必须指定代理服务器,并将本来要直接发送到 Web 服务器上的 Http 请求发送到代理服务器中。

 #### 反向代理：

反向代理服务器架设在服务器端，通过缓冲经常被请求的页面来缓解服务器的工作量，将客户机请求转发给内部网络上的目标服务器；并将从服务器上得到的结果返回给 Internet 上请求连接的客户端，此时代理服务器与目标主机一起对外表现为一个服务器。

### 虚拟主机：

​        虚拟主机是一种特殊的软硬件技术，它可以将网络上的每一台计算机分成多个虚拟主机，每个虚拟主机可以独立对外提供 www 服务，这样就可以实现一台主机对外提供多个 web 服务，每个虚拟主机之间是独立的，互不影响的。

通过 Nginx 可以实现虚拟主机的配置，Nginx 支持三种类型的虚拟主机配置

- 基于 IP 的虚拟主机
- 基于域名的虚拟主机
- 基于端口的虚拟主机

### nginx的使用：

​	Nginx 是一款高性能的 HTTP 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器。由俄罗斯的程序设计师 Igor Sysoev 所开发，官方测试 Nginx 能够支支撑 5 万并发链接，并且 CPU、内存等资源消耗却非常低，运行非常稳定。

​	Nginx既可以在内部的直接支持Rails和PHP程序对外进行服务,也可以支持HTTP代理服务对外进行服务，采用C语言编写,处理静态文件，索引文件以及自动索引;打开文件描述符缓冲。无缓存的反向代理加速，简单的负载均衡和容错。FastCGI，简单的负载均衡和容错。模块化的结构。包括 gzipping, byte ranges, chunked responses,以及 SSI-filter 等 filter。如果由 FastCG或其它代理服务器处理单页中存在的多个 SSI，则这项处理可以并行运行，而不需要相互等待。支持 SSL 和 TLSSNI。

  ####  Nginx 的应用场景

- HTTP 服务器：Nginx 是一个 HTTP 服务可以独立提供 HTTP 服务。可以做网页静态服务器。

- 虚拟主机：可以实现在一台服务器虚拟出多个网站。例如个人网站使用的虚拟主机。

- 反向代理，负载均衡：当网站的访问量达到一定程度后，单台服务器不能满足用户的请求时，需要用多台服务器集群可以使用 Nginx 做反向代理。并且多台服务器可以平均分担负载，不会因为某台服务器负载高宕机而某台服务器闲置的情况 .

  负载均衡，英文名称为 Load Balance，其意思就是分摊到多个操作单元上进行执行，例如 Web 服务器、FTP 服务器、企业关键应用服务器和其它关键任务服务器等，从而共同完成工作任务

##### 1.docker-compose.yml来使用nginx:

```yaml
version: '3.1'
  services:
  nginx:
    restart: always
    image: nginx
    container_name: nginx
    ports:
      - 81:80
    volumes:
      - ./conf/nginx.conf:/etc/nginx/nginx.conf
      - ./wwwroot:/usr/share/nginx/wwwroot
 # 配置nginx的数据卷：
  在dokcer文件下创建：nginx的目录下创建：nginx.conf文件：
  在当前目录创建wwwroot目录
  这里直接书写为
  /usr/local/dokcer/nginx/conf/nginx.conf
  /usr/local/dokcer/nginx/wwwroot
  在nginx.conf目录配置nginx的虚拟主机：
```

##### 2.nginx中使用虚拟主机配置：

​       需求

​            Nginx 对外提供 80 和 8080 两个端口监听服务

​            请求 80 端口则请求 html80 目录下的 html

​            请求 8080 端口则请求 html8080 目录下的 html

###### **创建目录及文件**

​            在 /usr/local/docker/nginx/wwwroot 目录下创建 html80 和 html8080 两个目录，并分辨创建两个 index.html 文件

```yml
# 配置虚拟主机
修改 /usr/local/docker/nginx/conf 目录下的 nginx.conf 配置文件：
worker_processes  1;

events {
	worker_connections  1024;
}

http {
	include       mime.types;
	default_type  application/octet-stream;

	sendfile        on;

		keepalive_timeout  65;
	# 配置虚拟主机 192.168.75.145
    server {
		# 监听的ip和端口，配置 192.168.75.145:80
		listen       80;
		# 虚拟主机名称这里配置ip地址
		server_name  192.168.75.145;
		# 所有的请求都以 / 开始，所有的请求都可以匹配此 location
		location / {
			# 使用 root 指令指定虚拟主机目录即网页存放目录
			# 比如访问 http://ip/index.html 将找到 /usr/local/docker/nginx/wwwroot/html80/index.html
			# 比如访问 http://ip/item/index.html 将找到 /usr/local/docker/nginx/wwwroot/html80/item/index.html

			root   /usr/share/nginx/wwwroot/html80;
				# 指定欢迎页面，按从左到右顺序查找
			index  index.html index.htm;
		}
	}
	# 配置虚拟主机 192.168.75.245
	server {
		listen       8080;
		server_name  192.168.75.145;

		  location / {
			root   /usr/share/nginx/wwwroot/html8080;
			index  index.html index.htm;
		  }
	}
}
    说明：这里的启动的端口必须和dockercompose中的nginx的启动端口一一对应：
    例如：这里有两个分别为8080和80那么port应这样写
    ports:
      - 80:80
      - 8080： 8080
    同时这里的location不能改变，改变的话也是改变对应的数据卷，其实就是这个文件映射到数据卷位置
    这里不需要改变只需改变html8000这个就可以：
    location / {
      root   /usr/share/nginx/wwwroot/html8080;
      index  index.html index.htm;
    }
```

###### 基于域名的虚拟主机配置

​	需求:两个域名指向同一台 Nginx 服务器，用户访问不同的域名显示不同的网页内容 两个域名是 admin.service.itoken.funtl.com 和 admin.web.itoken.funtl.comNginx 服务器使用虚拟机 192.168.75.145

**配置 Windows Hosts 文件**        

通过 host 文件指定 admin.service.itoken.funtl.com 和 admin.web.itoken.funtl.com 对应 192.168.75.145 虚拟机：
        修改 window 的 hosts 文件：（C:\Windows\System32\drivers\etc）

```
worker_processes  1;
events {
	worker_connections  1024;
}

http {
	include       mime.types;
	default_type  application/octet-stream;
	sendfile        on;
	keepalive_timeout  65;
	# 配置虚拟主机 192.168.75.145
​    server {
		# 监听的ip和端口，配置 192.168.75.145:80
		listen       80;
		# 虚拟主机名称这里配置ip地址
		server_name  www.kay.com;
		# 所有的请求都以 / 开始，所有的请求都可以匹配此 location
		location / {
			# 使用 root 指令指定虚拟主机目录即网页存放目录
			# 比如访问 http://ip/index.html 将找到 					 /usr/local/docker/nginx/wwwroot/html80/index.html
			# 比如访问 http://ip/item/index.html 将找到 /usr/local/docker/nginx/wwwroot/html80/item/index.html
			root   /usr/share/nginx/wwwroot/html80;
			# 指定欢迎页面，按从左到右顺序查找
​            index  index.html index.htm;
​        }
​    }
	# 配置虚拟主机 192.168.75.245
	server {
	   listen       8080;
	   server_name  192.168.75.145;
		location / {
			root   /usr/share/nginx/wwwroot/html8080;
			index  index.html index.htm;
		}
	}
}
通过 host 文件指定 admin.service.itoken.funtl.com 和 admin.web.itoken.funtl.com 对应 192.168.75.145 虚拟机：
这样通过域名即可访问：
# 创建目录及文件
在 /usr/local/docker/nginx/wwwroot 目录下创建 htmlservice 和 htmlweb 两个目录，并分辨创建两个 index.html 文件
# 配置虚拟主机

user  nginx;
worker_processes  1;

events {
	worker_connections  1024;
}

http {
	include   mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    server {
    	listen       80;
      	server_name  admin.service.itoken.funtl.com;
      	location / {
          	root   /usr/share/nginx/wwwroot/htmlservice;
          	index  index.html index.htm;
      	}
	}

    server {
		listen       80;
        server_name  admin.web.itoken.funtl.com;

       location / {
             root   /usr/share/nginx/wwwroot/htmlweb;
             index  index.html index.htm;
         }
    }                      
}
```

 ##### 3.使用nginx反向代理tomcat:

[![Dk7kWj.jpg](https://s3.ax1x.com/2020/11/16/Dk7kWj.jpg)](https://imgchr.com/i/Dk7kWj)

```yml
（1） 启动两个tomcat：在dokcer-compose.yml 编辑：
version: '3'
  services:
    tomcat1:
      image: tomcat
      container_name: tomcat1
      ports:
        - 9090:8080

    tomcat2:
      image: tomcat
      container_name: tomcat2
      ports:
        - 9091:8080

(2) local/docker/nginx/conf 目录下的 nginx.conf 配置文件：
user  nginx;
worker_processes  1;

events {
	worker_connections  1024;
}

http {
	include       mime.types;
	default_type  application/octet-stream;

	sendfile        on;

	keepalive_timeout  65;
	

	# 配置一个代理即 tomcat1 服务器

	upstream tomcatServer1 {
		server 192.168.75.145:9090;
	}

    #配置一个代理即 tomcat2 服务器

	upstream tomcatServer2 {
		server 192.168.75.145:9091;
	}
    # 配置一个虚拟主机
	server {
		listen 80;
		server_name admin.service.itoken.funtl.com;
		location / {
			#域名 admin.service.itoken.funtl.com 的请求全部转发到 tomcat_server1 即 tomcat1 服务上
			#可以直接书写tomcat的路径即可
			proxy_pass http://tomcatServer1;
			#欢迎页面，按照从左到右的顺序查找页面
			index index.jsp index.html index.htm;
		}
	}

	server {
		listen 80;
		server_name admin.web.itoken.funtl.com;
		location / {
			#域名 admin.web.itoken.funtl.com 的请求全部转发到 tomcat_server2 即 tomcat2 服务上
			proxy_pass http://tomcatServer2;
			index index.jsp index.html index.htm;
		}
	}
} 
 
（3）启动nginx在docker-compose.yml中配置：
version: '3'
  services:
    nexus:
      image: 'sonatype/nexus3'
      restart: always
      container_name: nexus
      ports:
        - '8081:8081'
      volumes:
        - '/usr/local/docker/nexus/data:/nexus-data'
    nignx:
      restart: always
      image: nginx
      container_name: nginx
      ports:
        - '8088:8088'
        - '9000:9000'
        - '80:80'
        - '/usr/local/docker/nginx/conf/nginx.conf:/etc/nginx/nginx.conf'
        - '/usr/local/docker/nginx/wwwroot:/usr/share/nginx/wwwroot'         
```

```

# 定义负载均衡设备的 Ip及设备状态 
upstream myServer {
    server 127.0.0.1:9090 down;
    server 127.0.0.1:8080 weight=2;
    server 127.0.0.1:6060;
    server 127.0.0.1:7070 backup;
}
```

- `upstream`：每个设备的状态:

- `down`：表示当前的 `server` 暂时不参与负载

- `weight`：默认为 1 `weight` 越大，负载的权重就越大。

- `max_fails`：允许请求失败的次数默认为 1 当超过最大次数时，返回 `proxy_next_upstream` 模块定义的错误

- `fail_timeout`:`max_fails` 次失败后，暂停的时间。

- `backup`：其它所有的非 `backup` 机器 `down` 或者忙的时候，请求 `backup` 机器。所以这台机器压力会最轻

  ![img](https://img-blog.csdnimg.cn/20190624193234777.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3MjU2ODk2,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/2019062419304284.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3MjU2ODk2,size_16,color_FFFFFF,t_70)

 ##### 4.实战：

```xml
 在一个虚拟主机配置两个tomcat:
1.vim /etc/profile
    export CATALINA1_BASE="tomcat路径"
    export CATALINA1_HOME="tomcat路径"
    export Tomcat1Home=CATALINA1_BASE
    export CATALINA2_BASE="tomcat2路径"
    export CATALINA2_HOME="tomcat2路径"
    export Tomcat2Home=CATALINA2_BASE
2.在bin文件中修改 catalina.sh
在首行加入：
    export CATALINA1_BASE=CATALINA1_BASE
    export CATALINA1_HOME=CATALINA1_HOME
3.修改host：
    8005→9005
    8009→9009
    8080→9080
    8443→9443
4. ./startup.sh启动即可
```

#### Nginx安装

##### 常规安装( centos下）：

1.先安装gcc升级: `yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel`

[![DkLrUf.md.png](https://s3.ax1x.com/2020/11/16/DkLrUf.md.png)](https://imgchr.com/i/DkLrUf)

2.下载PCRE安装包： `wget http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz`。

[![DkL0bt.md.png](https://s3.ax1x.com/2020/11/16/DkL0bt.md.png)](https://imgchr.com/i/DkL0bt)

3.解压安装包：

```
[root@localhost src]# tar zxvf pcre-8.35.tar.gz 
```

4.编译安装：

```
[root@localhost pcre-8.35]# ./configure
[root@bogon pcre-8.35]# make && make install
```

5.查看pcre版本

```
[root@bogon pcre-8.35]# pcre-config --version
```

[![DkLDVP.png](https://s3.ax1x.com/2020/11/16/DkLDVP.png)](https://imgchr.com/i/DkLDVP)

6.下载安装nginx

`wget http://nginx.org/download/nginx-1.10.2.tar.gz`

7.进入nginx目录编译安装

```
[root@localhost nginx-1.10.2]# ./configure --prefix=/usr/local/webserver/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/local/src/pcre-8.35
[root@localhost nginx-1.10.2]# make
[root@localhost nginx-1.10.2]# make install
```

8.查看版本：

```
[root@localhost nginx-1.10.2]# /usr/local/webserver/nginx/sbin/nginx -v
```

[![DkOkMd.png](https://s3.ax1x.com/2020/11/16/DkOkMd.png)](https://imgchr.com/i/DkOkMd)

**这里的配置使用的是webserver就是存放的文件的路径**

**关闭防火墙强：使其80端口开放：systemctl stop firewalld**

**访问：**

[![DkjV4f.png](https://s3.ax1x.com/2020/11/16/DkjV4f.png)](https://imgchr.com/i/DkjV4f)



##### 常规安装Ubuntu下：

1.先安装gcc : `sudo apt-get update`

2.安装 依赖库： 

`apt-get install zlib1g-dev`

`apt-get install openssl`

3.下载PCRE安装包： `wget http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz`。

[![DkL0bt.md.png](https://s3.ax1x.com/2020/11/16/DkL0bt.md.png)](https://imgchr.com/i/DkL0bt)

4.解压安装包：

```
[root@localhost src]# tar zxvf pcre-8.35.tar.gz 
```

5.编译安装：

```
[root@localhost pcre-8.35]# ./configure
[root@bogon pcre-8.35]# make && make install
```

6.查看pcre版本

```
[root@bogon pcre-8.35]# pcre-config --version
```

[![DkLDVP.png](https://s3.ax1x.com/2020/11/16/DkLDVP.png)](https://imgchr.com/i/DkLDVP)

7.下载安装nginx

`wget http://nginx.org/download/nginx-1.10.2.tar.gz`

8.进入nginx目录编译安装

```
[root@localhost nginx-1.10.2]# ./configure --prefix=/usr/local/webserver/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/local/src/pcre-8.35
[root@localhost nginx-1.10.2]# make
[root@localhost nginx-1.10.2]# make install
```

9.查看版本：

```
[root@localhost nginx-1.10.2]# /usr/local/webserver/nginx/sbin/nginx -v
```

[![DkOkMd.png](https://s3.ax1x.com/2020/11/16/DkOkMd.png)](https://imgchr.com/i/DkOkMd)

**这里的配置使用的是webserver就是存放的文件的路径**

**关闭防火墙强：使其80端口开放：systemctl stop firewalld**

##### Docker安装Nginx

`docker pull nginx:latest`

运行容器

`docker run --name nginx-test -p 8080:80 -d nginx`

- **--name nginx-test**：容器名称。
- **-p 8080:80**： 端口进行映射，将本地 8080 端口映射到容器内部的 80 端口。
- **-d nginx**： 设置容器在在后台一直运行。

##### DockerCompose安装Nginx

```yaml
version: '3.1'
  services:
  nginx:
    restart: always
    image: nginx
    container_name: nginx
    ports:
      - 81:80
    volumes:
      - ./conf/nginx.conf:/etc/nginx/nginx.conf
      - ./wwwroot:/usr/share/nginx/wwwroot
 # 配置nginx的数据卷：
  在dokcer文件下创建：nginx的目录下创建：nginx.conf文件：
  在当前目录创建wwwroot目录
  这里直接书写为
  /usr/local/dokcer/nginx/conf/nginx.conf
  /usr/local/dokcer/nginx/wwwroot
  在nginx.conf目录配置nginx的虚拟主机：
```

**运行**：`docker-compose up`

#### Nginx命令：

##### windows下：

cmd 进入Nginx解压目录 执行以下命令

`start nginx` : 启动nginx服务

`nginx -s reload` ：修改配置后重新加载生效

`nginx -s reopen` ：重新打开日志文件 `nginx -t -c /path/to/nginx.conf` 测试nginx配置文件是否正确

`nginx -t`：验证配置是否正确

`nginx -V`：查看Nginx的版本号

`nginx -s stop`：快速停止或关闭Nginx

`nginx -s quit`：正常停止或关闭Nginx

##### linux下：

**启动**

`./nginx` 可跟后面的以下参数

```

-c </path/to/config> 为 Nginx 指定一个配置文件，来代替缺省的。路径应为绝对路径

-t 不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件。

-v 显示 nginx 的版本。

-V 显示 nginx 的版本，编译器版本和配置参数。
```

直接使用./nginx 进行启动。

**关闭：**

```
ps -aux|grep nginx
kill -9 nginx主进程号
```

#### Nginx配置：

```yaml
########### 每个指令必须有分号结束。#################
#user administrator administrators;  #配置用户或者组，默认为nobody nobody。
#worker_processes 2;  #允许生成的进程数，默认为1
#pid /nginx/pid/nginx.pid;   #指定nginx进程运行文件存放地址
error_log log/error.log debug;  #制定日志路径，级别。这个设置可以放入全局块，http块，server块，级别以此为：debug|info|notice|warn|error|crit|alert|emerg
events {
    accept_mutex on;   #设置网路连接序列化，防止惊群现象发生，默认为on
    multi_accept on;  #设置一个进程是否同时接受多个网络连接，默认为off
    #use epoll;      #事件驱动模型，select|poll|kqueue|epoll|resig|/dev/poll|eventport
    worker_connections  1024;    #最大连接数，默认为512
}
http {
    include       mime.types;   #文件扩展名与文件类型映射表
    default_type  application/octet-stream; #默认文件类型，默认为text/plain
    #access_log off; #取消服务日志    
    log_format myFormat '$remote_addr–$remote_user [$time_local] $request $status $body_bytes_sent $http_referer $http_user_agent $http_x_forwarded_for'; #自定义格式
    access_log log/access.log myFormat;  #combined为日志格式的默认值
    sendfile on;   #允许sendfile方式传输文件，默认为off，可以在http块，server块，location块。
    sendfile_max_chunk 100k;  #每个进程每次调用传输数量不能大于设定的值，默认为0，即不设上限。
    keepalive_timeout 65;  #连接超时时间，默认为75s，可以在http，server，location块。

    upstream mysvr {   
      server 127.0.0.1:7878;
      server 192.168.10.121:3333 backup;  #热备
    }
    error_page 404 https://www.baidu.com; #错误页
    server {
        keepalive_requests 120; #单连接请求上限次数。
        listen       4545;   #监听端口
        server_name  127.0.0.1;   #监听地址       
        location  ~*^.+$ {       #请求的url过滤，正则匹配，~为区分大小写，~*为不区分大小写。
           #root path;  #根目录
           #index vv.txt;  #设置默认页
           proxy_pass  http://mysvr;  #请求转向mysvr 定义的服务器列表
           deny 127.0.0.1;  #拒绝的ip
           allow 172.18.5.54; #允许的ip           
        } 
    }
}
```

- 1、**全局块**：配置影响nginx全局的指令。一般有运行nginx服务器的用户组，nginx进程pid存放路径，日志存放路径，配置文件引入，允许生成worker process数等。
- 2、**events块**：配置影响nginx服务器或与用户的网络连接。有每个进程的最大连接数，选取哪种事件驱动模型处理连接请求，是否允许同时接受多个网路连接，开启多个网络连接序列化等。
- 3、**http块**：可以嵌套多个server，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置。如文件引入，mime-type定义，日志自定义，是否使用sendfile传输文件，连接超时时间，单连接请求数等。
- 4、**server块**：配置虚拟主机的相关参数，一个http中可以有多个server。
- 5、**location块**：配置请求的路由，以及各种页面的处理情况。

**需要注意以下几点：**

1、几个常见配置项：

- 1.`$remote_addr` 与 ​`$http_x_forwarded_for` 用以记录客户端的ip地址；
- 2.$remote_user ：用来记录客户端用户名称；
- 3.$time_local ： 用来记录访问时间与时区；
- 4.$request ： 用来记录请求的url与http协议；
- 5.$status ： 用来记录请求状态；成功是200；
- 6.$body_bytes_s ent ：记录发送给客户端文件主体内容大小；
- 7.$http_referer ：用来记录从那个页面链接访问过来的；
- 8.$http_user_agent ：记录客户端浏览器的相关信息；

2、惊群现象：一个网路连接到来，多个睡眠的进程被同时叫醒，但只有一个进程能获得链接，这样会影响系统性能。

3、每个指令必须有分号结束。

#### Nginx载均衡机制

主要的算法有：

1. `weight轮训(默认)`：接收到的请求按照顺序逐一分配到不同的后端服务器，如果某个服务器拓机的情况下，nginx会将其剔除队列，请求受理情况不会受到影响，可以给不同的后端服务配置权重值，用于调整不同的服务器上请求的分配率，权重越大被分配到的请求的几率越大
2. `ip_hash`:每个请求按照发起客户端的ip的hash结果进行匹配，这样的算法下一个固定ip地址的客户端总会访问到同一个后端服务器，这也在一定程度上解决了集群部署环境下session共享的问题
3. `fair`：智能调整调度算法，动态的根据后端服务器的请求处理到响应的时间进行均衡分配，响应时间短处理效率高的服务器分配到请求的概率高，响应时间长处理效率低的服务器分配到的请求少；结合了前两者的优点的一种调度算法。但是需要注意的是nginx默认不支持fair算法，如果要使用这种调度算法，请安装upstream_fair模块
4. `url_hash`：按照访问的url的hash结果分配请求，每个请求的url会指向后端固定的某个服务器，可以在nginx作为静态服务器的情况下提高缓存效率。同样要注意nginx默认不支持这种调度算法，要使用的话需要安装nginx的hash软件包
   ![img](https://www.runoob.com/wp-content/uploads/2018/08/1535725078-1224-20160201162405944-676557632.jpg)		

