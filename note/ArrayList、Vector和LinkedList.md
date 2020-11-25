## List深入实践



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

