# Spring Data&JPA:

## JPA概述

Java Persistence API：用于对象持久化的 API，规范，

Java EE 5.0 平台标准的 ORM 规范，使得应用程序以统一的方式访问持久层

![1569768122267](C:\Users\leoill\AppData\Roaming\Typora\typora-user-images\1569768122267.png)

### JPA和Hibernate

JPA 是 hibernate 的一个抽象（就像JDBC和JDBC驱动的关系）：

–**JPA** **是规范**：JPA 本质上就是一种  ORM 规范，不是ORM 框架 —— 因为 JPA 并未提供 ORM 实现，它只是制订了一些规范，提供了一些编程的 API 接口，但具体实现则由 ORM 厂商提供实现

–**Hibernate** **是实现**：Hibernate 除了作为 ORM 框架之外，它也是一种 JPA 实现

•从功能上来说， **JPA** **是** **Hibernate** 功能的一个子集

•JPA 的目标之一是**制定一个可以由很多供应商实现****的**
**API**，目前Hibernate3.2+、TopLink10.1+以及OpenJPA都提供了JPA 的实现

#### JPA优势

•**标准化**:  提供相同的 API，这保证了基于JPA 开发的企业应用能够经过少量的修改就能够在不同的 JPA 框架下运行。

•简单易用，集成方便:  JPA 的主要目标之一就是提供更加简单的编程模型，在 JPA 框架下创建实体和创建 Java  类一样简单，只需要使用 javax.persistence.Entity 进行注释；JPA 的框架和接口也都非常简单，

•**可媲美JDBC的查询能力**:  JPA的查询语言是面向对象的，JPA定义了独特的**JPQL**，而且能够支持批量更新和修改、JOIN、GROUP BY、HAVING 等通常只有 SQL 才能够提供的高级查询特性，甚至还能够支持子查询。

•**支持面向对象的高级特性**: JPA 中能够支持面向对象的高级特性，如类之间的继承、多态和类之间的复杂关系，最大限度的使用面向对象的模型

#### JPA的技术

•**ORM**  **映射元数据**：JPA 支持 **XML** **和**  **JDK 5.0** **注解**两种元数据的形式，元数据描述对象和表之间的映射关系，框架据此将实体对象持久化到数据库表中。  

•**JPA** **的** **API**：用来操作实体对象，执行CRUD操作，框架在后台完成所有的事情，开发者从繁琐的 JDBC和 SQL代码中解脱出来。  

•**查询语言（JPQL）**：这是持久化操作中很重要的一个方面，通过面向对象而非面向数据库的查询语言查询数据，避免程序和具体的  SQL 紧密耦合。

### 使用JPA持久化对象的步骤

•**创建** **persistence.xml**, **在****这个文件中配置持久化****单元**

–需要指定跟哪个数据库进行交互;

–需要指定 JPA 使用哪个持久化的框架以及配置该框架的基本属性

•**创建实体类 使用** annotation来描述实体类跟数据库表之间**的映射关系**.

•**使用** **JPA API** **完成数据增加、删除、修改和查询操作**

–**创建** **EntityManagerFactory** (对应 Hibernate 中的 SessionFactory);

–**创建** **EntityManager** (对应 Hibernate 中的Session);

### JPA注解

#### @Entity

标注用于实体类声明语句之前，**指出该Java为实体类**，**将映射到指定的数据库表**。如声明一个实体类Customer，它将映射到数据库中的customer表上。

#### @Table

•**当实体类与其映射的数据库表名不同名时**需要使用 **@Table** 标注说明，该标注**与** **@Entity** **标注****并列使用**，置于实体类声明语句之前，可写于单独语句行，也可与声明语句同行。

•@Table 标注的常用选项是 **name**，用于指明数据库的表名

@Table选项 和 用于uniqueConstraints选须设置

#### @Id

•@Id 标注用于声明一个实体类的属性映射为数据库的**主键列**。该属性通常置于属性声明语句之前，可与声明语句同行，也可写在单独行上。

•@Id标注也可置于属性的**getter方法之前**。

#### @GeneratedValue

