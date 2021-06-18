# Java深入实践

## 面向对象深入理解

根据 Java语言规范，使用instanceof运算符有一个限制：instanceof运算符前面操作数的编译时类型必须是如下3种情况。

■ 要么与后面的类相同；

■ 要么是后面类的父类；

■ 要么是后面类型的子类。

如果前面操作数的编译时类型与后面的类型没有任何关系，程序将没法通过编译。因此，当使用instanceof运算符的时候，应尽量从编译、运行两个阶段来考虑它：如果instanceof运算符使用不当，程序编译时就会抛出异常；当使用instanceof运算符通过编译后，才能考虑它的运算结果是true，还是false。

一旦instanceof运算符通过了编译，程序进入运行阶段。instanceof运算返回的结果与前一个操作数（引用变量）实际引用的对象的类型有关，如果它实际引用的对象是第二个操作数的实例，或者是第二个操作数的子类、实现类的实例，instanceof运算的结果返回true，否则返回false。

对于Java的强制转型而言，也可以分为编译、运行两个阶段来分析它。

■ 在编译阶段，强制转型要求被转型变量的编译时类型必须是如下3种情况之一：

➣ 被转型变量的编译时类型与目标类型相同；

➣ 被转型变量的编译时类型是目标类型父类；

➣ 被转型变量的编译时类型是目标类型子类，这种情况下可以自动向上转型，无须强制转换。

如果被转型变量的编译时类型与目标类型没有任何继承关系，编译器将提示编译错误。通过上面分析可以看出，强制转型的编译阶段只关心引用变量的编译时类型，至于该引用变量实际引用对象的类型，编译器并不关心，也没法关心。

■ 在运行阶段，被转型变量所引用对象的实际类型必须是目标类型的实例，或者是目标类型的子类、实现类的实例，否则在运行时将引发ClassCastException异常。

使null调用instanceof运算符时返回false是非常有用的行为，因为intanceof运算符有了一个额外的功能：它可以保证第一个操作数所引用的对象不是null。如果instanceof告知一个引用变量是某个特定类型的实例，那么就可以将其转型为该类型，并调用该类型的方法，而不用担心会抛出ClassCastException或NullPointerException异常。



### 构造器中的陷阱:

构造器是Java每个类都会提供的一个“特殊方法”。构造器负责对Java对象执行初始化操作，不管是定义实例变量时指定的初始值，还是在非静态初始化块中所做的操作，实际都会被提取到构造器中被执行。

构造器是Java面向对象特性中最基础、也是最早要接触的知识，但构造器也存在一些小陷阱，开发者需要小心地绕开它们。

当为构造器声明添加任何返回值类型声明，或者添加void声明该构造器没有返回值时，编译器并不会提示这个构造器有错误，只是系统会把这个所谓的“构造器”当成普通方法处理。构造器只是负责执行初始化，在构造器执行之前，Java对象所需要的内存空间，应该说是由new关键字申请出来的。

这个程序给出的教训是：无论如何不要导致构造器产生递归调用。也就是说，应该：

■ 尽量不要在定义实例变量时指定实例变量的值为当前类的实例；

■ 尽量不要在初始化块中创建当前类的实例；

■ 尽量不要在构造器内调用本构造器创建Java对象。



```java
/// 演示
/// 尽量不要在初始化块中创建当前类的实例
public class Test {

    private String id;

    private String name;

    private String describe;

    public Test(){
        System.out.println("构造函数中创建");
        new Test();
    }

    public static void main(String[] args) {
        Test test = new Test();
    }
}
//~out
//Exception in thread "main" java.lang.StackOverflowError
```



### 重载:

```java
public class OverrideTest3       {
    public void info(Object obj , double count)          //①    
    {
        System.out.println("obj参数为："+obj);
        System.out.println("count参数为："+count);
    }
    public void info(Object] objs , double count)       //②        
    {
        System.out.println("objs参数为："+objs);
        System.out.println("count参数为："+count);
    }
    public static void main(String] args)           {
        OverrideTest3 ot=new OverrideTest3();
        //试图调用ot的info方法              
        ot.info(null ， 5);
    }
}
```

 根据精确匹配的原则，当实际调用时传入的实参同时满足多个方法时，如果某个方法的形参要求参数范围越小，那这个方法就越精确。很明显，Object[]可看成 Object的子类， info(Object[] , int)方法匹配得更精确。执行上面程序将看到如下输出。

**Java的重载解析过程分成以下两个阶段。**

(1) 第一阶段 JVM 将会选取所有可获得并匹配调用的方法或构造器，在这个节点里，粗体字代码的调用将把①行代码处的方法、②行代码处的方法都选取出来。

(2) 第二阶段决定到底要调用哪个方法，此时 JVM 会在第一阶段所选取的方法或构造器中再次选取最精确匹配的那一个。对于上面程序来说，ot.info(“crazyit.org”,5);很明显更匹配info(String,int)方法，而不是更匹配info(String,double)方法。

总结:也就是说当多个方法重载时需要调用哪个方法时必须传入的参数给想使用的方法类型完全匹配，因为在使用时如果有多个类型时会精确匹配形参范围小的，当方法只有一个如果传入的参数与形参类型不匹配时，有可能会被向上转型，而到达匹配到这个方法

### 方法重写的陷阱:

对于使用private修饰符修饰的方法，只能在当前类中访问到该方法，子类无法访问父类中定义的private方法。既然子类无法访问父类的private方法，当然也就无法重写该方法。

如果子类中定义了一个与父类private方法具有相同方法名、相同形参列表、相同返回值类型的方法，依然不是重写，只是子类中重新定义了一个新方法。若其子类与该类处于同一个包中，子类就可以重写父类的方法；否则，子类将不能重写父类的方法。下面程序中子类Wolf与Animal不是处于同一个包里，因此无法重写父类的run()方法。

### 静态内部类:

当程序使用静态内部类时，外部类相当于静态内部类的一个包，因此使用起来比较方便；但另一方面，这也给静态内部类增加了一个限制—静态内部类不能访问。

### 字符串陷阱

#### 了解JVM对字符串的处理

了解JVM对字符串处理之前，首先来看如下一条简单的Java语句

```java
String java = new String("疯狂java");
// 上面语句创建了几个字符串对象？上面语句实际上创建了2个字符串对象，一个是“疯狂Java”这个直接量对应的字符串对象，一个是由newString()构造器返回的字符串对象
```

对于Java程序中的字符直接量，JVM会使用一个字符串池来保存它们：当第一次使用某个字符串直接量时，JVM会将它放入字符串池进行缓存。在一般情况下，字符串池中字符串对象不会被垃圾回收，当程序再次需要使用该字符串时，无须重新创建一个新的字符串，而是直接让引用变量指向字符串池中已有的字符

```java
public void test(){
	// str1的值是字符串直接常量
	// str1指向字符串缓存池的“hello” 字符串
	String str1= "hello";
	// str2 也指向字符串缓存池的“hello” 字符串
	String str2 = "hello";
    // true
	System.out.println(str1==str2);
    
    String str3 = "hello java";
    // 虽然str4的值不是直接常量，但因为str2的值可以在编译时确定，所以str2也会直接引用字符串缓存池中对应的字符串。
    String str4 = "hello"+"java";
    // true
    System.out.println(str3==str4);
}
```

注意上面程序中粗体字代码里的所有运算数，它们都是字符串直接量、整数直接量，没有变量参与，没有方法调用。因此，JVM可以在编译时就确定该字符串连接表达式的值，可以让该字符串变量指向字符串池中对应的字符串。<font color="red">但如果程序使用了变量，或者调用了方法，那就只能等到运行时才可确定该字符串连接表达式的值，也就无法在编译时确定该字符串变量的值，因此无法利用JVM的字符串池</font>。

```java
String str="hello" +"java "+"test"+"java";
//其实这条代码只创建了一个字符串对象，因为str的值可以在编译时确定下来，JVM 会在编译时就计算出str的值为“HelloJava testjava”，然后将该字符串直接量放入字符串池中，并让str指向该它
```

#### 不可变的字符串

String类是一个典型的不可变类。当一个String对象创建完成后，该String类里包含的字符序列就被固定下来了，以后永远都不能改变

```java
public class Test {
    public static void main(String[] args) {
        String str = "hello";
        //System 提供的identityHashCode()静态方法用于获取某个对象唯一的hashCode 值，这个identityHashCode()的返回值与该类是否重写了hashCode()方法无关。
        // 只有当两个对象相同时，它们的identityHashCode值才会相等
        System.out.println(System.identityHashCode(str)); // 864237698
        // 进行字符串运算
        str = str + "java";
        System.out.println(System.identityHashCode(str)); // 537548559
        // 进行字符串运算
        str = str + ", test";
        System.out.println(System.identityHashCode(str)); // 380894366
        /**
         * str变量原来指向的字符串对象并没有发生任何改变，它所包含的字符序列依然是“Hello”，
         * 只是str变量不再指向它而已。str变量指向了一个新的String对象，因此看到str变量所引用String对象的字符序列发生了改变。
         * 也就是说，发生改变的不是String对象，而是str变量本身，它改变了指向，指向了一个新的String对象
         */
    }
}
```

