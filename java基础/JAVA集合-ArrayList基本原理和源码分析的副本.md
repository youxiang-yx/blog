#### 基本原理

java自带的数组每次使用都要指定一个长度，这样对于要存储长度不固定的数据就会很麻烦。

随意jdk提供了一个ArrayList类来帮助我们存储不固定的数据，支持动态扩容。

#### 内部数据结构

ArrayList的内部实现就是存储在一个普通的java数组里面，但是会根据当前的数组长度进行扩容


#### 属性

```java
// 数组的默认长度
private static final int DEFAULT_CAPACITY = 10;
// 存储我们具体的内容，transient表示该字段不支持序列化，ArrayList自带了writeObject和readObject进行序列化
transient Object[] elementData;
// 在我们没有对arrylist进行具体操作前的默认值
DEFAULTCAPACITY_EMPTY_ELEMENTDATA
```

#### 添加内容

```java
public boolean add(E e) {
	// 记录对arraylist的添加次数用，修改和删除都会+1
    ++this.modCount;
    this.add(e, this.elementData, this.size);
    return true;
}


private void add(E e, Object[] elementData, int s) {
	// 扩容
    if (s == elementData.length) {
        elementData = this.grow();
    }
    // 保存值
    elementData[s] = e;
    // 记录当前数组的长度
    this.size = s + 1;
}
```