•**@GeneratedValue **用于标注主键的生成**策略**，**通过 **strategy 属性指定**。**默认情况下，**JPA自动选择一个最适合底层数据库的主键生成策略**：SqlServer 对应 identity，MySQL 对应 auto increment。

•在 javax.persistence.GenerationType 中定义了以下几种可供选择的策略：

–**IDENTITY**：采用数据库 ID自增长的方式来自增主键字段，Oracle 不支持这种方式；

–**AUTO**:JPA自动选择合适的策略，是默认选项；

–**SEQUENCE**：通过序列产生主键，通过 **@SequenceGenerator** 注解指定序列名，MySql 不支持这种方式

**TABLE**：通过表产生主键，框架借由表模拟序列产生主键，使用该策略可以使应用更易于移植

#### @Basic

•@Basic 表示一个简单的属性到数据库表的字段的映射,对于**没有任何标注的 getXxxx()方法**,默认即为**@Basic**

•fetch: 表示该属性的读取策略,有 EAGER 和 LAZY 两种,分别表示主支抓取和延迟加载,默认为 EAGER.

optional:表示该属性是否允许为null默认true

#### @Column

•**当实体的属性与其映射的数据库表的列不同名时需要使用@Column **标注说明，该属性通常置于实体的属性声明语句之前，还可与 @Id 标注一起使用。

•@Column 标注的常用属性是 name，用于设置映射数据库表的列名。此外，该标注还包含其它多个属性，如：**unique** 、**nullable**、**length** 等。

•@Column 标注的 columnDefinition 属性: **表示该字段在数据库中的实际类型**.通常 ORM 框架可以根据属性类型自动判断数据库中字段的类型,但是对于Date类型仍无法确定数据库中字段类型究竟是DATE,TIME还是TIMESTAMP.此外,String的默认映射类型为VARCHAR, 如果要将 String 类型映射到特定数据库的 BLOB 或TEXT 字段类型.

•@Column标注也可置于属性的getter方法之前

#### **@Transient**

•表示该属性并非一个到数据库表的字段的映射,ORM框架将忽略该属性

**如果一个属性并非数据库表的字段映射就务必将其标示为否则*框架默认其注解为@Basic**



## jPA中的API:

​	Persistence  类是用于获取 EntityManagerFactory 实例。该类包含一个名为 createEntityManagerFactory 的 静态方法 。

createEntityManagerFactory 方法有如下两个重载版本。
		带有一个参数的方法以 JPA 配置文件 persistence.xml 中的持久化单元名为参数

EntityManagerFactory 接口主要用来创建 EntityManager 实例。该接口约定了如下4个方法

​		createEntityManager()：用于创建实体管理器对象实例。
​		createEntityManager(Map map)：用于创建实体管理器对象实例的重载方法，Map 参数用于提供 EntityManager 的属性。
​		1.isOpen()：检查 EntityManagerFactory 是否处于打开状态。实体管理器工厂创建后一直处于打开状态，除非调用close()方法将其关闭。
​		2.close()：关闭 EntityManagerFactory 。 EntityManagerFactory 关闭后将释放所有资源，isOpen()方法测试将返回 false，其它方法将不能调用，否则将导致IllegalStateException异常。, EntityManager 是完成持久化操作的核心对象。实体作为普通 Java 对象，只有在调用 EntityManager 将其持久化后才会变成持久化对象。EntityManager 对象在一组实体类与底层数据源之间进行 O/R 映射的管理。它可以用来管理和更新 Entity Bean, 根椐主键查找 Entity Bean, 还可以通过JPQL语句查询实体。
​		3.persist (Object entity)：用于将新创建的 Entity 纳入到 EntityManager 的管理。该方法执行后，传入 persist() 方法的 Entity 对象转换成持久化状态。
​		4.remove (Object entity)：删除实例。如果实例是被管理的，即与数据库实体记录关联，则同时会删除关联的数据库记录
​		5.flush ()：同步持久上下文环境，即将持久上下文环境的所有未保存实体的状态信息保存到数据库中。
​		6.setFlushMode (FlushModeType flushMode)：设置持久上下文环境的Flush模式。参数可以取2个枚举
​					FlushModeType.AUTO 为自动更新数据库实体，
​					FlushModeType.COMMIT 为直到提交事务时才更新数据库记录。
​		7.getFlushMode ()：获取持久上下文环境的Flush模式。返回FlushModeType类的枚举值。
​		8.refresh (Object entity)：用数据库实体记录的值更新实体对象的状态，即更新实例的属性值。
​		9.clear ()：清除持久上下文环境，断开所有关联的实体。如果这时还有未提交的更新则会被撤消。
​		10.contains (Object entity)：判断一个实例是否属于当前持久上下文环境管理的实体。
​		11.isOpen ()：判断当前的实体管理器是否是打开状态。
​		12.getTransaction ()：返回资源层的事务对象。EntityTransaction实例可以用于开始和提交多个事务。
​		13,close ()：关闭实体管理器。之后若调用实体管理器实例的方法或其派生的查询对象的方法都将抛出 IllegalstateException 异常，除了getTransaction 和 isOpen方法(返回 false)。不过，当与实体管理器关联的事务处于活动状态时，调用 close 方法后持久上下文将仍处于被管理状态，直到事务完成。
​				createQuery (String qlString)：创建一个查询对象。
​				createNamedQuery (String name)：根据命名的查询语句块创建查询对象。参数为命名的查询语句。
​				createNativeQuery (String sqlString)：使用标准 SQL语句创建查询对象。参数为标准SQL语句字符串。
​				createNativeQuery (String sqls, String resultSetMapping)：使用标准SQL语句创建查询对象，并指定返回结果集 Map的 名称
​		EntityTransaction 接口用来管理资源层实体管理器的事务操作。通过调用实体管理器的getTransaction方法 获得其实例
​				

