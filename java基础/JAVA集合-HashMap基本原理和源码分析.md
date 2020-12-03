#### 基本原理

HashMap是java常用的key，value类型的数据存储结构，其内部使用数组+链表+红黑树实现。是非线程安全类，对应线程安全的类有ConcurrentHashMap和HashTable。

##### HashMap数据结构图

|        0         |     1     |     2     |           3            |     4     |     5     |
| :--------------: | :-------: | :-------: | :--------------------: | :-------: | :-------: |
| 8Node&lt;K,V&gt; | Node<K,V> | Node<K,V> | size >=8 TreeNode<K,V> | Node<K,V> | Node<K,V> |

#### HashMap 属性说明

```java
// 通过位运算计算出数组的默认长度，有些地方也喜欢叫桶。
// 注: 在ConcurrentHashMap里面这个变量是直接等于16，不清楚这里为什么要用位运算。
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16

// 数组最大容量,超过这个数字将不会再进行扩容，详见resize()方法
static final int MAXIMUM_CAPACITY = 1 << 30; // aka 1073741824

// 传说中的加载因子，LOAD_FACTOR * CAPACITY = 当内容达到多大长度需要进行扩容。
// 注:不是数组的长度，是所有数组中链表长度的和。详见putVal方法的最后部分代码。
static final float DEFAULT_LOAD_FACTOR = 0.75f;

// 当链表的长度大于8时会把链表转成红黑树
static final int TREEIFY_THRESHOLD = 8;
// 当链表的长度小于6时会把红黑树转成链表
static final int UNTREEIFY_THRESHOLD = 6;

// 当数组长度大于64的时候才会转成红黑树，否则将进行扩容resize()
static final int MIN_TREEIFY_CAPACITY = 64;

// 实际存储内容的属性
transient Node<K,V>[] table;

// 当内容达到多大长度需要进行扩容,详见reseize()
int threshold;
```

#### 添加内容源码分析

````java
// 添加内容方法
public V put(K key, V value) {
  // 通过对key的hash运算，计算出该key存放在数组中的下标
  return putVal(hash(key), key, value, false, true);
}

static final int hash(Object key) {
  int h;
  // 这里只是计算出需要存放的位置，但不是数组中具体的下标
  return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}

final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
  // 初始化变量
  Node<K,V>[] tab; Node<K,V> p; int n, i;
  // 将table赋值给tab，这里是引用赋值，所以修改tab也就是修改table的引用对象
  // 如果 table 等于空或者长度等于0就会进行初始化，调用resize方法
  if ((tab = table) == null || (n = tab.length) == 0)
    n = (tab = resize()).length;
  // 第一部分是计算添加内容的具体存放下标，也就是table的下标。如果下标的值等于null，就直接创建一个新的链表，添加第一个值。
  if ((p = tab[i = (n - 1) & hash]) == null)
    tab[i] = newNode(hash, key, value, null);
  else {
    Node<K,V> e; K k;
    // 到了这一步说明table的下标里面是有值的，判断当前链表的第一个元素和要添加的元素是不是一样的，如果是那么就修改。
    // 这里是一个优化方案，可以不用循环链表再去修改该值了
    if (p.hash == hash &&
        ((k = p.key) == key || (key != null && key.equals(k))))
      e = p;
    // 如果数组中的内容是红黑树，那么就直接使用树去添加内容
    else if (p instanceof TreeNode)
      e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
    // 否则就循环链表
    else {
      for (int binCount = 0; ; ++binCount) {
        //找到了最后一个内容
        if ((e = p.next) == null) {
          // 创建一个新的内容，添加后最后
          p.next = newNode(hash, key, value, null);
          // 如果链表的长度大于TREEIFY_THRESHOLD也就是8，就将链表转换成红黑树，但是treeifyBin里面会判断数组长度是否大于MIN_TREEIFY_CAPACITY
          // 如果小于MIN_TREEIFY_CAPACITY就会扩容，大于就会转红黑树
          if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
            treeifyBin(tab, hash);
          break;
        }
        //如果在循环过程中，找到了key一摸一样的值，就会返回当前key的地址
        if (e.hash == hash &&
            ((k = e.key) == key || (key != null && key.equals(k))))
          break;
        p = e;
      }
    }
    // 如果e存在就会修改值
    if (e != null) { // existing mapping for key
      V oldValue = e.value;
      if (!onlyIfAbsent || oldValue == null)
        e.value = value;
      afterNodeAccess(e);
      return oldValue;
    }
  }
  ++modCount;
  // 如果数组里面的内容长度之和大于threshold，就会进行扩容
  if (++size > threshold)
    resize();
  afterNodeInsertion(evict);
  return null;
}
````

#### resize 方法源码分析

``` java
// 这个方法用来初始化table或者进行扩容
final Node<K,V>[] resize() {
  Node<K,V>[] oldTab = table;
  //老的数组长度
  int oldCap = (oldTab == null) ? 0 : oldTab.length;
  //老的扩容值
  int oldThr = threshold;
  //声明新的数组和扩容值
  int newCap, newThr = 0;
  //如果老的数组长度大于0，就会进行扩容
  if (oldCap > 0) {
    if (oldCap >= MAXIMUM_CAPACITY) {
      threshold = Integer.MAX_VALUE;
      return oldTab;
    }
    else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
             oldCap >= DEFAULT_INITIAL_CAPACITY)
      newThr = oldThr << 1; // double threshold
  }
  // 如果老的扩容值大于0，就会把新的数组长度改成老的扩容值
  else if (oldThr > 0) // initial capacity was placed in threshold
    newCap = oldThr;
  // 这个是直接初始化
  else {               // zero initial threshold signifies using defaults
    newCap = DEFAULT_INITIAL_CAPACITY;
    newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
  }
  if (newThr == 0) {
    float ft = (float)newCap * loadFactor;
    newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
              (int)ft : Integer.MAX_VALUE);
  }
  threshold = newThr;
  @SuppressWarnings({"rawtypes","unchecked"})
  // 初始化table
  Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
  table = newTab;
  // 如果是对table进行扩容，那么就会进行一个复制过程。把老的table里面的值赋值到新的table里面。因为修改数组，还不如创建一个新的性能更好。
  if (oldTab != null) {
   ........
  }
  return newTab;
}
```

#### treeifyBin源码分析

```java
// 将链表转成红黑树的方法
final void treeifyBin(Node<K,V>[] tab, int hash) {
  int n, index; Node<K,V> e;
  // 如果当前数组的长度小于MIN_TREEIFY_CAPACITY，就不会将链表转成红黑树，不管长度是8还是9
  if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
    resize();
  // 转换
  else if ((e = tab[index = (n - 1) & hash]) != null) {
    // 创建一颗红黑树
    TreeNode<K,V> hd = null, tl = null;
    do {
      // 将链表的内容添加到红黑树里面去
      TreeNode<K,V> p = replacementTreeNode(e, null);
      if (tl == null)
        hd = p;
      else {
        p.prev = tl;
        tl.next = p;
      }
      tl = p;
    } while ((e = e.next) != null);
    if ((tab[index] = hd) != null)
      hd.treeify(tab);
  }
}
```

HashMap基本原理就是这样了需要注意以下几点

1.并不是说链表长度大于8就一定会转成红黑树，而需要数组（桶）的长度大于MIN_TREEIFY_CAPACITY 64

2.HashMap并不是线程安全的类

3.扩容是通过创建一个新的table，而不是修改。