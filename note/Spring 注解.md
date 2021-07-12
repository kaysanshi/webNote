## Spring注解：

在说spring注解时我们先看下什么是JAVA注解：Annotation(注解)是JDK1.5及以后版本引入的。它可以用于创建文档，跟踪代码中的依赖性，甚至执行基本编译时检查。注解是以‘@注解名’在代码中存在的，根据注解参数的个数，我们可以将注解分为`：标记注解、单值注解、完整注解`三类。它们都不会直接影响到程序的语义，只是作为注解（标识）存在，我们可以通过反射机制编程实现对这些元数据（用来描述数据的数据）的访问。

AnnotationConfigApplicationContext:是一个用来管理注解bean的容器

### 组件的添加：

![](https://mmbiz.qpic.cn/mmbiz_png/B77kSvewKqXRz1TazsVnVfXHaEO0ETqV9xyJUSPnRARAy8Eic9qHCXFwmFLptWzpQQg6HNWpffLFnibIJZuMmm8A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



`@Configuration`:告诉spring这是个配置类（等同于配置文件）

`@ComponentScan`:指定扫描的包	

```java
@ComponentScans(
        value = {
                @ComponentScan(value="com.atguigu", 
                        includeFilters = {
                                @Filter(type=FilterType.ANNOTATION,classes={Controller.class}), 
                                @Filter(type=FilterType.ASSIGNABLE_TYPE,classes={BookService.class}),
                                @Filter(type=FilterType.CUSTOM,classes={MyTypeFilter.class})
                },useDefaultFilters = false)
        }
)
//@ComponentScan  value:指定要扫描的包
//excludeFilters = Filter[] ：指定扫描的时候按照什么规则排除那些组件
//includeFilters = Filter[] ：指定扫描的时候只需要包含哪些组件
//FilterType.ANNOTATION：按照注解
//FilterType.ASSIGNABLE_TYPE：按照给定的类型；
//FilterType.ASPECTJ：使用ASPECTJ表达式
//FilterType.REGEX：使用正则指定
//FilterType.CUSTOM：使用自定义规则
```

`@Bean`:给容器中注册一个Bean;类型为返回值的类型，id默认是用方法名作为id

```java
@Bean
public Person person01(){
	return new Person("Bill Gates",62);
}
```

`@Component`:默认加在ioc容器中的组件，容器启动会调用无参构造器创建对象，再进行初始化赋值等操作

```java
@Component
public class Car {
	
	public Car(){
		System.out.println("car constructor...");
	}
	
	public void init(){
		System.out.println("car ... init...");
	}
	
	public void detory(){
		System.out.println("car ... detory...");
	}

}
```

`@Controller`: 声明是一个控制层 名字默认是类名首字母小写

`@Service`：声明是service层 名字默认是类名首字母小写 其中自己可以改变这个实例名字：	

`@Repository`：声明是dao层。名字默认是类名首字母小写 同样可以自己手动书写实例名称

`@Conditional`: 按照一定的条件进行判断，满足条件给容器中注册bean

```java
/**
* @Conditional({Condition}) ： 按照一定的条件进行判断，满足条件给容器中注册bean
* 如果系统是windows，给容器中注册("bill")
* 如果是linux系统，给容器中注册("linus")
*/
@Bean("bill")
public Person person01(){
	return new Person("Bill Gates",62);
}

@Conditional(LinuxCondition.class)
@Bean("linus")
public Person person02(){
	return new Person("linus", 48);
}
//LinuxCondition类。判断是否linux系统
public class LinuxCondition implements Condition {

	/**
	 * ConditionContext：判断条件能使用的上下文（环境）
	 * AnnotatedTypeMetadata：注释信息
	 */
	@Override
	public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
		// TODO是否linux系统
		//1、能获取到ioc使用的beanfactory
		ConfigurableListableBeanFactory beanFactory = context.getBeanFactory();
		//2、获取类加载器
		ClassLoader classLoader = context.getClassLoader();
		//3、获取当前环境信息
		Environment environment = context.getEnvironment();
		//4、获取到bean定义的注册类
		BeanDefinitionRegistry registry = context.getRegistry();
		
		String property = environment.getProperty("os.name");
		
		//可以判断容器中的bean注册情况，也可以给容器中注册bean
		boolean definition = registry.containsBeanDefinition("person");
		if(property.contains("linux")){
			return true;
		}
		
		return false;
	}

}
```

`@Primary`: 让Spring进行自动装配的时候，默认使用首选的bean；也可以继续使用 `@Qualifier`指定需要装配的bean的名字

`@Lazy`：懒加载：

- 单实例bean：默认在容器启动的时候创建对象；
- 懒加载：容器启动不创建对象。第一次使用(获取)Bean创建对象，并初始化；

```java
@Lazy
@Bean("person")
public Person person(){
    System.out.println("给容器中添加Person....");
    return new Person("张三", 25);
}
```

`@Scope` :设置作用域。默认为单例模式

```java
/**
 * ConfigurableBeanFactory#SCOPE_PROTOTYPE
 * @see ConfigurableBeanFactory#SCOPE_SINGLETON
 * @see org.springframework.web.context.WebApplicationContext#SCOPE_REQUEST  request
 * @see org.springframework.web.context.WebApplicationContext#SCOPE_SESSION     sesssion
 * 
 * @Scope:调整作用域 
 *参数：
 * singleton：单实例的（默认值）：ioc容器启动会调用方法创建对象放到ioc容器中。
 *            以后每次获取就是直接从容器（map.get()）中拿，
 * prototype：多实例的：ioc容器启动并不会去调用方法创建对象放在容器中。每次获取的时候才会调用方法创建对象；
 * 
 * request：同一次请求创建一个实例
 * session：同一个session创建一个实例
 * application:全局web应用级别的信作用域。
 */
```

`@Import`:要导入到容器的组件,容器中就会自动的注册这个组件，id默认是全类名

```java
// 使用import进行导入Color类
@Import({Color.class,Red.class})
//@Import导入组件，id默认是组件的全类名
public class MainConfig2 {}
```

**给容器中注册组件的方式：**

1. 包扫描+组件标注注解（@Controller/@Service/@Repository/@Component）[自己写的类]
2. @Bean[导入的第三方包里面的组件]
3. @Import[快速给容器中导入一个组件]
   1. @Import(要导入到容器中的组件)；容器中就会自动注册这个组件，id默认是全类名
   
      ```java
      // 使用import进行导入Color类
      @Import({Color.class,Red.class,MyImportSelector.class,MyImportBeanDefinitionRegistrar.class})
      //@Import导入组件，id默认是组件的全类名
      public class MainConfig2 {}
      
      ```
   
   2. 实现ImportSelector:返回需要导入的组件的全类名数组；实现IportSelector接口
   
      ```java
      //自定义逻辑返回需要导入的组件
      public class MyImportSelector implements ImportSelector {
      
      	//返回值，就是到导入到容器中的组件全类名
      	//AnnotationMetadata:当前标注@Import注解的类的所有注解信息
      	@Override
      	public String[] selectImports(AnnotationMetadata importingClassMetadata) {
      		// TODO Auto-generated method stub
      		//importingClassMetadata
      		//方法不要返回null值
      		return new String[]{"com.atguigu.bean.Blue","com.atguigu.bean.Yellow"};
      	}
      
      }
      // 然后在配置类中通过@Import进行注入
      @Import({Color.class,Red.class,MyImportSelector.class,MyImportBeanDefinitionRegistrar.class})
      //@Import导入组件，id默认是组件的全类名
      public class MainConfig2 {}
      ```
   
   3. 实现ImportBeanDefinitionRegistrar:手动注册bean到容器中
   
      ```java
      public class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {
      
      	/**
      	 * AnnotationMetadata：当前类的注解信息
      	 * BeanDefinitionRegistry:BeanDefinition注册类；
      	 * 		把所有需要添加到容器中的bean；调用
      	 * 		BeanDefinitionRegistry.registerBeanDefinition手工注册进来
      	 */
      	@Override
      	public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
      		
      		boolean definition = registry.containsBeanDefinition("com.atguigu.bean.Red");
      		boolean definition2 = registry.containsBeanDefinition("com.atguigu.bean.Blue");
      		if(definition && definition2){
      			//指定Bean定义信息；（Bean的类型，Bean。。。）
      			RootBeanDefinition beanDefinition = new RootBeanDefinition(RainBow.class);
      			//注册一个Bean，指定bean名
      			registry.registerBeanDefinition("rainBow", beanDefinition);
      		}
      	}
      
      }
      // 使用
      @Configuration
      @Import({Color.class,Red.class,MyImportSelector.class,MyImportBeanDefinitionRegistrar.class})
      //@Import导入组件，id默认是组件的全类名
      public class MainConfig2 {}
      ```
4. 使用Spring提供的 FactoryBean（工厂Bean）
   
   1. 默认获取到的是工厂bean调用getObject创建的对象
   
   2. 要获取工厂Bean本身，我们需要给id前面加一个&。&colorFactoryBean
   
      ```java
      //创建一个Spring定义的FactoryBean
      public class ColorFactoryBean implements FactoryBean<Color> {
      
      	//返回一个Color对象，这个对象会添加到容器中
      	@Override
      	public Color getObject() throws Exception {
      		// TODO Auto-generated method stub
      		System.out.println("ColorFactoryBean...getObject...");
      		return new Color();
      	}
      
      	@Override
      	public Class<?> getObjectType() {
      		// TODO Auto-generated method stub
      		return Color.class;
      	}
      
      	//是单例？
      	//true：这个bean是单实例，在容器中保存一份
      	//false：多实例，每次获取都会创建一个新的bean；
      	@Override
      	public boolean isSingleton() {
      		// TODO Auto-generated method stub
      		return false;
      	}
      
      }
      // 在配置类中进行获取
      @Bean
      public ColorFactoryBean colorFactoryBean(){
          return new ColorFactoryBean();
      }
      ```
   
      测试：
   
      ```java
      public class IOCTest {
      	AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(MainConfig2.class);
      	
      	
      	@Test
      	public void testImport(){
      		printBeans(applicationContext);
      		Blue bean = applicationContext.getBean(Blue.class);
      		System.out.println(bean);
      		
      		//工厂Bean获取的是调用getObject创建的对象
      		Object bean2 = applicationContext.getBean("colorFactoryBean");
      		Object bean3 = applicationContext.getBean("colorFactoryBean");
      		System.out.println("bean的类型："+bean2.getClass());// 获取的是color
      		System.out.println("bean的类型："+bean3.getClass());// 获取的是color
      		System.out.println(bean2 == bean3);// false
      
      		// 通过&id前缀是获取的工厂bean的本身
      		Object bean4 = applicationContext.getBean("&colorFactoryBean");
      		System.out.println(bean4.getClass());// colorFactoryBean
      	}
      	
      	private void printBeans(AnnotationConfigApplicationContext applicationContext){
      		String[] definitionNames = applicationContext.getBeanDefinitionNames();
      		for (String name : definitionNames) {
      			System.out.println(name);
      		}
      	}
      ```
   
      

#### Bean生命周期

bean创建---初始化----销毁的过程 

容器管理bean的生命周期；我们可以自定义初始化和销毁方法；容器在bean进行到当前生命周期的时候来调用我们自定义的初始化和销毁方法

##### Bean创建

  单实例(@Scope("singleton"))：在容器启动的时候创建对象 

  多实例(@Scope("prototype"))：在每次获取的时候创建对象

##### Bean初始化：

**bean初始化前调用进行额外操作：BeanPostProcessor.postProcessBeforeInitialization**

对象创建完成，并赋值后，调用bean的初始化方法

**bean初始化后调用进行额外操作：BeanPostProcessor.postProcessAfterInitialization**

> BeanPostProcessor原理
> 遍历得到容器中所有的BeanPostProcessor；挨个执行beforeInitialization，一但返回null，跳出for循环，不会执行后面的BeanPostProcessor.postProcessorsBeforeInitialization.

> populateBean(beanName, mbd, instanceWrapper);给bean进行属性赋值
> initializeBean { 
>     applyBeanPostProcessorsBeforeInitialization(wrappedBean, beanName);// 应用所有的beforeInilization;
>     invokeInitMethods(beanName, wrappedBean, mbd);执行自定义初始化 	
>     // 应用所有的afterInilization;
>     applyBeanPostProcessorsAfterInitialization(wrappedBean, beanName);  
> }

##### Bean销毁：

 单实例(@Scope("singleton"))：容器关闭的时候

  多实例(@Scope("prototype"))：容器不会管理这个bean；容器不会调用销毁方法

#### Bean初始化方式

##### 指定初始化和销毁方法

```java
@ComponentScan("com.atguigu.bean")
@Configuration
public class MainConfigOfLifeCycle {
	
	//@Scope("prototype")
	@Bean(initMethod="init",destroyMethod="detory")
	public Car car(){
		return new Car();
	}

}
@Component
public class Car {
	
	public Car(){
		System.out.println("car constructor...");
	}
	
	public void init(){
		System.out.println("car ... init...");
	}
	
	public void detory(){
		System.out.println("car ... detory...");
	}

}
```

##### 通过让Bean实现InitializingBean（定义初始化逻辑），DisposableBean（定义销毁逻辑）

```java
@Component
public class Cat implements InitializingBean,DisposableBean {
	
	public Cat(){
		System.out.println("cat constructor...");
	}

	@Override
	public void destroy() throws Exception {
		// TODO Auto-generated method stub
		System.out.println("cat...destroy...");
	}

	@Override
	public void afterPropertiesSet() throws Exception {
		// TODO Auto-generated method stub
		System.out.println("cat...afterPropertiesSet...");
	}

}
```

##### 可以使用JSR250：

- @PostConstruct：在bean创建完成并且属性赋值完成；来执行初始化方法    
- @PreDestroy：在容器销毁bean之前通知我们进行清理工作

```java
// 通过ApplicationContextAware创建ioc容器。这个就是beanPostProcessorAware进行操作的。
@Component
public class Dog implements ApplicationContextAware {
	
	//@Autowired
	private ApplicationContext applicationContext;
	
	public Dog(){
		System.out.println("dog constructor...");
	}
	
	//对象创建并赋值之后调用
	@PostConstruct
	public void init(){
		System.out.println("Dog....@PostConstruct...");
	}
	
	//容器移除对象之前
	@PreDestroy
	public void detory(){
		System.out.println("Dog....@PreDestroy...");
	}

	@Override
	public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
		// TODO Auto-generated method stub
		this.applicationContext = applicationContext;
	}
}
// 通过配置类进行扫描到@Component然后进行注入
@ComponentScan("com.atguigu.bean")
@Configuration
public class MainConfigOfLifeCycle {}
```

##### BeanPostProcessor【interface】：bean的后置处理器 

在bean初始化前后进行一些处理工作    postProcessBeforeInitialization:在初始化之前工作    postProcessAfterInitialization:在初始化之后工作

```
/**
 * 后置处理器：初始化前后进行处理工作
 * 将后置处理器加入到容器中
 */
@Component
public class MyBeanPostProcessor implements BeanPostProcessor {

	@Override
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		// TODO Auto-generated method stub
		System.out.println("postProcessBeforeInitialization..."+beanName+"=>"+bean);
		return bean;
	}

	@Override
	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		// TODO Auto-generated method stub
		System.out.println("postProcessAfterInitialization..."+beanName+"=>"+bean);
		return bean;
	}

}

```

### 组件赋值：

![](https://mmbiz.qpic.cn/mmbiz_png/B77kSvewKqXRz1TazsVnVfXHaEO0ETqV4tGYkQlibOBWjcxkOzRh1tHibVmJwVXmyib36jJ1aquZz0j09oZSkNJYQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

`@Value`: @Value赋值；

1. 基本数值
2. 可以写SPEL:#{}
3. 可以写${}；取出配置文件【properties】中的值（在运行环境变量里面的值）

```java
// 基本数值
@Value("张三")
private String name;
// 可以写SPEL:#{}
@Value("#{20-2}")
private Integer age;
// 可以写${}；取出配置文件【properties】中的值（在运行环境变量里面的值）
@Value("${person.nickName}")
private String nickName;
```

#### 自动装配：

##### @Autowired(Spring定义的注解)

`@Autowired` ：通过匹配`数据类型`自动装配 默认为 `byType`方式；标注在方法，Spring容器创建当前对象，就会调用方法，完成赋值；方法使用的参数，自定义类型的值从ioc容器中获取。

1. 默认优先按照类型去容器中找对应的组件:applicationContext.getBean(BookDao.class);找到就赋值

2. 如果找到多个相同类型的组件，再将属性的名称作为组件的id去容器中查找applicationContext.getBean("bookDao")

3. `@Qualifier("bookDao")`：使用@Qualifier指定需要装配的组件的id，而不是使用属性名

4. 自动装配默认一定要将属性赋值好，没有就会报错；可以使用@Autowired(required=false);

5. `@Primary`：让Spring进行自动装配的时候，默认使用首选的bean；也可以继续使用`@Qualifier`指定需要装配的bean的名字

   ```java
   class BookService{
       @Autowired
       BookDao  bookDao;
   }
   ```

###### @Autowired所在的位置

 @Autowired:可标注：构造器，参数，方法，属性；都是从容器中获取参数组件的值  

1. [标注在方法位置]：@Bean+方法参数；参数从容器中获取;默认不写@Autowired效果是一样的；都能自动装配  

   ```java
   //标注在方法，Spring容器创建当前对象，就会调用方法，完成赋值；
   //方法使用的参数，自定义类型的值从ioc容器中获取
   @Autowired 
   public void setCar(Car car) {
   	this.car = car;
   }
   ```

2. [标在构造器上]：如果组件只有一个有参构造器，这个有参构造器的@Autowired可以省略，参数位置的组件还是可以自动从容器中获取   

   ```java
   //构造器要用的组件，都是从容器中获取
   @Autowired
   public Boss(Car car){
       this.car = car;
       System.out.println("Boss...有参构造器");
   }
   ```

3. 放在参数位置：

   ```java
   public void setCar(@Autowired Car car) {
    this.car = car;
   }
   ```

##### Spring还支持使用@Resource(JSR250)和@Inject(JSR330)[java规范的注解]

`@Resources:`可以和@`Autowired`一样实现自动装配功能；默认是按照`组件名称`进行装配的 `byName`； 没有能支持@Primary功能没有支持@Autowired（reqiured=false）;

`@Inject：`需要导入javax.inject的包，和Autowired的功能一样。没有required=false的功能；

##### 自定义组件使用Spring容器的一些组件

自定义组件实现xxxAware；在创建对象的时候，会调用接口规定的方法注入相关组件；Aware；   

把Spring底层一些组件注入到自定义的Bean中；  都是使用的后置处理器进行注入的。

```java
@Component
public class Red implements ApplicationContextAware,BeanNameAware,EmbeddedValueResolverAware {
	
	private ApplicationContext applicationContext;

	@Override
	public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
		// TODO Auto-generated method stub
		System.out.println("传入的ioc："+applicationContext);
		this.applicationContext = applicationContext;
	}

	@Override
	public void setBeanName(String name) {
		// TODO Auto-generated method stub
		System.out.println("当前bean的名字："+name);
	}

	@Override
	public void setEmbeddedValueResolver(StringValueResolver resolver) {
		// TODO Auto-generated method stub
		String resolveStringValue = resolver.resolveStringValue("你好 ${os.name} 我是 #{20*18}");
		System.out.println("解析的字符串："+resolveStringValue);
	}

}

```

#### 读取外部配置文件@PropertySource

`@PropertySource：`读取外部配置文件中的k/v保存到运行的环境变量中;加载完外部的配置文件以后使用${}取出配置文件的值`@PropertySource(value{"classpath:/person.properties"})`

```java
//使用@PropertySource读取外部配置文件中的k/v保存到运行的环境变量中;加载完外部的配置文件以后使用${}取出配置文件的值
@PropertySource(value={"classpath:/person.properties"})
@Configuration
public class MainConfigOfPropertyValues {
	
	@Bean
	public Person person(){
		return new Person();
	}

}
```

`@PropertySources`:读取多个propertySource

```java
@PropertySources({ @PropertySource("classpath:person.properties"), 
				   @PropertySource("classpath:database.properties") })
```

#### 动态激活环境@Profile

`@Profile：`指定组件在哪个环境的情况下才能被注册到容器中，不指定，任何环境下都能注册这个组件

1. 加了环境标识的bean，只有这个环境被激活的时候才能注册到容器中。默认是default环境
2. 写在配置类上，只有是指定的环境的时候，整个配置类里面的所有配置才能开始生效
3. 没有标注环境标识的bean在，任何环境下都是加载的；

```java
/**
 * Profile：
 * 		Spring为我们提供的可以根据当前环境，动态的激活和切换一系列组件的功能；
 * 
 * 开发环境、测试环境、生产环境；
 * 数据源：(/A)(/B)(/C)；
 * 
 * 
 * @Profile：指定组件在哪个环境的情况下才能被注册到容器中，不指定，任何环境下都能注册这个组件
 * 
 * 1）、加了环境标识的bean，只有这个环境被激活的时候才能注册到容器中。默认是default环境
 * 2）、写在配置类上，只有是指定的环境的时候，整个配置类里面的所有配置才能开始生效
 * 3）、没有标注环境标识的bean在，任何环境下都是加载的；
 */

@PropertySource("classpath:/dbconfig.properties")
@Configuration
public class MainConfigOfProfile implements EmbeddedValueResolverAware{
	
	@Value("${db.user}")
	private String user;
	
	private StringValueResolver valueResolver;
	
	private String  driverClass;
	
	
	@Bean
	public Yellow yellow(){
		return new Yellow();
	}
	
	@Profile("test")
	@Bean("testDataSource")
	public DataSource dataSourceTest(@Value("${db.password}")String pwd) throws Exception{
		ComboPooledDataSource dataSource = new ComboPooledDataSource();
		dataSource.setUser(user);
		dataSource.setPassword(pwd);
		dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/test");
		dataSource.setDriverClass(driverClass);
		return dataSource;
	}
	
	
	@Profile("dev")
	@Bean("devDataSource")
	public DataSource dataSourceDev(@Value("${db.password}")String pwd) throws Exception{
		ComboPooledDataSource dataSource = new ComboPooledDataSource();
		dataSource.setUser(user);
		dataSource.setPassword(pwd);
		dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/ssm_crud");
		dataSource.setDriverClass(driverClass);
		return dataSource;
	}
	
	@Profile("prod")
	@Bean("prodDataSource")
	public DataSource dataSourceProd(@Value("${db.password}")String pwd) throws Exception{
		ComboPooledDataSource dataSource = new ComboPooledDataSource();
		dataSource.setUser(user);
		dataSource.setPassword(pwd);
		dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/scw_0515");
		
		dataSource.setDriverClass(driverClass);
		return dataSource;
	}

	@Override
	public void setEmbeddedValueResolver(StringValueResolver resolver) {
		// TODO Auto-generated method stub
		this.valueResolver = resolver;
		driverClass = valueResolver.resolveStringValue("${db.driverClass}");
	}
//// 测试
public class IOCTest_Profile {
	
	//1、使用命令行动态参数: 在虚拟机参数位置加载 -Dspring.profiles.active=test
	//2、代码的方式激活某种环境；
	@Test
	public void test01(){
		AnnotationConfigApplicationContext applicationContext = 
				new AnnotationConfigApplicationContext();
		//1、创建一个applicationContext
		//2、设置需要激活的环境
		applicationContext.getEnvironment().setActiveProfiles("dev");
		//3、注册主配置类
		applicationContext.register(MainConfigOfProfile.class);
		//4、启动刷新容器
		applicationContext.refresh();
		
		
		String[] namesForType = applicationContext.getBeanNamesForType(DataSource.class);
		for (String string : namesForType) {
			System.out.println(string);
		}
		
		Yellow bean = applicationContext.getBean(Yellow.class);
		System.out.println(bean);
		applicationContext.close();
	}

}
```

### 组件注入：

![](https://mmbiz.qpic.cn/mmbiz_png/B77kSvewKqXRz1TazsVnVfXHaEO0ETqVSKiatlgs1a0tQtbAqTQkEAPYZYCv2dicYARkaImlJ7h3gRMp83EzoAFA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

	#### 方法参数

```java
//标注在方法，Spring容器创建当前对象，就会调用方法，完成赋值；
//方法使用的参数，自定义类型的值从ioc容器中获取
@Autowired 
public void setCar(Car car) {
	this.car = car;
}
```

#### 构造器

```java
@Autowired
public Boss(Car car){
    this.car = car;
    System.out.println("Boss...有参构造器");
}
```

#### ApplicationContextAware

```java
@Component
public class Red implements ApplicationContextAware,BeanNameAware,EmbeddedValueResolverAware {
	
	private ApplicationContext applicationContext;

	@Override
	public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
		// TODO Auto-generated method stub
		System.out.println("传入的ioc："+applicationContext);
		this.applicationContext = applicationContext;
	}

	@Override
	public void setBeanName(String name) {
		// TODO Auto-generated method stub
		System.out.println("当前bean的名字："+name);
	}

	@Override
	public void setEmbeddedValueResolver(StringValueResolver resolver) {
		// TODO Auto-generated method stub
		String resolveStringValue = resolver.resolveStringValue("你好 ${os.name} 我是 #{20*18}");
		System.out.println("解析的字符串："+resolveStringValue);
	}

}

```

### Aop:

![](https://mmbiz.qpic.cn/mmbiz_png/B77kSvewKqXRz1TazsVnVfXHaEO0ETqVibfY9TS7UxDiad6krBJ6sHE7kH8CicALibrykUIPU4qFzcqscBp83Csd5g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

`@EnableAspectJAutoProxy：`开启AOP功能会给容器中注册AnnotationAwareAspectJAutoProxyCreator组件AnnotationAwareAspectJAutoProxyCreator是一个后置处理器；

`@Before/@Aftert/@AfterReturning/@AfterThrowing/@Around`

> 前置通知(@Before)：logStart：在目标方法(div)运行之前运行
>
> 后置通知(@After)：logEnd：在目标方法(div)运行结束之后运行（无论方法正常结束还是异常结束）
>
> 返回通知(@AfterReturning)：logReturn：在目标方法(div)正常返回之后运行
>
> 异常通知(@AfterThrowing)：logException：在目标方法(div)出现异常以后运行
>
> 环绕通知(@Around)：动态代理，手动推进目标方法运行（joinPoint.procced()）

`@Pointcut`：切点

#### 基于注解版实现AOP

Aop 指在程序运行期间动态的将某段代码切入到指定方法指定位置进行运行的编程方式；

1. 导入aop模块；Spring AOP：(spring-aspects)

2. 定义一个业务逻辑类（MathCalculator）；在业务逻辑运行的时候将日志进行打印（方法之前、方法运行结束、方法出现异常，xxx）

3. 定义一个日志切面类（LogAspects）：切面类里面的方法需要动态感知MathCalculator.div运行到哪里然后执行；

4. 给切面类的目标方法标注何时何地运行（通知注解）；

5. 将切面类和业务逻辑类（目标方法所在类）都加入到容器中;

6. 必须告诉Spring哪个类是切面类(给切面类上加一个注解：@Aspect)

7. 给配置类中加 @EnableAspectJAutoProxy 【开启基于注解的aop模式】


```java
@EnableAspectJAutoProxy
@Configuration
public class MainConfigOfAOP {
	 
	//业务逻辑类加入容器中
	@Bean
	public MathCalculator calculator(){
		return new MathCalculator();
	}

	//切面类加入到容器中
	@Bean
	public LogAspects logAspects(){
		return new LogAspects();
	}
}
```

定义一个切面

```java
@Aspect
public class LogAspects {
	
	//抽取公共的切入点表达式
	//1、本类引用
	//2、其他的切面引用
	@Pointcut("execution(public int com.atguigu.aop.MathCalculator.*(..))")
	public void pointCut(){};
	
	//@Before在目标方法之前切入；切入点表达式（指定在哪个方法切入）
	@Before("pointCut()")
	public void logStart(JoinPoint joinPoint){
		Object[] args = joinPoint.getArgs();
		System.out.println(""+joinPoint.getSignature().getName()+"运行。。。@Before:参数列表是：{"+Arrays.asList(args)+"}");
	}
	
	@After("com.atguigu.aop.LogAspects.pointCut()")
	public void logEnd(JoinPoint joinPoint){
		System.out.println(""+joinPoint.getSignature().getName()+"结束。。。@After");
	}
	
	//JoinPoint一定要出现在参数表的第一位
	@AfterReturning(value="pointCut()",returning="result")
	public void logReturn(JoinPoint joinPoint,Object result){
		System.out.println(""+joinPoint.getSignature().getName()+"正常返回。。。@AfterReturning:运行结果：{"+result+"}");
	}
	
	@AfterThrowing(value="pointCut()",throwing="exception")
	public void logException(JoinPoint joinPoint,Exception exception){
		System.out.println(""+joinPoint.getSignature().getName()+"异常。。。异常信息：{"+exception+"}");
	}

}
public class MathCalculator {
	
	public int div(int i,int j){
		System.out.println("MathCalculator...div...");
		return i/j;	
	}

}

```

#### 基于注解的AOP原理

我们先看一下关于AOP的类图：
[![WCSJw6.png](https://z3.ax1x.com/2021/07/11/WCSJw6.png)](https://imgtu.com/i/WCSJw6)

##### @EnableAspectJAutoProxy

@EnableAspectJAutoProxy是开启进入AOP的代理的配置。我们可以看到在这个注解上使用@Import进行导入了

AspectJAutoProxyRegistrar，那么我们的分析都应该从这里出发。    

@Import(AspectJAutoProxyRegistrar.class)：给容器中导入AspectJAutoProxyRegistrar    

 利用AspectJAutoProxyRegistrar自定义给容器中注册bean；BeanDefinetion

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import({AspectJAutoProxyRegistrar.class})
public @interface EnableAspectJAutoProxy {
    boolean proxyTargetClass() default false;

    boolean exposeProxy() default false;
}
```

##### AspectJAutoProxyRegistrar

根据给定的EnableAspectJAutoProxy注释，根据当前的BeanDefinitionRegistry注册AnnotationAwareAspectJAutoProxyCreator 。

```java
class AspectJAutoProxyRegistrar implements ImportBeanDefinitionRegistrar {

	/**
	根据导入的@Configuration类上的@ EnableAspectJAutoProxy.proxyTargetClass()属性的值注册、升级和配置AspectJ 自动代理创建者
	**/
	@Override
	public void registerBeanDefinitions(
			AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {

	// AopConfigutils:用于处理 AOP 自动代理创建者注册的实用程序类。只能注册一个自动代理创建者，但有多个具体实现可用。 因此这个类包装了一个简单的升级协议，允许类请求一个特定的自动代理创建者并知道该类or a subclass thereof最终将驻留在应用程序上下文中
        AopConfigUtils.registerAspectJAnnotationAutoProxyCreatorIfNecessary(registry);
// 
		AnnotationAttributes enableAspectJAutoProxy =
				AnnotationConfigUtils.attributesFor(importingClassMetadata, EnableAspectJAutoProxy.class);
		if (enableAspectJAutoProxy.getBoolean("proxyTargetClass")) {
			AopConfigUtils.forceAutoProxyCreatorToUseClassProxying(registry);
		}
		if (enableAspectJAutoProxy.getBoolean("exposeProxy")) {
			AopConfigUtils.forceAutoProxyCreatorToExposeProxy(registry);
		}
	}

}
```

在AopConfigUtils.registerAspectJAnnotationAutoProxyCreatorIfNecessary()这个方法中我们可以看到

```java
public abstract class AopConfigUtils {

	/**
	 * 内部管理的自动代理创建者的 bean 名称.
	 */
	public static final String AUTO_PROXY_CREATOR_BEAN_NAME =
			"org.springframework.aop.config.internalAutoProxyCreator";

	/**
	 * 按升级顺序存储自动代理创建者类.
	 */
	private static final List<Class<?>> APC_PRIORITY_LIST = new ArrayList<Class<?>>();

	/**
	 * Setup the escalation list.
	 */
	static {
		APC_PRIORITY_LIST.add(InfrastructureAdvisorAutoProxyCreator.class);
		APC_PRIORITY_LIST.add(AspectJAwareAdvisorAutoProxyCreator.class);
		APC_PRIORITY_LIST.add(AnnotationAwareAspectJAutoProxyCreator.class);
	}

	// ... 省略
	public static BeanDefinition registerAspectJAnnotationAutoProxyCreatorIfNecessary(BeanDefinitionRegistry registry) {
		return registerAspectJAnnotationAutoProxyCreatorIfNecessary(registry, null);
	}

	public static BeanDefinition registerAspectJAnnotationAutoProxyCreatorIfNecessary(BeanDefinitionRegistry registry, Object source) {
		return registerOrEscalateApcAsRequired(AnnotationAwareAspectJAutoProxyCreator.class, registry, source);
	}
    // ...
}
```

在这里我们可以看到注入了AnnotationAwareAspectJAutoProxyCreator(处理器)所以我们应该往下面看这个类

##### AnnotationAwareAspectJAutoProxyCreator 

AspectJAwareAdvisorAutoProxyCreator子类，用于处理当前应用程序上下文中的所有 AspectJ 注释方面，以及 Spring Advisor。任何带有 AspectJ 注释的类都将被自动识别，并且如果 Spring AOP 的基于代理的模型能够应用它们的建议。 这涵盖了方法执行连接点。如果使用 <aop:include> 元素，则只有名称与包含模式匹配的 @AspectJ bean 才会被视为定义用于 Spring 自动代理的方面.

其实AnnotationAwareAspectJAutoProxyCreator就是一个后置处理器

我们一直向上找AnnotationAwareAspectJAutoProxyCreator 的父类

```java
AnnotationAwareAspectJAutoProxyCreator extends AspectJAwareAdvisorAutoProxyCreator {}
```

AnnotationAwareAspectJAutoProxyCreator.initBeanFactory()

AspectJAwareAdvisorAutoProxyCreator的父类AbstractAdvisorAutoProxyCreator

```java
AspectJAwareAdvisorAutoProxyCreator extends AbstractAdvisorAutoProxyCreator {}
```

AbstractAdvisorAutoProxyCreator的父类：

```java
public abstract class AbstractAdvisorAutoProxyCreator extends AbstractAutoProxyCreator {}
```

AbstractAdvisorAutoProxyCreator.setBeanFactory()->initBeanFactory()

AbstractAutoProxyCreator的父类

```java
public abstract class AbstractAutoProxyCreator extends ProxyProcessorSupport
		implements SmartInstantiationAwareBeanPostProcessor, BeanFactoryAware {
```

在AbstractAutoProxyCreator类中我们发现有setBeanFactory方法。

```java
@Override
public void setBeanFactory(BeanFactory beanFactory) {
    this.beanFactory = beanFactory;
}
```

**AnnotationAwareAspectJAutoProxyCreator创建注册的整个过程如下：**

AnnotationAwareAspectJAutoProxyCreateor其实就是这个(InstantiationAwareBeanPostProcessor)类型的处理器.

1. 传入配置类，创建ioc容器

2. 注册配置类，调用refresh（）刷新容器；

3. registerBeanPostProcessors(beanFactory);注册bean的后置处理器来方便拦截bean的创建；

   1. 先获取ioc容器已经定义了的需要创建对象的所有BeanPostProcessor*        

   2. 给容器中加别的BeanPostProcessor

   3. 优先注册实现了PriorityOrdered接口的BeanPostProcessor；

   4. 再给容器中注册实现了Ordered接口的BeanPostProcessor；

   5. 注册没实现优先级接口的BeanPostProcessor；

   6. 注册BeanPostProcessor，实际上就是创建BeanPostProcessor对象，保存在容器中；

      创建internalAutoProxyCreator的BeanPostProcessor【AnnotationAwareAspectJAutoProxyCreator

      1. 创建Bean的实例
      2. populateBean；给bean的各种属性赋值
      3. initializeBean：初始化bean；
      4. BeanPostProcessor(AnnotationAwareAspectJAutoProxyCreator)创建成功；--》aspectJAdvisorsBuilder*
         1.  invokeAwareMethods()：处理Aware接口的方法回调
         2. applyBeanPostProcessorsBeforeInitialization()：应用后置处理器的postProcessBeforeInitialization（）
         3. nvokeInitMethods()；执行自定义的初始化方法
         4. applyBeanPostProcessorsAfterInitialization()；执行后置处理器的postProcessAfterInitialization（）；

   7. 把BeanPostProcessor注册到BeanFactory中；        beanFactory.addBeanPostProcessor(postProcessor);           



### 声明式事务：

`@EnableTransctionManagement：` 开启基于注解的事务管理功能；

`@Transactional`: 给方法上标注 @Transactional 表示当前方法是一个事务方法；

### SpringMVC注解：

除了spring的基本注解你还可以使用下面的注解

`@EnableWebMvc:`  在配置类中开启Web MVC的配置支持，如一些ViewResolver或者MessageConverter等，若无此句，重写WebMvcConfigurerAdapter方法（用于对SpringMVC的配置）

`@RequestMapping("item/{id}")`  声明请求的url{xxx}叫做占位符，请求的URL可以是“item /1”或“item/2”

`@PathVariable `获取url上的数据

```java
使用(@PathVariable() Integer id)获取url上的数据		
/**
* 使用RESTful风格开发接口，实现根据id查询商品
* 如果不一致，例如"item/{ItemId}"则需要指定名称@PathVariable("itemId")。
* @param id
* @return
*/
@RequestMapping("item/{id}")
@ResponseBody
public Item queryItemById(@PathVariable() Integer id) {
    Item item = this.itemService.queryItemById(id);
    return item;
}
			
```
`@PathVariable`:  是 获取 url 上数据 的。 @RequestParam获取请求参数的（包括post表单提交）

`@ResponseBody` :使用这个注解就不会走视图解析器，不会返回页面，目前返回的json数据。如果不加，就走视图解析器，返回页面

`@RequestBody`：  允许request的参数在request体中，而不是在直接连接在地址后面。（放在参数前） 需要和postMapping一起使用

`@NumberFormat` ：   支持对数字类型的属性使用 @NumberFormat 注解

`@DateTimeFormat`： – JodaDateTimeFormatAnnotationFormatterFactroy：支持对日期类型的属性使用 @DateTimeFormat 注解 ；可以对pattern 属性：类型为字符串。指定解析/格式化字段数据的模式，如：”yyyy-MM-dd hh:mm:ss”等其他的

`@RequestParam(name="file") `:接收前端传入的参数 required=false或者true来要@RequestParam配置的前端参数是否一定要传 