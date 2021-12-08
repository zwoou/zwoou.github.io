# HashMap

[TOC]



## 继承和实现

```
HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable
    
```

## 默认构造方法

  一个空的hashmap和默认16容量 负载因子0.75

```java
/**
 * Constructs an empty {@code HashMap} with the default initial capacity
 * (16) and the default load factor (0.75).
 */
public HashMap() {
    this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
}
```

## 二. 重要常量

```
/**
 * 最大容量
 * 必须是2的倍数
 */
static final int MAXIMUM_CAPACITY = 1 << 30;

/**
 * 在构造函数中未指定时使用的 负载因子  
 */
static final float DEFAULT_LOAD_FACTOR = 0.75f;

/**
  *  链表结构转化为红黑树的阈值，大于8时链表结构转化为红黑树
 */
static final int TREEIFY_THRESHOLD = 8;

/**
 * 红黑树转化为链表结构的阈值，桶(bucket)内元素个数小于6时转化为链表结构
 */
static final int UNTREEIFY_THRESHOLD = 6;

/**
 * 树形化最小hash表 元素个数，如果桶内元素已经达到转化红黑树阈值，但是表元素总数未达到阈值，则值进行扩容resize()，不进行树形化
 */
static final int MIN_TREEIFY_CAPACITY = 64;
```

## 三. Node

```
/**
* 该类只实现了 Map.Entry 接口，
* 所以该类只需要实现getKey、getValue、setValue三个方法即可
* 除此之外以什么样的方式来组织数据，就和接口无关了
*/
static class Node<K,V> implements Map.Entry<K,V> {
    final int hash;     // hash值，不可变
    final K key;        // 键，不可变
    V value;     // 值
    Node<K,V> next;   // 下一个节点，表明键值映射记录的存储方式是链表

    Node(int hash, K key, V value, Node<K,V> next) {
        this.hash = hash;
        this.key = key;
        this.value = value;
        this.next = next;
    }
    // 实现接口定义的方法，且该方法不可被重写
    public final K getKey()        { return key; }
   //实现接口定义的方法，且该方法不能被重写
    public final V getValue()      { return value; }
    public final String toString() { return key + "=" + value; }

    // 重写父类Object的hashCode方法，且该方法不可被自己的子类再重写
    // 返回：key的hashCode值和value的hashCode值进行异或运算结果
    public final int hashCode() {
        return Objects.hashCode(key) ^ Objects.hashCode(value);
    }
    //实现接口定义的方法，且该方法不可被重写
    // 设值，返回旧值
    public final V setValue(V newValue) {
        V oldValue = value;
        value = newValue;
        return oldValue;
    }
    
    // 重写父类Object的equals方法，且该方法不可被自己的子类再重写
    // 判断相等的依据是，只要是Map.Entry的一个实例，并且键键、值值都相等就返回True
    public final boolean equals(Object o) {
        if (o == this)
            return true;
        if (o instanceof Map.Entry) {
            Map.Entry<?,?> e = (Map.Entry<?,?>)o;
            if (Objects.equals(key, e.getKey()) &&
                Objects.equals(value, e.getValue()))
                return true;
        }
        return false;
    }
}
```

## 四. hash的实现

String的hashcode实现  

```java
/**
 * Returns a hash code for this string. The hash code for a
 * {@code String} object is computed as
 * <blockquote><pre>
 * s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]
 * </pre></blockquote>
 * using {@code int} arithmetic, where {@code s[i]} is the
 * <i>i</i>th character of the string, {@code n} is the length of
 * the string, and {@code ^} indicates exponentiation.
 * (The hash value of the empty string is zero.)
 *
 * @return  a hash code value for this object.
 */
public int hashCode() {
    int h = hash;
    if (h == 0 && value.length > 0) {
        hash = h = isLatin1() ? StringLatin1.hashCode(value)
                              : StringUTF16.hashCode(value);
    }
    return h;
}
```





