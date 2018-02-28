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

### JavaEE语法

