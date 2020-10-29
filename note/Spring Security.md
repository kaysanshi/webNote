## Spring Security

使用Spring Secruity的原因有很多，单大部分都发现了javaEE的Servlet规范或EJB规范中的安全功能缺乏典型企业应用场景所需的深度。提到这些规范，重要的是要认识到他们在WAR或EAR级别无法移植。因此如果你更换服务器环境，这里有典型的大量工作去重新配置你的应用程序员安全到新的目标环境。使用Spring Security 解决了这些问题，也为你提供许多其他有用的，可定制的安全功能。

Spring Security提供一套的授权功能。这里有三个主要的热点区域，授权web请求、授权方法是否可以被调用和授权访问单个域对象的实例。

<font color="red" font-size="18px">spring本质就是一个过滤器链。</font>

### demo实例:

pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.kaysanshi</groupId>
    <artifactId>demo-security</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>demo-security</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```

```java
/**
 * Description:
 *
 * @date:2020/10/23 10:18
 * @author: kaysanshi
 **/
@RestController
public class SecurityTestController {
    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }
}
@SpringBootApplication
public class DemoSecurityApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoSecurityApplication.class, args);
    }

}

```

启动项目后，访问会有一个认证登录界面。这时的密码是在控制台有打印。

[![BAPe8H.md.png](https://s1.ax1x.com/2020/10/23/BAPe8H.md.png)](https://imgchr.com/i/BAPe8H)
[![BAPm2d.md.png](https://s1.ax1x.com/2020/10/23/BAPm2d.md.png)](https://imgchr.com/i/BAPm2d)

**配置用户名和密码**

方式一：application.yml配置

```xml
server.port=82
## 配置文件配置Spring security 的认证用户名和密码
spring.security.user.name=kay
spring.security.user.password=sanshi

```

方式二：java代码配置：

```java
/**
 * Description:
 *
 * @date:2020/10/23 10:52
 * @author: kaysanshi
 **/
//@Configuration
public class SecurityConfigurer extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        super.configure(auth);
        // 在配置类中配置认证的密码与用户
        auth.inMemoryAuthentication()
                .withUser("kay")
                .roles("admin")
                .password("2a731e08-c7c2-4a44-bc9d-38ada3e824af")
                .and()
        .withUser("kkk")
        .roles("user")
        .password("2a731e08-c7c2-4a44-bc9d-38ada3e824af"); // 这里的password 放置加密后的字符串
    }
    @Bean
    PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }
}

```

Spring Security 中提供了 BCryptPasswordEncoder 密码编码工具，可以非常方便的实现密码的加密加盐，相同明文加密出来的结果总是不同，这样就不需要用户去额外保存`盐`的字段了，这一点比 Shiro 要方便很多。



### Spring过滤器链：

包含三个基本过滤器 里面有十几个过滤器：

##### FileterSecurityInterceptor

是一个方法级别的权限过滤器，基本位于过滤器链的最底部

##### ExceptionTranslationFilter

是一个异常过滤器，用来处理认证授权过程中抛出的异常

##### UsernamePasswordAuthenticationFilter

对/login的POST请求拦截，校验表单中用户名密码

#### SpringSecurity 过滤器加载过程

通过DelegatingFilterProxy 

dofilter()-->this.initDelegate()-->this.getTargetName()===(FilterChainProxy) -->this.getFilters()

### UserDetailsService接口

当我们不配置账号密码是由Spring Security定义生成的，而在实际项目中账号和密码是通过从数据库中查询出来的。所以我们要自定义逻辑控制认证逻辑。如果需要自定义逻辑则需要实现UserDetailsService接口中的loadUserByUsername方法。

```java

package org.springframework.security.core.userdetails;

public interface UserDetailsService {
    UserDetails loadUserByUsername(String var1) throws UsernameNotFoundException;
}

```

- 创建一个类继承UsernamePasswordAuthenticationFilter.重写三个方法
- 创建一个类实现UserDetailService编写查询数据库的过程，返回user对象，这个user对象是安全框架提供的对象。



### PasswordEncoder接口

在实际的操作中存储密码是加密的方式进行加密。

```java
package org.springframework.security.crypto.password;

public interface PasswordEncoder {
    // 表示把参数按照特定的解析规则进行解析
    String encode(CharSequence var1);
	// 表示验证从储存中获取的编码密码与编码后提交的原始密码是否匹配，如果匹配返回true.(被解析的，储存的密码)
    boolean matches(CharSequence var1, String var2);
	// 表示如果解析的密码能够再次进行解析且到达更安全的结果，则返回true,否则返回false.默认返回false.
    default boolean upgradeEncoding(String encodedPassword) {
        return false;
    }
}

```

BCryptPasswordEncoder是spring security推荐的密码解析器，平时使用最多的解析器。

BCryptPasswordEncoder是对Bcrypt强散列方法的具体实现。是基于hash算法的单向加密，可以同过strength控制机密程度。默认10.

#### 例子：

```java
@Test
    void testBCryptPasswordEncoder() {
        // 創建密碼解析器
        BCryptPasswordEncoder bCryptPasswordEncoder =new BCryptPasswordEncoder();
        // 對密碼進行加密
        String kay=bCryptPasswordEncoder.encode("kay");
		// $2a$10$J8hwGMIPusfpvAlWSTAshORaWk6ZtQq74vu4VRAPIiGR6Vbk1sb3i
        System.out.println(kay);

        // 判斷原字符串
        boolean result = bCryptPasswordEncoder.matches("kay",kay);

        System.out.println(result); // true
    }
