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



[参考：Spring Security OAuth2 开发指南中文版](https://www.ktanx.com/blog/p/5008)

[Spring Security系列一 权限控制基本功能实现](https://www.ktanx.com/blog/p/4600)

[Spring Security系列二 用户登录认证数据库实现](https://www.ktanx.com/blog/p/4916)

