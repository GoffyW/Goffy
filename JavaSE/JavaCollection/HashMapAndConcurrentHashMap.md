# HashMap与ConcurrentHashMap

### HashMap:

##### 类继承图

![](C:\Users\Goffy\Desktop\继承图谱\HashMap.jpg)

##### 实现原理：

JDK1.8之前：数组+链表（散列集）

JDK1.8之后：数组+链表（散列集）+红黑二叉树

##### 核心成员变量

![1557891042167](C:\Users\Goffy\AppData\Roaming\Typora\typora-user-images\1557891042167.png)

1. 序列化机制验证版本
2. 初始容量
3. 最大容量，2的31次方
4. 默认装载因子
5. 当添加一个元素被添加到有至少TREEIFY_THRESHOLD节点的桶中，桶中链表将转化为链表
6. 同上，不过是将树形结构转化为链表
7. 桶可能被转化成树形结构的最小容量

###### 实例代码

```Java
private static final long serialVersionUID = 362498820763181265L;
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
static final int MAXIMUM_CAPACITY = 1 << 30;
static final float DEFAULT_LOAD_FACTOR = 0.75f;
static final int TREEIFY_THRESHOLD = 8;
static final int UNTREEIFY_THRESHOLD = 6;
static final int MIN_TREEIFY_CAPACITY = 64;
```

##### 成员属性

![1557891742915](C:\Users\Goffy\AppData\Roaming\Typora\typora-user-images\1557891742915.png)

##### 构造方法

![1557892222219](C:\Users\Goffy\AppData\Roaming\Typora\typora-user-images\1557892222219.png)

##### 简单总结：

- 无序、允许为Null、非同步
- 底层由散列表实现
- 初始容量和加载因子对其影响较大