疯狂Java&Thinking in java

## 面向对象

### 面向对象概念

●面向对象的方式实际上由OOA（面向对象分析）、OOD （面向对象设计）和OOP（面向对象编程）三个部分有机组成。其中，OOA和OOD的结构需要使用一种方式来描述并记录，目前业界统一采用UML（统一建模语言）来描述并记录OOA和OOD的结果。

●对象是面向对象方法中最基本的概念，它的基本特点有：标识唯一性、分类性、多态性、封装性、模块独立性好。

① 类是具有共同属性、共同方法的一类事物。类是对象的抽象；对象则是类的实例。而类是整个软件系统最小的程序单元，类的封装性将各种信息细节隐藏起来，并通过公用方法来暴露该类对外所提供的功能，从而提高了类的内聚性，降低了对象之间的耦合性。

②对象间的这种相互合作需要一个机制协助进行，这样的机制称为“消息”。消息是一个实例与另一个实例之间相互通信的机制。

③在面向对象方法中，类之间共享属性和操作的机制称为继承。继承具有传递性。继承可分为单继承（一个继承只允许有一个直接父类，即类等级为树形结构）与多继承（一个类允许有多个直接父类）。

Field（状态数据）+方法（行为）=类定义

Java一切皆对象，在Java编程思想中这样写道：

1. 万物皆对象，
2. 程序是对象的集合，他们通过发送消息来告知彼此所要做的，
3. 每个对象拥有其类型，
4. 某一特定类型的对象都可以接收同样的消息