```

### web权限方案--认证（authentication）

认证简单地说就是让系统知道是不是你，比如：你有身份证ID卡，那么你刷身份证到火车站，则可以通过人脸识别通过或者能够在系统中查到你的信息。

#### 设置登录用户名和密码的三种方式：

##### 通过配置文件

配置yaml文件：

```
## 在這配置了就不用代碼配置了
spring.security.user.name=kay
spring.security.user.password=sanshi
```

##### 通过配置类

```java
package com.kaysanshi.demosecurity.config;
/**
 * Description:
 *
 * @date:2020/10/23 10:52
 * @author: kaysanshi
 **/
//@Configuration
public class SecurityConfigurer extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception { 
        // super.configure(auth); 这个不能使用，要不然程序直接走了父类的，下面的不生效。
         BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
        // 在配置类中配置认证的密码与用户
        auth.inMemoryAuthentication()
                .withUser("kay")
                .roles("admin")
                .password(passwordEncoder.encode("123"))
                .and()
        .withUser("kkk")
        .roles("user")
        .password(passwordEncoder.encode("123")); // 这里的password 放置加密后的字符串
    }

    /**
     * 为bean 定义如何解码，必须使用这个，如果不使用则会报错。java.lang.IllegalArgumentException: There is no PasswordEncoder mapped for the id "null"
     * @return
     */
    @Bean
    PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }
}
```

##### 通过编写自定义实现类

spring security在读取配置中是否有用户名和密码设置，如果有的话就去使用，如果没有配置则会去UserDetailsService接口查找。

- 创建一个配置类，设置使用哪个userDetailsservice实现类。
- 编写实现类（配置或者查数据库），返回User对象，User对象有用户名密码和操作权限。

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    //
    @Autowired
    private UserDetailsService userDetailsService;

    @Bean
    PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }


    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder());
    }
}


/**
 * @Author kay三石
 * @date:2020/10/24 實現自己的UserDetailsService
 */
@Service("userDetailsService")
public class MyUserDetailsServicce implements UserDetailsService {
    @Override
    public UserDetails loadUserByUsername(String s) throws UsernameNotFoundException {
        // 暫時設置一些權限
        List<GrantedAuthority> authorities= AuthorityUtils.commaSeparatedStringToAuthorityList("role");
        return new User("kay",new BCryptPasswordEncoder().encode("123"),authorities);
    }
}
```

**通过使用配置类去查数据库进行认证(常用)**

- 创建一个配置类，注入使用哪个userDetailsservice实现类。
- 编写实现类，查询数据库对应的用户名和密码，返回User对象，User对象有用户名密码和操作权限。

application.properties

<font color="red">mysql在springboot 2.0以后必须配置时区</font>

```properties
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=Asia/Shanghai
spring.datasource.username=root
spring.datasource.password=123
```

configer配置类

```java
/**
 * @Author kay三石
 * @date:2020/10/24
 */
@Configuration
public class SecurityConfigByDB extends WebSecurityConfigurerAdapter {
    //通过自定义实现UserDetailsService
    @Autowired
    private UserDetailsService userDetailsServiceByDB;


    @Bean
    PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsServiceByDB).passwordEncoder(passwordEncoder());
    }
}

```

```java
/**
 * @Author kay三石
 * @date:2020/10/24
 * 通过注入mapper然后在数据库中进行查询出响应的，然后作授权
 */
@Service("userDetailsServiceByDB")
public class UserDetailsServiceByDB implements UserDetailsService {
    @Autowired
    private UsersMapper usersMapper;


    @Override
    public UserDetails loadUserByUsername(String s) throws UsernameNotFoundException {
        // 調用userMapper 根據用戶名查數據庫
        QueryWrapper <Users> wrapper = new QueryWrapper <>();
        // where email=?
        wrapper.eq("email", s);
        Users users = usersMapper.selectOne(wrapper);
        if (users == null) {
            // 數據庫中認證失敗
            throw new UsernameNotFoundException("用戶名不存在");

        }
        List<GrantedAuthority> authorities= AuthorityUtils.commaSeparatedStringToAuthorityList("role");
        return new User(users.getEmail(),new BCryptPasswordEncoder().encode(users.getPassword()),authorities);
    }
}
```

实体类：

```java
/**
 * @Author kay三石
 * @date:2020/10/24
 */
@Data
@TableName("user")
public class Users {
    private Integer id;
    private String email;
    private String password;
}

```

控制器类：

```java
@RestController
public class TestController {

    @GetMapping("/hello")
    public String hello() {
        return "hello";

    }
}
```

通过访问localhost8080后会出现登录界面，然后输入数据库中的对应用户名和密码即可登录。

#### 自定义设置登录界面，配置不需要权限的路径

##### 配置不需要配置的路径