```
// hash实现，没有直接使用key的hashcode()，而是使key的hashcode()高16位不变，低16位与高16位异或作为最终hash值。
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

Java8中hash计算是通过key的hashCode()的高16位异或低16位实现的，既保证高低bit都能参与到hash的计算中，又不会有太大的开销。

这是源码中的注释：
Computes key.hashCode() and spreads (XORs) higher bits of hash to lower. Because the table uses power-of-two masking, sets of hashes that vary only in bits above the current mask will always collide. (Among known examples are sets of Float keys holding consecutive whole numbers in small tables.) So we apply a transform that spreads the impact of higher bits downward. There is a tradeoff between speed, utility, and quality of bit-spreading. Because many common sets of hashes are already reasonably distributed (so don’t benefit from spreading), and because we use trees to handle large sets of collisions in bins, we just XOR some shifted bits in the cheapest possible way to reduce systematic lossage, as well as to incorporate impact of the highest bits that would otherwise never be used in index calculations because of table bounds.

**大概的意思是：**
　　如果直接使用key的hashcode()作为hash很容易发生碰撞。比如，在n - 1为15(0x1111)时，散列值真正生效的只是低4位。当新增的键的hashcode()是2，18，34这样恰好以16的倍数为差的等差数列，就产生了大量碰撞。
　　因此，设计者综合考虑了速度、作用、质量，把高16bit和低16bit进行了异或。因为现在大多数的hashCode的分布已经很不错了，就算是发生了较多碰撞也用O(logn)的红黑树去优化了。仅仅异或一下，既减少了系统的开销，也不会造成因为高位没有参与下标的计算(table长度比较小时)，从而引起的碰撞。

## 五. hashMap中一个精巧的算法

```
/**
 * Returns a power of two size for the given target capacity.
*  精巧算法    返回最近的不小于输入参数的2的整数次幂。比如10，则返回16。
 */
static final int tableSizeFor(int cap) {
    int n = cap - 1;
    n |= n >>> 1;
    n |= n >>> 2;
    n |= n >>> 4;
    n |= n >>> 8;
    n |= n >>> 16;
    return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
}
```

**原理如下： **
先说5个移位操作，会使cap的二进制从最高位的1到末尾全部置为1。

假设cap的二进制为01xx…xx。
对cap右移1位：01xx…xx，位或：011xx…xx，使得与最高位的1紧邻的右边一位为1，
对cap右移2位：00011x..xx，位或：01111x..xx，使得从最高位的1开始的四位也为1，
以此类推，int为32位，所以在右移16位后异或最多得到32个连续的1，保证从最高位的1到末尾全部为1。

最后让结果+1，就得到了最近的大于cap的2的整数次幂。

再看第一条语句：

int n = cap - 1;

让cap-1再赋值给n的目的是令找到的目标值大于或等于原值。如果cap本身是2的幂，如8（1000(2)），不对它减1而直接操作，将得到16。
通过tableSizeFor()，保证了HashMap容量始终是2的次方，在通过hash寻找index时就可以用逻辑运算来替代取余，即hash％n用hash&(n -1)替代。

## 六. 一些变量的介绍

```
// 存储元素的数组，总是2的幂，可以为0 
transient Node<K,V>[] table;

// 存放具体元素的集合
transient Set<Map.Entry<K,V>> entrySet;

// 存放元素的个数，注意这个不等于数组的长度。
transient int size;

// 每次扩容和更改map结构的计数器
transient int modCount;

// 临界值 当实际节点个数超过临界值(容量*填充因子)时，会进行扩容
int threshold;

// 哈希表的负载因子
final float loadFactor
```

## 七. 4个构造函数

```
//制定初始容量和填充因子
public HashMap(int initialCapacity, float loadFactor) {
    if (initialCapacity < 0)    // 初始容量不能小于0，否则报错
        throw new IllegalArgumentException("Illegal initial capacity: " +
                                           initialCapacity);
    if (initialCapacity > MAXIMUM_CAPACITY)  // 初始容量不能大于最大值，否则为最大值
        initialCapacity = MAXIMUM_CAPACITY;
    if (loadFactor <= 0 || Float.isNaN(loadFactor))  // 填充因子不能小于或等于0，不能为非数字
        throw new IllegalArgumentException("Illegal load factor: " +
                                           loadFactor);
    this.loadFactor = loadFactor;   // 初始化填充因子
    this.threshold = tableSizeFor(initialCapacity);  // 通过tableSizeFor(cap)计算出不小于initialCapacity的最近的2的幂作为初始容量，将其先保存在threshold里，当put时判断数组为空会调用resize分配内存，并重新计算正确的threshold
}

//指定初始容量
public HashMap(int initialCapacity) {
    this(initialCapacity, DEFAULT_LOAD_FACTOR);
}

//默认构造函数
public HashMap() {
     // 初始化填充因子
    this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
}