![](https://mmbiz.qpic.cn/mmbiz_png/B77kSvewKqW8eompSpPObNEHzhEibibjtSa6FN2tuIicb1sj9QHzy0q1bfKblEGbFfKbKXicQZmAs0X3a2M8D5X8uQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 用引用操作对象：

尽管java把一起都看为一个对象，但是操作的标识符实际上是对象的一个“引用”。比如 String s;这里只创建了个引用，但是并未创建对象。

#### 创建对象：



new将对象存储在堆中，所以用new创建一个对象---特别小的，简单的变量，往往不是很有效。因此对于（基本类型）java不用new来创建这样的变量，而是创建一个并非是引用的“自动”变量。这个变量的值直接存储"值"到堆栈中。

![](https://upload-images.jianshu.io/upload_images/4748730-7942afd5d21fe639.jpg?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

程序创建，运行时对象是如何分配呢？内存是怎样分配呢？

[![dnZRHO.jpg](https://s1.ax1x.com/2020/08/17/dnZRHO.jpg)](https://imgchr.com/i/dnZRHO)

堆栈：位于RAM中，堆栈指针可以从处理器获得直接支持，堆栈指针向上移动，释放内存。堆栈指针向下移动，分配新的内存。创建程序时java必须知道存在堆栈内所有项的确切的生命周期。以便上下移动堆栈指针，一般“对象引用"存储于其中。

堆：堆是通常是内存池。用于存在java对象。堆不同于堆栈的是：编译器不需知道存在堆里的数据存活多久，因此在堆中分配存储，有很大的灵活性，当需要一个对象时只需new出即可。这是要付出代价就是，堆中的不用的对象何时回收内存问题。

常量存储：常量值通常直接存在程序代码内部，因为他们永远不会改变。

非RAM存储：如果数据完全存活于程序之外，那么他可以不受程序的任何控制，在程序没有运行时也是可以存在，比如流对象和持久化对象。



### 操作符：

运算符指明对操作数的运算方式。组成表达式的Java[操作符](https://baike.baidu.com/item/操作符)有很多种。运算符按照其要求的操作数数目来分，可以有[单目运算符](https://baike.baidu.com/item/单目运算符)、[双目运算](https://baike.baidu.com/item/双目运算)符和[三目运算符](https://baike.baidu.com/item/三目运算符)，它们分别对应于1个、2个、3个操作数。运算符按其功能来分，有[算术运算符](https://baike.baidu.com/item/算术运算符)、[赋值运算符](https://baike.baidu.com/item/赋值运算符)、[关系运算符](https://baike.baidu.com/item/关系运算符)、[逻辑运算符](https://baike.baidu.com/item/逻辑运算符)、[位运算符](https://baike.baidu.com/item/位运算符)和其他运算符。其中java编程思想中对操作符包含以下几个方面：

![](http://mmbiz.qpic.cn/mmbiz_png/B77kSvewKqVYgeJCdRzBplKMqwDK7axDmgCd3iaFwGBXrwsHe2tdcVOTyicuqpto1IDj3Gk4ICjP8xX5I2ndBnkQ/0?wx_fmt=png)

#### 对于自动递增和自动递减操作符：

通常包含前缀式（++i）和后缀式(i++)

“前缀递增"表示”++“的操作位于变量或表达式的前面；而“后缀递增”表示操作符在表达式的后面。对于前缀递增，和前缀递减（++i,--i）会先进行运算然后再进行生成值。而后缀表达式会先生成值，然后再计算。

```java
public class AutoInc{
    public static void main(String[] args) {
        int i=1;
        int m = ++i; // 2
        int n = i++; //2
        int i2 = i; // 3
        int m1 = --i; // 2
        int n2 = i--; // 2
        int i3= i; // 1
    }
}
```

#### 对于关系操作符==与equals()：

<font color="red">==和!= 比较的是两个对象的引用，比较两个对象的实际内容是否相等，需使用equals()方法，但这个方法不适用与“基本类型” 基本类型应直接使用==和!=。</font>

```java 
public class Equivalence{
    public static void main(String[] args) {
        Integer n1= new Integer(22);
        Integer n2= new Integer(22);
        System.out.print(n1==n2); // false
    }
}

public class Equivalence{
    public static void main(String[] args) {
        Integer n1= new Integer(22);
        Integer n2= new Integer(22);
        System.out.print(n1.equals(n2)); // true
    }
}
上面的是因为integer重写了equals方法。
但是对于我们自己所定义的类使用equals方法：
class Value{
	int i;
}
public class Equivalence{
    public static void main(String[] args) {
        Value n1= new Value();
        Value n2= new Value();
        n2.i=n1.i=100;
        System.out.print(n1.equals(n2)); // false
        // 这是因为equals默认比较的是引用。所以我们在自己的类中覆盖equals()方法、对于大多数的java类库比较的是对象的内容，而非比较对象的引用。
    }
}
String s3="as",s4="as"
String s5="asas"
String s6=s3+s4;
s5==s6 是false 因为s6的时候就是会new出一个新的String对象
final String s10=s3+s4;
s5==s10 是false 这里还会进行new出的，final只是影响的s10;


```

##### Q&A equals具有的特性：

- 自反性：x.equals(x)为true

- 对称性：当仅当y.equals(x)为true时x.equals(y)返回true.

- 传递性：如果x.equals(y)和y.equals(2) 均为true,x.equals(2)为true.

当接收一个Object参数并满足以下性质：

- 一致性：当两个对象均未被修改时反复调用x.equals(y)总是返回相反的值
- 非空性：x.equals(null)总是返回false

#### 逻辑与“短路”现象：

短路现象就是，一旦能够明确无误的确定整个表达式的值，就不在计算表达式剩余的部分。因此整个逻辑表达式靠后的部分有可能不会被计算。

```java 
public class ShortCircut{
    public static void main(String[] args) {
        boolean a=0<1 && 2<2 && 2<3; // 实际应该是true ,false,true
        // 但是最后能够计算的只有前面两个， true ,false 确定了整个表达式的值为false，所以不会计算最后一个
        System.out.print(a);
    }
}
```

#### 类型转换操作符：

在适当的时候java会允许将一种数据类型进行自动转换为另一种，但是如果有事后自己进行强转有可能是多余的，

对于将29.7专为int结果是30还是29？这肯定为29，如果你认为要四舍五入，那么你应该使用Math.round()方法

通常自动转换会在表达式中出现的数据类型决定表达式最终结果的数据类型，如果将一个float值与一个double值相乘结果就是double,如果一个int与一个long值进行相乘，那么就是long型。

### uml用例图：

用例图包括用例（以一个椭圆表示，用例的名称放在椭圆的中心或椭圆下面）、角色（Actor，也就是与系统交互的其他实体，以一个人形符号表示）、角色和用例之间的关系（以简单的线段来表示），以及系统内用例之间的关系。用例图一般表示出用例的组织关系——要么是整个系统的全部用例，要么是完成具体功能的一组用例

### 类图：

类在类图上使用包含三个部分的矩形来描述，最上面的部分显示类的名称，中间部分包含类的属性，最下面的部分包含类的方法。类图除了可以表示实体的静态内部结构之外，还可以表示实体之间的相互关系。类之间有三种基本关系： 关联（包括聚合、组合）泛化（与继承同一个概念） 依赖.

### java八大基本数据类型：

- 布尔型：Boolean
- 整数型：byte1 short 2.int 4.long8
- 字符型：char
- 浮点型：float double

常量池（constant pool）指的是在编译期被确定，并被保存在已编译的.class文件中的一些数据。它包括关于类、方法、接口中的常量，也包括字符串直接量。

| 类型    | 包装类    |                     |
| ------- | --------- | ------------------- |
| byte    | Byte      | 8位 -128-127        |
| boolean | Boolean   | 默认false           |
| short   | Short     | 16位-32768- 32767   |
| char    | Character | 16位，存储Unicode码 |
| int     | Integer   | 32位                |
| long    | Long      | 64位                |
| float   | Float     | 32位                |
| double  | Double    | 64位                |

#### 基本类型常见的面试题：

##### Q&A:什么是自动装箱与拆箱？

自动装箱就是java编译器在基本数据类型和对应的对象包装类型之间做一个转化，比如int转为Integer,double转为Double.反之为自动拆箱。

##### Q&A:String是基本的数据类型吗？

String不是节本的数据类型，是final不可修改的类，为了提高效率可以使用StringBuffer类。

##### Q&A:int 和integer有什么区别？

int是基本类型，integer是包装类型。

integer必须实例化后才能使用，而int变量不需要。

integer默认为null,而int默认为0；

integer实际是对象的引用，当new出一个integer时实际上是生成一个指针指向此对象，而int则是直接存储数据值。

##### Q&A:以下结果是什么？

```java
Integer a= new Integer(3);
Integer b= 3; // 将3 自动装箱为Integer类型
int c=3;
a==b  // false 两个引用没有引用统一对象。
a==c // true a 自动 拆箱为int 再和c 比较。
两个非new生成的integer对象，进行比较时如果两个变量值的区间在-128~127之间则比较为 true,如果不在此区间则为false;
Integer a=100;  => Integer a = Integer.valueOf(100);
Integer b=100;
a==b // true;
Integer a=128;
Integer b=128;
a== b // false;
对于valueOf方法在Integer类中会有一个cache判断：

```

##### Q&A  short s1 = 1; s1 = s1 + 1;有什么错? short s1 = 1; s1 +=1;有什么错?
1) 对于 shorts1=1;s1=s1+1 来说，在s1+1 运算时会自动提升表达式的类型为 int， 那么将int赋予给 short类型的变量 s1会出现类型转换错误。
2) 对于 short s1=1;s1+=1 来说 +=是java 语言规定的运算符，java 编译器会对它 进行特殊处理，因此可以正确编译。

### 类：

类是封装对象的行为和属性的载体，具有相同属行和行为的一类实体。类中包含方法和属性。

#### 类中的构造方法：

1.构造方法没有返回值2.名称与类名相同，在构造方法中可以为成员变量赋值，也就是初始化成员变量，<font color="red">若在类中的构造方法都不是无惨的构造方法，编译器不会为类设置一个无参的构造方法</font>，在类中没有设置构造方法时编译器才会在类中自定义一个无参的构造方法；

#### 类中的成员方法：

成员方法对应于类对象的行为，成员方法可以是有参也可以是无参，可以有返回值也可以没有返回值，成员方法中可以调用其他成员方法也可以调用类成员变量，成员方法中也可以定义成员变量这是的成原变量为局部变量，
静态成员变量，静态成员方法：都用 类名.成员方法名。类名.成员变量掉用。静态方法中不可以用this关键字，静态方法不能直接调用非静态方法静态类中不能用this传值，可以用 return进行返回值，和直接输出；用来测试静态方法

1.静态方法不可以访问非静态变量

2.非静态方法可以访问静态方法；

静态方法或属性在类加载时产生的。非静态方法是在new中产生的调用静态方法的格式：类名.静态方法名(参数)；

#### This关键字：

this也可以调用成员方法和成员变量只是不太规范。主要用于当前对象的调用；This也可以做为方法的返回值

this可以在当前方法中获取当前对象的引用。

#### Static关键字：

new创建对象，数据存储空间才会分配，其方法才会对外界调用。而static修饰的：（1）如果没有创建对象，也可以进行调用这个方法。（2）只想为某特定的区域分配单一存储空间，而不考虑究竟创建出多少对象。

当一个事物声明static那就意味着这个域或方法不会与包含他那个类所关联在一起，所以即使从未创建某个类的任何对象，也可以调用其static方法，或访问static域。

比如：

class StaticTest{

​	static int i= 47;

​	String a =1;

}

StaticTest  st1=new  StaticTest();

StaticTest  st22=new  StaticTest();

st1.i和st22.i指向的是同一个存储空间，他们具有相同的值。

![0QL4kd.png](https://s1.ax1x.com/2020/10/02/0QL4kd.png)

尽管当static作用于某个字段时，肯定会改变数据创建的方式（因为一个static字段对每个类来说都只有一份存储空间，而非static字段则是对每个对象有一存储空间）

static方法的内部能调用非静态方法。

static加载顺序：

父类静态代码块->子类静态代码块-->父类非静态代码块-->父类构造方法-->子类非静态代码块-->子类构造方法。

#### Super关键字： 

子类可易继承父类的非私有变量，和方法作为自己的成员变量和成员方法，子类中的方法名与父类的方法名相同时称为子类隐藏了父类的成员变量，子类不能继承父类的成员方法，此时称为重写父类的成员变量；

若子类想访问父类中被子类隐藏的成员方法或变量时可以用super关键字 此时必须由子类来使用super关键字；用途：

1.super（参数列表）；调用父类有参的构造器

2.操作被隐藏在父类中的成员变量或被重写的方法格式：super.成员变量名 必须放在方法的第一个语句中；

Super.成员方法名（[参数列表]）Extends 子类继承父类，子接口继承父接口，implements子类对接口的实现

#### final关键字：

final作用于数据，方法，类。

1.一个永不改变的编译时常量。

2.一个在运行时被初始化的值，而你不希望它被改变。

一个既是static又是final的域只占据一段不能改变的存储空间。

**空白的final:** java允许生成一个空白的final;所谓的空白的final是指的被声明为final但又未给定初始值的域。比如： private final int j;

**final参数**：java允许声明一个参数为final的对象。 

void(final Gizmo g){

g=new Gizmo() // Illegal ---g is final

}

**final方法:** 定义为final方法的原因有两个；1.把方法锁定，以防止任何继承类修改它的含义（保持不变，防止覆盖）。2.是效率

**private和final关键字：** 类中所有的private方法都隐式的指定为final,由于无法取用private方法，所以也就无法进行覆盖他，对于private方法添加final关键字并不能给方法增加任何意义。

**final类：** 将类设置为final即使永远不需要做任何变动，或者出于安全不希望有子类。由于final类禁止继承，所以final类的方法都隐式的指定为final的，因为无法覆盖他们，同样也可以给final类的方法添加final修饰词，但是这没有任何意义。

final修饰引用类型的时候，引用的指向不能修改，但是引用的值可以改

final Student stu = new Student(1,"assd");

stu.setAge(12); // 这里是可以的，因为改变的是堆内存中的数据，引用还是指的这一个堆内存地址。

stu=new Student(); // 这里是不允许的因为，final类型是不可以变的。

##### final好处：

1.final关键字提高了性能。JVM和Java应用都会缓存final变量。

2.final变量可以安全的在多线程的环境下共享，而不需要额外的同步开销。

3.使用final关键字，JVM会对方法，变量进行优化。

##### final修饰的String类

String 类代表字符串。Java 程序中的所有字符串字面值（如 "abc" ）都作为此类的实例实现。<font color="red">字符串是常量；它们的值在创建之后不能更改。字符串缓冲区支持可变的字符串。因为 String 对象是不可变的，所以可以共享。</font>

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /**该值用于字符存储。  */
    private final char value[];

    /**  缓存字符串的哈希代码*/
    private int hash; // Default to 0

    /** 使用jdk1.0.2中的serialVersionUID实现互操作性*/
    private static final long serialVersionUID = -6849794470754667710L;

    /**
     * 类字符串在序列化流协议中是特殊大小写的。
     */
    private static final ObjectStreamField[] serialPersistentFields =
        new ObjectStreamField[0];
    /**
     *初始化新创建的{@code String}对象，使其表示空字符序列。请注意，此构造函数的用法是没有必要，因为字符串是不可变的。
     */
    public String() {
        this.value = "".value;
    }
```

```
直接赋值方式创建对象是在方法区的常量池
String str="hello";//直接赋值的方式

通过构造方法创建字符串对象是在堆内存
String str=new String("hello");//实例化的方式

1）直接赋值（String str = "hello"）：只开辟一块堆内存空间，并且会自动入池，不会产生垃圾。

2）构造方法（String str=  new String("hello");）:会开辟两块堆内存空间，其中一块堆内存会变成垃圾被系统回收，而且不能够自动入池，需要通过public  String intern();方法进行手工入池。

在开发的过程中不会采用构造方法进行字符串的实例化。
```

##### Q&A String的不可变性

```java
Strings are constant; their values cannot be changed after theyare created. String buffers support mutable strings.Because String objects are immutable they can be shared. 
    For example:
 String str = "abc";
 与下面的相等;
 *     char data[] = {'a', 'b', 'c'};
 *     String str = new String(data);
 * Here are some more examples of how strings can be used:
 * <blockquote><pre>
 *     System.out.println("abc");
 *     String cde = "cde";
 *     System.out.println("abc" + cde);
 *     String c = "abc".substring(2,3);
 *     String d = cde.substring(1, 2);
 * </pre></blockquote>
 * <p>
```

##### Q&A **为何String设计为不可变？**

1、运行时常量池的需要,节省内存空间。

　　比如执行 String s = "abc";执行上述代码时，JVM首先在运行时常量池中查看是否存在String对象“abc”，如果已存在该对象，则不用创建新的String对象“abc”，而是将引用s直接指向运行时常量池中已存在的String对象“abc”；如果不存在该对象，则先在运行时常量池中创建一个新的String对象“abc”，然后将引用s指向运行时常量池中创建的新String对象。这样在运行时常量池中只会创建一个String对象"abc"，这样就节省了内存空间。

2、同步

　　因为String对象是不可变的，所以是多线程安全的，同一个String实例可以被多个线程共享。这样就不用因为线程安全问题而使用同步。

3、允许String对象缓存hashcode

　　查看上文JDK1.8中String类源码，可以发现其中有一个字段hash，String类的不可变性保证了hashcode的唯一性，所以可以用hash字段对String对象的hashcode进行缓存，就不需要每次重新计算hashcode。所以Java中String对象经常被用来作为HashMap等容器的键。

4、安全性

　　如果String对象是可变的，那么会引起很严重的安全问题。比如，数据库的用户名、密码都是以字符串的形式传入来获得数据库的连接，或者在socket编程中，主机名和端口都是以字符串的形式传入。因为String对象是不可变的，所以它的值是不可改变的，否则黑客们可以钻到空子，改变String引用指向的对象的值，造成安全漏洞。

##### Q&A  **String s = new String("xyz");创建了几个String Object?二者之间有什么区别？**

两个或一个，”xyz”对应一个对象，这个对象放在字符串常量缓冲区，常量”xyz”不管出现多少遍，都是缓冲区中的那一个。New String每写一遍，就创建一个新的对象，它一句那个常量”xyz”对象的内容来创建出一个新String对象。如果以前就用过’xyz’，这句代表就不会创建”xyz”自己了，直接从缓冲区拿。

#### 对象：

   对象是通过new关键字来创建的，通过引用来接收对象，当对象创建出来后引用就会为对象分配内存，new字是创建对象的操作符，对象的比较有两种形式：

1.“==”运算符是用来比较两个对象引用的地址是否相等，

2.“equal()”方法是来比较两对象引用的内容是否相等。对象的销毁是引用结束后就会被垃圾处理器进行回收；

##### Q&A 创建对象的方法：

1.使用new关键字。

2.使用Class类中的newInstance方法，newInstance方法调用无参构造方器创建对象。Class.forName.newInstance;

3.使用clone方法、

4.反序列化。

##### Q&A 对象的产生过程以及存储：

对象的产生：

new将对象存储在堆中，所以用new创建一个对象---特别小的，简单的变量，往往不是很有效。因此对于（基本类型）java不用new来创建这样的变量，而是创建一个并非是引用的“自动”变量。这个变量的值直接存储"值"到堆栈中。

![](https://upload-images.jianshu.io/upload_images/4748730-7942afd5d21fe639.jpg?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

程序创建，运行时对象是如何分配呢？内存是怎样分配呢？

![dnZRHO.jpg](https://s1.ax1x.com/2020/08/17/dnZRHO.jpg)

对象产生的时机 类加载，然后进行对象的实例化：

##### Q&A **什么时候会进行类加载？**

1.创建类的实例，也就是new一个对象

2.访问某个类或接口的静态变量，或者对该静态变量赋值

3.调用类的静态方法

4.反射（Class.forName("A")）

5.初始化一个类的子类（会首先初始化子类的父类）

6.JVM启动时标明的启动类，即文件名和类名相同的那个类

new ObjectInitTest（）
对象的产生过程
1.JVM会在ObjectInitTest.class文件
2.先加载类级别的成员（静态成员变量 静态块初始化）
3.再加载对象级别的成员（实例成员变量 实例块初始化）

##### Q&A**什么时候进行对象的实例化？**

类加载成功（或已加载过）后，就开始进行对象的实例化了。对象的实例化依次进行了如下几个步骤：

1.对象在堆中的内存空间分配

2.初始化零值，这时会将实例变量都赋予零值

3.设置对象头，对象头保存了一些对象在运行期的状态信息，包括类信息地址（知道对象是属于哪个类的）、hashcode（用于hashmap和hashset等hash结构中）、GC分代年龄（可以依此确定对象何时该放入老年代）等

4.init方法执行，这时对变量的实例变量进行初始化

对象初始化的过程也是线程安全的动作。

#####  StringBuffer StringBulider String 的区别：

StringBuffer线程安全，StringBulider线程不安全，底层实现StringBuffer比StringBulider多一个Synchronized.从源码中可以看得到：

```java
StringBuffer源码分析：
@Override
    public synchronized int length() {
        return count;
    }

    @Override
    public synchronized int capacity() {
        return value.length;
    }


    @Override
    public synchronized void ensureCapacity(int minimumCapacity) {
        if (minimumCapacity > value.length) {
            expandCapacity(minimumCapacity);
        }
    }

    /**
     * @since      1.5
     */
    @Override
    public synchronized void trimToSize() {
        super.trimToSize();
    }

    /**
     * @throws IndexOutOfBoundsException {@inheritDoc}
     * @see        #length()
     */
    @Override
    public synchronized void setLength(int newLength) {
        toStringCache = null;
        super.setLength(newLength);
    }

    /**
     * @throws IndexOutOfBoundsException {@inheritDoc}
     * @see        #length()
     */
    @Override
    public synchronized char charAt(int index) {
        if ((index < 0) || (index >= count))
            throw new StringIndexOutOfBoundsException(index);
        return value[index];
    }
```

##### String 是否可以继承，“+”如何实现的，与StringBuffer区别？

java中通过使用“+”符号串联时实际是使用的StringBuilder实例的appdenf()方法来实现的。



### 构造器

考虑到在初始化期间要自动代用构造器，构造器采用与类名相同的名称，在java中“初始化”和“创建”捆绑在一起，两者不能分离。

- 是一个类创建对象的根本途径，如果一个类没有构造器，这个类通常无法创建实例。因此， Java语言提供了一个功能：如果程序员没有为一个类编写构造器，则系统会为该类提供一个默认的构造器静态变量调用：
- Java编程时不要使用对象去调用static修饰的Field、方法，而是应该使用类去调用static修饰的Field、方法！如果在其他Java代码中看到对象调用static修饰的Field、方法的情形，完全可以把这种用法当成假象，将其替换成用类来调用static修饰的Field、方法的代码

### 封装（Encapsulation），继承，多态

#### 封装：

是面向对象的三大特征之一（另外两个是继承和多态），它指的是将对象的状态信息隐藏在对象内部，不允许外部程序直接访问对象内部信息，而是通过该类所提供的方法来实现对内部信息的操作和访问。将对象的Field和实现细节隐藏起来，不允许外部直接访问。把方法暴露出来，让方法来控制对这些Field进行安全的访问和操作。

String的equals()方法判断两个字符串相等的标准是：只要两个字符串所包含的字符序列相同，通过equals()比较将返回true，否则将返回false。当使用==来判断两个变量是否相等时，如果两个变量是基本类型变量，且都是数值类型（不一定要求数据类型严格相同），则只要两个变量的值相等，就将返回true。

#### *继承*：

用extends关键字来指定接口继承哪个父类，

[修饰符]class 子类名 extends 父类名{类体}子类名必须选参数一般情况首字母大写，继承的父类必须选择参数，用于指定要定义的子类继承于哪个父类在继承中也有重写，就是子类中的方法名与父类的方法名相同，子类就不能继承父类的方法，子类重写了父类的方法；重写体现了子类对父类的补充和改变父类的能力；使一个方法在不同的子类中表现的行为不同；

#### 抽象

抽象类中只有抽象方法没有方法体然后子类用继承的方法来把抽象方法重写，在抽象类中用abstract来定义类或者方法，修饰符不能为private,其中里面可包含普通方法，若子类不能实现其抽象方法则子类也必需定义为是抽象类在让他的子类来实现这些抽象类中的抽象方法；其中可以有静态的方法和变量。
抽象类是对一种事物的抽象属于一类（整个类）模板式设计在类中添加方法子类不会变动，
在调用时只能用子类new出一个对象来调用子类中重写的方法。

#### 多态：

通常使用的方法是重载和重写来实现类多态的性，<font color="red">多态作用：消除类型之间的耦合关系。</font>

**重载** ：是方法与方法之间的使用，其中方法的重载是一个类中有多个同名的方法，但是方法中的参数应不相同

// 判断时可以直接根据方法名字来判断；若调用时只需根据参数的类型，很参数的个数即可以调用

构造器就是一个很好的重载的例子：

比如有参构造器，无参构造器。

**涉及基本类型的重载：基本类型能从一个”较小“的类型自动提升至一个”较大“的类型，此过程一旦牵扯到重载会造成混淆**

**重写**：是子类与父类之间的关系中间涉及到继承与接口的知识

#### 抽象类：

体现的就是一种模板模式的设计，抽象类作为多个子类的通用模板，子类在抽象类的基础上进行扩展、改造，但子类总体上会大致保留抽象类的行为方式。

抽象方法和空方法体的方法不是同一个概念。例如，public abstract void test();是一个抽象方法，它根本没有方法体，即方法定义后面没有一对花括号；但public void test(){}方法是一个普通方法，它已经定义了方法体，只是方法体为空，即它的方法体什么也不做，因此这个方法不可使用abstract来修饰。

abstract不能用于修饰Field，不能用于修饰局部变量，即没有抽象变量、没有抽象Field等说法；abstract也不能用于修饰构造器，没有抽象构造器，抽象类里定义的构造器只能是普通构造器。

抽象父类可以只定义需要使用的某些方法，把不能实现的部分抽象成抽象方法，留给其子类去实现。父类中可能包含需要调用的其他系列方法的方法，这些被调方法既可以由父类实现，也可以由其子类实现。父类里提供的方法只是定义了一个通用算法，其实现也许并不完全由自身实现，而必须依赖于其子类的辅助。

#### 接口：

对于接口里定义的常量Field而言，它们是接口相关的，而且它们只能是常量，因此系统会自动为这些Field增加static和final两个修饰符。也就是说，在接口中定义Field时，不管是否使用public static final修饰符，接口里的Field总将使用这三个修饰符来修饰。而且，接口里没有构造器和初始化块

对于接口里定义的方法而言，它们只能抽象方法，因此系统会自动为其增加abstract修饰符；由于接口里的方法全部是抽象方法，因此接口里不允许定义静态方法，即不可使用static修饰接口里定义的方法。不管定义接口里方法时是否使用public abstract修饰符，接口里的方法总是使用public abstract来修饰。

接口的实现：[修饰符]class 类名[extends 父类名][implements 接口列表]。接口中的变量是静态终结变量public abstract final 变量名，方法：是抽象的方法；不能有静态代码与方法；接口是对行为的抽象（局部类）辐射式设计接口中添加方法子类中都要变动；使用implements时接口列表必须选参数接口列表中存在多个接口名时各个接口名之间使用逗号在接口中：系统默认接口中的方法为抽象方法，不能在接口中有方法体同时要想实现接口的方法也要用继承然后在子类中重写接口中的抽象方法接口可以继承父接口但不能继承类，可以用子类继承接口，调用时与调用抽象类的方法是一样的，

#### 接口与抽象类比较：

- 接口里只能包含抽象方法，不包含已经提供实现的方法；抽象类则完全可以包含普通方法。
- 接口里不能定义静态方法；抽象类里可以定义静态方法。
- 接口里只能定义静态常量Field，不能定义普通Field；抽象类里则既可以定义普通Field，也可以定义静态常量Field。
- 接口里不包含构造器；抽象类里可以包含构造器，抽象类里的构造器并不是用于创建对象，而是让其子类调用这些构造器来完成属于抽象类的初始化操作。
- 接口里不能包含初始化块；但抽象类则完全可以包含初始化块。
- 一个类最多只能有一个直接父类，包括抽象类；但一个类可以直接实现多个接口，通过实现多个接口可以弥补Java单继承的不足。

### 内部类：

匿名内部类不能是抽象类，因为系统在创建匿名内部类时，会立即创建匿名内部类的对象。因此不允许将匿名内部类定义成抽象类。

匿名内部类不能定义构造器，因为匿名内部类没有类名，所以无法定义构造器，但匿名内部类可以定义实例初始化块，通过实例初始化块来完成构造器需要完成的事情。

finalize()方法具有如下4个特点：

- 永远不要主动调用某个对象的finalize()方法，该方法应交给垃圾回收机制调用。
- finalize()方法何时被调用，是否被调用具有不确定性，不要把finalize()方法当成一定会被执行的方法。
- 当JVM执行可恢复对象的finalize()方法时，可能使该对象或系统中其他对象重新变成可达状态。
- 当JVM执行finalize()方法时出现异常时，垃圾回收机制不会报告异常，程序继续执行。

注意：

由于finalize()方法并不一定会被执行，因此如果想清理某个类里打开的资源，则不要放在finalize()方法中进行清理，后面会介绍专门用于清理资源的方法。

### java操作系统底层：

加载文件和动态链接库主要对native方法有用，对于一些特殊的功能（如访问操作系统底层硬件设备等）Java程序无法实现，必须借助C语言来完成，此时需要使用C语言为Java方法提供实现。其实现步骤如下：

1. Java程序中声明native()方法，类似于abstract方法，只有方法签名，没有实现。编译该Java程序，生成一个class文件。
2. 用javah编译第1步生成的class文件，将产生一个.h文件。
3. 写一个.cpp文件实现native方法，其中需要包含第2步产生的.h文件（.h文件中又包含了JDK带的jni.h文件）。
4. 将第3步的.cpp文件编译成动态链接库文件。
5. 在Java中用System类的loadLibrary..()方法或Runtime类的loadLibrary()方法加载第4步产生的动态链接库文件，Java程序中就可以调用这个native()方法了。

java中获取当地时间：

```java
public class LocaleList {

    public static void main(String[] args) {

        // 返回Java所支持的全部国家和语言的数组
        Locale[] localeList = Locale.getAvailableLocales();
        // 遍历数组的每个元素，依次获取所支持的国家和语言
        for (int i = 0; i < localeList.length; i++) {
            // 输出所支持的国家和语言
            System.out.println(localeList[i].getDisplayCountry()
                    + "=" + localeList[i].getCountry() + " "
                    + localeList[i].getDisplayLanguage()
                    + "=" + localeList[i].getLanguage());

        }

    }

}
```

### 初始化与清理：

[![dnQtJS.png](https://s1.ax1x.com/2020/08/17/dnQtJS.png)](https://imgchr.com/i/dnQtJS)

#### 垃圾清理的方式：

**标记清除法：** 这是垃圾收集算法中最基础的，它的思想是标记哪些要被回收的对象，然后统一回收。这种方法很简单，但是效率不高，标记和清除的效率都很低；此外会产生大量不连续的内存碎片，从而导致以后程序在分配较大对象时由于没有充足的连续内存而提前触发一次 GC 操作。

**复制算法：** 为了解决效率问题，复制算法将可用内存按容量划分为相等的两部分，然后每次只使用其中的一块，当一块内存用完后就将还存活的对象复制到第二块内存上，然后一次性清楚完第一块内存，再将第二块上的对象复制到第一块。**但是这种方式内存的代价太高，每次基本上都要浪费一半的内存；于是将该算法进行了改进，内存区域不再是按照 1：1 去划分，而是将内存划分为 8：1：1 三部分，较大那份内存是 Eden 区，其余是两块较小的内存区叫 Survior 区，每次都会优先使用 Eden 区，若 Eden 区满则将对象复制到第二块内存区上，然后清除 Eden 区，如果此时存活的对象太多，以至于 Survivor 不够时，会将这些对象通过分配担保机制复制到老年代中。**

**标记整理法：** 这种方法主要是为了解决标记清除法产生大量内存碎片的问题；当对象存活率较高时，也解决了复制算法的效率问题。它的不同之处就是在清除对象的时候现将可回收对象移动到一端，然后清除掉端边界以外的对象，这样就不会产生内存碎片了。

**分代收集法：** 现在的虚拟机垃圾收集大多采用这种方式，它根据对象的生存周期，将堆分为新生代和老年代。**在新生代中，由于对象生存期短，每次回收都会有大量对象死去，那么这时就采用复制算法。老年代里的对象存活率较高，没有额外的空间进行分配担保，所以可以使用标记整理法或标记清除法。**

#### 构造器初始化：

当Java创建一个对象时，系统先为该对象的所有实例属性分配内存（前提是该类已经被加载过了），接着程序开始对这些实例属性执行初始化，其初始化顺序是：先执行初始化块或声明属性时制定的初始值，再执行构造器里制定的初始值。 在类的内部，变量定义的先后顺序决定了初始化的顺序，即时变量散布于方法定义之间，它们仍就会在任何方法（包括构造器）被调用之前得到初始化。

```java
class Window {
  Window(int maker) {
    System.out.println("Window(" + maker + ")");
  }
}
class House {
  Window window1 = new Window(1);
  House() {
    System.out.println("House()");
    w3 = new Window(33);
  }
  Window window2 = new Window(2);
  void f() {
    System.out.println("f()");
  }
  Window w3 = new Window(3);
}
public class OrderOfInitialization {
  public static void main(String[] args) {
    House h = new House();
    h.f();
  }
}
输出：
Window(1)
Window(2)
Window(3)
House()
Window(33)
f()
由输出可见，w3这个引用会被初始化两次：一次在调用构造器之前，一次在调用期间（第一次引用的对象将被丢弃，并作为垃圾回收）。
```

##### 静态数据初始化

无论创建多少个对象，静态数据都只占一份存储区域。static关键字不能应用于局部变量，因此它只能作用于域。

```java
class Bowl {
  Bowl(int maker) {
    System.out.println("Bowl(" + maker + ")");
  }
  void f1(int maker) {
    System.out.println("f1(" + maker + ")");
  }
}
class Table {
  static Bowl bowl1 = new Bowl(1);
  Table() {
    System.out.println("Table()");
    bowl2.f1(1);
  }
  void f2(int maker) {
    System.out.println("f2(" + maker + ")");
  }
  static Bowl bowl2 = new Bowl(2);
}

class Cupboard {
  Bowl bowl3 = new Bowl(3);
  static Bowl bowl4 = new Bowl(4);
  Cupboard() {
    System.out.println("CupBoard()");
    bowl4.f1(2);
  }
  void f3(int maker) {
    System.out.println("f3(" + maker + ")");
  }
  static Bowl bowl5 = new Bowl(5);
}
public class StaticInitialization {
  public static void main(String[] args) {
    System.out.println("created new Cupboard() in main");
    new Cupboard();
    System.out.println("created new Cupboard in main");
    new Cupboard();
    table.f2(1);
    cupboard.f3(1);
  }
  static Table table = new Table();
  static Cupboard cupboard = new Cupboard();
}
输出
Bowl(1)
Bowl(2)
Table()
f1(1)
Bowl(4)
Bowl(5)
Bowl(3)
CupBoard()
f1(2)
created new Cupboard() in main
Bowl(3)
CupBoard()
f1(2)
created new Cupboard in main
Bowl(3)
CupBoard()
f1(2)
f2(1)
f3(1)
```

从某种程度上来看，初始化是一段固定执行的代码，它不能接受任何参数。因此初始化块对同一个类所有对象所进行的初始化处理完全相同。基于这个原因，不难发现初始化块的基本用法，如果有一段初始化处理代码对所有对象完全相同，且无须接受任何参数，就可以把这段初始化处理代码提取到初始化块中。

<font color="red">初始化顺序先是静态的，然后“非静态的”</font>

```java
public class UUIDUtil{
    Mug mug1;
    Mug mug2;
    {
        mug1=new Mug();
        mug2=new Mug();
    }
    static {
        System.out.println("UUIDUtil 静态代码块");
    }
    UUIDUtil(){
        System.out.println("UUIDUtil 构造函数");
    }
    public static void main(String[] args) {
        new UUIDUtil();
    }
}
class Mug{
    Mug(){
        System.out.println("MUG(构造方法)");
    }
    static{
        System.out.println("MUG static");
    }
}
输出：
UUIDUtil 静态代码块
MUG static
MUG(构造方法)
MUG(构造方法)
UUIDUtil 构造函数
// 形式二：
public class UUIDUtil{
    Mug mug1;
    Mug mug2;
    static {
        System.out.println("UUIDUtil 静态代码块");
    }
    UUIDUtil(){
        System.out.println("UUIDUtil 构造函数");

    }
    {
        mug1=new Mug();
        mug2=new Mug();
    }
    public static void main(String[] args) {
        new UUIDUtil();
    }
}
class Mug{
    static{
        System.out.println("MUG static");
    }
    Mug(){
        System.out.println("MUG(构造方法)");
    }
}
输出
UUIDUtil 静态代码块
MUG static
MUG(构造方法)
MUG(构造方法)
UUIDUtil 构造函数

上图把
static{
   System.out.println("MUG static");
}
修改为{
   System.out.println("MUG static");
}
UUIDUtil 静态代码块
MUG static
MUG(构造方法)
MUG static
MUG(构造方法)
UUIDUtil 构造函数  
```

有上面可见在初始化的时候首先会调用

<font color="red">静态代码块--》非静态代码块-（静态代码块（只有一次），构造函数）-》构造函数，而{}包含的非静态代码块就是和构造方法一样会默认初始化。</font>

#### 数组初始化：

数组是一个具备相同类型的，用一个标识符名称封装到一个起的一个对象序列或基本类型数据序列。

对于数组初始化必须指定其大小，但是这样不需要比如 int[] al={1,22,3,4,5} 这种情况下存储空间的分配由编译器负责（等价于new）

#### 数组的常用功能

**数组** 是一个效率高的存储和随机访问对象引用序列的方式。

常用方法：

equals():比较两个数组是否相等。

deepEquals():用于多维数组比较是否相等。

fill():填充数组。

sort():数组排序

binarySearch():用于已经排序的数组中查找元素。

toString():产生数组的toSting()表示

hashCode()：产生数组的散列码。

System.arrayCopy();复制数组比用for循环速度快了许多。

**数组元素的比较：** 1.实现java.lang.Comparable接口，

1：所有可以 “排序” 的类都实现了java.lang.Comparable接口，Comparable接口中只有一个方法。
2：public int compareTo(Object obj) ;
该方法：
返回 0 表示 this == obj
返回整数表示 this > obj
返回负数表示 this < obj
3:实现了 Comparable 接口的类通过实现 comparaTo 方法从而确定该类对象的排序方式。

#### 枚举类型：

可以参看：https://www.cnblogs.com/alter888/p/9163612.html

#### Q&A对于初始化与清理常见的面试题：

**Q1**:java 对象加载初始化的顺序？

静态代码块--》非静态代码块-（静态代码块（只有一次），构造函数）-》构造函数，而{}包含的非静态代码块就是和构造方法一样会默认初始化

**Q2**:如何判断一个对象是否要回收？

这就是所谓的对象存活性判断，常用的方法有两种：1.引用计数法;　2.对象可达性分析。由于引用计数法存在互相引用导致无法进行GC的问题，所以目前JVM虚拟机多使用对象可达性分析算法。从GC Roots对象计算引用链，能链上的就是存活的。

1、引用计数法

引用计数法的逻辑非常简单，但是存在问题，java并不采用这种方式进行对象存活判断。

引用计数法的逻辑是：在堆中存储对象时，在对象头处维护一个counter计数器，如果一个对象增加了一个引用与之相连，则将counter++。如果一个引用关系失效则counter–。如果一个对象的counter变为0，则说明该对象已经被废弃，不处于存活状态。

2、可达性分析算法

在主流的商用程序语言中(Java和C#)，都是使用可达性分析算法判断对象是否存活的。这个算法的基本思路就是通过一系列名为GC Roots的对象作为起始点，从这些节点开始向下搜索，搜索所走过的路径称为引用链(Reference Chain)，当一个对象到GC Roots没有任何引用链相连时，则证明此对象是不可用的，下图对象object5, object6, object7虽然有互相判断，但它们到GC Roots是不可达的，所以它们将会判定为是可回收对象。

![](https://img-blog.csdnimg.cn/20190529111953162.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTk2NzYx,size_16,color_FFFFFF,t_70)

参看：https://blog.csdn.net/qq_42996761/article/details/90667725

**Q3**:如何理解java中的垃圾回收机制？为什么需要垃圾回收机制？

java采用分代回收，分为年轻代、老年代、永久代。年轻代又分为E区、S1区、S2区。

到jdk1.8，永久代被元空间取代了。

年轻代都使用复制算法，老年代的收集算法看具体用什么收集器。默认是PS收集器，采用标记-整理算法。

System.gc()即垃圾收集机制是指jvm用于释放那些不再使用的对象所占用的内存。垃圾收集的目的在于清除不再使用的对象。gc通过确定对象是否被活动对象引用来确定是否收集该对象。
如果你向系统申请分配内存进行使用(new)，可是使用完了以后却不归还(delete)，结果你申请到的那块内存你自己也不能再访问,该块已分配出来的内存也无法再使用，随着服务器内存的不断消耗，而无法使用的内存越来越多，系统也不能再次将它分配给需要的程序，产生泄露。一直下去，程序也逐渐无内存使用，就会溢出。

**Q4**:java垃圾收集的方式有哪些？

上文已经讲述。

**Q5**:java垃圾回收机制的优缺点？

优点：

开发人员无须过多地关心内存管理，而是关注解决具体的业务。虽然内存泄漏在技术上仍然是可能出现的，但不常见。

GC 在管理内存上有很多智能的算法，它们自动在后台运行。与流行的想法相反，这些通常比手动回收更能确定什么时候是执行垃圾回收的最好时机。

缺点：

当垃圾回收发生时将影响程序的性能，显著地降低运行速度甚至将程序停止。所谓 “Stop the world” 就是当垃圾回收发生的时候应用程序的其他任务都将被冻结。对于应用程序的要求来说，这将是不可接收的，虽然 GC 调优可以最小化甚至消除这个影响。

虽然GC有很多方面可以调优，但你不能指定应用程序在何时怎样执行GC

**Q6**:java垃圾回收的时间有哪些？

CPU空闲时进行回收、堆内存满了后进行回收、调用System.gc()回收。

**Q7**:如果对象置为null，垃圾收集器是否会立即释放占用的内存？

不会。对象回收需要一个过程，这个过程中对象还能复活。而且垃圾回收具有不确定性，指不定什么时候开始回收。

**Q8**:什么是GC？为何有GC?

GC就是垃圾收集的意思（Gabage Collection）。我们在开发中会创建很多对象，这些对象一股脑的都扔进了堆里，如果这些对象只增加不减少，那么堆空间很快就会被耗尽。所以我们需要把一些没用的对象清理掉。jvm使用 jstat -gc 12538 5000   即会每5秒一次显示进程号为12538的java进成的GC情况.

**Q9:**方法区如何判断是否需要回收？

方法区主要回收的内容有：废弃常量和无用的类。对于废弃常量也可通过引用的可达性来判断，但是对于无用的类则需要同时满足下面3个条件：
① 该类所有的实例都已经被回收，也就是Java堆中不存在该类的任何实例；
② 加载该类的ClassLoader已经被回收；
③ 该类对应的java.lang.Class对象没有在任何地方被引用，无法在任何地方通过反射访问该类的方法。

**Q10:**如果频繁老年代回收怎么分析解决？





### 集合：

简单的集合大家庭先看一下：
[![dqmlY4.png](https://s1.ax1x.com/2020/08/30/dqmlY4.png)](https://imgchr.com/i/dqmlY4)

#### Collection

Collection是描述所有序列容器的共同的根接口，他可能被认为是一个附属接口，即因为要表示其他若干个接口的共性而出现的接口。java.util.AbstractCollection类提供了Collection的默认实现。

Collection接口是List、Set和Queue接口的父接口，该接口里定义的方法既可用于操作Set集合，也可用于操作List和Queue集合。

**Collection**:一个独立的元素序列，这些元素都服从一条或多条规则。List必须按照插入的顺序保存元素，而Set不能有重复元素，Query按照排队规则来确定对象产生的顺序（通常与插入的顺序相同）（List,Set,Query）

**Map**:一组成对的“键值对”对象，允许使用键来查找对象。将数字与对象关联，映射表允许我们使用另一个对象来查找某个对象，因此被称为关联数组。(HashMap,TreeMap)

Collection接口里定义了如下操作集合元素的方法：

- boolean add(Object o)：该方法用于向集合里添加一个元素。如果集合对象被添加操作改变了，则返回true。

- boolean addAll(Collection c)：该方法把集合c里的所有元素添加到指定集合里。如果集合对象被添加操作改变了，则返回true。

- void clear()：清除集合里的所有元素，将集合长度变为0。

- boolean contains(Object o)：返回集合里是否包含指定元素。

- boolean containsAll(Collection c)：返回集合里是否包含集合c里的所有元素。

- boolean isEmpty()：返回集合是否为空。当集合长度为0时返回true，否则返回false。

- Iterator iterator()：返回一个Iterator对象，用于遍历集合里的元素。

- boolean remove(Object o)：删除集合中的指定元素o，当集合中包含了一个或多个元素o时，这些元素将被删除，该方法将返回true。

- boolean removeAll(Collection c)：从集合中删除集合c里包含的所有元素（相当于用调用该方法的集合减集合c），如果删除了一个或一个以上的元素，则该方法返回true。

- boolean retainAll(Collection c)：从集合中删除集合c里不包含的元素（相当于把调用该方法的集合变成该集合和集合c的交集），如果该操作改变了调用该方法的集合，则该方法返回true。

- int size()：该方法返回集合里元素的个数。

- Object[] toArray()：该方法把集合转换成一个数组，所有的集合元素变成对应的数组元素。

  Collection的构造器可以接受另一个Collection，用它来将自身进行初始化。可以使用Arrays.list()来为这个构造器产生输入。但是Collection.addAll()方法运行起来要快，可以构建一个不包含元素的Collection，然后调用Collections.addAll()方法。但是这个不如Arrays.asList()或Collections.addAll()灵活，这两个方法都是使用的是可变参数列表。<font color="red">同样也可以使用Arrays.asList()的输出当做list,但是这个底层是数组，因此不能调整其大小，如果试图使用add()或delete()方法在这种列表中添加或删除元素，就有可能引起Unsupported Operation 的错误</font>

#### 迭代器Iterator：

Iterator接口也是Java集合框架的成员，但它与Collection系列、Map系列的集合不一样：Collection系列集合、Map系列集合主要用于盛装其他对象，而Iterator则主要用于遍历（即迭代访问）Collection集合中的元素，Iterator对象也被称为迭代器。

Iterator接口隐藏了各种Collection实现类的底层细节，向应用程序提供了遍历Collection集合元素的统一编程接口。Iterator接口里定义了如下三个方法。

- boolean hasNext()：如果被迭代的集合元素还没有被遍历，则返回true。
- Object next()：返回集合里的下一个元素。
- void remove()：删除集合里上一次next方法返回的元素。

Iterator必须依附于Collection对象，若有一个Iterator对象，则必然有一个与之关联的Collection对象。Iterator提供了两个方法来迭代访问Collection集合里的元素，并可通过remove()方法来删除集合中上一次next()方法返回的集合元素

foreach循环中的迭代变量也不是集合元素本身，系统只是依次把集合元素的值赋给迭代变量，因此在foreach循环中修改迭代变量的值也没有任何实际意义。同样，当使用foreach循环迭代访问集合元素时，该集合也不能被改变，否则将引发Concurrent ModificationException异常。

##### ListIterator:

ListIterator是一个更加强大的iterator的子类型。他只能用于各种list的访问，尽管iterator只能向前移动，而ListIterator可以双向移动，他还可以产生相对于迭代器在列表中指向当前一个和后一个元素的索引，并且可以使用set()方法替换它访问过的最后一个元素。

```java 
public class ListIterator {
	List<Pet> pets = Pets.arrayList(8);
	ListIterator<Pet> it = pets.listIterator();
	while(it.hasNext()){
		System.out.println(it.next()+","+ite.nextIndex()+","+it.previousIndex()+";");
	}
	while(it.hasPrevious()){
		System.out.print(it.previous().id()+" ")
	}
	it = pets.listIterator(3);
	while(it.hasNext()){
		it.next();
		it.set(Pets.randmomPet());
	}
}
```

##### 集合遍历常见的问题及错误：

参看个人博客总结：https://blog.csdn.net/qq_37256896/article/details/108089003

这个说的是比较全面的了https://www.cnblogs.com/chenpi/p/5552901.html

#### ArrayList和Vector类:

都是基于数组实现的List类，所以ArrayList和Vector类封装了一个动态的、允许再分配的Object[]数组。ArrayList或Vector对象使用initialCapacity参数来设置该数组的长度，当向ArrayList或Vector中添加元素超出了该数组的长度时，它们的initialCapacity会自动增加。

对于通常的编程场景，程序员无须关心ArrayList或Vector的initialCapacity。但如果向ArrayList或Vector集合中添加大量元素时，可使用ensureCapacity(int minCapacity)方法一次性地增加initialCapacity。这可以减少重分配的次数，从而提高性能。

如果开始就知道ArrayList或Vector集合需要保存多少个元素，则可以在创建它们时就指定initialCapacity大小。如果创建空的ArrayList或Vector集合时不指定initialCapacity参数，则Object[]数组的长度默认为10。

除此之外，ArrayList和Vector还提供了如下两个方法来重新分配Object[]数组。

- void ensureCapacity(int minCapacity)：将ArrayList或Vector集合的Object[]数组长度增加minCapacity。
- void trimToSize()：调整ArrayList或Vector集合的Object[]数组长度为当前元素的个数。程序可调用该方法来减少ArrayList或Vector集合对象占用的存储空间。

ArrayList和Vector在用法上几乎完全相同，但由于Vector是一个古老的集合（从JDK 1.0就有了），那时候Java还没有提供系统的集合框架，所以Vector里提供了一些方法名很长的方法，例如addElement(Object obj)，实际上这个方法与add (Object obj)没有任何区别。从JDK 1.2以后，Java提供了系统的集合框架，就将Vector改为实现List接口，作为List的实现之一，从而导致Vector里有一些功能重复的方法。

提示：

Vector里有一些功能重复的方法，这些方法中方法名更短的方法属于后来新增的方法，方法名更长的方法则是Vector原有的方法。Java改写了Vector原有的方法，将其方法名缩短是为了简化编程。而ArrayList开始就作为List的主要实现类，因此没有那些方法名很长的方法。实际上，Vector具有很多缺点，通常尽量少用Vector实现类。

除此之外，ArrayList和Vector的显著区别是：ArrayList是线程不安全的，当多个线程访问同一个ArrayList集合时，如果有超过一个线程修改了ArrayList集合，则程序必须手动保证该集合的同步性；但Vector集合则是线程安全的，无须程序保证该集合的同步性。因为Vector是线程安全的，所以Vector的性能比ArrayList的性能要低。实际上，即使需要保证List集合线程安全，也同样不推荐使用Vector实现类。后面会介绍一个Collections工具类，它可以将一个ArrayList变成线程安全的。

Vector还提供了一个Stack子类，它用于模拟“栈”这种数据结构，“栈”通常是指“后进先出”（LIFO）的容器。最后“push”进栈的元素，将最先被“pop”出栈。与Java中的其他集合一样，进栈出栈的都是Object，因此从栈中取出元素后必须进行类型转换，除非你只是使用Object具有的操作。所以Stack类里提供了如下几个方法。

- Object peek()：返回“栈”的第一个元素，但并不将该元素“pop”出栈。

- Object pop()：返回“栈”的第一个元素，并将该元素“pop”出

  

##### ArrayList与LinkedList

ArrayList擅长于随机访问，但是在List的中间插入和移除元素时比较慢。

LinkedList 他插入删除操作比较廉价，随机访问方面相对比较慢的。LinkedList还添加了可以使用栈，队列或双端队列的方法。

使用LinkedList实现一个栈的功能：

```java
public class Stack<T> {
	private LinkedList<T> storage = new LinkedList<T>();
	public void push(T v) { storage.addFirst(v);}
	public T peek(){ return storage.getFirst();}
	public T pop(){ return storage.removeFirst();}
	public boolean empty() { return storage.isEmpty();}
	public String toString() {return storage.toSting();}
}
```

##### 使用LinkedList构造一个栈：

```java
public class Stack<T> {
  private LinkedList<T> storage = new LinkedList<T>();
  public void push(T v) { storage.addFirst(v); }
  public T peek() { return storage.getFirst(); }
  public T pop() { return storage.removeFirst(); }
  public boolean empty() { return storage.isEmpty(); }
  public String toString() { return storage.toString(); }
} ///:~


public class StackTest {
  public static void main(String[] args) {
    Stack<String> stack = new Stack<String>();
    for(String s : "My dog has fleas".split(" "))
      stack.push(s);
    while(!stack.empty())
      System.out.print(stack.pop() + " ");
  }
} /* Output:
fleas has dog My
*///:~
```

可查看：[ArrayList与LinkedList](https://blog.csdn.net/qq_37256896/article/details/103325407)

#### Set集合：

[![dwJqQx.jpg](https://s1.ax1x.com/2020/08/23/dwJqQx.jpg)](https://imgchr.com/i/dwJqQx)

Set集合不允许包含相同的元素，如果试图把两个相同的元素加入同一个Set集合中，则添加操作失败，add方法返回false，且新元素不会被加入。

Set判断两个对象相同不是使用==运算符，而是根据equals方法。也就是说，只要两个对象用equals方法比较返回true，Set就不会接受这两个对象；反之，只要两个对象用equals方法比较返回false，Set就会接受这两个对象（甚至这两个对象是同一个对象，Set也可把它们当成两个对象处理，在后面程序中可以看到这种极端的情况

HashSet具有以下特点。

- 不能保证元素的排列顺序，顺序有可能发生变化。
- HashSet不是同步的，如果多个线程同时访问一个HashSet，假设有两个或者两个以上线程同时修改了HashSet集合时，则必须通过代码来保证其同步。集合元素值可以是null。
- 当向HashSet集合中存入一个元素时，HashSet会调用该对象的hashCode()方法来得到该对象的hashCode值，然后根据该HashCode值决定该对象在HashSet中的存储位置。如果有两个元素通过equals()方法比较返回true，但它们的hashCode()方法返回值不相等，HashSet将会把它们存储在不同的位置，依然可以添加成功。
- 简单地说，HashSet集合判断两个元素相等的标准是两个对象通过equals()方法比较相等，并且两个对象的hashCode()方法返回值也相等。
- 当把一个对象放入HashSet中时，如果需要重写该对象对应类的equals()方法，则也应该重写其hashCode()方法。其规则是：如果两个对象通过equals()方法比较返回true，这两个对象的hashCode值也应该相同。

如果两个对象通过equals()方法比较返回true，但这两个对象的hashCode()方法返回不同的hashCode值时，这将导致HashSet会把这两个对象保存在Hash表的不同位置，从而使两个对象都可以添加成功，这就与Set集合的规则有些出入了。

如果两个对象的hashCode()方法返回的hashCode值相同，但它们通过equals()方法比较返回false时将更麻烦：因为两个对象的hashCode值相同，HashSet将试图把它们保存在同一个位置，但又不行（否则将只剩下一个对象），所以实际上会在这个位置用链式结构来保存多个对象；而HashSet访问集合元素时也是根据元素的hashCode值来快速定位的，如果HashSet中两个以上的元素具有相同的hashCode值，将会导致性能下降。

*重写hashCode()方法的基本规则。*

- 在程序运行过程中，同一个对象多次调用hashCode()方法应该返回相同的值。
- 当两个对象通过equals()方法比较返回true时，这两个对象的hashCode()方法应返回相等的值。
- 对象中用作equals()方法比较标准的Field，都应该用来计算hashCode值

重写equals和hashCOde方法：

```java
public boolean equals(Object obj) {

    if (this == obj) return true;

    if (obj != null && obj.getClass() == R.class) {
        R r = (R) obj;
        if (r.count == this.count) {
            return true;
        }
    }
    return false;
}

public int hashCode() {
    return this.count;
}
```

<font color="red">HashSet所维护的顺序与TreeSet或LinkedHashSet都不同，因为他们实现具有不同的元素存储方式，TreeSet将元素存储在红-黑数据结构中，而HashSet使用的是散列函数，LinkedHashList因为查询速度的原因也使用了散列，但是他使用了链表来维护元素的插入顺序。</font>

LinkedHashSet使用了链表记录集合元素的添加顺序，但LinkedHashSet依然是HashSet，因此它依然不允许集合元素重复

如果向TreeSet中添加一个可变对象后，并且后面程序修改了该可变对象的Field，这将导致它与其他对象的大小顺序发生了改变，但TreeSet不会再次调整它们的顺序，甚至可能导致TreeSet中保存的这两个对象通过compareTo(Object obj)方法比较返回0。下面程序演示了这种情况。

```java
class R implements Comparable {

    int count;

    public R(int count) {

        this.count = count;

    }

    public String toString() {

        return "R[count:" + count + "]";

    }

//重写equals()方法，根据count来判断是否相等

    public boolean equals(Object obj) {

        if (this == obj) {

            return true;

        }

        if (obj != null && obj.getClass() == Z.class) {

            R r = (R) obj;

            if (r.count == this.count) {

                return true;

            }

        }

        return false;

    }

//重写compareTo()方法，根据count来比较大小

    public int compareTo(Object obj) {
        R r = (R) obj;
        return count > r.count ? 1 :count < r.count ? -1 : 0;
    }

}

public class TreeSetTest3 {

    public static void main(String[] args) {

        TreeSet ts = new TreeSet();

        ts.add(new R(5));

        ts.add(new R(-3));

        ts.add(new R(9));

        ts.add(new R(-2));

//打印TreeSet集合，集合元素是有序排列的

        System.out.println(ts); //①

//取出第一个元素R first=(R)ts.first();//对第一个元素的count赋值first.count=20;//取出最后一个元素R last=(R)ts.last();//对最后一个元素的count赋值，与第二个元素的count相同last.count=-2; //再次输出将看到TreeSet里的元素处于无序状态，且有重复元素

        System.out.println(ts); //②

//删除Field被改变的元素，删除失败

        System.out.println(ts.remove(new R(-2))); //③

        System.out.println(ts);

//删除Field没有改变的元素，删除成功

        System.out.println(ts.remove(new R(5))); //④

        System.out.println(ts);

    }

}  
```

上面程序中的R对象对应的类正常重写了equals()方法和compareTo()方法，这两个方法都以R对象的count实例变量作为判断的依据。当程序执行①行代码时，看到程序输出的Set集合元素处于有序状态；因为R类是一个可变类，因此可以改变R对象的count实例变量的值，程序通过粗体字代码行改变了该集合里第一个元素和最后一个元素的count实例变量的值。当程序执行②行代码输出时，将看到该集合处于无序状态，而且集合中包含了重复元素。

##### Set和存储顺序

:baby_chick:Set:存入的Set的每一个元素必须是唯一的，因为Set不保存重复的元素。加入Set的元素必须定义equals()方法以确保对象的唯一性，Set与Conllection有完全一样的接口。Set接口不保证维护单元的次序。

:baby_chick:HashSet:为快速查找而设计的Set，存入的Hashset的元素必须定义HashCode()

:baby_chick:TreeSet:保持次序的Set。底层是树结构，使用它可以从Set中提取有序的序列元素必须实现Comparable接口

:baby_chick:LInkedHashSet:具有与HashSet的查询速度，且内部使用链表维护元素的顺序，于是在使用迭代器遍历Set时结果会按元素插入的次序显示。元素也必须定义hashCode()方法。

<font color="red">HasSet应该是默认的选择因为他对速度进行了优化</font>

##### SortedSet

SortedSet的元素可以保证出于排序状态，这使得他可以通过在SortedSet接口中的下列方法提供附加的功能：Comparator comparator()返回当前Set使用的Comparator;或者返回null,

Object first() 返回容器的第一个元素

Object last() 返回容器的最末一个元素

SortedSet subSet(fromElemny,toElement) 生成set的子集，范围含前不含后

SortedSet headSet(toElement) 生成此Set的子集，由小于toElement的元素组成

SortedSet tailset(fromElemnt) 生成此Set的子集 由大于等于fromElement的元素组成。

```java
// A Java program to demonstrate working of SortedSet
import java.util.SortedSet;
import java.util.TreeSet;
 
public class Main
{
    public static void main(String[] args)
    {
        // Create a TreeSet and inserting elements
        SortedSet<String> sites = new TreeSet<>();
        sites.add("practice");
        sites.add("geeksforgeeks");
        sites.add("quiz");
        sites.add("code");
 
        System.out.println("Sorted Set: " + sites);
        System.out.println("First: " + sites.first());
        System.out.println("Last: " + sites.last());
 
        // Getting elements before quiz (Excluding) in a sortedSet
        SortedSet<String> beforeQuiz = sites.headSet("quiz");
        System.out.println(beforeQuiz);
 
        // Getting elements between code (Including) and
        // practice (Excluding)
        SortedSet<String> betweenCodeAndQuiz =
                                  sites.subSet("code","practice");
        System.out.println(betweenCodeAndQuiz);
 
        // Getting elements after code (Including)
        SortedSet<String> afterCode = sites.tailSet("code");
        System.out.println(afterCode);
    }
}
// 输出
Sorted Set: 
First: code
Last: quiz
```

#### Map：

对于一般的应用场景，程序应该多考虑使用HashMap，因为HashMap正是为快速查询设计的（HashMap底层其实也是采用数组来存储key-value对）。但如果程序需要一个总是排好序的Map时，则可以考虑使用TreeMap。

LinkedHashMap比HashMap慢一点，因为它需要维护链表来保持Map中key-value时的添加顺序。IdentityHashMap性能没有特别出色之处，因为它采用与HashMap基本相似的实现，只是它使用==而不是equals()方法来判断元素相等。EnumMap的性能最好，但它只能使用同一个枚举类的枚举值作为key。

Collections

```java
public class SynchronizedTest{

public static void main(String[] args){

    //下面程序创建了4个同步的集合对象

    Collection c=Collections.synchronizedCollection(new ArrayList());

    List list=Collections.synchronizedList(new ArrayList());

    Set s=Collections.synchronizedSet(new HashSet());

    Map m=Collections.synchronizedMap(new HashMap());
  }
}
```

实现一个Map 这个Map是说明性的，缺乏效率，并且具有固定的尺寸不灵活:

```java
//: containers/AssociativeArray.java
// Associates keys with values.
import static net.mindview.util.Print.*;

public class AssociativeArray<K,V> {
  private Object[][] pairs;
  private int index;
  public AssociativeArray(int length) {
    pairs = new Object[length][2];
  }
  public void put(K key, V value) {
    if(index >= pairs.length)
      throw new ArrayIndexOutOfBoundsException();
    pairs[index++] = new Object[]{ key, value };
  }
  @SuppressWarnings("unchecked")
  public V get(K key) {
    for(int i = 0; i < index; i++)
      if(key.equals(pairs[i][0]))
        return (V)pairs[i][1];
    return null; // Did not find key
  }
  public String toString() {
    StringBuilder result = new StringBuilder();
    for(int i = 0; i < index; i++) {
      result.append(pairs[i][0].toString());
      result.append(" : ");
      result.append(pairs[i][1].toString());
      if(i < index - 1)
        result.append("\n");
    }
    return result.toString();
  }
  public static void main(String[] args) {
    AssociativeArray<String,String> map =
      new AssociativeArray<String,String>(6);
    map.put("sky", "blue");
    map.put("grass", "green");
    map.put("ocean", "dancing");
    map.put("tree", "tall");
    map.put("earth", "brown");
    map.put("sun", "warm");
    try {
      map.put("extra", "object"); // Past the end
    } catch(ArrayIndexOutOfBoundsException e) {
      print("Too many objects!");
    }
    print(map);
    print(map.get("ocean"));
  }
} /* Output:
Too many objects!
sky : blue
grass : green
ocean : dancing
tree : tall
earth : brown
sun : warm
dancing
*///:~

```

##### Map之间的比较

| **Map** 实现          | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| **HashMap\***         | 基于哈希表的实现。（使用此类来代替 **Hashtable** 。）为插入和定位键值对提供了常数时间性能。可以通过构造方法调整性能，这些构造方法允许你设置哈希表的容量和装填因子。 |
| **LinkedHashMap**     | 与 **HashMap** 类似，但是当遍历时，可以按插入顺序或最近最少使用（LRU）顺序获取键值对。只比 **HashMap** 略慢，一个例外是在迭代时，由于其使用链表维护内部顺序，所以会更快些。 |
| **TreeMap**           | 基于红黑树的实现。当查看键或键值对时，它们按排序顺序（由 **Comparable** 或 **Comparator** 确定）。 **TreeMap** 的侧重点是按排序顺序获得结果。 **TreeMap** 是唯一使用 `subMap()` 方法的 **Map** ，它返回红黑树的一部分。 |
| **WeakHashMap**       | 一种具有 *弱键*（weak keys） 的 **Map** ，为了解决某些类型的问题，它允许释放 **Map** 所引用的对象。如果在 **Map** 外没有对特定键的引用，则可以对该键进行垃圾回收。 |
| **ConcurrentHashMap** | 不使用同步锁定的线程安全 **Map**。这在并发的一章中讨论。     |
| **IdentityHashMap**   | 使用 `==` 而不是 `equals()` 来比较键。仅用于解决特殊问题，不适用于一般用途。 |

```java
//: containers/Maps.java
// Things you can do with Maps.
import java.util.concurrent.*;
import java.util.*;
import net.mindview.util.*;
import static net.mindview.util.Print.*;

public class Maps {
  public static void printKeys(Map<Integer,String> map) {
    printnb("Size = " + map.size() + ", ");
    printnb("Keys: ");
    print(map.keySet()); // Produce a Set of the keys
  }
  public static void test(Map<Integer,String> map) {
    print(map.getClass().getSimpleName());
    map.putAll(new CountingMapData(25));
    // Map has 'Set' behavior for keys:
    map.putAll(new CountingMapData(25));
    printKeys(map);
    // Producing a Collection of the values:
    printnb("Values: ");
    print(map.values());
    print(map);
    print("map.containsKey(11): " + map.containsKey(11));
    print("map.get(11): " + map.get(11));
    print("map.containsValue(\"F0\"): "
      + map.containsValue("F0"));
    Integer key = map.keySet().iterator().next();
    print("First key in map: " + key);
    map.remove(key);
    printKeys(map);
    map.clear();
    print("map.isEmpty(): " + map.isEmpty());
    map.putAll(new CountingMapData(25));
    // Operations on the Set change the Map:
    map.keySet().removeAll(map.keySet());
    print("map.isEmpty(): " + map.isEmpty());
  }
  public static void main(String[] args) {
    test(new HashMap<Integer,String>());
    test(new TreeMap<Integer,String>());
    test(new LinkedHashMap<Integer,String>());
    test(new IdentityHashMap<Integer,String>());
    test(new ConcurrentHashMap<Integer,String>());
    test(new WeakHashMap<Integer,String>());
  }
} /* Output:
HashMap
Size = 25, Keys: [15, 8, 23, 16, 7, 22, 9, 21, 6, 1, 14, 24, 4, 19, 11, 18, 3, 12, 17, 2, 13, 20, 10, 5, 0]
Values: [P0, I0, X0, Q0, H0, W0, J0, V0, G0, B0, O0, Y0, E0, T0, L0, S0, D0, M0, R0, C0, N0, U0, K0, F0, A0]
{15=P0, 8=I0, 23=X0, 16=Q0, 7=H0, 22=W0, 9=J0, 21=V0, 6=G0, 1=B0, 14=O0, 24=Y0, 4=E0, 19=T0, 11=L0, 18=S0, 3=D0, 12=M0, 17=R0, 2=C0, 13=N0, 20=U0, 10=K0, 5=F0, 0=A0}
map.containsKey(11): true
map.get(11): L0
map.containsValue("F0"): true
First key in map: 15
Size = 24, Keys: [8, 23, 16, 7, 22, 9, 21, 6, 1, 14, 24, 4, 19, 11, 18, 3, 12, 17, 2, 13, 20, 10, 5, 0]
map.isEmpty(): true
map.isEmpty(): true
...
*///:~

```

##### HashMap:

jdk 1.7时是数组+链表组成，

jdk1.8时是 数组+ 链表 + 红黑树组成。 链表元素大于等于8时会把链表转为树结构，若桶中链的元素个数小于等于6时，树结构还原成链表。当链表的个数为8左右徘徊时就会生成树转链表，链表转树，效率低下。hasMap的负载因子默认为0.75，2^n是为了散列更加均匀。

更多参看：https://mp.csdn.net/console/editor/html/103325204

##### SortedMap

使用 **SortedMap** （由 **TreeMap** 或 **ConcurrentSkipListMap** 实现），键保证按排序顺序，这允许在 **SortedMap** 接口中使用这些方法来提供其他功能：

- `Comparator comparator()` ：生成用于此 **Map** 的比较器， **null** 表示自然排序。
- `T firstKey()` ：返回第一个键。
- `T lastKey()` ：返回最后一个键。
- `SortedMap subMap(fromKey，toKey)` ：生成此 **Map** 的视图，其中键从 **fromKey**（包括），到 **toKey** （不包括）。
- `SortedMap headMap(toKey)` ：使用小于 **toKey** 的键生成此 **Map** 的视图。
- `SortedMap tailMap(fromKey)` ：使用大于或等于 **fromKey** 的键生成此 **Map** 的视图。

这是一个类似于 **SortedSetDemo.java** 的示例，显示了 **TreeMap** 的这种额外行为：

```java
// collectiontopics/SortedMapDemo.java
// What you can do with a TreeMap
import java.util.*;
import onjava.*;

public class SortedMapDemo {
  public static void main(String[] args) {
    TreeMap<Integer,String> sortedMap =
      new TreeMap<>(new CountMap(10));
    System.out.println(sortedMap);
    Integer low = sortedMap.firstKey();
    Integer high = sortedMap.lastKey();
    System.out.println(low);
    System.out.println(high);
    Iterator<Integer> it =
      sortedMap.keySet().iterator();
    for(int i = 0; i <= 6; i++) {
      if(i == 3) low = it.next();
      if(i == 6) high = it.next();
      else it.next();
    }
    System.out.println(low);
    System.out.println(high);
    System.out.println(sortedMap.subMap(low, high));
    System.out.println(sortedMap.headMap(high));
    System.out.println(sortedMap.tailMap(low));
  }
}
/* Output:
{0=A0, 1=B0, 2=C0, 3=D0, 4=E0, 5=F0, 6=G0, 7=H0, 8=I0,
9=J0}
0
9
3
7
{3=D0, 4=E0, 5=F0, 6=G0}
{0=A0, 1=B0, 2=C0, 3=D0, 4=E0, 5=F0, 6=G0}
{3=D0, 4=E0, 5=F0, 6=G0, 7=H0, 8=I0, 9=J0}
*/复制ErrorOK!
```

这里，键值对按照键的排序顺序进行排序。因为 **TreeMap** 中存在顺序感，所以“位置”的概念很有意义，因此可以拥有第一个、最后一个元素或子图。

##### LinkedHashMap

**LinkedHashMap** 针对速度进行哈希处理，但在遍历期间也会按插入顺序生成键值对（ `System.out.println()` 可以遍历它，因此可以看到遍历的结果）。 此外，可以在构造方法中配置 **LinkedHashMap** 以使用基于访问的 *最近最少使用*（LRU） 算法，因此未访问的元素（因此是删除的候选者）会出现在列表的前面。 这样可以轻松创建一个能够定期清理以节省空间的程序。下面是一个显示这两个功能的简单示例：

```java
// collectiontopics/LinkedHashMapDemo.java
// What you can do with a LinkedHashMap
import java.util.*;
import onjava.*;

public class LinkedHashMapDemo {
  public static void main(String[] args) {
    LinkedHashMap<Integer,String> linkedMap =
      new LinkedHashMap<>(new CountMap(9));
    System.out.println(linkedMap);
    // Least-recently-used order:
    linkedMap =
      new LinkedHashMap<>(16, 0.75f, true);
    linkedMap.putAll(new CountMap(9));
    System.out.println(linkedMap);
    for(int i = 0; i < 6; i++)
      linkedMap.get(i);
    System.out.println(linkedMap);
    linkedMap.get(0);
    System.out.println(linkedMap);
  }
}
/* Output:
{0=A0, 1=B0, 2=C0, 3=D0, 4=E0, 5=F0, 6=G0, 7=H0, 8=I0}
{0=A0, 1=B0, 2=C0, 3=D0, 4=E0, 5=F0, 6=G0, 7=H0, 8=I0}
{6=G0, 7=H0, 8=I0, 0=A0, 1=B0, 2=C0, 3=D0, 4=E0, 5=F0}
{6=G0, 7=H0, 8=I0, 1=B0, 2=C0, 3=D0, 4=E0, 5=F0, 0=A0}
*/!
```

这些键值对确实是按照插入顺序进行遍历，即使对于LRU版本也是如此。 但是，在LRU版本中访问前六项（仅限）后，最后三项将移至列表的前面。然后，当再次访问“ **0** ”后，它移动到了列表的后面。

##### 创建不可修改的 Collection 或 Map

通常，创建 **Collection** 或 **Map** 的只读版本会很方便。 **Collections** 类通过将原始集合传递给一个方法然后返回一个只读版本的集合。 对于 **Collection** （如果不能将 **Collection** 视为更具体的类型）， **List** ， **Set** 和 **Map** ，这类方法有许多变体。这个示例展示了针对每种类型，正确构建只读版本集合的方法：

```java
// collectiontopics/ReadOnly.java
// Using the Collections.unmodifiable methods
import java.util.*;
import onjava.*;

public class ReadOnly {
  static Collection<String> data = new ArrayList<>(Countries.names(6));
  public static void main(String[] args) {
    Collection<String> c = Collections.unmodifiableCollection(new ArrayList<>(data));
    System.out.println(c); // Reading is OK
    //- c.add("one"); // Can't change it

    List<String> a = Collections.unmodifiableList(new ArrayList<>(data));
    ListIterator<String> lit = a.listIterator();
    System.out.println(lit.next()); // Reading is OK
    //- lit.add("one"); // Can't change it

    Set<String> s = Collections.unmodifiableSet(new HashSet<>(data));
    System.out.println(s); // Reading is OK
    //- s.add("one"); // Can't change it

    // For a SortedSet:
    Set<String> ss = Collections.unmodifiableSortedSet(new TreeSet<>(data));

    Map<String,String> m =Collections.unmodifiableMap(new 					                                        HashMap<(Countries.capitals(6)));
    System.out.println(m); // Reading is OK
    //- m.put("Ralph", "Howdy!");

    // For a SortedMap:
    Map<String,String> sm =
      Collections.unmodifiableSortedMap(
        new TreeMap<>(Countries.capitals(6)));
  }
}
/* Output:
[ALGERIA, ANGOLA, BENIN, BOTSWANA, BURKINA FASO,
BURUNDI]
ALGERIA
[BENIN, BOTSWANA, ANGOLA, BURKINA FASO, ALGERIA,
BURUNDI]
{BENIN=Porto-Novo, BOTSWANA=Gaberone, ANGOLA=Luanda,
BURKINA FASO=Ouagadougou, ALGERIA=Algiers,
BURUNDI=Bujumbura}
*/!
```

为特定类型调用 “unmodifiable” 方法不会导致编译时检查，但是一旦发生转换，对修改特定集合内容的任何方法调用都将产生 **UnsupportedOperationException** 异常。

在每种情况下，在将集合设置为只读之前，必须使用有意义的数据填充集合。填充完成后，最好的方法是用 “unmodifiable” 方法调用生成的引用替换现有引用。这样，一旦使得内容无法修改，那么就不会冒有意外更改内容的风险。另一方面，此工具还允许将可修改的集合保留为类中的**私有**集合，并从方法调用处返回对该集合的只读引用。所以，你可以在类内修改它，但其他人只能读它。

##### 同步 Collection 或 Map

**synchronized** 关键字是多线程主题的重要组成部分，更复杂的内容在并发中介绍。在这里，只需要注意到 **Collections** 类包含一种自动同步整个集合的方法。 语法类似于 “unmodifiable” 方法：

```java
// collectiontopics/Synchronization.java
// Using the Collections.synchronized methods
import java.util.*;

public class Synchronization {
  public static void main(String[] args) {
    Collection<String> c =
      Collections.synchronizedCollection(
        new ArrayList<>());
    List<String> list = Collections
      .synchronizedList(new ArrayList<>());
    Set<String> s = Collections
      .synchronizedSet(new HashSet<>());
    Set<String> ss = Collections
      .synchronizedSortedSet(new TreeSet<>());
    Map<String,String> m = Collections
      .synchronizedMap(new HashMap<>());
    Map<String,String> sm = Collections
      .synchronizedSortedMap(new TreeMap<>());
  }
}
```

最好立即通过适当的 “synchronized” 方法传递新集合，如上所示。这样，就不会意外地暴露出非同步版本。

###### Fail Fast

Java 集合还具有防止多个进程修改集合内容的机制。如果当前正在迭代集合，然后有其他一些进程介入并插入，删除或更改该集合中的对象，则会出现此问题。也许在集合中已经遍历过了那个元素，也许还没有遍历到，也许在调用 `size()` 之后集合的大小会缩小...有许多灾难情景。 Java 集合库使用一种 *fail-fast* 的机制，该机制可以检测到除了当前进程引起的更改之外，其它任何对集合的更改操作。如果它检测到其他人正在修改集合，则会立即生成 **ConcurrentModificationException** 异常。这就是“fail-fast”的含义——它不会在以后使用更复杂的算法尝试检测问题（快速失败）。

通过创建迭代器并向迭代器指向的集合中添加元素，可以很容易地看到操作中的 fail-fast 机制，如下所示：

```java
// collectiontopics/FailFast.java
// Demonstrates the "fail-fast" behavior
import java.util.*;

public class FailFast {
  public static void main(String[] args) {
    Collection<String> c = new ArrayList<>();
    Iterator<String> it = c.iterator();
    c.add("An object");
    try {
      String s = it.next();
    } catch(ConcurrentModificationException e) {
      System.out.println(e);
    }
  }
}
/* Output:
java.util.ConcurrentModificationException
*/
```

异常来自于在从集合中获得迭代器之后，又尝试在集合中添加元素。程序的两个部分可能会修改同一个集合，这种可能性的存在会产生不确定状态，因此异常会通知你更改代码。在这种情况下，应先将所有元素添加到集合，然后再获取迭代器。

**ConcurrentHashMap** ， **CopyOnWriteArrayList** 和 **CopyOnWriteArraySet** 使用了特定的技术来避免产生 **ConcurrentModificationException** 异常。

##### Set与Map之间的关系非常密切:

Set集合和Map集合的对应关系如下。

■ Set <-> Map

■ EnumSet <-> EnumMap

■ SortedSet <-> SortedMap

■ TreeSet <-> TreeMap

■ NavigableSet <-> NavigableMap

■ HashSet <-> HashMap

■ LinkedHashSet <-> LinkedHashMap

虽然Map中放的元素是key-value对，Set集合中放的元素是单个对象，但如果我们把key-value对中的value当成key的附庸：key在哪里，value就跟在哪里。这样就可以像对待Set一样来对待Map了。事实上，Map提供了一个Entry内部类来封装key-value对，而计算Entry存储时则只考虑Entry封装的key。从Java源码来看， Java是先实现了Map，然后通过包装一个所有value都为null的Map就实现了Set集合。

如果把Map里的所有value放在一起来看，它们又非常类似于一个List：元素与元素之间可以重复，每个元素可以根据索引来查找，只是Map中的索引不再使用整数值，而是以另一个对象作为索引。如果需要从List集合中取出元素，则需要提供该元素的数字索引；如果需要从Map中取出元素，则需要提供该元素的key索引。因此，Map有时也被称为字典，或关联数组

遍历map用keyset()方法：

```java
public class LinkedHashMapTest {

    public static void main(String[] args) {

        LinkedHashMap scores = new LinkedHashMap();

        scores.put("语文", 80);
        scores.put("英文", 82);
        scores.put("数学", 76);
		//遍历scores里的所有key-value对
        for (Object key : scores.keySet()) {
            System.out.println(key + "------>" + scores.get(key));
        }
    }
}
```

##### 归纳起来简单地说HashMap：

- HashMap在底层将key-value对当成一个整体进行处理，这个整体就是一个Entry对象。

- HashMap底层采用一个Entry[]数组来保存所有的key-value对，当需要存储一个Entry对象时，会根据Hash算法来决定其存储位置；当需要取出一个Entry时，也会根据Hash算法找到其存储位置，直接取出该Entry。由此可见，HashMap之所以能快速存、取它所包含的Entry，完全类似于现实生活中的：不同的东西要放在不同的位置，需要时才能快速找到它。

- 当创建HashMap时，有一个默认的负载因子（load factor），其默认值为0.75。这是时间和空间成本上的一种折衷：增大负载因子可以减少Hash表（就是那个Entry数组）所占用的内存空间，但会增加查询数据的时间开销，而查询是最频繁的的操作（HashMap的get()与put()方法都要用到查询）；减小负载因子会提高数据查询的性能，但会增加Hash表所占用的内存空间

- HashMap时根据实际需要适当地调整load factor的值。如果程序比较关心空间开销，内存比较紧张，可以适当地增加负载因子；如果程序比较关心时间开销，内存比较宽裕，则可以适当减少负载因子。通常情况下，程序员无需改变负载因子的值。

- 如果开始就知道HashMap会保存多个key-value对，可以在创建时就使用较大的初始化容量，如果HashMap中Entry的数量一直不会超过极限容量（capacity ＊ load factor），HashMap就无需调用resize()方法重新分配table数组，从而保证较好的性能。当然，开始就将初始容量设置太高可能会浪费空间（系统需要创建一个长度为capacity的Entry数组），因此创建HashMap时初始化容量设置也需要小心对待

  ​	

[Set与Map更多参看对hashset和Map有一个比较好的认知：](https://blog.csdn.net/qq_37256896/article/details/103325204)

#### Queue

对列是一个先进先出的容器，即从容器的一端放入事物，从另一端取出来并且事物放入容器的顺序与取出的顺序是相同的，队列常被当做一种可靠的将对象从程序的某个区域传输到另一个区域的途径。

LinkedList可以用作Queue的一种实现，可以将LinkedList向上转型为Queue。

Queue接口窄化了对LinkedList的方法的访问权限，以使得只有恰当的方法才可使用。

##### 使用LinkedList向上转型构造一个Queue

```java
import java.util.*;

public class QueueDemo {
  public static void printQ(Queue queue) {
    while(queue.peek() != null)
      System.out.print(queue.remove() + " ");
    System.out.println();
  }
  public static void main(String[] args) {
    Queue<Integer> queue = new LinkedList<Integer>();
    Random rand = new Random(47);
    for(int i = 0; i < 10; i++)
      queue.offer(rand.nextInt(i + 10));
    printQ(queue);
    Queue<Character> qc = new LinkedList<Character>();
    for(char c : "Brontosaurus".toCharArray())
      qc.offer(c);
    printQ(qc);
  }
} /* Output:
8 1 1 1 5 14 3 1 0 1
B r o n t o s a u r u s
*///:~

```

##### PriorityQueue

先进先出描述了最典型的对列规则。对列规则是指定在给定一组对列中的元素的情况下，确定下一个弹出对列的元素的规则。先进先出是声明的下一个元素应该等待的最长的元素。

当使用PriorityQueue上调用offer()方法来插入一个对象时，这个对象会在对列中被排序。默认的排揎将使用对象在对列中的自然排序、PriorityQueue可以确保当你调用peek(),poll()和remove()方法时，获取的元素将是对列中优先级最高的元素。

#### [集合工具类]

集合有许多独立的实用工具程序，在 **java.util.Collections** 中表示为静态方法。之前已经见过其中一些，例如 `addAll()` ， `reverseOrder()` 和 `binarySearch()` 。以下是其他内容（同步和不可修改的实用工具程序将在后面的章节中介绍）。在此表中，在需要的时候使用了泛型：

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **checkedCollection(Collection<T> c, Class<T> type)**  **checkedList(List<T> list, Class<T> type)**  **checkedMap(Map<K, V> m, Class<K> keyType, Class<V> valueType)**  **checkedSet(Set<T> s, Class<T> type)**  **checkedSortedMap(SortedMap<K, V> m, Class<K> keyType, Class<V> valueType)**  **checkedSortedSet(SortedSet<T> s, Class<T> type)** | 生成 **Collection** 的动态类型安全视图或 **Collection** 的特定子类型。 当无法使用静态检查版本时使用这个版本。  这些方法的使用在[第九章 多态](https://lingcoder.github.io/OnJava8/#/)章节的“动态类型安全”标题下进行了展示。 |
| **max(Collection)**  **min(Collection)**                     | 使用 **Collection** 中对象的自然比较方法生成参数集合中的最大或最小元素。 |
| **max(Collection, Comparator)**  **min(Collection, Comparator)** | 使用 **Comparator** 指定的比较方法生成参数集合中的最大或最小元素。 |
| **indexOfSubList(List source, List target)**                 | 返回 **target** 在 **source** 内第一次出现的起始索引，如果不存在则返回 -1。 |
| **lastIndexOfSubList(List source, List target)**             | 返回 **target** 在 **source** 内最后一次出现的起始索引，如果不存在则返回 -1。 |
| **replaceAll(List<T> list, T oldVal, T newVal)**             | 用 **newVal** 替换列表中所有的 **oldVal** 。                 |
| **reverse(List)**                                            | 反转列表                                                     |
| **reverseOrder()**  **reverseOrder(Comparator<T>)**          | 返回一个 **Comparator** ，它与集合中实现了 **comparable<T>** 接口的对象的自然顺序相反。第二个版本颠倒了所提供的 **Comparator** 的顺序。 |
| **rotate(List, int distance)**                               | 将所有元素向前移动 **distance** ，将尾部的元素移到开头。（译者注：即循环移动） |
| **shuffle(List)**  **shuffle(List, Random)**                 | 随机置换指定列表（即打乱顺序）。第一个版本使用了默认的随机化源，或者也可以使用第二个版本，提供自己的随机化源。 |
| **sort(List<T>)**  **sort(List<T>, Comparator<? super T> c)** | 第一个版本使用元素的自然顺序排序该 **List<T>** 。第二个版本根据提供的 **Comparator** 排序。 |
| **copy(List<? super T> dest, List<? extends T> src)**        | 将 **src** 中的元素复制到 **dest** 。                        |
| **swap(List, int i, int j)**                                 | 交换 **List** 中位置 **i** 和 位置 **j** 的元素。可能比你手工编写的速度快。 |
| **fill(List<? super T>, T x)**                               | 用 **x** 替换 **List** 中的所有元素。                        |
| **nCopies(int n, T x)**                                      | 返回大小为 **n** 的不可变 **List<T>** ，其引用都指向 **x** 。 |
| **disjoint(Collection, Collection)**                         | 如果两个集合没有共同元素，则返回 **true** 。                 |
| **frequency(Collection, Object x)**                          | 返回 **Collection** 中，等于 **x** 的元素个数。              |
| **emptyList()**  **emptyMap()**  **emptySet()**              | 返回不可变的空 **List** ， **Map** 或 **Set** 。这些是泛型的，因此生成的 **Collection** 可以被参数化为所需的类型。 |
| **singleton(T x)**  **singletonList(T x)**  **singletonMap(K key, V value)** | 生成一个不可变的 **List** ， **Set** 或 **Map** ，其中只包含基于给定参数的单个元素。 |
| **list(Enumeration<T> e)**                                   | 生成一个 **ArrayList<T>** ，其中元素为（旧式） **Enumeration** （ **Iterator** 的前身）中的元素。用于从遗留代码向新式转换。 |
| **enumeration(Collection<T>)**                               | 为参数集合生成一个旧式的 **Enumeration<T>** 。               |

请注意， `min（)` 和 `max()` 使用 **Collection** 对象，而不使用 **List** ，因此不必担心是否应对 **Collection** 进行排序。（如前所述，在执行 `binarySearch()` 之前，将会对 **List** 或数组进行`sort()` 排序。）

下面是一个示例，展示了上表中大多数实用工具程序的基本用法：

```java
// collectiontopics/Utilities.java
// Simple demonstrations of the Collections utilities
import java.util.*;

public class Utilities {
  static List<String> list = Arrays.asList(
    "one Two three Four five six one".split(" "));
  public static void main(String[] args) {
    System.out.println(list);
    System.out.println("'list' disjoint (Four)?: " +
      Collections.disjoint(list,
        Collections.singletonList("Four")));
    System.out.println(
      "max: " + Collections.max(list));
    System.out.println(
      "min: " + Collections.min(list));
    System.out.println(
      "max w/ comparator: " + Collections.max(list,
      String.CASE_INSENSITIVE_ORDER));
    System.out.println(
      "min w/ comparator: " + Collections.min(list,
      String.CASE_INSENSITIVE_ORDER));
    List<String> sublist =
      Arrays.asList("Four five six".split(" "));
    System.out.println("indexOfSubList: " +
      Collections.indexOfSubList(list, sublist));
    System.out.println("lastIndexOfSubList: " +
      Collections.lastIndexOfSubList(list, sublist));
    Collections.replaceAll(list, "one", "Yo");
    System.out.println("replaceAll: " + list);
    Collections.reverse(list);
    System.out.println("reverse: " + list);
    Collections.rotate(list, 3);
    System.out.println("rotate: " + list);
    List<String> source =
      Arrays.asList("in the matrix".split(" "));
    Collections.copy(list, source);
    System.out.println("copy: " + list);
    Collections.swap(list, 0, list.size() - 1);
    System.out.println("swap: " + list);
    Collections.shuffle(list, new Random(47));
    System.out.println("shuffled: " + list);
    Collections.fill(list, "pop");
    System.out.println("fill: " + list);
    System.out.println("frequency of 'pop': " +
      Collections.frequency(list, "pop"));
    List<String> dups =
      Collections.nCopies(3, "snap");
    System.out.println("dups: " + dups);
    System.out.println("'list' disjoint 'dups'?: " +
      Collections.disjoint(list, dups));
    // Getting an old-style Enumeration:
    Enumeration<String> e =
      Collections.enumeration(dups);
    Vector<String> v = new Vector<>();
    while(e.hasMoreElements())
      v.addElement(e.nextElement());
    // Converting an old-style Vector
    // to a List via an Enumeration:
    ArrayList<String> arrayList =
      Collections.list(v.elements());
    System.out.println("arrayList: " + arrayList);
  }
}
/* Output:
[one, Two, three, Four, five, six, one]
'list' disjoint (Four)?: false
max: three
min: Four
max w/ comparator: Two
min w/ comparator: five
indexOfSubList: 3
lastIndexOfSubList: 3
replaceAll: [Yo, Two, three, Four, five, six, Yo]
reverse: [Yo, six, five, Four, three, Two, Yo]
rotate: [three, Two, Yo, Yo, six, five, Four]
copy: [in, the, matrix, Yo, six, five, Four]
swap: [Four, the, matrix, Yo, six, five, in]
shuffled: [six, matrix, the, Four, Yo, five, in]
fill: [pop, pop, pop, pop, pop, pop, pop]
frequency of 'pop': 7
dups: [snap, snap, snap]
'list' disjoint 'dups'?: true
arrayList: [snap, snap, snap]
*/复制ErrorOK!
```

输出解释了每种实用方法的行为。请注意由于大小写的缘故，普通版本的 `min()` 和 `max()` 与带有 **String.CASE_INSENSITIVE_ORDER** 比较器参数的版本的区别。

##### [排序和搜索列表]

用于执行排序和搜索 **List** 的实用工具程序与用于排序对象数组的程序具有相同的名字和方法签名，只不过是 **Collections** 的静态方法而不是 **Arrays** 。 这是一个使用 **Utilities.java** 中的 **list** 数据的示例：

```java
// collectiontopics/ListSortSearch.java
// Sorting/searching Lists with Collections utilities
import java.util.*;

public class ListSortSearch {
  public static void main(String[] args) {
    List<String> list =
      new ArrayList<>(Utilities.list);
    list.addAll(Utilities.list);
    System.out.println(list);
    Collections.shuffle(list, new Random(47));
    System.out.println("Shuffled: " + list);
    // Use ListIterator to trim off last elements:
    ListIterator<String> it = list.listIterator(10);
    while(it.hasNext()) {
      it.next();
      it.remove();
    }
    System.out.println("Trimmed: " + list);
    Collections.sort(list);
    System.out.println("Sorted: " + list);
    String key = list.get(7);
    int index = Collections.binarySearch(list, key);
    System.out.println(
      "Location of " + key + " is " + index +
      ", list.get(" + index + ") = " +
      list.get(index));
    Collections.sort(list,
      String.CASE_INSENSITIVE_ORDER);
    System.out.println(
      "Case-insensitive sorted: " + list);
    key = list.get(7);
    index = Collections.binarySearch(list, key,
      String.CASE_INSENSITIVE_ORDER);
    System.out.println(
      "Location of " + key + " is " + index +
      ", list.get(" + index + ") = " +
      list.get(index));
  }
}
/* Output:
[one, Two, three, Four, five, six, one, one, Two,
three, Four, five, six, one]
Shuffled: [Four, five, one, one, Two, six, six, three,
three, five, Four, Two, one, one]
Trimmed: [Four, five, one, one, Two, six, six, three,
three, five]
Sorted: [Four, Two, five, five, one, one, six, six,
three, three]
Location of six is 7, list.get(7) = six
Case-insensitive sorted: [five, five, Four, one, one,
six, six, three, three, Two]
Location of three is 7, list.get(7) = three
*/复制ErrorOK!
```

就像使用数组进行搜索和排序一样，如果使用 **Comparator** 进行排序，则必须使用相同的 **Comparator** 执行 `binarySearch()` 。

该程序还演示了 **Collections** 中的 `shuffle()` 方法，该方法随机打乱了 **List** 的顺序。 **ListIterator** 是在打乱后的列表中的特定位置创建的，用于从该位置删除元素，直到列表末尾。

#### 集合常见的面试题：

###### Q&A HashMap的组成？

jdk 1.7时是数组+链表组成，

jdk1.8时是 数组+ 链表 + 红黑树组成。 链表元素大于等于8时会把链表转为树结构，若桶中链的元素个数小于等于6时，树结构还原成链表。当链表的个数为8左右徘徊时就会生成树转链表，链表转树，效率低下。hasMap的负载因子默认为0.75，2^n是为了散列更加均匀。

###### Q&A HashMap的key为自定义的类应该怎么办？

如果key为自定义的类应该重写hashcode()和equals()方法。

###### Q&A HashMap为何线程不安全？

1.在JDK1.7中，当并发执行扩容操作时会造成环形链和数据丢失的情况。
2.在JDK1.8中，在并发执行put操作时会发生数据覆盖的情况。

###### Q&A HashMap如何保证线程安全？

使用Collections.synchronizedMap()包装一下就可以了，原理就是对所有的修改操作都加上synchronized，保证了线程的安全。

```java
Map  map = Collections.synchronizedMap(new HashMap());
```

###### Q&A HashMap中的key可以为任意类型吗？

不能使用基本类型，HashMap中key是可以为null, 只能存储一个null, 因为计算key的hash值的时候，如果key为null， 则其hash值为0

之所以key不能为基本数据类型，则是因为基本数据类型不能调用其hashcode()方法和equals()方法，进行比较，所以HashMap集合的key只能为引用数据类型，不能为基本数据类型，可以使用基本数据类型的包装类，例如Integer Double等。

###### Q&A HashMap 和 HashTable 区别?

HashMap和HashTable都实现了Map接口，HashMap允许键和值是null而HashTable不允许键和值是null,HashTable是同步的，而HashMap不是，因此hashMap适用于单线程环境，而HashTable适用于多线程环境。HashMap提供了可供应用迭代的键的集合。

###### Q&A HashMap和ConCurrentHashMap区别？

hashMap线程不安全，put时在多线程的情况下会形成环而导致循环。

ConCurrentHashMap是线程安全的，采用分段机制，减少锁粒度。

ConCurrentHashMap是线程安全，在jdk1.7时采用Segment+HashEntry的方式进行实现 lock加上Segment上面。1.7 size计算是线采用不加锁的方式。连续计算元素的个数，最多计算3次。

1.8中取而代之是采用Node+CAS +Synchronized来保证并发安全，1.8实现使用一个volatile类型的变量baseCount记录元素的各少数。当插入新数据或删除新数据时，会通过addCount()方法更新baseCount，通过累计baseCount和CounterCell数组中的数量，即可得到元素的总个数。

###### Q&A ConcurrentHashMap 和 HashTable 区别?

**ConcurrentHashMap**

- 底层采用分段的数组+链表实现，线程**安全**
- 通过把整个Map分为N个Segment，可以提供相同的线程安全，但是效率提升N倍，默认提升16倍。(读操作不加锁，由于HashEntry的value变量是 volatile的，也能保证读取到最新的值。)
- Hashtable的synchronized是针对整张Hash表的，即每次锁住整张表让线程独占，ConcurrentHashMap允许多个修改操作并发进行，其关键在于使用了锁分离技术
- 有些方法需要跨段，比如size()和containsValue()，它们可能需要锁定整个表而而不仅仅是某个段，这需要按顺序锁定所有段，操作完毕后，又按顺序释放所有段的锁
- 扩容：段内扩容（段内元素超过该段对应Entry数组长度的75%触发扩容，不会对整个Map进行扩容），插入前检测需不需要扩容，有效避免无效扩容

**HashTable**

- 底层数组+链表实现，无论key还是value都**不能为null**，线程**安全**，实现线程安全的方式是在修改数据时锁住整个HashTable，效率低，ConcurrentHashMap做了相关优化
- 初始size为**11**，扩容：newsize = olesize*2+1
- 计算index的方法：index = (hash & 0x7FFFFFFF) % tab.length

两则的区别：

hashtable线程安全，采用的是线程同步得方法。

- ConcurrentHashMap提供了与Hashtable和SynchronizedMap不同的锁机制。Hashtable中采用的锁机制是一次锁住整个hash表，从而在同一时刻只能由一个线程对其进行操作；而ConcurrentHashMap中则是一次锁住一个桶。

- Java5提供了ConcurrentHashMap，它是HashTable的替代，比HashTable的扩展性更好。

###### Q&A Linkedhashmap 与 hashmap 的区别?

- **HashMap**
- HashMap 是一个最常用的Map，它根据键的HashCode 值存储数据，根据键可以直接获取它的值，具有很快的访问速度。遍历时，取得数据的顺序是完全随机的。
- HashMap最多只允许一条记录的键为Null；允许多条记录的值为 Null。
- HashMap不支持线程的同步（即任一时刻可以有多个线程同时写HashMap），可能会导致数据的不一致。如果需要同步，可以用 Collections的synchronizedMap方法使HashMap具有同步的能力，或者使用ConcurrentHashMap。
- Hashtable与 HashMap类似，它继承自Dictionary类。不同的是：Hashtable不允许记录的键或者值为空；它支持线程的同步（即任一时刻只有一个线程能写Hashtable），因此也导致了 Hashtable在写入时会比较慢。
- **LinkedHashMap**
- **保存插入顺序**：LinkedHashMap保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的。也可以在构造时带参数，按照应用次数排序。
- **速度慢**：在遍历的时候会比HashMap慢，不过有种情况例外：当HashMap容量很大，实际数据较少时，遍历起来可能会比LinkedHashMap慢。因为LinkedHashMap的遍历速度只和实际数据有关，和容量无关，而HashMap的遍历速度和他的容量有关。

###### Q&A  hashmap 与 hashset 区别?

HashSet和HashMap之间有很多相似之处。对于HashSet而言，系统采用Hash算法决定集合元素的存储位置，这样可以保证快速存、取集合元素；对于HashMap而言，系统将value当成key的附属，系统根据Hash算法来决定key的存储位置，这样可以保证快速存、取集合key，而value总是紧随key存储。

HashSet的add()方法添加集合元素时实际上转变为调用HashMap的put()方法来添加 key-value对，当新放入 HashMap的Entry 中key 与集合中原有Entry的key 相同（hashCode()返回值相等，通过equals比较也返回true）时，新添加的Entry的value将覆盖原来Entry的value，但key不会有任何改变。因此，如果向HashSet中添加一个已经存在的元素，新添加的集合元素（底层由HashMap的key保存）不会覆盖已有的集合元素

###### Q&A ArrayList和Vector有何异同点？

相同点：
（1）两者都是基于索引的，都是基于数组的。
（2）两者都维护插入顺序，我们可以根据插入顺序来获取元素。
（3）ArrayList 和 Vector 的迭代器实现都是 fail-fast 的。
（4）ArrayList 和 Vector 两者允许 null 值，也可以使用索引值对元素进行随机访问。
不同点：
（1）Vector 是同步，线程安全，而 ArrayList 非同步，线程不安全。对于 ArrayList，如果 迭代时改变列表，应该使用 CopyOnWriteArrayList。
（2）但是，ArrayList 比 Vector 要快，它因为有同步，不会过载。
（3）在使用上，ArrayList 更加通用，因为 Collections 工具类容易获取同步列表和只读列 表。ArrayList在并发add()可能出现下标越界异常。

###### Q&A ArrayList 与 LinkedList 区别 ?

ArrayList 和 LinkedList 都实现了 List 接口，他们有以下的不同点： ArrayList 是基于索引的数据接口，它的底层是数组。它可以以 O(1)时间复杂度对元素进行 随机访问。与此对应，LinkedList 是以元素列表的形式存储它的数据，每一个元素都和它的 前一个和后一个元素链接在一起，在这种情况下，查找某个元素的时间复杂度是 O(n)。 相对于 ArrayList，LinkedList 的插入，添加，删除操作速度更快，因为当元素被添加到集合 任意位置的时候，不需要像数组那样重新计算大小或者是更新索引。 LinkedList 比 ArrayList 更占内存，因为 LinkedList 为每一个节点存储了两个引用，一个指 向前一个元素，一个指向下一个元素。 

###### Q&A  数组(Array)和列表(ArrayList)有什么区别？什么时候应该使用 Array 而不是 ArrayList？

array包含基本类型和对象类型。Arraylist只能包含对象类型。

Arraylist是采用数组实现的，arraylist是可以自动扩容的。比array提供了更多的特性，比如 addAll(),removeAll() 等。

###### Q&A 使用ArrayList的迭代器会出现什么问题？单线程和多线程环境下； 

常用的迭代器设计模式，iterator 方法返回一个父类实现的迭代器。 1、迭代器的 hasNext 方法的作用是判断当前位置是否是数组最后一个位置，相等为 false， 否则为 true。 2、迭代器 next 方法用于返回当前的元素，并把指针指向下一个元素，值得注意的是，每次 使用 next 方法的时候，都会判断创建迭代器获取的这个容器的计数器 modCount 是否与此 时 的 不 相 等 ， 不 相 等 说 明 集 合 的 大 小 被 修 改 过 ， 如 果 是 会 抛 出 ConcurrentModificationException 异常，如果相等调用 get 方法返回元素即可

#### Properties类:

Properties 继承于 Hashtable。表示一个持久的属性集，属性列表以key-value的形式存在，key和value都是字符串。

[![03KBlR.png](https://s1.ax1x.com/2020/10/03/03KBlR.png)](https://imgchr.com/i/03KBlR)Properties 类被许多Java类使用。例如，在获取环境变量时它就作为System.getProperties()方法的返回值。
我们在很多**需要避免硬编码的应用场景**下需要使用properties文件来加载程序需要的配置信息，比如JDBC、MyBatis框架等。Properties类则是properties文件和程序的中间桥梁，不论是从properties文件读取信息还是写入信息到properties文件都要经由Properties类。

```java

    public static void main(String[] args) throws Exception {

        Properties props = new Properties();

		//向Properties中添加属性
        props.setProperty("username", "yeeku");
        props.setProperty("password", "123456");
		//将Properties中的key-value对保存到a.ini文件中
        props.store(new FileOutputStream("a.ini"), "comment line");
		//①//新建一个Properties对象
        Properties props2 = new Properties();
		//向Properties中添加属性
        props2.setProperty("gender", "male");
		//将a.ini文件中的key-value对追加到props2中
        props2.load(new FileInputStream("a.ini"));
		//②
        System.out.println(props2);

    }
// 加载获取Connection 
public Connection getConnection() throws Exception{
      Properties info=new Properties();
      info.load(this.getClass().getClassLoader().getResourceAsStream("jdbc.properties"));
             String  driver=info.getProperty("driver");
             String jdbcUrl=info.getProperty("jdbcUrl");
             String user=info.getProperty("user");
             String password=info .getProperty("password");
             Class.forName(driver);
             Connection connection=DriverManager.getConnection(jdbcUrl,user,password);
             return connection;
       }

```

可以把Map对象和属性文件关联起来，从而可以把Map对象中的key-value对写入属性文件中，也可以把属性文件中的“属性名=属性值”加载到Map对象中。由于属性文件里的属性名、属性值只能是字符串类型，所以Properties里的key、value都是字符串类型。该类提供了如下三个方法来修改Properties里的key、value值。

更多参看Properties类的使用：https://www.cnblogs.com/leeego-123/p/11535967.html

### 泛型：

要使代码能够应用于“某种具体的类型而不是一个具体的接口或类”。增加了泛型支持后的集合，完全可以记住集合中元素的类型，并可以在编译时检查集合中元素的类型，如果试图向集合中添加不满足类型要求的对象，编译器就会提示错误。增加泛型后的集合，可以让代码更加简洁，程序更加健壮（Java泛型可以保证如果程序在编译时没有发出警告，运行时就不会产生ClassCastException异常）。除此之外，Java泛型还增强了枚举类、反射等方面的功能

当创建带泛型声明的自定义类，为该类定义构造器时，构造器名还是原来的类名，不要增加泛型声明。

例如，为Apple<T>类定义构造器，其构造器名依然是Apple，而不是Apple<T>！调用该构造器时却可以使用Apple<T>的形式，当然应该为T形参传入实际的类型参数。Java 7提供了菱形语法，允许省略<>中的类型实参。

#### 元组：

将一组对象直接打包存储于其中的一个单一的对象。这个容器对象允许读取其中的元素，但是不允许对其存放新的对象（这也称为：数据传送对象）

比如一个二维的元组：

```java
public class TwoTuple<A,B>{
	public final A first;
	public final B second;
	public TwoTuple(A a, B b){ first = a; second = b;} 
	public String toStirng() {
		return "("+first+","+second+")";
	}
}

```

#### 构造一个堆栈类：

下面的例子使用了末端哨兵来判断堆栈何时为空，这个末端哨兵在构造LinkedStack时创建，然后每调用一次push()方法，就会创建一个Node<T>对象，并将其链接到前一个Node<T>对象。

```java
public class LinkedStack<T> {
  private static class Node<U> {
    U item;
    Node<U> next;
    Node() { item = null; next = null; }
    Node(U item, Node<U> next) {
      this.item = item;
      this.next = next;
    }
    boolean end() { return item == null && next == null; }
  }
  private Node<T> top = new Node<T>(); // End sentinel
  public void push(T item) {
    top = new Node<T>(item, top);
  }	
  public T pop() {
    T result = top.item;
    if(!top.end())
      top = top.next;
    return result;
  }
  public static void main(String[] args) {
    LinkedStack<String> lss = new LinkedStack<String>();
    for(String s : "Phasers on stun!".split(" "))
      lss.push(s);
    String s;
    while((s = lss.pop()) != null)
      System.out.println(s);
  }
} /* Output:
stun!
on
Phasers
*///:~

```

#### 泛型接口：

```
public interface Generator<T> {T next();}
```

用泛型接口实现生成Fibonacci数列：

```java
public class Fibonacci implements Generator<Interger> {
	private int count=0;
	public Integer next(){return fib(count++);}
	private int fib(int n){
		if(n<2) return 1;
		return fib(n-2)+fib(n-1);
	}
	public static void main(String[] args){
		Fibonacci gen = new Fibonacci();
		for(int i=0;i<18;i++){
			System.out.println(gen.next()+" ");
		}
	}
}
/* output
1 1 2 3 5 8 13 21 34 55 89 144 233 377 ...
```

#### 泛型方法：

泛型方法使得该方法能够独立于类而产生变化。无论何时，只要你能做到，就应该尽量使用泛型方法。也就是说如果使用泛型方法可以取代整个类泛型化，那么就应该使用泛型方法，因为他可以使得事情更清楚明白。

定义泛型方法： 修饰符 <T> 返回参数 方法名（接收参数）{}

```java
public class MyUtils{
// <T> 标明是一个泛型方法。
public static <T> void copy(Collection<T> dest , Collection<? extends T> src){..
    .} //①

public static <T> T copy(Collection<? super T> dest , Collection<T> src){.
    ..} //②

}

泛型中的类型转换，在未知类型时不能进行强转：用下面方式应该先判断

List<?>[] lsa=new ArrayList<?>[10];

Object[] oa=(Object[]) lsa;

List<Integer> li=new ArrayList<Integer>();

li.add(new Integer(3));

oa[1]=li;

Object target=lsa[1].get(0);

if (target instanceof String){

// 下面代码安全了
String s=(String) target;
}
```

#### 使用泛型构建一个Set的使用工具（交集，差集）：

```java
import java.util.*;

public class Sets {
  // 将两个set合并为一个set  
  public static <T> Set<T> union(Set<T> a, Set<T> b) {
    Set<T> result = new HashSet<T>(a);
    result.addAll(b);
    return result;
  }
  // 返回共有的交集
  public static <T>
  Set<T> intersection(Set<T> a, Set<T> b) {
    Set<T> result = new HashSet<T>(a);
    result.retainAll(b);
    return result;
  }	
  // 从superset移除subset包含的元素  
  // Subtract subset from superset:
  public static <T> Set<T>
  difference(Set<T> superset, Set<T> subset) {
    Set<T> result = new HashSet<T>(superset);
    result.removeAll(subset);
    return result;
  }
  // 返回交集之外的所有元素
  // Reflexive--everything not in the intersection:
  public static <T> Set<T> complement(Set<T> a, Set<T> b) {
    return difference(union(a, b), intersection(a, b));
  }
} ///:~

```

#### 泛型擦除：

在泛型代码内部，无法获取任何有关泛型参数类型的信息。java使用泛型擦除，比如List<String> 和List<Integer> 在运行时都是相同的类型，均被擦除为"原生"类型即List. 泛型擦除就是被擦除为父类。

### 异常：

Java的异常机制主要依赖于try、catch、finally、throw和throws五个关键字，其中try关键字后紧跟一个花括号扩起来的代码块（花括号不可省略），简称try块，它里面放置可能引发异常的代码。catch后对应异常类型和一个代码块，用于表明该catch块用于处理这种类型的代码块。多个catch块后还可以跟一个finally块，finally块用于回收在try块里打开的物理资源，异常机制会保证finally块总被执行。throws关键字主要在方法签名中使用，用于声明该方法可能抛出的异常；而throw用于抛出一个实际的异常，throw可以单独作为语句使用，抛出一个具体的异常对象。

#### Checked异常和Runtime异常

Checked异常和Runtime异常， Java认为Checked异常都是可以在编译阶段被处理的异常，所以它强制程序处理所有的Checked异常；而Runtime异常则无须处理。Checked异常可以提醒程序员需要处理所有可能发生的异常，但Checked异常也给编程带来一些烦琐之处，所以Checked异常也是Java领域一个备受争论的话题。

不要在finally块中使用如return或throw等导致方法终止的语句，（throw语句将在后面介绍），一旦在finally块中使用了return或throw语句，将会导致try块、catch块中的return、throw语句失效。看如下程序。

```java
public class FinallyFlowTest {

    public static void main(String[] args) throws Exception {
        boolean a = test();
        System.out.println(a);
    }
    public static boolean test() {
        try {
            // 因为finally块中包含了return语句
            // 所以下面的return语句失去作用return true;
        } finally {
            return false;
        }
    }
}
```



<font color="red">如果有finally块，系统立即开始执行finally块——只有当finally块执行完成后，系统才会再次跳回来执行try块、catch块里的return或throw语句；如果finally块里也使用了return或throw等导致方法终止的语句，finally块已经终止了方法，系统将不会跳回去执行try块、catch块里的任何代码</font>

#### throws语句

- 用了throws来声明抛出IOException异常，一旦使用throws语句声明抛出该异常，程序就无须使用try...catch块来捕获该异常了
- 使用throws声明抛出异常时有一个限制，就是方法重写时“两小”中的一条规则：子类方法声明抛出的异常类型应该是父类方法声明抛出的异常类型的子类或相同，子类方法声明抛出的异常不允许比父类方法声明抛出的异常多。
- 如果需要在程序中自行抛出异常，则应使用throw语句：throw语句可以单独使用，throw语句抛出的不是异常类，而是一个异常实例，而且每次只能抛出一个异常实例。throw语句的语法格式如下：throw ExceptionInstance;

#### 异常使用指南：

1. 在恰当的级别处理问题。
2. 解决问题并且重新调用产生异常的方法
3. 进行少许修补，然后绕过异常发生的地方继续执行
4. 用别的数据进行计算，以代替方法预计返回的值
5. 把当前运行环境下能做的事情尽量做完，然后把相同的异常重新抛到更高层
6. 把当前运行环境下能做的事情尽量做完，然后把不同的异常重新抛到更高层
7. 终止程序
8. 进行简化
9. 让类库和程序更安全

#### 异常常见的面试题：

1．java中用来抛出异常的关键字是什么？

throw

2．异常和Error（错误）的区别？

error：是不可捕捉到的，无法采取任何恢复的操作，顶多只能显示错误信息。
Exception ：表示可恢复的例外，这是可捕捉到的

3．什么是异常？

所谓异常是指程序在运行过程中发生的一些不正常事件。（如：除0溢出，数组下标越界，所读取的文件不存在）

4．什么类是所有异常类的父类

Throwable类

5．java虚拟机能自动处理的异常是什么？

运行异常

6．Try-catch-finally的执行过程

（1）try{}语句块中放的是要检测的java代码，可能有会抛出异常，也可能会正常执行
（2）catch（异常类型）{}块是当java运行时系统接收到try块中所抛出异常对象时，会寻找处理这一异常catch块来进行处理（可以有多个catch块）
（3）finally{}不管系统有没有抛出异常都会去执行，一般用来释放资源。除了在之前执行了System.exit（0）

7．常见的异常？你的理解。

常见异常：RuntimeException,IOException,SQLException,ClassNotFoundException

8．final, finally, finalize的区别。

final用于声明属性，方法和类，分别表示属性不可交变，方法不可覆盖，类不可继承。
**finally是异常处理语句结构的一部分，表示总是执行。**
**finalize是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，供垃圾收集时的其他资源回收，例如关闭文件等。（**在垃圾回收的时候会调用被回收对象的此方法。**）**

9．Java中的异常处理机制的简单原理和应用。

​     当JAVA程序违反了JAVA的语义规则时，JAVA虚拟机就会将发生的错误表示为一个异常。违反语义规则包括2种情况。一种是JAVA类库内置的语义检查。例如数组下标越界,会引发IndexOutOfBoundsException;访问null的对象时会引发NullPointerException。另一种情况就是JAVA允许程序员扩展这种语义检查，程序员可以创建自己的异常，并自由选择在何时用throw关键字引发异常。所有的异常都是java.lang.Thowable的子类。

10．运行时异常与一般异常有何异同？

Java提供了两类主要的异常:运行时异常runtime exception和一般异常checked exception。checked 异常。对于后者这种异常，JAVA要求程序员对其进行catch。所以，面对这种异常不管我们是否愿意，只能自己去写一大堆catch块去处理可能的异常。
运行时异常我们可以不处理。这样的异常由虚拟机接管。出现运行时异常后，系统会把异常一直往上层抛，一直遇到处理代码。如果不对运行时异常进行处理，那么出现运行时异常之后，要么是线程中止，要么是主程序终止。

11、你平时在项目中是怎样对异常进行处理的。

（1）尽量避免出现runtimeException 。例如对于可能出现空指针的代码，带使用对象之前一定要判断一下该对象是否为空，必要的时候对runtimeException

也进行try catch处理。

（2）进行try catch处理的时候要在catch代码块中对异常信息进行记录，通过调用异常类的相关方法获取到异常的相关信息，返回到web端，不仅要给用户良好

的用户体验，也要能帮助程序员良好的定位异常出现的位置及原因。例如，以前做的一个项目，程序遇到异常页面会显示一个图片告诉用户哪些操作导致程序出现

了什么异常，同时图片上有一个按钮用来点击展示异常的详细信息给程序员看的。

### Jdbc:

JDBC的全称是JavaDatabaseConnectivity，即Java数据库连接，它是一种可以执行SQL语句的JavaAPI。程序可通过JDBC API连接到关系数据库，并使用结构化查询语言（SQL，数据库标准的查询语言）来完成对数据库的查询、更新。

JDBC驱动通常有如下4种类型。

- 第1种JDBC驱动：称为JDBC–ODBC桥，这种驱动是最早实现的JDBC驱动程序，主要目的是为了快速推广JDBC。这种驱动将JDBC API映射到ODBC API。JDBC-ODBC也需要驱动，这种驱动由Sun公司提供实现。
- 第2种JDBC驱动：直接将JDBC API映射成数据库特定的客户端API。这种驱动包含特定数据库的本地代码，用于访问特定数据库的客户端。
- 第3种JDBC驱动：支持三层结构的JDBC访问方式，主要用于Applet阶段，通过Applet访问数据库。
- 第4种JDBC驱动：是纯Java的，直接与数据库实例交互。这种驱动是智能的，它知道数据库使用的底层协议。这种驱动是目前最流行的JDBC驱动。

#### SQL的全称是Structured Query Language，也就是结构化查询语言。

SQL是操作和检索关系数据库的标准语言，标准的SQL语句可用于操作任何关系数据库。

使用SQL语句，程序员和数据库管理员（DBA）可以完成如下任务。

- 在数据库中检索信息。
- 对数据库的信息进行更新。
- 改变数据库的结构。
- 更改系统的安全设置。
- 增加用户对数据库或表的许可权限。

在上面5个任务中，一般程序员可以管理前3个任务，后面2个任务通常由DBA来完成

查询语句：主要由select关键字完成，查询语句是SQL语句中最复杂、功能最丰富的语句。

- DML（Data Manipulation Language，数据操作语言）语句：主要由insert、update和delete三个关键字完成。
- DDL（Data Definition Language，数据定义语言）语句：主要由create、alter、drop和truncate四个关键字完成。
- DCL（Data Control Language，数据控制语言）语句：主要由grant和revoke两个关键字完成。

事务控制语句：主要由commit、rollback和savepoint三个关键字完成。

SQL语句的关键字不区分大小写，也就是说，create和CREATE的作用完全一样。

在上面5种SQL语句中，DCL语句用于为数据库用户授权，或者回收指定用户的权限，通常无须程序员操作，所以本节不打算介绍任何关于DCL的知识。

在SQL命令中也可能需要使用标识符，标识符可用于定义表名、列名，也可用于定义变量等。这些标识符的命名规则如下。

- 标识符通常必须以字母开头。
- 标识符包括字母、数字和三个特殊字符（# _ $）。
- 不要使用当前数据库系统的关键字、保留字，通常建议使用多个单词连缀而成，单词之间以_分隔。
- 同一个模式下的对象不应该同名，这里的模式指的是外模式。

#### ResultSet：

里包含一个getMetaData()方法，该方法返回该ResultSet对应的ResultSetMetaData对象。一旦获得了ResultSetMetaData对象，就可通过ResultSetMetaData提供的大量方法来返回ResultSet的描述信息。常用的方法有如下3个。

- int getColumnCount()：返回该ResultSet的列数量。
- String getColumnName(int column)：返回指定索引的列名。
- int getColumnType(int column)：返回指定索引的列类型。

例代码块：

```java
    ResultSet rs = stmt.executeQuery(sqlField.getText()))

        {

// 取出ResultSet的MetaData

        ResultSetMetaData rsmd=rs.getMetaData();

        Vector<String> columnNames=new Vector<>();

        Vector<Vector<String>>data=new Vector<>();

// 把ResultSet的所有列名添加到Vector里

        for(int i=0;i<rsmd.getColumnCount();i++){

        columnNames.add(rsmd.getColumnName(i+1));

        }

// 把ResultSet的所有记录添加到Vector里

        while(rs.next()){

        Vector<String> v=new Vector<>();

        for(int i=0;i<rsmd.getColumnCount();i++){

        v.add(rs.getString(i+1));

        }

        data.add(v);

        }
```



#### 事务:

自动提交和开启事务恰好相反，如果开启自动提交就是关闭事务；关闭自动提交就是开启事务。

Connection也提供了设置中间点的方法：setSavepoint()，Connection提供了两个方法来设置中间点。

- Savepoint setSavepoint()：在当前事务中创建一个未命名的中间点，并返回代表该中间点的Savepoint对象。
- Savepoint setSavepoint(String name)：在当前事务中创建一个具有指定名称的中间点，并返回代表该中间点的Savepoint对象。

通常来说，设置中间点时没有太大的必要指定名称，因为Connection回滚到指定中间点时，并不是根据名字回滚的，而是根据中间点对象回滚的。Connection提供了rollback(Savepoint savepoint)方法来回滚到指定中间点。

下面程序通过DatabaseMetaData分析了当前Connection连接对应数据库的一些基本信息，包括当前数据库包含多少数据表，存储过程，student_table表的数据列、主键、外键等信息。

```java
public class DatabaseMetaDataTest {

    private String driver;

    private String url;

    private String user;

    private String pass;

    public void initParam(String paramFile) throws Exception {

// 使用Properties类来加载属性文件

        Properties props = new Properties();

        props.load(new FileInputStream(paramFile));

        driver = props.getProperty("driver");

        url = props.getProperty("url");

        user = props.getProperty("user");

        pass = props.getProperty("pass");

    }

    public void info() throws Exception {

// 加载驱动

        Class.forName(driver);

        try (

// 获取数据库连接

                Connection conn = DriverManager.getConnection(url

                        , user, pass)) {

// 获取DatabaseMetaData对象

            DatabaseMetaData dbmd = conn.getMetaData();

// 获取MySQL支持的所有表类型ResultSet rs=dbmd.getTableTypes();System.out.println("--MySQL支持的表类型信息--");

            printResultSet(rs);

// 获取当前数据库的全部数据表rs=dbmd.getTables(null,null, "%" , new String[]{"TABLE"});System.out.println("--当前数据库里的数据表信息--");

            printResultSet(rs);

// 获取student_table表的主键rs=dbmd.getPrimaryKeys(null , null, "student_table");System.out.println("--student_table表的主键信息--");

            printResultSet(rs);

// 获取当前数据库的全部存储过程rs=dbmd.getProcedures(null , null, "%");System.out.println("--当前数据库里的存储过程信息--");

            printResultSet(rs);

// 获取teacher_table表和student_table表之间的外键约束

            rs = dbmd.getCrossReference(null, null, "teacher_table", null, null, "student_table");

            System.out.println("--teacher_table表和student_table表之间" + "的外键约束--");

            printResultSet(rs);

// 获取student_table表的全部数据列

            rs = dbmd.getColumns(null, null, "student_table", "%");

            System.out.println("--student_table表的全部数据列--");

            printResultSet(rs);

        }

    }

    public void printResultSet(ResultSet rs) throws SQLException {

        ResultSetMetaData rsmd = rs.getMetaData();

// 打印ResultSet的所有列标题

        for (int i = 0; i < rsmd.getColumnCount(); i++) {

            System.out.print(rsmd.getColumnName(i + 1) + "\t");

        }

        System.out.print("\n");

// 打印ResultSet里的全部数据

        while (rs.next()) {

            for (int i = 0; i < rsmd.getColumnCount(); i++) {

                System.out.print(rs.getString(i + 1) + "\t");

            }

            System.out.print("\n");

        }

        rs.close();

    }

    public static void main(String[] args)

            throws Exception {

        DatabaseMetaDataTest dt = new DatabaseMetaDataTest();

        dt.initParam("mysql.ini");

        dt.info();

    }

}
```

![拖曳以移動](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==

#### DBCP：

Apache软件基金组织下的开源连接池实现，该连接池依赖该组织下的另一个开源系统：common-pool。如果需要使用该连接池实现，则应在系统中增加如下两个jar文件。

commons-dbcp.jar：连接池的实现。 commons-pool.jar：连接池实现的依赖库。

- 创建数据源对象
- BasicDataSource ds=new BasicDataSource();
- // 设置连接池所需的驱动
- ds.setDriverClassName("com.mysql.jdbc.Driver");
- // 设置连接数据库的URL
- ds.setUrl("jdbc:mysql://localhost:3306/javaee");
- // 设置连接数据库的用户名
- ds.setUsername("root");
- // 设置连接数据库的密码
- ds.setPassword("pass");
- // 设置连接池的初始连接数
- ds.setInitialSize(5);
- // 设置连接池最多可有多少个活动连接数
- ds.setMaxActive(20);
- // 设置连接池中最少有2个空闲的连接
- ds.setMinIdle(2)
- // 通过数据源获取数据库连接
- Connection conn=ds.getConnection();

#### c3p0：

C3P0数据源性能更胜一筹，Hibernate就推荐使用该连接池。C3P0连接池不仅可以自动清理不再使用的Connection，还可以自动清理Statement和ResultSet

- // 创建连接池实例     ComboPooledDataSource ds=new ComboPooledDataSource();
- // 设置连接池连接数据库所需的驱动   ds.setDriverClass("com.mysql.jdbc.Driver");
- // 设置连接数据库的URL    ds.setJdbcUrl("jdbc:mysql://localhost:3306/javaee");
- // 设置连接数据库的用户名     ds.setUser("root");
- // 设置连接数据库的密码     ds.setPassword("32147");
- // 设置连接池的最大连接数   ds.setMaxPoolSize(40);
- // 设置连接池的最小连接数    ds.setMinPoolSize(2);
- // 设置连接池的初始连接数    ds. setInitialPoolSize(10);
- // 设置连接池的缓存Statement的最大数    ds.setMaxStatements(180);

在程序中创建C3P0连接池的方法与前面介绍的创建DBCP连接池的方法基本类似，此处不再解释。

一旦获取了C3P0连接池之后，程序同样可以通过如下代码来获取数据库连接。

// 获得数据库连接

Connection conn=ds.getConnection();

### 注解：

Annotation提供了一种为程序元素设置元数据的方法，从某些方面来看，Annotation就像修饰符一样，可用于修饰包、类、构造器、方法、成员变量、参数、局部变量的声明，这些信息被存储在Annotation的“name=value”对中。

注意：

Annotation是一个接口，程序可以通过反射来获取指定程序元素的Annotation对象，然后通过Annotation对象来取得注释里的元数据。

有的Annotation指的是java.lang.Annotation接口，有的指的是注释本身：

4个基本的Annotation如下：

- @Override
- @Deprecated
- @SuppressWarnings
- @SafeVarargs
- @Override只能作用于方法，不能作用于其他程序元素。
- @Deprecated用于表示某个程序元素（类、方法等）已过时，当其他程序使用已过时的类、方法时，编译器将会给出警告
- @SuppressWarnings指示被该Annotation修饰的程序元素（以及该程序元素中的所有子元素）取消显示指定的编译器警告
- java.lang.annotation包下提供了4个Meta Annotation（元Annotation），这4个元Annotation都用于修饰其他的Annotation定义
- @Retention只能用于修饰一个Annotation定义，用于指定被修饰的Annotation可以保留多长时间，
- @Retention包含一个RetentionPolicy类型的value成员变量，所以使用@Retention时必须为该value成员变量指定值。
- // 定义下面的Testable Annotation保留到运行时
- @Retention(value=RetentionPolicy.RUNTIME)
- public @interface Testable{}
- //定义下面的Testable Annotation将被编译器直接丢弃
- @Retention(RetentionPolicy.SOURCE)
- public @interface Testable{}
- 如果Annotation里只有一个value成员变量，使用该Annotation时可以直接在Annotation后的括号里指定value成员变量的值，
- 无须使用name=value的形式。
- @Target也只能修饰一个Annotation定义，它用于指定被修饰的Annotation能用于修饰哪些程序单元。
- @Target(ElementType.METHOD)
- public @interface Testable { }…………
- @Documented用于指定被该元Annotation修饰的Annotation类将被javadoc工具提取成文档，如果定义Annotation类时使用了@Documented修饰，则所有使用该Annotation修饰的程序元素的API文档中将会包含该Annotation说明

### IO操作：

Java的IO流是实现输入/输出的基础，它可以方便地实现数据的输入/输出操作，在Java中把不同的输入/输出源（键盘、文件、网络连接等）抽象表述为“流”（stream），通过流的方式允许Java程序使用相同的方式来访问不同的输入/输出源。stream是从起源（source）到接收（sink）的有序数据。

Java把所有传统的流类型（类或抽象类）都放在java.io包中，用以实现输入/输出功能。

Java的IO流的40多个类都是从如下4个抽象基类派生的：

- InputStream/Reader：所有输入流的基类，前者是字节输入流，后者是字符输入流。
- OutputStream/Writer：所有输出流的基类，前者是字节输出流，后者是字符输出流。

#### FIle：

```java
public class FileTest {

    public static void main(String[] args)

            throws IOException {

// 以当前路径来创建一个File对象

        File file = new File(".");

// 直接获取文件名，输出一点

        System.out.println(file.getName());

// 获取相对路径的父路径可能出错，下面代码输出nullSystem.out.println(file.getParent());

// 获取绝对路径

        System.out.println(file.getAbsoluteFile());

// 获取上一级路径

        System.out.println(file.getAbsoluteFile().getParent());

// 在当前路径下创建一个临时文件

        File tmpFile = File.createTempFile("aaa", ".txt", file);

// 指定当JVM退出时删除该文件

        tmpFile.deleteOnExit();

// 以系统当前时间作为新文件名来创建新文件

        File newFile = new File(System.currentTimeMillis() + "");

        System.out.println("newFile对象是否存在：" + newFile.exists());

// 以指定newFile对象来创建一个文件

        newFile.createNewFile();

// 以newFile对象来创建一个目录，因为newFile已经存在

// 所以下面方法返回false，即无法创建该目录

        newFile.mkdir();

// 使用list()方法列出当前路径下的所有文件和路径

        String[] fileList = file.list();

        System.out.println("====当前路径下所有文件和路径如下====");

        for (String fileName : fileList) {

            System.out.println(fileName);

        }

// listRoots()静态方法列出所有的磁盘根路径

        File[] roots = File.listRoots();

        System.out.println("====系统所有根路径如下====");

        for (File root : roots) {

            System.out.println(root);

        }

    }

}
```
运行上面程序，可以看到程序列出当前路径的所有文件和路径时，列出了程序创建的临时文件，但程序运行结束后，aaa.txt临时文件并不存在，因为程序指定虚拟机退出时自动删除该文件。

上面程序还有一点需要注意，当使用相对路径的File对象来获取父路径时可能引起错误，因为该方法返回将File对象所对应的目录名、文件名里最后一个子目录名、子文件名删除后的结果

#### 字节流和字符流：

- OutputStream和Writer
- InputStream和Reader

java虚拟机读取其他进程

下面程序示范了读取其他进程的输出信息。

```java
public class ReadFromProcess {

    public static void main(String[] args)throws IOException {

// 运行javac命令，返回运行该命令的子进程Process p=Runtime.getRuntime().exec("javac");try(

// 以p进程的错误流创建BufferedReader对象

// 这个错误流对本程序是输入流，对p进程则是输出流BufferedReader br=new BufferedReader(newInputStreamReader(p.getErrorStream()))) {

        String buff = null;

// 采取循环方式来读取p进程的错误输出

        while ((buff = br.readLine()) != null) {

            System.out.println(buff);

        }

    }

}
```



随机读取写：

下面程序实现了向指定文件、指定位置插入内容的功能。

```java

public class InsertContent {

    public static void insert(String fileName, long pos

            , String insertContent) throws IOException {

        File tmp = File.createTempFile("tmp", null);

        tmp.deleteOnExit();

        try (

                RandomAccessFile raf = new RandomAccessFile(fileName, "rw");

// 创建一个临时文件来保存插入点后的数据

                FileOutputStream tmpOut = new FileOutputStream(tmp);

                FileInputStream tmpIn = new FileInputStream(tmp)) {

            raf.seek(pos);

// ------下面代码将插入点后的内容读入临时文件中保存------

            byte[] bbuf = new byte[64];

// 用于保存实际读取的字节数

            int hasRead = 0;

// 使用循环方式读取插入点后的数据

            while ((hasRead = raf.read(bbuf)) > 0) {

// 将读取的数据写入临时文件

                tmpOut.write(bbuf, 0, hasRead);

            }

// ----------下面代码用于插入内容----------

// 把文件记录指针重新定位到pos位置

            raf.seek(pos);

// 追加需要插入的内容

            raf.write(insertContent.getBytes());

// 追加临时文件中的内容

            while ((hasRead = tmpIn.read(bbuf)) > 0) {

                raf.write(bbuf, 0, hasRead);

            }

        }

    }

    public static void main(String[] args)

            throws IOException {

        insert("InsertContent.java", 45, "插入的内容\r\n");

    }

}
```
上面程序中使用File的createTempFile(String prefix, String suffix)方法创建了一个临时文件（该临时文件将在JVM退出时被删除），用以保存被插入文件的插入点后面的内容。程序先将文件中插入点后的内容读入临时文件中，然后重新定位到插入点，将需要插入的内容添加到文件后面，最后将临时文件的内容添加到文件后面，通过这个过程就可以向指定文件、指定位置插入内容。

每次运行上面程序，都会看到向InsertContent.java中插入了一行字符串。

流：是一组有序的数据序列，流可以分为：Input流

Output流，Java中有数据流来处理输出输入模式，

输入模式：由，文件，网络，压缩包，其他， -------》 目的地

输出模式：源  据流数  -----------》    文件，网络，压缩包，其他，

1所有的输入流都是抽象类InputStream《字节流》或Reader《字符》的子类，

2所有的输出流都是抽象类OutputStream<字节输出流>或Writer《字符输出流》的子类

字节流的一些常见的操作形式：

**程序----》****FileOutputStream---》****文件---》****F**ileInputStream --》**控制台**

​                 **write()****方法**                                               **read()****方法；**

**程序----》****FileOutputStream-----》****文件---》****F**ileInputStream --》**控制台**

​                 **OutputStreamWriter;   inputStreamReader;**

​                       **write()****方法**           **read()****方法；**

**文件** **FileinputStream(read****方法****)****文件**

**FileOutputStream****（****write****方法）**

字节流中的缓冲流是有些技巧：

BufferedInputStream();

BufferedOutputStream();必须flush()刷新

**字符流：**

BufferedReader();

BufferedWriter();必须flush()刷新；

这些带缓冲流的是不需要多次与文件进行交互；它能一次性读取很多然后在其要读取时直接从内存里读取；内置一个buff[size]数组；字节行的；char[size]形的可以指定大小要在构造方法中实现；

字符流中的FileReader是InputStreamReader的子类；

读取字符时有对编码集的影响；不同的平台的编码集有时可能不一样；

在Java中的对字符和字节的操作主要应用于对磁盘的操作，及时的把数据保存到磁盘中而不是内存这样的话可以让数据不丢失，保证了数据的完整性，不在是指在内存中的操作了。

#### 序列化

对象的序列化（Serialize）指将一个Java对象写入IO流中，与此对应的是，对象的反序列化（Deserialize）则指从IO流中恢复该Java对象。

如果需要让某个对象支持序列化机制，则必须让它的类是可序列化的（serializable）。为了让某个类是可序列化的，该类必须实现如下两个接口之一：

Serializable， Externalizable

Java的很多类已经实现了Serializable，该接口是一个标记接口，实现该接口无须实现任何方法，它只是表明该类的实例是可序列化的。

所有可能在网络上传输的对象的类都应该是可序列化的，否则程序将会出现异常，比如RMI （Remote Method Invoke，即远程方法调用，是JavaEE的基础）过程中的参数和返回值；所有需要保存到磁盘里的对象的类都必须可序列化，比如Web应用中需要保存到HttpSession或ServletContext属性的Java对象。

通过在Field前面使用transient关键字修饰，可以指定Java序列化时无须理会该Field。但被transient修饰的Field将被完全隔离在序列化机制之外，这样导致在反序列化恢复Java对象时无法取得该Field值。

#### Charset类：

JDK 1.4提供了Charset来处理字节序列和字符序列（字符串）之间的转换关系，该类包含了用于创建解码器和编码器的方法，还提供了获取Charset所支持字符集的方法，Charset类是不可变的。使用了CharsetEncoder和CharsetDecoder完成了ByteBuffer和CharBuffer之间的转换。

```java
public class CharsetTransform {

    public static void main(String[] args)

            throws Exception {

// 创建简体中文对应的Charset

        Charset cn = Charset.forName("GBK");

// 获取cn对象对应的编码器和解码器

        CharsetEncoder cnEncoder = cn.newEncoder();

        CharsetDecoder cnDecoder = cn.newDecoder();

// 创建一个CharBuffer对象

        CharBuffer cbuff = CharBuffer.allocate(8);

        cbuff.put('孙');

        cbuff.put('悟');

        cbuff.put('空');

        cbuff.flip();

// 将CharBuffer中的字符序列转换成字节序列ByteBuffer bbuff=cnEncoder.encode(cbuff);// 循环访问ByteBuffer中的每个字节

        for (int i = 0; i < bbuff.capacity(); i++) {

            System.out.print(bbuff.get(i) + " ");

        }

// 将ByteBuffer的数据解码成字符序列System.out.println("\n" + cnDecoder.decode(bbuff)); }

    }
}
```

上面程序中的两行粗体字代码分别实现了将CharBuffer转换成ByteBuffer，将ByteBuffer转换成CharBuffer的功能。实际上，Charset类也提供了如下3个方法。

- CharBuffer decode(ByteBuffer bb)：将ByteBuffer中的字节序列转换成字符序列的便捷方法。
- ByteBuffer encode(CharBuffer cb)：将CharBuffer中的字符序列转换成字节序列的便捷方法。
- ByteBuffer encode(String str)：将String中的字符序列转换成字节序列的便捷方法。

也就是说，获取了Charset对象后，如果仅仅需要进行简单的编码、解码操作，其实无须创建CharsetEncoder和CharsetDecoder对象，直接调用Charset的encode()和decode()方法进行编码、解码即可。

#### 文件锁：

在NIO中，Java提供了FileLock来支持文件锁定功能，在FileChannel中提供的lock()/tryLock()方法可以获得文件锁FileLock对象，从而锁定文件。lock()和tryLock()方法存在区别：当lock()试图锁定某个文件时，如果无法得到文件锁，程序将一直阻塞；而tryLock()是尝试锁定文件，它将直接返回而不是阻塞，如果获得了文件锁，该方法则返回该文件锁，否则将返回null。

如果FileChannel只想锁定文件的部分内容，而不是锁定全部内容，则可以使用如下的lock()或tryLock()方法。

- lock(long position, long size, boolean shared)：对文件从position开始，长度为size的内容加锁，该方法是阻塞式的。
- tryLock(long position, long size, boolean shared)：非阻塞式的加锁方法。参数的作用与上一个方法类似。

当参数shared为true时，表明该锁是一个共享锁，它将允许多个进程来读取该文件，但阻止其他进程获得对该文件的排他锁。当shared为false时，表明该锁是一个排他锁，它将锁住对该文件的读写。程序可以通过调用FileLock的isShared来判断它获得的锁是否为共享锁。

注意：

直接使用lock()或tryLock()方法获取的文件锁是排他锁。

下面程序简单示范了Path接口的功能和用法。

```java

public class PathTest {

    public static void main(String[] args)

            throws Exception {

// 以当前路径来创建Path对象

        Path path = Paths.get(".");

        System.out.println("path里包含的路径数量：" + path.getNameCount());
        System.out.println("path的根路径：" + path.getRoot());

// 获取path对应的绝对路径

        Path absolutePath = path.toAbsolutePath();
        System.out.println(absolutePath);

// 获取绝对路径的根路径

        System.out.println("absolutePath的跟路径：" + absolutePath.getRoot());

// 获取绝对路径所包含的路径数量

        System.out.println("absolutePath里包含的路径数量：" + absolutePath.getNameCount());

        System.out.println(absolutePath.getName(3));

// 以多个String来构建Path对象Path path2=Paths.get("g:" , "publish" , "codes");

        System.out.println(path2);

    }

}
```


从上面程序可以看出，Paths提供了get(String first, String... more)方法来获取Path对象，Paths会将给定的多个字符串连缀成路径，比如Paths.get("g:" , "publish" , "codes")就返回g:\publish\codes路径。上面程序中的粗体字代码示范了Path接口的常用方法，读者可能对getNameCount()方法感到有点困惑，此处简要说明一下：它会返回Path路径所包含的路径名的数量，例如g:\publish\codes调用该方法就会返回3。

Files是一个操作文件的工具类，它提供了大量便捷的工具方法，下面程序简单示范了Files类的用法。

```java

public class FilesTest {

    public static void main(String[] args)

            throws Exception {

// 复制文件

        Files.copy(Paths.get("FilesTest.java"), new FileOutputStream("a.txt"));

// 判断FilesTest.java文件是否为隐藏文件

        System.out.println("FilesTest.java是否为隐藏文件：" + Files.isHidden(Paths.get("FilesTest.java")));

// 一次性读取FilesTest.java文件的所有行

        List <String> lines = Files.readAllLines(Paths.get("FilesTest.java"), Charset.forName("gbk"));
        System.out.println(lines);

// 判断指定文件的大小

        System.out.println("FilesTest.java的大小为：" + Files.size(Paths.get("FilesTest.java")));

        List <String> poem = new ArrayList <>();

        poem.add("水晶潭底银鱼跃");

        poem.add("清徐风中碧竿横");

// 直接将多个字符串内容写入指定文件中

        Files.write(Paths.get("pome.txt"), poem, Charset.forName("gbk"));

        FileStore cStore = Files.getFileStore(Paths.get("C:"));

// 判断C盘的总空间、可用空间

        System.out.println("C:共有空间：" + cStore.getTotalSpace());

        System.out.println("C:可用空间：" + cStore.getUsableSpace());

    }

}
```

上面程序中的粗体字代码简单示范了Files工具类的用法。从上面程序不难看出，Files类是一个高度封装的工具类，它提供了大量的工具方法来完成文件复制、读取文件内容、写入文件内容等功能——这些原本需要程序员通过IO操作才能完成的功能。

### 线程：

多线程则扩展了多进程的概念，使得同一个进程可以同时并发处理多个任务。线程（Thread）也被称作轻量级进程（Lightweight Process），线程是进程的执行单元。就像进程在操作系统中的地位一样，线程在程序中是独立的、并发的执行流。当进程被初始化后，主线程就被创建了。对于绝大多数的应用程序来说，通常仅要求有一个主线程，但也可以在该进程内创建多条顺序执行流，这些顺序执行流就是线程，每个线程也是互相独立的。

线程是进程的组成部分，一个进程可以拥有多个线程，一个线程必须有一个父进程。线程可以拥有自己的堆栈、自己的程序计数器和自己的局部变量，但不拥有系统资源，它与父进程的其他线程共享该进程所拥有的全部资源。因为多个线程共享父进程里的全部资源，因此编程更加方便；但必须更加小心，我们必须确保线程不会妨碍同一进程里的其他线程。

线程可以完成一定的任务，可以与其他线程共享父进程中的共享变量及部分环境，相互之间协同来完成进程所要完成的任务。

线程是独立运行的，它并不知道进程中是否还有其他线程存在。线程的执行是抢占式的，也就是说，当前运行的线程在任何时候都可能被挂起，以便另外一个线程可以运行。

一个线程可以创建和撤销另一个线程，同一个进程中的多个线程之间可以并发执行。

从逻辑角度来看，多线程存在于一个应用程序中，让一个应用程序中可以有多个执行部分同时执行，但操作系统无须将多个线程看作多个独立的应用，对多线程实现调度和管理以及资源分配。线程的调度和管理由进程本身负责完成。

简而言之，一个程序运行后至少有一个进程，一个进程里可以包含多个线程，但至少要包含一个线程。

线程比进程具有更高的性能，这是由于同一个进程中的线程都有共性——多个线程共享同一个进程虚拟空间。线程共享的环境包括：进程代码段、进程的公有数据等。利用这些共享的数据，线程很容易实现相互之间的通信。

当操作系统创建一个进程时，必须为该进程分配独立的内存空间，并分配大量的相关资源；但创建一个线程则简单得多，因此使用多线程来实现并发比使用多进程实现并发的性能要高得多。

总结起来，使用多线程编程具有如下几个优点。

进程之间不能共享内存，但线程之间共享内存非常容易。

系统创建进程时需要为该进程重新分配系统资源，但创建线程则代价小得多，因此使用多线程来实现多任务并发比多进程的效率高。

Java语言内置了多线程功能支持，而不是单纯地作为底层操作系统的调度方式，从而简化了Java的多线程编程。

当发生如下情况时，线程将会进入阻塞状态。

- 线程调用sleep()方法主动放弃所占用的处理器资源。
- 线程调用了一个阻塞式IO方法，在该方法返回之前，该线程被阻塞。
- 线程试图获得一个同步监视器，但该同步监视器正被其他线程所持有。关于同步监视器的知识、后面将有更深入的介绍。
- 线程在等待某个通知（notify）。
- 程序调用了线程的suspend()方法将该线程挂起。但这个方法容易导致死锁，所以应该尽量避免使用该方法。
- 当前正在执行的线程被阻塞之后，其他线程就可以获得执行的机会。被阻塞的线程会在合适的时候重新进入就绪状态，注意是就绪状态而不是运行状态。也就是说，被阻塞线程的阻塞解除后，必须重新等待线程调度器再次调度它。

针对上面几种情况，当发生如下特定的情况时可以解除上面的阻塞，让该线程重新进入就绪状态。

- 调用sleep()方法的线程经过了指定时间。
- 线程调用的阻塞式IO方法已经返回。
- 线程成功地获得了试图取得的同步监视器。
- 线程正在等待某个通知时，其他线程发出了一个通知。
- 处于挂起状态的线程被调用了resume()恢复方法。

线程从阻塞状态只能进入就绪状态，无法直接进入运行状态。而就绪和运行状态之间的转换通常不受程序控制，而是由系统线程调度所决定，当处于就绪状态的线程获得处理器资源时，该线程进入运行状态；当处于运行状态的线程失去处理器资源时，该线程进入就绪状态。但有一个方法例外，调用yield()方法可以让运行状态的线程转入就绪状态。关于yield()方法后面有更详细的介绍。

#### Thread类

提供了setPriority(int newPriority)、getPriority()方法来设置和返回指定线程的优先级，其中setPriority()方法的参数可以是一个整数，范围是1～10之间，也可以使用Thread类的如下3个静态常量。

MAX_PRIORITY：其值是10。MIN_PRIORITY：其值是1 NORM_PRIORITY：其值是5。

下面程序使用了setPriority()方法来改变主线程的优先级，并使用该方法改变了两个线程的优先级，从而可以看到高优先级的线程将会获得更多的执行机会。

```java
public class PriorityTest extends Thread {

// 定义一个有参数的构造器，用于创建线程时指定name

    public PriorityTest(String name) {

        super(name);

    }

    public void run() {

        for (int i = 0; i < 50; i++) {

            System.out.println(getName() + ",其优先级是："

                    + getPriority() + ",循环变量的值为:" + i);

        }

    }

    public static void main(String[] args) {

// 改变主线程的优先级Thread.currentThread().setPriority(6); for (int i=0 ; i < 30 ; i++ )

        {

            if (i == 10) {

                PriorityTest low = new PriorityTest("低级");

                low.start();

                System.out.println("创建之初的优先级:"

                        + low.getPriority());

// 设置该线程为最低优先级

                low.setPriority(Thread.MIN_PRIORITY);

            }

            if (i == 20) {

                PriorityTest high = new PriorityTest("高级");

                high.start();

                System.out.println("创建之初的优先级:"

                        + high.getPriority());

// 设置该线程为最高优先级

                high.setPriority(Thread.MAX_PRIORITY);

            }

        }

    }

}
```

上面程序中的第一行粗体字代码改变了主线程的优先级为6，这样由main线程所创建的子线程的优先级默认都是6，所以程序直接输出low、high两个线程的优先级时应该看到6。接着程序将low线程的优先级设为Priority.MIN_PRIORITY，将high线程的优先级设置为Priority.MAX_PRIORITY。

#### ThreadLocal类：

ThreadLocal不是解决对象的共享访问问题，ThreadLocal提供了线程内存储变量的能力，这些变量不同之处在于每一个线程读取的变量是对应的互相独立的。通过get和set方法就可以得到当前线程对应的值。

```java
public void set(T value) {
    	// 获取当前线程
        Thread t = Thread.currentThread();
    	// 实际存储的数据结构类型
        ThreadLocalMap map = getMap(t);
    	// 如果存在map就直接set没有则直接创建并set
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
    }
ThreadLocalMap getMap(Thread t) {
    // thread中维护了一个ThreadLocalMap
        return t.threadLocals;
    }
void createMap(Thread t, T firstValue) {
    //实例化一个新的ThreadLocalMap，并赋值给线程的成员变量threadLocals
        t.threadLocals = new ThreadLocalMap(this, firstValue);
    }
从上面代码可以看出每个线程持有一个ThreadLocalMap对象。每一个新的线程Thread都会实例化一个ThreadLocalMap并赋值给成员变量threadLocals，使用时若已经存在threadLocals则直接使用已经存在的对象。
static class ThreadLocalMap {

        /**
         * The entries in this hash map extend WeakReference, using
         * its main ref field as the key (which is always a
         * ThreadLocal object).  Note that null keys (i.e. entry.get()
         * == null) mean that the key is no longer referenced, so the
         * entry can be expunged from table.  Such entries are referred to
         * as "stale entries" in the code that follows.
         */
        static class Entry extends WeakReference<ThreadLocal<?>> {
            /** The value associated with this ThreadLocal. */
            Object value;

            Entry(ThreadLocal<?> k, Object v) {
                super(k);
                value = v;
            }
        }

// ThreadLocalMap的构造方法
ThreadLocalMap(ThreadLocal<?> firstKey, Object firstValue) {
			// 
            table = new Entry[INITIAL_CAPACITY];
            int i = firstKey.threadLocalHashCode & (INITIAL_CAPACITY - 1);
            table[i] = new Entry(firstKey, firstValue);
            size = 1;
            setThreshold(INITIAL_CAPACITY);
        }
通过上面的代码不难看出在实例化ThreadLocalMap时创建了一个长度为16的Entry数组。通过hashCode与length位运算确定出一个索引值i，这个i就是被存储在table数组中的位置。        
```

<font color="red">对于某一ThreadLocal来讲，他的索引值i是确定的，在不同线程之间访问时访问的是不同的table数组的同一位置即都为table[i]，只不过这个不同线程之间的table是独立的。</font>

<font color="red">对于同一线程的不同ThreadLocal来讲，这些ThreadLocal实例共享一个table数组，然后每个ThreadLocal实例在table中的索引i是不同的。</font>

当使用 ThreadLocal 维护变量时， ThreadLocal 为每个使用该变量的线程提供独立的变 量副本，所以每一个线程都可以独立地改变自己的副本，而不会影响其它线程所对应的副本。

ThreadLocal和Synchronized都是为了解决多线程中相同变量的访问冲突问题，不同的点是

- Synchronized是通过线程等待，牺牲时间来解决访问冲突
- ThreadLocal是通过每个线程单独一份存储空间，牺牲空间来解决冲突，并且相比于Synchronized，ThreadLocal具有线程隔离的效果，只有在线程内才能获取到对应的值，线程外则不能访问到想要的值。

正因为ThreadLocal的线程隔离特性，使他的应用场景相对来说更为特殊一些。在android中Looper、ActivityThread以及AMS中都用到了ThreadLocal。当某些数据是以线程为作用域并且不同线程具有不同的数据副本的时候，就可以考虑采用ThreadLocal。

#### 什么是线程安全？

- 多线程环境下，
- 对对象的访问不需加入额外的同步控制，
- 操作的数据结果依然是正确的。

##### 保证线程安全：

**1、synchronized**

synchronized关键字，就是用来控制线程同步的，保证我们的线程在多线程环境下，不被多个线程同时执行，确保我们数据的完整性，使用方法一般是加在方法上。

对于非静态代码块synchronized方法，锁的对象就是本身的this方法。

虽然加synchronized关键字，可以让我们的线程变得安全，但是我们在用的时候，也要注意缩小synchronized的使用范围，如果随意使用时很影响程序的性能，别的对象想拿到锁，结果你没用锁还一直把锁占用，这样就有点浪费资源。

**2、Lock**

先来说说它跟synchronized有什么区别吧，Lock是在Java1.6被引入进来的，Lock的引入让锁有了可操作性，什么意思？就是我们在需要的时候去手动的获取锁和释放锁，甚至我们还可以中断获取以及超时获取的同步特性，但是从使用上说Lock明显没有synchronized使用起来方便快捷。我们先来看下一般是如何使用的：

```java
private Lock lock = new ReentrantLock(); // ReentrantLock是Lock的子类

   private void method(Thread thread){
       lock.lock(); // 获取锁对象
       try {
           System.out.println("线程名："+thread.getName() + "获得了锁");
           // Thread.sleep(2000);
       }catch(Exception e){
           e.printStackTrace();
       } finally {
           System.out.println("线程名："+thread.getName() + "释放了锁");
           lock.unlock(); // 释放锁对象
       }
   }
```

这里跟synchronized不同的是，Lock获取的所对象需要我们亲自去进行释放，为了防止我们代码出现异常，所以我们的释放锁操作放在finally中，因为finally中的代码无论如何都是会执行的。

#### 线程的面试题：

##### **Q&A 1.什么是线程？**

 线程是程序执行运算的最小的基本单位

##### **Q&A2.线程与进程的区别？**

- 进程是系统中正在运行的一个应用程序，表示资源分配的基本单位。又是调度运行的基本单位。
- 一个线程只能属于一个进程，而一个进程可以拥有多个线程。
- 同一进程的所有线程共享该进程的所有资源。同一进程的多个线程共享代码段。

##### Q&A**3.如何保证线程安全？**

加锁是最简单的直接的方式。synchronized关键字

##### Q&A**4.如何使用线程？线程是如何启动的？**

- 实现runnable接口，

- 实现Callable接口，

- 继承Thread类，
- 线程启动，重写run方法然后调用start()即可开启一个线程。

##### Q&A**5.线程的几种状态？**

5种状态：创建，就绪，运行状态，阻塞，死亡

线程创建时new状态，调用start()进入runnable就绪状态，争夺cpu资源后进入running状态，由于某种原因进入（1）等待阻塞：运行的线程会释放占用的所有资源，jvm会把该线程放入“等待池”进入这个状态后，是不能自动唤醒的，必须依靠其他线程调用notify()或notifyAll()方法才能被唤醒，
（2）同步阻塞：运行的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM会把该线程放入“锁池”中。
（3）其他阻塞：运行的线程执行sleep()或join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。
当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入就绪状态。
当线程正常执行结束会进入dead状态（一个未捕获的异常也会使线程终止）

- yield()只是使当前线程重新回到runnable状态
- sleep()会让出cpu，不会释放锁
- join()会让出cpu，释放锁
- wait() 和 notify() 方法与suspend()和 resume()的区别在于wait会释放锁，suspend不会释放锁
- wait() 和 notify()只能运行在Synchronized代码块中，因为wait()需要释放锁，如果不在同步代码块中，就无锁可以释放
- 当线程调用wait()方法后会进入等待队列（进入这个状态会释放所占有的所有资源，与阻塞状态不同），进入这个状态后，是不能自动唤醒的，必须依靠其他线程调用notify()或notifyAll()方法才能被唤醒

##### **Q&A6.并发和并行的区别？**

并发：同一时段，多个任务都在执行，

并行：单位时间内多个任务同时执行。

##### Q&A**7.使用多线程带来什么问题可能？**

并发是为了能提高程序的执行效率提高程序运行速度，但是并发编程并不总是能提高程序运行速度的，而且并发编程可能会遇到很多问题，比如：内存泄漏、上下文切换、死锁

##### Q&A**8.什么是死锁？如何避免死锁？**

死锁:死锁是指两个或两个以上的进程在执行过程中，由于竞争资源或者由于彼此通信而造成的一种阻塞的现象，若无外力作用，它们都将无法推进下去。此时称系统处于死锁状态或系统产生了死锁，这些永远在互相等待的进程称为死锁进程

破坏死锁的四个条件：

1. 互斥条件：该资源任意一个时刻只由一个线程占用。 （无法破坏）
2. 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。（可以使用一次性申请所有资源）
3. 不剥夺条件:线程已获得的资源在末使用完之前不能被其他线程强行剥夺，只有自己使用完毕后才释放资源。（占用部分资源线程去申请其他资源，如果不能申请到就主动释放它占有的资源）
4. 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。（按顺序申请资源。）

##### Q&A **9.sleep（）和wait()方法的区别？**

- sleep方法会让出cpu没有释放锁，wait方法释放了锁。

- 两者都可以暂停线程的执行。
- Wait 通常被用于线程间交互/通信，sleep 通常被用于暂停执行。
- wait() 方法被调用后，线程不会自动苏醒，需要别的线程调用同一个对象上的 notify() 或者 notifyAll() 方法。sleep() 方法执行完成后，线程会自动苏醒。或者可以使用wait(long timeout)超时后线程会自动苏醒。

##### Q&A  介绍一下Syncronized锁，

synchronized修饰静态方法以及同步代码块的Synchronized用法锁的是类，线程想要执行对应的同步代码就需要或的类锁。

synchronized修饰成员方法，线程获取的是调用当前对象实例的对象锁。

##### Q&A  介绍一下Synchronized和lock，

synchronized是java的关键字，当用来修饰一个方法或者代码块的时候，能够保证在同一时刻最多只有一个线程执行该代码。jdk1.5后引入自旋锁，锁粗化，轻量级锁，偏向锁来优化关键字的性能。

Lock是一个接口，Synchronized发生异常时自动释放线程占有的锁，因此不会导致死锁的现象。Lock发生异常时需要通过unLock()去释放锁，则需要在使用finally块中释放锁，Lock可以让等待锁的线程响应中断，而synchronized却不行，synchronized时等待的线程会一直等待。Lock可以知道是否成功获取锁，而synchronized却无法办到。

##### Q&A  介绍一下volatile

volatile修饰的是保障有序性和可见性，比如我们写的代码不一定会按照我们书写的顺序来执行。

volatile是Java提供的轻量级的同步机制，比sync的开销要小

被volatile定义的变量,系统每次用到它时都是直接从主存中读取,而不是各个线程的工作内存

volatile可以像sync一样保持变量在多线程环境中是实时可见的

可见性：

每个线程都有自己的工作内存，每次线程执行时，会从主存获得变量的拷贝，对变量的操作是在线程的工作内存中进行，不同的线程之间不共享工作内存；对于volatile（sync，final）来说，打破了上述的规则，当线程修改了变量的值，其他线程可以立即知道该变量的改变。而对于普通变量，当一个线程修改了变量，需要将变量写回主存，其他线程从主存中读取变量后才对该线程可见

volatile具有sync的可见性，但是不具备原子性（解决java多线程的执行有序性）。volatile适用于多个变量之间或者某个变量当前值和修改之后值之间没有约束。因此，单独使用volatile还不足以实现计数器，互斥锁等。

在并发编程中谈及到的无非是可见性、有序性及原子性。而这里的Volatile只能够保证前两个性质，对于原子性还是不能保证的，只能通过锁的形式帮助他去解决原子性操作

### 网络编程：

InetAddress类没有提供构造器，而是提供了如下两个静态方法来获取InetAddress实例。

- getByName(String host)：根据主机获取对应的InetAddress对象。
- getByAddress(byte[] addr)：根据原始IP地址来获取对应的InetAddress对象。

InetAddress还提供了如下三个方法来获取InetAddress实例对应的IP地址和主机名。

- String getCanonicalHostName()：获取此IP地址的全限定域名。
- String getHostAddress()：返回该InetAddress实例对应的IP地址字符串（以字符串形式）。
- String getHostName()：获取此IP地址的主机名。

除此之外，InetAddress类还提供了一个getLocalHost()方法来获取本机IP地址对应的InetAddress实例。

InetAddress类还提供了一个isReachable()方法，用于测试是否可以到达该地址。该方法将尽最大努力试图到达主机，但防火墙和服务器配置可能阻塞请求，使得它在访问某些特定的端口时处于不可达状态。如果可以获得权限，典型的实现将使用ICMP ECHO REQUEST；否则它将试图在目标主机的端口7（Echo）上建立TCP连接。下面程序测试了InetAddress类的简单用法。

```java
public class InetAddressTest {

    public static void main(String[] args)

            throws Exception {

// 根据主机名来获取对应的InetAddress实例

        InetAddress ip = InetAddress.getByName("www.crazyit.org");

// 判断是否可达

        System.out.println("crazyit是否可达：" + ip.isReachable(2000));

// 获取该InetAddress实例的IP字符串

        System.out.println(ip.getHostAddress());

// 根据原始IP地址来获取对应的InetAddress实例

        InetAddress local = InetAddress.getByAddress(

                new byte[]{127, 0, 0, 1});

        System.out.println("本机是否可达：" + local.isReachable(5000));

// 获取该InetAddress实例对应的全限定域名

        System.out.println(local.getCanonicalHostName());

    }

}
```

### URLDecoder和URLEncoder类：

URLDecoder类包含一个decode(String s,String enc)静态方法，它可以将看上去是乱码的特殊字符串转换成普通字符串。

URLEncoder类包含一个encode(String s,String enc)静态方法，它可以将普通字符串转换成application/x-www-form-urlencoded MIME字符串。

下面程序示范了如何将图17.3所示地址栏中的“乱码”转换成普通字符串，并示范了如何将普通字符串转换成application/x-www-form-urlencoded MIME字符串。

```java
public class URLDecoderTest {

    public static void main(String[] args) throws Exception {
// 将application/x-www-form-urlenco
        疯狂java:
        public class URLDecoderTest {
            public static void main(String[] args) throws Exception {

            // 将application/x-www-form-urlencoded字符串
            // 转换成普通字符串
            // 其中的字符串直接从图17.3所示的窗口中复制过来String keyWord=URLDecoder.decode("%B7%E8%BF%F1java", "GBK");System.out.println(keyWord);
            // 将普通字符串转换成
            // application/x-www-form-urlencoded字符串String urlStr=URLEncoder.encode("疯狂Android讲义" , "GBK"); System.out.println(urlStr);

            }

        }

例子：

package com.kayleoi.springbootdatajdbc;

import java.io.InputStream;
import java.io.RandomAccessFile;
import java.net.HttpURLConnection;
import java.net.URL;

/**
 * @Author kay三石
 * @date:2020/10/3
 */
public class DownUtil {
    // 定义下载资源的路径
    private String path;
    // 指定所下载的文件的保存位置
    private String targetFile;
    // 定义需要使用多少个线程下载资源
    private int threadNum;
    // 定义下载的线程对象
    private DownThread[] threads;
    // 定义下载的文件的总大小
    private int fileSize;

    public DownUtil(String path, String targetFile, int threadNum) {
        this.path = path;
        this.threadNum = threadNum;
        // 初始化threads数组
        threads = new DownThread[threadNum];
        this.targetFile = targetFile;

    }

    public void download() throws Exception {

        URL url = new URL(path);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setConnectTimeout(5 * 1000);
        conn.setRequestMethod("GET");
        conn.setRequestProperty(
                "Accept",

                "image/gif, image/jpeg, image/pjpeg, image/pjpeg, "

                        + "application/x-shockwave-flash, application/xaml+xml, "

                        + "application/vnd.ms-xpsdocument, application/x-ms-xbap, "

                        + "application/x-ms-application, application/vnd.ms-excel, "

                        + "application/vnd.ms-powerpoint, application/msword, ＊/＊");

        conn.setRequestProperty("Accept-Language", "zh-CN");
        conn.setRequestProperty("Charset", "UTF-8");
        conn.setRequestProperty("Connection", "Keep-Alive");
        // 得到文件大小

        fileSize = conn.getContentLength();

        conn.disconnect();

        int currentPartSize = fileSize / threadNum + 1;

        RandomAccessFile file = new RandomAccessFile(targetFile, "rw");

// 设置本地文件的大小

        file.setLength(fileSize);

        file.close();

        for (int i = 0; i < threadNum; i++) {

            // 计算每个线程下载的开始位置
            int startPos = i * currentPartSize;
            // 每个线程使用一个RandomAccessFile进行下载
            RandomAccessFile currentPart = new RandomAccessFile(targetFile, "rw");
            // 定位该线程的下载位置
            currentPart.seek(startPos);
            // 创建下载线程
            threads[i] = new DownThread(startPos, currentPartSize, currentPart);
            // 启动下载线程
            threads[i].start();

        }

    }

    // 获取下载的完成百分比
    public double getCompleteRate() {
        // 统计多个线程已经下载的总大小
        int sumSize = 0;
        for (int i = 0; i < threadNum; i++) {
            sumSize += threads[i].length;
        }
        // 返回已经完成的百分比
        return sumSize * 1.0 / fileSize;

    }

    private class DownThread extends Thread {

        // 当前线程的下载位置
        public int length;

        // 定义当前线程负责下载的文件大小
        private int startPos;

        // 当前线程需要下载的文件块
        private int currentPartSize;

        // 定义该线程已下载的字节数
        private RandomAccessFile currentPart;

        public DownThread(int startPos, int currentPartSize,

                          RandomAccessFile currentPart) {

            this.startPos = startPos;

            this.currentPartSize = currentPartSize;

            this.currentPart = currentPart;

        }

        public void run() {

            try {

                URL url = new URL(path);

                HttpURLConnection conn = (HttpURLConnection) url

                        .openConnection();

                conn.setConnectTimeout(5 * 1000);

                conn.setRequestMethod("GET");

                conn.setRequestProperty(

                        "Accept",

                        "image/gif, image/jpeg, image/pjpeg, image/pjpeg, "

                                + "application/x-shockwave-flash, application/xaml+xml, "

                                + "application/vnd.ms-xpsdocument, application/x-ms-xbap, "

                                + "application/x-ms-application, application/vnd.ms-excel, "

                                + "application/vnd.ms-powerpoint, application/msword, ＊/＊");

                conn.setRequestProperty("Accept-Language", "zh-CN");

                conn.setRequestProperty("Charset", "UTF-8");

                InputStream inStream = conn.getInputStream();

                // 跳过startPos个字节，表明该线程只下载自己负责的那部分文件

                inStream.skip(this.startPos);

                byte[] buffer = new byte[1024];

                int hasRead = 0;

                // 读取网络数据，并写入本地文件

                while (length < currentPartSize && (hasRead = inStream.read(buffer)) != -1) {
                    currentPart.write(buffer, 0, hasRead);
                    // 累计该线程下载的总大小
                    length += hasRead;

                }
                currentPart.close();
                inStream.close();
            } catch (Exception e) {
                e.printStackTrace();

            }

        }

    }

}

```

上面程序中定义了DownThread线程类，该线程负责读取从start开始，到end结束的所有字节数据，并写入RandomAccessFile对象。这个DownThread线程类的run()方法就是一个简单的输入、输出实现。

程序中DownUtils类中的download()方法负责按如下步骤来实现多线程下载。

- （1）创建URL对象。
- （2）获取指定URL对象所指向资源的大小（通过getContentLength()方法获得），此处用到了URLConnection类，该类代表Java应用程序和URL之间的通信链接。后面还有关于URLConnection更详细的介绍。
- （3）在本地磁盘上创建一个与网络资源具有相同大小的空文件。
- （4）计算每个线程应该下载网络资源的哪个部分（从哪个字节开始，到哪个字节结束）。
- （5）依次创建、启动多个线程来下载网络资源的指定部分。

提示：

上面程序已经实现了多线程下载的核心代码，如果要实现断点下载，则需要额外增加一个配置文件（读者可以发现，所有的断点下载工具都会在下载开始时生成两个文件：一个是与网络资源具有相同大小的空文件，一个是配置文件），该配置文件分别记录每个线程已经下载到哪个字节，当网络断开后再次开始下载时，每个线程根据配置文件里记录的位置向后下载即可。

有了上面的DownUtil工具类之后，接下来就可以在主程序中调用该工具类的down()方法执行下载，如下程序所示。

```java

public class MultiThreadDown {

    public static void main(String[] args) throws Exception {

// 初始化DownUtil对象

        final DownUtil downUtil = new DownUtil("http://www.crazyit.org/"

                + "attachment.php?aid=MTY0NXxjNjBiYzNjN3wxMzE1NTQ2MjU5fGNhO"

                + "DlKVmpXVmhpNGlkWmVzR2JZbnluZWpqSllOd3JzckdodXJOMUpOWWt0aTJz,"

                , "oracelsql.rar", 4);

// 开始下载downUtil.download(); new Thread()

        {

            public void run ()

            {

                while (downUtil.getCompleteRate() < 1) {

// 每隔0.1秒查询一次任务的完成进度

// GUI程序中可根据该进度来绘制进度条

                    System.out.println("已完成："

                            + downUtil.getCompleteRate());

                    try {

                        Thread.sleep(1000);

                    } catch (Exception ex) {
                    }

                }

            }

        }.start();

    }

}
```

运行上面程序，即可看到程序从www.crazyit.org下载得到一份名为oracelsql.rar的压缩文件。

上面程序还用到URLConnection和HttpURLConnection对象，其中前者表示应用程序和URL之间的通信连接，后者表示与URL之间的HTTP连接。程序可以通过URLConnection实例向该URL发送请求、读取URL引用的资源。

通常创建一个和URL的连接，并发送请求、读取此URL引用的资源需要如下几个步骤。

- （1）通过调用URL对象的openConnection()方法来创建URLConnection对象。
- （2）设置URLConnection的参数和普通请求属性。
- （3）如果只是发送GET方式请求，则使用connect()方法建立和远程资源之间的实际连接即可；如果需要发送POST方式的请求，则需要获取URLConnection实例对应的输出流来发送请求参数。
- （4）远程资源变为可用，程序可以访问远程资源的头字段或通过输入流读取远程资源的数据。

### 类加载器:

类加载指的是将类的class文件读入内存，并为之创建一个java.lang.Class对象，也就是说，当程序中使用任何类时，系统都会为之建立一个java.lang.Class对象。

通过使用不同的类加载器，可以从不同来源加载类的二进制数据，通常有如下几种来源。

- 从本地文件系统加载class文件，这是前面绝大部分示例程序的类加载方式。
- 从JAR包加载class文件，这种方式也是很常见的，前面介绍JDBC编程时用到的数据库驱动类就放在JAR文件中，JVM可以从JAR文件中直接加载该class文件。
- 通过网络加载class文件。
- 把一个Java源文件动态编译，并执行加载。

类加载器通常无须等到“首次使用”该类时才加载该类，Java虚拟机规范允许系统预先加载某些类当类被加载之后，系统为之生成一个对应的Class对象，接着将会进入连接阶段，连接阶段负责把类的二进制数据合并到JRE中。类连接又可分为如下3个阶段。

- （1）验证：验证阶段用于检验被加载的类是否有正确的内部结构，并和其他类协调一致。
- 2）准备：类准备阶段则负责为类的静态Field分配内存，并设置默认初始值。
- （3）解析：将类的二进制数据中的符号引用替换成直接引用。

当使用ClassLoader类的loadClass()方法来加载某个类时，该方法只是加载该类，并不会执行该类的初始化。

使用Class的forName()静态方法才会导致强制初始化该类。例如如下代码。

```java
class Tester {

    static {

        System.out.println("Tester类的静态初始化块...");

    }

}

public class ClassLoaderTest {

    public static void main(String[] args)

            throws ClassNotFoundException {

        ClassLoader cl = ClassLoader.getSystemClassLoader();

// 下面语句仅仅是加载Tester类cl.loadClass("Tester");System.out.println("系统加载Tester类");

// 下面语句才会初始化Tester类Class.forName("Tester"); }

    }
```

![拖曳以移動](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

类加载器负责将.class文件（可能在磁盘上，也可能在网络上）加载到内存中，并为之生成对应的java.lang.Class对象

当JVM启动时，会形成由3个类加载器组成的初始类加载器层次结构。

- Bootstrap ClassLoader：根类加载器。
- Extension ClassLoader：扩展类加载器。
- System ClassLoader：系统类加载器。

Bootstrap ClassLoader被称为引导（也称为原始或根）类加载器，它负责加载Java的核心类。在Sun的JVM中，当执行java.exe命令时，使用-Xbootclasspath选项或使用-D选项指定sun.boot.class.path系统属性值可以指定加载附加的类。

根类加载器非常特殊，它并不是java.lang.ClassLoader的子类，而是由JVM自身实现的。

JVM的类加载机制主要有如下3种。

- 全盘负责。所谓全盘负责，就是当一个类加载器负责加载某个Class时，该Class所依赖的和引用的其他Class也将由该类加载器负责载入，除非显式使用另外一个类加载器来载入。
- 父类委托。所谓父类委托，则是先让parent（父）类加载器试图加载该Class，只有在父类加载器无法加载该类时才尝试从自己的类路径中加载该类。
- 缓存机制。缓存机制将会保证所有加载过的Class都会被缓存，当程序中需要使用某个Class时，类加载器先从缓存区中搜寻该Class，只有当缓存区中不存在该Class对象时，系统才会读取该类对应的二进制数据，并将其转换成Class对象，存入缓存区中。这就是为什么修改了Class后，必须重新启动JVM，程序所做的修改才会生效的原因。

注意：

类加载器之间的父子关系并不是类继承上的父子关系，这里的父子关系是类加载器实例之间的关系。

类加载器加载Class大致要经过如下8个步骤。

- 1）检测此Class是否载入过（即在缓存区中是否有此Class），如果有则直接进入第8步，否则接着执行第2步。
- （2）如果父类加载器不存在（如果没有父类加载器，则要么parent一定是根类加载器，要么本身就是根类加载器），则跳到第4步执行；如果父类加载器存在，则接着执行第3步。
- （3）请求使用父类加载器去载入目标类，如果成功载入则跳到第8步，否则接着执行第5步。
- （4）请求使用根类加载器来载入目标类，如果成功载入则跳到第8步，否则跳到第7步。
- （5）当前类加载器尝试寻找Class文件（从与此ClassLoader相关的类路径中寻找），如果找到则执行第6步，如果找不到则跳到第7步。
- （6）从文件中载入Class，成功载入后跳到第8步。
- （7）抛出ClassNotFoundException异常。
- （8）返回对应的java.lang.Class对象。

```java
public class URLClassLoaderTest {

    private static Connection conn;

// 定义一个获取数据库连接的方法

    public static Connection getConn(String url,

                                     String user, String pass) throws Exception {

        if (conn == null) {

// 创建一个URL数组

            URL[] urls = {new URL(

                    "file:mysql-connector-java-3.1.10-bin.jar")};

// 以默认的ClassLoader作为父ClassLoader，创建URLClassLoaderURLClassLoader myClassLoader=new URLClassLoader(urls);// 加载MySQL的JDBC驱动，并创建默认实例Driver driver=(Driver)myClassLoader.loadClass("com.mysql.jdbc.Driver").newInstance(); // 创建一个设置JDBC连接属性的Properties对象

            Properties props = new Properties();

// 至少需要为该对象传入user和password两个属性

            props.setProperty("user", user);

            props.setProperty("password", pass);

// 调用Driver对象的connect方法来取得数据库连接

            conn = driver.connect(url, props);

        }

        return conn;

    }

    public static void main(String[] args) throws Exception {

        System.out.println(getConn("jdbc:mysql://localhost:3306/mysql"

                , "root", "32147"));

    }

}
```

### 动态代理：

动态代理（以下称代理），利用Java的反射技术(Java Reflection)，在运行时**创建一个实现某些给定接口的新类**（也称“动态代理类”）及其实例（对象）

(Using Java Reflection to create dynamic implementations of interfaces at runtime)。

代理的是接口(Interfaces)，不是类(Class)，更不是抽象类。

解决特定问题：一个接口的实现在编译时无法知道，需要在运行时才能实现

实现某些设计模式：适配器(Adapter)或修饰器(Decorator) 

其实个人感觉在适配器方面和Aop中使用的动态代理还挺多的，个人感觉还是不太清晰，只是其中通过反射获取接口中的实现类通过Handler中持有的代理的实例target然后所有代理对象的方法都会通过invoke方法执行，

```java
interface Person {
    void walk();
    void sayHello(String name);
}

class MyInvokationHandler implements InvocationHandler {
/*
执行动态代理对象的所有方法时，都会被替换成执行如下的invoke方法
其中：
proxy：代表动态代理对象
method：代表正在执行的方法
args：代表调用目标方法时传入的实参
*/
    public Object invoke(Object proxy, Method method, Object[] args) {
        System.out.println("----正在执行的方法:" + method);
        if (args != null) {
            System.out.println("下面是执行该方法时传入的实参为：");
            for (Object val : args) {
                System.out.println(val);
            }
        } else {
            System.out.println("调用该方法没有实参！");
        }
        return null;
    }

}

public class ProxyTest {

    public static void main(String[] args)

            throws Exception {

// 创建一个InvocationHandler对象InvocationHandler handler=new MyInvokationHandler();// 使用指定的InvocationHandler来生成一个动态代理对象Person p=(Person)Proxy.newProxyInstance(Person.class.getClassLoader(), new Class[]{Person.class}, handler); // 调用动态代理对象的walk()和sayHello()方法
        p.walk();
        p.sayHello("孙悟空");
    }

}
```
本文有个人阅读疯狂java和java编程思想所生成的文档，其中一部分面试题是有面试积累的，是对javaSE的总结与回顾，后序会不定期的进行对其补充和复习。