```java
 /**
     * 核心过滤器配置的方法
     * configure(WebSecurity web)
     * 用于配置WebSecurity webSecurity是基于servlet Filter的配置SpringSecurityFilterChain.而 springSecurityFilterChain 又被委托给了 Spring Security 核心过滤器 Bean DelegatingFilterProxy 。 相关逻辑你可以在 WebSecurityConfiguration 中找到。
     * 我们一般不会过多来自定义 WebSecurity , 使用较多的使其ignoring() 方法用来忽略 Spring Security 对静态资源的控制。
     * 如果一个请求路径不设置拦截：
     * 1.设置地址匿名访问
     * 2.直接过滤掉该地址，及该地址不走Spring Security 过滤器链。
     * 下面方法是演示直接过率掉该地址。
     * WebSecurity的使用
     *
     * @param web
     * @throws Exception
     */
    @Override
    public void configure(WebSecurity web) throws Exception {
        super.configure(web);
        // 忽略那些拦截
        web.ignoring().antMatchers("/vercode");
    }
```

##### 自定义登录界面

```java
 /**
     * configure(HttpSecurity)方法定义了哪些URL路径应该被保护，哪些不应该。具体来说，“/”和“/ home”路径被配置为不需要任何身份验证。所有其他路径必须经过身份验证。
     * 用来配置 HttpSecurity 。 HttpSecurity 用于构建一个安全过滤器链 SecurityFilterChain 。SecurityFilterChain 最终被注入核心过滤器
     * HttpSecurity的使用：
     *
     * @param http
     * @throws Exception
     */
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // addFilterBefore 在指定的Filter类的位置添加过滤器
//        http.addFilterBefore(verifyCodeFilter, UsernamePasswordAuthenticationFilter.class);
        http.authorizeRequests()//开启登录配置
                // 可以通过访问的多个URL模式。任何用户都可以访问URL以"/resources/", equals "/signup", 或者 "/about"开头的URL。
//                .antMatchers("/resources/**", "/signup", "/about", "/home").permitAll()
                .antMatchers("/hello").hasRole("admin")//表示访问 /hello 这个接口，需要具备 admin 这个角色
                //	任何以"/db/" 开头的URL需要用户同时具有 "ROLE_ADMIN" 和 "ROLE_DBA"。和上面一样我们的 hasRole 方法也没有使用 "ROLE_" 前缀
                .antMatchers("/db/**").access("hasRole('ADMIN') and hasRole('DBA')")
                .anyRequest().authenticated()//表示剩余的其他接口，登录之后就能访问
                .and()
            // 定义自己编写的登录页面
                .formLogin()
                //定义登录页面(并不是接口)，未登录时，访问一个需要登录之后才能访问的接口，会自动跳转到该页面
                .loginPage("/templates/login_p.html")
                //登录处理接口，就是那个controller，这个过程有springsecurity自己实现，不需要自己实现。
                .loginProcessingUrl("/doLogin")
                //定义登录时，用户名的 key，默认为 username
                .usernameParameter("uname")
                //定义登录时，用户密码的 key，默认为 password
                .passwordParameter("passwd")
                //登录成功的处理器
                .successHandler(new AuthenticationSuccessHandler() {
                    @Override
                    public void onAuthenticationSuccess(HttpServletRequest req, HttpServletResponse resp, Authentication authentication) throws IOException, ServletException {
                        resp.setContentType("application/json;charset=utf-8");
                        PrintWriter out = resp.getWriter();
                        out.write("success");
                        out.flush();
                    }
                })
                .failureHandler(new AuthenticationFailureHandler() {
                    @Override
                    public void onAuthenticationFailure(HttpServletRequest req, HttpServletResponse resp, AuthenticationException exception) throws IOException, ServletException {
                        resp.setContentType("application/json;charset=utf-8");
                        PrintWriter out = resp.getWriter();
                        out.write("fail");
                        out.flush();
                    }
                })
                .permitAll()//和表单登录相关的接口统统都直接通过
                .and()
                .logout()  // 提供注销支持，使用WebSecurityConfigurerAdapter会自动被应用。
                .logoutUrl("/logout") // 	设置触发注销操作的URL (默认是/logout). 如果CSRF内启用（默认是启用的）的话这个请求的方式被限定为POST。
                .logoutSuccessHandler(new LogoutSuccessHandler() {
                    // 注销后的操作
                    @Override
                    public void onLogoutSuccess(HttpServletRequest req, HttpServletResponse resp, Authentication authentication) throws IOException, ServletException {
                        resp.setContentType("application/json;charset=utf-8");
                        PrintWriter out = resp.getWriter();
                        out.write("logout success");
                        out.flush();
                    }
                })
                .invalidateHttpSession(true) // 指定是否在注销时让HttpSession无效。 默认设置为 true。
                // 添加一个LogoutHandler.默认SecurityContextLogoutHandler会被添加为最后一个LogoutHandler
                .addLogoutHandler(new LogoutHandler() {
                    @Override
                    public void logout(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Authentication authentication) {

                    }
                })
                // 允许指定在注销成功时将移除的cookie
                .deleteCookies("")
                .permitAll()
                .and()
                .httpBasic()
                .and()
                .csrf().disable();
    }
```

### web权限解决方案--用户授权(authorization)

