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



### @SpringBootApplication注解

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

###  **@EnableAutoConfiguration注解**

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

#### @EnableAutoConfiguration 自动配置开关



#### @EnableAutoConfiguration 加载元数据配置

#### @EnableAutoConfiguration 加载自动配置组件

#### @EnableAutoConfiguration 排除指定组件

#### @EnableAutoConfiguration 过滤自动配置组件

#### @EnableAutoConfiguration 事件注册



###  AutoCongfigurationImportSelector源码解析

#### @EnableAutoConfiguration的关键功能是通过@Import注解导入的ImportSelector来完成的。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {}
```

从源代码得知@Import(AutoConfigurationImportSelector.class)是@EnableAutoConfiguration注解的组成部分，也是自动配置功能的核心实现者。@Import(AutoConfigurationImportSelector.class)又可以分为两部分：@Import和对应的ImportSelector。我们先看下这个引入的注解

#### @Import注解

@Import的作用和xml配置中`<import/>`标签的作用一样，我们可以通过@Import引入@Configuration注解的类，也可以导入实现了ImportSelector或ImportBeanDefinitionRegistrar的类，还可以通过@Import导入普通的POJO（将其注册成Spring Bean，导入POJO需要Spring 4.2以上版本） 

*可以在类级别声明或作为元注释声明*。*如需要引入XML或其他类型的文件，使用@ImportResource注解*

```java
package org.springframework.context.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Import {
    // 
    Class<?>[] value();
}

```

> @Import[快速给容器中导入一个组件]
>  *        1）、@Import(要导入到容器中的组件)；容器中就会自动注册这个组件，id默认是全类名
>  *        2）、ImportSelector:返回需要导入的组件的全类名数组；
>  *        3）、ImportBeanDefinitionRegistrar:手动注册bean到容器中

<font color="red">**@import 的许多功能都是需要借助接口ImportSelector实现。ImportSelector 决定可引入哪些@Configuration.**</font>

#### ImportSelector接口

```java
package org.springframework.context.annotation;

import java.util.function.Predicate;
import org.springframework.core.type.AnnotationMetadata;
import org.springframework.lang.Nullable;

public interface ImportSelector {
    String[] selectImports(AnnotationMetadata var1);

    @Nullable
    default Predicate<String> getExclusionFilter() {
        return null;
    }
}
```

ImportSelector 接口提供了一个参数为AnnotationMetadata的方法，返回一个字符串数组。其中参数AnnotationMetadata内包含了被`@Import`注解的类的注解信息。在selectImports方法内可根据具体实现决定返回哪些配置类的全限定名，将结果以字符串数组的形式返回。

如果实现了接口ImportSelector的类的同时又实现了以下4个Aware接口，那么Spring保证在调用ImportSelector之前会先调用Aware接口的方法。这4个接口为：`EnvironmentAware`、`BeanFactoryAware`、`BeanClassLoaderAware`和 `ResourceLoaderAware。`

**ImportSelector接口子接口DeferredImportSelector：**

```java
public interface DeferredImportSelector extends ImportSelector {
    @Nullable
    default Class<? extends DeferredImportSelector.Group> getImportGroup() {
        return null;
    }

    public interface Group {
        void process(AnnotationMetadata var1, DeferredImportSelector var2);

        Iterable<DeferredImportSelector.Group.Entry> selectImports();

        public static class Entry {}
    }
}
```

#### AutoCongfigurationImportSelector

在@EnableAutoConfiguration注解中包含了@Import注解，在@import注解中传入了AutoConfigurationimportSelector类。先看下AutoConfigurationImportSelector类的实现。

```java
public class AutoConfigurationImportSelector implements DeferredImportSelector, BeanClassLoaderAware, ResourceLoaderAware, BeanFactoryAware, EnvironmentAware, Ordered {
    private static final AutoConfigurationImportSelector.AutoConfigurationEntry EMPTY_ENTRY = new AutoConfigurationImportSelector.AutoConfigurationEntry();
    private static final String[] NO_IMPORTS = new String[0];
    private static final Log logger = LogFactory.getLog(AutoConfigurationImportSelector.class);
    private static final String PROPERTY_NAME_AUTOCONFIGURE_EXCLUDE = "spring.autoconfigure.exclude";
    private ConfigurableListableBeanFactory beanFactory;
    private Environment environment;
    private ClassLoader beanClassLoader;
    private ResourceLoader resourceLoader;
    private AutoConfigurationImportSelector.ConfigurationClassFilter configurationClassFilter;

