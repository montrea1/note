## java集合分类  
###集合
    装载数据的容器。根据集合的类型特点，针对特定场景的数据进行装载、处理。

### java集合类型：
    主要类型： 
        list: 线性结构，物理或逻辑上。
        java中的主要集合类型：
        collection
            list
                有序元素，可重复集合，有索引
            set
                无序元素，重复集合，无索引
            queue
                队列，先进先出
        map
            键值对

### List
###### 1.ArrayList
    结构：数组实现。连续的内存地址。
    特点：随机查询快。可重复插入数据.允许null.线程不同步
    缺点：随机插入删除慢。随机数组中间插入或删除一个数据时，可能会慢。
```java
    //add
    public boolean add(E e){
        /*添加某个元素。添加时判断list容量。
         *添加到最后一个元素
         */
    }

    public void add(int index, E element) {
         //1.判断索引是否越界
         //2.判断是否需要扩容
         //采用数组复制的方式,对指定的索引进行让位复制处理
         //3.添加元素
     }

     public boolean addAll(Collection<? extends E> c) {
         //1.将传入的集合转为object数组
         //2.判断list容量
         //3.复制数组
     }
    public boolean addAll(int index, Collection<? extends E> c) {
        //1.判断索引是否越界
        //2.将传入的集合转为object数组
        //3.扩容判断
        //4.复制数组
    }
    
    //remove
    public E remove(int index) {
        //1.判断索引是否越界
        //2.复制数组
        //3.将指定索引设置为null
    }
    public boolean remove(Object o) {
        //1.判断判断是否为null
        //2.通过数组复制得到先数组
        //3.对最后一个索引数据设置为null
    }
    public void clear() {
        //遍历list,将所有值设置为null
    }

```


###### 2.Vector
    与ArrayListList实现基本类似。synchronized 线程锁，"线程安全"。实际上并不绝对安全。

###### 3.LinkedList
    链表，实现了list和queue接口。拥有这两个集合的特点。
    结构：逻辑上线性连接。
    特点：增删快，查询慢

#### Set
###### 1.HashSet   
    由hash表实现。实际上是一个hashMap实例。无序。可以存在null

###### 2.TreeSet
    基于TreeMap实现。通过Comparator，保证元素顺序。


#### Queue
###### 1.Deque
    双端队列，支持两端插入和删除。

###### 2.PriorityQueue
    优先队列。不允许null，不支持不可比较类型。
###### 2.LinkedList
    链表

#### Map
###### 1.HashMap
    实现map的hashtable。拥有map的所有特性。允许null，无序
        不支持线程同步
```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
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
```

###### 2.TreeMap
    实现map的红黑树。通过Comparable/Comparator实现元素排序。有序
    不支持线程同步

###### 3.HashTable
    通过key-value方式实现了一个hashtable。允许任何not-null对象
    线程同步