授权就是你在系统中有没有这些权限。比如你进站趁车，那么你必须买票和刷人份证，你刷身份证时可以显示到你的身份信息则是认证的过程。在显示身份信息后可以看到具体的车票信息，这是通过你的身份信息看到了车票信息，让你去哪个地方等候上车，这个地方就是授权

#### 基于角色或权限进行访问控制

##### hasAuthority 方法

如果当前的主题指定的权限则返回true否则返回false.

下面的hasAuthority("admins")必须和权限列表中一致

1.在配置类配置

```java
 // 当前登录用户(可以登录)必须有admins权限才可以访问这个路径。
http.authorizeRequests().antMatchers("/test/index").hasAuthority("admins")
               
```

2.在UserDetailsService中把返回User对象设置权限

```java
  List<GrantedAuthority> authorities= AuthorityUtils.commaSeparatedStringToAuthorityList("admins,manager");
        return new User(users.getEmail(),new BCryptPasswordEncoder().encode(users.getPassword()),authorities);
 
```

这是访问会出现问题：



##### hasAnyAuthority 方法

如果当前的主题没有提供任何的角色（给定的作为一个逗号分隔的字符串列表）的话，返回true

```java
// 当用户权限有其中一个就可以
                .antMatchers("/hello").hasAnyAuthority("admins,manager")
                    
  List<GrantedAuthority> authorities= AuthorityUtils.commaSeparatedStringToAuthorityList("admins,manager");
        return new User(users.getEmail(),new BCryptPasswordEncoder().encode(users.getPassword()),authorities);
```

##### hasRole方法

如果用户具备给定的角色就允许访问否则403

如果当前主题具有指定的角色，则返回true

```java
                // 必须有这个role 的，才可以访问 hasRole("sale")==>ROLE_sale
                .antMatchers("/hello").hasRole("sale")
```

```java
List<GrantedAuthority> authorities= AuthorityUtils.commaSeparatedStringToAuthorityList("admins,ROLE_sale");
        return new User(users.getEmail(),new BCryptPasswordEncoder().encode(users.getPassword()),authorities);
```

如果配置的hasRole("sale1") AuthorityUtils.commaSeparatedStringToAuthorityList("admins,ROLE_sale"); 为这个是则访问失败403

##### hasAnyRole方法

表示用户具备任何一个条件都可访问。

```java
.antMatchers("/hello").hasAnyRole("sale1,admin")


        List<GrantedAuthority> authorities= AuthorityUtils.commaSeparatedStringToAuthorityList("admins,ROLE_sale");

```

#### 基于数据库实现权限认证



#### 自定义403页面

在配置类中进行配置即可

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
        http.exceptionHandling().accessDeniedPage("/unauth.html");
}
```

### Web权限解决方案--用户注销

```
http.logout()  // 提供注销支持，使用WebSecurityConfigurerAdapter会自动被应用。
                .logoutUrl("/logout") // 	设置触发注销操作的URL (默认是/logout). 如果CSRF内启用（默认是启用的）的话这个请求的方式被限定为POST。
                .logoutSuccessHandler(new LogoutSuccessHandler() {
                    // 注销后的操作
                    @Override
                    public void onLogoutSuccess(HttpServletRequest req, HttpServletResponse resp, Authentication authentication) throws IOException, ServletException {
                        resp.setContentType("application/json;charset=utf-8");
                        PrintWriter out = resp.getWriter();
                        out.write("logout success");
                        out.flush();
                    }
                })
```



### Web权限解决方案--自动登录

#### 实现原理

[![Bej7GQ.png](https://s1.ax1x.com/2020/10/25/Bej7GQ.png)](https://imgchr.com/i/Bej7GQ)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.kaysanshi</groupId>
    <artifactId>demo4-security</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>demo4-security</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-springsecurity5</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.8</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.0.5</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```

1.建表

```sql
CREATE TABLE persistent_logins (
	username VARCHAR (64) NOT NULL,
	series VARCHAR (64) PRIMARY KEY,
	token VARCHAR (64) NOT NULL,
	last_used TIMESTAMP NOT NULL
)
```

2.编写登录界面

```html
 <form action="/user/login" method="post">
        用户名：<input type="text" name="username">
        <br/>
        密码：<input type="text" name="password">
        <br/>
        记住我：<input type="checkbox" name="remember-me">
        <input type="submit" value="login">
        <br/>
    </form>
```

<font color="red">记住我的；name必须为remember-me 不可以为其他值</font>

2.配置类逻辑进行书写，注入数据源，

```java
 // 注入数据源对象
    @Autowired
    private DataSource dataSource;

    public PersistentTokenRepository  persistentTokenRepository(){
        JdbcTokenRepositoryImpl jdbcTokenRepository = new JdbcTokenRepositoryImpl();
        jdbcTokenRepository.setDataSource(dataSource);
        // 这里可以自己进行创建表
        // jdbcTokenRepository.setCreateTableOnStartup();
        return jdbcTokenRepository;
    }
```

3.配置类中配置自动登录

