<font color="blue" size="10px">&copy;&reg;&trade;</font>

### Properties

```
## mysql
spring.datasource.platform=mysql
spring.datasource.url=jdbc:mysql://${mysql.ip:192.168.3.200}:${mysql.port:13306}/demoa_service?serverTimezone=Asia/Shanghai&useSSL=false&characterEncoding=utf-8
spring.datasource.username=${mysql.username:demo}
spring.datasource.password=${mysql.password:123456}
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
## redis
spring.redis.host=${redis.host:192.168.3.201}
spring.redis.port=${redis.port:6379}
spring.redis.password=${redis.password:123456}
spring.redis.timeout=5000
spring.redis.maxTotal=50
spring.redis.maxIdle=10
spring.redis.database=3
## strem集成rabbitMq
spring.cloud.stream.binders.rabbitmq.type=rabbit
spring.cloud.stream.binders.rabbitmq.environment.spring.rabbitmq.host=${rabbitmq.host:192.168.3.201}
spring.cloud.stream.binders.rabbitmq.environment.spring.rabbitmq.port=${rabbitmq.port:5672}
spring.cloud.stream.binders.rabbitmq.environment.spring.rabbitmq.username=${rabbitmq.username:admin}
spring.cloud.stream.binders.rabbitmq.environment.spring.rabbitmq.password=${rabbitmq.password:admin}
## eureka
eureka.instance.prefer-ip-address=true
eureka.instance.metadata-map.auth-code=HWJCGliay4uXH3xhUTaXnB==
eureka.instance.metadata-map.service-name=demoa
eureka.instance.metadata-map.service-desc=demoa
eureka.instance.metadata-map.service-sys=${SERVICE_SYS:BEWEMP}
eureka.instance.metadata-map.service-namespace=${SERVICE_NAMESPACE:XMGL}
eureka.client.serviceUrl.defaultZone=http://direwolf:direwolf@${REGISTRY_MASTER_HOST:192.168.3.200}:${REGISTRY_MASTER_PORT:8761}/eureka/
## Spring Secutity Outh2
security.oauth2.client.loadBalanced=true
security.oauth2.client.clientId=demoa-service
security.oauth2.client.clientSecret=123456
security.oauth2.client.accessTokenUri=http://${TOKEN_URI:192.168.3.200}:${TOKEN_PORT:7777}/auth/oauth/token
security.oauth2.client.grant-type=client_credentials
security.oauth2.client.scope=server
security.oauth2.resource.loadBalanced=true
security.oauth2.resource.user-info-uri=http://${eureka.instance.metadata-map.service-sys}.auth-service/auth/oauth/current
server.port=${port:7878}

### 配置tkmybatis的日志
logging.level.包名=debug
logging.level.com.blueeearth.bewemp= debug
```

#### yaml的执行顺序

**一、bootstrap.yml（bootstrap.properties）与application.yml（application.properties）执行顺序**

bootstrap.yml（bootstrap.properties）用来程序引导时执行，应用于更加早期配置信息读取，如可以使用来配置application.yml中使用到参数等

application.yml（application.properties) 应用程序特有配置信息，可以用来配置后续各个模块中需使用的公共参数等。

加载顺序

bootstrap.yml > application.yml > application-dev(prod).yml

- **在springcloud工程中，资源文件夹中有 application.yml 、 application-dev(prod、test).yml时，,工程启动时会启动一个端口为8080的tomcat，即使在application.yml、application-dev.yml中指定了端口，工程也不会加载，推测资源文件加载失败。**
- **当资源文件中有bootstrap.yml、application.yml 、 application-dev(prod、test).yml文件时，并且三个文件都设置三个不同的tomcat的端口，此时application-dev或者bootstrap-dev.yml会覆盖掉bootstrap.yml中的端口。**
- **当只有bootstrap.yml application.yml时，application.yml中的端口则不会覆盖掉bootstrap.yml中的端口**

