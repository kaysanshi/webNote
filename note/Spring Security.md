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

`WebSecurity`是`Spring Security`的一个`SecurityBuilder`。它的任务是基于一组`WebSecurityConfigurer`构建出一个`Servlet Filter`,具体来讲就是构建一个`Spring Security`的`FilterChainProxy`实例。这个`FilterChainProxy`实现了`Filter`接口，也是通常我们所说的`Spring Security Filter Chain`,所构建的`FilterChainProxy`实例缺省会使用名称`springSecurityFilterChain`作为`bean`注册到容器，运行时处理`web`请求过程中会使用该`bean`进行安全控制。

每个`FilterChainProxy`包装了一个`HttpFirewall`和若干个`SecurityFilterChain`, 这里每个 `SecurityFilterChain`要么对应于一个要忽略安全控制的`URL`通配符(`RequestMatcher`)；要么对应于一个要进行安全控制的`URL`通配符(`HttpSecurity`)

`WebSecurity`在初始化的时候会扫描`WebSecurityConfigurerAdapter`配置器适配器的子类（即生成`HttpSecurity配置器``的FilterChainProxy`）。所有的`配置器`会被调用`init();configure();`初始化配置，其中生成的每个`HttpSecurity配置器`都代表了一个过滤器链。

[![BBjxzV.png](https://s1.ax1x.com/2020/11/02/BBjxzV.png)](

#### HttpSecurity

`HttpSecurity`作为一个`建造者`，是如何去建造出`SecurityFilterChain`过滤器链实例的！

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

 Oauth2

第三方认证技术方案主要是解决认证协议的通用标准，因为要实现跨系统认证，各系统之间要通过遵循一定的接口协议

OAuth（开放授权）是一个开放标准，允许用户授权第三方移动应用访问他们存储在另外的服务提供者上的信息，而不需要将用户名和密码提供给第三方移动应用或分享他们数据的所有内容，OAuth2.0是OAuth协议的延续版本，但不向后兼容OAuth 1.0即完全废止了OAuth1.0。

[![BUCyw9.png](https://s1.ax1x.com/2020/10/31/BUCyw9.png)](https://imgchr.com/i/BUCyw9)

#### 角色

##### 客户端

本身不存储资源，需要通过资源拥有者的授权去请求资源服务器的资源

##### 资源拥有者

通常为用户，也可以是应用程序，即该资源的拥有者。

##### 授权服务器

用来对资源拥有的身份进行认证，对访问资源进行授权，客户端想要访问资源需要通过认证服务器由资源拥有者授权可访问。

##### 资源服务器

存储资源的服务器，比如我，网站用户管理服务器存储了网站的的信息。

#### 常用术语

- 客户凭证：客户端的clientid和密码用户认证用户。
- 令牌(tokens) :授权服务器在接收到客户端请求后颁发的访问令牌
- 作用域(scopes): 客户端请求访问令牌时由资源拥有者额外指定的细分权限

#### 令牌类型

授权码：用于授权码授权类型，用于交换访问令牌和刷新令牌

- 访问令牌：用于代表一个用户或服务直接去访问受保护的资源。
- 刷新令牌：用于去授权服务器获取一个刷新访问令牌。
- BearerToken: 不管谁拿到Token 都可以访问资源，类似现金。
- Proof of Possession Token ：可以校验client是否对token有明确的拥有权。

#### 特点

##### 优点：

安全，客户端不接触用户密码，服务器端更集中保护。

使用广泛。

短寿命和封装的token

资源服务器和授权服务器解耦

集中式授权，简化客户端。

Http/JSON友好以用于请求和传递token

##### 缺点：

协议框架太广泛，造成各种实现的兼容性和互操作性差

不是一个认证协议，本身不能告诉你任何信息。

#### 授权模式

##### 授权码模式

使用最多的一种授权模式。

##### 简化授权模式

##### 密码模式

##### 客户端模式

[参考这个](https://www.jianshu.com/p/68f22f9a00ee)

### Spring Security Oauth2

#### 授权服务器

[![BUFxOO.png](https://s1.ax1x.com/2020/10/31/BUFxOO.png)](https://imgchr.com/i/BUFxOO)

- `Authorize Endpoint : 授权端点，进行授权`
- `Token endpoint :令牌端点，进行授权拿到对应的token`
- `Introspection Endpoint :校验端点，校验token `
- `Revocation Endpoint` : `撤销端点，撤销授权`

#### Spring Security Oauth2

[![BUAdr8.png](https://s1.ax1x.com/2020/10/31/BUAdr8.png)](https://imgchr.com/i/BUAdr8)

流程：

1.用户访问，此时，没有token , Oauth2RestTemplate会出错，这个报错信息将会被Oauth2ClientContextFilter 捕获并重定向到认证服务器。

2.认证服务器通过Authorization endpoint 进行授权，并通过AuthhorizationServerTokenServices生成授权码并返回给客户端

3.客户端拿到授权码去认证服务器通过Token endpoint 调用AuthorizationServerTokenServices生成Token并返回给客户端

4.客户端拿到Token 去资源服务器访问资源，一般会通过Oauth2AuthenticationManager调用ResourceServerTokenServices进行校验，校验通过后可以获取到资源

##### 授权码模式：

效果是访问http://localhost:8080/oauth/authorize?response_type=code&client_id=admin&redirect_uti=http://www.baidu.com 跳到百度页面并且带授权码，并获取token

先看效果图

![BUvA0I.png](https://s1.ax1x.com/2020/10/31/BUvA0I.png)

![BUvE7t.png](https://s1.ax1x.com/2020/10/31/BUvE7t.png)

通过postman获取token:

![BUz7p8.png](https://s1.ax1x.com/2020/10/31/BUz7p8.png)

![BUzH1S.png](https://s1.ax1x.com/2020/10/31/BUzH1S.png)


通过token获取到资源，比如获取当前的用户。

[![BaSwjg.png](https://s1.ax1x.com/2020/10/31/BaSwjg.png)](https://imgchr.com/i/BaSwjg)

当我们把令牌给修改了就会报错。未认证。

看代码：

###### pom.xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.5.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.kaysanshi</groupId>
    <artifactId>spring-security-oauth2-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring-security-oauth2-demo</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
        <spring-cloud.version>Greenwich.SR2</spring-cloud.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-oauth2</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
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
    </dependencies>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

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

###### java代码：

```java
/**
 * 使用授权码模式
 */
@SpringBootApplication
public class SpringSecurityOauth2DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringSecurityOauth2DemoApplication.class, args);
    }

}
////////////////////////////////////////////
//////////// 配置实现类         /////////////
////////////////////////////////////////////
/**
 * Description:
 * spring security配置
 * WebSecurityConfigurerAdapter是默认情况下 Spring security的http配置
 * 优先级高于ResourceServerConfigurer，用于保护oauth相关的endpoints，同时主要作用于用户的登录（form login，Basic auth）
 *@EnableWebSecurity :表示启动webSecurity
 * @date:2020/10/29 9:41
 * @author: kaysanshi
 **/
@Configuration
@EnableWebSecurity
public class SecurityConfigure extends WebSecurityConfigurerAdapter {

    @Bean
    PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    /**
     * 进行
     *
     * @param http
     * @throws Exception
     */
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
                .authorizeRequests()
                .antMatchers("/oauth/**", "/login/**", "logout/**")
                .permitAll()
                .anyRequest()
                .authenticated()
                .and()
                .formLogin()
                .permitAll();
    }
}
/////////////////////////////

/**
 * Description: 授权服务器的配置
 * author2配置
 * AuthorizationServerConfigurerAdapter 包括：
 * ClientDetailsServiceConfigurer：用来配置客户端详情服务（ClientDetailsService），客户端详情信息在这里进行初始化，你能够把客户端详情信息写死在这里或者是通过数据库来存储调取详情信息。
 * AuthorizationServerSecurityConfigurer：用来配置令牌端点(Token Endpoint)的安全约束.
 * AuthorizationServerEndpointsConfigurer：用来配置授权（authorization）以及令牌（token）的访问端点和令牌服务(token services)。
 *
 * @date:2020/10/29 9:30
 * @author: kaysanshi
 **/
@Configuration
@EnableAuthorizationServer // 开启认证授权服务器
public class AuthorizationServerConfigure extends AuthorizationServerConfigurerAdapter {

    /**
     * 配置密码加密，因为再UserDetailsService是依赖与这个类的。
     *
     * @return
     */
    // 指定密码的加密方式
    @Autowired
    private PasswordEncoder passwordEncode;

    /**
     * ClientDetailsServiceConfigurer
     * 主要是注入ClientDetailsService实例对象（唯一配置注入）。其它地方可以通过ClientDetailsServiceConfigurer调用开发配置的ClientDetailsService。
     * 系统提供的二个ClientDetailsService实现类：JdbcClientDetailsService、InMemoryClientDetailsService。
     * 演示提供了用户名和密码
     * @param clients
     * @throws Exception
     */
    @Override
    public void configure(ClientDetailsServiceConfigurer clients)
            throws Exception {
        // 配置一个客户端用于password认证的。同时也可以同时配置两个，可以再配置一个基于client认证的。
        clients.inMemory()
                // 配置clientId
                .withClient("admin")
                // 配置client-secret
                .secret(passwordEncode.encode("112233"))
                // 配置token过期时间
                .accessTokenValiditySeconds(2630)
                // 配置 redirectUri，用于授权成功后跳转
                .redirectUris("http://www.baidu.com")
                // 配置申请的权限范围
                .scopes("all")
                // 配置grant_type 表示授权类型。 使用授权码模式
                .authorizedGrantTypes("authorization_code");

    }
}
/////////////////////////
/**
 * Description:
 * 配置资源服务器 : ResourceServerConfigurerAdapter
 * ResourceServerConfigurerAdapter是默认情况下spring security oauth 的http配置。
 *
 * @date:2020/10/29 9:44
 * @author: kaysanshi
 **/
@Configuration
@EnableResourceServer
public class ResourceServerConfigure extends ResourceServerConfigurerAdapter {
    /**
     * 配置响应资源的访问。
     *
     * @param http
     * @throws Exception
     */
    // 配置 URL 访问权限
    @Override
    public void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .anyRequest()
                .authenticated()
                .and()
                .requestMatchers()
                .antMatchers("/test/**");
    }
}
////////////////////////////////////////////
//////////// user类  ///////////////////////
////////////////////////////////////////////
/**
 * 自定义User 实现时一定要返回true 否则不能进行认证
 * @Author kay三石
 * @date:2020/10/31
 */
public class User  implements UserDetails {
    private  String username;
    private  String password;
    private List<GrantedAuthority> authorities;

    public User(String username, String password, List <GrantedAuthority> authorities) {
        this.username = username;
        this.password = password;
        this.authorities = authorities;
    }

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return null;
    }

    @Override
    public String getPassword() {
        return password;
    }

    @Override
    public String getUsername() {
        return username;
    }
	// 自动生成时是返回的为false,会导致不能进入
    @Override
    public boolean isAccountNonExpired() {
        return true;
    }
	// 自动生成时是返回的为false,会导致不能进入

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }
	// 自动生成时是返回的为false,会导致不能进入

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }	
	// 自动生成时是返回的为false,会导致不能进入
    @Override
    public boolean isEnabled() {
        return true;
    }
}
////////////////////////////////////////////
//////////// userService实现类  /////////////
////////////////////////////////////////////
/**
 * @Author kay三石
 * @date:2020/10/31
 */
@Service
public class UserService implements UserDetailsService {
    /**
     * 注入刚引入的bean
     */
    @Autowired
    private PasswordEncoder passwordEncoder;

    @Override
    public UserDetails loadUserByUsername(String s) throws UsernameNotFoundException {
        String password =passwordEncoder.encode("123");
        return new User("admin",password, AuthorityUtils.commaSeparatedStringToAuthorityList("admin"));
    }
}
////////////////////////////////////////////
//////////// controller  ///////////////////
////////////////////////////////////////////
/**
 * Description:
 *  加入了author2的认证，所以要加入token访问资源时
 * @date:2020/10/29 9:46
 * @author: kaysanshi
 **/
@RestController
@RequestMapping("/test")
public class TestController {
    /**
     * 获取当前用户
     * @return
     */
    @GetMapping("/getCurrentUser")
    public Object getCurrentUser(Authentication authentication){
        return authentication.getPrincipal();
    }
}



```

<font color="red">遇到问题，但是后台并未报错，只需要把这个给修改即可 : 用户已失效，用户账户已经锁定</font>

[![BUjSMj.png](https://s1.ax1x.com/2020/10/31/BUjSMj.png)](https://imgchr.com/i/BUjSMj)

<font color="blue">解决：</font>明明用户名和密码正确，而且没有设置状态锁定，怎么被锁定了呢？这是由于我们在重写`UserDetails`接口时，有个默认实现的方法`public boolean isAccountNonLocked()`，默认返回的是`false`，

```java
 @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
```



##### 密码模式

使用密码模式获取资源：

看效果：

获取token

[![BaiSQf.md.png](https://s1.ax1x.com/2020/10/31/BaiSQf.md.png)](https://imgchr.com/i/BaiSQf)



通过token获取信息

[![Baipy8.md.png](https://s1.ax1x.com/2020/10/31/Baipy8.md.png)](https://imgchr.com/i/Baipy8)

###### pom.xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.5.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.kaysanshi</groupId>
    <artifactId>spring-security-oauth2</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring-security-oauth2</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
        <spring-cloud.version>Greenwich.SR2</spring-cloud.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-oauth2</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
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
    </dependencies>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

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

###### java 代码

由于这个只是配置类中不太一样，这里我只展现出配置类的代码：

```java

/**
 * Description:
 * author2配置
 * AuthorizationServerConfigurerAdapter 包括：
 * ClientDetailsServiceConfigurer：用来配置客户端详情服务（ClientDetailsService），客户端详情信息在这里进行初始化，你能够把客户端详情信息写死在这里或者是通过数据库来存储调取详情信息。
 * AuthorizationServerSecurityConfigurer：用来配置令牌端点(Token Endpoint)的安全约束.
 * AuthorizationServerEndpointsConfigurer：用来配置授权（authorization）以及令牌（token）的访问端点和令牌服务(token services)。
 *
 * @date:2020/10/29 9:30
 * @author: kaysanshi
 **/
@Configuration
@EnableAuthorizationServer // 开启认证授权服务器
public class AuthorizationServerConfigure extends AuthorizationServerConfigurerAdapter {

    // 密码授权的操作就是通过这个对象把密码传入授权服务器的
    @Autowired
    private AuthenticationManager authenticationManager;

    // 将令牌信息存储到内存中
    @Autowired(required = false)
    private TokenStore inMemoryTokenStore;

    // 该对象将为刷新token提供支持
    @Autowired
    private UserService userService;

    @Autowired
    private PasswordEncoder passwordEncoder;

    /**
     * ClientDetailsServiceConfigurer
     * 主要是注入ClientDetailsService实例对象（唯一配置注入）。其它地方可以通过ClientDetailsServiceConfigurer调用开发配置的ClientDetailsService。
     * 系统提供的二个ClientDetailsService实现类：JdbcClientDetailsService、InMemoryClientDetailsService。
     *
     * @param clients
     * @throws Exception
     */
    @Override
    public void configure(ClientDetailsServiceConfigurer clients)
            throws Exception {
        // 配置一个基于password认证的。
        clients.inMemory()
                // 配置clientId
                .withClient("admin")
                // 配置client-secret
                .secret(passwordEncoder.encode("112233"))
                // 配置token过期时间
                .accessTokenValiditySeconds(2630)
                // 配置 redirectUri，用于授权成功后跳转
                .redirectUris("http://www.baidu.com")
                // 配置申请的权限范围
                .scopes("all")
                // 配置grant_type 表示授权类型。 使用密码模式
                .authorizedGrantTypes("password");
    }

    /**
     * 使用密码模式所需配置
     * AuthorizationServerEndpointsConfigurer 访问端点配置 是一个装载类
     * 装载Endpoints所有相关的类配置（AuthorizationServer、TokenServices、TokenStore、ClientDetailsService、UserDetailsService）。
     * @param endpoints
     */
    @Override
    public void configure(AuthorizationServerEndpointsConfigurer endpoints) {
        endpoints.tokenStore(inMemoryTokenStore) //配置令牌的存储（这里存放在内存中）
                .authenticationManager(authenticationManager)
                .userDetailsService(userService);
    }
}
///////////////////////////////////////////////
///                                         ///
////////////////////////////////////////////////
/**
 * Description:
 * spring security配置
 * WebSecurityConfigurerAdapter是默认情况下 Spring security的http配置
 * 优先级高于ResourceServerConfigurer，用于保护oauth相关的endpoints，同时主要作用于用户的登录（form login，Basic auth）
 *
 * @date:2020/10/29 9:41
 * @author: kaysanshi
 **/
@Configuration
@EnableWebSecurity
public class SecurityConfigure extends WebSecurityConfigurerAdapter {

    @Bean
    public PasswordEncoder passwordEncoder() {
        // 使用BCrypt强哈希函数加密方案（密钥迭代次数默认为10）
        return new BCryptPasswordEncoder();
    }

    /**
     * 为了让认证配置类注入使用
     *
     * @return
     * @throws Exception
     */
    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }

    /**
     * @param http
     * @throws Exception
     */
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
                .authorizeRequests()
                .antMatchers("/oauth/**", "/login/**", "logout/**")
                .permitAll()
                .anyRequest()
                .authenticated()
                .and()
                .formLogin()
                .permitAll();
    }
}

```



##### 将token存到redis

```java
/**
 * Description:
 * 配置把token存到redis 中的redis链接
 * @date:2020/10/29 15:14
 * @author: kaysanshi
 **/
@Configuration
public class TokenStoreConfigure {

    @Autowired
    private RedisConnectionFactory redisConnectionFactory;

    @Bean
    public TokenStore redisTokenStore(){
        return new RedisTokenStore(redisConnectionFactory);
    }
}

```

在授权服务器中使用redis进行存储

```java
package com.kaysanshi.springsecurityoauth2.configure;


import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.oauth2.config.annotation.configurers.ClientDetailsServiceConfigurer;
import org.springframework.security.oauth2.config.annotation.web.configuration.AuthorizationServerConfigurerAdapter;
import org.springframework.security.oauth2.config.annotation.web.configuration.EnableAuthorizationServer;
import org.springframework.security.oauth2.config.annotation.web.configurers.AuthorizationServerEndpointsConfigurer;
import org.springframework.security.oauth2.config.annotation.web.configurers.AuthorizationServerSecurityConfigurer;
import org.springframework.security.oauth2.provider.token.TokenStore;

/**
 * Description: 授权服务器的配置
 * author2配置
 * AuthorizationServerConfigurerAdapter 包括：
 * ClientDetailsServiceConfigurer：用来配置客户端详情服务（ClientDetailsService），客户端详情信息在这里进行初始化，你能够把客户端详情信息写死在这里或者是通过数据库来存储调取详情信息。
 * AuthorizationServerSecurityConfigurer：用来配置令牌端点(Token Endpoint)的安全约束.
 * AuthorizationServerEndpointsConfigurer：用来配置授权（authorization）以及令牌（token）的访问端点和令牌服务(token services)。
 *
 * @date:2020/10/29 9:30
 * @author: kaysanshi
 **/
@Configuration
@EnableAuthorizationServer // 开启认证授权服务器
public class AuthorizationServerConfigure extends AuthorizationServerConfigurerAdapter {

    // 该对象用来支持 password 模式
    @Autowired
    AuthenticationManager authenticationManager;

    // 将令牌信息存储redis中
    @Autowired
    TokenStore redisToken;

    // 该对象将为刷新token提供支持
    @Autowired
    UserDetailsService userDetailsService;

    /**
     * 配置密码加密，因为再UserDetailsService是依赖与这个类的。
     *
     * @return
     */
    // 指定密码的加密方式
    @Bean
    PasswordEncoder passwordEncoder() {
        // 使用BCrypt强哈希函数加密方案（密钥迭代次数默认为10）
        return new BCryptPasswordEncoder();
    }

    /**
     * ClientDetailsServiceConfigurer
     * 主要是注入ClientDetailsService实例对象（唯一配置注入）。其它地方可以通过ClientDetailsServiceConfigurer调用开发配置的ClientDetailsService。
     * 系统提供的二个ClientDetailsService实现类：JdbcClientDetailsService、InMemoryClientDetailsService。
     *
     * @param clients
     * @throws Exception
     */
    @Override
    public void configure(ClientDetailsServiceConfigurer clients)
            throws Exception {
        // 配置一个客户端用于password认证的。同时也可以同时配置两个，可以再配置一个基于client认证的。
        clients.inMemory()
                .withClient("password")
                .authorizedGrantTypes("password", "refresh_token") //授权模式为password和refresh_token两种
                .accessTokenValiditySeconds(1800) // 配置access_token的过期时间
                .resourceIds("rid") //配置资源id
                .scopes("all") // 允许授权范围
                .secret("$2a$10$RMuFXGQ5AtH4wOvkUqyvuecpqUSeoxZYqilXzbz50dceRsga.WYiq") //123加密后的密码
                ;
    }

    /**
     * AuthorizationServerEndpointsConfigurer 访问端点配置 是一个装载类
     * 装载Endpoints所有相关的类配置（AuthorizationServer、TokenServices、TokenStore、ClientDetailsService、UserDetailsService）。
     * tokenService用于存到redis中
     *
     * @param endpoints
     */
    @Override
    public void configure(AuthorizationServerEndpointsConfigurer endpoints) {
        endpoints.tokenStore(redisToken) //配置令牌的存到redis
                .authenticationManager(authenticationManager)
                .userDetailsService(userDetailsService);
    }

    /**
     * AuthorizationServerSecurityConfigurer继承SecurityConfigurerAdapter.
     * 也就是一个 Spring Security安全配置提供给AuthorizationServer去配置AuthorizationServer的端点（/oauth/****）的安全访问规则、过滤器Filter。
     *
     * @param security
     */
    @Override
    public void configure(AuthorizationServerSecurityConfigurer security) {
        // 表示支持 client_id 和 client_secret 做登录认证
        // 允许使用表单认证
        security.allowFormAuthenticationForClients();
    }
}

```

### JWT

#### 常见的认证机制

##### HTTP Basic Auth

HTTP Basic Auth简单点说明就是每次请求API时都提供用户的username和password，简言之，Basic Auth是配合RESTful API 使用的最简单的认证方式，只需提供用户名密码即可，但由于有把用户名密码暴露给第三方客户端的风险，在生产环境下被使用的越来越少。因此，在开发对外开放的RESTful API时，<font color="red">尽量避免采用HTTP Basic Auth</font>

##### Cookie Auth

Cookie认证机制就是为一次请求认证在服务端创建一个Session对象，同时在客户端的浏览器端创建了一个Cookie对象；通过客户端带上来Cookie对象来与服务器端的session对象匹配来实现状态管理的。默认的，当我们关闭浏览器的时候，cookie会被删除。但可以通过修改cookie 的expire time使cookie在一定时间内有效；

[![Bwb5o6.png](https://s1.ax1x.com/2020/11/01/Bwb5o6.png)](https://imgchr.com/i/Bwb5o6)

##### Oauth

OAuth（开放授权）是一个开放标准，允许用户授权第三方移动应用访问他们存储在另外的服务提供者上的信息，而不需要将用户名和密码提供给第三方移动应用或分享他们数据的所有内容.

OAuth 允许用户提供一个令牌，而不是用户名和密码来访问他们存放在特定服务提供者的数据。而每一个令牌授权一个特定的第三方系统，在特定的时刻访问特定的资源。这样OAuth让用户可以授权第三方网站访问他们存储在另外服务提供者的某些特定的内信息，而非所有内容。比如一个微博开放平台的OAuth请求。

![](http://www.sinaimg.cn/blog/developer/wiki/oauth_flowchart.jpg)

这种基于OAuth的认证机制适合哟用于个人消费者类的互联网产品，比如社交类APP等应用。缺点：过重

##### Token Auth

使用基于token的身份验证方法，在服务端不需要存储用户的登录信息。大概的流程是：

1. 客户端使用用户名和密码请求登录
2. 服务端收到请求，去验证用户名与密码
3. 验证成功后，服务端会签发一个token,再把token发送得到客户端。
4. 客户端收到token以后可以把他存储起来，比如放到cookie中
5. 客户端每次向服务器请求资源的时候需要带着服务端签发的token
6. 服务端收到请求，然后去验证客户端请求里面带着的token,如果验证成功，就向客户端返回请求的数据。

[![BwboFK.png](https://s1.ax1x.com/2020/11/01/BwboFK.png)](https://imgchr.com/i/BwboFK)

这比http basic auth更安全，比cookie auth更节约服务器资源，比Oauth更轻量。

<font color="green">Token机制相对于Cookie机制又有什么好处呢？</font>

- 支持跨域访问: Cookie是不允许垮域访问的，这一点对Token机制是不存在的，前提是传输的用户认证信息通过HTTP头传输.
- 无状态(也称：服务端可扩展行):Token机制在服务端不需要存储session信息，因为Token 自身包含了所有登录用户的信息，只需要在客户端的cookie或本地介质存储状态信息.
- 更适用CDN: 可以通过内容分发网络请求你服务端的所有资料（如：javascript，HTML,图片等），而你的服务端只要提供API即可.
- 去耦: 不需要绑定到一个特定的身份验证方案。Token可以在任何地方生成，只要在你的API被调用的时候，你可以进行Token生成调用即可.
- 更适用于移动应用: 当你的客户端是一个原生平台（iOS, Android，Windows 8等）时，Cookie是不被支持的（你需要通过Cookie容器进行处理），这时采用Token认证机制就会简单得多。
- CSRF:因为不再依赖于Cookie，所以你就不需要考虑对CSRF（跨站请求伪造）的防范。
- 性能: 一次网络往返时间（通过数据库查询session信息）总比做一次HMACSHA256计算 的Token验证和解析要费时得多.
- 不需要为登录页面做特殊处理: 如果你使用Protractor 做功能测试的时候，不再需要为登录页面做特殊处理.
- 基于标准化:你的API可以采用标准化的 JSON Web Token (JWT). 这个标准已经存在多个后端库（.NET, Ruby, Java,Python, PHP）和多家公司的支持（如：Firebase,Google, Microsoft）

[参考几种常见的认证机制](https://www.cnblogs.com/xiaofenguo/p/7514854.html)

#### JWT简介

https://jwt.io/

Json web token (JWT), 是为了在网络应用环境间传递声明而执行的一种基于JSON的开放标准（[(RFC 7519](https://link.jianshu.com?t=https://tools.ietf.org/html/rfc7519)).该token被设计为紧凑且安全的，特别适用于分布式站点的单点登录（SSO）场景。JWT的声明一般被用来在身份提供者和服务提供者间传递被认证的用户身份信息，以便于从资源服务器获取资源，也可以增加一些额外的其它业务逻辑所必须的声明信息，该token也可直接被用于认证，也可被加密。

优点：

1. JWT基于jon,非常方便解析
2. 可以在令牌中自定义丰富的内容，易扩展
3. 通过非对称加密算法及数字签名技术，JWT防止篡改，安全性高。
4. 资源服务使用JWT可不依赖认证服务即可完成授权。

缺点：

JWT令牌长，占用空间比较大。

##### JWT组成：

一个JWT实际上就是一个字符串，他有三部分组成，头部，载荷与签名。

###### 头部（Header）

头部是用于描述关于该JWT的最基本的信息，例如其类型及签名所用的算法等。这也可以被表示成一个JSON对象 。base64编码，可以进行明文解码

```json
{
 "alg":"HS256",
 "typ":"JWT"
}
```

- `typ`:是类型。

- `alg`:签名算法，这里使用的算法是HS256算法。

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```



###### 载荷（Payload）

负载就是存放有效信息的的地方。包含3个部分：

- 标准中注册的声明

  **iss**: jwt签发者

   **sub**: jwt所面向的用户

   **aud**: 接收jwt的一方

   **exp**: jwt的过期时间，这个过期时间必须要大于签发时间

   **nbf**: 定义在什么时间之前，该jwt都是不可用的.

   **iat**: jwt的签发时间

   **jti**: jwt的唯一身份标识，主要用来作为一次性token,从而回避重放攻击。

- 公共的声明

  公共的声明可以添加任何的信息，一般添加用户的相关信息或其他业务需要的必要信息.但不建议添加敏感信息，因为该部分在客户端可解密.

- 私有的声明

  私有声明是提供者和消费者所共同定义的声明，一般不建议存放敏感信息，因为base64是对称解密的，意味着该部分信息可以归类为明文信息。定义一个私有的声明

  ```json
  {
    "sub": "1234567890",
    "name": "John Doe",
    "admin": true
  }
  ```

  然后将其进行base64加密，得到Jwt的第二部分。

  ```undefined
  eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9
  ```

###### 签名

是Jwt的第三部分，这个签证信息是由三部分组成。

- header (base64后的)
- payload (base64后的)
- secret（盐，一定要保密）

这个部分需要base64加密后的header和base64加密后的payload使用`.`连接组成的字符串，然后通过header中声明的加密方式进行加盐`secret`组合加密，然后就构成了jwt的第三部分。

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

<font color="red">注意：secret是保存在服务器端的，jwt的签发生成也是在服务器端的，`secret就是用来进行jwt的签发和jwt的验证`，`所以，它就是你服务端的私钥，在任何场景都不应该流露出去`。一旦客户端得知这个secret, 那就意味着客户端是可以自我签发jwt了。</font>

jwt 官方进行debug

[![B030nU.png](https://s1.ax1x.com/2020/11/01/B030nU.png)](https://imgchr.com/i/B030nU)

#### JJWT简介

```pom.xml
<!---JWT依赖-->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.1</version>
        </dependency>
```

```java
package com.kaysanshi.springsecurityjjwtdemo;

import io.jsonwebtoken.*;
import io.jsonwebtoken.impl.Base64Codec;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

import java.text.SimpleDateFormat;
import java.util.Base64;
import java.util.Date;

@SpringBootTest
public class SpringSecurityJjwtDemoApplicationTests {
    /**
     * 创建token
     */
    @Test
    public void testCreateToken() {
        // 设置token失效时间：
        long now =System.currentTimeMillis();
        // 过期时间计算
        long exp = now + 60*1000;

        // 创建jwtBuilder对象
        JwtBuilder jwtBuilder = Jwts.builder();
        // 声明标识{"jti":"8888"}
        jwtBuilder.setId("8888");
        // 声明主题{"sub":"Rose"}
        jwtBuilder.setSubject("Rose");
        // 创建日期{"ita":"xxxxx"}
        jwtBuilder.setIssuedAt(new Date());
        // 设置密钥
        jwtBuilder.signWith(SignatureAlgorithm.HS256, "XXXX");
        // 设置过期时间
        jwtBuilder.setExpiration(new Date(exp));

        // 获取jwt token
        String token = jwtBuilder.compact();
        System.out.println(token);

        System.out.println("==================");
        String[] split = token.split("\\.");
        System.out.println(Base64Codec.BASE64.decodeToString(split[0]));
        System.out.println(Base64Codec.BASE64.decodeToString(split[1]));

        // 盐是无法解密的。
        System.out.println(Base64Codec.BASE64.decodeToString(split[2]));

    }

    /**
     * 解析token:
     */
    @Test
    public void testParseToken() {
        String token="eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiI4ODg4Iiwic3ViIjoiUm9zZSIsImlhdCI6MTYwNDMyMzY1NiwiZXhwIjoxNjA0MzIzNzE2fQ.Qm6yJibqb12tA2txJE_eVa6EjCx1yexK9hRUjTn15-A";
        // 解析token获取负载中声明的对象
        Claims claims = Jwts.parser()
                .setSigningKey("XXXX")
                .parseClaimsJws(token)
                .getBody();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        System.out.println("id:"+claims.getId());
        System.out.println("subject:"+claims.getSubject());
        System.out.println("issuedAt:"+claims.getIssuedAt());
        System.out.println("签发时间:"+simpleDateFormat.format(claims.getIssuedAt()));
        System.out.println("过期时间:"+simpleDateFormat.format(claims.getExpiration()));
        System.out.println("当前时间:"+simpleDateFormat.format(new Date()));

    }
}

```

结果：

[![BrN1oV.md.png](https://s1.ax1x.com/2020/11/02/BrN1oV.md.png)](https://imgchr.com/i/BrN1oV)

失效的token

[![BrN8iT.md.png](https://s1.ax1x.com/2020/11/02/BrN8iT.md.png)](https://imgchr.com/i/BrN8iT)

### Spring Security Oauth2整合JWT

### Spring Security Oauth2整合单点登录





[参考：Spring Security OAuth2 开发指南中文版](https://www.ktanx.com/blog/p/5008)

[Spring Security系列一 权限控制基本功能实现](https://www.ktanx.com/blog/p/4600)

[Spring Security系列二 用户登录认证数据库实现](https://www.ktanx.com/blog/p/4916)