// HashMap(Map<? extends K>)型构造函数
public HashMap(Map<? extends K, ? extends V> m) {
     // 初始化填充因子
    this.loadFactor = DEFAULT_LOAD_FACTOR;
    // 将m中的所有元素添加至HashMap中
    putMapEntries(m, false);
}
```

## 八. 构造函数中的putMapEntries方法

```
//  将m中的所有元素添加至本HashMap中
final void putMapEntries(Map<? extends K, ? extends V> m, boolean evict) {
    int s = m.size();
    if (s > 0) {
        if (table == null) { //  判断table是否已经初始化
            float ft = ((float)s / loadFactor) + 1.0F;  // 未初始化，s为m的实际元素个数
            int t = ((ft < (float)MAXIMUM_CAPACITY) ?
                     (int)ft : MAXIMUM_CAPACITY);
    // 计算得到的t大于阈值，则初始化阈值
            if (t > threshold)
                threshold = tableSizeFor(t);
        }
        else if (s > threshold)   // 已初始化，并且m元素个数大于阈值，进行扩容处理
            resize();
// 将m中的所有元素添加至HashMap中
        for (Map.Entry<? extends K, ? extends V> e : m.entrySet()) {
            K key = e.getKey();
            V value = e.getValue();
            putVal(hash(key), key, value, false, evict);
        }
    }
}

// 集合大小
public int size() {
    return size;
}

// 本HashMap大小 是否 为0    true表示本HashMap没有存储任何键值对
public boolean isEmpty() {
    return size == 0;
}
```

## 九. 扩容

resize用来重新分配内存

- 当数组未初始化，按照之前在threashold中保存的初始容量分配内存，没有就使用缺省值
- 当超过限制时，就扩充两倍，因为我们使用的是2次幂的扩展，所以，元素的位置要么是在原位置，要么是在原位置再移动2次幂的位置

```
final Node<K,V>[] resize() {
    Node<K,V>[] oldTab = table;   // 当前table保存
    int oldCap = (oldTab == null) ? 0 : oldTab.length;    / /保存table大小
    int oldThr = threshold;    // 保存当前阈值
    int newCap, newThr = 0;
    if (oldCap > 0) {   // 之前table大小大于0，即已初始化
        if (oldCap >= MAXIMUM_CAPACITY) {   // 超过最大值就不再扩充了，只设置阈值
            threshold = Integer.MAX_VALUE;    // 阈值为最大整形
            return oldTab;
        }
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&     // 容量翻倍
                 oldCap >= DEFAULT_INITIAL_CAPACITY)
            newThr = oldThr << 1; // double threshold    阈值翻倍
    }
    else if (oldThr > 0) // 初始容量已存在threshold中
        newCap = oldThr;
    else {               //  使用缺省值（使用默认构造函数初始化）
        newCap = DEFAULT_INITIAL_CAPACITY;
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    if (newThr == 0) {   // 计算新阈值
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                  (int)ft : Integer.MAX_VALUE);
    }
    threshold = newThr;
    @SuppressWarnings({"rawtypes","unchecked"})
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];    // 初始化table
    table = newTab;
    if (oldTab != null) {   // 之前的table已经初始化过
        for (int j = 0; j < oldCap; ++j) {     // 复制元素，重新进行hash
            Node<K,V> e;
            if ((e = oldTab[j]) != null) {
                oldTab[j] = null;  // 将原数组位置置为空，应该是便于垃圾回收吧
                if (e.next == null)    // 桶中只有一个结点
                    newTab[e.hash & (newCap - 1)] = e;
                else if (e instanceof TreeNode)  //红黑树
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);  //根据(e.hash & oldCap)分为两个，如果哪个数目不大于UNTREEIFY_THRESHOLD，就转为链表
                else { // preserve order
                    Node<K,V> loHead = null, loTail = null;
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    do {   // 将同一桶中的元素根据(e.hash & oldCap)是否为0进行分割成两个不同的链表，完成rehash
                        next = e.next;    //保存下一个节点
                        if ((e.hash & oldCap) == 0) {      //保留在低部分即原索引
                            if (loTail == null)     //第一个结点让loTail和loHead都指向它
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        }
                        else {    //hash到高部分即原索引+oldCap
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```

进行扩容，会重新进行内存分配，并且会遍历hash表中所有的元素，是非常耗时的。在编写程序中，要尽量避免resize。
分配内存统一放在resize()中，包括创建后首次put时初始化数组和存放元素个数超过阈值时扩容。
如果映射很多，创建HashMap时设置充足的初始容量(预计大小/负载因子 + 1）会比让其自动扩容获得更好的效率，一方面减少了碰撞可能，另一方面减少了resize的损耗。
总结一下：把原数组在内存中的位置置为空，然后重新分配内存，把原来的一张表分为两张表

## 十.put函数大致的思路

1.对key的hashCode()做hash，然后再计算桶的index;
2.如果没碰撞直接放到桶bucket里；
3.如果碰撞了，以链表的形式存在buckets后；
4.如果碰撞导致链表过长(大于等于TREEIFY_THRESHOLD)，就把链表转换成红黑树（若数组容量小于MIN_TREEIFY_CAPACITY，不进行转换而是进行resize操作）
5.如果节点已经存在就替换old value(保证key的唯一性)
6.如果表中实际元素个数超过阈值(超过load factor*current capacity)，就要resize

```
public V put(K key, V value) {
// 对key的hashCode()做hash
    return putVal(hash(key), key, value, false, true);
}

/**
 * Implements Map.put and related methods
 * 用于实现put()方法和其他相关的方法
 * @param hash hash for key
 * @param key the key
 * @param value the value to put
 * @param onlyIfAbsent if true, don't change existing value
 * @param evict if false, the table is in creation mode.
 * @return previous value, or null if none
 */
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)   // table未初始化或者长度为0，进行扩容，n为桶的个数
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)  // (n - 1) & hash 确定元素存放在哪个桶中，桶为空，新生成结点放入桶中(此时，这个结点是放在数组中)
        tab[i] = newNode(hash, key, value, null);
    else {   // 桶中已经存在元素
        Node<K,V> e; K k;   // 比较桶中第一个元素的hash值相等，key相等
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;    // 将第一个元素赋值给e，用e来记录
        else if (p instanceof TreeNode)   // hash值不相等或key不相等     红黑树
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);  // 放入树中
        else {    // 为链表结点
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {     // 到达链表的尾部
                    p.next = newNode(hash, key, value, null);   // 在尾部插入新结点
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st      结点数量达到阈值，调用treeifyBin()做进一步判断是否转为红黑树
                        treeifyBin(tab, hash);
                    break;  // 跳出循环
                }
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))   // 判断链表中结点的key值与插入的元素的key值是否相等
                    break;  // 相等 跳出循环
                p = e;   // 用于遍历桶中的链表，与前面的e = p.next组合，可以遍历链表
            }
        }
        if (e != null) { // existing mapping for key   表示在桶中找到key值、hash值与插入元素相等的结点
            V oldValue = e.value;   // 记录e的value
            if (!onlyIfAbsent || oldValue == null)    // onlyIfAbsent为false或者旧值为null
                e.value = value;    //用新值替换旧值 
            afterNodeAccess(e);    // 访问后回调
            return oldValue;   // 返回旧值
        }
    }
    ++modCount;    // 结构性修改
    if (++size > threshold)   // 实际大小大于阈值则扩容
        resize();
    afterNodeInsertion(evict);   // 插入后回调
    return null; 
}