    public AutoConfigurationImportSelector() {
    }
}
```

<font color="red">再看AutoConfigurationImportSelector接口可以看到它实现了一个 DeferredImportSelector 这个接口。而在往DeferredImportSelector接口中进入发现他是ImportSelector的子接口</font>

[![Bym21P.png](https://s1.ax1x.com/2020/11/03/Bym21P.png)](https://imgchr.com/i/Bym21P)

**AutoConfigurationImportSelector并没有直接实现ImportSelector接口，而是实现了它的子接口DeferredImportSelector。DeferredImportSelector接口与ImportSelector的区别是，前者会在所有的@Configuration类加载完成之后再加载返回的配置类，而ImportSelector是在加载完@Configuration类之前先去加载返回的配置类。**

DeferredImportSelector的加载顺序可以通过@Order注解或实现Ordered接口来指定。同时，DeferredImportSelector提供了新的方法getImportGroup()来跨DeferredImportSelector实现自定义Configuration的加载顺序。

##### AutoConfigurationImportSelector类深入解析

[![BcyEND.png](https://s1.ax1x.com/2020/11/04/BcyEND.png)](https://imgchr.com/i/BcyEND)

当AutoConfigurationImportSelector被@Import注解引入之后，它的selectImports方法会被调用并执行其实现的自动装配逻辑。selectImports(AnnotationMetadata annotationMetadata) 方法 几乎涵盖了组件自动装配的所有处理逻辑

```java
@Override
public String[] selectImports(AnnotationMetadata annotationMetadata) {
    // 检查自动配置功能是否开启，默认是开启
    if (!isEnabled(annotationMetadata)) {
        return NO_IMPORTS;
    }
    // 封装将被引入的自动配置信息
    AutoConfigurationEntry autoConfigurationEntry = getAutoConfigurationEntry(annotationMetadata);
    // 返回符合条件的配置类的全限定名数组
    return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
}
```



```java
/**
	 * Return the {@link AutoConfigurationEntry} based on the {@link AnnotationMetadata}
	 * of the importing {@link Configuration @Configuration} class.
	 * @param annotationMetadata the annotation metadata of the configuration class
	 * @return the auto-configurations that should be imported
	 */
protected AutoConfigurationEntry getAutoConfigurationEntry(AnnotationMetadata annotationMetadata) {
    if (!isEnabled(annotationMetadata)) {
        return EMPTY_ENTRY;
    }
    // 获取所有的注解属性同时会合并一些注解
    AnnotationAttributes attributes = getAttributes(annotationMetadata);
    // 通过SpringFactoriesloader类提供的方法加载类路径中MeTA——INF目录下的 Spring.factories文件中针对EnableAutoConfigurationtion的注册配置类
    List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
    // 对获得的注册配置类集合进行去重处理，防止多个项目引入同样的配置类。
    configurations = removeDuplicates(configurations);
    // 获得注解中被exclude或excludeName所排除的类的集合，《《供下面的去除使用》》
    Set<String> exclusions = getExclusions(annotationMetadata, attributes);
    // 检查被排除类是否可实例化，是否被自动注册配置所使用，不符合条件则抛出异常
    checkExcludedClasses(configurations, exclusions);
    // 从自动配置类集合中去除被排除的类
    configurations.removeAll(exclusions);
    // 检查配置类的注解是否符合spring.factories文件中AutoConfigurationImportFilter指定的注解检查条件。
    configurations = getConfigurationClassFilter().filter(configurations);
    // 将筛选完成的配置类和排查的配置类构建事件类，并传入监听器，监听器配置在spring.factories文件中，通过AutoConfigurationImportListener指定
    fireAutoConfigurationImportEvents(configurations, exclusions);
    return new AutoConfigurationEntry(configurations, exclusions);
}
```

### @Conditional条件注解

@Conditional注解是由Spring 4.0版本引入的新特性，可根据是否满足指定的条件来决定是否进行Bean的实例化及装配，比如，设定当类路径下包含某个jar包的时候才会对注解的类进行实例化操作。总之，就是根据一些特定条件来控制Bean实例化的行为，@Conditional注解代码如下。



**综合以上可以得到@SpringBootApplication注解的图解**

[![BcWDV1.png](https://s1.ax1x.com/2020/11/04/BcWDV1.png)](https://imgchr.com/i/BcWDV1)



