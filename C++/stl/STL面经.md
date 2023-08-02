## vector

连续内存的数组

```cpp
class vector{
	T* start;
  T* finish;
  T* end_of_story;
}
```

扩容：

```cpp
size()  // 当前元素个数
capacity（）//总的容量大小
  
resize() // 会减小或新增元素，新增会赋初始值
reserve() // 只会改变capacity，若大于当前capacity，会copy到其他内存，并销毁原来的空间
```

扩容1.5/2倍

申请空间、元素移动、释放原空间

push_back涉及到扩容移动，所以线程不安全 ==> tbb::concurent_vector

## list

循环带头双向链表

链表结点和链表分开定义，结点包含pre、next指针和data数据

优点：不会造成内存的浪费，占用内存小

## deque

多段连续空间构成的双端队列，可以o(1)的从头和尾插入

```cpp
class deque{
T** map; //中控器
T* start;
T* finish;
}
```

中控器是一个默认长度为8的map，依次指向每一个连续区域

下标访问 o(1)但是比vector慢

头尾插入o(1) 中间比较慢

## stack和queue

以deque为底层容器

stack封住了头段开口

queue封住了尾段开口

## priority_queue

以vector作为底层容器，heap作为处理规则（静态堆）

## set和map

底层红黑树,基于链表

和AVL的区别：

* AVL不适合频繁插入、删除，会引起多次旋转操作
* 红黑树插入最多两次、删除最多三次

增删查改o(logn)，链表占用内存小

## Unordered_map/ Unordered_set

哈希表 增删查改复杂度o(1) 数据分布不好时，一直解决冲突，最差能到o(n) 

## STL内存管理

两级配置器（Allocator）

为避免内存碎片

* 第一级配置器（malloc、realloc、free）：大于128B

* 第二级配置器（内存池、空闲链表）：小于128B

### 次级分配器的内存池技术

做法：TODO

优点：避免外部碎片，不用频繁的内存用户态切内核态，性能高效

缺点：可能有内部碎片，比如申请120B一定会分配128B

## push_back 和 emplace_back

emplace/emplace_back函数使用传递来的参数直接在容器管理的内存空间中构造元素（只调用了构造函数）；push_back会创建一个局部临时对象，并将其压入容器中（可能调用拷贝构造函数或移动构造函数）

## STL的排序算法

数据量很大用**快排**

划分区域较小（**16**）的时候用**插入排序**

划分有导致最坏情况的倾向时（递归层次过高）使用**堆排序**

> 为什么不直接堆排序，因为堆访问是1，2，4，8..Cache不友好，而快排顺序访存