/**
 * Replaces all linked nodes in bin at index for given hash unless
 * table is too small, in which case resizes instead.
    将链表转换为红黑树
 */
final void treeifyBin(Node<K,V>[] tab, int hash) {
    int n, index; Node<K,V> e;
    if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)  //若数组容量小于MIN_TREEIFY_CAPACITY，不进行转换而是进行resize操作
        resize();
    else if ((e = tab[index = (n - 1) & hash]) != null) {
        TreeNode<K,V> hd = null, tl = null;
        do {
            TreeNode<K,V> p = replacementTreeNode(e, null);   // 将Node转换为TreeNode
            if (tl == null)
                hd = p;
            else {
                p.prev = tl;
                tl.next = p;
            }
            tl = p;
        } while ((e = e.next) != null);
        if ((tab[index] = hd) != null)
            hd.treeify(tab);    // 重新排序形成红黑树
    }
}

/**
 * 将指定映射的所有映射关系复制到此映射中
 *
 * @param m mappings to be stored in this map
 * @throws NullPointerException if the specified map is null
 */
public void putAll(Map<? extends K, ? extends V> m) {
    putMapEntries(m, true);
}


```

## 十一.get函数大致思路

1. 链表结构（bucket）里的第一个节点，直接命中；
2. 如果有冲突，则通过key.equals(k)去查找对应的entry，若为树，复杂度O(logn)， 若为链表，O(n)

```
public V get(Object key) {
    Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}

