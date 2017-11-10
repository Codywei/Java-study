# Java 基础知识（2）

标签（空格分隔）： Java 基础

---

<h2>1.ArrayList和vector区别</h2>
ArrayList和Vector都实现了List接口，都是通过数组实现的。</br>
Vector是线程安全的，而ArrayList是非线程安全的。</br>
List第一次创建的时候，会有一个初始大小，随着不断向List中增加元素，当List 认为容量不够的时候就会进行扩容。Vector缺省情况下自动增长原来一倍的数组长度，ArrayList增长原来的50%。</br>
Vector与ArrayList对比分析可参见链接https://segmentfault.com/a/1190000003507587


<h2>2.ArrayList和LinkedList区别及使用场景</h2>
区别</br>
ArrayList:</br>
1.ArrayList底层是用数组实现的，可以认为ArrayList是一个可改变大小的数组。随着越来越多的元素被添加到ArrayList中，其规模是动态增加的。</br>
2.ArrayList不是线程安全的,多线程环境下可以考虑用Collections.synchronizedList(List l)函数返回一个线程安全的ArrayList类，也可以使用concurrent并发包下的CopyOnWriteArrayList类。</br>

Linklist:</br>
1.是一个继承于AbstractSequentialList的双向链表。它也可以被当作堆栈、队列或双端队列进行操作。</br>
2.实现 List 接口，能对它进行队列操作。</br>
3.实现 Deque 接口，即能将LinkedList当作双端队列使用。</br>
4.实现了Cloneable接口，即覆盖了函数clone()，能克隆。</br>
5.实现java.io.Serializable接口，这意味着LinkedList支持序列化，能通过序列化去传输。</br>
6.是非同步的。</br>
7.还实现了Queue接口，所以他还提供了offer(),peek(), poll()等方法。</br>

使用场景
LinkedList更适合从中间插入或者删除（链表的特性）。</br>
ArrayList更适合检索（数组的特性）。</br>

LinkList查找元素实现代码（但是未必有ArrayList快）</br>
```java
/** 
 * 获取index处的节点 
 * 算法思想：简单的二分查找。 
 * 如果index大于size的一半，从后往前找；否则从前往后找 
 */  
Node<E> node(int index) {  
    // assert isElementIndex(index);  
  
    if (index < (size >> 1)) {  
        Node<E> x = first;  
        for (int i = 0; i < index; i++)  
            x = x.next;  
        return x;  
    } else {  
        Node<E> x = last;  
        for (int i = size - 1; i > index; i--)  
            x = x.prev;  
        return x;  
    }  
```
参考一下链接：http://blog.csdn.net/ochangwen/article/details/50586260</br>
http://blog.csdn.net/qq_30739519/article/details/50877217



<h2>3.Collection和Collections的区别</h2>
1.java.util.Collection 是一个集合接口。它提供了对集合对象进行基本操作的通用接口方法。Collection接口在Java 类库中有很多具体的实现。Collection接口的意义是为各种具体的集合提供了最大化的统一操作方式。</br>
2.java.util.Collections 是一个包装类。它包含有各种有关集合操作的静态多态方法。此类不能实例化，就像一个工具类，服务于Java的Collection框架。


<h2>4.Concurrenthashmap实现原理</h2>
（这一部分可与HashMap、HashTable结合）
![此处输入图片的描述][1]
  上图显示了Concurrenthashmap的锁分段技术，ConcurrentHashMap是由Segment数组结构和HashEntry数组结构组成。</br>
(1)Segment是一种可重入锁ReentrantLock，在ConcurrentHashMap里扮演锁的角色，HashEntry则用于存储键值对数据。</br>
(2)一个ConcurrentHashMap里包含一个Segment数组，Segment的结构和HashMap类似，是一种数组和链表结构。</br> 
(3)一个Segment里包含一个HashEntry数组，每个HashEntry是一个链表结构的元素。</br> 
(4)每个Segment守护者一个HashEntry数组里的元素,当对HashEntry数组的数据进行修改时，必须首先获得它对应的Segment锁。</br> 

Concurrenthashmap因为添加了segment，所以需要两次hash定位。还利用了volatile关键字实现了多线程中共享变量的及时更新。Concurrenthashmap的get操作十分高效，无需加锁。</br> 
  
ConcurrentHashMap使用modCount变量进行size操作，在put , remove和clean方法里操作元素前都会将变量modCount进行加1，那么在统计size前后比较modCount是否发生变化，从而得知容器的大小是否发生变化。</br> 
 因为在累加count操作过程中，之前累加过的count发生变化的几率非常小，所以ConcurrentHashMap的做法是先尝试2次通过不锁住Segment的方式来统计各个Segment大小，如果统计的过程中，容器的count发生了变化，则再采用加锁的方式来统计所有Segment的大小。
 参考链接：http://ifeve.com/concurrenthashmap/
 
 
<h2>5.Error、Exception区别</h2>
Error类和Exception类的父类都是throwable类，他们的区别是：</br>
Error类一般是指与虚拟机相关的问题，如系统崩溃，虚拟机错误，内存空间不足，方法调用栈溢等。对于这类错误的导致的应用程序中断，仅靠程序本身无法恢复和和预防，遇到这样的错误，建议让程序终止。</br>
Exception类表示程序可以处理的异常，可以捕获且可能恢复。遇到这类异常，应该尽可能处理异常，使程序恢复运行，而不应该随意终止异常</br>


<h2>6.Unchecked Exception和Checked Exception，各列举几个</h2>

Unchecked Exception:</br>
1. 指的是程序的瑕疵或逻辑错误，并且在运行时无法恢复。</br>
2. 包括Error与RuntimeException及其子类，如：OutOfMemoryError,undeclaredThrowableException, IllegalArgumentException,IllegalMonitorStateException, NullPointerException,IllegalStateException,
IndexOutOfBoundsException等。</br>
3. 语法上不需要声明抛出异常。</br>


Checked Exception:</br>
1. 代表程序不能直接控制的无效外界情况（如用户输入，数据库问题，网络异常，文件丢失等）</br>
2. 除了Error和RuntimeException及其子类之外，如：ClassNotFoundException,
NamingException, ServletException, SQLException, IOException等。</br>
3. 需要try catch处理或throws声明抛出异常</br>

<h2> 7.Java中如何实现代理机制(JDK、CGLIB)</h2>

JDK动态代理：代理类和目标类实现了共同的接口，用到InvocationHandler接口。</br>
CGLIB动态代理：代理类是目标类的子类，用到MethodInterceptor接口。</br>
 


  [1]: http://ifeve.com/wp-content/uploads/2012/12/ConcurrentHashMap%E7%BB%93%E6%9E%84%E5%9B%BE.jpg