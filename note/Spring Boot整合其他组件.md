# SpringBoot与数据访问

## 1、JDBC

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
```



```yaml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://192.168.15.22:3306/jdbc
    driver-class-name: com.mysql.jdbc.Driver
```

效果：

​	默认是用org.apache.tomcat.jdbc.pool.DataSource作为数据源；

​	数据源的相关配置都在DataSourceProperties里面；

自动配置原理：

org.springframework.boot.autoconfigure.jdbc：

1、参考DataSourceConfiguration，根据配置创建数据源，默认使用Tomcat连接池；可以使用spring.datasource.type指定自定义的数据源类型；

2、SpringBoot默认可以支持；

```
org.apache.tomcat.jdbc.pool.DataSource、HikariDataSource、BasicDataSource、
```

3、自定义数据源类型

```java
/**
 * Generic DataSource configuration.
 */
@ConditionalOnMissingBean(DataSource.class)
@ConditionalOnProperty(name = "spring.datasource.type")
static class Generic {

   @Bean
   public DataSource dataSource(DataSourceProperties properties) {
       //使用DataSourceBuilder创建数据源，利用反射创建响应type的数据源，并且绑定相关属性
      return properties.initializeDataSourceBuilder().build();
   }

}
```

4、**DataSourceInitializer：ApplicationListener**；

​	作用：

​		1）、runSchemaScripts();运行建表语句；

​		2）、runDataScripts();运行插入数据的sql语句；

默认只需要将文件命名为：

```properties
schema-*.sql、data-*.sql
默认规则：schema.sql，schema-all.sql；
可以使用   
	schema:
      - classpath:department.sql
      指定位置
```

5、操作数据库：自动配置了JdbcTemplate操作数据库

## 2、Springboot整合Druid数据源

```java
导入druid数据源
@Configuration
public class DruidConfig {

    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druid(){
       return  new DruidDataSource();
    }

    //配置Druid的监控
    //1、配置一个管理后台的Servlet
    @Bean
    public ServletRegistrationBean statViewServlet(){
        ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");
        Map<String,String> initParams = new HashMap<>();

        initParams.put("loginUsername","admin");
        initParams.put("loginPassword","123456");
        initParams.put("allow","");//默认就是允许所有访问
        initParams.put("deny","192.168.15.21");

        bean.setInitParameters(initParams);
        return bean;
    }


    //2、配置一个web监控的filter
    @Bean
    public FilterRegistrationBean webStatFilter(){
        FilterRegistrationBean bean = new FilterRegistrationBean();
        bean.setFilter(new WebStatFilter());

        Map<String,String> initParams = new HashMap<>();
        initParams.put("exclusions","*.js,*.css,/druid/*");

        bean.setInitParameters(initParams);

        bean.setUrlPatterns(Arrays.asList("/*"));

        return  bean;
    }
}

```

## 3、Springboot整合MyBatis

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.1</version>
</dependency>
```
显示log:

logging.level.com.kaysanshi.dao=debug

步骤：

​	1）、配置数据源相关属性（见上一节Druid）

​	2）、给数据库建表

​	3）、创建JavaBean

### 1）、注解版

```java
//指定这是一个操作数据库的mapper
@Mapper
public interface DepartmentMapper {

    @Select("select * from department where id=#{id}")
    public Department getDeptById(Integer id);

    @Delete("delete from department where id=#{id}")
    public int deleteDeptById(Integer id);

    @Options(useGeneratedKeys = true,keyProperty = "id")
    @Insert("insert into department(departmentName) values(#{departmentName})")
    public int insertDept(Department department);

    @Update("update department set departmentName=#{departmentName} where id=#{id}")
    public int updateDept(Department department);
}
```

问题：

自定义MyBatis的配置规则；给容器中添加一个ConfigurationCustomizer；

```java
@org.springframework.context.annotation.Configuration
public class MyBatisConfig {

    @Bean
    public ConfigurationCustomizer configurationCustomizer(){
        return new ConfigurationCustomizer(){

            @Override
            public void customize(Configuration configuration) {
                configuration.setMapUnderscoreToCamelCase(true);
            }
        };
    }
}
```