final Node<K,V> getNode(int hash, Object key) {
    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
    // table已经初始化，长度大于0，且根据hash寻找table中的项也不为空
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (first = tab[(n - 1) & hash]) != null) {
        if (first.hash == hash && // always check first node  比较桶中第一个节点
            ((k = first.key) == key || (key != null && key.equals(k))))
            return first;
        if ((e = first.next) != null) { // 桶中不止一个节点
            if (first instanceof TreeNode)  // 为红黑树结点
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);   // 在红黑树中查找
            do {  // 否则，在链表中查找
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
}

// 本HashMap 是否包含这个key
public boolean containsKey(Object key) {
    return getNode(hash(key), key) != null;
}

//  本HashMap 是否包含这个值
public boolean containsValue(Object value) {
    Node<K,V>[] tab; V v;
    if ((tab = table) != null && size > 0) {
// 外层循环搜索数组
        for (int i = 0; i < tab.length; ++i) {
     // 内层循环搜索链表
            for (Node<K,V> e = tab[i]; e != null; e = e.next) {
                if ((v = e.value) == value ||  (value != null && value.equals(v)))   //  如果value地址相同，或者本身的值相同，然后equals方法
                    return true;
            }
        }
    }
    return false;
}
```

## 十二.keySet() 、entrySet()、values()等方法

```
// keySet成员初始为null，且并没有在构造函数中初始化过
transient volatile Set<K> keySet = null;
// 返回一个内部引用，并指向一个内部类对象，该内部类重写了迭代器方法，当在增强for循环时才调用，并从外部类的table中取值
public Set<K> keySet() {
    Set<K> ks = keySet;
    if (ks == null) {
        ks = new KeySet();
        keySet = ks;
    }
    return ks;
}

所以初次调用keySet()方法时会new KeySet()，而KeySet()是一个内部类
final class KeySet extends AbstractSet<K> {
    public final int size()                 { return size; }
    public final void clear()               { HashMap.this.clear(); }
    public final Iterator<K> iterator()     { 
return new KeyIterator();   // 指向创建的另一个内部类对象KeyIterator()，该类继承HashIterator也是HashMap的内部类
    }
    public final boolean contains(Object o) { return containsKey(o); }
    public final boolean remove(Object key) {
        return removeNode(hash(key), key, null, false, true) != null;
    }
    public final Spliterator<K> spliterator() {
        return new KeySpliterator<>(HashMap.this, 0, -1, 0, 0);
    }
    public final void forEach(Consumer<? super K> action) {
        Node<K,V>[] tab;
        if (action == null)
            throw new NullPointerException();
        if (size > 0 && (tab = table) != null) {
            int mc = modCount;
            for (int i = 0; i < tab.length; ++i) {
                for (Node<K,V> e = tab[i]; e != null; e = e.next)
                    action.accept(e.key);
            }
            if (modCount != mc)
                throw new ConcurrentModificationException();
        }
    }
}

// 供KeySet内部类调用的
final class KeyIterator extends HashIterator
    implements Iterator<K> {
    public final K next() { return nextNode().key; }
}

示例：
Map<String,Object> maps = new HashMap<>();
Set<String> strings = maps.keySet();

for (String string: strings) {
    System.out.println(string);
}
```

当我们在增强for循环时会调用该next()方法，它指向的是nextEntry().getKey()，Entry中不仅存放了key，value，也存放了next，指向下一个Entry对象，我们知道，HashMap的数据层实现是数组+链表，nextEntry会先遍历链表，然后再继续遍历下一个数组位置的链表，直至全部遍历完成。

```
// 获取所有value的集合
public Collection<V> values() {
    Collection<V> vs = values;
    if (vs == null) {
        vs = new Values();
        values = vs;
    }
    return vs;
}

final class Values extends AbstractCollection<V> {
    public final int size()                 { return size; }
    public final void clear()               { HashMap.this.clear(); }
    public final Iterator<V> iterator()     { return new ValueIterator(); }
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

这个和上面的keySet是类似的，只是多做了一步取值hash碰撞后用equals比较获取到对应的值value

为什么要用Collection不和keySet一样使用Set是因为key是不重复的，value是可以重复的.

同时获取键和值得方法
entrySet()方法来实现对Map.Entry接口对象实例的遍历，Map.Entry是Map接口里面的一个内部接口，该接口声明为泛型。当我们获得了接口对象后就可以调用接口方法

getKey(), getValue()

这个和上面的keySet是类似的，只是多做了一步取值hash碰撞后用equals比较获取到对应的值value

为什么要用Collection不和keySet一样使用Set是因为key是不重复的，value是可以重复的.

同时获取键和值得方法
entrySet()方法来实现对Map.Entry接口对象实例的遍历，Map.Entry是Map接口里面的一个内部接口，该接口声明为泛型。当我们获得了接口对象后就可以调用接口方法

getKey(), getValue()

```
public Set<Map.Entry<K,V>> entrySet() {
    Set<Map.Entry<K,V>> es;
    return (es = entrySet) == null ? (entrySet = new EntrySet()) : es;
}

final class EntrySet extends AbstractSet<Map.Entry<K,V>> {
    public final int size()                 { return size; }
    public final void clear()               { HashMap.this.clear(); }
    public final Iterator<Map.Entry<K,V>> iterator() {
        return new EntryIterator();
    }
    public final boolean contains(Object o) {
        if (!(o instanceof Map.Entry))
            return false;
        Map.Entry<?,?> e = (Map.Entry<?,?>) o;
        Object key = e.getKey();
        Node<K,V> candidate = getNode(hash(key), key);
        return candidate != null && candidate.equals(e);
    }
    public final boolean remove(Object o) {
        if (o instanceof Map.Entry) {
            Map.Entry<?,?> e = (Map.Entry<?,?>) o;
            Object key = e.getKey();
            Object value = e.getValue();
            return removeNode(hash(key), key, value, true, true) != null;
        }
        return false;
    }
    public final Spliterator<Map.Entry<K,V>> spliterator() {
        return new EntrySpliterator<>(HashMap.this, 0, -1, 0, 0);
    }
    /**
*在进行foreach遍历EntrySet的时候实际上是会遍历table[],hashmap中实际具体数据都是存储在这个数组中，包括entry。这也进一步验证了那句话entrySet()
*该方法返回的是map包含的映射集合视图，视图的概念相当于数据库中视图及提供一个窗口，没有具体到相关数据，而真正获取数据还是从table[]中来
*/
    public final void forEach(Consumer<? super Map.Entry<K,V>> action) {
        Node<K,V>[] tab;
        if (action == null)
            throw new NullPointerException();
        if (size > 0 && (tab = table) != null) {
            int mc = modCount;
            for (int i = 0; i < tab.length; ++i) {
                for (Node<K,V> e = tab[i]; e != null; e = e.next)
                    action.accept(e);
            }
            if (modCount != mc)
                throw new ConcurrentModificationException();
        }
    }
}
```

## 十三.删除的方法

```
public V remove(Object key) {
    Node<K,V> e;
    return (e = removeNode(hash(key), key, null, false, true)) == null ?
        null : e.value;
}

/**
 * Implements Map.remove and related methods
 *  用于实现 remove()方法和其他相关的方法
 * @param hash 键的hash值
 * @param key 键
 * @param value the value to match if matchValue, else ignored
 * @param matchValue if true only remove if value is equal
 * @param movable if false do not move other nodes while removing
 * @return the node, or null if none
 */
final Node<K,V> removeNode(int hash, Object key, Object value,
                           boolean matchValue, boolean movable) {
    Node<K,V>[] tab; Node<K,V> p; int n, index;
    if ((tab = table) != null && (n = tab.length) > 0 &&   // table数组非空，键的hash值所指向的数组中的元素非空
        (p = tab[index = (n - 1) & hash]) != null) {
        Node<K,V> node = null, e; K k; V v;     // node指向最终的结果结点，e为链表中的遍历指针

        if (p.hash == hash &&    // 检查第一个节点
            ((k = p.key) == key || (key != null && key.equals(k))))
            node = p;
        else if ((e = p.next) != null) {  //如果第一个节点不匹配
            if (p instanceof TreeNode)  //树
                node = ((TreeNode<K,V>)p).getTreeNode(hash, key);
            else {
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key ||
                         (key != null && key.equals(k)))) {
                        node = e;
                        break;
                    }
                    p = e;  //保存上个节点
                } while ((e = e.next) != null);
            }
        }
        if (node != null && (!matchValue || (v = node.value) == value ||         //判断是否存在，如果matchValue为true，需要比较值是否相等
                             (value != null && value.equals(v)))) {
            if (node instanceof TreeNode)   //树
                ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
            else if (node == p)   //匹配第一个节点
                tab[index] = node.next;
            else
                p.next = node.next;
            ++modCount;
            --size;
            afterNodeRemoval(node);
            return node;
        }
    }
    return null;
}

// 清空整个hashmap
public void clear() {
    Node<K,V>[] tab;
    modCount++;
    if ((tab = table) != null && size > 0) {
        size = 0;
        for (int i = 0; i < tab.length; ++i)
            tab[i] = null;
    }
}
```