- begin ()用于启动一个事务，此后的多个数据库操作将作为整体被提交或撤消。若这时事务已启动则会抛出 IllegalStateException 异常。

- commit ()用于提交当前事务。即将事务启动以后的所有数据库更新操作持久化至数据库中。
  				

- rollback ()撤消(回滚)当前事务。即撤消事务启动后的所有数据库更新操作，从而不对数据库产生影响。

- setRollbackOnly ()使当前事务只能被撤消。

- getRollbackOnly ()查看当前事务是否设置了只能撤消标志

- isActive ()查看当前事务是否是活动的。如果返回true则不能调用begin方法，否则将抛出 IllegalStateException 异常；如果返回 false 则不能调用 commit、rollback、setRollbackOnly 及 getRollbackOnly 方法，否则将抛出 IllegalStateException 异常

  ### 映射关系：

  ​				双向一对多关系中，必须存在一个关系维护端，在 JPA 规范中，要求  many 的一方作为关系的维护端(owner side), one 的一方作为被维护端(inverse side)。
  ​				可以在 one 方指定 @OneToMany 注释并设置 mappedBy 属性，以指定它是这一关联中的被维护端，many 为维护端。
  ​				在 many 方指定 @ManyToOne 注释，并使用 @JoinColumn 指定外键名称
  ​				@OrderBy("OREDER_NAME")
  ​				@OneToMany(targetEntity=Order.class,mappedBy="customer")
  ​				public Set<Order> getOrders(){
  ​					return orders;
  ​				}
  ​				@JoinColumn(name="Customer_ID")
  ​				@ManyToOne(targetEntity=Customer.class)
  ​				public Customer getCustomer(){
  ​					return customer;
  ​				}

  #### 双向一对一映射：

  ​				在双向的一对一关联中，需要在关系被维护端(inverse side)中的 @OneToOne 注释中指定 mappedBy，以指定是这一关联中的被维护端。
  ​				同时需要在关系维护端(owner side)建立外键列指向关系被维护端的主键列。
  ​				@OneToOne(mappedBy="mgr")
  ​				public department getDept(){
  ​					return dept;
  ​				}
  ​				@joinColumn(name="MGR_Id",unique=true)
  ​				@OneToOne(fetch=FetchType.LAZY)
  ​				public Manager getMgr(){
  ​					return mgr;
  ​				}

  #### 双向多对多：指定一个维护端

  ​				@ManyToMany
  ​				@JoinTable(name="中间表名称",
  ​				joinColumns=@joinColumn(name="本类的外键",
  ​				referencedColumnName="本类与外键对应的主键"),
  ​				inversejoinColumns=@JoinColumn(name="对方类的外键",
  ​				referencedColunName="对方类与外键对应的主键")
  ​				)
  ​				public Set<Item> getItems(){
  ​					return items;
  ​				}
  ​				另一个维护端：
  ​				@ManyToMany(mappedBy='' items")
  ​				public Set<Category> getCategories(){
  ​					return categories;
  ​				}

### JPQL:

JPQL语言，即 Java Persistence Query Language 的简称。JPQL 是一种和 SQL 非常类似的中间性和对象化查询语言，它最终会被编译成针对不同底层数据库的 SQL 查询，从而屏蔽不同数据库的差异。
JPQL语言的语句可以是 select 语句、update 语句或delete语句，它们都通过 Query 接口封装执行

#### Query接口：

​	调用EntityManager的createQuery,create NamedQuery 及 createNativeQuery 方法可以获得查询对象，
​		

- setHint(String hintName, Object value) 设置与查询对象相关的特定供应商参数或提示信息。参数名及其取值需要参考特定 JPA 实现库提供商的文档。如果第二个参数无效将抛出IllegalArgumentException异常
- setParameter(int position, Object value) 为查询语句的指定位置参数赋值。Position 指定参数序号，value 为赋给参数的值。
- setParameter(int position, Date d, TemporalType type) 为查询语句的指定位置参数赋 Date 值。Position 指定参数序号，value 为赋给参数的值，temporalType 取 TemporalType 的枚举常量，包括 DATE、TIME 及 TIMESTAMP 三个，用于将 Java 的 Date 型值临时转换为数据库支持的日期时间类型（java.sql.Date、java.sql.Time及java.sql.Timestamp）。
- setParameter(int position, Calendar c, TemporalType type) 为查询语句的指定位置参数赋 Calenda r值。position 指定参数序号，value 为赋给参数的值，temporalType 的含义及取舍同前。
- setParameter(String name, Object value) 为查询语句的指定名称参数赋值。
- setParameter(String name, Date d, TemporalType type) 为查询语句的指定名称参数赋 Date 值。用法同前。
- setParameter(String name, Calendar c, TemporalType type) 为查询语句的指定名称参数设置Calendar值。name为参数名，其它同前。该方法调用时如果参数位置或参数名不正确，或者所赋的参数值类型不匹配，将抛出 IllegalArgumentException 异常。

#### 语句：

		##### 查询

```
		select o from Order o 或  select o from Order as o
		Query query = entityManager.createQuery( "select o from Order o"); 
		List orders = query.getResultList();
		Iterator iterator = orders.iterator();
		while( iterator.hasNext() ) {
			// 处理Order
		}
```

​		JPQL也支持包含参数的查询，例如：

```
select o from Orders o where o.id = :myId
		select o from Orders o where o.id = :myId and o.customer = :customerName
```

​	　注意：参数名前必须冠以冒号(:)，执行查询前须使用Query.setParameter(name, value)方法给参数赋值。
​		也可以不使用参数名而使用参数的序号，例如：
​		select o from Order o where o.id = ?1 and o.customer = ?2
​		其中 ?1 代表第一个参数，?2 代表第一个参数。在执行查询之前需要使用重载方法Query.setParameter(pos, value) 提供参数值。

```
Query query = entityManager.createQuery( "select o from 　	Orders o where o.id = ?1 and o.customer = ?2" );
		query.setParameter( 1, 2 );
		query.setParameter( 2, "John" );
		List orders = query.getResultList();
```

##### group by字句

```
Query.getSingleResult()得到查询结果。例如：
		Query query = entityManager.createQuery(
							"select max(o.id) from Orders o");
		Object result = query.getSingleResult();
		Long max = (Long)result;
```

##### having子句：		

```
例如，以下语句用于查询订购总数大于100的商家所售商品及数量：
			select o.seller, o.goodId, sum(o.amount) from V_Orders o group by 
			o.seller, o.goodId having sum(o.amount) > 100
			having子句与where子句一样都可以使用参数。
```

##### update语句用于执行更新主要对于单个实体类的批量更新：

```
update Customers c set c.status = '未偿付' where c.balance < 10000
```

#### delete语句：


#### JPQL中提供了内置的函数：

- concat(String s1, String s2)：字符串合并/连接函数。
- substring(String s, int start, int length)：取字串函数。
- trim([leading|trailing|both,] [char c,] String s)：从字符串中去掉首/尾指定的字符或空格。
- lower(String s)：将字符串转换成小写形式。
- upper(String s)：将字符串转换成大写形式。
- length(String s)：求字符串的长度。
- locate(String s1, String s2[, int start])：从第一个字符串中查找第二个字符串(子串)出现的位置。若未找到则返回0。



## Spring整合JPA

spring整合JPA三种方式：

- 1.LocalEntityManagerFactoryBean：适用于那些仅使用 JPA 进行数据访问的项目，该 FactoryBean 将根据JPA PersistenceProvider 自动检测配置文件进行工作，一般从“META-INF/persistence.xml”读取配置信息，这种方式最简单，但不能设置 Spring 中定义的DataSource，且不支持 Spring 管理的全局事务
- 2.从JNDI中获取：用于从 Java EE 服务器获取指定的EntityManagerFactory，这种方式在进行 Spring 事务管理时一般要使用 JTA 事务管理
- 3.LocalContainerEntityManagerFactoryBean：适用于所有环境的 FactoryBean，能全面控制 EntityManagerFactory 配置,如指定 Spring 定义的 DataSource 等等。

	
		一：
		<!-- 配置 JPA 提供者的适配器 -->
		<bean id="jpaVendorAdapter"  	class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
			<property name="databasePlatform">
				<bean class="com.atguigu.ssps.modules.persistence.Hibernates" 
					factory-method="getDialect">
						<constructor-arg ref="dataSource"></constructor-arg>
				</bean>
			</property>
		</bean>
		二：
			<!-- 配置 JPA 的 EntityManager -->
			<bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
				<property name="dataSource" ref="dataSource"></property>
				<property name="jpaVendorAdapter" ref="jpaVendorAdapter"></property>
				<property name="packagesToScan" value="com.atuigu.crm"></property>
				<property name="jpaProperties">
					<props>
						<!-- 二级缓存相关 -->
						<prop key="hibernate.cache.region.factory_class">
							org.hibernate.cache.ehcache.EhCacheRegionFactory</prop>
						<prop key="net.sf.ehcache.configurationResourceName">
							ehcache-hibernate.xml</prop>
						<!-- 生成的数据表的列的映射策略 -->
						<prop key="hibernate.ejb.naming_strategy">
							org.hibernate.cfg.ImprovedNamingStrategy</prop>
						<!-- hibernate 基本属性 -->
						<prop key="hibernate.show_sql">true</prop>
						<prop key="hibernate.format_sql">true</prop>
						<prop key="hibernate.hbm2ddl.auto">update</prop>
					</props>
				</property>
			</bean>
### DEMO

```
package com.leo.jpa.test;

import java.util.Date;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;

import org.hibernate.Hibernate;
import org.junit.After;
import org.junit.Before;
import org.junit.jupiter.api.Test;

import com.leo.jpa.Customer;
import com.leo.jpa.Order;

public class TestJPA {
	private EntityManagerFactory factory;
	private EntityManager manager;
	private EntityTransaction transaction;
	@Before
	public void intit() {
		factory=Persistence.createEntityManagerFactory("JpaDemo");
		manager=factory.createEntityManager();
		transaction=manager.getTransaction();
		transaction.begin();
	}
	@After
	public void destory() {
		transaction.commit();
		manager.close();
		factory.close();
	}
	//类似于hibernate中的saveOrUpdate方法：
	@Test
	public void testMerge1() {
		Customer customer=new Customer();
		customer.setAge("12");
		customer.setEmail("1233@163.com");
		customer.setBirth(new Date());
		customer.setCreateTime(new Date());
		customer.setLastName("ccc");
		//manger是会先创建出来一个新的对象，把临时对象的属性复制到新的对象中
		//新的对象中会有id但是以前的临时对象中没有id;
		Customer customer2 = manager.merge(customer);
		
		System.out.println("customer#id:"+customer.getId());
		System.out.println("customer2#id:"+customer2.getId());
	}
	/**
	 * 传入的是游离对象即有对象的id
	 * 若在缓存中没有对象
	 * 在数据库中也没有对应的记录
	 * JPA会创建一个对象，然后把游离的对象复制到新创建的对象
	 * 执行insert对象
	 */
	@Test
	public void testMerge2() {
		Customer customer=new Customer();
		customer.setAge("12");
		customer.setEmail("dd@163.com");
		customer.setBirth(new Date());
		customer.setCreateTime(new Date());
		customer.setLastName("ddd");
		customer.setId(100);
		Customer customer2 = manager.merge(customer);
		
		System.out.println("customer#id:"+customer.getId());
		System.out.println("customer2#id:"+customer2.getId());
	}
	/**
	 * 传入的是游离对象即有对象的id
	 * 若在缓存中没有对象
	 * 在数据库中有对应的记录
	 * JPA会查询对应的记录，然后返回该记录的一个对象再然后把游离的对象的属性复制到查询到的对象中
	 * 执行update
	 */
	@Test
	public void testMerge3() {
		Customer customer=new Customer();
		customer.setAge("12");
		customer.setEmail("dd@163.com");
		customer.setBirth(new Date());
		customer.setCreateTime(new Date());
		customer.setLastName("ddd");
		customer.setId(2);
		Customer customer2 = manager.merge(customer);
		System.out.println(customer==customer2);
		System.out.println("customer#id:"+customer.getId());
		System.out.println("customer2#id:"+customer2.getId());
	}
	/**
	 * 传入的是游离对象即有对象的id
	 * 若在缓存中有对象
	 * JPA会把游离的对象的属性复制到缓存中的对象
	 * 对缓存中的对象执行update
	 */
	@Test
	public void testMerge4() {
		Customer customer=new Customer();
		customer.setAge("12");
		customer.setEmail("dd@163.com");
		customer.setBirth(new Date());
		customer.setCreateTime(new Date());
		customer.setLastName("ddd");
		customer.setId(4);
		//这个是查询出的
		Customer customer2 = manager.find(Customer.class, 4);
		manager.merge(customer);
		System.out.println(customer==customer2);//false
		System.out.println("customer#id:"+customer.getId());
		System.out.println("customer2#id:"+customer2.getId());
	}
	
	//类似于hibernate中session的load方法
	@Test
	public void  getRefe() {
		// TODO Auto-generated method stub
		Customer customer=manager.find(Customer.class, 1);
		System.out.println("---");
		System.out.println(customer.getClass().getName());
	}
	//类似于hibernate 中的session的get方法
	@Test
	public void testFind() {
		Customer customer=manager.find(Customer.class, 1);
		System.out.println("---");
		System.out.println(customer);
	}
	//类似于hibernate中的save方法若对象中的有设置id则会出现异常错诶，不能执行insert方法
	@Test
	public void testPersistence() {
		Customer customer=new Customer();
		customer.setAge("11");
		customer.setEmail("12233@163.com");
		customer.setBirth(new Date());
		customer.setCreateTime(new Date());
		manager.persist(customer);
		System.out.println(customer.getId());
	}
	//类似于hibernate的delete方法，把对象对应的记录从数据库中移除
	//该方法只能移除持久化的对象，不能移除游离对象
	public void testDelete() {
		//这样会报错
//		Customer customer=new Customer();
//		Hibernate中可以这样使用
//		manager.remove(customer);
		//
		Customer customer=manager.find(Customer.class, 1);
		manager.remove(customer);
	}
	//和hibernate中的session的flush方法一致
	@Test
	public void testFlush() {
		Customer customer=manager.find(Customer.class, 1);
		System.err.println(customer);
		customer.setLastName("AA");
		//强制发送一条sql让缓存中的数据和数据库中的一致
		manager.flush();
	}
	/**
	 * 保存多对一时先保存1的一端后保存多的一端，这样不会多出额外的Update语句
	 */
	public void testManyToOne() {
		Customer customer=new Customer();
		customer.setAge("11");
		customer.setEmail("12233@163.com");
		customer.setBirth(new Date());
		customer.setCreateTime(new Date());
		Order order=new Order();
		order.setOrderName("O_F_1");
		Order order2=new Order();
		order2.setOrderName("O_F_2");
		//设置关联关系
		order.setCustomer(customer);
		order2.setCustomer(customer);
		//执行保存
		manager.persist(customer);
		manager.persist(order);
		manager.persist(order2);
	}
	//不能直接删除1的一端，因为有外键约束
	//只能删除多的一方
	public void testManyToOnewRemove() {
//		Order order=manager.find(Order.class, 1);
//		manager.remove(order);
		Customer customer =manager.find(Customer.class, 1);
		manager.remove(customer);
	}
	/**
	 * 默认情况下使用左外连接来获取n的一方的对象和其关联的1的一方对象
	 * 可以使用@ManyToOne 的fetch属性来修改默认的关联属性的加载测略
	 */
	public void testManyToOneFind() {
		Order order=manager.find(Order.class, 1);
		System.out.println(order.getOrderName());
		System.out.println(order.getCustomer().getLastName());
	}
}
```



## SpringData简述

•Spring Data : Spring 的一个子项目。用于简化数据库访问，支持**NoSQL** 和 **关系数据存储**。其主要目标是使数据库的访问变得方便快捷。

•SpringData 项目所支持 **NoSQL** **存储**：

–MongoDB （文档数据库）

–Neo4j（图形数据库）

–Redis（键/值存储）

–Hbase（列族数据库）

•SpringData 项目所支持的**关系数据存储技术**：

JDBC      JPA

## JPA Spring Data 概述

•JPA Spring Data : 致力于减少数据访问层 (DAO) 的开发量. 开发者唯一要做的，就只是**声明持久层的接口**，其他都交给 Spring Data JPA 来帮你完成！

•框架怎么可能代替开发者实现业务逻辑呢？比如：当有一个 UserDao.findUserById()  这样一个方法声明，大致应该能判断出这是根据给定条件的 ID 查询出满足条件的 User  对象。Spring Data JPA 做的便是规范方法的名字，根据符合规范的名字来确定方法需要实现什么样的逻辑。

### 如何使用Spring Data JPA 呢

–**配置** **Spring** **整合** **JPA**

–**在** **Spring** **配置文件中配置** **Spring Data****，**让 Spring 为声明的接口创建代理对象。配置了 <jpa:repositories> 后，Spring 初始化容器时将会扫描 base-package  指定的包目录及其子目录，为继承 Repository 或其子接口的接口创建代理对象，并将代理对象注册为 Spring Bean，业务层便可以通过 Spring 自动封装的特性来直接使用该对象。

–**声明持久层的接口，该接口继承**  **Repository**，Repository 是一个标记型接口，它不包含任何方法，如必要，Spring Data 可实现 Repository 其他子接口，其中定义了一些常用的增删改查，以及分页相关的方法。

–**在接口中声明需要****的方****法**。Spring Data 将根据给定的策略（具体策略稍后讲解）来为其生成实现代码。

### Repository接口简介：

•Repository 接口是 Spring Data 的一个核心接口，它不提供任何方法，开发者需要在自己定义的接口中声明需要的方法 

​    **public** **interface Repository<T, ID extends** **Serializable****> { }** 

•Spring Data可以让我们只定义接口，只要遵循 **Spring Data****的规范**，就无需写实现类。  

•**与****继承** **Repository** **等价的一种方式，就是在持久层接口上使用** **@****RepositoryDefinition** **注解**，并为其指定 domainClass 和 idClass 属性。如下两种方式是完全等价的

•基础的 Repository 提供了最基本的数据访问功能，其几个子接口则扩展了一些功能。它们的继承关系如下： 

–**Repository**： 仅仅是一个标识，表明任何继承它的均为仓库接口类

–**CrudRepository**： 继承 Repository，实现了一组 CRUD 相关的方法 

–**PagingAndSortingRepository**： 继承 CrudRepository，实现了一组分页排序相关的方法 

–**JpaRepository**： 继承 PagingAndSortingRepository，实现一组 JPA 规范相关的方法 

–**自定义的** **XxxxRepository** 需要继承 JpaRepository，这样的 XxxxRepository 接口就具备了通用的数据访问控制层的能力。

–**JpaSpecificationExecutor**： 不属于Repository体系，实现一组 JPA **Criteria** 查询相关的方法 

## Spring Data方法定义规范：

### 简单条件查询

•简单条件查询: 查询某一个实体类或者集合 

•**按照** **Spring** **Data** **的规范**，查询方法以 **find | read | get** 开头， 
 **涉及条件查询时，条件的属性用条件关键字连接，要注意的是：条件属性以首字母****大写**。 

•例如：定义一个 Entity 实体类 
 class User｛ 
   private String firstName; 
   private String lastName; 
 ｝ 
 使用And条件连接时，应这样写： 
 findByLastNameAndFirstName(String lastName,String firstName); 
 条件的属性名称与个数要与参数的位置与个数一一对应 

#### 支持的关键字

![1569658585843](C:\Users\leoill\AppData\Roaming\Typora\typora-user-images\1569658585843.png)

![1569658602100](C:\Users\leoill\AppData\Roaming\Typora\typora-user-images\1569658602100.png)

#### 查询方法解析

•findBy**UserDepUuid**()，框架在解析该方法时，首先剔除 findBy，然后对剩下的属性进行解析，假设查询实体为Doc

–先判断 **userDepUuid** （根据 POJO 规范，首字母变为小写）是否为查询实体的一个属性，如果是，则表示根据该属性进行查询；如果没有该属性，继续第二步；

–从右往左截取第一个大写字母开头的字符串(此处为**Uuid**)，然后检查剩下的字符串是否为查询实体的一个属性，如果是，则表示根据该属性进行查询；如果没有该属性，则重复第二步，继续从右往左截取；最后假设 user 为查询实体的一个属性；

–接着处理剩下部分（DepUuid），先判断 user 所对应的类型是否有depUuid属性，如果有，则表示该方法最终是根据 “ Doc.user.depUuid” 的取值进行查询；否则继续按照步骤 2 的规则从右往左截取，最终表示根据 “**Doc.user.dep.uuid**” 的值进行查询。

–可能会存在一种特殊情况，比如 Doc包含一个 user 的属性，也有一个 userDep 属性，此时会存在混淆。**可以明确在属性之间加上** **"_"** **以显式表达意图**，比如 "findByUser_DepUuid()" 或者 "findByUserDep_uuid()"

•特殊的参数： 还可以直接在方法的参数上加入分页或排序的参数，比如：

–Page<UserModel> findByName(String name, Pageable pageable);

–List<UserModel> findByName(String name, Sort sort);

@Query
与
@Modifying
执行更新操作

### CrudRepository接口

•CrudRepository 接口提供了最基本的对实体类的添删改查操作 

–T save(T entity);//保存单个实体 

–Iterable<T> save(Iterable<? extends T> entities);//保存集合        

–T findOne(ID id);//根据id查找实体         

–boolean exists(ID id);//根据id判断实体是否存在         

–Iterable<T> findAll();//查询所有实体,不用或慎用!         

–long count();//查询实体数量         

–void delete(ID id);//根据Id删除实体         

–void delete(T entity);//删除一个实体 

–void delete(Iterable<? extends T> entities);//删除一个实体的集合         

–void deleteAll();//删除所有实体,不用或慎用! 

### PagingAndSortingRepository接口

•该接口提供了分页与排序功能 

–Iterable<T> findAll(Sort sort); //排序 

–Page<T> findAll(Pageable pageable); //分页查询（含排序功能） 

### JpaRepository接口

•该接口提供了JPA的相关功能 

–List<T> findAll(); //查找所有实体 

–List<T> findAll(Sort sort); //排序、查找所有实体 

–List<T> save(Iterable<? extends T> entities);//保存集合 

–void flush();//执行缓存与数据库同步 

–T saveAndFlush(T entity);//强制执行持久化 

–void deleteInBatch(Iterable<T> entities);//删除一个实体集合 

### JpaSpecificationExecutor接口

•不属于Repository体系，实现一组 JPA **Criteria** 查询相关的方法 

![1569765381258](C:\Users\leoill\AppData\Roaming\Typora\typora-user-images\1569765381258.png)

•Specification：封装  JPA Criteria 查询条件。通常使用匿名内部类的方式来创建该接口的对象