```java
使用MapperScan批量扫描所有的Mapper接口；
@MapperScan(value = "com.atguigu.springboot.mapper")
@SpringBootApplication
public class SpringBoot06DataMybatisApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringBoot06DataMybatisApplication.class, args);
	}
}
```

### 2）、配置文件版

```yaml
mybatis:
  config-location: classpath:mybatis/mybatis-config.xml 指定全局配置文件的位置
  mapper-locations: classpath:mybatis/mapper/*.xml  指定sql映射文件的位置
```

更多使用参照

http://www.mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/



## 4、Springboot整合SpringData JPA

### 1）、SpringData简介

Spring Data的使命是为数据访问提供一个熟悉且一致的，基于Spring的编程模型，同时仍保留基础数据存储的特殊特征。

它使使用数据访问技术，关系和非关系数据库，map-reduce框架以及基于云的数据服务变得容易。 这是一个总括项目，其中包含许多特定于给定数据库的子项目。 这些项目是通过与这些令人兴奋的技术背后的许多公司和开发人员共同开发的。

**主要模块**

Spring Data Commons-每个Spring Data模块的核心Spring概念。

Spring Data JDBC-Spring Data存储库对JDBC的支持。

Spring Data JDBC Ext-支持标准JDBC的数据库特定扩展，包括对Oracle RAC快速连接故障转移的支持，对AQ JMS的支持以及对使用高级数据类型的支持。

Spring Data JPA-对JPA的Spring Data存储库支持。

Spring Data KeyValue-基于映射的存储库和SPI，可以轻松地为键值存储构建Spring Data模块。

Spring Data LDAP-Spring数据存储库对Spring LDAP的支持。

Spring Data MongoDB-MongoDB的基于Spring的对象文档支持和存储库。

Spring Data Redis-轻松配置和从Spring应用程序访问Redis。

Spring Data REST-将Spring Data存储库导出为超媒体驱动的RESTful资源。

适用于Apache Cassandra的Spring数据-易于配置和访问Apache Cassandra或大规模，高可用性，面向数据的Spring应用程序。

用于Apache Geode的Spring Data-易于配置和访问Apache Geode，以实现高度一致，低延迟的面向数据的Spring应用程序。

适用于Apache Solr的Spring数据-易于配置，并可访问面向搜索的Spring应用程序访问Apache Solr。

用于Pivotal GemFire的Spring数据-可以轻松配置并访问Pivotal GemFire，以实现高度一致，低延迟/高吞吐量的面向数据的Spring应用程序。

### 2）、整合SpringData JPA

JPA:ORM（Object Relational Mapping）；

1）、编写一个实体类（bean）和数据表进行映射，并且配置好映射关系；

```java
//使用JPA注解配置映射关系
@Entity //告诉JPA这是一个实体类（和数据表映射的类）
@Table(name = "tbl_user") //@Table来指定和哪个数据表对应;如果省略默认表名就是user；
public class User {

    @Id //这是一个主键
    @GeneratedValue(strategy = GenerationType.IDENTITY)//自增主键
    private Integer id;

    @Column(name = "last_name",length = 50) //这是和数据表对应的一个列
    private String lastName;
    @Column //省略默认列名就是属性名
    private String email;
```

2）、编写一个Dao接口来操作实体类对应的数据表（Repository）

```java
//继承JpaRepository来完成对数据库的操作
public interface UserRepository extends JpaRepository<User,Integer> {
}

```

3）、基本的配置JpaProperties

```yaml
spring:  
 jpa:
    hibernate:
#     更新或者创建数据表结构
      ddl-auto: update
#    控制台显示SQL
    show-sql: true
```



# 更多SpringBoot整合示例

https://github.com/spring-projects/spring-boot/tree/master/spring-boot-samples



# SpringBoot与缓存

## 1.JSR-107

Java Caching定义了5个核心接口，分别是CachingProvider, CacheManager, Cache, Entry 和 Expiry。

