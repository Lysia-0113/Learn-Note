# 概念

## 数组和集合区别在哪？常使用的？

### 区别

1. 数组一般是不可变长的，而集合是可以的，提供了一些接口和方法来实现可变长度
2. 集合内只能是对象，而数组可以是基本数据类型
3. 数组可以直接访问对象，而集合要通过迭代器或者对应方法

### 常用

1. ArrayList：数组列表，基于数组实现，add(object o) /clear()/remove(object o)/get()
2. HashMap：哈希map，基于哈希表实现，键值对（key，value），put(object o)/containsKey(key)/getOrdefault(key,default)
3. HashSet：哈希set，哈希表实现，不可重复，add(object 0)/contains(key)
4. LinkedList:基于链表的列表，高效插入删除，addFirst()/addLast()/getFirst()/getLast
5. LinkedQueue：链表实现的队列，先进先出，offer()/poll()/peek()

## 集合遍历的方式

```java
//最普通的for循环
for(int i;i<list.length();i++)
//增强for，
for(element e:list)
//foreach
list.forEach(element->sout(element))
//迭代器
Iterator<Object> iterator=list.iterator();
while(iterator.hasNext){
    Object o=iterator.next();//每调用一次next，游标都会往后移一次
} 
```

> 后三者都涉及到了迭代器，迭代器的remove方法是并发修改安全的，而集合本身的remove是不安全的。

## Java中的集合概览

java.util.Collection (接口)  ————————— 顶级集合接口，存储单个元素
       │
       ├── List (接口)  ————— 有序、可重复，按索引访问
       │    ├── ArrayList (实现类) — 动态数组（随机访问快）
       │    ├── LinkedList (实现类) — 双向链表（增删快）
       │    └── Vector (实现类) — 线程安全的动态数组（效率低，基本淘汰）
       │
       ├── Set (接口)  ————— 无序、不可重复（依赖equals/hashCode）
       │    ├── HashSet (实现类) — 基于HashMap，无序不可重复
       │    │    └── LinkedHashSet (实现类) — 有序（维护插入顺序）的HashSet
       │    └── TreeSet (实现类) — 基于红黑树，自然排序（如数字升序、字符串字典序）
       │
       └── Queue (接口)  ————— 队列（先进先出FIFO）
            ├── LinkedList (实现类) — 同时实现Queue，可作为双向队列
            └── PriorityQueue (实现类) — 优先级队列（按优先级出队，非FIFO）

java.util.Map (接口)  ————————— 顶级映射接口，存储键值对（key-value）
       │
       ├── HashMap (实现类) — 哈希表（数组+链表/红黑树），无序，key唯一，允许null
       │    └── LinkedHashMap (实现类) — 有序（维护插入/访问顺序）的HashMap
       ├── Hashtable (实现类) — 线程安全的哈希表（效率低，不允许null）
       └── TreeMap (实现类) — 基于红黑树，key自然排序

# List

核心是保证有序，可重复

## 实现

- ArrayList
- LinkedList
- Vector

## 应用场景

数组在查找，增加场景；链表在插入，删除场景；vector则是线程安全的，除非要求，一般不用。

## ArrayList的扩容机制

1. 创建无参构造的ArrayList时第一次扩容从0扩容到10

2. 容量不够时扩容1.5倍(通过位运算实现，效率更高)，

3. 通过Arrays.copyOf()对数组进行复制扩容

   > 有最大的容量MAX_ARRAY_SIZE约为21亿（Integer.MAX_VALUE)

# Set

特点在于元素唯一，无序，但是一些特殊实现TreeSet，LinkedHashSet能够保证有序

> 这里无序指的是添加元素和存储元素不一定一致，打印出来也可能不一致，所以使用它不应依赖任何特定顺序

## 实现

- HashSet
- LinkedHashSet（用链表维护顺序，按插入顺序）
- TreeSet（底层基于红黑树，）

# Map

特点是键值对，key唯一（要用哈希函数，只能唯一），value可重复，查询复杂度*O（1）*

## 实现

- HashMap
- LinkedHashMap
- TreeMap

## HashMap的实现原理

- 在JDK1.7及之前，利用的是**数组+链表**，数组中的位置叫桶（bucket），在发生哈希冲突时候会形成链表，最坏查询复杂度*O(n)*（**此时采用的是头插法 **，后因为多线程情况下会使链表成环改为尾插法）

- JDK1.8后，就是**数组+链表+红黑树**，在链表长度超过8且数组长度大于64的情况下会转用红黑树存储，最坏查询复杂度为 *O（Log n）*

  > 数组长度不够，链表达到8了只会扩容，不会树化





