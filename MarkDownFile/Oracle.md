# Oracle

为了使用方便直接使用docker进行使用。

## docker安装 oracle

1.拉去oracle数据库镜像
docker pull registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g 
2.启动oracle  自动启动镜像 --restart=always
docker run -p 1521:1521 --name oracle_11g -d --restart=always registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
3.启动服务
docker start oracle_11g
4.进入控制台设置用户信息 
docker exec -it oracle_11g bash
5.切换到root用户模式下
su root
输入密码helowin
6.编辑profile文件配置ORACLE环境变量
export ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/dbhome_2
export ORACLE_SID=helowin
export PATH=$ORACLE_HOME/bin:$PATH
7.重启配置文件服务
source /etc/profile
8.建立sqlplus软连接
ln -s $ORACLE_HOME/bin/sqlplus /usr/bin
9.切换到oracle用户，修改oracle的相关账号密码

> su oracle
> //登录sqlplus并修改sys、system用户密码
> sqlplus /nolog
> conn /as sysdba
> alter user system identified by oracle;  # 将system的密码改为oracle
> alter user sys identified by oracle; # 将sys的密码改为oracle
> ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED; # 更改配置文件默认限制密码

10.使用navicat进行连接：

[![0wqRa9.png](https://s1.ax1x.com/2020/10/08/0wqRa9.png)](https://imgchr.com/i/0wqRa9)

![0wq9E9.png](https://s1.ax1x.com/2020/10/08/0wq9E9.png)

## pom.xml中配置oracle连接不生效

对于oracle如果直接在pom.xml直接配置：

```xml
<dependency>    <groupId>com.oracle</groupId>    <artifactId>ojdbc6</artifactId>    <version>11.2.0.1.0</version></dependency>
```

<font color="red">这样是不可以使用的会报错找不到驱动   需要在oracle在官网下载然后安装到maven仓库中</font>

mvn install:install-file -Dfile=C:\Users\desktop\ojdbc6.jar -DgroupId=com.oracle -DartifactId=ojdbc6 -Dversion=11.2.0.1.0 -Dpackaging=jar -DgeneratePom=true

其中，Dfile为下载的驱动的路径，本例中为C:\Users\desktop\ojdbc6.jar

然后在pom.xml使用以上就不会报找不到驱动了。但是通过配置说找不到表：

在spring 配置文件 application.yml配置如下：

```yml
spring:
  datasource:
#   数据源基本配置
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: oracle.jdbc.driver.OracleDriver
    url: jdbc:oracle:thin:@192.168.147.132:1521:helowin
    username: system
    password: oracle
```



## oracle中的sys用户和system用户的区别：

【sys】所有 oracle 的数据字典的基表和视图都存放在 sys 用户中，这些基表和视图对于 oracle 的运行是至关重要的，由数据库自己维护，任何用户都不能手动更改。 sys 用户拥有 dba ， sysdba ， sysoper 等角色或权限，是 oracle 权限最高的用户。

【 system 】 用户用于存放次一级的内部数据，如 oracle 的一些特性或工具的管理信息。 system 用户拥有普通 dba 角色权限。

 【system 】用户只能用 normal 身份登陆 em ，除非你对它授予了 sysdba 的系统权限(grant sysdba to system)或者 sysoper 系统权限。
【 sys 】用户具有 “SYSDBA” 或者 “SYSOPER” 系统权限，登陆 em 也只能用这两个身份，不能用 normal 。

## Oracle 基本数据概念

### 数据库

oracle数据库是数据的物理存储（ORA,DBF,控制文件，联机文件，参数文件），这里的Oracle数据库是一个操作系统只有一个库。可以看做Oracle只有一个大数据库。

### 实例

一个Oracle实例有一系列的后台进程和内存结构，一个数据库可以有n个实例。

### 用户

用户是在实例下建立的，一个实例可以有多个用户。

### 表空间

表空间是Oracle对物理数据库上相关数据文件（ORA 或者DBF文件）

一个数据库逻辑上被划分成一到若干个表空间，每个表空间包含了再逻辑上相关联的一组结构。每个数据库至少有一个表空间，system表空间。

oracle数据源：

driver: oracle.jdbc.OracleDriver

url: jdbc:oracle:thin:@localhost:1521:ord

username :zhansan

password:123456 

使用jdbc连接时有两种方式的连接：

**格式一: Oracle JDBC Thin using an SID:**jdbc:oracle:thin:@host:port:SID

Example: jdbc:oracle:thin:@localhost:1521:orcl

**格式二: Oracle JDBC Thin using a ServiceName:**
jdbc:oracle:thin:@//host:port/service_name
Example:jdbc:oracle:thin:@//localhost:1521/orcl.city.com
注意这里的格式，@后面有//, port后面:换成了/,这种格式是Oracle 推荐的格式，因为对于集群来说，每个节点的SID 是不一样的，但是SERVICE_NAME 确可以包含所有节点。

### 一些基本命令

cmd 

sqlplus sys/oracle as sysdba;

登录后就可进入管理员的权限。

普通用户进入 直接 sqlplus sys/oracle 

 show user :查看当前用户

coon sys/oracle as sysdba ： 切换不同的用户

coon scott/tiger  :切换为普通用户

## Oracle基本数据类型



## Oracle中的数据操作

对于Oracle中的查询需要使用表名必须用双引号引起来。select * from "student"

```java
**
 * @Author kay三石
 * @date:2019/6/30
 */
@Controller
@RequestMapping("/test")
public class JdbcController {
    @Autowired
    JdbcTemplate jdbcTemplate;

//    execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句；
//
//    update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；batchUpdate方法用于执行批处理相关语句；
//
//    query方法及queryForXXX方法：用于执行查询相关语句；
//
//    call方法：用于执行存储过程、函数相关语句。

    @RequestMapping(value = "/add",method = RequestMethod.GET)
    @ResponseBody
    public Map<String,Object> add(){
        Map<String,Object> map=new HashMap();
        jdbcTemplate.execute("insert into \"student\"(\"id\", \"name\", \"sex\",\"age\", \"phone\") values('5','skdin','男','12','12345678910')");
        map.put("code","0");
        map.put("data","success");
        return map;
    }

    /**
     * 必须使用""把字段名字给包裹住
     * @return
     */
    @RequestMapping(value = "/update",method = RequestMethod.GET)
    @ResponseBody
    public Map<String,Object> update(){
        Map<String,Object> map=new HashMap();
        jdbcTemplate.execute("UPDATE \"student\" SET  \"name\"='doelo', \"sex\"='男', \"age\"='12', \"phone\"='123456910' WHERE \"id\"= '5'");
        map.put("code","0");
        map.put("data","success");
        return map;
    }

    @ResponseBody
    @GetMapping("/querylist")
    public Map<String,Object> map(){
        List<Map<String, Object>> list = jdbcTemplate.queryForList("select * FROM \"student\"");
        return list.get(0);
    }
    @ResponseBody
    @GetMapping("/query")
    public List<Student> getForList(){
        return  jdbcTemplate.query("select * FROM \"student\"", new RowMapper<Student>() {
            @Override
            public Student mapRow(ResultSet resultSet, int i) throws SQLException {
                Student model = new Student();
                model.setId(resultSet.getInt("Id"));
                model.setAge(resultSet.getInt("Age"));
                model.setName(resultSet.getString("Name"));
                model.setSex(resultSet.getString("Sex"));
                model.setPhone(resultSet.getString("Phone"));
                return model;
            }
        });
    }


    @ResponseBody
    @GetMapping("/queryobj")
    public Map<String,Object> getForObject(){
        Map<String,Object> map=new HashMap();
        //  最后一个参数是返回值的类型（只能是基本数据类型的封装类，如Integer、String）
        Student list = jdbcTemplate.queryForObject("select \"id\",\"name\",\"sex\",\"age\",\"phone\" from  \"student\"  where \"id\" ='3'", new BeanPropertyRowMapper<Student>(Student.class));
        map.put("code","0");
        map.put("data",list);
        return map;
    }


```

