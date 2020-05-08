- [Map](#Map)
    - [HashMap](#HashMap)
        - [HashMap的数据结构](#HashMap的数据结构)
        - [HashMap中的成员属性](#HashMap中的成员属性)
        - [HashMap四种构造方法](#HashMap四种构造方法)
        - [HashMap中常数级时间复杂度操作](#HashMap中常数级时间复杂度操作)
        - [HashMap确定哈希桶索引位置](#HashMap确定哈希桶索引位置)
        - [HashMap扩容机制](#HashMap扩容机制)
        - [HashMap的put方法](#HashMap的put方法)
        - [HashMap的get方法](#HashMap的get方法)
        - [HashMap的remove方法](#HashMap的remove方法)
    - [ConcurrentHashMap采取了哪些方法来提高并发表现](#ConcurrentHashMap采取了哪些方法来提高并发表现)
    - [Hashtable](#Hashtable)
    - [LinkedHashMap](#LinkedHashMap)
    - [TreeMap](#TreeMap)

# Map
## HashMap
- 包：java.util;
- HashMap允许key和value为null
- 不保证映射的顺序一直不变
- 像get、put的时间复杂度是不变的
- 如果迭代性能要求比较高，不要把初始容量设置过高，或把负载因子设置过低
- 初始容量和负载因子直接影响其性能
- 容量（capacity）：是桶的个数，即数组的大小
- 负载因子（load factor）：是HashTable中Node个数达到多满时自动扩容，当节点个数>负载因子*桶的个数时扩容
- 负载因子设置为0.75可以达到时间和空间的权衡，如果该值设置过大，在节省空间的同时也加大了查找时间
- 在使用HashMap时，应预估容量，以减少扩容次数，即key个数大于16*0.75=12时需要设置初始容量
- 当有多个key的hashCode相等时，会影响性能，如果key是可比较的，那就实现Comparable方法来处理该问题
- HashMap非线程安全。如果需要满足线程安全，可以用`Map m = Collections.synchronizedMap(new HashMap(...));`方法使HashMap具有线程安全的能力，或者使用ConcurrentHashMap
- key为数组，value为链表，当链表长度超过8时转为红黑树，以提高搜索速度
- 它根据键的hashCode值存储数据，大多数情况下可以直接定位到它的值，因而具有很快的访问速度，但遍历顺序却是不确定的
- 红黑树占用的空间是链表的二倍，当链表长度超过8时才进行转换，当长度缩短时，也会由红黑树转换为链表

参考文章：
[Java 8系列之重新认识HashMap](https://zhuanlan.zhihu.com/p/21673805)

### HashMap的数据结构
- HashMap是由内部类Node<K,V>构成的数组，索引是key的hash值对数组长度取余，值是有Node<K,V>构成的单项链表；
- 当链表元素个数`大于`8且总元素个数超过`大于`64时，链表结构转为红黑树；
- 当链表元素个数`小于等于`6时，红黑树结构将转为链表

transient Node<K,V>[] table;

### HashMap中的成员属性
```java
public class HashMap<K, V> extends AbstractMap<K, V> implements Map<K, V>, Cloneable, Serializable {
    // 默认的初始容量，必须是2的幂
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16

    // 最大容量（必须是2的幂且小于2的30次方，传入容量过大将被这个值替换）
    static final int MAXIMUM_CAPACITY = 1 << 30;

    // 默认装载因子，默认值为0.75，如果实际元素所占容量占分配容量的75%时就要扩容了
    // 如果填充比很大，说明利用的空间很多，但是查找的效率很低，因为链表的长度很大（当然最新版本使用了红黑树后会改进很多），HashMap本来是以空间换时间，所以填充比没必要太大
    // 但是填充比太小，又会导致空间浪费
    // 如果关注内存，填充比可以稍大，如果主要关注查找性能，填充比可以稍小
    static final float DEFAULT_LOAD_FACTOR = 0.75f;

    // 一个桶的树化阈值
    // 当桶中元素个数超过这个值时，需要使用红黑树节点替换链表节点
    static final int TREEIFY_THRESHOLD = 8;

    // 一个树的链表还原阈值
    // 当缩容时，桶中元素个数小于这个值，就会把树形的桶元素还原为链表结构
    // 这个值应小于TREEIFY_THRESHOLD，最大为6
    static final int UNTREEIFY_THRESHOLD = 6;

    // 当哈希表中元素个数大于该值，且桶的长度大于8时才会转红黑树，否则为扩容
    static final int MIN_TREEIFY_CAPACITY = 64;

    // 存储数据的Node数组，长度是2的幂。
    transient Node<K,V>[] table;
    
    transient Set<Map.Entry<K,V>> entrySet;
    
    // map中保存的键值对的数量
    transient int size;
    
    // 扩容阈值，容量*负载因子
    // 如果尚未分配数组，则保留数组的初始容量
    // 或为0，表示默认的初始容量
    int threshold;
    
    // 负载因子
    final float loadFactor;
    
    // map结构被改变的次数
    transient volatile int modCount;
}
```

### HashMap四种构造方法
```java
public class HashMap<K, V> extends AbstractMap<K, V> implements Map<K, V>, Cloneable, Serializable {
    // 第一种构造方法：使用默认数组容量16和默认负载因子0.75，构造空的哈希表
    public HashMap() {
        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
    }
    
    // 第二种构造方法：指定初始容量和负载因子
    // 如果初始容量<0，或负载因子<=0.0f或为非数值，则抛出异常：IllegalArgumentException
    public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " + initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " + loadFactor);
        this.loadFactor = loadFactor;
        this.threshold = tableSizeFor(initialCapacity);
    }
    
    // 第三种构造方法：指定初始容量，负载因为使用默认0.75
    public HashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }
    
    // 第四种构造方法：使用指定的哈希表数据填充，负载因子使用默认0.75
    public HashMap(Map<? extends K, ? extends V> m) {
        this.loadFactor = DEFAULT_LOAD_FACTOR;
        putMapEntries(m, false);
    }
    // 第二个参数：当初始构造哈希表时传入false，puAll时传入true
    final void putMapEntries(Map<? extends K, ? extends V> m, boolean evict) {
        int s = m.size();
        if (s > 0) {
            // 如果当前表为空
            if (table == null) { // pre-size
                // 根据m的元素数量和负载因子，计算容量，+1.0F是为了上取整
                float ft = ((float)s / loadFactor) + 1.0F;
                // 矫正容量并转为整数
                int t = ((ft < (float)MAXIMUM_CAPACITY) ? (int)ft : MAXIMUM_CAPACITY);
                // 如果原容量大于当前阈值则扩容阈值为2的次方
                if (t > threshold)
                    threshold = tableSizeFor(t);
            // 如果当前表不为空，元素个数大于当前阈值，则扩容
            } else if (s > threshold) {
                resize();
            }
            for (Map.Entry<? extends K, ? extends V> e : m.entrySet()) {
                K key = e.getKey();
                V value = e.getValue();
                putVal(hash(key), key, value, false, evict);
            }
        }
    }
    // 当给定容量>=阈值时扩容 容量为2的次方
    static final int tableSizeFor(int cap) {
        int n = cap - 1;
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
    // 把m中的K-V批量添加到当前map中
    public void putAll(Map<? extends K, ? extends V> m) {
        putMapEntries(m, true);
    }
}
```

### HashMap中常数级时间复杂度操作
```java
public class HashMap<K, V> extends AbstractMap<K, V> implements Map<K, V>, Cloneable, Serializable {
    public int size() {
        return size;
    }

    public boolean isEmpty() {
        return size == 0;
    }
}
```

### HashMap确定哈希桶索引位置
这样key的hashCode值的所有位都参与hash值的计算，任意一位改动hash值就会变化，可以减少哈希碰撞
```java
public class HashMap<K, V> extends AbstractMap<K, V> implements Map<K, V>, Cloneable, Serializable {
    // 扰动函数，用来减少hash碰撞
    // 取key的hashCode值、高位运算、高低位异或运算
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
}
```

### HashMap扩容机制
1. 创建一个新Node数组，容量是原来的2倍
2. 将Node重新索引，所在位置有两种，一种是原来索引位置，一种是原来索引位置+原来容量大小
3. 如果桶位红黑树结构，且长度小于等于6，则转化位单链表结构
```java
public class HashMap<K, V> extends AbstractMap<K, V> implements Map<K, V>, Cloneable, Serializable {
    // 初始化或扩容2倍
    final Node<K,V>[] resize() {
        // 当前哈希表
        Node<K,V>[] oldTab = table;
        // 当前哈希表的容量
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        // 当前哈希表的阈值
        int oldThr = threshold;
        // 新哈希表的容量和阈值
        int newCap, newThr = 0;
        // 如果当前哈希表已有数据
        if (oldCap > 0) {
            // 如果当前哈希表容量>=最大容量，阈值改为整型的最大值(2^31-1)，不进行扩容
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            // 如果当前哈希表容量未超最大值
            // 新容量=原容量*2，且不超过最大容量 并且 原容量>=默认初始容量16
            // 则新阈值=原阈值*2
            } else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY && oldCap >= DEFAULT_INITIAL_CAPACITY) {
                newThr = oldThr << 1; // double threshold
            }
        // 如果当前表是空的，但是有阈值。代表的是初始化时指定了容量为0的情况，此时阈值为1
        // 如果当前阈值大于0则新容量=当前阈值
        } else if (oldThr > 0) { // initial capacity was placed in threshold
            newCap = oldThr;
        // 初始分配容量为16，阈值为0.75*16=12
        } else { // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        // 如果新的阈值是0，对应的是：当前表是空的，当前阈值为1的情况
        if (newThr == 0) {
            float ft = (float)newCap * loadFactor;
            // 矫正新阈值
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ? (int)ft : Integer.MAX_VALUE);
        }
        // 更新阈值
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
        if (oldTab != null) {
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
                    // 当前元素没有发生哈希碰撞 直接放入指定位置
                    if (e.next == null)
                        // 注意这里取下标 是用哈希值与桶的长度-1，由于桶的长度是2的n次方，这么做其实是等于一个模运算。但是效率更高
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            next = e.next;
                            // 利用位运算代替常规运算：利用哈希值按位与oldCap
                            // 可以得到哈希值对newCap取模后，是>=oldCap还是<oldCap
                            // 如果大于0，代表>=oldCap，应该放在高位；如果等于0，代表小于oldCap，应该存放在低位
                            // 利用为插入法，保证链表的顺序
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            } else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                        } while ((e = next) != null);
                        if (loTail != null) {
                            loTail.next = null;
                            // 将低位链表放在原index处
                            newTab[j] = loHead;
                        }
                        if (hiTail != null) {
                            hiTail.next = null;
                            // 将高位链表存放在新index(原index+oldCap)处
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }
}
```

### HashMap的put方法
1. 计算key的hash值，通过位运算找到key所在桶在数组中的索引下标
2. 判断桶是否为null，是则添加新Node
3. 否则，判断桶的第一个Node是否为查找key，是则覆盖
4. 否则，判断第一个Node是否为红黑树，是则按照红黑树查找key所在位置
5. 否则，查找单链表表中是否已经存在key，有则覆盖，无则添加；如果链表长度超过8且元素个数大于64，则将链表转化位红黑树
6. 如果元素个数超过阈值，则进行扩容；扩容时如果红黑树长度小于等于6，则转化位链表

时间复杂度：O(logK) ~ O(K)，K是链表或红黑树的平均长度，logK是以2为底K的对数

```java
public class HashMap<K, V> extends AbstractMap<K, V> implements Map<K, V>, Cloneable, Serializable {
    public V put(K key, V value) {
        // 第四个参数：对已存在的key的值，true-不覆盖，false-覆盖
        // 第五个参数：true-保留，false-移除(创建模式)
        return putVal(hash(key), key, value, false, true);
    }
    
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
       Node<K,V>[] tab; 
       Node<K,V> p; 
       int n, i;
       // table为空 初始化table
       if ((tab = table) == null || (n = tab.length) == 0)
           n = (tab = resize()).length;
       // 找到hash索引下标 该下标无数据 创建节点
       if ((p = tab[i = (n - 1) & hash]) == null)
           tab[i] = newNode(hash, key, value, null);
       // 有数据，查看第一个节点是否是目标key
       // 查看第一个节点是否为红黑树，是则查找红黑树
       // 否则，顺序遍历单项链表
       else {
           Node<K,V> e; 
           K k;
           if (p.hash == hash &&
               ((k = p.key) == key || (key != null && key.equals(k))))
               e = p;
           else if (p instanceof TreeNode)
               e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
           else {
               for (int binCount = 0; ; ++binCount) {
                   if ((e = p.next) == null) {
                       p.next = newNode(hash, key, value, null);
                       if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                           treeifyBin(tab, hash);
                       break;
                   }
                   if (e.hash == hash &&
                       ((k = e.key) == key || (key != null && key.equals(k))))
                       break;
                   p = e;
               }
           }
           if (e != null) { // existing mapping for key
               V oldValue = e.value;
               if (!onlyIfAbsent || oldValue == null)
                   e.value = value;
               afterNodeAccess(e);
               return oldValue;
           }
       }
       ++modCount;
       if (++size > threshold)
           resize();
       afterNodeInsertion(evict);
       return null;
    }
}
```

### HashMap的get方法
`注意`值为null或不存在时都返回null

1. 计算key的hash值，通过位运算找到key所在桶在数组中的索引下标
2. 如果与桶的第一个元素的key和hash值相等，直接返回
3. 如果桶是红黑树结构，通过红黑树查找
4. 如果是单向链表，顺序查找
5. 否则，返回null

```java
public class HashMap<K, V> extends AbstractMap<K, V> implements Map<K, V>, Cloneable, Serializable {
    public V get(Object key) {
        Node<K,V> e;
        return (e = getNode(hash(key), key)) == null ? null : e.value;
    }
    final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; 
        Node<K,V> first, e; 
        int n; 
        K k;
        // 当前map不为null
        // 且长度>0
        // 且该hash对长度取余获取到该下标的单向链表部位null
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
            // 如果第一个元素就是，直接返回
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            if ((e = first.next) != null) {
                if (first instanceof TreeNode)
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
    }
    // 红黑树查找
    // 查找红黑树根节点
    static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
        TreeNode<K,V> parent;  // red-black tree links
        TreeNode<K,V> left;
        TreeNode<K,V> right;
        TreeNode<K,V> prev;    // needed to unlink next upon deletion
        boolean red;
        TreeNode(int hash, K key, V val, Node<K,V> next) {
            super(hash, key, val, next);
        }
        final TreeNode<K,V> getTreeNode(int h, Object k) {
            return ((parent != null) ? root() : this).find(h, k, null);
        }
        final TreeNode<K,V> root() {
            for (TreeNode<K,V> r = this, p;;) {
                if ((p = r.parent) == null)
                    return r;
                r = p;
            }
        }
        // kc：comparableClassFor(key)
        final TreeNode<K,V> find(int h, Object k, Class<?> kc) {
            TreeNode<K,V> p = this;
            do {
                int ph, dir; K pk;
                TreeNode<K,V> pl = p.left, pr = p.right, q;
                if ((ph = p.hash) > h)
                    p = pl;
                else if (ph < h)
                    p = pr;
                else if ((pk = p.key) == k || (k != null && k.equals(pk)))
                    return p;
                else if (pl == null)
                    p = pr;
                else if (pr == null)
                    p = pl;
                else if ((kc != null ||
                          (kc = comparableClassFor(k)) != null) &&
                         (dir = compareComparables(kc, k, pk)) != 0)
                    p = (dir < 0) ? pl : pr;
                else if ((q = pr.find(h, k, kc)) != null)
                    return q;
                else
                    p = pl;
            } while (p != null);
            return null;
        }
    }
}
```

### HashMap的remove方法
1. 计算key的hash值，通过位运算找到key所在桶在数组中的索引下标
2. 如果与桶的第一个元素的key和hash值相等，则进入第五步
3. 否则，如果桶是红黑树结构，通过红黑树查找
4. 否则，如果桶是单向链表，顺序查找
5. 找到节点后，将节点的父节点指向该节点的next节点
6. 否则返回null

时间复杂度：O(logK)~O(K)

```java
public class HashMap<K, V> extends AbstractMap<K, V> implements Map<K, V>, Cloneable, Serializable {
    public V remove(Object key) {
        Node<K,V> e;
        return (e = removeNode(hash(key), key, null, false, true)) == null ?
            null : e.value;
    }
    // 第三个字段判断是否需要value也匹配，true-value也要相等，false-不需要判断value是否相等
    // 第四个参数：true-移动，false-不移动
    final Node<K,V> removeNode(int hash, Object key, Object value,
                               boolean matchValue, boolean movable) {
        Node<K,V>[] tab;
        Node<K,V> p; 
        int n, index;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (p = tab[index = (n - 1) & hash]) != null) {
            Node<K,V> node = null, e; 
            K k; 
            V v;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                node = p;
            else if ((e = p.next) != null) {
                if (p instanceof TreeNode)
                    node = ((TreeNode<K,V>)p).getTreeNode(hash, key);
                else {
                    do {
                        if (e.hash == hash &&
                            ((k = e.key) == key ||
                             (key != null && key.equals(k)))) {
                            node = e;
                            break;
                        }
                        p = e;
                    } while ((e = e.next) != null);
                }
            }
            if (node != null && (!matchValue || (v = node.value) == value ||
                                 (value != null && value.equals(v)))) {
                if (node instanceof TreeNode)
                    ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
                else if (node == p)
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

    /**
     * Removes all of the mappings from this map.
     * The map will be empty after this call returns.
     */
    public void clear() {
        Node<K,V>[] tab;
        modCount++;
        if ((tab = table) != null && size > 0) {
            size = 0;
            for (int i = 0; i < tab.length; ++i)
                tab[i] = null;
        }
    }
}
```

## ConcurrentHashMap采取了哪些方法来提高并发表现
- Hashtable是通过`synchronized`关键字来实现线程安全的，所有操作竞争同一把锁，大大降低了并发操作的效率
- JDK 1.7 中使用分段锁（ReentrantLock + Segment + HashEntry），相当于把一个 HashMap 分成多个段，每段分配一把锁，这样支持多线程访问。锁粒度：基于 Segment，包含多个 HashEntry。
- JDK 1.8 中使用 CAS + synchronized + Node + 红黑树。锁粒度：Node（首结点）（实现 Map.Entry<K,V>）。锁粒度降低了。
- 键值都不能为null

## Hashtable
- Hashtable是遗留类，很多映射的常用功能与HashMap类似，不同的是它承自Dictionary类
- 并且是线程安全的
- 任一时间只有一个线程能写Hashtable，并发性不如ConcurrentHashMap，因为ConcurrentHashMap引入了分段锁
- Hashtable不建议在新代码中使用，不需要线程安全的场合可以用HashMap替换，需要线程安全的场合可以用ConcurrentHashMap替换

## LinkedHashMap
- LinkedHashMap是HashMap的一个子类，保存了记录的插入顺序
- 在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的
- 也可以在构造时带参数，按照访问次序排序

## TreeMap
- TreeMap实现SortedMap接口
- 能够把它保存的记录根据键排序，默认是按键值的升序排序，也可以指定排序的比较器
- 当用Iterator遍历TreeMap时，得到的记录是排过序的。如果使用排序的映射，建议使用TreeMap
- 在使用TreeMap时，key必须实现Comparable接口或者在构造TreeMap传入自定义的Comparator，否则会在运行时抛出java.lang.ClassCastException类型的异常