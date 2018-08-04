#### Collection

```
优质博客文章：https://www.cnblogs.com/gaochundong/p/data_structures_and_asymptotic_analysis.html

      |   List   |   Set   |   Map   |   Queue
Array
Linked
Queue
Stack

Tree
Red Black Tree    
2-3-4 Tree
Heap

Hash

Graph

```

- #### java.util.List
```
package java.util;
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