- CachingProvider定义了创建、配置、获取、管理和控制多个CacheManager。一个应用可 以在运行期访问多个CachingProvider。
- CacheManager定义了创建、配置、获取、管理和控制多个唯一命名的Cache，这些Cache 存在于CacheManager的上下文中。一个CacheManager仅被一个CachingProvider所拥有。
- Cache是一个类似Map的数据结构并临时存储以Key为索引的值。一个Cache仅被一个 CacheManager所拥有。
- Entry是一个存储在Cache中的key-value对。
- Expiry 每一个存储在Cache中的条目有一个定义的有效期。一旦超过这个时间，条目为过期 的状态。一旦过期，条目将不可访问、更新和删除。缓存有效期可以通过ExpiryPolicy设置。

[![0w69zt.md.png](https://s1.ax1x.com/2020/10/08/0w69zt.md.png)](https://imgchr.com/i/0w69zt)

## 2.Spring缓存抽象

Spring从3.1开始定义了org.springframework.cache.Cache

和org.springframework.cache.CacheManager接口来统一不同的缓存技术；

并支持使用JCache（JSR-107）注解简化我们开发；

•Cache接口为缓存的组件规范定义，包含缓存的各种操作集合；

•Cache接口下Spring提供了各种xxxCache的实现；如RedisCache，EhCacheCache , ConcurrentMapCache等；

•每次调用需要缓存功能的方法时，Spring会检查检查指定参数的指定的目标方法是否已经被调用过；如果有就直接从缓存中获取方法调用后的结果，如果没有就调用方法并缓存结果后返回给用户。下次调用直接从缓存中获取。

•使用Spring缓存抽象时我们需要关注以下两点；

1、确定方法需要被缓存以及他们的缓存策略

2、从缓存中读取之前缓存存储的数据



[![0w6PQP.md.png](https://s1.ax1x.com/2020/10/08/0w6PQP.md.png)](https://imgchr.com/i/0w6PQP)



## 3.概念和缓存注解

| **Cache**          | **缓存接口，定义缓存操作。实现有：****RedisCache****、****EhCacheCache****、****ConcurrentMapCache****等** |
| ------------------ | ------------------------------------------------------------ |
| **CacheManager**   | **缓存管理器，管理各种缓存（****Cache****）组件**            |
| **@Cacheable**     | **主要针对方法配置，能够根据方法的请求参数对其结果进行缓存** |
| **@CacheEvict**    | **清空缓存**                                                 |
| **@CachePut**      | **保证方法被调用，又希望结果被缓存。**                       |
| **@EnableCaching** | **开启基于注解的缓存**                                       |
| **keyGenerator**   | **缓存数据时key生成策略**                                    |
| **serialize**      | **缓存数据时value序列化策略**                                |

| @Cacheable/@CachePut/ @CacheEvict主要的参数 |                                                              |                                                              |
| ------------------------------------------- | ------------------------------------------------------------ | :----------------------------------------------------------: |
| value                                       | 缓存的名称，在   spring 配置文件中定义，必须指定至少一个     | 例如：      @Cacheable(value=”mycache”) 或者       @Cacheable(value={”cache1”,”cache2”} |
| key                                         | 缓存的   key，可以为空，如果指定要按照   SpEL 表达式编写，如果不指定，则缺省按照方法的所有参数进行组合 |       例@Cacheable(value=”testcache”,key=”#userName”)        |
| condition                                   | 缓存的条件，可以为空，使用   SpEL 编写，返回   true 或者 false，只有为   true 才进行缓存/清除缓存，在调用方法之前之后都能判断 | 例如：      @Cacheable(value=”testcache”,condition=”#userName.length()>2”) |
| allEntries   (**@CacheEvict**   )           | 是否清空所有缓存内容，缺省为   false，如果指定为 true，则方法调用后将立即清空所有缓存 |  例如：      @CachEvict(value=”testcache”,allEntries=true)   |
| unless   **(@CachePut)**   **(@Cacheable)** | 用于否决缓存的，不像condition，该表达式只在方法执行之后判断，此时可以拿到返回值result进行判断。条件为true不会缓存，fasle才缓存 | 例如：      @Cacheable(value=”testcache”,unless=”#result   == null”) |
| beforeInvocation   **(@CacheEvict)**        | 是否在方法执行前就清空，缺省为   false，如果指定为 true，则在方法还没有执行的时候就清空缓存，缺省情况下，如果方法执行抛出异常，则不会清空缓存 | 例如：   @CachEvict(value=”testcache”，beforeInvocation=true) |

| **名字**        | **位置**           | **描述**                                                     | **示例**             |
| --------------- | ------------------ | ------------------------------------------------------------ | -------------------- |
| methodName      | root object        | 当前被调用的方法名                                           | #root.methodName     |
| method          | root object        | 当前被调用的方法                                             | #root.method.name    |
| target          | root object        | 当前被调用的目标对象                                         | #root.target         |
| targetClass     | root object        | 当前被调用的目标对象类                                       | #root.targetClass    |
| args            | root object        | 当前被调用的方法的参数列表                                   | #root.args[0]        |
| caches          | root object        | 当前方法调用使用的缓存列表（如@Cacheable(value={"cache1",   "cache2"})），则有两个cache | #root.caches[0].name |
| *argument name* | evaluation context | 方法参数的名字. 可以直接 #参数名 ，也可以使用 #p0或#a0 的形式，0代表参数的索引； | #iban 、 #a0 、  #p0 |
| result          | evaluation context | 方法执行后的返回值（仅当方法执行之后的判断有效，如‘unless’，’cache put’的表达式 ’cache evict’的表达式beforeInvocation=false） | #result              |

## 4.缓存的使用

• 1、引入spring-boot-starter-cache模块

 • 2、@EnableCaching开启缓存

 • 3、使用缓存注解

 • 4、切换为其他缓存

## 5.SpringBoot整合Redis实现缓存

1.引入pom.xml。前提说下这里使用的mybatis的文件是这样的：

```xml
<dependency>   
    <groupId>org.mybatis.spring.boot</groupId>    
    <artifactId>mybatis-spring-boot-starter</artifactId>   
    <version>2.0.1</version>
</dependency>
```

如果这里是高版本的当我们自定义配置cache的方式就不是本文章所看到的那样了

2.spring.application的配置文件

```xml
spring.datasource.url=jdbc:mysql://localhost:3306/spring-boot-cache?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
spring.datasource.password=123
spring.datasource.username=root
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
#开启驼峰mybatis.configuration.map-underscore-to-camel-case=trues
pring.redis.host=127.0.0.1
```

3.redis的配置类：

```java
package com.kayleoi.springbootcache.config;

import com.kayleoi.springbootcache.bean.Department;
import com.kayleoi.springbootcache.bean.Employee;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.core.io.ResourceLoader;
import org.springframework.data.redis.cache.RedisCacheManager;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;

import java.net.UnknownHostException;
import java.util.LinkedHashSet;
import java.util.List;

/**
 * @Author kay三石
 * @date:2019/8/3
 * 当有多个缓存管理器时，必须指定一个默认的缓存管理器
 *     @Primary 指定默认的缓存管理器，这里我们话应该使用spring中自带的(不加会出现以下错误)
 *     java.lang.IllegalStateException: No CacheResolver specified, and no unique bean of type CacheManager found.
 *     Mark one as primary or declare a specific CacheManager to use.
 *需要使用低版本的对于2.0.0以上的版本使用方法不是这样的
 */
@Configuration
public class MyRedisConfig {
    /**
     * 用自己的CacheManager 为 employee序列化缓存
     *@Primary 将某个缓存管理器设为默认的
     * @param
     * @return
     */
    @Bean
    public RedisCacheManager employeeCacheManager(RedisTemplate<Object,Employee> empRedisTemplate) {
        RedisCacheManager cacheManager = new RedisCacheManager(empRedisTemplate);
        //key 使用前缀，将CacheName最为前缀
        cacheManager.setUsePrefix(true);
        return cacheManager;
    }
    /**
     * 用自己的CacheManager 为 depart序列化缓存管理器
     * @param
     * @return
     */
    @Bean
    public RedisCacheManager departmentCacheManager(RedisTemplate<Object,Department> depRedisTemplate) {
        RedisCacheManager cacheManager = new RedisCacheManager(depRedisTemplate);
        //key 使用前缀，将CacheName最为前缀
        cacheManager.setUsePrefix(true);
        return cacheManager;
    }

    /**
     * 使用自己的将object对象转化为json
     * @param redisConnectionFactory
     * @return
     * @throws UnknownHostException
     */
    @Bean
    public RedisTemplate<Object,Employee> empRedisTemplate(
            RedisConnectionFactory redisConnectionFactory)throws UnknownHostException {
        RedisTemplate<Object, Employee> template = new RedisTemplate <>();
        template.setConnectionFactory(redisConnectionFactory);
        template.setDefaultSerializer(new Jackson2JsonRedisSerializer <Employee>(Employee.class));
        return template;
    }

    @Bean
    public RedisTemplate<Object, Department> depRedisTemplate(
            RedisConnectionFactory redisConnectionFactory)throws UnknownHostException {
        RedisTemplate<Object, Department> template = new RedisTemplate <>();
        template.setConnectionFactory(redisConnectionFactory);
        template.setDefaultSerializer(new Jackson2JsonRedisSerializer <Department>(Department.class));
        return template;
    }

    /**
     * 选择redis作为默认缓存工具
     * @param redisTemplate
     * @return
     */
    @Primary
    @Bean
    public RedisCacheManager cacheManager(RedisTemplate redisTemplate) {
        RedisCacheManager caheManager = new RedisCacheManager(redisTemplate);
        return caheManager;
    }
}

/**
 * 默认使用时ConcurrentMapCacheManager == ConcurrentMapCache 将数据保存到ConcurrentMap<Object,Object>
 *  实际开发中使用的是 中间件， redis,ehcache
 *
 *  redis 测试缓存，原理 CacheManager === Cache 将组件来实际给缓存存取数据
 *  引入redis的start后容器所用的是RedisCacheManager
 *  RedisCacheManager 帮我们创建 RedisCache来作为缓存组件
 *  默认保存数据 k,v 都是object 利用序列化转化为json
 *      1.引入redis的start后CacheManager变为 RedisCacheManager
 *      2.默认创建的 RedisCacheManager 操作redis的时候使用的是 RedisTemplate<Object,Object>
 *      3.RedisTemplate<Object, Object>是默认使用jdk的序列化机制
 *   自定义CacheManager:
 *
 */

@SpringBootApplication
@MapperScan("com.kayleoi.springbootcache.mapper")
@EnableCaching
public class SpringBootCacheApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBootCacheApplication.class, args);
    }

}

```

service层：

```java
package com.kayleoi.springbootcache.service;

import com.kayleoi.springbootcache.bean.Employee;
import com.kayleoi.springbootcache.mapper.EmployeeMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cache.annotation.*;
import org.springframework.stereotype.Service;

/**
 * @Author kay三石
 * @date:2019/7/22
 * @CacheConfig(cacheNames = "emp") 抽取缓存的公共配置
 * 原理：
 *  1.自动配置类：CacheAutoConfiguration
 *  2.缓存的配置类：
 *
 *  3.默认的配置类：SimpleCaCheConfiguration
 *  当写了多个cacheManager 的需要指定哪个管理器使用的
 *  当使用了不同的管理器时，需要指定一个默认的缓存管理器，要不然不会找到自己写的缓存管理器
 *
 */
@CacheConfig(cacheNames = "emp", cacheManager = "employeeCacheManager")
@Service
public class EmployeeService {

    @Autowired
    EmployeeMapper employeeMapper;

    /**
     * 属性：
     * cacheNames/value: 执行缓存组件的名字，将方法的返回结果放到哪个缓存中是数组方式，可指定多个缓存
     * key: 缓存数据使用的key，可以用来指定默认使用方法参数的值，1-方法的返回值
     * 编写spel： # id: 参数id的值， #a0 #p0  #root.args[0]
     * 执行使用的key ：key = "#root.methodName+'['+#id+']'" ====getEmployee(id)
     * keyGenerator:key的生成器，可以自己指定key的生成器的组件的id
     * key/keyGenerator二则选一使用
     * <p>
     * cacheManager: 指定缓存管理器，或者cacheResolver指定获取解析器
     * condition: 指定符合条件的情况下才缓存 condion= "#id >0 and #id <10"
     * unless: 否定缓存，当unless指定条件为true时，方法的返回值就不会被缓存，可以获取到结果进行判断
     * unless = "#result == null"
     * unless = "#a0==2" 如果第一个参数为2时则不缓存
     * sync:  是否使用异步的方式
     *
     * @param id
     * @return
     */
    @Cacheable(value = {"emp"}, key = "#root.methodName+'['+#id+']'")
    public Employee getEmployee(Integer id) {
        //用默认的需要序列化
        System.out.println("查询数据库");
        Employee empById = employeeMapper.getEmpById(id);
        return empById;
    }

    /**
     * 指定缓存
     *
     * @param id
     * @return
     */
    @Cacheable(value = {"emp2"}, keyGenerator = "myKeyGenerator", condition = "#a0>0", unless = "#a0==2")
    public Employee getEmployee2(Integer id) {
        Employee empById = employeeMapper.getEmpById(id);
        return empById;
    }

    /**
     * @CachePut: 调用方法，又更新缓存  ，修改的同时更新(同步更新缓存)
     * 运行实机：
     *  1.先调用目标方法
     *  2.将目标方法的结果缓存起来
     *      方法运行后将结果放入
     *
     * 调式步骤：
     *  1.查询1号员工，查到的结果放入缓存中：
     *      key :1 value: lastName:
     *  2. 以后查询还是之前的结果
     *  3. 更新1号员工，
     *      将方法的返回值放入了缓存 key:传入的emplyee对象，返回的也是对象
     *  4. 查询1号员工
     *      应该是更新后的员工
     *          key = "#employee.id" 使用传入参数的id
     *          key = "#result.id" 使用返回值的id
     * @Caheable的key不能使用#result
     *      但为什么是没有更新前呢？【1号员工没有在缓冲中更新,就是缓存中没有这个key】
     */
    @CachePut(value = {"emp"}, key = "#employee.id")
    public Employee update(Employee employee){
        System.out.println("employee = [" + employee + "]");
        employeeMapper.updateEmp(employee);
        return employee;

    }

    /**
     * 清除缓存
     *  key:指定清除的数据
     *  allEntries = true 指定清除这缓存中的所有的数据
     *  beforeInvocation = false：
     *          默认是方法执行之后执行，如果出现异常缓存不会清除
     *
     *  beforeInvocation = true
     *          代表清除缓存操作是在方法运行之前执行，无论方法是否出现异常，缓存都清除
     *
     *
     * @param id
     */
    @CacheEvict(value = "emp",beforeInvocation = true, key = "#id")
    public void deleteEmp(Integer id){
        employeeMapper.deleteEmById(id);
    }
    @Caching(
            cacheable = {
                    @Cacheable(value = "emp", key = "#lastName")
            },
            put = {
                    @CachePut(value = "emp", key = "#result.id"),
                    @CachePut(value = "emp", key = "#result.email")
            }
    )
    public Employee getEmployeeByName(String lastName){
        return employeeMapper.getEmpByName(lastName);
    }
}

```

通过以上的配置的代码可以实现redis操作缓存数据。

参看fork的一个项目：https://github.com/kaysanshi/spring-boot/tree/master/spring-boot-Redis-master

# SpringBoot与消息队列

## 1.简述：

1.大多应用中，可通过消息服务中间件来提升系统异步通信、扩展解耦能力

2.消息服务中两个重要概念：

​       消息代理（message broker）和目的地（destination）

当消息发送者发送消息以后，将由消息代理接管，消息代理保证消息传递到指定目的地。

3.消息队列主要有两种形式的目的地

- 1.队列（queue）：点对点消息通信（point-to-point）
- 2.主题（topic）：发布（publish）/订阅（subscribe）消息通信6

4.点对点式：

消息发送者发送消息，消息代理将其放入一个队列中，消息接收者从队列中获取消息内容，消息读取后被移出队列

**消息只有唯一的发送者和接受者，但并不是说只能有一个接收者**

5.发布订阅式：

发送者（发布者）发送消息到主题，多个接收者（订阅者）监听（订阅）这个主题，那么就会在消息到达时同时收到消息

6.JMS（Java Message Service）JAVA消息服务：

基于JVM消息代理的规范。ActiveMQ、HornetMQ是JMS实现

7.AMQP（Advanced Message Queuing Protocol）

高级消息队列协议，也是一个消息代理的规范，兼容JMS

RabbitMQ是AMQP的实现

|              | JMS                                                          | AMQP                                                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 定义         | Java   api                                                   | 网络线级协议                                                 |
| 跨语言       | 否                                                           | 是                                                           |
| 跨平台       | 否                                                           | 是                                                           |
| Model        | 提供两种消息模型：   （1）、Peer-2-Peer   （2）、Pub/sub     | 提供了五种消息模型：   （1）、direct   exchange   （2）、fanout   exchange   （3）、topic   change   （4）、headers   exchange   （5）、system   exchange   本质来讲，后四种和JMS的pub/sub模型没有太大差别，仅是在路由机制上做了更详细的划分； |
| 支持消息类型 | 多种消息类型：   TextMessage   MapMessage   BytesMessage   StreamMessage   ObjectMessage   Message   （只有消息头和属性） | byte[]   当实际应用时，有复杂的消息，可以将消息序列化后发送。 |
| 综合评价     | JMS   定义了JAVA   API层面的标准；在java体系中，多个client均可以通过JMS进行交互，不需要应用修改代码，但是其对跨平台的支持较差； | AMQP定义了wire-level层的协议标准；天然具有跨平台、跨语言特性。 |

8.Spring支持

**spring-jms提供了对JMS的支持**

**spring-rabbit提供了对AMQP的支持**

**需要ConnectionFactory的实现来连接消息代理**

**提供JmsTemplate、RabbitTemplate来发送消息**

**@JmsListener（JMS）、@RabbitListener（AMQP）注解在方法上监听消息代理发布的消息**

**@EnableJms、@EnableRabbit启支持**

9.Spring Boot自动配置

**JmsAutoConfiguration**

**RabbitAutoConfiguration**

## 2.RabbitMQ简介：

RabbitMQ是由erlang开发的AMQP(Advanved Message Queue Protocol)的开源实现

### **核心概念**

#### **Message**

消息，消息是不具名的，它由消息头和消息体组成。消息体是不透明的，而消息头则由一系列的可选属性组成，这些属性包括routing-key（路由键）、priority（相对于其他消息的优先权）、delivery-mode（指出该消息可能需要持久性存储）等。

#### **Publisher**

消息的生产者，也是一个向交换器发布消息的客户端应用程序。

#### **Exchange**

交换器，用来接收生产者发送的消息并将这些消息路由给服务器中的队列。

Exchange有4种类型：direct(默认)，fanout, topic, 和headers，不同类型的Exchange转发消息的策略有所区别

#### **Queue**

消息队列，用来保存消息直到发送给消费者。它是消息的容器，也是消息的终点。一个消息可投入一个或多个队列。消息一直在队列里面，等待消费者连接到这个队列将其取走。

#### **Binding**

绑定，用于消息队列和交换器之间的关联。一个绑定就是基于路由键将交换器和消息队列连接起来的路由规则，所以可以将交换器理解成一个由绑定构成的路由表。

Exchange 和Queue的绑定可以是多对多的关系。

#### **Connection**

网络连接，比如一个TCP连接。

#### **Channel**

信道，多路复用连接中的一条独立的双向数据流通道。信道是建立在真实的TCP连接内的虚拟连接，AMQP 命令都是通过信道发出去的，不管是发布消息、订阅队列还是接收消息，这些动作都是通过信道完成。因为对于操作系统来说建立和销毁 TCP 都是非常昂贵的开销，所以引入了信道的概念，以复用一条 TCP 连接。

#### **Consumer**

消息的消费者，表示一个从消息队列中取得消息的客户端应用程序。

#### **Virtual Host**

虚拟主机，表示一批交换器、消息队列和相关对象。虚拟主机是共享相同的身份认证和加密环境的独立服务器域。每个 vhost 本质上就是一个 mini 版的 RabbitMQ 服务器，拥有自己的队列、交换器、绑定和权限机制。vhost 是 AMQP 概念的基础，必须在连接时指定，RabbitMQ 默认的 vhost 是 / 。

#### **Broker**

表示消息队列服务器实体



## 3.RabbitMQ运行机制：

AMQP中的消息路由：

AMQP中消息的路由过程和java开发中jms存在差别AMQP

中增加了 **Exchange** 和 **Binding** 的角色。生产者把消息发布到 Exchange 上，消息最终到达队列并被消费者接收，而 Binding 决定交换器的消息应该发送到那个队列。：

[![0wyzid.md.png](https://s1.ax1x.com/2020/10/08/0wyzid.md.png)](https://imgchr.com/i/0wyzid)



每个发到 fanout 类型交换器的消息都会分到所有绑定的队列上去。fanout
交换器不处理路由键，只是简单的将队列绑定到交换器上，每个发送到交换器的消息都会被转发到与该交换器绑定的所有队列上。很像子网广播，每台子网内的主机都获得了一份复制的消息。fanout
类型转发消息是最快的。

[![0w6isf.png](https://s1.ax1x.com/2020/10/08/0w6isf.png)](https://imgchr.com/i/0w6isf)

topic 交换器通过模式匹配分配消息的路由键属性，将路由键和某个模式进行匹配，此时队列需要绑定到一个模式上。它将路由键和绑定键的字符串切分成单词，这些**单词之间用点隔开**。它同样也会识别两个通配符：符号“#”和符号“**”**。**#**匹配**0**个或多个单词**，****匹配一个单词。

[![0w6FL8.png](https://s1.ax1x.com/2020/10/08/0w6FL8.png)](https://imgchr.com/i/0w6FL8)



## 4.RabbitMQ整合使用：

1.**引入** spring-boot-starter-amqp

2.**application.yml配置**

```
spring.rabbitmq.host=192.167.147.132
spring.rabbitmq.username= rabbit
spring.rabbitmq.password= 123456
spring.rabbitmq.port=15672
#spring.rabbitmq.virtual-host
```

3.**测试RabbitMQ**

1.**AmqpAdmin：管理组件**

```java
@Autowired
    AmqpAdmin amqpAdmin;

    /**
     * 以declare开头是创建的机制
     */
    @Test
    public void createExchange(){
        amqpAdmin.declareExchange(new DirectExchange("amqpadmin.exchange"));
    }

    /**
     * 创建对列
     */
    @Test
    public void createExchangeQueue(){
        amqpAdmin.declareQueue(new Queue("amqpadmin.queue",true));
        amqpAdmin.declareBinding(new Binding("amqpadmin.queue",Binding.DestinationType.QUEUE,"amqpadmin.exchange","amqpadmin.rout",null));

    }
    /**
     * 删除
     */
    public void deleteExchange(){
        amqpAdmin.deleteQueue("amqpadmin.exchange");
    }
```

2.**RabbitTemplate：消息发送处理组件**

```java
 /**
     * 1.单播(点兑点)
     */
    @Test
    public void contextLoads(){
        //Message需要自己构造一个，自定义的消息头，和消息内容
        //rabbitTemplate.send(exchange,routerKey,message);

        //object默认当成消息体，只需要传入发送的对象，自动序列发送
        //rabbitTemplate.convertAndSend(exchange,routerKey,object);
        Map<String,Object> map = new HashMap<>();
        map.put("msg","rabbitQ消息");
        map.put("data", Arrays.asList("helloworld",123,true));
        //对象被默认序列化后发送
        rabbitTemplate.convertAndSend("exchange.direct","atguigu.news",map);

    }

    /**
     *接受数据
     */
    @Test
    public void receive(){
        Object o = rabbitTemplate.receiveAndConvert("atguigu.news");

        System.out.println(o.getClass());
        System.out.println(o);
    }
    /***
     * 序列化数据，序列化为json数据
     */

    /***
     * 广播
     */
    @Test
    public void sendMsg(){
        rabbitTemplate.convertAndSend("exchange.fanout","",new Book("北京姑娘","那方"));
    }

```

## 5.环境搭建：

docker-comose.yml

```yaml
version:'3'
services:
 rabbitmq:
   restart: always
   image: rabbitmq:management
   container_name:rabbitmq
   ports:
     - 5672:5672
     - 15672:15672
   enviroment:
     TZ: Asia/Shanghai
     RABBITMQ_DEFAULT_USER:rabbit
     RABBITMQ_DEFAULT_PASS:123456
   volumes:
     - ./data:/var/lib/rabbitmq
     
```



#  SpringBoot与检索

## 1.简述

安装

