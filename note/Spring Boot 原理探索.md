# Spring Boot 原理篇

## Spring Boot核心运行原理

### 核心运行原理

Spring Boot通过@EnableAutoConfiguration注解开启自动配置，加载spring.factories中注册的各种AutoConfiguration类，当某个AutoConfiguration类满足其注解@Conditional指定的生效条件（Starters提供的依赖、配置或Spring容器中是否存在某个Bean等）时，实例化该AutoConfiguration类中定义的Bean（组件等），并注入Spring容器，就可以完成依赖自动配置。

[![BsCKw6.png](https://s1.ax1x.com/2020/11/03/BsCKw6.png)](https://imgchr.com/i/BsCKw6)

```
github不支持 这个语法(本来用流程图给展示的，但是github语法不支持)
graph TD
 A[EnableAutoConfiguration<br/><br/>注解开启配置:] -->|扫描加载| B(扫描加载 spring.factoties:<br/><br/>配置注册内容)
 B -->|指定类路径| C[xxxAutoConfiguration:<br/><br/>具体实例化配置]
 D[Condiation注解:<br/><br/>判断前置条件] -->|指定前置条件|C
 E[Startes:配置及依赖支持] -->|提供配依赖支持| C 
 F[SpringBoot运行涉及的核心部分]
```

上图描述了几个核心功能的相互之间的关系。

**先说明什么是spring.factories文件**

对于在maven中引用的其他外部包加入容器的过程，需要用到spring.factories.

在springboot的运行时对于maven的项目调用其他包时需要用到这文件。

[![BDaYOs.png](https://s1.ax1x.com/2020/11/02/BDaYOs.png)](https://imgchr.com/i/BDaYOs)

<font color="red" >&hearts;也就是springboot启动时通过做个这个键值对的key去加载到这个maven jar包所依赖的其他的包的类(bean) 注入到项目中。</font>

**各个属性值的说明：**

`@EnableAutoConfiguration`：该注解由组合注解@SpringBootApplication引入，完成自动配置开启，扫描各个jar包下的spring.factories文件，并加载文件中注册的AutoConfiguration类等。

`spring.factories：`配置文件，位于jar包的META-INF目录下，按照指定格式注册了自动配置的AutoConfiguration类。spring.factories也可以包含其他类型待注册的类。该配置文件不仅存在于Spring Boot项目中，也可以存在于自定义的自动配置（或Starter）项目中。

`AutoConfiguration类：`自动配置类，代表了Spring Boot中一类以XXAutoConfiguration命名的自动配置类。其中定义了三方组件集成Spring所需初始化的Bean和条件。

`@Conditional：`条件注解及其衍生注解，在AutoConfiguration类上使用，当满足该条件注解时才会实例化AutoConfiguration类。

`Starters：`三方组件的依赖及配置，Spring Boot已经预置的组件。Spring Boot默认的Starters项目往往只包含了一个pom依赖的项目。如果是自定义的starter，该项目还需包含spring.factories文件、AutoConfiguration类和其他配置类。



### @EnableAutoConfiguration注解

#### @SpringBootApplication注解

@SpringBootApplication注是SpringBoot的核心注解，用于开启自动配置，其实是结合内部的@EnableAutoConfiguration注解开启配置

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
    
    // 排除指定自动配置类，覆盖了@EnableAutoConfiguration中定义的exclude属性
	@AliasFor(annotation = EnableAutoConfiguration.class)
	Class<?>[] exclude() default {};
    
	// 排除指定自动配置类， 覆盖了@EnableAutoConfiguration中定义的excludeName属性
	@AliasFor(annotation = EnableAutoConfiguration.class)
	String[] excludeName() default {};
    
	// 指定扫描的基础包，激活注解组件的初始化,用于激活@Compoment等注解的类的初始化。
	@AliasFor(annotation = ComponentScan.class, attribute = "basePackages")
	String[] scanBasePackages() default {};
	
    // 用于指定要扫描带注释组件的包。将扫描指定的每个类的包
    @AliasFor(annotation = ComponentScan.class, attribute = "basePackageClasses")
	Class<?>[] scanBasePackageClasses() default {};

	// 指定扫描的类，用于初始化
	@AliasFor(annotation = ComponentScan.class, attribute = "nameGenerator")
	Class<? extends BeanNameGenerator> nameGenerator() default BeanNameGenerator.class;
    
	// 是否代理指定@Bean方法以强制执行bean的生命周期行为。
	@AliasFor(annotation = Configuration.class)
	boolean proxyBeanMethods() default true;
}
```

**下面说下@EnableAutoConfiguration注解**

Spring Boot的核心功能之一就是根据约定自动管理该注解标注的类。用来实现该功能的组件之一便是@EnableAutoConfiguration注解。

@EnableAutoConfiguration位于spring-boot-autoconfigure包内，当使用@SpringBootApplication注解时，@EnableAutoConfiguration注解会自动生效。@EnableAutoConfiguration的主要功能是启动Spring应用程序上下文时进行自动配置，它会尝试猜测并配置项目可能需要的Bean。自动配置通常是基于项目classpath中引入的类和已定义的Bean来实现的。在此过程中，被自动配置的组件来自项目自身和项目依赖的jar包中。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
	// 用于覆盖配置开启/关闭自动配置的功能
	String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";

	// 根据Class排除指定的自动配置
	Class<?>[] exclude() default {};

	// 根据类名排除指定的自动配置
	String[] excludeName() default {};

}
```

<font color="green">@EnableAutoConfiguration会猜测你需要使用的Bean，但如果在实战中你并不需要它预置初始化的Bean，可通过该注解的exclude或excludeName参数进行有针对性的排除。比如，当不需要数据库的自动配置时，可通过以下两种方式让其自动配置失效</font>

```java
// 排除DataSourceAutoConfiguration
@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
public class Application {
    
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

```

###  AutoCongfigurationImportSelector源码解析

@EnableAutoConfiguration的关键功能是通过@Import注解导入的ImportSelector来完成的。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {}
```

从源代码得知@Import(AutoConfigurationImportSelector.class)是@EnableAutoConfiguration注解的组成部分，也是自动配置功能的核心实现者。@Import(AutoConfigurationImportSelector.class)又可以分为两部分：@Import和对应的ImportSelector。



### @Conditional条件注解