[![swNxQ1.png](https://s3.ax1x.com/2021/01/15/swNxQ1.png)](https://imgchr.com/i/swNxQ1)

上面程序中使用了System类的identityHashCode()静态方法来获取str的identityHashCode值，将会发生3次返回的identityHashCode值并不相同的状况，这表明3次访问str时分别指向3个不同的String对象

对于String类而言，它代表字符串序列不可改变的字符串，因此如果程序需要一个字符序列会发生改变的字符串，那应该考虑使用StringBuilder或StringBuffer。很多资料上都推荐使用StringBuffer

StringBuilder与StringBuffer唯一的区别在于，StringBuffer 是线程安全的，也就是说StringBuffer类里绝大部分方法都增加了synchronized修饰符。对方法增加 synchronized 修饰符可以保证该方法线程安全，但会降低该方法的执行效率。在没有多线程的环境下，应该优先使用StringBuilder类来表示字符串

```java
public class Test {
    public static void main(String[] args) {
        StringBuilder stringBuilder = new StringBuilder("hello");
        //System 提供的identityHashCode()静态方法用于获取某个对象唯一的hashCode 值，这个identityHashCode()的返回值与该类是否重写了hashCode()方法无关。
        // 只有当两个对象相同时，它们的identityHashCode值才会相等
        System.out.println(System.identityHashCode(stringBuilder)); // 864237698
        stringBuilder.append("java");
        System.out.println(System.identityHashCode(stringBuilder)); // 864237698
        // 进行字符串运算
        stringBuilder.append("test");
        System.out.println(System.identityHashCode(stringBuilder)); // 864237698
    }
}
```

程序3次打印StringBuilder对象将看到输出不同的字符串，但程序3次输出str的identityHashCode时将会完全相同，因为str依然引用同一个StringBuilder对象

### 多线程的陷阱

Java语言提供了非常优秀的多线程支持，使得开发者能以简单的代码来创建、启动多线程，而且Java语言内置的多线程支持极好地简化了多线程编程。虽然如此，Java多线程编程中依然存在一些容易混淆的陷阱

#### 不要调用run方法应该调用start方法启动线程

从JDK 1.5开始，Java提供了3种方式来创建、启动多线程：

■ 继承Thread类来创建线程类，重写run()方法作为线程执行体；

■实现Runnable接口来创建线程类，重写run()方法作为线程执行体；

■实现Callable接口来创建线程类，重写call()方法作为线程执行体。

**其中，第1种方式的效果最差，它有2点坏处：**

■ 线程类继承了Thread类，无法再继承其他父类；

■ 因为每条线程都是一个Thread子类的实例，因此多个线程之间共享数据比较麻烦。

**对于第2种和第3种方式，它们的本质是一样的，只是Callable接口里包含的call()方法既可以声明抛出异常，也可以拥有返回值。除此之外，如果采用继承Thread类的方式来创建多线程，程序还有一个潜在的危险。示例如下**

```java
public class Test {
    public static void main(String[] args) {
        for(int i=0;i<100;i++){
            // 获取当前线程的名称
            System.out.println(Thread.currentThread().getName()+ " " + i);
            if(i==20){
                // 直接调用run方法：
                new InvokeRun().run();
                new InvokeRun().run();

            }
        }
    }
}
class InvokeRun extends Thread{
    private int i;

    @Override
    public void run() {
        for(;i<100;i++){
            // 获取当前线程的名称
            System.out.println(Thread.currentThread().getName()+ " " + i);
        }
    }
}
// 输出
/**
         * (1) 输出main 20之后，又重新开始输出main 0。
         * (2) 从main 0一直输出到main 99，再次从main 0开始输出。
         * (3) 再次从main 0一直输出到main 99，再次从main 22开始输出，直到main 99结束。上面程序始终只有一条线程，并没有启动任何新线程，关键是因为粗体字代码那里调用了线程对象的run()方法，
         * 而不是start()方法—启动线程应该使用start()方法，而不是run()方法
         */
// 而应该使用这个：
for(int i=0;i<100;i++){
     // 获取当前线程的名称
     System.out.println(Thread.currentThread().getName()+ " " + i);
     if(i==20){
        new InvokeRun().start();
     }
   }
```

#### 静态同步方法

Java提供了synchronized关键字用于修饰方法，使用synchronized修饰的方法被称为同步方法。当然，synchronized关键字除了修饰方法之外，还可以修饰普通代码块，使用synchronized修饰的代码块被称为同步代码块。

Java语法规定，任何线程进入同步方法、同步代码块之前，必须先获取同步方法、同步代码块对应的同步监视器。

对于同步代码块而言，程序必须显式为它指定同步监视器；对于同步非静态方法而言，该方法的同步监视器是this—即调用该方法的Java对象；对于静态的同步方法而言，该方法的同步监视器不是this，而是该类本身。下面程序提供了一个静态的同步方法及一个同步代码块。同步代码块使用this 作为同步监视器，即这两个同步程序单元并没有使用相同的同步监视器，因此它们可以同时并发执行，相互之间不会有任何影响

```java
public class Test {
    public static void main(String[] args) throws InterruptedException {
        SynchronizedStatic synchronizedStatic = new SynchronizedStatic();
        new Thread(synchronizedStatic).start();
        // 保证第一条线程开始运行
        Thread.sleep(1);
        new Thread(synchronizedStatic).start();
    }
}
class SynchronizedStatic implements Runnable{
    static boolean staticFlag = true;
    public static synchronized void test(){
        for(int i=0;i<100;i++){
            System.out.println("test:" + Thread.currentThread().getName()+" "+i);
        }
    }
    public  void test2(){
        synchronized (this){
            for(int i=0;i<100;i++){
                System.out.println("test2:" + Thread.currentThread().getName()+" "+i);
            }
        }

    }
    @Override
    public void run() {
        if(staticFlag){
            staticFlag = false;
            test();
        }else{
            staticFlag = true;
            test2();
        }
    }
}
/**output
     * test:Thread-0 23
     * test:Thread-0 24
     * test:Thread-0 25
     * 调用test2
     * test:Thread-0 26
     * test2:Thread-1 0
     * test:Thread-0 27
     * test2:Thread-1 1
     * test2:Thread-1 2
     */
```

面程序中定义了一个SynchronizedStatic类，该类实现了Runnable接口，因此可作为线程的target来运行。SynchronizedStatic类通过一个staticFlag旗标控制线程使用哪个方法作为线程执行体：

■ 当staticFlag为真时，程序使用test()方法作为线程执行体；

■ 当staticFlag为假时，程序使用test1()方法作为线程执行体。

程序第一次执行SynchronizedStatic对象作为target的线程时，staticFlag初始值为true，因此程序将以 test0()方法作为线程执行体，而且程序将会把 staticFlag修改为false；这使得第二次执行SynchronizedStatic对象作为target的线程时，程序将以test1()方法作为线程执行体

```java
 public  void test2(){
        synchronized (this){
            for(int i=0;i<100;i++){
                System.out.println("test2:" + Thread.currentThread().getName()+" "+i);
            }
        }

    }
```

静态同步方法可以和以this为同步监视器的同步代码块同时执行，当第一条线程（以 test0()方法作为线程执行体的线程）进入同步代码块执行以后，该线程获得了对同步监视器（SynchronizedStatic类）的锁定；第二条线程（以 test2()方法作为线程执行体的线程）尝试进入同步代码块执行，进入同步代码块之前，该线程必须获得对this引用（也就是ss变量所引用的对象）的锁定。因为第一条线程锁定的是SynchronizedStatic类，而不是ss变量所引用的对象，所以第二条线程完全可以获得对ss变量所引用的对象的锁定，因此系统可以切换到执行第二条线程

将test2()方法改为上面的形式之后，该同步代码块的同步监视器也是SynchronizedStatic类，也就是与同步静态方法test()具有相同的同步监视器。再次运行该程序

```java
public  void test2(){
        synchronized (SynchronizedStatic.class){
            for(int i=0;i<100;i++){
                System.out.println("test2:" + Thread.currentThread().getName()+" "+i);
            }
        }

    }
 /**output
         * test:Thread-0 96
         * test:Thread-0 97
         * test:Thread-0 98
         * test:Thread-0 99
         * 调用test2
         * test2:Thread-1 0
         * test2:Thread-1 1
         * test2:Thread-1 2
         * test2:Thread-1 3
         * test2:Thread-1 4
         */
```

静态同步方法和以当前类为同步监视器的同步代码块不能同时执行，当第一条线程（以 test0()方法作为线程执行体的线程）进入同步代码块执行以后，该线程获得了对同步监视器（SynchronizedStatic类）的锁定；第二条线程（以 test1()方法作为线程执行体的线程）尝试进入同步代码块执行，进入同步代码块之前，该线程必须获得对SynchronizedStatic类的锁定。因为第一条线程已经锁定了SynchronizedStatic类，在第一条线程执行结束之前，它不会释放对SynchronizedStatic类的锁定，所以第二条线程无法获得对SynchronizedStatic类的锁定，因此只有等到第一条线程执行结束时才可以切换到执行第二条线程.

#### 实例

```java
public class Test{

    public static void main(String[] args){
        AccountSync accountSync = new AccountSync("123456",1000);
        new DrawThread("甲",accountSync,800).start();
        new DrawThread("乙",accountSync,800).start();
    }
}

/**
 * 银行账户取钱
 */
class AccountSync{
    private String accountNo;
    private double balance;
    public AccountSync(String accountNo,double balance){
        this.accountNo=accountNo;
        this.balance =balance;
    }
    //访问 该账户的余额
    public synchronized double getBalance(){
        return this.balance;
    }
    // 使用synchronized修饰符将其变为同步的方法
    public synchronized void draw(double drawAccount){
        if(balance>=drawAccount){
            // 暂停当前线程，切换为其他线程
            try {
                Thread.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName()+"取钱成功！吐出钞票" + drawAccount);
            // 修改余额
            balance-= drawAccount;
            System.out.println("\t余额为："+balance);
        }else{
            System.out.println(Thread.currentThread().getName()+"取钱失败！余额不足");
        }

    }
    //重写hashcode()方法
    @Override
    public int hashCode(){
        return accountNo.hashCode();
    }
    public boolean equals(Object obj){
        if(obj == this) {
            return true;
        }
        if(obj !=null && obj.getClass() == AccountSync.class){
            AccountSync target=(AccountSync) obj;
            return target.accountNo.equals(accountNo);
        }
        return false;
    }
}
class DrawThread extends Thread{
    // 模拟用户账户
    private AccountSync account;
    // 当前取钱数
    private double drawAmount;
    public DrawThread(String name,AccountSync account,double drawAmount){
        super(name);
        this.account=account;
        this.drawAmount = drawAmount;
    }
    //当多条线程修改同一个共享数据时，将涉及数据安全问题
    public void run(){
        account.draw(drawAmount);
    }
}
/**
 * 甲取钱成功！吐出钞票800.0
 * 余额为：200.0
 * 乙取钱失败！余额不足
 */
```



## 集合深入实践

### ArrayList和Vector类:

[![DNo4fK.png](https://s3.ax1x.com/2020/11/24/DNo4fK.png)](https://imgchr.com/i/DNo4fK)

**请看写ArrayList和Vector的初始化：**

```java
	/**
     * 默认初始容量.
     */
    private static final int DEFAULT_CAPACITY = 10;
    /**
     * 存储ArrayList的元素的数组缓冲区。 ArrayList的容量是此数组缓冲区的长度。 添加第一个元素时，任何	 * 具有elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA的空ArrayList都将扩展为DEFAULT_CAPACITY
     */
    transient Object[] elementData; // non-private to simplify nested class access
    /**
     * 用于空实例的共享空数组实例.
     */
    private static final Object[] EMPTY_ELEMENTDATA = {};

    /**
     * 共享的空数组实例，用于默认大小的空实例。 我们将此与EMPTY_ELEMENTDATA区别开来，以了解添加第一个元素时要增加多少.
     */
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

	/**
     * @serial
     */
    private int size;

    /**
     * 构造一个具有指定初始容量的空列表
     */
    public ArrayList(int initialCapacity) {
        // 先进行判断是
        if (initialCapacity > 0) {
            // 然后初始化数组
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            // 当inittialCapacity为0是初始为空
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }

    /**
     * 构造一个初始容量为10的空列表.
     * 这里构造的时候会把上面的变量进行初始化了
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    /**
     * 
     */
    public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // c.toArray might (incorrectly) not return Object[] (see 6260652)
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // replace with empty array.
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }

```

ArrayList和Vector类都是基于数组实现的List类，封装了一个动态的、允许再分配的Object[]数组。ArrayList或Vector对象使用initialCapacity参数来设置该数组的长度，当向ArrayList或Vector中添加元素超出了该数组的长度时，它们的initialCapacity会自动增加。

对于通常的编程场景，我们程序员无须关心ArrayList或Vector的initialCapacity。但如果向ArrayList或Vector集合中添加大量元素时，可使用ensureCapacity(int minCapacity)方法一次性地增加initialCapacity。这可以减少重分配的次数，从而提高性能。

<font color="red">如果开始就知道ArrayList或Vector集合需要保存多少个元素，则可以在创建它们时就指定initialCapacity大小。如果创建空的ArrayList或Vector集合时不指定initialCapacity参数，则Object[]数组的长度默认为10。</font>



除此之外，ArrayList和Vector还提供了如下两个方法来重新分配Object[]数组。

- void ensureCapacity(int minCapacity)：将ArrayList或Vector集合的Object[]数组长度增加minCapacity。
- void trimToSize()：调整ArrayList或Vector集合的Object[]数组长度为当前元素的个数。程序可调用该方法来减少ArrayList或Vector集合对象占用的存储空间。

**对于ensureCapacity(int min Capacity) 方法我们从add()方法进入：**

```java
 public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        // 数组中加入e.
        elementData[size++] = e;
        return true;
   }
```

看下ensureCapacityInternal() 是如何将Object[]数组增加minCapacity的。

```java
    private void ensureCapacityInternal(int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            // 因为如果是空的话，minCapacity=size+1；其实就是等于1，空的数组没有长度就存放不了，所以就将minCapacity变成10，也就是默认大小，但是带这里，还没有真正的初始化这个elementData的大小
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }
		// //确认实际的容量，上面只是将minCapacity=10，这个方法就是真正的判断elementData是否够用
        ensureExplicitCapacity(minCapacity);
    }

    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;
		//minCapacity如果大于了实际elementData的长度，那么就说明elementData数组的长度不够用，不够用那么就要增加elementData的length。这里有的同学就会模糊minCapacity到底是什么呢，这里给你们分析一下

/*第一种情况：由于elementData初始化时是空的数组，那么第一次add的时候，minCapacity=size+1；也就minCapacity=1，在上一个方法(确定内部容量ensureCapacityInternal)就会判断出是空的数组，就会给
　　将minCapacity=10，到这一步为止，还没有改变elementData的大小。
　第二种情况：elementData不是空的数组了，那么在add的时候，minCapacity=size+1；也就是minCapacity代表着elementData中增加之后的实际数据个数，拿着它判断elementData的length是否够用，如果length
不够用，那么肯定要扩大容量，不然增加的这个元素就会溢出。
*/
        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
	private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8
    /**
     * 增加容量以确保其至少可以容纳最小容量参数指定的元素数
     */
    private void grow(int minCapacity) {
        // overflow-conscious code
        // 将扩充前的elementData大小给oldCapacity
        int oldCapacity = elementData.length;
        // newCapacity就是1.5倍的oldCapacity
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        //elementData就空数组的时候，length=0，那么oldCapacity=0，newCapacity=0，所以这个判断成立，在这里就是真正的初始化elementData的大小了，就是为10.前面的工作都是准备工作
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        //如果newCapacity超过了最大的容量限制，就调用hugeCapacity，也就是将能给的最大值给newCapacity
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            //如果newCapacity超过了最大的容量限制，就调用hugeCapacity，也就是将能给的最大值给newCapacity
            newCapacity = hugeCapacity(minCapacity);
        ////新的容量大小已经确定好了开始copy数组，改变容量
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

**对于trimToSize()方法我们去看下源码结构：**

```java
public void trimToSize() {
        modCount++;
        if (size < elementData.length) {
            elementData = (size == 0)
              ? EMPTY_ELEMENTDATA
              : Arrays.copyOf(elementData, size);
        }
    }
```

比如我们初始化一个容量为10的list这时ArrayList的扩容机制会令elementData的容量为15，因为 10+ 10右移一位所以为15.然而我们只用到了11个位置所以会有四个位置为null.调用`al.trimToSize()`方法会将ArrayList的为空的给去除，源码中，判断size是否小于elementData。这时是成立的所以会将数组进行从新copy.

```
ArrayList al = new ArrayList(10);
        for(int i=0;i<10;i++){
            al.add(i);
        }
        al.add(1);
        al.trimToSize();
```

ArrayList和Vector在用法上几乎完全相同，但由于Vector是一个古老的集合（从JDK 1.0就有了），那时候Java还没有提供系统的集合框架，所以Vector里提供了一些方法名很长的方法，例如addElement(Object obj)，实际上这个方法与add (Object obj)没有任何区别。从JDK 1.2以后，Java提供了系统的集合框架，就将Vector改为实现List接口，作为List的实现之一，从而导致Vector里有一些功能重复的方法。

**提示：**

Vector里有一些功能重复的方法，这些方法中方法名更短的方法属于后来新增的方法，方法名更长的方法则是Vector原有的方法。Java改写了Vector原有的方法，将其方法名缩短是为了简化编程。而ArrayList开始就作为List的主要实现类，因此没有那些方法名很长的方法。实际上，Vector具有很多缺点，通常尽量少用Vector实现类。

**除此之外，ArrayList和Vector的显著区别是：ArrayList是线程不安全的，当多个线程访问同一个ArrayList集合时，如果有超过一个线程修改了ArrayList集合，则程序必须手动保证该集合的同步性；但Vector集合则是线程安全的，无须程序保证该集合的同步性。因为Vector是线程安全的，所以Vector的性能比ArrayList的性能要低。实际上，即使需要保证List集合线程安全，也同样不推荐使用Vector实现类。可以使用Collections工具类，它可以将一个ArrayList变成线程安全的。**

`List<String> k=Collections.synchronizedList(new ArrayList<>());`

下面看下synchronizedList的源码：

```java
 public static <T> List<T> synchronizedList(List<T> list) {
        return (list instanceof RandomAccess ?
                new SynchronizedRandomAccessList<>(list) :
                new SynchronizedList<>(list));
    }
// 在new SychronizedList中我们看到：他所有的类返回值都加了
synchronized (mutex) 这段代码所以这个构造出来是线程安全的。
```

<font color="red">这里有一个Arrays.asList()；可以转为ArrayList但是Arrays.ArrayList是一个固定长度的List集合，程序只能遍历访问该集合里的元素，不可增加、删除该集合里的元素</font>

**看下Vector的类的结构**

[![DNofFx.png](https://s3.ax1x.com/2020/11/24/DNofFx.png)](https://imgchr.com/i/DNofFx)

Vector也是实现了List接口。下面看下Vector的构造方法和insert方法

```java
 public Vector(int initialCapacity, int capacityIncrement) {
        super();
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        this.elementData = new Object[initialCapacity];
        this.capacityIncrement = capacityIncrement;
    }

public synchronized void insertElementAt(E obj, int index) {
        modCount++;
        if (index > elementCount) {
            throw new ArrayIndexOutOfBoundsException(index
                                                     + " > " + elementCount);
        }
    	// 确认扩容
        ensureCapacityHelper(elementCount + 1);
        System.arraycopy(elementData, index, elementData, index + 1, elementCount - index);
        elementData[index] = obj;
        elementCount++;
    }
```

**Vector还有一个Stack子类.**

Vector还提供了一个Stack子类，它用于模拟“栈”这种数据结构，“栈”通常是指“后进先出”（LIFO）的容器。最

后“push”进栈的元素，将最先被“pop”出栈。与Java中的其他集合一样，进栈出栈的都是Object，因此从栈中取出元素后必须进行类型转换，除非你只是使用Object具有的操作。所以Stack类里提供了如下几个方法。

- Object peek()：返回“栈”的第一个元素，但并不将该元素“pop”出栈。

- Object pop()：返回“栈”的第一个元素，并将该元素“pop”出

  

这个Stack子类仅在Vectot父类的基础上增加了5个方法，这5个方法就将一个Vector扩展成了Stack。本质上，Stack依然是一个Vector，它只是比Vector多了5个方法

```java
public class Stack<E> extends Vector<E> {
    //无参数的构造器
    public Stack() {
    }

    //实现向栈顶添加元素的方法
    public E push(E item) {
        //调用父类的方法来添加元素
        addElement(item);
        return item;
    }

    //实现出栈的方法（位于栈顶的元素将被弹出栈FILO）
    public synchronized E pop() {
        E obj;
        int len = size();
        //取出最后一个元素
        obj = peek();
        //删除最后一个元素
        removeElementAt(len - 1);
        return obj;
    }

    //取出最后一个元素，但并不弹出栈
    public synchronized E peek() {
        int len = size();
        //如果不包含任何元素，直接抛出异常
        if (len == 0)
            throw new EmptyStackException();
        return elementAt(len - 1);
    }

    public boolean empty() {
        //集合不包含任何元素就是空栈
        return size() == 0;
    }

    public synchronized int search(Object o) {
        //获取o在集合中的位置
        int i = lastIndexOf(o);
        if (i >= 0) {
            //用集合长度减去o在集合中的位置，就得到该元素到栈顶的距离
            return size() - i;
        }
        return -1;
    }
}
```



Stack是一个线程安全的类，这也是为了让Stack和Vector保持一致—Vector也是一个线程安全的类。

> 实际上即使当程序中需要栈这种数据结构时，Java 也不再推荐使用Stack类，而是推荐使用Deque实现类。从JDK 1.6 开始，Java 提供了一个Deque 接口，并为该接口提供一个ArrayDeque实现类。在无需保证线程安全的情况下，程序完全可以使用ArrayDueue 来代替Stack类。
>
> Deque 接口代表双端队列这种数据结构。双端队列已经不再是简单的队列了，它既具有队列的性质先进先出（FIFO），也具有栈的性质（FILO），也就是说双端队列既是队列，也是栈。Java为Deque提供了一个常用的实现类ArrayDeque。

Vector和ArrayList这两个集合类的本质并没有太大的不同，它们都实现了List接口，而且底层都是基于Java数组来存储集合元素。

**在ArrayList集合类的源代码中可以看到如下一行。**

```java
//采用elementData数组来保存集合元素

private transient Object[] elementData;

```

**在Vector集合类的源代码中也可看到类似的一行。**

```java
//采用elementData数组来保存集合元素

protected Object[] elementData;

```

**从上面代码可以看出，ArrayList使用transient修饰了elementData数组。这保证系统序列化ArrayList对象时不会直接序列化elementData数组，而是通过ArrayList提供的writeObject、readObject方法来实现定制序列化；但对于Vector而言，它没有使用transient修饰elementData数组，而且Vector只提供了一个writeObject方法，并未完全实现定制序列化。**

从序列化机制的角度来看，ArrayList的实现比Vector的实现更安全。

除此之外，Vector其实就是ArrayList的线程安全版本，ArrayList和Vector绝大部分方法的实现都是相同的，只是Vector的方法增加了synchronized修饰。

前面已经看到：ArrayList 底层采用一个elementData数组来保存所有的集合元素，因此ArrayList在插入元素时需要完成下面两件事情。

■ 保证ArrayList底层封装的数组长度大于集合元素的个数；

■ 将插入位置之后的所有数组元素“整体搬家”，向后移动一“格”。

反过来，当删除ArrayList集合中指定位置的元素时，程序也要进行“整体搬家”，而且还需将被删索引处的数组元素赋为null。下面是ArrayList集合的remove(int index)方法的源代码。



```java
public E remove(int index) {
        //如果index是大于或等于size，抛出异常
        RangeCheck(index);                        //①
        modCount++;
        //保证index索引处的元素
        E oldValue = (E) elementData[index];
        //计算需要“整体搬家”的元素个数
        int numMoved = size - index - 1;
        //当numMoved大于0时，开始搬家
        if (numMoved > 0)
            System.arraycopy(elementData， index + 1，
                    elementData， index， numMoved);
        //释放被删除的元素，以便GC回收该元素
        elementData[--size] = null;
        return oldValue;
    }

```

**ArrayList底层采用数组来保存每个集合元素，LinkedList则是一种链式存储的线性表。其本质上就是一个双向链表，但它不仅实现了List接口，还实现了Deque接口。**

**也就是说LinkedList既可以当成双向链表使用，也可以当成队列使用，还可以当成栈来使用（Deque代表双端队列，既具有队列的特征，也具有栈的特征）**

**对于ArrayList集合而言，当程序向ArrayList中添加、删除集合元素时，ArrayList底层都需要对数组进行“整体搬家”，因此性能非常差**。

### LinkedList

[![DNohY6.png](https://s3.ax1x.com/2020/11/24/DNohY6.png)](https://imgchr.com/i/DNohY6)

> 
>
> **下面来看LinkedList**
>
> 

**LinkedList本质上就是一个双向链表，因此它使用如下内部类来保存每个集合元素**。

```java
private static class Entry<E> {
    //集合元素
    E element;
    //保存指向下一个链表节点的引用
    Entry<E> next;
    //保存指向上一个链表节点的引用
    Entry<E> previous;

    //普通构造器
    Entry(E element, Entry<E> next, Entry<E> previous) {
        this.element = element;
        this.next = next;
        this.previous = previous;
    }
}    

```

从上面程序中的粗体字代码可以看出，一个Entry对象代表双向链表的一个节点，该对象中next变量指向下一个节点，previous则指向上一个节点

在指定位置插入新节点

```java
public void add(int index, E element) {
        //如果index==size，直接在header之前插入新节点
        //否则，在index索引处的节点之前插入新节点
        addBefore(element， (index == size ? header ：entry(index)));
}
// 在指定节点前添加一个新的节点
pubic Entry<E> addBefore(int index){
    //创建节点，新节点的下一个节点指向entry上一个节点指向entry的上一个节点
    Entry<E> newEntry = new Entry<E>(e,entry,entry.previous);
    // 让entry的上一个节点向后指向新的节点
    newEntry.previous.next = newEntry;
    // 让entry指向新节点
    newEntry.next.previous=newEntry;
    size++;
    modCount++;
    return newEntry;
}

```

从上面代码看出，由于LinkiedList本质上就是一个双向链表，因此它可以非常方便地在指定节点之前插入新节点，LinkedList在指定位置添加新节点也是通过这种方式来实现的。

上面add(int index, E element)方法实现中用到了以下两个方法。

■ entry(int index)：搜索指定索引处的元素。

■ addBefore(E element , Entry ref)：在ref节点之前插入element新节点。

**entry(int index)实际上就是get(int index)方法的底层实现。对于ArrayList而言，由于它底层采用数组来保存集合元素，因此可以直接根据数组索引取出 index 位置的元素；但对于LinkedList就比较麻烦了，LinkedList必须一个元素一个元素地搜索，直到找到第index个元素为止。**



下面是entry(int index)方法的源代码。

```java
//获取指定索引处的节点
private Entry<E> entry(int index) {
    //如果index越界，抛出异常
    if (index < 0 || index >= size)
        throw new IndexOutOfBoundsException("Index： " + index + ", Size： " + size);
    //从链表的头开始搜索
    Entry<E> e = header;
    //如果index小于size/2
    if (index < (size >> 1)) {
        //从链表的头端开始搜索
        for (int i = 0; i <= index; i++)
            e = e.next;
    }
    //如果index大于size/2
    else {
        //从链表的尾端开始搜索
        for (int i = size; i > index; i--)
            e = e.previous;
    }
    return e;
}

```

上面entry(int index)方法就是一个元素一个元素地找到 index 索引处的元素，只是由于LinkedList 是一个双向链表，因此程序先根据 index的值判断它到底离链表头端近（当index<size/2时），还是离链表尾端近。如果离头端近则从头端开始搜索，如果离尾端近则从尾端搜索

LinkedList的get(int index)方法只是对上面entry(int index)方法的简单包装。get(int index)方法的源代码如下

```java
public E get(int index){
	return entry(index).element;
}

```

如果只是单纯地添加某个节点，LinkedList的性能会非常好，可惜如果需要向指定索引处添加节点，LinkedList必须先找到指定索引处的节点—这个搜索过程的系统开销并不小，因此LinkedList的add(int index, E element)方法的性能并不是特别好.

当单纯地把LinkedList当成双向链表来使用，通过addFirst(E e)、addList(E e)、offerFirst(E e)、offerLast(E e)、pollFirst()、pollLast()等方法来操作LinkedList 集合元素时，LinkedList的性能非常好—因为此时可以避免搜索过程。

**ArrayList与LinkedList性能分析**

- 当程序需要以get(int index)方法获取List集合指定索引处的元素时，ArrayList性能大大地优于LinkedList。因为ArrayList底层以数组来保存集合元素，所示调用get(int index)方法获取指定索引处的元素时，底层实际上调用elementData[index]来返回该元素，因此性能非常好。而LinkedList则必须一个一个地搜索过去。
- 当程序调用add(int index, Object obj)向List集合中添加元素时，ArrayList必须对底层数组元素进行“整体搬家”。如果添加元素导致集合长度将要超过底层数组长度，ArrayList必须创建一个长度为原来长度 1.5 倍的数组，再由垃圾回收机制回收原有数组，因此系统开销比较大。对于LinkedList而言，它的主要开销集中在entry(int index)方法上，该方法必须一个一个地搜索过去，直到找到index处的元素，然后再在该元素之前插入新元素。即使如此，执行该方法的时候LinkedList方法的性能依然高于ArrayList。
- 当程序调用remove(int index)方法删除index索引处的元素时，ArrayList同样也需要对底层数组元素进行“整体搬家”。但调用remove(int index)方法删除集合元素时，ArrayList无需考虑创建新数组，因此执行ArrayList的remove(int index)方法比执行add(int index ,Object obj)方法略快一点。当LinkedList调用remove(int index)方法删除集合元素时，与调用add(int index , Object obj)方法添加元素的系统开销几乎完全相同。
- 当程序调用add(Object obj)方法向List集合尾端添加一个元素时，大部分时候ArrayList无需对底层数组元素进行“整体搬家”，因此也可以获得很好的性能（甚至比LinkedList的add(Object obj)方法的性能更好）；但如果添加这个元素导致集合长度将要超过底层数组长度，那么 ArrayList必须创建一个长度为原来长度 1.5 倍的数组，再由垃圾回收机制回收原有数组—这样系统开销就比较大了。但LinkedList调用add(Object obj)方法添加元素时总可以获得较好的性能。
- 当程序把LinkedList当成双端队列、栈使用，调用addFirst(E e)、addLast(E e)、getFirst(E e)、getLast(E e)、offer(E e)、offerFirst()、offerLast()等方法来操作集合元素时，LinkedList可以快速定位需要操作的元素，因此LinkedList总是具有较好的性能表现。

**上面分析了ArrayList、LinkedList各自的适用场景。大部分情况下，ArrayList的性能总是优于LinkedList，因此绝大部分都应该考虑使用ArrayList集合。但如果程序经常需要添加、删除元素，尤其是经常需要调用add(E e)方法向集合中添加元素时，则应该考虑使用LinkedList集合。**

### HashMap和HashSet

参考：https://blog.csdn.net/qq_37256896/article/details/103325204

### TreeMap和TreeSet

 TreeSet 底层则采用一个NavigableMap 来保存 TreeSet 集合的元素。但实际上，由于NavigableMap只是一个接口，因此底层依然是使用TreeMap来包含Set集合中的所有元素

```java
public class TreeSet<E> extends AbstractSet<E>
    implements NavigableSet<E>, Cloneable, java.io.Serializable
{	// navigableMap继承于sortMap--》继承于Map
    // 使用NavigableMap的key来保存Set集合的元素
    private transient NavigableMap<E,Object> m;
	// 使用一个PRESENT作为Map集合的所有的Value
    private static final Object PRESENT = new Object();
	// 指定NavigableMap对象创建Set集合
    TreeSet(NavigableMap<E,Object> m) {
        this.m = m;
    }

    // 以自然排序方式创建一个新的TreeMap，使用TreeMap的key保存Set集合的元素
    public TreeSet() {
        this(new TreeMap<E,Object>());
    }

    // 以定制排序方式创建一个新的TreeMap，根据TreeMap创建一个TreeSet
    public TreeSet(Comparator<? super E> comparator) {
        this(new TreeMap<>(comparator));
    }

    // 像TreeSet中添加Collection集合c里的所有元素
    public TreeSet(Collection<? extends E> c) {
        // 调用的是 Tree(){}
        this();
        addAll(c);
    }

    // 向TreeSet中添加SortSet集合里s的所有元素
    public TreeSet(SortedSet<E> s) {
        // 调用的是 TreeSet(Comparator<? super E> comparator)
        this(s.comparator());
        addAll(s);
    }

    
    public Iterator<E> iterator() {
        return m.navigableKeySet().iterator();
    }

    
    public Iterator<E> descendingIterator() {
        return m.descendingKeySet().iterator();
    }

    
    public NavigableSet<E> descendingSet() {
        return new TreeSet<>(m.descendingMap());
    }

    
    public int size() {
        return m.size();
    }

   
    public boolean isEmpty() {
        return m.isEmpty();
    }

    
    public boolean contains(Object o) {
        return m.containsKey(o);
    }

    
    public boolean remove(Object o) {
        return m.remove(o)==PRESENT;
    }

  
    public void clear() {
        m.clear();
    }
	// TreeSet的其他方法都是掉用TreeMap的方法提供的实现
    public  boolean addAll(Collection<? extends E> c) {
        // Use linear-time version if applicable
        if (m.size()==0 && c.size() > 0 &&
            c instanceof SortedSet &&
            m instanceof TreeMap) {
            // 把c集合强制转换为SortSet集合
            SortedSet<? extends E> set = (SortedSet<? extends E>) c;
            // 把m集合强制转换为TreeMap集合
            TreeMap<E,Object> map = (TreeMap<E, Object>) m;
            Comparator<?> cc = set.comparator();
            Comparator<? super E> mc = map.comparator();
            if (cc==mc || (cc != null && cc.equals(mc))) {
                // 把Collection中的所有元素添加成TreeMap集合中的key
                map.addAllForTreeSet(set, PRESENT);
                return true;
            }
        }
        //调用父类的addAll()方法来实现
        return super.addAll(c);
    }
```

**对于TreeMap 而言，它采用一种被称为“红黑树”的排序二叉树来保存 Map 中每个Entry—每个Entry都被当成“红黑树”的一个节点对待**

```java
    public static void main(String[] args){
        TreeMap<String,Double> treeMap = new TreeMap<>();
        treeMap.put("ccc",99.0);
        treeMap.put("bbb",90.0);
        treeMap.put("aaa",90.0);
        treeMap.put("ddd",99.0);
        System.out.println(treeMap);
    }
//output~ {aaa=90.0, bbb=90.0, ccc=99.0, ddd=99.0}
```

以后每向TreeMap中放入一个key-value对，系统都需要将该Entry当成一个新节点，添加到已有红黑树中，通过这种方式就可保证TreeMap中所有key总是由小到大地排列.

对于TreeMap 而言，由于它底层采用一棵“红黑树”来保存集合中的Entry，这意味着TreeMap添加元素、取出元素的性能都比HashMap低。当TreeMap添加元素时，需要通过循环找到新增Entry的插入位置，因此比较耗性能；当从TreeMap中取出元素时，需要通过循环才能找到合适的Entry，也比较耗性能。但TreeMap、TreeSet相比HashMap、HashSet的优势在于：TreeMap中的所有Entry总是按key根据指定排序规则保持有序状态，TreeSet中的所有元素总是根据指定排序规则保持有序状态

**对于TreeMap集合而言，其关键就是put(K key, V value)，该方法实现了将Entry放入TreeMap的Entry链，并保证该Entry链总是处于有序状态。下面是该方法的源代码**

```java
public class TreeMap<K,V>
    extends AbstractMap<K,V>
    implements NavigableMap<K,V>, Cloneable, java.io.Serializable
{
    
    private final Comparator<? super K> comparator;

    private transient Entry<K,V> root;

    
    private transient int size = 0;

    
    private transient int modCount = 0;

    
    public TreeMap() {
        comparator = null;
    }

   
    public TreeMap(Comparator<? super K> comparator) {
        this.comparator = comparator;
    }

    
    public TreeMap(Map<? extends K, ? extends V> m) {
        comparator = null;
        putAll(m);
    }

   
    public TreeMap(SortedMap<K, ? extends V> m) {
        comparator = m.comparator();
        try {
            buildFromSorted(m.size(), m.entrySet().iterator(), null, null);
        } catch (java.io.IOException cannotHappen) {
        } catch (ClassNotFoundException cannotHappen) {
        }
    }
    // put方法
    public V put(K key, V value) {
        // 先以t保存链表的root节点
        Entry<K,V> t = root;
        // 如果t==null是一个空链表，TreeMap中无任何的Entry
        if (t == null) {
            // 比较key是否相同
            compare(key, key); // type (and possibly null) check
			// 将一个新的key-value创建一个Entry并将Entry作为root
            root = new Entry<>(key, value, null);
            // 将map集合的size为1，代表包含一个Entry
            size = 1;
            // 记录修改次数为1
            modCount++;
            return null;
        }
        int cmp;
        Entry<K,V> parent;
        // split comparator and comparable paths
        Comparator<? super K> cpr = comparator;
        // 如果比较器不为null,即采用定制排序
        if (cpr != null) {
            do {
                // 使用parent上次循环后的t所引用的Entry
                parent = t;
                // 拿新插入的key和t的key比较
                cmp = cpr.compare(key, t.key);
                // 如果新插入的key小于t的key t等于t的左边节点，否则 t等于t的右边节点，如果相等key 新的value覆盖原有的value并返回原有的value
                if (cmp < 0)
                    t = t.left;
                else if (cmp > 0)
                    t = t.right;
                else
                    return t.setValue(value);
            } while (t != null);
        }
        else {
            if (key == null)
                throw new NullPointerException();
            @SuppressWarnings("unchecked")
                Comparable<? super K> k = (Comparable<? super K>) key;
            do {
                parent = t;
                cmp = k.compareTo(t.key);
                if (cmp < 0)
                    t = t.left;
                else if (cmp > 0)
                    t = t.right;
                else
                    return t.setValue(value);
            } while (t != null);
        }
        // 将新插入的节点作为parent节点的子节点
        Entry<K,V> e = new Entry<>(key, value, parent);
         // 如果新插入的key小于t的key t等于t的左边节点，否则 t等于t的右边节点，
        if (cmp < 0)
            parent.left = e;
        else
            parent.right = e;
        // 修复红黑树
        fixAfterInsertion(e);
        size++;
        modCount++;
        return null;
    }
}    
```

上面代码就是实现“排序二叉树”的关键算法。每当程序希望添加新节点时，总是从树的根节点开始比较，即将根节点当成当前节点。

如果新增节点大于当前节点且当前节点的右子节点存在，则以右子节点作为当前节点；

如果新增节点小于当前节点且当前节点的左子节点存在，则以左子节点作为当前节点；

如果新增节点等于当前节点，则用新增节点覆盖当前节点，并结束循环—直到找到某个节点的左、右子节点不存在，将新节点添加为该节点的子节点。如果新节点比该节点大，则添加其为右子节点；如果新节点比该节点小，则添加其为左子节点

**当TreeMap根据key来取value时TreeMap对应的方法如下：**

```java
    public V get(Object key) {
        // 根据指定的key取出Entry
        Entry<K,V> p = getEntry(key);
        // 返回Entry包含的value
        return (p==null ? null : p.value);
    }

    final Entry<K,V> getEntry(Object key) {
        // 如果comparator不为null 表明程序采用定制排序
        if (comparator != null)
            // 调用getEntryUsingComparator取出对应的key
            return getEntryUsingComparator(key);
        // 如果key形参的值为null 抛出异常
        if (key == null)
            throw new NullPointerException();
        @SuppressWarnings("unchecked")
        	// 将key强制转换为Comparable实例
            Comparable<? super K> k = (Comparable<? super K>) key;
        // 从树的根节点开始
        Entry<K,V> p = root;
        while (p != null) {
            // 拿key与当前节点的key进行比较
            int cmp = k.compareTo(p.key);
            // key小于当前节点的key “左子树” 搜索，大于时 “右子树”搜索，否者返回目标Entry
            if (cmp < 0)
                p = p.left;
            else if (cmp > 0)
                p = p.right;
            else
                return p;
        }
        return null;
    }
```

上面的getEntry(Object obj)方法也是充分利用排序二叉树的特征来搜索目标Entry。程序依然从二叉树的根节点开始，如果被搜索节点大于当前节点，程序向“右子树”搜索；如果被搜索节点小于当前节点，程序向“左子树”搜索；如果相等，那就是找到了指定节点。

当TreeMap里的comparator != null，即表明该TreeMap采用了定制排序。在采用定制排序的方式下，TreeMap采用getEntryUsingComparator(key)方法来根据key获取Entry。下面是该方法的代码

```java
final Entry<K,V> getEntryUsingComparator(Object key) {
        @SuppressWarnings("unchecked")
            K k = (K) key;
    	// 获取TreeMap的comparator
        Comparator<? super K> cpr = comparator;
        if (cpr != null) {
            // 从树的根节点开始
            Entry<K,V> p = root;
            while (p != null) {
                // 拿key与当前节点的key进行比较
                int cmp = cpr.compare(k, p.key);
                // key小于当前节点的key “左子树” 搜索，大于时 “右子树”搜索，否者返回目标Entry
                if (cmp < 0)
                    p = p.left;
                else if (cmp > 0)
                    p = p.right;
                else
                    return p;
            }
        }
        return null;
    }
```

从内部结构来看，TreeMap本质上就是一棵“红黑树”，而TreeMap的每个Entry就是该红黑树的一个节点

### HashMap和TreeMap

Map集合是一个关联数组，它包含两组值：一组是所有key组成的集合，因为Map集合的key不允许重复，而且Map不会保存key加入的顺序，因此这些key可以组成一个Set集合；另外一组是value组成的集合，因为Map集合的value完全可以重复，而且Map可以根据key来获取对应的value，所以这些value可以组成一个List集合

```java
public class Test{

    public static void main(String[] args){
        HashMap<String,Double> hashMap = new HashMap<>();
        TreeMap<String,Double> treeMap = new TreeMap<>();
        hashMap.put("java",100.0);
        hashMap.put("js",79.0);
        hashMap.put("vue",88.0);
        System.out.println(hashMap.values()); // [100.0, 88.0, 79.0]
        System.out.println(hashMap.values().getClass()); // class java.util.HashMap$Values
        treeMap.put("英语",100.0);
        treeMap.put("数学",120.3);
        System.out.println(treeMap.values()); // [120.3, 100.0]
        System.out.println(treeMap.values().getClass());// class java.util.TreeMap$Values
    }

}
```

**HashMap的Values()方法**

```java
public Collection<V> values() {
        Collection<V> vs;
        return (vs = values) == null ? (values = new Values()) : vs;
    }
```

看下内部类Vaules类

```java
 final class Values extends AbstractCollection<V> {
        public final int size()                 { return size; }
        public final void clear()               { HashMap.this.clear(); }
        public final Iterator<V> iterator()     { 
            // 返回newValueIterator()方法的返回值
            return new ValueIterator(); 
        }
        public final boolean contains(Object o) { return containsValue(o); }
        public final Spliterator<V> spliterator() {
            return new ValueSpliterator<>(HashMap.this, 0, -1, 0, 0);
        }
        public final void forEach(Consumer<? super V> action) {
            Node<K,V>[] tab;
            if (action == null)
                throw new NullPointerException();
            if (size > 0 && (tab = table) != null) {
                int mc = modCount;
                for (int i = 0; i < tab.length; ++i) {
                    for (Node<K,V> e = tab[i]; e != null; e = e.next)
                        action.accept(e.value);
                }
                if (modCount != mc)
                    throw new ConcurrentModificationException();
            }
        }
    }
```







```java
    final class ValueIterator extends HashIterator
        implements Iterator<V> {
        public final V next() { return nextNode().value; }
    }
```

HashMap的values()方法表面上返回了一个Values 集合对象，但这个集合对象并不能添加元素。它的主要功能是用于遍历HashMap里的所有value，而遍历集合的所有value则主要依赖于HashIterator的nextNode()方法来实现。对于HashMap而言，每个Entry都持有一个引用变量指向下一个Entry，因此HashMap实现nextNode()方法非常简单。下面是HashIterator的nextNode()方法的源代码

```java
final Node<K,V> nextNode() {
            Node<K,V>[] t;
            Node<K,V> e = next;
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
            if (e == null)
                throw new NoSuchElementException();
            if ((next = (current = e).next) == null && (t = table) != null) {
                do {} while (index < t.length && (next = t[index++]) == null);
            }
            return e;
        }
```



**TreeMap的Values()方法**

```java
    public Collection<V> values() {
        Collection<V> vs = values;
        return (vs != null) ? vs : (values = new Values());
    }
```

```java
    class Values extends AbstractCollection<V> {
        public Iterator<V> iterator() {
            // 返回 以TreeMap中最小的节点创建一个ValueIterator对象
            return new ValueIterator(getFirstEntry());
        }
		// 返回外部类实例的size
        public int size() {
            return TreeMap.this.size();
        }

        public boolean contains(Object o) {
            return TreeMap.this.containsValue(o);
        }

        public boolean remove(Object o) {
            // 从TreeMap中最小的节点开始搜索，不断搜索下一个节点
            for (Entry<K,V> e = getFirstEntry(); e != null; e = successor(e)) {
                // 如果找到节点
                if (valEquals(e.getValue(), o)) {
                    // 执行删除
                    deleteEntry(e);
                    return true;
                }
            }
            return false;
        }

        public void clear() {
            // 调用外部类的clear()实例方法来清空该集合
            TreeMap.this.clear();
        }

        public Spliterator<V> spliterator() {
            return new ValueSpliterator<K,V>(TreeMap.this, null, null, 0, -1, 0);
        }
    }
```

上面Values类与HashMap中Values类的区别不是太大，其中size()、contains(Object o)和clear()等方法也依赖于外部类HashMap的方法来提供实现。不过由于TreeMap是通过“红黑树”来实现的，因此上面程序中还用到了TreeMap提供的以下两个简单的工具方法。

■ getFirstEntry()：获取TreeMap底层“红黑树”中最左边的“叶子节点”，也就是“红黑树”中最小的节点，即TreeMap中第一个节点。

■ successor(Entry<K,V> t)：获取TreeMap中指定Entry（t）的下一个节点，也就是“红黑树”中大于t节点的最小节点。

getFirstEntry()方法的实现比较简单：程序不断搜索“左子树”，直到找到最左边的“叶子节点”。该方法的实现代码如下

```java
    final Entry<K,V> getFirstEntry() {
        Entry<K,V> p = root;
        if (p != null)
            // 不断的搜索左子树，直到p成为最左子树的叶子节点
            while (p.left != null)
                p = p.left;
        return p;
    }
```

successor(Entry<K,V> t)方法实现稍稍复杂一点，该方法实现了搜索“红黑树”中大于指定节点的最小节点，其代码如下

```java
static <K,V> TreeMap.Entry<K,V> successor(Entry<K,V> t) {
        if (t == null)
            return null;
    	// 如果右子树存在，搜索右子树中最小的节点
        else if (t.right != null) {
            // 先获取其右子树节点
            Entry<K,V> p = t.right;
            // 不断的搜索左子树节点，直到找到最左的叶子节点
            while (p.left != null)
                p = p.left;
            return p;
        } else {
            // 右子树不存在
            Entry<K,V> p = t.parent;
            Entry<K,V> ch = t;
            // 只需父节点存在，且ch是父节点的右节点
            // 表明ch大于其父子树节点，循环一直继续
            // 直到父节点为null，或者ch变成父节点的子节点，此时父节点大于搜索的节点
            while (p != null && ch == p.right) {
                ch = p;
                p = p.parent;
            }
            return p;
        }
    }
```

通过TreeMap提供的这个successor(Entry<K,V> t)静态方法，可以非常方便地、由小到大遍历 TreeMap 底层的“二叉树”。实际上，完全可以通过这个静态方法来由小到大地遍历TreeMap的所有元素。但TreeMap为了保持Map用法上的一致性，依然通过Values的iterator()方法来遍历Map中所有value。

**总结：**

HashMap、TreeMap的values()方法的实现要更巧妙。这两个Map对象values()方法返回的是一个不存储元素的Collection集合，当程序遍历Collection集合时，实际上就是遍历Map对象的value。HashMap和TreeMap的values()方法并未把Map中的value重新组合成一个包含元素的集合对象，这样就可以降低系统内存开销

## 引用深入理解

### 引用概念



通过关键字new创建Java对象，即可视作为Java对象申请内存空间，JVM会在堆内存中为每个对象分配空间；当一个Java对象失去引用时，JVM的垃圾回收机制会自动清除它们，并回收它们所占用的内存空间。

Java内存管理包括内存分配（创建Java对象时）和内存回收两个方面（回收Java对象时）。这两方面工作都是由JVM自动完成的，因此降低了Java程序员的学习难度，以至让很多初级Java程序员不再关心程序内存分配。



对于JVM的垃圾回收机制来说，是否回收一个对象的标准在于：是否还有引用变量引用该对象？只要有引用变量引用该对象，垃圾回收机制就不会回收它

可以把JVM内存中对象引用理解成一种有向图，把引用变量、对象都当成为有向图的顶点，将引用关系当成图的有向边，有向边总是从引用端指向被引用的Java对象。



当一个对象在堆内存中运行时，根据它在对应有向图中的状态，可以把它所处的状态分成如下3种。

■可达状态：当一个对象被创建后，有一个以上的引用变量引用它。在有向图中可从起始顶点导航到该对象，那它就处于可达状态，程序可通过引用变量来调用该对象的属性和方法。

■可恢复状态：如果程序中某个对象不再有任何引用变量引用它，它将先进入可恢复状态，此时从有向图的起始顶点不能导航到该对象。在这个状态下，系统的垃圾回收机制准备回收该对象所占用的内存。在回收该对象之前，系统会调用可恢复状态的对象的finalize方法进行资源清理，如果系统在调用finalize方法重新让一个以上引用变量引用该对象，则这个对象会再次变为可达状态；否则，该对象将进入不可达状态。

■ 不可达状态：当对象的所有关联都被切断，且系统调用所有对象的finalize方法依然没有使该对象变成可达状态，那这个对象将永久性地失去引用，最后变成不可达状态。只有当一个对象处于不可达状态时，系统才会真正回收该对象所占有的资源。



​     ![0](data:image/jpeg;base64,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) 



一个对象可以被一个方法局部变量所引用，也可以被其他类的类变量引用，或者被其他对象的实例变量所引用。当某个对象被其他类的类变量引用时，只有该类被销毁后，该对象才会进入可恢复状态；当某个对象被其他对象的实例变量引用时，只有当引用该对象的对象被销毁或变成不可达状态后，该对象才会进入不可达状态。

### 引用分类

为了更好地管理对象的引用，从JDK 1.2开始， Java在java.lang.ref包下提供了3个类：SoftReference 、PhantomReference和WeakReference。它们分别代表了系统对对象的3种引用方式：软引用、虚引用和弱引用。

#### **强引用**

程序创建一个对象，并把这个对象赋给一个引用变量，这个引用变量就是强引用。

Java程序可通过强引用来访问实际的对象，前面介绍的程序中的所有引用变量都是强引用的方式。当一个对象被一个或一个以上的强引用变量所引用时，它处于可达状态，它不可能被系统垃圾回收机制回收。

强引用是Java编程中广泛使用的引用类型，被强引用所引用的Java对象绝不会被垃圾回收机制回收，即使系统内存非常紧张；即使有些Java对象以后永远都不会被用到，JVM也不会回收被强引用所引用的Java对象。

由于JVM肯定不会回收强引用所引用的Java对象，因此强引用是造成Java内存泄漏的主要原因之一。



比如创建一个Person类型的数组，初始化为100000，这个时候通过循环依次赋值，然后调用gc进行回收，.gc（） .runFinallize（）这个时候会进行内存泄露，

```java
class Person{
    String name;
    int age;
    public Person(String name , int age){
        this.name=name;
        this.age=age;
    }
    public String toString(){
        return "Personname="+name
                           +", age="+age+"]";
    }
}
public class StrongReferenceTest{
    public static void main(String] args){
        Person[] people =
                            new Person[100000];
        for (int i=0 ; i < people.length ; i++){
            people[i]=new Person(
                                  "名字"+i , (i+1) ＊ 4 % 100);
        }
        System.out.println(people[2].get());
        System.out.println(people[4].get());
        //通知系统进行垃圾回收
        System.gc();
        System.runFinalization();
        //垃圾回收机制运行之后，SoftReference数组里的元素保持不变
        System.out.println(people[2].get());
        System.out.println(people[4].get());
    }
}
```

上面程序以传统方式创建了一个Person数组，该Person长度为100000，即程序的堆内存将保存100000个Person对象，这将使得程序因为系统内存不足而中止。运行上面程序出现异常<font color="red"> Exception in thread "main" java.lang.OutOfMemoryError:java heap space</font>

解决方法使用软引用

#### **软引用**

软引用需要通过SoftReference类来实现，当一个对象只具有软引用时，它有可能被垃圾回收机制回收。对于只有软引用的对象而言，当系统内存空间足够时，它不会被系统回收，程序也可使用该对象；当系统内存空间不足时，系统将会回收它。

软引用通常用于对内存敏感的程序中，软引用是强引用很好的替代。对于被强引用所引用的Java对象而言，无论系统的内存如何紧张，即使某些Java对象以后再也不可能使用，垃圾回收机制依然不会回收它所占用的内存。对于软引用则不同，当系统内存空间充足时，软引用与强引用没有太大的区别；当系统内存空间不足时，被软引用所引用的Java对象可以被垃圾回收机制回收，从而避免系统内存不足的异常。

当程序需要大量创建某个类的新对象，而且有可能重新访问已创建老对象时可以充分使用软引用来解决内存紧张的难题。

```JAVA
class Person{
    String name;
    int age;
    public Person(String name , int age){
        this.name=name;
        this.age=age;
    }
    public String toString(){
        return "Personname="+name
                           +", age="+age+"]";
    }
}
public class SoftReferenceTest{
    public static void main(String] args){
        SoftReference<Person>[] people =
                            new SoftReference[100];
        for (int i=0 ; i < people.length ; i++){
            people[i]=new SoftReference<Person>(new Person(
                                  "名字"+i , (i+1) ＊ 4 % 100));
        }
        System.out.println(people[2].get());
        System.out.println(people[4].get());
        //通知系统进行垃圾回收
        System.gc();
        System.runFinalization();
        //垃圾回收机制运行之后，SoftReference数组里的元素保持不变
        System.out.println(people[2].get());
        System.out.println(people[4].get());
    }
}
```

​               

#### **弱引用**

弱引用与软引用有点相似，区别在于弱引用所引用对象的生存期更短。弱引用通过WeakReference类实现，弱引用和软引用很像，但弱引用的引用级别更低。对于只有弱引用的对象而言，当系统垃圾回收机制运行时，不管系统内存是否足够，总会回收该对象所占用的内存。当然，并不是说当一个对象只有弱引用时，它就会立即被回收—正如那些失去引用的对象一样，必须等到系统垃圾回收机制运行时才会被回收。

```java
public class WeakReferenceTest{
     public static void main(String[] args) throws Exception{
                //创建一个字符串对象
                String str=new String("疯狂Java讲义");
                //创建一个弱引用，让此弱引用引用到“疯狂Java讲义”字符串
                WeakReference wr=new WeakReference(str);  //①
                //切断str引用和“疯狂Java讲义”字符串之间的引用
                str=null;     //②
                //取出弱引用所引用的对象
                System.out.println(wr.get());  //③                
                //强制垃圾回收
                System.gc();
                System.runFinalization();

               //再次取出弱引用所引用的对象
                System.out.println(wr.get());  //④
      }
}
```

上面程序创建了一个“疯狂Java 讲义”字符串对象，并让 str 引用变量引用它。执行①行粗体字代码行时，系统创建了一个弱引用对象，并让该对象和str引用同一个对象。当程序执行到②行代码时，切断了str和“疯狂Java 讲义”字符串对象之间的引用关系，此时系统内有如图4.6所示。

从图4.6中可以看出：此时“疯狂Java讲义”字符串对象只有一个弱引用对象引用它，程序依然可以通过这个弱引用对象来访问该字符串常量，程序中③行代码依然可以输出“疯狂Java讲义”。接下来程序强制垃圾回收，如果系统垃圾回收机制启动，只有弱引用的对象就会被清理掉。当程序执行④行代码时，通常就会看到输出null，这表明该对象已经被清理了。



弱引用具有很大的不确定性，因为每次垃圾回收机制执行时都会回收弱引用所引用的对象，而垃圾回收机制的运行又不受程序员的控制，因此程序获取弱引用所引用的Java对象时必须小心空指针异常— 通过弱引用所获取的Java对象可能是null。

#### **虚引用**

软引用和弱引用可以单独使用，但虚引用不能单独使用，单独使用虚引用没有太大的意义。虚引用的主要作用就是跟踪对象被垃圾回收的状态，程序可以通过检查与虚引用关联的引用队列中是否已经包含指定的虚引用，从而了解虚引用所引用对象是否即将被回收。

引用队列由java.lang.ref. ReferenceQueue类表示，它用于保存被回收后对象的引用。当把软引用、弱引用和引用队列联合使用时，系统回收被引用的对象之后，将会把被回收对象对应的引用添加到关联的引用队列中。与软引用和弱引用不同的是，虚引用在对象被释放之前，将把它对应的虚引用添加到它的关联的引用队列中，这使得可以在对象被回收之前采取行动。

虚引用通过PhantomReference类实现，它完全类似于没有引用。虚引用对对象本身没有太大影响，对象甚至感觉不到虚引用的存在。如果一个对象只有一个虚引用，那它和没有引用的效果大致相同。虚引用主要用于跟踪对象被垃圾回收的状态，虚引用不能单独使用，虚引用必须和引用队列（ReferenceQueue）联合使用。

```java
public class PhantomReferenceTest {
    public static void main(String[] args) throws Exception {
        //创建一个字符串对象
        String str = new String("疯狂Java讲义");

        //创建一个引用队列
        ReferenceQueue rq = new ReferenceQueue();

        //创建一个虚引用，让此虚引用引用到“疯狂Java讲义”字符串
        PhantomReference pr = new PhantomReference(str, rq);

        //切断str引用和“疯狂Java讲义”字符串之间的引用
        str = null;

        //试图取出虚引用所引用的对象

        //程序并不能通过虚引用访问被引用的对象，所以此处输出null
        System.out.println(pr.get()); //①

        //强制垃圾回收
        System.gc();

        System.runFinalization();

        //取出引用队列中最先进入队列中引用与pr进行比较
        System.out.println(rq.poll() == pr); //②
    }
}

```

因为系统无法通过虚引用来获得被引用的对象，所以执行①处的输出语句时，程序将输出null（即使此时并未强制进行垃圾回收）。当程序强制垃圾回收后，只有虚引用引用的字符串对象将会被垃圾回收，当被引用的对象被回收后，对应引用将被添加到关联的引用队列中，因而将在②代码处看到输出true。

使用这些引用类可以避免在程序执行期间将对象留在内存中。如果以软引用、弱引用或虚引用的方式引用对象，垃圾回收器就能够随意地释放对象。如果希望尽可能减小程序在其生命周期中所占用的内存大小，这些引用类就很有好处。

最后需要指出的是：要使用这些特殊的引用类，就不能保留对对象的强引用。如果保留了对对象的强引用，就会浪费这些类所提供的任何好处。



## java内存泄露深入理解:

程序运行过程中会不断地分配内存空间，那些不再使用的内存空间应该即时回收它们，从而保证系统可以再次使用这些内存，如果存在无用的内存没有被回收回来，那就是内存泄漏

### 垃圾回收机制

**垃圾回收机制主要完成下面两件事情：**

■ 跟踪并监控每个Java对象，当某个对象处于不可达状态时，回收该对象所占用的内存；

■ 清理内存分配、回收过程中产生的内存碎片。



**对于一个垃圾回收器的设计算法来说**，大致有如下可供选择的设计。

■ 串行回收（Serial）和并行回收（Parallel）：串行回收就是不管系统有多少个CPU，始终只用一个CPU来执行垃圾回收操作；而并行回收就是把整个回收工作拆分成多部分，每个部分由一个CPU负责，从而让多个CPU并行回收。并行回收的执行效率很高，但复杂度增加，另外也有其他一些副作用，比如内存碎片会增加。

■ 并发执行（Concurrent）和应用程序停止（Stop-the-world）：Stop-the-world的垃圾回收方式在执行垃圾回收的同时会导致应用程序的暂停。并发执行的垃圾回收虽然不会导致应用程序的暂停，但由于并发执行垃圾回收需要解决和应用程序的执行冲突（应用程序可能会在垃圾回收的过程中修改对象），因此并发执行垃圾回收的系统开销比Stop-the-world更高，而且执行时也需要更多的堆内存。

■ 压缩（Compacting）和不压缩（Non-compacting）和复制（Copying）：为了减少内存碎片，支持压缩的垃圾回收器会把所有的活对象搬迁到一起，然后将之前占用的内存全部回收。不压缩式的垃圾回收器只是回收内存，这样回收回来的内存不可能是连续的，因此将会有较多的内存碎片。较之压缩式的垃圾回收，不压缩式的垃圾回收回收内存快，而分配内存时就会更慢，而且无法解决内存碎片的问题。复制式的垃圾回收会将所有可达对象复制到另一块相同的内存中，这种方式的优点是垃圾回收过程不会产生内存碎片，但缺点也很明显，需要复制数据和额外的内存。

上面介绍的复制、不压缩、压缩都是垃圾回收器回收已用内存空间的方式，关于这3 种回收方式的详述如下。

■ 复制：将堆内分成两个相同空间，从根（类似于前面介绍的有向图的起始顶点）开始访问每一个关联的可达对象，将空间A的可达对象全部复制到空间B，然后一次性回收整个空间A。

对于复制算法而言，因为只需访问所有的可达对象，将所有可达对象复制走之后就回收整个空间，完全不用理会那些不可达的对象，所以遍历空间的成本较小，但需要巨大的复制成本和较多的内存。

■ 标记清除（mark-sweep）：也就是不压缩的回收方式。垃圾回收器先从根开始访问所有可达对象，将它们标记为可达状态，然后再遍历一次整个内存区域，把所有没有标记为可达的对象进行回收处理。

标记清除（mark-sweep）无需进行大规模的复制操作，而且内存利用率高。但这种算法需要两次遍历堆内存空间，遍历的成本较大，因此造成应用程序暂停的时间随堆空间大小线性增大。而且垃圾回收回来的内存往往是不连续的，因此整理后堆内存里的碎片很多。

■ 标记压缩（mark-sweep-compact）：这是压缩方式，这种方式充分利用上述两种算法的优点，垃圾回收器先从根开始访问所有可达对象，将它们标记为可达状态。接下来垃圾回收器会将这些活动对象搬迁在一起，这个过程也被称为内存压缩，然后垃圾回收机制再次回收那些不可达对象所占用的内存空间，这样就避免了回收产生的内存碎片。



从上面介绍可以看出，不论采用哪种机制实现垃圾回收，不论采用哪种内存回收方式，具体实现起来总是利弊参半的。因此，实际实现垃圾回收时总会综合使用多种设计方式，也就是针对不同的情况采用不同的垃圾回收实现。

现行的垃圾回收器用分代的方式来采用不同的回收设计。分代的基本思路是根据对象生存时间的长短，把堆内存分成3个代：

■ Young（年轻代）；

■ Old（老年代）；

■ Permanent（永久代）。

垃圾回收器会根据不同代的特点采用不同的回收算法，从而充分利用各种回收算法的优点。

注意

此处将Young翻译为年轻代，将Old翻译为老年代，将Permanent翻译为永久代其实感觉有点别扭，只是希望读者能大致理解垃圾回收器对堆内存进行分代的思路，至于这3个代的名称并不重要，本书后文将直接采用英文说法。

###  **堆内存分代回收**

分代回收的一个依据就是对象生存时间的长短，然后再根据不同代采取不同的垃圾回收策略。采用这种“分代回收”的策略基于如下两点事实：

■ 绝大对数的对象不会被长时间引用，这些对象在其Young期间就会被回收；

■ 很老的对象（生存时间很长）和很新的对象（生存时间很短）之间很少存在相互引用的情况。

上面这两点事实不仅在Java语言中如此，其他面向对象的编程语言也大致遵循这两个事实。

根据上面两点事实，对于Young代的对象而言，大部分对象都会很快就进入不可达状态，只有少量的对象能熬到垃圾回收执行时，而垃圾回收器只需要保留Young代中处于可达状态的对象，如果采用复制算法只需要少量的复制成本，因此大部分垃圾回收器对Young代都采用复制算法。

**1.Young代**

对Young代采用复制算法只需遍历那些处于可达状态的对象，而且这些对象的数量较少，可复制成本也不大，因此可以充分发挥复制算法的优点。

Young代由1个Eden区和2个Survivor区构成。绝大多数对象先分配到Eden区中（有一些大的对象可能会直接被分配到Old代中），Survivor区中的对象都至少在Young代中经历过一次垃圾回收，所以这些对象在被转移到Old代之前会先保留在Survivor空间中。同一时间2个Survivor空间中有一个用来保存对象，而另一个是空的，用来在下次垃圾回收时保存Young代中的对象。每次复制就是将Eden和第1个Survivor的可达对象复制到第2个Survivor区，然后清空Eden与第1个Survivor区。Eden与第1个Survivor区如图4.11所示。



图4.11 Yound代的分区

Eden和Survivor的比例通过-XX:SurvivorRatio附加选项来设定，默认为32。如果Survivor太大会产生浪费，太小则会使一些Young代的对象提前进入Old代。

**2.Old代**

如果Young代中对象经过数次垃圾回收依然还没有被回收掉，即这个对象经过足够长的时间还处于可达状态，垃圾回收机制就会将这个对象转移到Old代。图显示了这个对象由Young代提升为Old代的过程。





Old代的大部分对象都是“久经考验”的“老人”了，因此它们没那么容易死。而且随着时间的流逝，Old代的对象会越来越多，因此Old代的空间要比Young代空间更大。出于这两点考虑，Old代的垃圾回收具有如下两个特征：

■ Old代垃圾回收的执行频率无需太高，因为很少有对象会死掉；

■ 每次对Old代执行垃圾回收需要更长的时间来完成。

基于以上考虑，垃圾回收器通常会使用标记压缩算法。这种算法可以避免复制 Old代的大量对象，而且由于Old代的对象不会很快死亡，回收过程不会大量地产生内存碎片，因此相对比较划算。

**3.Permanent代**

Permanent代主要用于装载Class、方法等信息，默认为64M，垃圾回收机制通常不会回收Permanent代中的对象。对于那些需要加载很多类的服务器程序，往往需要加大Permanent代内存，否则可能因为内存不足而导致程序终止。

注意

对于像Hibernate、Spring这类喜欢AOP动态生成类的框架，往往会生成大量的动态代理类，因此需要更多的Permanent代内存，相信读者在调试、运行Hibernate、Spring程序时应该见过java.lang.OutOfMemoryError: PermGen space的错误，这就是由Permanent代内存耗尽所导致的错误。

当Young代的内存将要用完的时候，垃圾回收机制会对Young代进行垃圾回收，垃圾回收机制会采用较高的频率对Young代进行扫描和回收。因为这种回收的系统开销比较小，因此也被称为次要回收（minor collection）。当Old代的内存将要用完时，垃圾回收机制会进行全回收，也就是对Young代和Old代都要进行回收，此时回收成本就大得多了，因此也称为主要回收（major collection）。

通常来说，Young代的内存会先被回收，而且会使用专门的回收算法（复制算法）来回收Young代的内存；对于Old代的回收频率则要低得多，因此也会采用专门的回收算法。如果需要进行内存压缩，每个代都独立地进行压缩。

4.3.3 与垃圾回收的附加选项

下面两个选项用于设置Java虚拟机内存大小。

■-Xmx：设置Java虚拟机堆内存的最大容量，如java-Xmx256m XxxClass。

■-Xms：设置Java虚拟机堆内存的初始容量，如java-Xms128m XxxClass。

下面选项都是关于Java垃圾回收的附加选项。

■-XX:MinHeapFreeRatio=40：设置Java堆内存最小的空闲百分比，默认值为40，如java-XX:MinHeapFreeRatio=40 XxxClass。

■-XX:MaxHeapFreeRatio=70：设置Java堆内存最大的空闲百分比，默认值为70，如java-XX:MaxHeapFreeRatio=70 XxxClass。

■-XX:NewRatio=2：设置Young/Old内存的比例，如java-XX:NewRatio=1 XxxClass。

■-XX:NewSize=size：设置 Young代内存的默认容量，如java-XX:NewSize=64m XxxClass。

■-XX:SurvivorRatio=8：设置Young代中eden/survivor的比例，如java-XX:SurvivorRatio =8 XxxClass。

■-XX:MaxNewSize=size：设置Young代内存的最大容量，如java-XX:MaxNewSize =128m XxxClass。

注意

当设置Young代的内存超过了-Xmx设置的大小时，Young设置的内存大小将不会起作用，JVM会自动将Young代内存设置为与-Xmx设置的大小相等。

■-XX:PermSize=size：设置 Permanent代内存的默认容量，如java-XX:PermSize =128m XxxClass。

■-XX:MaxPermSize=64m：设置Permanent代内存的最大容量，如java-XX:MaxPermSize= 128m XxxClass。

此处只是介绍了垃圾回收相关的常用选项，关于Java垃圾回收的常用选项请参看Sun官方站点http://java.sun.com/javase/technologies/hotspot/vmoptions.jsp页面的介绍。

### 常见的垃圾回收器

#### 串行回收器

串行回收器通过运行Java程序时使用-XX:+UseSerialGC附加选项启用

串行回收器对Young代和Old代的回收都是串行的（只使用一个CPU），而且垃圾回收执行期间会使得应用程序产生暂停。具体策略为，Young代采用串行复制的算法，Old代采用串行标记压缩算法

串行回收器对Old代的回收采用串行、标记压缩算法（mark-sweep-compact），这个算法有3个阶段：mark（标识可达对象）、sweep（清除）、compact（压缩）。在 mark阶段，回收器会识别出哪些对象仍然是可达的，在 sweep阶段将会回收不可达对象所占用的内存。在compact阶段回收器执行sliding compaction，把活动对象往Old代的前端启动，而在尾部保留一块连续的空间，以便下次为新对象分配内存空间



#### 并行回收器

并行回收器通过运行Java程序时使用-XX:+UseParallelGC附加选项启用，它可以充分利用计算机的多个CPU来提高来垃圾回收吞吐量。并行回收器对于Young代采用与串行回收器基本相似的回收算法，只是增加了多CPU并行的能力，即同时启动多线程并行来执行垃圾回收。线程数默认为CPU个数，当计算机CPU很多时，可用-XX:ParallelGCThreads=size来减少并行线程的数目。并行回收器对于Old代采用与串行回收器完全相同的回收算法，不管计算机有几个CPU，并行回收器依然采用单线程、标记整理的方式进行回收。对于并行回收器而言，只有多CPU并行的机器才能发挥其优势

#### 并行压缩回收器（Parallel Compacting Collector）

并行压缩回收器是在J2SE 5.0 update 6开始引入的，它和并行回收器最大的不同是对Old代的回收使用了不同的算法，并行压缩回收器最终会取代并行回收器。并行压缩回收器通过运 行 Java程序时 使 用-XX:+UseParallelOldGC 附 加 选项 启用，一 样可 通 过-XX:ParallelGCThreads=size来设置并行线程的数目。

并行压缩回收器对于Young代采用与并行回收器完全相同的回收算法。

并行压缩回收器的改变主要体现在对Old代的回收上。系统首先将Old代划分成几个固定大小的区域。在mark阶段，多个垃圾回收线程会并行标记Old代中的可达对象。当某个对象被标记为可达对象时，还会更新该对象所在区域的大小以及该对象的位置信息

接下来是summary阶段。summary阶段操作直接操作Old代的区域，而不是单个的对象。由于每次垃圾回收的压缩都会在 Old代的左边部分存储大量可达对象，对这样的高密度可达对象的区域进行压缩往往很不划算。所以summary阶段会从最左边的区域开始检验每个区域的密度，当检测到某个区域中能回收的空间达到了某个数值的时候（也就是可达对象的密度较小时），垃圾回收器会判定该区域以及该区域右边的所有区域都应该进行回收，而该区域左边的区域都会被会被标识为密集区域，垃圾回收器既不会把新对象移动到这些密集区域中去，也不会对这些密集区域进行压缩。该区域和其右边的所有区域都会被进行压缩并回收空间。summary阶段目前还是串行操作，虽然并行是可以实现的，但重要性不如对mark和压缩阶段的并行重要。

最后是compact阶段。回收器利用summary阶段生成的数据识别出有哪些区域是需要装填的，多个垃圾回收线程可以并行地将数据复制到这些区域中。经过这个过程后，Old代的一端会密集地存在大量活动对象，另一端则存在大块的空闲块

#### 并行标识-清理  (Mark-Sweep) 回收器（CMS）

并发标识-清理回收器通过运行 Java程序时使用-XX:+UseConcMarkSweepGC 附加选项启用。CMS 回收器对 Young代的回收方式和并行回收器的回收方式完全相同。由于对Young的回收依然采用复制回收算法，因此垃圾回收时依然会导致程序暂停，除非依靠多CPU并行来提高垃圾回收的速度。

通常来说，建议适当加大Young代的内存。如果Young代内存够大就不用频繁地进行垃圾回收，而且增加垃圾回收的时间间隔后可以让更多的Young代对象自己死掉，从而避免复制。但将Young代内存设得过大也有一个坏处：当垃圾回收器回收Young代内存时，复制成本会显著上升（复制算法必须等Young满了之后才开回收），所以回收时会让系统暂停时间显著加长

CMS对Old代的回收多数是并发操作，而不是并行操作。垃圾回收开始的时候需要一个短暂的暂停，称之为初始标识（initial mark）。这个阶段仅仅标识出那些被直接引用的可达对象。接下来进入了并发标识阶段（concurrent markingphase），垃圾回收器会依据在初始标识中发现的可达对象来寻找其他可达对象。由于在并发标识阶段应用程序也会同时在运行，无法保证所有的可达对象都被标识出来，因此应用程序会再次很短地暂停一下，多线程并行地重新标记之前可能因为并发而漏掉的对象，这个阶段也被称为再标记（remark）阶段。完成了再标记以后，所有的可达对象都已经被标识出来了，接下来就可以运行并发清理操作了

对于串行、标记压缩的回收器而言，它可以等到 Old代满了之后再开始回收，反正垃圾回收总会让应用程序暂停。但CMS回收器要与应用程序并发运行，如果Old满了才开始回收，那应用程序将无内存可用，所以系统默认在Old代68%满的时候就开始回收。如果系统内存设得比较大，而且程序分配内存速度不是特别快时，可以通过-XX:CMSInitiatingOccupancy Fraction=ratio适当增大这个比例

对于CMS回收器而言，当垃圾回收器执行并发标识时，应用程序在运行的同时也在分配对象，因此 Old代也同时在增长。而且，虽然可达对象在标识阶段会被识别出来，但有些在标识阶段成为垃圾的对象并不能立即被回收，只有等到下次垃圾回收时才能被回收。因此CMS回收器较之前面的几种回收器需要更大的堆内存。对于Permanent代内存，CMS可通过运行 Java程序时使用-XX:+CMSClassUnloading Enabled-XX:+CMSPermGenSweepingEnabled附加选项强制回收Permanent代内存

### 内存管理技巧

#### 使用直接常量

当需要使用字符串，还有Byte、Short、Integer、Long、Float、Double、Boolean、Character包装类的实例时，程序不应该采用new的方式来创建对象，而应该直接采用直接量来创建它们

```java
// 例如需要使用hello字符串
String str="hello";
//上面这种方式会创建一个“hello”字符串，而且JVM的字符串缓存池还会缓存这个字符串。但如果程序使用如下代码
String str=new String("hello")
//此时程序同样创建了一个缓存在字符串缓存池中的“hello”字符串。除此之外str所引用的String对象底层还包含一个char[]数组，这个char[]数组里依次存放了h、e、l、l、o等字符。
```

#### 使用StringBuilder和StringBuffer进行字符串连接

String、StringBuilder、StringBuffer都可代表字符串，其中String代表字符序列不可变的字符串，而StringBuilder和StringBuffer都代表字符序列可变的字符串。

如果程序使用多个String对象进行字符串连接运算，在运行时将生成大量临时字符串，这些字符串会保存在内存中从而导致程序性能下降

#### 尽早释放无用对象

大部分时候，方法局部引用变量所引用的对象会随着方法结束而变成垃圾，因为局部变量的生存期限很短，当方法运行结束之时，该方法内的局部变量就结束了生命期限。因此，大部分时候程序无须将局部、引用变量显式设为null

```java
//例如下面就是无用的
public void info(){
	Object obj =new Object();
	System.out.println(obj.toString())
	// 下面的这句无用:随着info()方法执行完成，程序中obj引用变量的作用域就结束了，原来 obj所引用的对象就会变成垃圾
	obj=null;
}
```

#### 尽量少用静态变量

从理论上来说，Java对象何时被回收由垃圾回收机制决定，对程序员来说是不确定的。由于垃圾回收机制判断一个对象是否是垃圾的唯一标准就是该对象是否有引用变量引用它，因此推荐尽早释放对象的引用

最坏的情况是某个对象被static变量所引用，那么垃圾回收机制通常是不会回收这个对象所占的内存 

```java
class Person{
	static Object obj = new Object();
}
```

对于上面的Object对象而言，只要obj变量还引用到它，它就不会被垃圾回收机制所回收。

obj变量是Person类的静态变量，因此它的生命周期与Person类同步。在Person类不被卸载的情况下，Person类对应的Class对象会常驻内存，直到程序运行结束。因此obj所引用的Object对象一旦被创建，也会常驻内存，直到程序运行结束

#### 避免在经常调用的方法，循环中创建java对象

```java
public class test(){
	for(int i=0;i<10;i++){
		Object obj = new Object();
	}
}
```

上面代码在循环中创建了10个Object对象，虽然上面程序中的obj变量都是代码块的局部变量，当循环执行结束时这些局部变量都会失效，但由于这段循环导致Object对象会被创建10次，因此系统需要不断地为这10个对象分配内存空间，执行初始化操作。这10个对象的生存时间并不长，接下来系统又需要回收它们所占的内存空间，在这种不断的分配、回收操作中，程序的性能受到巨大的影响

#### 缓存经常使用的对象

如果有些对象需要被经常使用，可以考虑把这些对象用缓存池保存起来，这样当下次需要时就可直接拿出这些对象来用。典型的缓存就是数据连接池，数据连接池里缓存了大量数据库连接，每次程序需要访问数据库时都可直接取出数据库连接。

除此之外，如果系统中还有一些常用的基础信息，比如信息化信息里包含的员工信息、物料信息等，也考虑对它们进行缓存。实现缓存时通常有两种方式：

■ 使用HashMap进行缓存；

■ 直接使用某些开源的缓存项目。

如果直接使用HashMap进行缓存，程序员需要手动控制HashMap容器里key-value对不至于太多，因为当key-value太多时将导致HashMap占用过大的内存，从而导致系统性能下降

除了使用HashMap进行缓存之外，还可使用一些开源的缓存项目来解决这个问题。这些缓存项目都会主动分配一个一定大小的缓存容器，再按照一定算法来淘汰容器中不需要继续缓存的对象。这样一方面可以通过缓存已用过的对象来提高系统的运行效率，另一方面又可以控制缓存容器的无限制扩大，从而减少系统的内存占用。对于这种开源的缓存实现有很多选择，如OSCache、Ehcache等，它们大都实现了FIFO、MRU等常见的缓存算法

#### 尽量是不要使用finalize方法

在垃圾回收机制时已经提到，在一个对象失去引用之后，垃圾回收器准备回收该对象之前，垃圾回收机制会先调用该对象的finalize()方法来执行资源清理。出于这种考虑，可能有些开发者会考虑使用finalize()方法来进行资源清理。

实际上，将资源清理放在 finalize()方法中完成是非常拙劣的选择。根据前面介绍的垃圾回收算法，垃圾回收机制的工作量已经够大了，尤其是回收Young代内存时，大都会引起应用程序暂停，使得用户难以忍受。

在垃圾回收器本身已经严重制约应用程序性能的情况下，如果再选择使用finalize()方法进行资源清理，无疑是一种火上浇油的行为，这将导致垃圾回收器的负担更大，导致程序运行效率更差

#### 考虑使用SoftReference

当程序需要创建长度很大的数组时，可以考虑使用SoftReference来包装数组元素，而不是直接让将数组元素来引用对象。

SoftReference 是个很好的选择：当内存足够时，它的功能等同于普通引用；当内存不够时，它会牺牲自己，释放软引用所引用的对象

## java类加载器深入理解

https://blog.csdn.net/javazejian/article/details/73413292

类加载指的是将类的class文件读入内存，并为之创建一个java.lang.Class对象，也就是说，当程序中使用任何类时，系统都会为之建立一个java.lang.Class对象。

通过使用不同的类加载器，可以从不同来源加载类的二进制数据，通常有如下几种来源。

- 从本地文件系统加载class文件，这是前面绝大部分示例程序的类加载方式。
- 从JAR包加载class文件，这种方式也是很常见的，前面介绍JDBC编程时用到的数据库驱动类就放在JAR文件中，JVM可以从JAR文件中直接加载该class文件。
- 通过网络加载class文件。
- 把一个Java源文件动态编译，并执行加载。

类加载器通常无须等到“首次使用”该类时才加载该类，Java虚拟机规范允许系统预先加载某些类当类被加载之后，系统为之生成一个对应的Class对象，接着将会进入连接阶段，连接阶段负责把类的二进制数据合并到JRE中。类连接又可分为如下3个阶段。

- （1）验证：验证阶段用于检验被加载的类是否有正确的内部结构，并和其他类协调一致。
- （2）准备：类准备阶段则负责为类的静态Field分配内存，并设置默认初始值。
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

    public static void main(String[] args)throws ClassNotFoundException {

        ClassLoader cl = ClassLoader.getSystemClassLoader();
		// 下面语句仅仅是加载Tester类
        cl.loadClass("Tester");System.out.println("系统加载Tester类");
		// 下面语句才会初始化Tester类
        Class.forName("Tester");
    }

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

- （1）检测此Class是否载入过（即在缓存区中是否有此Class），如果有则直接进入第8步，否则接着执行第2步。
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
    public static Connection getConn(String url,String user, String pass) throws Exception {

        if (conn == null) {
			// 创建一个URL数组
            URL[] urls = {new URL("file:mysql-connector-java-3.1.10-bin.jar")};

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
        System.out.println(getConn("jdbc:mysql://localhost:3306/mysql" "root", "32147"));
    }

}
```

## java类型信息 Class对象与反射技术深入理解

