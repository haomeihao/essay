#### java.util.nodir.*;

##### Summary 总结
```
优质博客文章：
  https://www.cnblogs.com/gaochundong/p/data_structures_and_asymptotic_analysis.html
解释；
  E -> 元素
  T -> 类型
  K,V -> 键值对
用到的数据结构：
  数组(Array)、链表(Linked)、队列(Queue,Deque)、栈(Stack)
  哈希表(Hash)、二叉树(Tree)、红黑树(Red Black Tree)、堆(Heap)
  最基本的：数组、链表、红黑树、哈希表

单一数据结构实现：
  ArrayList -> Object[]数组 
    Vector(ArrayList的线程安全类)
    Stack extends Vector
  ArrayDeque -> 基于Object[]数组实现的双端队列

  LinkedList -> Node<E>双向链表
  
  TreeMap -> Entry<K,V>红黑树
    TreeSet

组合数据结构实现：
  HashMap -> Node<K,V>[]数组 + 单向链表/红黑树
    HashSet
  LinkedHashMap -> Entry<K,V>[]数组 + 双向链表/红黑树
    LinkedHashSet

  PriorityQueue -> 基于Object[]数组实现的完全二叉树，即堆(Heap)
```

* [1.接口](#_1)
```
  Collection
  Comparator java.lang.Comparable
  Iterator ListIterator java.lang.Iterable
  List
  Set SortedSet
  Map SortedMap
  Queue Deque
```

* [2.抽象类](#_2)
```
  AbstractCollection
  AbstractList
  AbstractSet
  AbstractMap
  AbstractQueue
  Calendar 日历
```

* [3.实现类](#_3)
```
  Collections
  Arrays(asList()返回一个ArrayList,自己实现的内部静态类，没有add()方法)

  ArrayList(底层就是一个Object[]数组) Vector  
  LinkedList(底层就是一个Node<E>双向链表)

  HashSet LinkedHashSet TreeSet

  HashMap(一个Node<K,V>[]数组 + 一个单向链表或者一个红黑树) Hashtable
    非常重要的方法resize()，
    扩容方法，并发情况下会有链表指向死循环的情况，导致CPU飙升。
  LinkedHashMap(一个Entry<K,V>[]数组 + 一个双向链表或者一个红黑树) 
    是否按照访问顺序遍历，默认插入顺序
  TreeMap(底层是一个Entry<K,V>红黑树)
    K要实现实现自内置比较器，或者比较时自定义实现外置比较器

  PriorityQueue(基于Object[]数组实现的完全二叉树(最大,最小),即堆(最大,最小))
    heapify()方法使Object[]数组进行堆化
  ArrayDeque(基于Object[]数组实现的双端队列)

  Stack extends Vector
  Properties extends Hashtable

  Random
  Date 日期
```

* [4.不可变类](#_4)
```
  Objects
  java.lang.Object(引用是否相等 equals(){==}) 
  java.lang.String(引用是否相等 是否都是String对象 值是否相等)
    如果是基本数据类型，就是equals() == 都是比较的值相等
    如果是引用数据类型，就是equals() 比较的值相等 ==比较引用相等 不一样
    Object类的equals()方法比较的是==引用是否相等
    子类重写了equals()方法，比较的是值相等，不是==
  UUID
```

##### 接口
```
package java.util;
1.Collection
  public interface Collection<E> extends Iterable<E>
2.Comparator 比较器
  @FunctionalInterface
  public interface Comparator<T>
    int compare(T o1, T o2);
3.Iterator 迭代器
  public interface Iterator<E>
    boolean hasNext();
    E next();
4.ListIterator 双向迭代器
  public interface ListIterator<E> extends Iterator<E>
    boolean hasPrevious();
    E previous();
5.List 列表
  public interface List<E> extends Collection<E>
6.Set 集合
  public interface Set<E> extends Collection<E>
7.SortedSet 排序集合
  public interface SortedSet<E> extends Set<E>
    Comparator<? super E> comparator();
8.Map 键值对
  public interface Map<K,V>
9.SortedMap 按K排序键值对
  public interface SortedMap<K,V> extends Map<K,V>
    Comparator<? super K> comparator();
10.Queue 队列
  public interface Queue<E> extends Collection<E>
11.Deque 双端队列
  public interface Deque<E> extends Queue<E>

package java.lang;
1.public interface Iterable<T> 可迭代的
  Iterator<T> iterator();
2.public interface Comparable<T> 可比较的
  public int compareTo(T o);
```

##### 抽象类
```
1.AbstractCollection
  public abstract class AbstractCollection<E> implements Collection<E>
2.AbstractList  
  public abstract class AbstractList<E> extends AbstractCollection<E> implements List<E>
3.AbstractSet
  public abstract class AbstractSet<E> extends AbstractCollection<E> implements Set<E>  
4.AbstractMap
  public abstract class AbstractMap<K,V> implements Map<K,V>  
5.AbstractQueue
  public abstract class AbstractQueue<E> extends AbstractCollection<E>
  implements Queue<E>
6.Calendar 日历
  public abstract class Calendar implements Serializable, Cloneable, Comparable<Calendar>
    public static Calendar getInstance() {
        return createCalendar(TimeZone.getDefault(), Locale.getDefault(Locale.Category.FORMAT));
    }
    public final Date getTime() {
        return new Date(getTimeInMillis());
    }
    public long getTimeInMillis() {
        if (!isTimeSet) {
            updateTime();
        }
        return time;
    }
```

##### 实现类
```
实现类
1.Collections
  public class Collections
    static class UnmodifiableCollection<E> implements Collection<E>, Serializable {}
    static class UnmodifiableList<E> extends UnmodifiableCollection<E>
                                 implements List<E> {}
    static class UnmodifiableSet<E> extends UnmodifiableCollection<E>
                                 implements Set<E>, Serializable {}
    private static class UnmodifiableMap<K,V> implements Map<K,V>, Serializable {}

    static class SynchronizedCollection<E> implements Collection<E>, Serializable {}
    static class SynchronizedList<E> extends SynchronizedCollection<E>
                                 implements List<E> {}
    static class SynchronizedSet<E> extends SynchronizedCollection<E>
                                 implements Set<E> {}
    private static class SynchronizedMap<K,V> implements Map<K,V>, Serializable {}
    
    public static <T extends Comparable<? super T>> void sort(List<T> list) {
        list.sort(null);
    }
    public static <T> void sort(List<T> list, Comparator<? super T> c) {
        list.sort(c);
    }
    default void sort(Comparator<? super E> c) {
        Object[] a = this.toArray();
        Arrays.sort(a, (Comparator) c);
        ListIterator<E> i = this.listIterator();
        for (Object e : a) {
            i.next();
            i.set((E) e);
        }
    }
2.Arrays
  public class Arrays
    public static void sort(int[] a) {
        // 双路快速排序
        DualPivotQuicksort.sort(a, 0, a.length - 1, null, 0, 0); 
    }
    public static void sort(Object[] a) {
        if (LegacyMergeSort.userRequested)
            // 归并排序 To be removed in a future release
            legacyMergeSort(a);
        else
            /** TimSort算法是一种起源于归并排序和插入排序的混合排序算法，设计初衷是为了在真实世界中的各种数据中可以有较好的性能。该算法最初是由Tim Peters于2002年在Python语言中提出的。 */
            ComparableTimSort.sort(a, 0, a.length, null, 0, 0);
    }
    private static void swap(Object[] x, int a, int b) {
        Object t = x[a];
        x[a] = x[b];
        x[b] = t;
    }
    public static <T> void sort(T[] a, Comparator<? super T> c) {
        if (c == null) {
            sort(a);
        } else {
            if (LegacyMergeSort.userRequested)
                legacyMergeSort(a, c);
            else
                TimSort.sort(a, 0, a.length, c, null, 0, 0);
        }
    }
    public static int binarySearch(int[] a, int key) {
        return binarySearch0(a, 0, a.length, key);
    }
    public static int binarySearch(Object[] a, Object key) {
        return binarySearch0(a, 0, a.length, key);
    }
    public static <T> int binarySearch(T[] a, T key, Comparator<? super T> c) {
        return binarySearch0(a, 0, a.length, key, c);
    }
    public static <T> List<T> asList(T... a) {
        return new ArrayList<>(a); 内部静态类
    }
    private static class ArrayList<E> extends AbstractList<E>
        implements RandomAccess, java.io.Serializable {
        private final E[] a;
        不支持add()方法; throw new UnsupportedOperationException();
    }
3.ArrayList
  public class ArrayList<E> extends AbstractList<E>
    implements List<E>, RandomAccess, Cloneable, java.io.Serializable {
      transient Object[] elementData; //transient 不被序列化
      public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            // 底层就是一个Object[]数组
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
    public Object[] toArray() {
        return Arrays.copyOf(elementData, size);
    }
  }
4.LinkedList 
  public class LinkedList<E> extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable {
    private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;
        // 底层就是一个双向链表
        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
    public boolean add(E e) {
        linkLast(e); // 最后追加
        return true;
    }
  }      
5.HashSet
  public class HashSet<E> extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable {
    private transient HashMap<E,Object> map;    
    private static final Object PRESENT = new Object();
    public HashSet() {
        map = new HashMap<>();
    }
    public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }
    public boolean remove(Object o) {
        return map.remove(o)==PRESENT;
    }
  }
6.LinkedHashSet
  public class LinkedHashSet<E> extends HashSet<E>
    implements Set<E>, Cloneable, java.io.Serializable {
    public LinkedHashSet() {
        super(16, .75f, true); // HashSet的构造方法
    }
    HashSet(int initialCapacity, float loadFactor, boolean dummy) {
        map = new LinkedHashMap<>(initialCapacity, loadFactor);
    }
  }      
7.TreeSet
  public class TreeSet<E> extends AbstractSet<E>
    implements NavigableSet<E>, Cloneable, java.io.Serializable {
    private transient NavigableMap<E,Object> m;
    private static final Object PRESENT = new Object();
    public TreeSet() {
        this(new TreeMap<E,Object>());
    }
    public boolean add(E e) {
        return m.put(e, PRESENT)==null;
    }
    public boolean remove(Object o) {
        return m.remove(o)==PRESENT;
    }
  }
8.HashMap
  public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {
    static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;
        // HashMap实现：一个Node<K,V>[]数组 + 一个单向链表
        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }
    }
    static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
        TreeNode<K,V> parent;  // red-black tree links
        TreeNode<K,V> left;
        TreeNode<K,V> right;
        TreeNode<K,V> prev;    // needed to unlink next upon deletion
        boolean red;
        // HashMap实现：一个TreeNode<K,V>[]数组 + 一个红黑树
        TreeNode(int hash, K key, V val, Node<K,V> next) {
            super(hash, key, val, next);
        }
    }
    // 初始化构造方法 默认(16, .75f)
    public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);
        this.loadFactor = loadFactor;
        this.threshold = tableSizeFor(initialCapacity);
    }
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
    public V get(Object key) {
        Node<K,V> e;
        return (e = getNode(hash(key), key)) == null ? null : e.value;
    }
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        if (++size > threshold)
            resize();            
    }
    // 非常重要的方法resize()扩容方法 
    // 并发情况下会有链表指向死循环的情况，导致CPU飙升
    final Node<K,V>[] resize() {}
    public Set<Map.Entry<K,V>> entrySet() {
        Set<Map.Entry<K,V>> es;
        return (es = entrySet) == null ? (entrySet = new EntrySet()) : es;
    }
  }
9.Hashtable HashMap的线程安全实现 方法上加了synchronized关键字
  public class Hashtable<K,V> extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable {
    private static class Entry<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Entry<K,V> next;
        // Hashtable实现：一个Entry<K,V>[]数组 + 一个单向链表
        protected Entry(int hash, K key, V value, Entry<K,V> next) {
            this.hash = hash;
            this.key =  key;
            this.value = value;
            this.next = next;
        }
    }    
  }
10.LinkedHashMap
  public class LinkedHashMap<K,V> extends HashMap<K,V> implements Map<K,V> {
    static class Entry<K,V> extends HashMap.Node<K,V> {
        Entry<K,V> before, after;
        // LinkedHashMap实现：一个Entry<K,V>[]数组 + 一个双向链表
        Entry(int hash, K key, V value, Node<K,V> next) {
            super(hash, key, value, next);
        }
    }
    static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
        TreeNode<K,V> parent;  // red-black tree links
        TreeNode<K,V> left;
        TreeNode<K,V> right;
        TreeNode<K,V> prev;    // needed to unlink next upon deletion
        boolean red;
        // LinkedHashMap实现：一个TreeNode<K,V>[]数组 + 一个红黑树
        TreeNode(int hash, K key, V val, Node<K,V> next) {
            super(hash, key, val, next);
        }
    }
    public LinkedHashMap(int initialCapacity, float loadFactor) {
        super(initialCapacity, loadFactor); // HashMap的构造方法
        accessOrder = false; // 是否按照访问顺序，默认插入顺序
    }
    public LinkedHashMap(int initialCapacity,float loadFactor,
        boolean accessOrder) {
        super(initialCapacity, loadFactor);
        this.accessOrder = accessOrder; // 初始化时设置遍历顺序
    }
    public V get(Object key) {
        Node<K,V> e;
        if ((e = getNode(hash(key), key)) == null)
            return null;
        if (accessOrder)
            afterNodeAccess(e); // move node to last
        return e.value;
    }
  }
11.WeakHashMap
  public class WeakHashMap<K,V> extends AbstractMap<K,V> implements Map<K,V> {}
12.TreeMap
  public class TreeMap<K,V> extends AbstractMap<K,V>
    implements NavigableMap<K,V>, Cloneable, java.io.Serializable {
    private final Comparator<? super K> comparator; // 自定义K比较器
    private transient Entry<K,V> root; // 树的根节点root
    static final class Entry<K,V> implements Map.Entry<K,V> {
        K key;
        V value;
        Entry<K,V> left;
        Entry<K,V> right;
        Entry<K,V> parent;
        boolean color = BLACK;
        // 底层是一个红黑树
        Entry(K key, V value, Entry<K,V> parent) {
            this.key = key;
            this.value = value;
            this.parent = parent;
        }
    }
    public TreeMap() {
        comparator = null;
    }
    public V get(Object key) {
        Entry<K,V> p = getEntry(key);
        return (p==null ? null : p.value);
    }
    final Entry<K,V> getEntry(Object key) {
        if (comparator != null)
            return getEntryUsingComparator(key);
        int cmp = k.compareTo(p.key); // 实现自内置比较器
    }
    final Entry<K,V> getEntryUsingComparator(Object key) {
        int cmp = cpr.compare(k, p.key); // 实现自外置比较器
    }
    final int compare(Object k1, Object k2) {
        return comparator==null ? 
          ((Comparable<? super K>)k1).compareTo((K)k2)
          : comparator.compare((K)k1, (K)k2);
    }
  }
13.PriorityQueue 
  优先级队列：基于Object[]数组实现的完全二叉树(最大,最小),即堆(最大,最小)
  public class PriorityQueue<E> extends AbstractQueue<E>
    implements java.io.Serializable {
    private static final int DEFAULT_INITIAL_CAPACITY = 11;
    transient Object[] queue;
    private final Comparator<? super E> comparator;
    public PriorityQueue() {
        this(DEFAULT_INITIAL_CAPACITY, null);
    }
    public PriorityQueue(int initialCapacity) {
        this(initialCapacity, null);
    }
    public PriorityQueue(Comparator<? super E> comparator) {
        this(DEFAULT_INITIAL_CAPACITY, comparator);
    }
    public PriorityQueue(int initialCapacity,Comparator<? super E> comparator) {
        if (initialCapacity < 1)
            throw new IllegalArgumentException();
        this.queue = new Object[initialCapacity];
        this.comparator = comparator;
    }
    private void heapify() { // Object[]数组进行堆化
        for (int i = (size >>> 1) - 1; i >= 0; i--)
            siftDown(i, (E) queue[i]);
    }
    siftUp siftUpComparable siftUpUsingComparator
    siftDown siftDownComparable siftDownUsingComparator
  }
14.ArrayDeque 基于Object[]数组实现的双端队列 elements[head] elements[tail]
  public class ArrayDeque<E> extends AbstractCollection<E>
    implements Deque<E>, Cloneable, Serializable {
    transient Object[] elements;
    transient int head;
    transient int tail;
    private static final int MIN_INITIAL_CAPACITY = 8;
    public ArrayDeque() {
        elements = new Object[16];
    }
  }
15.Vector ArrayList的线程安全实现 方法上加了synchronized关键字
  public class Vector<E> extends AbstractList<E>
    implements List<E>, RandomAccess, Cloneable, java.io.Serializable {
    protected Object[] elementData;
  }
16.Stack 栈 线程安全的Object[]实现，操作方法做了阉割
  public class Stack<E> extends Vector<E> {}
17.Properties 借用了Hashtable的实现方法，加上了自己的一些方法load() store()...
  public class Properties extends Hashtable<Object,Object> {}
18.Random
  public class Random implements java.io.Serializable {
    private final AtomicLong seed;
    public int nextInt() {
        return next(32);
    }
    public long nextLong() {
        return ((long)(next(32)) << 32) + next(32);
    }
  }
19.Date
  public class Date
  implements java.io.Serializable, Cloneable, Comparable<Date> {
    private transient long fastTime;
    public Date() {
        this(System.currentTimeMillis());
    }
    public Date(long date) {
        fastTime = date;
    }
    public long getTime() {
        return getTimeImpl();
    }
    private final long getTimeImpl() {
        if (cdate != null && !cdate.isNormalized()) {
            normalize();
        }
        return fastTime;
    }
    public boolean equals(Object obj) {
        return obj instanceof Date && getTime() == ((Date) obj).getTime();
    }
  }
20.ComparableTimSort  
  class ComparableTimSort {}
21.TimSort  
  class TimSort<T> {}
```

##### 不可变类
```
package java.util;
1.Objects
  public final class Objects {
    public static boolean equals(Object a, Object b) {
        return (a == b) || (a != null && a.equals(b)); // 引用相等 或者 值相等
    }
    public static int hashCode(Object o) {
        return o != null ? o.hashCode() : 0;
    }
    public static <T> int compare(T a, T b, Comparator<? super T> c) {
        return (a == b) ? 0 :  c.compare(a, b);
    }
    public static boolean isNull(Object obj) {
        return obj == null;
    }
    public static boolean nonNull(Object obj) {
        return obj != null;
    }
  }
顺便介绍下java.lang.Object 和 java.lang.String
package java.lang;
public class Object {
    public boolean equals(Object obj) {
        return (this == obj); // 引用是否相等
    }
}
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    public boolean equals(Object anObject) {
        if (this == anObject) { // 引用是否相等
            return true;
        }
        if (anObject instanceof String) { // 是否都是String对象
            String anotherString = (String)anObject;
            int n = value.length;
            if (n == anotherString.value.length) {
                char v1[] = value;
                char v2[] = anotherString.value;
                int i = 0;
                while (n-- != 0) {
                    if (v1[i] != v2[i])
                        return false; // 判断String对象的字符是否都对应相等
                    i++;
                }
                return true; // 值是否相等
            }
        }
        return false;
    }
}
2.UUID
  public final class UUID implements java.io.Serializable, Comparable<UUID> {}
3.DualPivotQuicksort
  final class DualPivotQuicksort {} 都是排序算法的方法
```

##### 总结(废弃)
```
public interface Collection<E> extends Iterable<E>
@FunctionalInterface public interface Comparator<T>  
  -> 比较器
  int compare(T o1, T o2);
public interface Deque<E> extends Queue<E>
  -> 双端队列
public interface Iterator<E>
  -> 迭代器
  boolean hasNext();
  E next();
  default void remove() {
    throw new UnsupportedOperationException("remove");
  }
public interface List<E> extends Collection<E>

package java.lang;
public interface Iterable<T>
  -> 可迭代的
  Iterator<T> iterator();
public interface Comparable<T>
  -> 可比较的
  public int compareTo(T o);

package java.util;
public class ArrayDeque<E> extends AbstractCollection<E>
                           implements Deque<E>, Cloneable, Serializable 
  -> 基于Array实现的双端队列
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable 
  -> 基于Array实现的列表
public class Arrays 
  -> Array工具类
public class Collections 
  -> Collection工具类，创建Set List Map
public class Date
    implements java.io.Serializable, Cloneable, Comparable<Date>
final class DualPivotQuicksort
  -> 双路快速排序算法    
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable
  静态内部类(Node)  
  static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next; -> 链表节点,单向链表
    }
  static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
        TreeNode<K,V> parent;  // red-black tree links
        TreeNode<K,V> left;
        TreeNode<K,V> right;
        TreeNode<K,V> prev;    // needed to unlink next upon deletion
        boolean red; -> 红黑树
    }
  遍历顺序为 数组顺序+单向链表顺序，导致会变化的原因是rehash()
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable
  -> 就是基于HashMap的keySet实现的
public class Hashtable<K,V>
    extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable
  -> public方法都加 synchronized 保证线程安全
public class LinkedHashMap<K,V>
    extends HashMap<K,V>
    implements Map<K,V>
  -> 双向链表改造 HashMap  
  static class Entry<K,V> extends HashMap.Node<K,V> {
        Entry<K,V> before, after; -> 链表节点,双向链表
    }
  final boolean accessOrder;
    true -> access-order 访问顺序
    false -> insertion-order 插入顺序  
  private void linkNodeLast(LinkedHashMap.Entry<K,V> p) {
        LinkedHashMap.Entry<K,V> last = tail;
        tail = p;
        if (last == null)
            head = p;
        else {
            p.before = last;
            last.after = p;
        }
    }
  public V get(Object key) {
        Node<K,V> e;
        if ((e = getNode(hash(key), key)) == null)
            return null;
        if (accessOrder)
            afterNodeAccess(e); -> move node to last
        return e.value;
    }
```
