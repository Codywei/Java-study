# HashMap ,HashTable

标签（空格分隔）： java 基础

---

<h2>HashMap 实现原理</h2>

定义： HashMap是基于哈希表的Map接口的非同步实现。此实现提供所有可选的映射操作，并允许使用null值和null键。此类不保证映射的顺序，特别是它不保证该顺序恒久不变。</br>
    
数据结构：在java编程语言中，最基本的结构就是两种，一个是数组，另外一个是模拟指针（引用），所有的数据结构都可以用这两个基本结构来构造的，HashMap也不例外。HashMap实际上是一个“链表散列”的数据结构，即数组和链表的结合体。</br>
![此处输入图片的描述][1]
图片链接：http://www.admin10000.com/UploadFiles/Document/201311/16/20131116091617994485.JPG
```java
/** 
 * The table, resized as necessary. Length MUST Always be a power of two. 
 */  
transient Entry[] table;  
  
static class Entry<K,V> implements Map.Entry<K,V> {  
    final K key;  
    V value;  
    Entry<K,V> next;  
    final int hash;  
    ……  
}  
```
可以看出，Entry就是数组中的元素，每个 Map.Entry 其实就是一个key-value对，它持有一个指向下一个
元素的引用，这就构成了链表。</br>

存取：HashMap 在底层将 key-value 当成一个整体进行处理，这个整体就是一个 Entry 对象。HashMap 底层
采用一个 Entry[] 数组来保存所有的 key-value 对，当需要存储一个 Entry 对象时，会根据hash算法
来决定其在数组中的存储位置，再根据equals方法决定其在该数组位置上的链表中的存储位置；当需要取出一个Entry时，也会根据hash算法找到其在数组中的存储位置，再根据equals方法从该位置上的链表中取出该Entry。

put实现：</br>
```java
public V put(K key, V value) {  
    // HashMap允许存放null键和null值。  
    // 当key为null时，调用putForNullKey方法，将value放置在数组第一个位置。  
    if (key == null)  
        return putForNullKey(value);  
    // 根据key的keyCode重新计算hash值。  
    int hash = hash(key.hashCode());  
    // 搜索指定hash值在对应table中的索引。  
    int i = indexFor(hash, table.length);  
    // 如果 i 索引处的 Entry 不为 null，通过循环不断遍历 e 元素的下一个元素。  
    for (Entry<K,V> e = table[i]; e != null; e = e.next) {  
        Object k;  
        if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {  //此处需讨论
            V oldValue = e.value;  
            e.value = value;  
            e.recordAccess(this);  
            return oldValue;  
        }  
    }  
    // 如果i索引处的Entry为null，表明此处还没有Entry。  
    modCount++;  
    // 将key、value添加到i索引处。  
    addEntry(hash, key, value, i);  
    return null;  
}  
```



存元素：</br>
```java
void addEntry(int hash, K key, V value, int bucketIndex) {  
    // 获取指定 bucketIndex 索引处的 Entry   
    Entry<K,V> e = table[bucketIndex];  
    // 将新创建的 Entry 放入 bucketIndex 索引处，并让新的 Entry 指向原来的 Entry  
    table[bucketIndex] = new Entry<K,V>(hash, key, value, e);  
    // 如果 Map 中的 key-value 对的数量超过了极限  
    if (size++ >= threshold)  
    // 把 table 对象的长度扩充到原来的2倍。  
        resize(2 * table.length);  
} 
```

    
取元素：   </br> 
```java
public V get(Object key) {  
    if (key == null)  
        return getForNullKey();  
    int hash = hash(key.hashCode());  
    for (Entry<K,V> e = table[indexFor(hash, table.length)];  
        e != null;  
        e = e.next) {  
        Object k;  
        if (e.hash == hash && ((k = e.key) == key || key.equals(k)))  //此处需要讨论
            return e.value;  
    }  
    return null;  
} 
```

(k = e.key) == key || key.equals(k))这一句看似是重复的，其实要了解==和equal之间的区别，个人认为equal主要是针对key不为基本数据类型的类型（两个key值相同但不是存放在同个地址），==和equal之间的区别可以参考以下链接
http://www.cnblogs.com/dolphin0520/p/3592500.html</br>

关于HashMap的resize(rehash),性能参数和 Fail-Fast机制（因线程不安全提出的）可参考链接http://zhangshixi.iteye.com/blog/672697</br>
本文HashMap大部分内容也是参考的上文链接。</br>

关于HashMap的面试点：http://www.admin10000.com/document/3322.html</br>




<h2>HashTable 实现原理</h2>
简介</br>
和HashMap一样，Hashtable 也是一个散列表，它存储的内容是键值对(key-value)映射。</br>
Hashtable 继承于Dictionary，实现了Map、Cloneable、java.io.Serializable接口。</br>
Hashtable 的函数都是同步的，这意味着它是线程安全的。它的key、value都不可以为null。</br>
此外，Hashtable中的映射不是有序的。</br>

  
  
 HashTable源码分析参照以下链接（主要突出它是线程安全的）</br>
 http://www.cnblogs.com/skywang12345/p/3310887.html</br>
 
 
额外说一下HashTable 的key、value都不可以为null，这和HashMap不同，HashMap更注重轻量级的实现，既然是轻量的实现，那么不判断为空就更加提高了效率，而且如果你需要判断的话，有hashtable，这里再实现功能就重复了。



<h2>HashMap和HashTable的区别</h2>
（1）HashTable的方法前面都有synchronized来同步，是线程安全的；HashMap未经同步，是非线程安全的。</br>
（2）HashTable不允许null值(key和value都不可以) ；HashMap允许null值(key和value都可以)。</br>
（3）HashTable有一个contains(Object value)功能和containsValue(Object value)功能一样。</br>
（4）HashTable使用Enumeration进行遍历；HashMap使用Iterator进行遍历。</br>
（5）HashTable中hash数组默认大小是11，增加的方式是old*2+1；HashMap中hash数组的默认大小是16，而且一定是2的指数。</br>
（6）哈希值的使用不同，HashTable直接使用对象的hashCode；HashMap重新计算hash值，而且用与代替求模。


  [1]: http://www.admin10000.com/UploadFiles/Document/201311/16/20131116091617994485.JPG
  [2]: http://images.cnitblog.com/blog/497634/201401/280030194375782.jpg