**在application.yml或者bootstrap.yml中用 spring.profiles.active=dev 或者prod 来指定运行哪个环境。如果在dev中包含了bootstrap.yml中没有的参数配置 则会进行给加入到运行时环境，bootstrap.yml可当作一个基本的配置文件，对于bootstrap-dev.yml可看作是指定的运行，会先运行加载bootstrap.yml再加载bootstrap.dev.xml**

[参考：使用 spring.profiles.active来区分配置](https://blog.csdn.net/uniquewonderq/article/details/79963719)

[SpringCloud 配置文件 application.yml和 bootstrap.yml区别](https://www.cnblogs.com/vwater/p/10374515.html)

### Spring Security

使用Spring Secruity的原因有很多，单大部分都发现了javaEE的Servlet规范或EJB规范中的安全功能缺乏典型企业应用场景所需的深度。提到这些规范，重要的是要认识到他们在WAR或EAR级别无法移植。因此如果你更换服务器环境，这里有典型的大量工作去重新配置你的应用程序员安全到新的目标环境。使用Spring Security 解决了这些问题，也为你提供许多其他有用的，可定制的安全功能。

Spring Security提供一套的授权功能。这里有三个主要的热点区域，授权web请求、授权方法是否可以被调用和授权访问单个域对象的实例。

#### demo实例:

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

#### Spring Security配置&WebSecurityConfigurerAdapter

WebSecurityConfigurerAdapter提供了简洁方式来创建WebSecurityConfigurer，其作为基类，可通过实现该类自定义配置类。其自动从SpringFactoriesLoader查找AbstractHttpConfigurer让我们去扩展，想要实现必须创建一个AbstractHttpConfigurer的扩展类，并在classpath路径下创建一个文件META-INF/spring.factories 

##### configure(AuthenticationManagerBuilder auth) 认证管理器配置方法

```
认证管理器配置configure(AuthenticationManagerBuilder auth)
用于配置认证管理器AuthenticationManager,就是所有的userDetails相关的它都会管，包含PasswordEncoder密码机。
```

##### configure(HttpSecurity http) 核心过滤器配置方法

```
configure(HttpSecurity)方法定义了哪些URL路径应该被保护，哪些不应该。具体来说，“/”和“/ home”路径被配置为不需要任何身份验证。所有其他路径必须经过身份验证。
用来配置 HttpSecurity 。 HttpSecurity 用于构建一个安全过滤器链 SecurityFilterChain 。SecurityFilterChain 最终被注入核心过滤器
```

##### configure(WebSecurity web) 安全过滤器配置方法

```
configure(WebSecurity web) 用于配置WebSecurity webSecurity是基于servlet Filter的配置SpringSecurityFilterChain.而 springSecurityFilterChain 又被委托给了 Spring Security 核心过滤器 Bean DelegatingFilterProxy 。 相关逻辑你可以在 WebSecurityConfiguration 中找到。
我们一般不会过多来自定义 WebSecurity , 使用较多的使其ignoring() 方法用来忽略 Spring Security 对静态资源的控制。如果一个请求路径不设置拦截：
 1.设置地址匿名访问
 2.直接过滤掉该地址，及该地址不走Spring Security 过滤器链。
```

##### 配置实例

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

[![BBjxzV.png](https://s1.ax1x.com/2020/11/02/BBjxzV.png)](https://imgchr.com/i/BBjxzV)

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

[参考git中学习：Spring Security](https://github.com/kaysanshi/webNote/blob/master/note/Spring%20Security.md)

[参考：Spring Security OAuth2 开发指南中文版](https://www.ktanx.com/blog/p/5008)

[Spring Security系列一 权限控制基本功能实现](https://www.ktanx.com/blog/p/4600)

[Spring Security系列二 用户登录认证数据库实现](https://www.ktanx.com/blog/p/4916)





### Spring Cloud Stream



#### Spring Cloud Stream 集成RabbitMQ

主要配置：

1.是rabbitmq的连接环境设置

```yaml
spring:
 cloud: 
    stream: 
      binders: 
        rabbitmq: 
          type: rabbit
          environment:
            spring:
              rabbitmq:
                host: ${rabbitmq.host:192.168.3.201}
                port: ${rabbitmq.port:5672}
                username: ${rabbitmq.username:dev}
                password: ${rabbitmq.password:dev}
                virtual-host: ${rabbitmq.virtual-host:/dev}
```

2.是bindings的操作：Bindings是：exchange通过特定的规则绑定到queue(Bindings就是规则)，例如一个Exchange E通过路由方式发送消息到Queue Q上面，那么Q就被绑定到E上面。Bindins可能有一个操作路由属性一般用在一些Exchange 类型上面。路由key一般都是由exchange通过路由方式将消息推送到被绑定的队列上，这个路由key就像一个过滤器一样

```yml
spring: 
  cloud:     
    stream: 
      bindings: 
        authorityOutput: 
          destination: authority
          content-type: application/json
          binder: rabbitmq
        beLogOutput: 
          destination: beLog
          content-type: application/json
          binder: rabbitmq
```

3.消息处理方

```java
public interface AuthoritySink {
    
    String AUTHORITY_INPUT = "authorityInput";
    
    @Input(AuthoritySink.AUTHORITY_INPUT)
    SubscribableChannel authorityInput();
}



@Slf4j
@EnableBinding(value = {AuthoritySink.class})
public class AuthorityReceiver {
    @Autowired
    private SAuthorityMapper authorityMapper;
    
    @StreamListener(AuthoritySink.AUTHORITY_INPUT)
    @Transactional(rollbackFor = Exception.class)
    public void receiveMessageFromChannel(@Payload Message <String> payload) {
        List<AuthorityMsg> auths = JSON.parseArray(payload.getPayload(), AuthorityMsg.class);
        if(CollectionUtils.isEmpty(auths))
            return;
        String serviceName = auths.get(0).getServiceName();
        Example example = new Example(SAuthority.class);
        example.createCriteria().andEqualTo("serviceName", serviceName);
        authorityMapper.deleteByExample(example);
        List<SAuthority> records = auths.stream().map(el -> {
            SAuthority record = new SAuthority();
            record.setId(el.getAuthorityId());
            record.setModuleName(el.getModuleName());
            record.setServiceName(serviceName);
            record.setDescription(el.getDescription());
            return record;
        }).collect(Collectors.toList());
        authorityMapper.insertListSelective(records);
        log.info("🗡🗡🗡微服务：{}，已经权限入库🗡🗡🗡",serviceName);
    }
}

public interface BELogSink {
    
    String BE_LOG_INPUT = "beLogInput";
    
    @Input(BELogSink.BE_LOG_INPUT)
    SubscribableChannel beLogInput();
}

@EnableBinding(value = {BELogSink.class})
public class BELogReceiver {
    @Autowired
    private BeLogMapper beLogMapper;
    
    @StreamListener(BELogSink.BE_LOG_INPUT)
    public void receiveMessageFromChannel1(@Payload Message <String> payload) {
        BeLog log = JSON.parseObject(payload.getPayload(), BeLog.class);
        if(null == log)
            return;
        log.setId(IdUtils.simpleUUID());
        beLogMapper.insertSelective(log);
    }
}
```



### Spring Cloud openFeign

Feign是声明性Web服务客户端。 它使编写Web服务客户端更加容易。 要使用Feign，请创建一个接口并对其进行注释。 它具有可插入注释支持，包括Feign注释和JAX-RS注释。 Feign还支持可插拔编码器和解码器。 Spring Cloud添加了对Spring MVC注释的支持，并支持使用Spring Web中默认使用的相同HttpMessageConverters。 Spring Cloud集成了Eureka和Spring Cloud LoadBalancer，以在使用Feign时提供负载平衡的http客户端。 就是通过把http请求封装到了注解中。

Spring Cloud 的 Feign 支持中的一个核心概念是命名客户机。每个佯装的客户机都是一个组件集合的一部分，这些组件一起工作，根据需要联系一个远程服务器，这个集合有一个名称，作为一个使用@feignclient 注释的应用程序开发人员，你可以给它一个名称。Spring Cloud 使用 FeignClientsConfiguration 根据需要为每个命名客户机创建一个新的集合，作为 ApplicationContext。其中包括一个假动作。解码器，一个假装。编码器，和一个假装。合约。可以使用@feignclient 注释的 contextId 属性覆盖集合的名称。

Hystrix 支持熔断(fallback)的概念: 一个默认的代码路径，在熔断或出现错误时执行。要为给定的@feignclient 启用熔断，请将熔断属性设置为实现熔断的类名。您还需要将实现声明为 springbean。

```java
/**
 * 去请求feign服务端itoken-service-admin中的服务接口
 * @Author kay三石
 * @date:2019/6/22
 */
// value 是声明的方式指向了 服务提供者
@FeignClient(value="itoken-service-admin",fallback = AdminServiceFallback.class)
public interface AdminService  extends BaseClientService {

    /**
     * 根据 ID 获取管理员
     *
     * @return
     */
    @RequestMapping(value = "v1/admins", method = RequestMethod.GET)
    public String get(
            @RequestParam(required = true, value = "userCode") String userCode
    );

```

如果需要访问制造回退触发器的原因，可以在@feignclient 中使用 fallbackFactory 属性。

```java
// name 调用服务的名称和value等
@FeignClient(name=ServiceNameConstants.DEMOB_SERVICE,fallbackFactory = DemobServiceClientFallbackFactory.class)
public interface DemobServiceClient {
    
    @GetMapping(value = "/demob/test/getDemobById")
    DemobDTO getDemobById(@RequestParam("id")String id);
    
}   
@Component
public class DemobServiceClientFallbackFactory implements FallbackFactory<DemobServiceClient> {

    @Override
    public DemobServiceClient create(Throwable cause) {
        DemobServiceClientFallback demobServiceClientFallback = new DemobServiceClientFallback();
        demobServiceClientFallback.setCause(cause);
        return demobServiceClientFallback;
    }

}
@Slf4j
public class DemobServiceClientFallback implements DemobServiceClient {
    @Setter
    private Throwable cause;
    
    @Override
    public DemobDTO getDemobById(String id) {
        log.error("根据id获取demob信息失败",cause);
        throw new FirstException();
    }

}
```

| 注解           | 接口Target     | 使用说明                                                     |
| -------------- | -------------- | ------------------------------------------------------------ |
| `@RequestLine` | 方法上         | 定义HttpMethod 和 UriTemplate. UriTemplate 中使用`{}` 包裹的表达式，可以通过在方法参数上使用@Param 自动注入 |
| `@Param`       | 方法参数       | 定义模板变量，模板变量的值可以使用名称的方式使用模板注入解析 |
| `@Headers`     | 类上或者方法上 | 定义头部模板变量，使用@Param 注解提供参数值的注入。如果该注解添加在接口类上，则所有的请求都会携带对应的Header信息；如果在方法上，则只会添加到对应的方法请求上 |
| `@QueryMap`    | 方法上         | 定义一个键值对或者 pojo，参数值将会被转换成URL上的 query 字符串上 |
| `@HeaderMap`   | 方法上         | 定义一个HeaderMap, 与 UrlTemplate 和HeaderTemplate 类型，可以使用@Param 注解提供参数值 |



#### FeignClient 的配置参数

| 属性名        | 默认值     | 作用                                                         | 备注                                        |
| ------------- | ---------- | ------------------------------------------------------------ | ------------------------------------------- |
| value         | 空字符串   | 调用服务名称，和name属性相同                                 |                                             |
| serviceId     | 空字符串   | 服务id，作用和name属性相同                                   | 已过期                                      |
| name          | 空字符串   | 调用服务名称，和value属性相同                                |                                             |
| url           | 空字符串   | 全路径地址或hostname，http或https可选                        |                                             |
| decode404     | false      | 配置响应状态码为404时是否应该抛出FeignExceptions             |                                             |
| configuration | {}         | 自定义当前feign client的一些配置                             | 参考FeignClientsConfiguration               |
| fallback      | void.class | 熔断机制，调用失败时，走的一些回退方法，可以用来抛出异常或给出默认返回数据。 | 底层依赖hystrix，启动类要加上@EnableHystrix |
| path          | 空字符串   | 自动给所有方法的requestMapping前加上前缀，类似与controller类上的requestMapping |                                             |
| primary       | true       |                                                              |                                             |

#### Feign原理：

- 启动时，程序会进行包扫描，扫描所有包下所有@FeignClient注解的类，并将这些类注入到spring的IOC容器中。当定义的Feign中的接口被调用时，通过JDK的动态代理来生成RequestTemplate。
- RequestTemplate中包含请求的所有信息，如请求参数，请求URL等。
- RequestTemplate声场Request，然后将Request交给client处理，这个client默认是JDK的HTTPUrlConnection，也可以是OKhttp、Apache的HTTPClient等。
- 最后client封装成LoadBaLanceClient，结合ribbon负载均衡地发起调用。



### 注解：

```
//Retention注解决定MyAnnotation注解的生命周期
//Target注解决定MyAnnotation注解可以加在哪些成分上，如加在类身上，或者属性身上，或者方法身上等成分
 /*
 * @Retention(RetentionPolicy.SOURCE)
 * 这个注解的意思是让MyAnnotation注解只在java源文件中存在，编译成.class文件后注解就不存在了
 * @Retention(RetentionPolicy.CLASS)
 * 这个注解的意思是让MyAnnotation注解在java源文件(.java文件)中存在，编译成.class文件后注解也还存在，
 * 被MyAnnotation注解类标识的类被类加载器加载到内存中后MyAnnotation注解就不存在了
 */
```

```java
@Target({ ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface BELog {
    
    /**
    * 描述
     */
    String value();
    /**
     *模块id<br>   
     *  不传则自动获取当前模块id,如果当前不在任何模块则为-1
    */
    String moduleId() default "-1";
    
    
    public Type type() default Type.NORMAL;
    
    
    
    
    enum Type {

        NORMAL(0),
        
        DEBUG(1),
        
        WARNING(2),
        
        DANGER(3);
        
        private Integer value;

        public Integer getValue() {
            return value;
        }

        public void setValue(Integer value) {
            this.value = value;
        }

        private Type(Integer value) {
            this.value = value;
        }
        
        
            
    }

}
 @BELog("获取所有Demoa")
    public List<DemoaDTO> getAllDemoa() {
        return demoaService.getAllDemoa();
    }
```

### BeanUtils

#### BeanUtils.copyProperties(demob, result)

https://blog.csdn.net/w05980598/article/details/79134379

### 注解 @RequiredArgsConstructor

生成带有必需参数的构造函数。 必需的参数是最终字段和具有约束的字段，例如@NonNull 。
完整的文档可在@lconstructor的项目lombok功能页面上找到 。即使未列出，该注释也具有onConstructor参数。 有关更多详细信息，请参见完整的文档。

这个事基于lombok的使用时必须导入lombok包。

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.12</version>
</dependency>
```

同时注意几点：
	1.必须声明的变量为final

​	2.根据构造器注入的，相当于当容器调用带有一组参数的类构造函数时，基于构造函数的 DI 就完成了，其中每个参数代表一个对其他类的依赖。基于构造方法为属性赋值，容器通过调用类的构造方法将其进行依赖注入

使用当我们需要注入Bean的时候可以直接在类名称上使用。代替了Autowrited注解

```java
@RequiredArgsConstructor
@RestController
@RequestMapping("/omponent")
public class ComponentController {
    private final ComponentService ComponentService;
}    
```

### ResponseBodyAdvice接口实现统一的响应拦截

ResponseBodyAdvice接口时spring4.1的特性，其作用时在响应体写出前做一些处理，比如修改返回值，加密等。

允许在执行@ResponseBody或ResponseEntity控制器方法之后但在使用HttpMessageConverter编写正文之前自定义响应。
可以直接在RequestMappingHandlerAdapter和ExceptionHandlerExceptionResolver注册实现，或者更有可能在@ControllerAdvice中注解，在这种情况下，它们都会被两者自动检测到。

```java
public interface ResponseBodyAdvice<T> {

	/**
	 * 此组件是否支持给定的控制器方法返回类型和所选的HttpMessageConverter类型。
	 * 这个方法返回true后才会执行下面的beforeBodyWrite方法
	 */
	boolean supports(MethodParameter returnType, Class<? extends HttpMessageConverter<?>> converterType);

	/**
	 * 在选择HttpMessageConverter之后且在调用其write方法之前调用
	 */
	@Nullable
	T beforeBodyWrite(@Nullable T body, MethodParameter returnType, MediaType selectedContentType,
			Class<? extends HttpMessageConverter<?>> selectedConverterType,
			ServerHttpRequest request, ServerHttpResponse response);

}
```

```java
// 统一的返回
@RestControllerAdvice(basePackages = "com.kaysanshi")
public class ResponseControllerAdvice implements ResponseBodyAdvice<Object> {

    @Override
    public boolean supports(MethodParameter returnType, Class<? extends HttpMessageConverter<?>> converterType) {
        // 获取当前controller请求的方法是否有SkipR注解
        SkipR skipR = returnType.getMethod().getAnnotation(SkipR.class);
        if(null != skipR)
            return false;
        // 是否是返回的R对象
        return !returnType.getMethod().getReturnType().equals(R.class);
    }

    @Override
    public Object beforeBodyWrite(Object body, MethodParameter returnType, MediaType selectedContentType,
            Class<? extends HttpMessageConverter<?>> selectedConverterType, ServerHttpRequest request,
            ServerHttpResponse response) {
        if(request.getHeaders().containsKey(SecurityConstants.INNER) && SecurityConstants.INNER_TRUE.equalsIgnoreCase(request.getHeaders().getFirst(SecurityConstants.INNER))) {
            return body;
        }
        if (returnType.getGenericParameterType().equals(String.class)) {
            return JSON.toJSONString(new R<>(body));
        }
        return new R<>(body);
    }

}
//
@Builder
@Accessors(chain = true)
@AllArgsConstructor
@NoArgsConstructor
@Data
public class R<T> implements Serializable {
    private static final long serialVersionUID = -9141307870732228707L;
    
    
    private String state;
    private String msg;
    private T obj;
    
    public R(T obj){
        super();
        this.state = CommonConstants.SUCCESS;
        this.obj = obj;
    }
    
    public R(FirstException e) {
        super();
        this.state = e.getCode();
        this.msg = e.getMessage();
    }
    
    public R(FirstArgsException e) {
        super();
        this.state = e.getCode();
        this.msg = e.getMessage();
    }
    
    public R(LogicException e) {
        super();
        this.state = e.getCode();
        this.msg = e.getMessage();
    }
    
    public R(LogicArgsException e) {
        super();
        this.state = e.getCode();
        this.msg = e.getMessage();
    }
    
    public R(AccessDeniedException e) {
        super();
        this.state = SecurityConstants.ACCESS_DENIED;
        this.msg = e.getMessage();
    }
    
    public R(NotBreakerException e) {
        super();
        this.state = e.getCode();
        this.msg = e.getMessage();
    }
    
    public R(Throwable e) {
        super();
        if(InfoUtils.isContain(e.getMessage())) {
            this.state = e.getMessage();
            this.msg = InfoUtils.getInfo(this.state);
        } else {            
            this.state = CommonConstants.FAIL;
            this.msg = InfoUtils.getInfo(FirstException.ERROR_CODE);
        }
    }
    
    
    public R<T> success(T obj){
        this.state = CommonConstants.SUCCESS;
        this.obj = obj;
        return this;
    }
    
    public R<T> error(String msg){
        this.state = CommonConstants.FAIL;
        this.msg = msg;
        return this;
    }
    
    public R<T> error(){
        this.state = CommonConstants.FAIL;
        this.msg = InfoUtils.getInfo(FirstException.ERROR_CODE);
        return this;
    }
    
    @JSONField(serialize = false)
    @JsonIgnore
    public Boolean getIsSuccess() {
        return Objects.equal(CommonConstants.SUCCESS, this.state);
    }
    
    public String toJson() {
        return JSON.toJSONString(this);
    }
}
```



### nginx

#### nginx在windows下的命令

cmd 进入Nginx解压目录 执行以下命令

start nginx : 启动nginx服务

nginx -s reload ：修改配置后重新加载生效

nginx -s reopen ：重新打开日志文件
nginx -t -c /path/to/nginx.conf 测试nginx配置文件是否正确



验证配置是否正确: nginx -t

查看Nginx的版本号：nginx -V

启动Nginx：start nginx

快速停止或关闭Nginx：nginx -s stop

正常停止或关闭Nginx：nginx -s quit

配置文件修改重装载命令：nginx -s reload

#### nginx在linux下的命令

```javascript
-c </path/to/config> 为 Nginx 指定一个配置文件，来代替缺省的。路径应为绝对路径

-t 不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件。

-v 显示 nginx 的版本。

-V 显示 nginx 的版本，编译器版本和配置参数。
```

验证配置是否正确：进入nginx安装目录sbin下，输入命令./nginx -t

配置文件修改重装载命令：进入nginx可执行目录sbin下，输入命令**./nginx -s reload** 即可

启动代码格式：nginx安装目录地址 -c nginx配置文件地址  或着：进入到安装nginx目录下面的sbin启动

./nginx

```javascript
nginx -c /xxxx/nginx/nginx.conf
```

关闭：

```javascript
ps -aux|grep nginx
kill -9 nginx主进程号
```

### 数据字典



### 目录树：

我们一般查询的时候都是通过递归查询的。比如获取到某个父节点下的所有的子集就可以使用levelDeep



level_deep

[![DW7jXT.png](https://s3.ax1x.com/2020/12/01/DW7jXT.png)](https://imgchr.com/i/DW7jXT)

比如一个数据的level_deep: f1884a28f8a8459c9c41cd0fc52ef556|97cba5e00c304f4788a4f615d17b1cc7|c3dee6049730481fa9a78b924b42c0e6|23a562a989824fbe9e5d8fa13b5709bf

当前数据：
| id                               | create_date           | code     | name               | pid                              | seq  | level_deep                                                   |
| -------------------------------- | --------------------- | -------- | ------------------ | -------------------------------- | ---- | ------------------------------------------------------------ |
| 23a562a989824fbe9e5d8fa13b5709bf | 2020-11-30 17:27:13.0 | 30526020 | 乙供设备或材料采购 | c3dee6049730481fa9a78b924b42c0e6 | 2    | f1884a28f8a8459c9c41cd0fc52ef556&#124;97cba5e00c304f4788a4f615d17b1cc7&#124;c3dee6049730481fa9a78b924b42c0e6&#124;23a562a989824fbe9e5d8fa13b5709bf |

拆解level_deep 
最后一个是自身的id，倒数第二个是pid,倒数第三个是pid的pid,一直找到顶层目录。顶层的pid置为-1

levelDeep字段维护：查看gitee上的代码示例



### 前端JS库

underscore.js https://underscorejs.net/#each

klay.js https://github.com/kieler/klayjs

angular.js

