## JavaSE基础

### Java面向对象

#### 面向对象特征

+ 封装
+ 继承
+ 多态
+ 抽象

#### 访问权限修饰符

|    修饰符    | 当前类  | 相同包  |  子类  | 其他包  |
| :-------: | :--: | :--: | :--: | :--: |
|  public   |  可以  |  可以  |  可以  |  可以  |
| protected |  可以  |  可以  |  可以  |      |
|  default  |  可以  |  可以  |      |      |
|  private  |  可以  |      |      |      |

#### 对象clone

```java
// 1. 实现Cloneable接口
// 2. 重写clone方法
public class Person implements Cloneable{
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

public class Test{
	Person p = new Person();
  	Person p1 = (Person) p.clone();
}
```

##### 浅拷贝和深拷贝

+ 浅拷贝 : 被拷贝对象中的对象只拷贝引用
+ 深拷贝 : 被拷贝对象中的对象创建新对象

clone是浅拷贝

```java
// 深拷贝
public class Body implements Cloneable {
    public Head head;

    @Override
    protected Object clone() throws CloneNotSupportedException {
        Body body = new Body((Head) this.head.clone());
        return body;
    }
}

public class Head implements Cloneable{
    public String face;

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

#### String

String是final类,不可以被继承.

### 抽象abstract

以下不能和抽象方法共存

+ private (无法重写)
+ final (无法重写)
+ static (无法重写)
+ native (native由本地代码实现,抽象方法没有实现)
+ synchronized (synchronized与实现细节有关,抽象方法没有实现)

### 常见的RuntimeException

+ java.lang.NullPointerException 空指针异常
+ java.lang.ClassNotFoundException 类未找到异常
+ java.lang.IndexOutOfBoundsExcetion 数组角标越界异常
+ java.lang.NumberFormatException 字符串转为数字异常
+ java.lang.ClassCastException 数据类型转换异常
+ java.lang.NoSuchMethodException 方法不存在异常

### 常用API

#### switch作用类型

+ Java5以前 : byte short char int
+ Java5添加 : enum
+ Java7添加 : String

#### String

##### String对象的intern()方法

返回常量池中对应的字符串,如果常量池中没有就添加字符串到常量池,再返回.

```java
String s1 = "Programming";
String s2 = new String("Programming");
String s3 = "Program";
String s4 = "ming";
String s5 = "Program" + "ming";
String s6 = s3 + s4;
System.out.println(s1 == s2);			// false
System.out.println(s1 == s5);			// true
System.out.println(s1 == s6);			// false
System.out.println(s1 == s6.intern());	// true
System.out.println(s2 == s2.intern());	// false
```

#### Integer

```java
Integer f1 = 100;
Integer f2 = 100;
Integer f3 = 150;
Integer f4 = 150;
System.out.println(f1 == f2);	// true
System.out.println(f1 == f2);	// false
```

如果整型字面量的值在-128到127之间,不会创建新的Integer对象,直接引用常量池中的Integer对象.

### 集合

+ **Collection** 单列集合
  + **List** 有序, 元素可以重复
    + **ArrayList** 数组结构, 线程不同步, 查询快, 增删慢
    + **LinkedList** 链表结构(循环双向链表), 线程不同步, 增删快, 查询慢
    + **Vector** 数组结构, 线程同步, 增删查询都慢
  + **Set** 元素不能重复, 底层由Map实现
    + **HashSet** 哈希表结构, 线程不同步
      + **LinkedHashSet** 有序
    + **TreeSet** 二叉树结构, 线程不同步, 可以排序
+ **Map** 双列集合, 存储的是键值对，键唯一
  + **HashMap** 哈希表结构, 线程不同步, 允许null键null值
    + **LinkedHashMap** 有序
  + **TreeMap** 二叉树结构, 线程不同步, 可以排序
  + **Hashtable** 哈希表结构, 线程同步, 不允许null键null值
    + **Properties**

### 多线程

#### 线程和进程的区别

+ 进程 : 具有一定独立功能的程序关于某个数据集合上的一次运行活动,是操作系统进行资源分配和调度的一个独立单位
+ 线程 : 是进程的一个实体,是cpu调度和分派的基本单位,是比进程更小的可以独立运行的基本单位

#### 创建多线程的方式

+ 继承Thread类
+ 实现Runnable接口
+ 实现Callable接口 (Java5之后)

#### wait和sleep的区别

1. wait可以指定时间,也可以不指定

   sleep必须指定时间

2. wait释放执行权,释放锁

   sleep释放执行权,不释放锁

3. wait通常被用于线程间交互

   sleep通常被用于线程暂停执行

#### 线程的五种状态

+ 新建状态 (new创建一个线程)
+ 就绪状态 (调用start方法)
+ 运行状态 (获得cpu时间)
+ 阻塞状态 (暂时让出cpu)
+ 死亡状态 (调用isAlive方法返回false)

