# Spring Data:

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

–**在** **Spring** **配置文件中配置** **Spring Data**，让 Spring 为声明的接口创建代理对象。配置了 <jpa:repositories> 后，Spring 初始化容器时将会扫描 base-package  指定的包目录及其子目录，为继承 Repository 或其子接口的接口创建代理对象，并将代理对象注册为 Spring Bean，业务层便可以通过 Spring 自动封装的特性来直接使用该对象。

–**声明持久层的接口，该接口继承**  **Repository**，Repository 是一个标记型接口，它不包含任何方法，如必要，Spring Data 可实现 Repository 其他子接口，其中定义了一些常用的增删改查，以及分页相关的方法。

–**在接口中声明需要的方法**。Spring Data 将根据给定的策略（具体策略稍后讲解）来为其生成实现代码。

### Repository接口简介：

•Repository 接口是 Spring Data 的一个核心接口，它不提供任何方法，开发者需要在自己定义的接口中声明需要的方法 

​    **public** **interface Repository<T, ID extends** **Serializable> { }**

•Spring Data可以让我们只定义接口，只要遵循 **Spring Data****的规范**，就无需写实现类。  

•**与继承 Repository** **等价的一种方式，就是在持久层接口上使用** **@RepositoryDefinition** **注解**，并为其指定 domainClass 和 idClass 属性。如下两种方式是完全等价的

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

[![0w6VoQ.png](https://s1.ax1x.com/2020/10/08/0w6VoQ.png)](https://imgchr.com/i/0w6VoQ)

[![0w6Edg.png](https://s1.ax1x.com/2020/10/08/0w6Edg.png)](https://imgchr.com/i/0w6Edg)

#### 查询方法解析

•findBy**UserDepUuid**()，框架在解析该方法时，首先剔除 findBy，然后对剩下的属性进行解析，假设查询实体为Doc

–先判断 **userDepUuid** （根据 POJO 规范，首字母变为小写）是否为查询实体的一个属性，如果是，则表示根据该属性进行查询；如果没有该属性，继续第二步；

–从右往左截取第一个大写字母开头的字符串(此处为**Uuid**)，然后检查剩下的字符串是否为查询实体的一个属性，如果是，则表示根据该属性进行查询；如果没有该属性，则重复第二步，继续从右往左截取；最后假设 user 为查询实体的一个属性；

–接着处理剩下部分（DepUuid），先判断 user 所对应的类型是否有depUuid属性，如果有，则表示该方法最终是根据 “ Doc.user.depUuid” 的取值进行查询；否则继续按照步骤 2 的规则从右往左截取，最终表示根据 “**Doc.user.dep.uuid**” 的值进行查询。

–可能会存在一种特殊情况，比如 Doc包含一个 user 的属性，也有一个 userDep 属性，此时会存在混淆。**可以明确在属性之间加上** **"_"** **以显式表达意图**，比如 "findByUser_DepUuid()" 或者 "findByUserDep_uuid()"

•特殊的参数： 还可以直接在方法的参数上加入分页或排序的参数，比如：

```java
–Page<UserModel> findByName(String name, Pageable pageable);

–List<UserModel> findByName(String name, Sort sort);

@Query
与
@Modifying
执行更新操作
```



### CrudRepository接口

•CrudRepository 接口提供了最基本的对实体类的添删改查操作 

`–T save(T entity);//保存单个实体` 

`–Iterable<T> save(Iterable<? extends T> entities);//保存集合`        

`–T findOne(ID id);//根据id查找实体`         

`–boolean exists(ID id);//根据id判断实体是否存在`         

`–Iterable<T> findAll();//查询所有实体,不用或慎用!`         

`–long count();//查询实体数量`         

`–void delete(ID id);//根据Id删除实体`         

`–void delete(T entity);//删除一个实体` 

`–void delete(Iterable<? extends T> entities);//删除一个实体的集合`         

`–void deleteAll();//删除所有实体,不用或慎用!` 

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

[![0w6AeS.png](https://s1.ax1x.com/2020/10/08/0w6AeS.png)](https://imgchr.com/i/0w6AeS)

•Specification：封装  JPA Criteria 查询条件。通常使用匿名内部类的方式来创建该接口的对象