```java
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()//开启登录配置
                // 设置哪些访问路径不需要访问权限，可以直接访问
                .antMatchers("/resources/**", "/signup", "/about", "/home").permitAll()
                // 设置user对象的权限 只能设置一个权限
                .antMatchers("/test/index").hasAuthority("admin")
                // 当用户权限有其中一个就可以
                .antMatchers("/hello").hasAnyAuthority("admins,manager")
                // 必须有这个role 的，才可以访问 hasRole("sale")==>ROLE_sale
                //如果配置的hasRole("sale1") AuthorityUtils.commaSeparatedStringToAuthorityList("admins,ROLE_sale"); 为这个是则访问失败403
                // .antMatchers("/hello").hasRole("sale")
                // 配置多个
                // .antMatchers("/hello").hasAnyRole("sale1,admin")
                .anyRequest().authenticated()//表示剩余的其他接口，登录之后就能访问
                .and()
                .formLogin()
                //定义登录页面，未登录时，访问一个需要登录之后才能访问的接口，会自动跳转到该页面
                .loginPage("/login.html")
                .loginProcessingUrl("/user/login") // 登录访问路径
                .defaultSuccessUrl("/success.html").permitAll()
                .and().rememberMe().tokenRepository(persistentTokenRepository())
                .tokenValiditySeconds(60) //设置有效时长以秒为单位
                .userDetailsService(userDetailsServiceByDB)
                .and()
                .csrf().disable(); // 关闭CSrf的配置
        http.exceptionHandling().accessDeniedPage("/unauth.html");
        // 跳转到log
        http.logout().logoutUrl("/logout").logoutSuccessUrl("/log");
    }
```

controller层：

```java
/**
 * @Author kay三石
 * @date:2020/10/25
 */
@Controller
public class TestController {

    @GetMapping("/hello")
    @ResponseBody
    public String hello(){
        return "hello";
    }
	/**
     * @ResponseBody返回Json串不走视图解析器
     * @return
     */
    @GetMapping("/log")
    @ResponseBody
    public String log(){
        return "login";
    }
}

```

访问后勾选记住我：

