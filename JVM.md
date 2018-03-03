### JVM基本结构

+ PC寄存器
  + 每个线程拥有一个PC寄存器
  + 在线程创建时创建
  + 指向下一条指令的地址
  + 执行本地方法时,PC的值为undefined

+ 方法区
  + 保存装载的类的信息
  + 通常和永久区(Perm)关联在一起

+ Java堆
  + 保存Java对象
  + 所有线程共享
  + GC的主要工作区间
  + 对分代GC来说,堆也是分代的

+ Java栈
  + 线程私有
  + 栈由一系列帧组成(因此也叫帧栈)
  + 帧保存一个方法的局部变量、操作数帧、常量池指针
  + 每一次方法调用创建一个帧,并压帧

### GC

#### GC算法

+ 引用计数 (很难处理循环引用,Java未采用)
+ 标记-清除
+ 标记-压缩
+ 复制算法

#### 分代

+ 新生代 (短命对象) 适合复制算法
+ 老年代 (长命对象) 适合标记清理或标记压缩

#### 可触及性

+ 可触及的 (从根节点可触及到的对象)
+ 可复活的 (引用释放,finallize()未执行)
+ 不可触及的 (finallize()执行完,不可能复活,可以回收)

#### Stop-The-World

Java中的一种全局暂停的对象,所有Java代码停止,native代码可以执行,但不能和JVM交互,多半由于GC引起.

#### GC分类

+ 串行收集器
  + 新生代、老年代使用串行回收
  + 新生代复制算法
  + 老年代标记-压缩
+ 并行收集器
  + 新生代并行
  + 老年代串行
  + 复制算法
  + 多线程(需多核支持)
+ Paraller收集器
  + 新生代复制算法
  + 老年代标记-压缩
  + 更加关注吞吐量
+ CMS收集器
  + 并发标记清除
  + 与用户线程一起执行

### 类装载器

#### class装载验证流程

- 加载 (将类加载到方法区,在堆中生成对应的Class对象)
- 链接
  - 验证 (保证Class流的格式正确)
  - 准备 (分配内存,并为类设置初始值)
  - 解析 (符号引用替换为直接引用)
- 初始化 (执行父类构造器,执行本类构造器)

#### ClassLoader

ClassLoader是一个抽象类,负责类装载过程的加载阶段,它的实例将读入Java字节码并将类装载到JVM中,ClassLoader可以定制.

+ BootStrap ClassLoader (启动/根类加载器) ` $JAVA_HOME$/jre/lib `
+ Extension ClassLoader (扩展类加载器) ` $JAVA_HOME/jre/lib/ext `
+ App/System ClassLoader (应用/系统类加载器) ` classpath `
+ Custom ClassLoader (自定义类加载器)

默认使用双亲委派机制,由上至下加载类

### 锁

+ 偏向锁
+ 轻量级锁
+ 自旋锁
