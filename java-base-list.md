- [List](#List)
    - [ArrayList和LinkedList的区别](#ArrayList和LinkedList的区别)
    - [ArrayList和Vector的区别](#ArrayList和Vector的区别)
    - [ArrayList的扩容机制](#ArrayList的扩容机制)
    - [ArrayList在指定索引位置添加元素](#ArrayList在指定索引位置添加元素)
    - [ArrayList如果添加元素特别多应设置初始容量](#ArrayList如果添加元素特别多应设置初始容量)
    - [System.arraycopy方法和Arrays.copyOf方法区别](#System.arraycopy方法和Arrays.copyOf方法区别)
    - [ArrayList和LinkedList在指定下标插入元素的时间复杂度](#ArrayList和LinkedList在指定下标插入元素的时间复杂度)
    
# List
## ArrayList和LinkedList的区别
相同点：
- 都是不同步的，不能保证线程安全

不同点：
1. ArrayList底层实现使用的是Object数组；LinkedList使用的是双向链表
2. ArrayList支持随机访问，但是插入和删除元素时间复杂度为O(N)；LinkedList支持O(1)时间插入和删除，但是不支持随机访问
3. ArrayList空间占用体现在列表末尾的预留空间；LinkedList空间占用体现每个元素需要保存在前驱后继信息

实现了RandomAccess接口的list，优先选择普通for循环，其次foreach；
未实现RandomAccess接口的list，优先选择iterator遍历（foreach遍历底层也是通过iterator实现的），大size的数据千万不要使用普通for循环

## ArrayList和Vector的区别
```
Vector中的所有方法都是同步的，保证线程安全，但是在处理线程同步上会耗费大量的时间；
ArrayList是不同步的，不能保证线程安全
```

## ArrayList的扩容机制
```
    // 默认容量大小
    private static final int DEFAULT_CAPACITY = 10;

    // 空元素
    private static final Object[] EMPTY_ELEMENTDATA = {};

    // 初始化空元素
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

    // 如果等于DEFAULTCAPACITY_EMPTY_ELEMENTDATA，当第一个元素添加之后，会扩展DEFAULT_CAPACITY容量
    transient Object[] elementData; // non-private to simplify nested class access

    // 元素个数
    private int size;

   // 方式一：默认构造函数，实际上初始化赋值的是一个空数组。当真正对数组进行添加元素操作时，才真正分配容量。即向数组中添加第一个元素时，数组容量扩为10
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    // 方式二：用户自己指定容量
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+initialCapacity);
        }
    }

    // 方式三：用指定集合填充
    public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // c.toArray might (incorrectly) not return Object[] (see 6260652)
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }

    // 针对方式一，添加元素
    public boolean add(E e) {
        // 确保添加新元素的最小容量可满足
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
    private void ensureCapacityInternal(int minCapacity) {
        ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
    }
    // 计算容量
    private static int calculateCapacity(Object[] elementData, int minCapacity) {
        // 如果列表还未添加任何元素，返回默认值10和1中的最大值
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        }
        return minCapacity;
    }
    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;
        // 最小容量超出分配空间，需要扩容
        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ? Integer.MAX_VALUE : MAX_ARRAY_SIZE;
    }
```

## ArrayList在指定索引位置添加元素
```
// 在index位置添加新元素，将index及之后的元素往后移动，不建议如此使用
public void add(int index, E element) {
    rangeCheckForAdd(index);

    ensureCapacityInternal(size + 1);  // Increments modCount!!
    System.arraycopy(elementData, index, elementData, index + 1, size - index);
    elementData[index] = element;
    size++;
}
private void rangeCheckForAdd(int index) {
    if (index > size || index < 0)
        throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
}
```

## ArrayList如果添加元素特别多应设置初始容量
调用此方法，就不会持续的一次次扩容，插入一千万的数据，两者时间消耗是 2～20倍左右
ArrayList<Object> list = new ArrayList<>();
list.ensureCapacity(10000000);
```
public void ensureCapacity(int minCapacity) {
    int minExpand = (elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
        // any size if not default element table
        ? 0
        // larger than default for default empty table. It's already
        // supposed to be at default size.
        : DEFAULT_CAPACITY;

    if (minCapacity > minExpand) {
        ensureExplicitCapacity(minCapacity);
    }
}
```

## System.arraycopy方法和Arrays.copyOf方法区别
```
// Arrays.copyOf()主要用来扩容，底层也是System.arraycopy
int[] oldArr = {0, 1, 2, 3};
oldArr = Arrays.copyOf(oldArr, 10);
System.out.println(Arrays.toString(oldArr));
// [0, 1, 2, 3, 0, 0, 0, 0, 0, 0]

// System.arraycopy主要是插入删除数组元素
System.arraycopy(oldArr, 2, oldArr, 3, 2);
oldArr[2] = 99;
System.out.println(Arrays.toString(oldArr));
// [0, 1, 99, 2, 3, 0, 0, 0, 0, 0]
```

## ArrayList和LinkedList在指定下标插入元素的时间复杂度
ArrayList:O(n)
1. 检查下标是否合法
2. 检查容量是否足够，不足扩容为原来的1.5倍
3. 将指定索引及之后的值往后移动一位 O(n)
4. 给index赋值

LinkedList:O(n)
1. 检查下标是否合法，不合法则抛出下标越界异常
2. 如果插入位置是双向链表的长度，则获取末尾节点，并在其后插入该节点
3. 否则，根据插入位置靠前还是考后选择从头还是尾遍历链表，找到指定位置，找到其前指向节点 O(n)
4. 添加有前后指向的节点，并修改其后节点的前指针，前节点的后指针

```
/******************************** arraylist ********************************/    
    public void add(int index, E element) {
        // 检查下标是否合法
        rangeCheckForAdd(index);

        ensureCapacityInternal(size + 1);  // Increments modCount!!
        System.arraycopy(elementData, index, elementData, index + 1, size - index);
        elementData[index] = element;
        size++;
    }
    private void rangeCheckForAdd(int index) {
        if (index > size || index < 0)
            throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }
    private void ensureCapacityInternal(int minCapacity) {
        ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
    }
    private static int calculateCapacity(Object[] elementData, int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        }
        return minCapacity;
    }
    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;

        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
    
    
/******************************** linkedlist ********************************/    
    public void add(int index, E element) {
        checkPositionIndex(index);

        if (index == size)
            linkLast(element);
        else
            linkBefore(element, node(index));
    }
    private void checkPositionIndex(int index) {
        if (!isPositionIndex(index))
            throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }
    private boolean isPositionIndex(int index) {
        return index >= 0 && index <= size;
    }
    void linkLast(E e) {
        final Node<E> l = last;
        final Node<E> newNode = new Node<>(l, e, null);
        last = newNode;
        if (l == null)
            first = newNode;
        else
            l.next = newNode;
        size++;
        modCount++;
    }
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
    }
    void linkBefore(E e, Node<E> succ) {
        // assert succ != null;
        final Node<E> pred = succ.prev;
        final Node<E> newNode = new Node<>(pred, e, succ);
        succ.prev = newNode;
        if (pred == null)
            first = newNode;
        else
            pred.next = newNode;
        size++;
        modCount++;
    }
```