[![BeObFg.png](https://s1.ax1x.com/2020/10/25/BeObFg.png)](https://imgchr.com/i/BeObFg)

cookie中会有这样一条记录

[![BeOfSA.md.png](https://s1.ax1x.com/2020/10/25/BeOfSA.md.png)](https://imgchr.com/i/BeOfSA)

这个时候可以在数据库中看到有这样的一条记录，这个是spring自动给加入的数据

[![BeOMzn.md.png](https://s1.ax1x.com/2020/10/25/BeOMzn.md.png)](https://imgchr.com/i/BeOMzn)

### 注解的使用

#### @Secured

判断是否具有角色，另外需要注意的是这个匹配的字符串需要添加前缀“ROLE_”; 用户具有哪些角色可以访问这个方法

用注解之前必须在启动类中先开启注解。 

@EnableGlobalMethodSecurity(securedEnabled=true)

```java
@SpringBootApplication
@EnableGlobalMethodSecurity(securedEnabled=true)
public class Demo3SecurityApplication {

    public static void main(String[] args) {
        SpringApplication.run(Demo3SecurityApplication.class, args);
    }

}

```

```
 /**
     * 注解的方式进行设置
     * @return
     */
    @GetMapping("/hello2")
    @Secured({"ROLE_sale","ROLE_manage"})
    public String hello2() {
        return "hello2";
    }
```

在userdeatailService设置用户的角色

```java
        List<GrantedAuthority> authorities= AuthorityUtils.commaSeparatedStringToAuthorityList("sale");

```

#### @PreAuthorize

进入方法之前进去进行验证

先开启注解功能@EnableGlobalMethodSecurity(prePostEnabled = true)

@PreAuthorize :注解适合计入方法前的权限验证，@Preauthorize可以将登录用户的roles/premissions参数传到方法中、

```java
@SpringBootApplication
@EnableGlobalMethodSecurity(securedEnabled=true,prePostEnabled = true)
public class Demo3SecurityApplication {

    public static void main(String[] args) {
        SpringApplication.run(Demo3SecurityApplication.class, args);
    }

}
```

```java
@GetMapping("/hello3")
//@PreAuthorize("hasRole('ROLE_管理员')")
@PreAuthorize("hasAnyAuthority('menu:system')")
public String hello23() {
    return "hello2";
}
```

在userdeatailService设置用户的角色(同上)

####  @PostAuthorize

在方法执行之后再校验，适合用于有返回值的校验

先开启注解功能@EnableGlobalMethodSecurity(prePostEnabled = true)

```java
  /**
     * 进入方法之后进行校验
     * @return
     */
    @GetMapping("/hello3")
    //@PreAuthorize("hasRole('ROLE_管理员')")
    @PostAuthorize("hasAnyAuthority('admins')")
    public String hello231() {
        return "hello2";
    }
```

在userdeatailService设置用户的角色(同上)

#### @PostFilter

权限验证之后对数据进行过虑，留下用户名是admin1的用户，

表达式中filterObject引用的是方法返回值List的某一个元素

```java
@RequestMapping("getall")
@PreAuthorize("hasRole('ROLE_管理员')")
@PostFilter("filterObject.username == 'admin1'")
public List<UserInfo> getAllUser(){

}
```

在userdeatailService设置用户的角色(同上)

####  @PreFilter

进入控制器之前对数据进行过虑

```java
  /**
     * 进入方法之前进行过滤
     * @return
     */
    @GetMapping("/hello3")
    @PreAuthorize("hasRole('ROLE_管理员')")
    @PreFilter(value="filterObject.id%2==0")
    public String hello23() {
        return "hello2";
    }
```

### Spring Security配置&WebSecurityConfigurerAdapter

WebSecurityConfigurerAdapter提供了简洁方式来创建WebSecurityConfigurer，其作为基类，可通过实现该类自定义配置类。其自动从SpringFactoriesLoader查找AbstractHttpConfigurer让我们去扩展，想要实现必须创建一个AbstractHttpConfigurer的扩展类，并在classpath路径下创建一个文件META-INF/spring.factories 

#### configure(AuthenticationManagerBuilder auth) 认证管理器配置方法

```
认证管理器配置configure(AuthenticationManagerBuilder auth)
用于配置认证管理器AuthenticationManager,就是所有的userDetails相关的它都会管，包含PasswordEncoder密码机。
```

#### configure(HttpSecurity http) 核心过滤器配置方法

```
configure(HttpSecurity)方法定义了哪些URL路径应该被保护，哪些不应该。具体来说，“/”和“/ home”路径被配置为不需要任何身份验证。所有其他路径必须经过身份验证。
用来配置 HttpSecurity 。 HttpSecurity 用于构建一个安全过滤器链 SecurityFilterChain 。SecurityFilterChain 最终被注入核心过滤器
```

#### configure(WebSecurity web) 安全过滤器配置方法

```
configure(WebSecurity web) 用于配置WebSecurity webSecurity是基于servlet Filter的配置SpringSecurityFilterChain.而 springSecurityFilterChain 又被委托给了 Spring Security 核心过滤器 Bean DelegatingFilterProxy 。 相关逻辑你可以在 WebSecurityConfiguration 中找到。
我们一般不会过多来自定义 WebSecurity , 使用较多的使其ignoring() 方法用来忽略 Spring Security 对静态资源的控制。如果一个请求路径不设置拦截：
 1.设置地址匿名访问
 2.直接过滤掉该地址，及该地址不走Spring Security 过滤器链。
```

#### 配置实例

```java
package com.kaysanshi.demosecurity.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.builders.WebSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.AuthenticationFailureHandler;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import org.springframework.security.web.authentication.logout.LogoutHandler;
import org.springframework.security.web.authentication.logout.LogoutSuccessHandler;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/**
 * Description:
 * 这个类也是spring security 的配置的一种方式
 *
 * @EnableWebSecurity :以启用Spring Security的Web安全支持，并提供Spring MVC集成
 * @date:2020/10/23 11:38
 * @author: kaysanshi
 **/
@Configuration
public class SecurityAllConfig extends WebSecurityConfigurerAdapter {

    Filter verifyCodeFilter=new Filter() {
        @Override
        public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {

        }
    };


    /**
     * init初始化：获取HttpSecurity和配置FilterSecurityInterceptor拦截器到WebSecurity
     *
     * @param web
     * @throws Exception
     */
    @Override
    public void init(WebSecurity web) throws Exception {
        super.init(web);
    }

    /**
     * 认证管理器配置方法：
     * configure(AuthenticationManagerBuilder auth)
     * 用于配置认证管理器AuthenticationManager,就是所有的userDetails相关的它都会管，包含PasswordEncoder密码机。
     *
     * @param auth
     * @throws Exception
     */
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        super.configure(auth);
    }


    /**
     * 核心过滤器配置的方法
     * configure(WebSecurity web)
     * 用于配置WebSecurity webSecurity是基于servlet Filter的配置SpringSecurityFilterChain.而 springSecurityFilterChain 又被委托给了 Spring Security 核心过滤器 Bean DelegatingFilterProxy 。 相关逻辑你可以在 WebSecurityConfiguration 中找到。
     * 我们一般不会过多来自定义 WebSecurity , 使用较多的使其ignoring() 方法用来忽略 Spring Security 对静态资源的控制。
     * 如果一个请求路径不设置拦截：
     * 1.设置地址匿名访问
     * 2.直接过滤掉该地址，及该地址不走Spring Security 过滤器链。
     * 下面方法是演示直接过率掉该地址。
     * WebSecurity的使用
     *
     * @param web
     * @throws Exception
     */
    @Override
    public void configure(WebSecurity web) throws Exception {
        super.configure(web);
        // 忽略那些拦截
        web.ignoring().antMatchers("/vercode");
    }

    /**
     * configure(HttpSecurity)方法定义了哪些URL路径应该被保护，哪些不应该。具体来说，“/”和“/ home”路径被配置为不需要任何身份验证。所有其他路径必须经过身份验证。
     * 用来配置 HttpSecurity 。 HttpSecurity 用于构建一个安全过滤器链 SecurityFilterChain 。SecurityFilterChain 最终被注入核心过滤器
     * HttpSecurity的使用：
     *
     * @param http
     * @throws Exception
     */
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // addFilterBefore 在指定的Filter类的位置添加过滤器
//        http.addFilterBefore(verifyCodeFilter, UsernamePasswordAuthenticationFilter.class);
        http.authorizeRequests()//开启登录配置
                // 可以通过访问的多个URL模式。任何用户都可以访问URL以"/resources/", equals "/signup", 或者 "/about"开头的URL。
//                .antMatchers("/resources/**", "/signup", "/about", "/home").permitAll()
                .antMatchers("/hello").hasRole("admin")//表示访问 /hello 这个接口，需要具备 admin 这个角色
                //	任何以"/db/" 开头的URL需要用户同时具有 "ROLE_ADMIN" 和 "ROLE_DBA"。和上面一样我们的 hasRole 方法也没有使用 "ROLE_" 前缀
                .antMatchers("/db/**").access("hasRole('ADMIN') and hasRole('DBA')")
                .anyRequest().authenticated()//表示剩余的其他接口，登录之后就能访问
                .and()
                .formLogin()
                //定义登录页面(并不是接口)，未登录时，访问一个需要登录之后才能访问的接口，会自动跳转到该页面
                .loginPage("/templates/login_p.html")
                //登录处理接口
                .loginProcessingUrl("/doLogin")
                //定义登录时，用户名的 key，默认为 username
                .usernameParameter("uname")
                //定义登录时，用户密码的 key，默认为 password
                .passwordParameter("passwd")
                //登录成功的处理器
                .successHandler(new AuthenticationSuccessHandler() {
                    @Override
                    public void onAuthenticationSuccess(HttpServletRequest req, HttpServletResponse resp, Authentication authentication) throws IOException, ServletException {
                        resp.setContentType("application/json;charset=utf-8");
                        PrintWriter out = resp.getWriter();
                        out.write("success");
                        out.flush();
                    }
                })
                .failureHandler(new AuthenticationFailureHandler() {
                    @Override
                    public void onAuthenticationFailure(HttpServletRequest req, HttpServletResponse resp, AuthenticationException exception) throws IOException, ServletException {
                        resp.setContentType("application/json;charset=utf-8");
                        PrintWriter out = resp.getWriter();
                        out.write("fail");
                        out.flush();
                    }
                })
                .permitAll()//和表单登录相关的接口统统都直接通过
                .and()
                .logout()  // 提供注销支持，使用WebSecurityConfigurerAdapter会自动被应用。
                .logoutUrl("/logout") // 	设置触发注销操作的URL (默认是/logout). 如果CSRF内启用（默认是启用的）的话这个请求的方式被限定为POST。
                .logoutSuccessHandler(new LogoutSuccessHandler() {
                    // 注销后的操作
                    @Override
                    public void onLogoutSuccess(HttpServletRequest req, HttpServletResponse resp, Authentication authentication) throws IOException, ServletException {
                        resp.setContentType("application/json;charset=utf-8");
                        PrintWriter out = resp.getWriter();
                        out.write("logout success");
                        out.flush();
                    }
                })
                .invalidateHttpSession(true) // 指定是否在注销时让HttpSession无效。 默认设置为 true。
                // 添加一个LogoutHandler.默认SecurityContextLogoutHandler会被添加为最后一个LogoutHandler
                .addLogoutHandler(new LogoutHandler() {
                    @Override
                    public void logout(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Authentication authentication) {

                    }
                })
                // 允许指定在注销成功时将移除的cookie
                .deleteCookies("")
                .permitAll()
                .and()
                .httpBasic()
                .and()
                .csrf().disable();
    }

    /**
     * 将单个用户设置在内存中。该用户的用户名为“user”，密码为“password”，角色为“USER”。
     *
     * @param auth
     * @throws Exception
     */
    @Autowired
    public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
        auth
                .inMemoryAuthentication()
                .withUser("user").password("password").roles("USER");
    }
}

package com.kaysanshi.demosecurity.config;


import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;

/**
 * Description:
 *
 * @date:2020/10/23 15:16
 * @author: kaysanshi
 **/
@Configuration
public class MvcConfig  extends WebMvcConfigurerAdapter {
    /**
     * 配置静态资源访问
     * @param registry
     */
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/templates/**").addResourceLocations("classpath:/templates/");
        super.addResourceHandlers(registry);
    }
}

```

#### WebSecurity



#### HttpSecurity

```java
public final class HttpSecurity extends AbstractConfiguredSecurityBuilder<DefaultSecurityFilterChain, HttpSecurity> implements SecurityBuilder<DefaultSecurityFilterChain>, HttpSecurityBuilder<HttpSecurity> {}

```

`HttpSecurity`最终可以得到一个`DefaultSecurityFilterChain`通过的是`build()`方法

- `HttpSecurity`维护了一个过滤器的列表，这个过滤器的列表最终放入了`DefaultSecurityFilterChain`这个过滤器链中
- `HttpSecurity`最终提供了很多的配置，然而所有的配置也都是为了处理维护我们的过滤器列表



| 方法                  | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| `openidLogin()`       | 用于基于 OpenId 的验证                                       |
| `headers()`           | 将安全标头添加到响应                                         |
| `cors()`              | 配置跨域资源共享（ CORS ）                                   |
| `sessionManagement()` | 允许配置会话管理                                             |
| `portMapper()`        | 允许配置一个`PortMapper`(`HttpSecurity#(getSharedObject(class))`)，其他提供`SecurityConfigurer`的对象使用 `PortMapper` 从 HTTP 重定向到 HTTPS 或者从 HTTPS 重定向到 HTTP。默认情况下，Spring Security使用一个`PortMapperImpl`映射 HTTP 端口8080到 HTTPS 端口8443，HTTP 端口80到 HTTPS 端口443 |
| `jee()`               | 配置基于容器的预认证。 在这种情况下，认证由Servlet容器管理   |
| `x509()`              | 配置基于x509的认证                                           |
| `rememberMe`          | 允许配置“记住我”的验证                                       |
| `authorizeRequests()` | 允许基于使用`HttpServletRequest`限制访问                     |
| `requestCache()`      | 允许配置请求缓存                                             |
| `exceptionHandling()` | 允许配置错误处理                                             |
| `securityContext()`   | 在`HttpServletRequests`之间的`SecurityContextHolder`上设置`SecurityContext`的管理。 当使用`WebSecurityConfigurerAdapter`时，这将自动应用 |
| `servletApi()`        | 将`HttpServletRequest`方法与在其上找到的值集成到`SecurityContext`中。 当使用`WebSecurityConfigurerAdapter`时，这将自动应用 |
| `csrf()`              | 添加 CSRF 支持，使用`WebSecurityConfigurerAdapter`时，默认启用 |
| `logout()`            | 添加退出登录支持。当使用`WebSecurityConfigurerAdapter`时，这将自动应用。默认情况是，访问URL”/ logout”，使HTTP Session无效来清除用户，清除已配置的任何`#rememberMe()`身份验证，清除`SecurityContextHolder`，然后重定向到”/login?success” |
| `anonymous()`         | 允许配置匿名用户的表示方法。 当与`WebSecurityConfigurerAdapter`结合使用时，这将自动应用。 默认情况下，匿名用户将使用`org.springframework.security.authentication.AnonymousAuthenticationToken`表示，并包含角色 “ROLE_ANONYMOUS” |
| `formLogin()`         | 指定支持基于表单的身份验证。如果未指定`FormLoginConfigurer#loginPage(String)`，则将生成默认登录页面 |
| `oauth2Login()`       | 根据外部OAuth 2.0或OpenID Connect 1.0提供程序配置身份验证    |
| `requiresChannel()`   | 配置通道安全。为了使该配置有用，必须提供至少一个到所需信道的映射 |
| `httpBasic()`         | 配置 Http Basic 验证                                         |
| `addFilterAt()`       | 在指定的Filter类的位置添加过滤器                             |

#### AuthenticationManagerBuilder

### CSRF(Cross-site request forgery)

跨站请求伪造，也成为on-click attack 通常缩写为CSRF 或者XSRF，

CSRF利用的是网站对用户浏览器的信任。XSS利用的是用户对指定网站的信任。

跨站请求攻击，简单地说，是攻击者通过一些技术手段欺骗用户的浏览器去访问一个自己曾经认证过的网站并运行一些操作（如发邮件，发消息，甚至财产操作如转账和购买商品）。由于浏览器曾经认证过，所以被访问的网站会认为是真正的用户操作而去运行。这利用了web中用户身份验证的一个漏洞：**简单的身份验证只能保证请求发自某个用户的浏览器，却不能保证请求本身是用户自愿发出的**。

从spring Security 4.0开始 默认情况下会启用CSRF保护，以防止CSRF攻击应用程序，Spring Security CSRF 会针对PARCH,POST,pUT,DELTE方法进行防护。

Spring Security 实现 CSRF的原理：

1. 生成 csrfToken 保存到 HttpSession 或者 Cookie 中。 
2. 请求到来时，从请求中提取 csrfToken，和保存的 csrfToken 做比较，进而判断当 前请求是否合法。主要通过 CsrfFilter 过滤器来完成。 

### 微服务认证授权实现思路

1、认证授权过程分析 
（1）如果是基于 Session，那么 Spring-security 会对 cookie里的 sessionid进行解析，找 到服务器存储的 session信息，然后判断当前用户是否符合请求的要求。 

（2）如果是 token，则是解析出 token，然后将当前请求加入到 Spring-security 管理的权限 信息中去 如果系统的模块众多，每个模块都需要进行授权与认证，所以我们选择基于 token 的形式 进行授权与认证，用户根据用户名密码认证成功，然后获取当前用户角色的一系列权限 值，并以用户名为 key，权限列表为 value 的形式存入 redis 缓存中，根据用户名相关信息 生成 token 返回，浏览器将 token 记录到 cookie 中，每次调用 api 接口都默认将 token 携带 到 header 请求头中，Spring-security 解析 header 头获取 token 信息，解析 token 获取当前 用户名，根据用户名就可以从 redis 中获取权限列表，这样 Spring-security 就能够判断当前 请求是否有权限访问 

[![BmKME8.png](https://s1.ax1x.com/2020/10/25/BmKME8.png)](https://imgchr.com/i/BmKME8)

2、权限管理数据模型 

 

[参考：Spring Security OAuth2 开发指南中文版](https://www.ktanx.com/blog/p/5008)

[Spring Security系列一 权限控制基本功能实现](https://www.ktanx.com/blog/p/4600)

[Spring Security系列二 用户登录认证数据库实现](https://www.ktanx.com/blog/p/4916)

