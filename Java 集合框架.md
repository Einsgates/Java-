# Java 集合框架

## 集合概述

### 集合框架

### List Set Map 之区别

- List

  对付顺序的好帮手
  存储元素有序、可重复

- Set

  注重独一无二
  存储的元素有序、不可重复

- Map

  用 Key 来搜索的专家
  使用键值对 (key-value) 存储，类似于数学上的 y = f(x)，x = key, y = value
  Key 无序不可重复，Value 无需、可重复
  每个键最大映射到一个值

### 集合框架底层数据结构

- List

	- ArrayList

	  Object[] 数组

	- Vector

	  Object[] 数组

	- LinkedList

	  双向链表

- Set

	- HashSet

	  无序唯一
	  基于HashMap实现的，底层采用HashMap 来保存元素

	- LinkedHashSet

	  LinkedHashSet 是 HashSet 的子类，并且其内部是通过 LinkedHashMap 来实现的。
	  类似于LinkedHashMap 内部基于 HashMap 实现

	- TreeSet

	  有序唯一
	  红黑树

- Map

	- HashMap

	  JDK 1.8 之前 HashMap 由数组+链表组成，数组是 HashMap 的主体，链表则是为了解决哈希冲突而存在 (拉链法解决冲突)
	  JDK 1.8 之后，当链表长度大于阈值 (默认为8 将链表转化为红黑树前会判断，如果数组长度小于64，会先对数组进行扩容，而不是转化为红黑树)时，将链表转化为红黑树，以减少搜索时间。

	- HashTable

	  数组+链表，数组是 HashMap 的主体，链表则是为了解决哈希冲突而存在的

	- TreeMap

	  红黑树 (自动平衡的排序二叉树)

### 如何选用集合

根据集合特点

- 根据键值对取元素——Map

	- 排序——TreeMap
	- 不排序——HashMap
	- 线程安全——ConcurrentHashMap

- 存放元素值——Collection

	- 保证元素唯一——Set

		- TreeSet
		- HashSet

	- 不需要元素唯一——List

		- ArrayList
		- LinkedList

### 为什么要使用集合

当需要保存一组类型相同的数据时，应该用一个容器来保存——数组，但数组具有一定弊端。
一旦声明之后，长度不可变。同时，声明数组时的数据类型也决定了该数组存储的数据的类型。而且，数组存储的数据是有序的、可重复的、特点单一。
但是集合提高了数据存储的灵活性，Java 集合不仅可以用来存储不同类型不同数量的对象，还可以还可以保存具有映射关系的数据

### Iterator 迭代器

- 是什么

  public interface Iterator<E> {
      //集合中是否还有元素
      boolean hasNext();
      //获得集合中的下一个元素
      E next();
      ......
  }
  
  Iterator 对象称为迭代器 (设计模式的一种)，迭代器可以对集合进行遍历，但每一个集合内部的数据结构可能不尽相同。所以每一个集合存和取都很可能是不一样的，虽然我们可以自定义hasNext() 和 next()，但此甚为臃肿。
  迭代器由此而生。
  
  迭代器是将这样的方法抽取出接口，然后在每个类的内部，定义自己的迭代方式，这样做就规定了整个集合的遍历方式都是 hasNext() 和 next() 方法，使用者会用即可，不许考虑如何实现。
  
  迭代器定义为：提供一种方法，访问一个容器对象中各个元素，而不需要暴露该对象的内部细节。

- 有啥用

  迭代器可以对集合进行遍历，更加安全。
  因为它可以确保，在当前遍历的集合元素被修改的时候，就会抛出 
  ConcurrentModificationException 异常

- 怎么用

  Map<Integer, String> map = new HashMap();
  map.put(1, "Java");
  map.put(2, "C++");
  map.put(3, "PHP");
  Iterator<Map.Entry<Integer, String>> iterator = map.entrySet().iterator();
  while (iterator.hasNext()) {
    Map.Entry<Integer, String> entry = iterator.next();
    System.out.println(entry.getKey() + entry.getValue());
  }
  1Java
  2C++
  3PHP

### 哪些集合线程不安全？如何解决？

ArrayList LinkedList HashSet TreeSet TreeMap PriorityQueue
都不是线程安全的

- CocurrentHashMap

  线程安全的 HashMap

- CopyOnWriteArrayList

  线程安全的 ArrayList，在读多写少的场合非常好，远远好于 Vector

- CocurrentLinkedQueue

  高效的并发队列，使用链表实现。
  可以看作一个线程安全的 LinkedList，这是一个非阻塞队列。

- BlockingQueue

  这是一个接口，JDK 内部通过链表、数组等方式实现了这个接口。
  表示阻塞队列，非常适用于作为数据共享的通道。

- CocurrentSkipListMap

  跳表的实现。
  这是一个 Map，使用跳表的数据结构进行快速查找。

## Collection子接口之 List

### ArrayList 和 Vector 区别

- ArrayList

  ArrayList 是 List 的主要实现类，底层使用 Object[] 存储，适用于频繁的查找工作，线程不安全。

- Vector

  Vector 是 List 的古老实现类，底层使用 Object[] 存储，线程安全。

### ArrayList 与 LinkedList 区别

- 是否保证线程安全

  二者都是不同步，都不保证线程安全

- 底层数据结构

  ArrayList 底层采用 Object[] 数组；
  LinkedList 底层采用 双向链表。

- 插入和删除是否受元素位置的影响

  ArrayList 采用数组存储，所以插入和删除元素的时间复杂度受元素位置影响O(n)。
  LinkedList 采用链表存储，所以对于插入删除，O(1)。若在指定位置插入删除，则O(n)，因为要先移动到指定位置再插入

- 是否支持快速随机访问

  LinkedList 不支持高效的随机访问，而 ArrayList 支持。可通过元素序号快速获取。

- 内存空间占用

  ArrayList 空间浪费主要体现在 list 列表的结尾会预留一定的容量空间
  LinkedList 空间浪费则体现在它的每一个元素都需要消耗比 ArrayList 更多的空间 (因为要存放直接后继和直接前驱已经数据)。

### ArrayList 的扩容机制

可以看到，底层其实是调用了Arrays.copyOf方法来进行扩充数组容量的。这里我们主要看一下最后一个方法newCapacity(int minCapacity)的实现。
默认情况下，新的容量会是原容量的1.5倍，这里用了位运算提高效率。一般情况下，如果扩容1.5倍后就大于期望容量，那就返回这个1.5倍旧容量的值。而如果小于期望容量，那就返回期望容量。这里对默认容量10做了特殊处理。
使用1.5倍这个数值而不是直接使用期望容量，是为了防止频繁扩容影响性能。试想如果每次add操作都要扩容一次，那性能将会非常低下。

## Collection子接口之Set

### Comparable 和 Comparator 区别

### 无序性和不可重复性

### 比较 HashSet LinkedHashSet TreeSet 异同

## Map 接口

### HashMap 和 HashTable 区别

### HashMap 和 HashSet 区别

### HashMap 和 TreeMap 区别

### HashSet 检查重复

### HashMap 底层实现

### HashMap 相关问题

- HashMap 长度为什么是2的次幂
- HashMap 多线程操作导致死循环问题
- HashMap 常见遍历方式
- CocurrentHashMap 和 HashTable 区别
- CocurrentHashMap 线程安全与底层实现
- Subtopic 6

## Collections 工具类

## 其他重要问题

