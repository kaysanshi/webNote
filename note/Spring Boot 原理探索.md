# Spring Boot 原理篇

## Spring Boot核心运行原理

内容涉及自动配置的运作原理、核心功能模块、核心注解以及使用到的核心源代码分析。主要讲解各个配置是如何加载的，spring的启动的准备的流程。

### 核心运行原理

​		Spring Boot通过@EnableAutoConfiguration注解开启自动配置，加载spring.factories中注册的各种AutoConfiguration类，当某个AutoConfiguration类满足其注解@Conditional指定的生效条件（Starters提供的依赖、配置或Spring容器中是否存在某个Bean等）时，实例化该AutoConfiguration类中定义的Bean（组件等），并注入Spring容器，就可以完成依赖自动配置。

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

​		@SpringBootApplication注是SpringBoot的核心注解，用于开启自动配置，其实是结合内部的@EnableAutoConfiguration注解开启配置

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

[![DJRRFH.png](https://s3.ax1x.com/2020/11/23/DJRRFH.png)](https://imgchr.com/i/DJRRFH)

###  **@EnableAutoConfiguration注解**

​		Spring Boot的核心功能之一就是根据约定自动管理该注解标注的类。用来实现该功能的组件之一便是@EnableAutoConfiguration注解。

​		@EnableAutoConfiguration位于spring-boot-autoconfigure包内，当使用@SpringBootApplication注解时，@EnableAutoConfiguration注解会自动生效。@EnableAutoConfiguration的主要功能是启动Spring应用程序上下文时进行自动配置，它会尝试猜测并配置项目可能需要的Bean。自动配置通常是基于项目classpath中引入的类和已定义的Bean来实现的。在此过程中，被自动配置的组件来自项目自身和项目依赖的jar包中。

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

<font color="green">		@EnableAutoConfiguration会猜测你需要使用的Bean，但如果在实战中你并不需要它预置初始化的Bean，可通过该注解的exclude或excludeName参数进行有针对性的排除。比如，当不需要数据库的自动配置时，可通过以下两种方式让其自动配置失效</font>

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

​		从源代码得知@Import(AutoConfigurationImportSelector.class)是@EnableAutoConfiguration注解的组成部分，也是自动配置功能的核心实现者。@Import(AutoConfigurationImportSelector.class)又可以分为两部分：@Import和对应的ImportSelector。我们先看下这个引入的注解

#### @Import注解

​		@Import的作用和xml配置中`<import/>`标签的作用一样，我们可以通过@Import引入@Configuration注解的类，也可以导入实现了ImportSelector或ImportBeanDefinitionRegistrar的类，还可以通过@Import导入普通的POJO（将其注册成Spring Bean，导入POJO需要Spring 4.2以上版本） 

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

​		ImportSelector 接口提供了一个参数为AnnotationMetadata的方法，返回一个字符串数组。其中参数AnnotationMetadata内包含了被`@Import`注解的类的注解信息。在selectImports方法内可根据具体实现决定返回哪些配置类的全限定名，将结果以字符串数组的形式返回。

​		如果实现了接口ImportSelector的类的同时又实现了以下4个Aware接口，那么Spring保证在调用ImportSelector之前会先调用Aware接口的方法。这4个接口为：`EnvironmentAware`、`BeanFactoryAware`、`BeanClassLoaderAware`和 `ResourceLoaderAware。`

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

​		在@EnableAutoConfiguration注解中包含了@Import注解，在@import注解中传入了AutoConfigurationimportSelector类。先看下AutoConfigurationImportSelector类的实现。

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

<font color="red">		再看AutoConfigurationImportSelector接口可以看到它实现了一个 DeferredImportSelector 这个接口。而在往DeferredImportSelector接口中进入发现他是ImportSelector的子接口</font>

[![Bym21P.png](https://s1.ax1x.com/2020/11/03/Bym21P.png)](https://imgchr.com/i/Bym21P)

​		**AutoConfigurationImportSelector并没有直接实现ImportSelector接口，而是实现了它的子接口DeferredImportSelector。DeferredImportSelector接口与ImportSelector的区别是，前者会在所有的@Configuration类加载完成之后再加载返回的配置类，而ImportSelector是在加载完@Configuration类之前先去加载返回的配置类。**

​		DeferredImportSelector的加载顺序可以通过@Order注解或实现Ordered接口来指定。同时，DeferredImportSelector提供了新的方法getImportGroup()来跨DeferredImportSelector实现自定义Configuration的加载顺序。

##### AutoConfigurationImportSelector类深入解析

[![BcyEND.png](https://s1.ax1x.com/2020/11/04/BcyEND.png)](https://imgchr.com/i/BcyEND)

​		当AutoConfigurationImportSelector被@Import注解引入之后，它的selectImports方法会被调用并执行其实现的自动装配逻辑。selectImports(AnnotationMetadata annotationMetadata) 方法 几乎涵盖了组件自动装配的所有处理逻辑

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

<font color="green">接下来看这个封装的AutoConfigurationEntry对象是如何封装的。</font>

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
    // 加载类路径下metadata配置
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

下面看下全部的过程的代码解析

[![B2X58g.png](https://s1.ax1x.com/2020/11/05/B2X58g.png)](https://imgchr.com/i/B2X58g)

[![BRP4PI.png](https://s1.ax1x.com/2020/11/05/BRP4PI.png)](https://imgchr.com/i/BRP4PI)

[![BRPIRP.png](https://s1.ax1x.com/2020/11/05/BRPIRP.png)](https://imgchr.com/i/BRPIRP)

[![BRPXIs.png](https://s1.ax1x.com/2020/11/05/BRPXIs.png)](https://imgchr.com/i/BRPXIs)



<font color="blue">**看过AutoConfigurationImportSelector源码后我们继续看@EnableAutoConfiguration注解**</font>

#### @EnableAutoConfiguration 自动配置开关

​		位于selectImports方法中第一段代码进行判断了是否开启自动配置功能，如果开启了自动配置就进行后续功能；如果未开启就返回空数组。

```java
public String[] selectImports(AnnotationMetadata annotationMetadata) {
		if (!isEnabled(annotationMetadata)) {
			return NO_IMPORTS;
		}
    ...
        ...
}
// 在EnableAutoConfiguration的类中 有配置 String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";
protected boolean isEnabled(AnnotationMetadata metadata) {
		if (getClass() == AutoConfigurationImportSelector.class) {
			return getEnvironment().getProperty(EnableAutoConfiguration.ENABLED_OVERRIDE_PROPERTY, Boolean.class, true);
		}
		return true;
}
```

​		通过isEnabled方法可以看出，如果当前类为AutoConfigurationImportSelector，程序会从环境中获取key为EnableAutoConfiguration.ENABLED_OVERRIDE_PROPERTY的配置，该常量的值为spring.boot.enableautoconfiguration。如果获取不到该属性的配置，isEnabled默认为true，也就是默认会使用自动配置。如果当前类为其他类，直接返回true。

如果我们想覆盖或重置EnableAutoConfiguration.ENABLED_OVERRIDE_PROPERTY的配置可以在application.propertie或application.yml进行配置。<font color="blue">比如关闭自动配置：</font>

```properties
spring.boot.enableautoconfiguration=false
```

#### @EnableAutoConfiguration 加载元数据配置



#### @EnableAutoConfiguration 加载自动配置组件

加载自动配置组件是自动配置的核心组件之一，这些自动配置组件在类路径中META-INF目录下的spring.factories文件中进行注册。Spring Boot预置了一部分常用组件，如果我们需要创建自己的组件，可参考Spring Boot预置组件在自己的Starters中进行配置通过Spring Core提供的SpringFactoriesLoader类可以读取spring.factories文件中注册的类。下面我们通过源代码来看一下如何在AutoConfigurationImportSelector类中通过getCandidateConfigurations方法来读取spring.factories文件中注册的类

```java
protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
		List<String> configurations = SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(),
				getBeanClassLoader());
		Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you "
				+ "are using a custom packaging, make sure that file is correct.");
		return configurations;
	}
```

在上面的getCandidateConfigurations方法中使用的是SpringFactoriesLoader类提供的loadFactoryNames方法读取 META-INF/spring.factories 中的配置。如果程序未读取到任何配置内容，会抛出异常信息。而loadFactoryNames方法的第一个参数为getSpringFactoriesLoaderFactoryClass方法返回的EnableAutoConfiguration.class，也就是说loadFactoryNames只会读取配置文件中针对自动配置的注册类

##### SpringFactoriesLoader类

SpringFactoriesLoader 工厂加载机制是 Spring 内部提供的一个约定俗成的加载方式，与 java spi 类似，只需要在模块的 META-INF/spring.factories 文件中，以 Properties 类型(即 key-value 形式)配置，就可以将相应的实现类注入 Spirng 容器中。**这个也就就是为了引入所需要的类，比如这个类对其他包下的依赖类**。

```java
public final class SpringFactoriesLoader {
    // 类加载文件的路径，可能存在多个
	public static final String FACTORIES_RESOURCE_LOCATION = "META-INF/spring.factories";
    private static final Log logger = LogFactory.getLog(SpringFactoriesLoader.class);
    private static final Map<ClassLoader, MultiValueMap<String, String>> cache = new ConcurrentReferenceHashMap();

    private SpringFactoriesLoader() {
    }
}    
```

###### loadFactories方法与loadFactoryNames方法

loadFactories方法：读取 classpath 上所有的jar包中的所有 META-INF/spring.factories 属性文件，找出其中定义的匹配类型 factoryClass 的工厂类，然后创建每个工厂类的对象/实例，并返回这些工厂类对象/实例的列表

```java
 public static <T> List<T> loadFactories(Class<T> factoryType, @Nullable ClassLoader classLoader) {
        Assert.notNull(factoryType, "'factoryType' must not be null");
    	 // 如果未指定类加载器，则使用默认的
        ClassLoader classLoaderToUse = classLoader;
        if (classLoader == null) {
            classLoaderToUse = SpringFactoriesLoader.class.getClassLoader();
        }
		// 加载加载所有的META-INF/spring.factories文件封装成map并从中获取指定类名列表 
        List<String> factoryImplementationNames = loadFactoryNames(factoryType, classLoaderToUse);
     	// 如果记录器Trace跟踪激活的话,将工厂名称列表输出
        if (logger.isTraceEnabled()) {
            logger.trace("Loaded [" + factoryType.getName() + "] names: " + factoryImplementationNames);
        }
		// 创建结果集
        List<T> result = new ArrayList(factoryImplementationNames.size());
        Iterator var5 = factoryImplementationNames.iterator();

        while(var5.hasNext()) {
            String factoryImplementationName = (String)var5.next();
            // 实例化工厂类,并添加到结果集中
            result.add(instantiateFactory(factoryImplementationName, factoryType, classLoaderToUse));
        }
		// 对结果进行排序
        AnnotationAwareOrderComparator.sort(result);
        return result;
    }
```

SpringFactoriesLoader加载器加载指定ClassLoader下面的所有META-INF/spring.factories文件，并将文件解析内容存于Map<String,List<String>>内。然后，通过loadFactoryNames传递过来的class的名称从Map中获得该类的配置列表

具体流程如下图所示：

[![DJTze1.png](https://s3.ax1x.com/2020/11/23/DJTze1.png)](https://imgchr.com/i/DJTze1)

loadFactoryNames方法：读取 classpath上 所有的 jar 包中的所有 META-INF/spring.factories属 性文件，找出其中定义的匹配类型 factoryClass 的工厂类，然后并返回这些工厂类的名字列表，注意是包含包名的全限定名

```java
 // 加载加载所有的META-INF/spring.factories文件封装成map并从中获取指定类名列表 
public static List<String> loadFactoryNames(Class<?> factoryType, @Nullable ClassLoader classLoader) {
       // 获取包含包名的工厂类名称
        String factoryTypeName = factoryType.getName();
     	 // 获取所有配置在 META-INF/spring.factories 文件的值
  		// 然后获取指定类的实现类名列表
        return (List)loadSpringFactories(classLoader).getOrDefault(factoryTypeName, Collections.emptyList());
    }
	// 加载所有的META-INF/spring.factories文件封装成map并从中获取指定类名列表 key为接口的全类名，Value为对应配置值的List集合
    private static Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader) {
        // 判断是否有缓存结果，如果有直接返回
        MultiValueMap<String, String> result = (MultiValueMap)cache.get(classLoader);
        if (result != null) {
            return result;
        } else {
            try {
                // 扫描 classpath 上所有 JAR 中的文件 META-INF/spring.factories
                Enumeration<URL> urls = classLoader != null ? classLoader.getResources("META-INF/spring.factories") : ClassLoader.getSystemResources("META-INF/spring.factories");
                LinkedMultiValueMap result = new LinkedMultiValueMap();

                while(urls.hasMoreElements()) {
                    // 找到的每个 META-INF/spring.factories 文件都是一个 Properties 文件，将其内容加载到一个 Properties 对象然后处理其中的每个属性
                    URL url = (URL)urls.nextElement();
                    // 根据路径创建一个UrlReaource对象
                    UrlResource resource = new UrlResource(url);
                    // 通过PropertiesLoaderUtil获取.properties文件
                    Properties properties = PropertiesLoaderUtils.loadProperties(resource);
                    // Properties是一个hash表 是hashtable的子类
                    Iterator var6 = properties.entrySet().iterator();
                    while(var6.hasNext()) {
                        // 获取工厂类名称（接口或者抽象类的全限定名）
                        Entry<?, ?> entry = (Entry)var6.next();
                        String factoryTypeName = ((String)entry.getKey()).trim();
                        // 将其转为数组
                        String[] var9 = StringUtils.commaDelimitedListToStringArray((String)entry.getValue());
                        int var10 = var9.length;
						// 将逗号分割的属性值逐个取出，然后放到 结果result 中去
                        for(int var11 = 0; var11 < var10; ++var11) {
                            String factoryImplementationName = var9[var11];
                            result.add(factoryTypeName, factoryImplementationName.trim());
                        }
                    }
                }
				// 将结果存放到缓存中
                cache.put(classLoader, result);
                return result;
            } catch (IOException var13) {
                throw new IllegalArgumentException("Unable to load factories from location [META-INF/spring.factories]", var13);
            }
        }
    }
```



#### @EnableAutoConfiguration 排除指定组件

​			AutoConfigurationImportSelector中通过调用getExclusions方法来获取被排除类的集合。它会收集@EnableAutoConfiguration注解中配置的exclude属性值、excludeName属性值，并通过方法getExcludeAutoConfigurationsProperty获取在配置文件中key为spring.autoconfigure.exclude的配置值。以排除自动配置DataSourceAutoConfiguration为例，配置文件中的配置形式如下。<font color="blue" >比如在application.properties进行配置</font>

```properties
spring.autoconfigure.exclude=org.springframework.boot.autoconfiguration.jdbc.DataSourceAutoConfiguration
```

#### @EnableAutoConfiguration 过滤自动配置组件

​		当完成初步的自动配置组件排除工作之后，AutoConfigurationImportSelector会结合在此之前获取的AutoConfigurationMetadata对象，对组件进行再次过滤

#### @EnableAutoConfiguration 事件注册

​		通过SpringFactoriesLoader类提供的loadFactories方法将spring.factories中配置的接口AutoConfigurationImportListener的实现类加载出来。然后，将筛选出的自动配置类集合和被排除的自动配置类集合封装成AutoConfigurationImportEvent事件对象，并传入该事件对象通过监听器提供的onAutoConfigurationImportEvent方法，最后进行事件广播

spring.factories中自动配置监听器相关配置代码如下:

```properties
# Auto Configuration Import Listeners
org.springframework.boot.autoconfigure.AutoConfigurationImportListener=\
org.springframework.boot.autoconfigure.condition.ConditionEvaluationReportAutoConfigurationImportListener

```

### @Conditional条件注解

​		前面自动配置类的读取和筛选，在这个过程中已经涉及了像@Conditional-OnClass这样的条件注解。打开每一个自动配置类，都会看到@Conditional或其衍生的条件注解。下面就先认识一下@Conditional注解。

​		@Conditional注解是由Spring 4.0版本引入的新特性，可根据是否满足指定的条件来决定是否进行Bean的实例化及装配，比如，设定当类路径下包含某个jar包的时候才会对注解的类进行实例化操作。总之，就是根据一些特定条件来控制Bean实例化的行为，@Conditional注解代码如下。

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Conditional {

	/**
	 * All {@link Condition} classes that must {@linkplain Condition#matches match}
	 * in order for the component to be registered.
	 */
	Class<? extends Condition>[] value();

}
// 注解的唯一元素就是Condition的数组，只有在数组指定的所有的Condition的matches方法都返回true的时候被注解的类才会被加载。
@FunctionalInterface
public interface Condition {
	// 决定条件是否匹配
	boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata);

}
```

matches方法的第一个参数为ConditionContext,可通过该接口提供的方法来获取得到spring应用的上下文信息。第二个参数 AnnotatedTypeMetadata 该接口提供了访问特定类或方法的注解功能，并且不需要加载类，可以用来检查带有@Bean注解的方法上是否还有其他注解。

#### ConditionContext接口

matches方法的第一个参数为ConditionContext，可通过该接口提供的方法来获得Spring应用的上下文信息

```java
public interface ConditionContext {

	// 返回BeanDefintionRegistry注册表，可以检查Bean的定义
	BeanDefinitionRegistry getRegistry();

	// 返回ConfigurableBeanFactory,可以检查Bean是否已经存在，进一步检查Bean
	@Nullable
	ConfigurableListableBeanFactory getBeanFactory();
    
	// 返回Environment 可获取当前应用环境的变量。检查当前环境变量是否存在
	Environment getEnvironment();

	// 返回ResourceLoader 用于读取或检查所加载的资源。
	ResourceLoader getResourceLoader();

	// 返回classLoader 用于检查类是否存在。
	@Nullable
	ClassLoader getClassLoader();

}
```

#### AnnotatedTypeMetadata接口

该接口提供了访问特定类或方法的注解功能，并且不需要加载类，可以用来检查带有@Bean注解的方法上是否还有其他注解，AnnotatedTypeMetadata接口定义如下

```java
public interface AnnotatedTypeMetadata {
    MergedAnnotations getAnnotations();
	// 判断带有@Bean的注解的方法是否还有其他注解的功能
    default boolean isAnnotated(String annotationName) {
        return this.getAnnotations().isPresent(annotationName);
    }

    @Nullable
    default Map<String, Object> getAnnotationAttributes(String annotationName) {
        return this.getAnnotationAttributes(annotationName, false);
    }

    @Nullable
    default Map<String, Object> getAnnotationAttributes(String annotationName, boolean classValuesAsString) {
        MergedAnnotation<Annotation> annotation = this.getAnnotations().get(annotationName, (Predicate)null, MergedAnnotationSelectors.firstDirectlyDeclared());
        return !annotation.isPresent() ? null : annotation.asAnnotationAttributes(Adapt.values(classValuesAsString, true));
    }

    @Nullable
    default MultiValueMap<String, Object> getAllAnnotationAttributes(String annotationName) {
        return this.getAllAnnotationAttributes(annotationName, false);
    }

    @Nullable
    default MultiValueMap<String, Object> getAllAnnotationAttributes(String annotationName, boolean classValuesAsString) {
        Adapt[] adaptations = Adapt.values(classValuesAsString, true);
        return (MultiValueMap)this.getAnnotations().stream(annotationName).filter(MergedAnnotationPredicates.unique(MergedAnnotation::getMetaTypes)).map(MergedAnnotation::withNonMergedAttributes).collect(MergedAnnotationCollectors.toMultiValueMap((map) -> {
            return map.isEmpty() ? null : map;
        }, adaptations));
    }
}
```

#### 条件注解衍生注解

在Spring Boot的autoconfigure项目中提供了各类基于@Conditional注解的衍生注解，它们适用不同的场景并提供了不同的功能。以下相关注解均位于spring-boot-autoconfigure项目的org.springframework.boot.autoconfigure.condition包下。

·@ConditionalOnBean：在容器中有指定Bean的条件下。

·@ConditionalOnClass：在classpath类路径下有指定类的条件下。

·@ConditionalOnCloudPlatform：当指定的云平台处于active状态时。

·@ConditionalOnExpression：基于SpEL表达式的条件判断。

·@ConditionalOnJava：基于JVM版本作为判断条件。

·@ConditionalOnJndi：在JNDI存在的条件下查找指定的位置。

·@ConditionalOnMissingBean：当容器里没有指定Bean的条件时。

·@ConditionalOnMissingClass：当类路径下没有指定类的条件时。

·@ConditionalOnNotWebApplication：在项目不是一个Web项目的条件下。

·@ConditionalOnProperty：在指定的属性有指定值的条件下。

·@ConditionalOnResource：类路径是否有指定的值

·@ConditionalOnSingleCandidate：当指定的Bean在容器中只有一个或者有多个但是指定了首选的Bean时。

·@ConditionalOnWebApplication：在项目是一个Web项目的条件下。如果仔细观察这些注解的源码，你会发现它们其实都组合了@Conditional注解，不同之处是它们在注解中指定的条件（Condition）不同。下面我们以@ConditionalOnWebApplication为例来对衍生条件注解进行一个简单的分析

condition接口相关的功能及实现类

[![B7jJTU.png](https://s1.ax1x.com/2020/11/09/B7jJTU.png)](https://imgchr.com/i/B7jJTU)

在抽象类SpringBootCondition中实现了matches方法，而该方法中最核心的部分是通过调用新定义的抽象方法getMatchOutcome并交由子类来实现，在matches方法中根据子类返回的结果判断是否匹配

```java
public abstract class SpringBootCondition implements Condition {

	private final Log logger = LogFactory.getLog(getClass());

	@Override
	public final boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
		String classOrMethodName = getClassOrMethodName(metadata);
		try {
			ConditionOutcome outcome = getMatchOutcome(context, metadata);
			logOutcome(classOrMethodName, outcome);
			recordEvaluation(context, classOrMethodName, outcome);
			return outcome.isMatch();
		}
		catch (NoClassDefFoundError ex) {
			throw new IllegalStateException("Could not evaluate condition on " + classOrMethodName + " due to "
					+ ex.getMessage() + " not found. Make sure your own configuration does not rely on "
					+ "that class. This can also happen if you are "
					+ "@ComponentScanning a springframework package (e.g. if you "
					+ "put a @ComponentScan in the default package by mistake)", ex);
		}
		catch (RuntimeException ex) {
			throw new IllegalStateException("Error processing condition on " + getName(metadata), ex);
		}
	}
}
```



**综合以上可以得到@SpringBootApplication注解的图解**

[![BcWDV1.png](https://s1.ax1x.com/2020/11/04/BcWDV1.png)](https://imgchr.com/i/BcWDV1)



## SpringBoot构造流程分析

主要围绕的是SpringApplication这个类的静态方法run()进行初始化类的SpringApplication的自身的说明与进入。**在此过程中完成了基本配置文件的加载和实例化。当SpringApplication对象被创建之后，通过调用其run方法来进行Spring Boot的启动和运行，至此正式开启了SpringApplication的生命周期**

### SpringApplication类

在入口类中主要通过SpringApplication的静态方法——run方法进行SpringApplication类的实例化操作，然后再针对实例化对象调用另外一个run方法来完成整个项目的初始化和启动。可以看到SpringApplication的实例化只是在它提供的静态run方法中新建了一个SpringApplication对象。其中参数primarySources为加载的主要资源类，通常就是Spring Boot的入口类，args为传递给应用程序的参数信息

#### SpringApplication实例化流程（就是构造函数初始化流程阶段深入分析）

```java
/**
** 
**/
public SpringApplication(Class<?>... primarySources) {
   this((ResourceLoader)null, primarySources);
}

/**
** 主要做了一下几个功能：参数赋值给成员变量、应用类型及方法推断和ApplicationContext相关内容加载及实例化
**/
public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
        this.resourceLoader = resourceLoader;
        Assert.notNull(primarySources, "PrimarySources must not be null");
        this.primarySources = new LinkedHashSet(Arrays.asList(primarySources));
    	// 推断web应用类型
        this.webApplicationType = WebApplicationType.deduceFromClasspath();
  		 // 加载并初始化ApplicationContextInitializer及其相关类
		setInitializers((Collection)getSpringFactoriesInstances(ApplicationContextInitializer.class));
   		// 加载并初始化ApplicationListener及相关实现类     			
		setListeners((Collection)getSpringFactoriesInstances(ApplicationListener.class));    
    	// 推断main方法Class类
        this.mainApplicationClass = this.deduceMainApplicationClass();
    }
```

**通过源码的查看可得到springApplication的实例化（构造函数初始化）的核心流程**

[![BHPmzF.png](https://s1.ax1x.com/2020/11/09/BHPmzF.png)](https://imgchr.com/i/BHPmzF)

**下面根据上面的流程图我们将深入的进行对每个阶段进行深入学习**

##### 1.Web类型的推断WebAppliationType

在实例化流程中有这样的一段代码是对web应用类型的推断。

`this.webApplicationType = WebApplicationType.deduceFromClasspath();`

该行代码调用了WebApplicationType的deduceFromClasspath方法，并将获得的Web应用类型赋值给私有成员变量webApplicationType。

WebApplicationType为枚举类，它定义了可能的Web应用类型，该枚举类提供了三类定义：枚举类型、推断类型的方法和用于推断的常量。枚举类型包括非Web应用、基于SERVLET的Web应用和基于REACTIVE的Web应用

```java
public enum WebApplicationType {
    NONE, // 非web应用类型
    SERVLET, // 基于servlet的web应用类型
    REACTIVE; // 基于reactive的web应用类型
}
```

方法deduceFromClasspath是基于classpath中类是否存在来进行类型推断的，就是判断指定的类是否存在于classpath下，并根据判断的结果来进行组合推断该应用属于什么类型。

deduceFromClasspath在判断的过程中用到了ClassUtils的isPresent方法。isPresent方法的核心机制就是通过反射创建指定的类，根据在创建过程中是否抛出异常来判断该类是否存在

```java
/**
**	deduceFromClasspath是基于classpath中类是否存在来进行类型推断的
**/
static WebApplicationType deduceFromClasspath() {
        if (ClassUtils.isPresent("org.springframework.web.reactive.DispatcherHandler", (ClassLoader)null) && !ClassUtils.isPresent("org.springframework.web.servlet.DispatcherServlet", (ClassLoader)null) && !ClassUtils.isPresent("org.glassfish.jersey.servlet.ServletContainer", (ClassLoader)null)) {
            return REACTIVE;
        } else {
            String[] var0 = SERVLET_INDICATOR_CLASSES;
            int var1 = var0.length;

            for(int var2 = 0; var2 < var1; ++var2) {
                String className = var0[var2];
                if (!ClassUtils.isPresent(className, (ClassLoader)null)) {
                    return NONE;
                }
            }

            return SERVLET;
        }
    }
```

通过源代码`deduceFromClasspath()`可以看出执行流程

- 当DispatcherHandler存在，并且DispatcherServlet和ServletContainer都不存在，则返回类型为WebApplicationType.REACTIVE。
- 当SERVLET或ConfigurableWebApplicationContext任何一个不存在时，说明当前应用为非Web应用，返回WebApplicationType.NONE。
- 当应用不为REACTIVE Web应用，并且SERVLET和ConfigurableWebApplicationContext都存在的情况下，则为SERVLET的Web应用，返回WebApplicationType.SERVLET。

##### 2.ApplicationContextInitializer加载

​	ApplicationContextInitializer是Spring IOC容器提供的一个接口，它是一个回调接口，主要目的是允许用户在ConfigurableApplicationContext类型（或其子类型）的ApplicationContext做refresh方法调用刷新之前，对ConfigurableApplicationContext实例做进一步的设置或处理。通常用于应用程序上下文进行编程初始化的Web应用程序中

```java
public interface ApplicationContextInitializer<C extends ConfigurableApplicationContext> {

	/**
	 * Initialize the given application context.
	 * @param applicationContext the application to configure
	 */
	void initialize(C applicationContext);

}
```

ApplicationContextInitializer接口只定义了一个initialize方法主要是为了初始化指定的应用上下文。而对应的上下文由参数传入，参数为ConfigurableApplicationContext的子类。

在完成了Web应用类型推断之后，ApplicationContextInitializer便开始进行加载工作，该过程可分两步骤：获得相关实例和设置实例。对应的方法分别为getSpringFactoriesInstances和setInitializers。

###### **getSpringFactoriesInstances方法**

```java
private <T> Collection<T> getSpringFactoriesInstances(Class<T> type) {
		return getSpringFactoriesInstances(type, new Class<?>[] {});
	}

private <T> Collection<T> getSpringFactoriesInstances(Class<T> type, Class<?>[] parameterTypes, Object... args) {
		ClassLoader classLoader = getClassLoader();
		// 加载对应配置，这里采用LinkedHashSet和名称来确保加载的唯一性。
		Set<String> names = new LinkedHashSet<>(SpringFactoriesLoader.loadFactoryNames(type, classLoader));
    	// 创建实例
		List<T> instances = createSpringFactoriesInstances(type, parameterTypes, classLoader, args, names);
    	// 排序操作
		AnnotationAwareOrderComparator.sort(instances);
		return instances;
	}
```

getSpringFactoriesInstances方法依然是通过SpringFactoriesLoader类的loadFactoryNames方法来获得META-INF/spring.factories文件中注册的对应配置

**createSpringFactoriesInstances方法**

```java
private <T> List<T> createSpringFactoriesInstances(Class<T> type, Class<?>[] parameterTypes,
			ClassLoader classLoader, Object[] args, Set<String> names) {
		List<T> instances = new ArrayList<>(names.size());
    // 遍历加载到类名（全限定名）
		for (String name : names) {
			try {
                // 获取class
				Class<?> instanceClass = ClassUtils.forName(name, classLoader);
				Assert.isAssignable(type, instanceClass);
                // 获取有参构造器
				Constructor<?> constructor = instanceClass.getDeclaredConstructor(parameterTypes);
                // 创建对象
				T instance = (T) BeanUtils.instantiateClass(constructor, args);
				instances.add(instance);
			}
			catch (Throwable ex) {
				throw new IllegalArgumentException("Cannot instantiate " + type + " : " + name, ex);
			}
		}
		return instances;
	}
```

###### **setInitializers**

完成获取配置类集合和实例化操作之后，调用setInitializers方法将实例化的集合添加到SpringApplication的成员变量initializers中，类型为List<ApplicationContextInitiali-zer<?>>，代码如下

```java
public void setInitializers(Collection<? extends ApplicationContextInitializer<?>> initializers) {
	this.initializers = new ArrayList<>(initializers);
}
```

setInitializers方法将接收到的initializers作为参数创建了一个新的List，并将其赋值给SpringApplication的initializers成员变量。由于是创建了新的List，并且直接赋值，因此该方法一旦被调用，便会导致数据覆盖，使用时需注意

****

##### 3.ApplicationListener加载

​       完成了ApplicationContextInitializer的加载之后，便会进行ApplicationListener的加载。它的常见应用场景为：当容器初始化完成之后，需要处理一些如数据的加载、初始化缓存、特定任务的注册等操作。而在此阶段，更多的是用于ApplicationContext管理Bean过程的场景。

​       Spring事件传播机制是基于观察者模式（Observer）实现的。比如，在ApplicationContext管理Bean生命周期的过程中，会将一些改变定义为事件（ApplicationEvent）。ApplicationContext通过ApplicationListener监听ApplicationEvent，当事件被发布之后，ApplicationListener用来对事件做出具体的操作。

​        ApplicationListener的整个配置和加载流程与ApplicationContextInitializer完全一致，也是先通过SpringFactoriesLoader的loadFactoryNames方法获得META-INF/spring.factories中对应配置，然后再进行实例化，最后将获得的结果集合添加到SpringApplication的成员变量listeners中，

```java
public void setListeners(Collection<? extends ApplicationListener<?>> listeners) {
		this.listeners = new ArrayList<>(listeners);
}
```

在调用setListeners方法时也会进行覆盖赋值的操作，之前加载的内容会被清除

ApplicationListener接口和ApplicationEvent类配合使用，可实现ApplicationContext的事件处理。如果容器中存在ApplicationListener的Bean，当ApplicationContext调用publishEvent方法时，对应的Bean会被触发。这就是观察者模式的实现

```java
@FunctionalInterface
public interface ApplicationListener<E extends ApplicationEvent> extends EventListener {

	/**
	 * Handle an application event.
	 * @param event the event to respond to
	 */
	void onApplicationEvent(E event);

}
```

onApplicationEvent方法一般用于处理应用程序事件，参数event为ApplicationEvent的子类，是具体响应（接收到）的事件。当ApplicationContext被初始化或刷新时，会触发ContextRefreshedEvent事件。

##### 4.入口类的推断

创建SpringApplication的最后一步便是推断入口类，我们通过调用自身的deduceMainApplicationClass方法来进行入口类的推断。这个方法或进行获取栈数组，遍历栈数组，判断类方法是否包含main方法。第一个被匹配的类会通过Class.forName方法创建对象，并将其返回，最后在上层方法中将对象赋值给SpringApplication的成员变量mainApplicationClass。在遍历过程中如果发生异常，会忽略掉该异常并继续执行遍历操作。

```java
private Class<?> deduceMainApplicationClass() {
		try {
            // 获取栈元素数组
			StackTraceElement[] stackTrace = new RuntimeException().getStackTrace();
            // 遍历栈元素数组
			for (StackTraceElement stackTraceElement : stackTrace) {
                // 匹配第一个main方法并返回
				if ("main".equals(stackTraceElement.getMethodName())) {
					return Class.forName(stackTraceElement.getClassName());
				}
			}
		}
		catch (ClassNotFoundException ex) {
			// Swallow and continue
		}
		return null;
	}
```



#### SpringApplication构造方法参数

由上面可以看到SpringApplicaiton的构造函数的参数有 `ResourceLoader resourceLoader, Class<?>... primarySources`

`ResourceLoader`为资源加载的接口，在Spring Boot启动时打印对应的banner信息，默认采用的就是 DefaultResourceLoader。实战过程中，如果程序未按照Spring Boot的 “约定” 将banner的内容放置于classpath下，或者文件名不是 `banner.*` 格式，默认资源加载器是无法加载到对应的banner信息的，此时可通过 ResourceLoader 来指定需要加载的文件路径。

第二个参数`Class<?>...primarySources`，为可变参数，默认传入Spring Boot入口类。如果作为项目的引导类，此参数传入的类需要满足一个条件，就是被注解`@EnableAutoConfiguration`或其组合注解标注。由于`@SpringBootApplication`注解中包含了`@EnableAutoConfiguration`注解，因此被`@SpringBootApplication`注解标注的类也可作为参数传入。当然，该参数也可传入其他普通类。但只有传入被`@EnableAutoConfiguration`标注的类才能够开启Spring Boot的自动配置。



## SpringBoot运行流程分析

主要围绕了SpringApplaictionRunlisteners, ApplicationArguments,ConfigurableEnvironment以及应用上下文的部分信息。

#### **SpringApplication中的run方法（run方法运行流程分析）**

```java
public ConfigurableApplicationContext run(String... args) {
    	// 创建StopWatch对象，用于统计run方法启动时长
        StopWatch stopWatch = new StopWatch();
    	// 启动统计
        stopWatch.start();
        ConfigurableApplicationContext context = null;
        Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList();
    	// 配置handless属性
        this.configureHeadlessProperty();
    	// 获得SpringApplicationRunLister数组，该数组封装于SpringApplicationRunListeners中的listeners中
        SpringApplicationRunListeners listeners = this.getRunListeners(args);
    	// 启动监听，遍历SpringApplicationRunLister数组，并执行
        listeners.starting();

        Collection exceptionReporters;
        try {
            // 创建ApplicationArguments对象
            ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
            // 加载属性配置，包括所有的属性配置，如application.properties中和外部属性
            ConfigurableEnvironment environment = this.prepareEnvironment(listeners, applicationArguments);
            this.configureIgnoreBeanInfo(environment);
            // 打印 banner
            Banner printedBanner = this.printBanner(environment);
            // 创建容器
            context = this.createApplicationContext();
            // 异常报告器
            exceptionReporters = this.getSpringFactoriesInstances(SpringBootExceptionReporter.class, new Class[]{ConfigurableApplicationContext.class}, context);
            // 准备容器，组件对象间的关系
             //1.将environment保存到容器中
            // 2.触发监听事件——调用每个SpringApplicationRunListeners的contextPrepared方法
            // 3.调用ConfigurableListableBeanFactory的registerSingleton方法向容器中注入applicationArguments与printedBanner
            // 4.触发监听事件——调用每个SpringApplicationRunListeners的contextLoaded方法
            this.prepareContext(context, environment, listeners, applicationArguments, printedBanner);
            // 初始化容器
            this.refreshContext(context);
            // 初始化操作后的统计，默认实现为空
            this.afterRefresh(context, applicationArguments);
            // 停止计数器
            stopWatch.stop();
            // 打印启动日志
            if (this.logStartupInfo) {
                (new StartupInfoLogger(this.mainApplicationClass)).logStarted(this.getApplicationLog(), stopWatch);
            }
			// 通知监听器，启动完成
            listeners.started(context);
            // 调用ApplicationRunner和CommandLineRunner的运行方法
            this.callRunners(context, applicationArguments);
        } catch (Throwable var10) {
            // 异常处理
            this.handleRunFailure(context, var10, exceptionReporters, listeners);
            throw new IllegalStateException(var10);
        }

        try {
            // 通知监听器：容器正在运行
            listeners.running(context);
            return context;
        } catch (Throwable var9) {
            // 异常处理
            this.handleRunFailure(context, var9, exceptionReporters, (SpringApplicationRunListeners)null);
            throw new IllegalStateException(var9);
        }
    }
```

**从上面的源代码我们忽略Spring Boot计时和统计的功能可以将其转化为下面的流程图表示。** spring run方法运行流程

[![DYP6nx.png](https://s3.ax1x.com/2020/11/23/DYP6nx.png)](https://imgchr.com/i/DYP6nx)

下面将对其具体的过程进行深入了解

####  1.获取SpringApplicationRunListener监听器并进行监听

通过this.getRunListeners(args)；方法 我们可以得到SpringApplicationRunListeners对象。

```java
// 获得SpringApplicationRunLister数组，该数组封装于SpringApplicationRunListeners中的listeners中
SpringApplicationRunListeners listeners = this.getRunListeners(args);
```

在this.getRunListeners(args) 方法中可以看到

##### getRunListeners(args)方法：

```java
	private SpringApplicationRunListeners getRunListeners(String[] args) {
		// 构造CLass数组
		Class<?>[] types = new Class<?>[] { SpringApplication.class, String[].class };
        // 调用SpringApplicationRunListeners构造方法，第二个参数是一个集合
		return new SpringApplicationRunListeners(logger,
				getSpringFactoriesInstances(SpringApplicationRunListener.class, types, this, args));
	}
	// 
	private <T> Collection<T> getSpringFactoriesInstances(Class<T> type) {
		return getSpringFactoriesInstances(type, new Class<?>[] {});
	}
	// 获取集合 获取到factories配置文件中的注册类，并进行实例化操作
	private <T> Collection<T> getSpringFactoriesInstances(Class<T> type, Class<?>[] parameterTypes, Object... args) {
        // 通过获取类加载器
		ClassLoader classLoader = getClassLoader();
		// Use names and ensure unique to protect against duplicates
        // 加载META-INF/spring.factories中对应监听器的配置，并将结果存放于Set中（去重）
		Set<String> names = new LinkedHashSet<>(SpringFactoriesLoader.loadFactoryNames(type, classLoader));
        // 实例化监听器
		List<T> instances = createSpringFactoriesInstances(type, parameterTypes, classLoader, args, names);
        // 排序
		AnnotationAwareOrderComparator.sort(instances);
		return instances;
	}
```

##### SpringApplicationRunListener源码解析

接口SpringApplicationRunListener是SpringApplication的run方法监听器。上节提到了SpringApplicationRunListener通过SpringFactoriesLoader加载，并且必须声明一个公共构造函数，该函数接收SpringApplication实例和String[ ]的参数，而且每次运行都会创建一个新的实例。

SpringApplicationRunListener提供了一系列的方法，用户可以通过回调这些方法，在启动各个流程时加入指定的逻辑处理

```java
public interface SpringApplicationRunListener {
	// 当run方法第一次被执行时会被立即调用，可用于非常早期的初始化工作
	default void starting() {
	}
	// 当environment准备完成后，在ApplicationContext创建之前，该方法被调用
	default void environmentPrepared(ConfigurableEnvironment environment) {
	}
	// 当ApplicationContext构建完成，资源未被加载时该方法被调用
	default void contextPrepared(ConfigurableApplicationContext context) {
	}
	// 当ApplicationContext加载完成，未被刷新之前，该方法被调用
	default void contextLoaded(ConfigurableApplicationContext context) {
	}
	// 当ApplicationContext刷新并启动后，CommandLineRunner和ApplicationRunner未被调用之前，该方法被调用
	default void started(ConfigurableApplicationContext context) {
	}
	// 当所有准备工作就绪，run方法执行完成之前，方法被调用
	default void running(ConfigurableApplicationContext context) {
	}
	// 当程序出现错误时方法被调用
	default void failed(ConfigurableApplicationContext context, Throwable exception) {
	}
}
```

SpringApplicationRunListener作为一个监听器它在spring启动run()方法时被调用的过程如下图。

[![DYP7ut.png](https://s3.ax1x.com/2020/11/23/DYP7ut.png)](https://imgchr.com/i/DYP7ut)



##### SpringApplicationRunListener实现类EventPublishingRunListener

![](https://s3.ax1x.com/2020/11/23/DYiart.png)

**从类图可以看到EventPublishRunListener类是SpringApplicationRunListener接口的唯一的内建的实现类。EventPublishingRunListener使用内置的SimpleApplicationEventMulticaster来广播在上下文刷新之前触发的事件。默认情况下，Spring Boot在初始化过程中触发的事件也是交由EventPublishingRunListener来代理实现的**

**构造方法**

```java
public class EventPublishingRunListener implements SpringApplicationRunListener, Ordered {

	private final SpringApplication application; // 类型为springApplication

	private final String[] args;// 启动程序时的命令参数。

	private final SimpleApplicationEventMulticaster initialMulticaster; // 事件广播器
	// 有参构造函数
	public EventPublishingRunListener(SpringApplication application, String[] args) {
        // 
		this.application = application;
		this.args = args;
        // 创建SimpleApplicationEventMulticaster 广播器
		this.initialMulticaster = new SimpleApplicationEventMulticaster();
        // 遍历applicationListener并关联SimpleApplicationEventMulticaster
		for (ApplicationListener<?> listener : application.getListeners()) {
			this.initialMulticaster.addApplicationListener(listener);
		}
	}
}
```

EventPublishingRunListener 对于不同的事件有不同的处理方法，但是他们的流程基本相同，都是  某个事件被调用 ，封装application和args到事件内，通过multiEvent或publishEvent发布事件。

```java
// 使用的是initialMulticaster.multicastEvent()
@Override
public void contextPrepared(ConfigurableApplicationContext context) {
    this.initialMulticaster
        .multicastEvent(new ApplicationContextInitializedEvent(this.application, this.args, context));
}


@Override
public void contextLoaded(ConfigurableApplicationContext context) {
    // 遍历application所有的监听器实现类
    for (ApplicationListener<?> listener : this.application.getListeners()) {
        // 如果为ApplicationContextAware，则将上下文信息设置到该监听器内
        if (listener instanceof ApplicationContextAware) {
            ((ApplicationContextAware) listener).setApplicationContext(context);
        }
        // 将application中的监听器实现类全部添加到上下文中
        context.addApplicationListener(listener);
    }
    // 广播事件ApplicationPreparedEvent
    this.initialMulticaster.multicastEvent(new ApplicationPreparedEvent(this.application, this.args, context));
}
//使用的是publishEvent
@Override
public void started(ConfigurableApplicationContext context) {
    context.publishEvent(new ApplicationStartedEvent(this.application, this.args, context));
    AvailabilityChangeEvent.publish(context, LivenessState.CORRECT);
}
```

**某些方法是通过initialMulticaster的multicastEvent进行事件的广播，某些方法是通过context参数的publishEvent方法来进行发布的**

> contextLoaded方法在发布事件之前做了两件事：第一，遍历application的所有监听器实现类，如果该实现类还实现了ApplicationContextAware接口，则将上下文信息设置到该监听器内；第二，将application中的监听器实现类全部添加到上下文中。最后一步才是调用事件广播

> 在此方法之前执行的事件广播都是通过multicastEvent来进行的，而该方法之后的方法则均采用publishEvent来执行。这是因为只有到了contextLoaded方法之后，上下文才算初始化完成，才可通过它的publishEvent方法来进行事件的发布



如果想自定义实现SpringApplicationRunListener在对应的方法中实现自己的业务逻辑，则可以去实现这个SpringApplicationRunListener类,然后在spring.factories中配置出扫描的包名和路径类。



#### 2.初始化ApplicationArguments，将SpringApplication的需要的参数进行初始化

```java
// 创建ApplicationArguments对象
ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
```

监听器启动之后，紧接着便是执行ApplicationArguments对象的初始化，Application-Arguments是用于提供访问运行SpringApplication时的参数。

ApplicationArguments的初始化过程非常简单，只是调用了它的实现类Default-ApplicationArguments并传入main方法中的args参数

**在DefaultApplicationArguments中将参数args封装为Source对象，Source对象是基于Spring框架的SimpleCommandLinePropertySource来实现的。只需知道通过main方法传递进来的参数被封装成ApplicationArguments对象即可**

#### 3.初始化ConfigurableEnvironment

完成ApplicationArguments参数准备之后便开始通过prepareEnvironment方法对ConfigurableEnvironment对象进行初始化操作

```java
 // 加载属性配置，包括所有的属性配置，如application.properties中和外部属性
ConfigurableEnvironment environment = this.prepareEnvironment(listeners, applicationArguments);
```

这里我们先看下ConfigurableEnvironment这个接口：

```java
public interface ConfigurableEnvironment extends Environment, ConfigurablePropertyResolver {
    // 设置激活的组集合
    void setActiveProfiles(String... var1);
	// 向当前激活的组集合中添加一个profile组
    void addActiveProfile(String var1);
	// 设置默认激活的组激活。激活组集合为空时会使用默认的组集合
    void setDefaultProfiles(String... var1);
	// 获取当前的环境对象中的属性源集合，也就是应用环境变量
    // 属性原集合其实就是一个容纳PropertySource容器
    // 该方法提供了直接配置属性源的入口
    MutablePropertySources getPropertySources();
	// 获取提供直接配置虚拟机环境变量的入口
    Map<String, Object> getSystemProperties();
	// 获取操作系统环境变量
    Map<String, Object> getSystemEnvironment();
	// 合并指定环境中的配置到当前的环境
    void merge(ConfigurableEnvironment var1);
}

```

从代码可以看到ConfigurableEnvironment接口继承自Environment接口和ConfigurablePropertyResolver，最终都继承自接口PropertyResolver。ConfigurableEnvironment接口的主要作用是提供当前运行环境的公开接口，比如配置文件profiles各类系统属性和变量的设置、添加、读取、合并等功能

我们在看下`this.prepareEnvironment(listeners, applicationArguments);`方法

```java
private ConfigurableEnvironment prepareEnvironment(SpringApplicationRunListeners listeners,
			ApplicationArguments applicationArguments) {
		// Create and configure the environment
    	// 获取或创建环境
		ConfigurableEnvironment environment = getOrCreateEnvironment();
    	// 配置环境，主要包括PropertySources和activeProfiles的配置
		configureEnvironment(environment, applicationArguments.getSourceArgs());
    	// 将ConfigurationPropertySources附加到指定环境中的第一位，并动态跟踪环境的添加或删除
		ConfigurationPropertySources.attach(environment);
    	// 监听器准备
		listeners.environmentPrepared(environment);
    	// 将环境绑定到SpringApplication
		bindToSpringApplication(environment);
    	// 判断是否时定制的环境，如果不是定制的环境则将环境转为StandardEnvironment
		if (!this.isCustomEnvironment) {
			environment = new EnvironmentConverter(getClassLoader()).convertEnvironmentIfNecessary(environment,
					deduceEnvironmentClass());
		}
    	// 将ConfigurationPropertySources附件到指定环境中方
		ConfigurationPropertySources.attach(environment);
		return environment;
	}
	// 推断环境类型
	private Class<? extends StandardEnvironment> deduceEnvironmentClass() {
		switch (this.webApplicationType) {
		case SERVLET:
			return StandardServletEnvironment.class;
		case REACTIVE:
			return StandardReactiveWebEnvironment.class;
		default:
			return StandardEnvironment.class;
		}
	}
	//获取或者创建环境
	private ConfigurableEnvironment getOrCreateEnvironment() {
		if (this.environment != null) {
			return this.environment;
		}
		switch (this.webApplicationType) {
		case SERVLET:
			return new StandardServletEnvironment();
		case REACTIVE:
			return new StandardReactiveWebEnvironment();
		default:
			return new StandardEnvironment();
		}
	}
```

####  4.忽略信息配置

ConfigurableEnvironment的初始化和准备工作已经完成。之后，程序又对环境中的忽略信息配置项“spring.beaninfo.ignore”的值进行获取判断，进而设置为系统参数中的忽略项

spring.beaninfo.ignore的配置用来决定是否跳过BeanInfo类的扫描，如果设置为true，则跳过。

```java
private void configureIgnoreBeanInfo(ConfigurableEnvironment environment) {
    // 如果系统参数中spring.beaninfo.ignore为null
		if (System.getProperty(CachedIntrospectionResults.IGNORE_BEANINFO_PROPERTY_NAME) == null) {
            // 获取环境中spring.beaninfo.ignore的配置
			Boolean ignore = environment.getProperty("spring.beaninfo.ignore", Boolean.class, Boolean.TRUE);
            // 设置对应的系统参数
			System.setProperty(CachedIntrospectionResults.IGNORE_BEANINFO_PROPERTY_NAME, ignore.toString());
		}
	}
```

####  5.打印banner

完成环境的基本处理之后，下面就是控制台Banner的打印了

```java
private Banner printBanner(ConfigurableEnvironment environment) {
    // 如果处于关闭状态返回null
		if (this.bannerMode == Banner.Mode.OFF) {
			return null;
		}
    // 如果ResourceLoader不存在则创建一个默认的
		ResourceLoader resourceLoader = (this.resourceLoader != null) ? this.resourceLoader
				: new DefaultResourceLoader(null);
    // 构建SpringApplicationBannerPrinter
		SpringApplicationBannerPrinter bannerPrinter = new SpringApplicationBannerPrinter(resourceLoader, this.banner);
		if (this.bannerMode == Mode.LOG) {
			return bannerPrinter.print(environment, this.mainApplicationClass, logger);
		}
    // 打印到控制台
		return bannerPrinter.print(environment, this.mainApplicationClass, System.out);
	}
```

#### 6.Spring应用上下文的创建

在前面的章节中已经多次涉及WebApplicationType枚举类，无论是推断Web应用类型，还是创建不同的配置环境都与此枚举类有关。Spring Boot创建Spring的应用上下文时，如果未指定要创建的类，则会根据之前推断出的类型来进行默认上下文类的创建。

在Spring Boot中通过SpringApplication类中的createApplicationContext来进行应用上下文的创建，代码如下

```java
// 变量
public static final String DEFAULT_CONTEXT_CLASS = "org.springframework.context."
			+ "annotation.AnnotationConfigApplicationContext";
	
public static final String DEFAULT_SERVLET_WEB_CONTEXT_CLASS = "org.springframework.boot."
			+ "web.servlet.context.AnnotationConfigServletWebServerApplicationContext";

public static final String DEFAULT_REACTIVE_WEB_CONTEXT_CLASS = "org.springframework."
			+ "boot.web.reactive.context.AnnotationConfigReactiveWebServerApplicationContext";


protected ConfigurableApplicationContext createApplicationContext() {
    // 获取容器的类变量
		Class<?> contextClass = this.applicationContextClass;
    // 如果为null，则根据web应用类型按照默认类进行创建
		if (contextClass == null) {
			try {
				switch (this.webApplicationType) {
				case SERVLET:
					contextClass = Class.forName(DEFAULT_SERVLET_WEB_CONTEXT_CLASS);
					break;
				case REACTIVE:
					contextClass = Class.forName(DEFAULT_REACTIVE_WEB_CONTEXT_CLASS);
					break;
				default:
					contextClass = Class.forName(DEFAULT_CONTEXT_CLASS);
				}
			}
			catch (ClassNotFoundException ex) {
				throw new IllegalStateException(
						"Unable create a default ApplicationContext, please specify an ApplicationContextClass", ex);
			}
		}
    // 如果存在对应的Class配置
		return (ConfigurableApplicationContext) BeanUtils.instantiateClass(contextClass);
	}
```



#### 7.Spring应用上下文的准备



```java
private void prepareContext(ConfigurableApplicationContext context, ConfigurableEnvironment environment,
			SpringApplicationRunListeners listeners, ApplicationArguments applicationArguments, Banner printedBanner) {
    // 设置上下文的的配置环境
		context.setEnvironment(environment);
    // 应用任何相关的后处理{@link ApplicationContext}。子类可以*根据需要应用其他处理。 * @param context应用程序上下文
		postProcessApplicationContext(context);
    //  在context ressh之前调用 appplicationContextInitializer
		applyInitializers(context);
    // 上下文准备之后调用（目前是空实现，可用于拓展）
		listeners.contextPrepared(context);
		if (this.logStartupInfo) {
            // 调用日志记录启动信息的子类可以重写以添加其他*日志记录
			logStartupInfo(context.getParent() == null);
			logStartupProfileInfo(context);
		}
    //  获取单例的bean工厂
		// Add boot specific singleton beans
		ConfigurableListableBeanFactory beanFactory = context.getBeanFactory();
     // 向beanFactory注册单例bean：命令行参数bean
		beanFactory.registerSingleton("springApplicationArguments", applicationArguments);
		if (printedBanner != null) {
            // 打印banner
			beanFactory.registerSingleton("springBootBanner", printedBanner);
		}
    // 判断是否允许bean定义重写
		if (beanFactory instanceof DefaultListableBeanFactory) {
			((DefaultListableBeanFactory) beanFactory)
					.setAllowBeanDefinitionOverriding(this.allowBeanDefinitionOverriding);
		}
    // 设置懒加载bean
		if (this.lazyInitialization) {
			context.addBeanFactoryPostProcessor(new LazyInitializationBeanFactoryPostProcessor());
		}
		// Load the sources
		Set<Object> sources = getAllSources();
		Assert.notEmpty(sources, "Sources must not be empty");
       // 将bean加载到应用上下文中
		load(context, sources.toArray(new Object[0]));
        // 向上下文中添加ApplicationListener，并广播ApplicationPreparedEvent事件
		listeners.contextLoaded(context);
	}
```



#### 8 .Spring应用上下文的刷新

```java
private void refreshContext(ConfigurableApplicationContext context) {
		refresh((ApplicationContext) context);
		if (this.registerShutdownHook) {
			try {
				context.registerShutdownHook();
			}
			catch (AccessControlException ex) {
				// Not allowed in some environments.
			}
		}
	}

	/**
	 * 刷新底层.
	 * @param applicationContext the application context to refresh
	 * @deprecated since 2.3.0 in favor of
	 * {@link #refresh(ConfigurableApplicationContext)}
	 */
	@Deprecated
	protected void refresh(ApplicationContext applicationContext) {
		Assert.isInstanceOf(ConfigurableApplicationContext.class, applicationContext);
		refresh((ConfigurableApplicationContext) applicationContext);
	}

	/**
	 * 刷新底层 {@link ApplicationContext}.
	 * @param applicationContext the application context to refresh
	 */
	protected void refresh(ConfigurableApplicationContext applicationContext) {
		applicationContext.refresh();
	}
// 实际的refresh()的刷新逻辑是在AbstractApplicationContext的类中进行书写的。 
@Override
	public void refresh() throws BeansException, IllegalStateException {
		synchronized (this.startupShutdownMonitor) {
			// Prepare this context for refreshing.
			prepareRefresh();

			// Tell the subclass to refresh the internal bean factory.
			ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

			// Prepare the bean factory for use in this context.
			prepareBeanFactory(beanFactory);

			try {
				// Allows post-processing of the bean factory in context subclasses.
				postProcessBeanFactory(beanFactory);

				// Invoke factory processors registered as beans in the context.
				invokeBeanFactoryPostProcessors(beanFactory);

				// Register bean processors that intercept bean creation.
				registerBeanPostProcessors(beanFactory);

				// Initialize message source for this context.
				initMessageSource();

				// Initialize event multicaster for this context.
				initApplicationEventMulticaster();

				// Initialize other special beans in specific context subclasses.
				onRefresh();

				// Check for listener beans and register them.
				registerListeners();

				// Instantiate all remaining (non-lazy-init) singletons.
				finishBeanFactoryInitialization(beanFactory);

				// Last step: publish corresponding event.
				finishRefresh();
			}

			catch (BeansException ex) {
				if (logger.isWarnEnabled()) {
					logger.warn("Exception encountered during context initialization - " +
							"cancelling refresh attempt: " + ex);
				}

				// Destroy already created singletons to avoid dangling resources.
				destroyBeans();

				// Reset 'active' flag.
				cancelRefresh(ex);

				// Propagate exception to caller.
				throw ex;
			}

			finally {
				// Reset common introspection caches in Spring's core, since we
				// might not ever need metadata for singleton beans anymore...
				resetCommonCaches();
			}
		}
	}
```



#### 9. 调用ApplicationRunner和CommandLineRunner

```java
	private void callRunners(ApplicationContext context, ApplicationArguments args) {
		List<Object> runners = new ArrayList<>();
		runners.addAll(context.getBeansOfType(ApplicationRunner.class).values());
		runners.addAll(context.getBeansOfType(CommandLineRunner.class).values());
		AnnotationAwareOrderComparator.sort(runners);
		for (Object runner : new LinkedHashSet<>(runners)) {
			if (runner instanceof ApplicationRunner) {
				callRunner((ApplicationRunner) runner, args);
			}
			if (runner instanceof CommandLineRunner) {
				callRunner((CommandLineRunner) runner, args);
			}
		}
	}

	private void callRunner(ApplicationRunner runner, ApplicationArguments args) {
		try {
			(runner).run(args);
		}
		catch (Exception ex) {
			throw new IllegalStateException("Failed to execute ApplicationRunner", ex);
		}
	}
```

到此为止SpringApplication的run()方法已经全部的进行完毕。