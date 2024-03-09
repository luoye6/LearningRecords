## 1-20题

### 1.JDK和JRE有什么区别?

**答：**

- 首先要了解中文名称
- JDK(Java Development Kit) Java开发工具包
- JRE (Java Runtime Environment) Java运行环境
- JVM (Java virtual machine) Java虚拟机

JDK = JRE +Java开发工具(比如你用的Javac.exe 编译源码 转化成字节码)

JRE = JVM + Java的核心类库(比如awt javaFx(界面) )

### 2.Java的重要特点说几个

**答:**

- 面向对象(OOP)
- 健壮、强类型机制(Java静态语言，像JS,PHP,Python动态语言，不用指明类型，声明的时候简单，但优点也是缺点，所以JS->TS,用泛型会更加安全，在多态的向上传型和向下转型的过程中也会减少类型转化次数)、异常处理机制(try catch finally(可以省略),注意trc catch 块中有return 阻挡不了fianlly执行)、垃圾自动收集(system,gc(),弱引用，软引用，强引用，虚引用之类的问题)
- 跨平台性(指的是编译后的字节码文件是跨平台性的，比如windows->Linux OR mac) 因为操作系统(OS)是在硬件上的第一层软件
- 解释性语言,编译性语言比如:c/c++,解释性语言JS,PHP,Java，解释性语言编译后不能直接被机器执行，需要一个环境(解释器JVM),Java既可以说是解释性又可以说是编译性，解释性主要是跨平台性,编译性主要是Java代码需要编译才能运行,主流说法还是解释性语言。

### 3.Java的基本数据类型有哪些?

- byte、short、char、int、long、float、double、boolean

补充:类,接口类,数组,枚举.注解，字符串都属于引用类型

- 存储位置的区别:
- 如果在方法中定义的基本数据类型，是在栈中存放，因为都听说过方法栈，在方法中定义的引用类型，它的内存地址值放在栈中，而该变量所指向的对象放在堆中，感觉和string为什么是不可变的差不多，底层维护的private final char value[] 单个字符内容可变，但是数组的引用地址不可变。
- 如果是全局变量，那么不管是引用类型还是基本数据类型都放在堆中

### 4.==和equals的区别是什么?

- == 可以判断基本数据类型和引用类型
- == 判断基本数据类型，比较的是值是否相等
- == 判断引用类型，比较的地址是否相等
- equals是Object类中的方法，Object类还有clone()克隆,创建对象的一种方式，toString()全类名+@+哈希值的十六进制,notify()随机唤醒线程配置synchornized使用,wait()线程等待，释放当前锁,hashCode(),getClass()等等。equals只能用于比较引用类型
- 当你想比较两个对象中的内容是否相等，记得重写equals，不然判断的是两个的地址，肯定不同，判断的是对象的内容

### 5.final在Java中有什么引用?

- 修饰基本数据类型，表示常量，比如CACHE_SIZE = 10
- 修饰引用数据类型，比如对象，数组，则该对象,数组本身内容可以修改，但地址的引用不能改变
- 修饰全局变量时必须在定义时，赋初值，当然赋初值有很多位置，比如：定义时候、构造器（构造函数)、代码块,当然final+static 一起的话，构造器就不能进行赋初值了，静态的类加载相关的，构造器是对象实例哦，跟this,super差不多
- 修饰方法，表示不被重写，当然重写肯定是父类和子类,也不必多说，重载是一个类中
- 修饰类，表示不可继承类,比如String
- 别把fianl修饰类，和实例化挂钩，final只是不能继承，不是抽象类的概念哦
- 已经是final类，就没有必要用fianl修饰方法，因为方法重写是在继承的基础只上的
- final+static 效率更高，可以从接口那块看出来，默认常量,public static fianl 编译器底层做了优化

### 6.Math.round、Math.ceil、Math.floor用法

- round可以用+0.5，然后向下取整(小于等于)
- 比如Math.round(-11.3) = -11,Math.round(-11.8)= -12
- floor:向下取整 Math.floor(11.3) = 11;Math.floor(-11.3) = -12;
- ceil: 向上取整 Math.ceil(11.3) = 12; Math.ceil(-11.3) = 11;
- ceil是天花板的意思，floor是地板，应该很容易记忆

### 7.String两种创建对象的区别

- 方法一: 直接赋值: String s = "xiaobaitiao"
- 方法二：调用构造器: String s2 = new String ("xiaobaitiao")
- 方法一过程; 先从常量池查看是否有"xiaobaitiao",如果有，直接指向，如果没有，重新创建，然后指向，S最终指向的是常量池的空间地址 创建0或1个对象
- 方法二过程: 先在堆中创建空间，里面维护了一个value属性，指向常量池的xiaobaitiao空间,如果常量池没有"xiaobaitiao",重新创建，如果有，通过value指向，S2最终指向的是堆中的空间地址 创建1或2个对象
- 补充: intern()方法，会查看常量池是否包含此字符串用equals方法确定，返回的是常量池的地址对象

### 8.抽象类和普通类的区别在哪里?

- 抽象类不能被实例化
- 抽象类可以（可以没有)有抽象方法，没有方法体
- 有抽象方法，一定是抽象类
- abstract只能修饰类和方法
- 抽象类的子类必须实现抽象类中的所有抽象方法，除非子类也是抽象类
- 抽象方法不能使用private,final和静态来修饰，因为与重写相违背

### 9.接口和抽象类有什么区别和联系?

- 联系
- 接口和抽象类都不能被实例化
- 接口和抽象类都阔以包含抽象方法
- 区别
- 接口是可以被一个类实现多个，而抽象类因为Java是单继承机制,只能直接继承一个
- 抽象类可以定义普通成员变量和静态常量，接口中只能定义静态常量，不能定义普通成员变量
- 抽象类，说到底还是一个类，可以与构造器完成初始化操作，接口不能包含构造器
- JDK1.8接口中可以有方法体，因为添加了静态方法和默认方法，JDK1.9添加私有方法

### 10.BIO、NIO、AIO有什么区别?

- BIO是同步阻塞的 (JDK1.4之前)
- NIO同步非阻塞 (JDK1.4)
- AIO异步非阻塞 (JDK1.7)
- 异步和同步的区别:在于调用者，如果是同步，调用者调用接口，需要等到接口返回数据，才做其他工作，如果是异步，调用者调用接口后，直接做其他工作，不用等待接口返回数据。
- 阻塞和非阻塞区别:在于被调用者，如果是阻塞：被调用者做完调用者给的任务后给出反馈，这是阻塞，如果是非阻塞，被调用者直接给出反馈，然后再干活，这是非阻塞。

### 11.什么是反射？反射有哪些作用？反射在Sping中的体现

#### (1): 什么是反射?

- 反射可以在运行时获取到一个类的所有信息，包括(成员变量，成员方法，构造器等)
- 反射可以直接操作类的私有属性
- 反射就是把Java类中的各种成分映射成一个个的Java对象

#### (2): 反射有哪些作用?

- 获取类对应的字节码的对象 对象.getClass() Person.class Class.forName("类的全路径") 第三种是最安全，性能最好，补充第四种方法通过类加载器 getClassLoader().loadClass()
- 反射可以获取一个类的所有信息，比如包名，类名，构造器，方法，成员变量 可以操作私有属性和私有方法

#### (3): 反射在Spring中的体现

- SprignIOC控制反转利用：工厂模式+反射+xml解析(配置文件)
- 利用Class.forName("类的全路径").newInstance
- @Bean注入组件就是利用反射+代理模式
- 从容器中拿取，一般会写getBean("指定类名称")或者是getBean("类名称"，返回的Bean类型)
- 单元测试。JUnit等单元测试框架可以使用反射机制在运行时动态地获取类和方法的信息，实现自动化测试。

### 12.为什么要使用克隆? 怎么实现对象克隆? 克隆有几种方式和它们的区别

#### (1): 为什么要使用克隆?

- 业务逻辑中存在，想要对一个对象进行复制，但又不想更改原有的对象，比如DTO操作中，将POJO中的属性拷贝到DTO中

#### (2): 怎么实现对象克隆?

- 实现Cloneable接口，重写clone方法
- 实现Serializable接口，通过对象的序列化和反序列化实现克隆，属于深拷贝的克隆，对象转换字节流，然后写入对象流，然后从对象流中读取readObject获取对象
- Spring、Apache都提供了BeanUtils来拷贝对象，属于浅拷贝，推荐使用Spring自带的BeanUtils，效率更高

#### (3): 克隆有几种方式和区别

- 浅拷贝： 克隆基本数据类型，当克隆引用类型时，克隆对象的引用类型改变会导致原来的对象也发生改变，因此当克隆存在引用类型的属性时，推荐采用深拷贝。比如Dept部门中有Emp员工的情况
- 深拷贝： 克隆基本数据类型和引用类型，并且互相隔离，互不影响。
- 浅拷贝可以直接采用clone(),或者用BeanUtils
- 深拷贝，可以重写clone()方法，但当属性类型比较多，层级深的时候，不推荐。可以采用第二种方法，序列化和反序列化，当然类要实现Serializable接口。

### 13.常见的运行时异常有哪些?

- NumberFormatException 数字转换异常
- ArrayIndexOutofBoundsException 数组下标越界异常
- NullPointerException 空指针异常
- ArithmeticException 算术逻辑异常
- ClassCastException 类型转换异常

### 14.String,StringBuffer,StringBuilder之间的区别和联系

- 首先String类是final修饰，不可继承，底层维护了private final char value[] 属性，因此是常量，不能修改其引用地址,但是单个字符内容是可以发生改变，其实就是常量池和堆，String修改内容，会导致大量副本残留，因此效率会降低。
- StringBuffer是线程安全的，因为有synchronized修饰，底层用的和StringBuilder一样的AbstractStringBuilder的append。
- StringBuilder是线程不安全的，适合单线程情况下使用，但速度最快。
- 效率对比: StringBuilder>StringBuffer>String

### 15.重写和重载的区别和使用场景

- 重写: 是建立在继承关系上的，子类在继承父类的基础上，可以增加新的功能，使用场景:在不修改原方法的基础上对方法进行扩展和增加，例如：CGLIB实现动态代理(SpringBoot2.x),SpringBoot1.0和Spring5,AOP还是使用的是JDK动态代理，但JDK动态代理有局限性，必须要有接口。但CGLIB不存在这个问题，代理对象无论是赋值给接口还是实现类，这两者都是代理对象的父类。
- 重载:重载是多态的体现，一个类中处理不同类型的参数可以用重载，比如构造器重载，方法重载，

### 16.实例化对象有哪几种方式

- 直接new
- clone()克隆
- 反射机制 Class.forName("类的全路径").newInstance()
- 对象的序列化和反序列化，利用对象流

### 17.类什么时候会被加载

- 创建对象实例(new)
- 创建子类对象实例时，父类也会被加载
- 使用类的静态成员（属性的访问或赋值或方法的调用)
- 静态常量在访问时**不会触发**类的加载机制，常量在常量池，本质上没有直接引用到定义常量的类（静态常量存储在元数据区(JDK1.8)的静态常量池和运行时常量池并列）。
- 当子类引用父类的静态字段，不会触发子类的类加载
- 当通过数组来定义引用类，不会触发该类的类加载机制,数组在编译时不能确定元素类型，只有在运行时才能确定元素类型。

### 18.什么是双亲委派模型？

- 双亲委派模型，就是加载类的时候，先请求其父类加载器去加载，如果父类加载器无法加载类，再尝试自己去加载类。如果都没加载到，就抛出异常。

**好处:**

- 使得 Java 类随着它的类加载器一起具有一种带有优先级的层次关系，从而使得基础类得到统一
- 通过这种层级关可以避免类的重复加载，当父亲已经加载了该类时，就没有必要子ClassLoader再加载一次，避免了多份同样字节码的加载，。
- 安全，避免核心类被修改java核心api中定义类型不会被随意替换，假设通过网络传递一个名为java.lang.Integer的类，通过双亲委托模式传递到启动类加载器，而启动类加载器在核心Java API发现这个名字的类，发现该类已被加载，并不会重新加载网络传递的过来的java.lang.Integer，而直接返回已加载过的Integer.class，这样便可以防止核心API库被随意篡改。

### 19.HashMap和HashTable有什么区别?

- HashMap是JDK1.2引入的，线程不安全
- HashTable是JDK1.0引入的，线程安全
- HashMap中允许键和值为Null，而HashTable不允许
- HashTable直接使用对象的hashcode
- HashMap重新计算hash值（高低16位异或）
- HashMap没有HashTable的contains方法，改为containsKey和containsValue
- HashTable默认容量为11，HashMap为16
- HashTable扩容机制为11乘2+1,HashMap为16乘2
- HashTable继承Dictionary，HashMap继承AbstractMap
- 不建议使用HashTable,在多线程环境下，JDK1.5引入ConcurrentHashMap,在HashMap的基础上增加线程安全性保障

### 20.HashMap的实现原理

- 初始化大小默认16,2倍扩容机制
- 负载因子0.75
- HashMap在JDK1.7中存储结构采用数组+链表。HashMap采取Entry数组来存储key-value，每一个键值对组成了一个Entry实体，Entry类实际上是一个单向的链表结构，它具有next指针，指向下一个Entry实体，以此来解决Hash冲突的问题。
- HashMap在JDK1.8中采用数组+链表+红黑树，当链表长度大于等于8，并且数组长度大于等于64的时候进行树化，如果数组长度不大于64，仅进行正常的数组扩容

### 21.泛型擦除机制

https://blog.csdn.net/weixin_45630258/article/details/121573687

https://www.jianshu.com/p/0a8ef023a804

https://juejin.cn/post/6999797611146248222?searchId=202311082121471322E0011A94ACF1D0E0

https://juejin.cn/post/6993532464622731278?searchId=202311082121471322E0011A94ACF1D0E0

### 22.JDK8的Optional类详细解释

[地址](https://blog.csdn.net/weixin_43888891/article/details/124788806?ops_request_misc=%7B%22request%5Fid%22%3A%22169953185616800188556357%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=169953185616800188556357&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-124788806-null-null.142^v96^pc_search_result_base2&utm_term=optional&spm=1018.2226.3001.4187)

https://blog.csdn.net/weixin_45080272/article/details/128211460

## 21-40题

### 21.说下Java8的Stream流的常用方法

**答:**

- forEach遍历、find、match进行匹配
- reduce进行归约，比如求和，乘，除
- 聚合:max,min,count
- 收集:collect: 1.统计summarizing,counting,averaging 2.groupingby,partitioningby 3.接合:joining 4.归约: reduce 5.归集 toList,toSet,toMap
- 筛选: filter、映射: map 排序: sorted

### 22.说下ConcurrentHashMap原理

**答:**

- ConcurrentHashMap是JDK1.5引入,在HashMap的基础上增加了线程安全的保障。

**总结**

做插入操作时，首先进入乐观锁，先计算得到哈希值，然后，在乐观锁中判断容器是否初始化，如果没初始化则初始化容器，如果已经初始化，则判断该hash位置的节点是否为空，如果为空，则通过CAS操作进行插入，失败则利用锁自旋保证其成功。如果该节点不为空，再判断容器是否在扩容中，如果在扩容，则帮助其扩容。如果没有扩容，则进行最后一步，先加锁，然后判断是链表还是红黑树，如果是链表，找到hash值相同的那个节点(hash冲突)，循环判断这个节点上的链表，决定做覆盖操作还是插入操作，如果是红黑树，那么用红黑树的方式进行插入，如果链表长度大于等于8，数组长度大于等于64，要进行树化。

### 23.Java8开始ConcurrentHashMap,为什么舍弃分段锁？

**通过 JDK 的源码和官方文档看来， 他们认为的弃用分段锁的原因由以下几点：**

- 加入多个分段锁浪费内存空间。
- 生产环境中， map 在放入时竞争同一个锁的概率非常小，分段锁反而会造成更新的操作的长时间等待。
- 为了提高 GC 的效率

### 24.ConcurrentHashMap(JDK1.8)为什么要使用synchronized而不是如ReentranLock这样的可重入锁？

（1）锁的粒度

首先锁的粒度并没有变粗，甚至变得更细了。每当扩容一次，ConcurrentHashMap的并发度就扩大一倍。

（2）Hash冲突

JDK1.7中，ConcurrentHashMap(通过二次hash的方式（Segment -> HashEntry）能够快速的找到查找的元素。在1.8中通过链表加红黑树的形式弥补了put、get时的性能差距。JDK1.8中，在ConcurrentHashmap进行扩容时，其他线程可以通过检测数组中的节点决定是否对这条链表（红黑树）进行扩容，减小了扩容的粒度，提高了扩容的效率。

下面是我对面试中的那个问题的一下看法。

为什么是synchronized，而不是ReentranLock

（1）减少内存开销

假设使用可重入锁来获得同步支持，那么每个节点都需要通过继承AQS来获得同步支持。但并不是每个节点都需要获得同步支持的，只有链表的头节点（红黑树的根节点）需要同步，这无疑带来了巨大内存浪费。

（2）获得JVM的支持

可重入锁毕竟是API这个级别的，后续的性能优化空间很小。synchronized则是JVM直接支持的，JVM能够在运行时作出相应的优化措施：锁粗化、锁消除、锁自旋等等。这就使得synchronized能够随着JDK版本的升级而不改动代码的前提下获得性能上的提升。

- JDK1.7分段数组+HashEntry数组+链表
- JDK1.8Node数组+链表+红黑树

### 25.yml和properties的区别

- yml是一种标记语言，可以跨语言，而properties只适用于SpringBoot项目。
- yml采用key:value 键值对形式,而properties是k-v格式
- yml采用UTF-8，中文不会乱码，但properties通过IO读取，默认ISO-8859-1中文会产生乱码
- properties不保证加载顺序，yml有先后加载顺序。
- 先加载yml,再properties,相同配置properties会覆盖yml的配置。

### 26.类加载器有哪些

Java 中有以下四种类加载器:

![img](https://pic.yupi.icu/5563/202403092042544.png)

1. 引导类加载器 (Bootstrap ClassLoader): 也作根类加载器，它用来加载 Java 的核心类，是用原生代码来实现的，并不继承自 java.lang.ClassLoader（负责加载$JAVA_HOME中jre/lib/rt.jar里所有的class，由C++实现，不是ClassLoader子类）。由于引导类加载器涉及到虚拟机本地实现细节，开发者无法直接获取到启动类加载器的引用，所以不允许直接通过引用进行操作。
2. 扩展类加载器（extensions class loader）：它负责加载JRE的扩展目录，lib/ext或者由java.ext.dirs系统属性指定的目录中的JAR包的类。由Java语言实现，父类加载器为null。
3. 系统类加载器（system class loader）：被称为系统（也称为应用）类加载器，它负责在JVM启动时加载来自Java命令的-classpath选项、java.class.path系统属性，或者CLASSPATH换将变量所指定的JAR包和类路径。程序可以通过ClassLoader的静态方法getSystemClassLoader()来获取系统类加载器。如果没有特别指定，则用户自定义的类加载器都以此类加载器作为父加载器。由Java语言实现，父类加载器为ExtClassLoader。

需要注意的是，引导类加载器是最顶层的类加载器，它不会受到类加载器层级的限制。因此，如果引导类加载器无法加载所需的类，则可以尝试使用其他类加载器进行加载。

**补充：**

- **怎么打破双亲委派机制？为什么要打破？有哪些场景打破了？**

自定义加载器的话，需要继承 ClassLoader 。如果我们不想打破双亲委派模型，就重写 ClassLoader 类中的 findClass() 方法即可，无法被父类加载器加载的类最终会通过这个方法被加载。但是，如果想打破双亲委派模型则需要重写 loadClass() 方法。

为什么是重写 loadClass() 方法打破双亲委派模型呢？双亲委派模型的执行流程已经解释了：

类加载器在进行类加载的时候，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成（调用父加载器 loadClass()方法来加载类）。

我们比较熟悉的 Tomcat 服务器为了能够优先加载 Web 应用目录下的类，然后再加载其他目录下的类，就自定义了类加载器 WebAppClassLoader 来打破双亲委托机制。这也是 Tomcat 下 Web 应用之间的类实现隔离的具体原理。

**场景：**

**Tomcat 的类加载器的层次结构如下：**

![img](https://pic.yupi.icu/5563/202403092042436.webp)

![img](https://pic.yupi.icu/5563/202403092042310.png)

**Tomcat为什么要打破？**

Tomcat打破双亲委派机制的目的其实很简单，我们知道web容器可能是需要部署多个应用程序的，这在早期的部署架构中也经常见到，如上图。但是假设不同的应用程序可能会同时依赖第三方类库的不同版本。我们通过上文在了解类加载机制的时候知道它是要确保唯一性的，但是总不能要求同一个类库在web容器中只有一份吧？所以Tomcat就需要保证每个应用程序的类库都是相互隔离并独立的，这也是它为什么打破双亲委派机制的主要目的。

**Tomcat是如何打破双亲委派机制的呢？**

从上文中，我们不难看出，真正实现web应用程序之间的类加载器相互隔离独立的是WebAppClassLoader类加载器。它为什么可以隔离每个web应用程序呢？原因就是它打破了"双亲委派"的机制，如果收到类加载的请求，它会先尝试自己去加载，如果找不到在交给父加载器去加载，这么做的目的就是为了优先加载Web应用程序自己定义的类来实现web应用程序相互隔离独立的。

1.重写findClass方法，先在应用本地目录下查找类，如果在本地目录没有找到，委派父加载器去查找，如果都没有找到，抛出异常。

2.重写loadClass方法:

1. 先在本地缓存中查找该类是否已经加载过，如果加载过就返回缓存中的。
2. 如果没有加载过，委托给AppClassLoader是否加载过，如果加载过就返回。
3. 如果AppClassLoader也没加载过，委托给ExtClassLoader去加载，这么做的目的就是：

1. 1. 防止应用自己的类库覆盖了核心类库，因为WebAppClassLoader需要打破双亲委托机制，假如应用里自定义了一个叫java.lang.String的类，如果先加载这个类，就会覆盖核心类库的java.lang.String，所以说它会优先尝试用ExtClassLoader去加载，因为ExtClassLoader加载不到同样也会委托给BootstrapClassLoader去加载，也就避免了覆盖了核心类库的问题。

1. 如果ExtClassLoader也没有查找到，说明核心类库中没有这个类，那么就在本地应用目录下查找此类并加载。
2. 如果本地应用目录下还有没有这个类，那么肯定不是应用自己定义的类，那么就由AppClassLoader去加载。

1. 1. 这里是通过Class.forName()调用AppClassLoader类加载器的，因为Class.forName()的默认加载器就是AppClassLoader。

1. 如果上述都没有找到，那么只能抛出ClassNotFoundException了。

**2.类加载的流程**

1.加载 加载指的是将类的class文件读入到内存，并为之创建一个java.lang.Class对象，也就是说，当程序中使用任何类时，系统都会为之建立一个java.lang.Class对象。

- 方法区存放类的字节码二进制数据
- 堆区存放类的class对象类的加载由类加载器完成，类加载器通常由JVM提供，这些类加载器也是前面所有程序运行的基础，JVM提供的这些类加载器通常被称为系统类加载器。除此之外，开发者可以通过继承ClassLoader基类来创建自己的类加载器。通过使用不同的类加载器，可以从不同来源加载类的二进制数据，通常有如下几种来源。
- 从本地文件系统加载class文件，这是前面绝大部分示例程序的类加载方式。
- 从JAR包加载class文件，这种方式也是很常见的，前面介绍JDBC编程时用到的数据库驱动类就放在JAR文件中，JVM可以从JAR文件中直接加载该class文件。
- 通过网络加载class文件。
- 把一个Java源文件动态编译，并执行加载。
- 类加载器通常无须等到“首次使用”该类时才加载该类，Java虚拟机规范允许系统预先加载某些类。

2.链接 当类被加载之后，系统为之生成一个对应的Class对象，接着将会进入连接阶段，连接阶段负责把类的二进制数据合并到JRE中。类连接又可分为如下3个阶段。

1)验证：验证阶段用于检验被加载的类是否有正确的内部结构，并和其他类协调一致。Java是相对C++语言是安全的语言，例如它有C++不具有的数组越界的检查。这本身就是对自身安全的一种保护。验证阶段是Java非常重要的一个阶段，它会直接的保证应用是否会被恶意入侵的一道重要的防线，越是严谨的验证机制越安全。验证的目的在于确保Class文件的字节流中包含信息符合当前虚拟机要求，不会危害虚拟机自身安全。其主要包括四种验证，文件格式验证，元数据验证，字节码验证，符号引用验证。

- 四种验证做进一步说明：
- 文件格式验证：主要验证字节流是否符合Class文件格式规范，并且能被当前的虚拟机加载处理。例如：主，次版本号是否在当前虚拟机处理的范围之内。常量池中是否有不被支持的常量类型。指向常量的中的索引值是否存在不存在的常量或不符合类型的常量。（是否以魔数0Xcafebabe开头)
- 元数据验证：对字节码描述的信息进行语义的分析，分析是否符合java的语言语法的规范。
- 字节码验证：最重要的验证环节，分析数据流和控制，确定语义是合法的，符合逻辑的。主要的针对元数据验证后对方法体的验证。保证类方法在运行时不会有危害出现。
- 符号引用验证：主要是针对符号引用转换为直接引用的时候，是会延伸到第三解析阶段，主要去确定访问类型等涉及到引用的情况，主要是要保证引用一定会被访问到，不会出现类等无法访问的问题。

2)准备：类准备阶段负责为类的静态变量分配内存，并设置默认初始值。

3)解析：将类的二进制数据中的符号引用替换成直接引用。说明一下：符号引用：符号引用是以一组符号来描述所引用的目标，符号可以是任何的字面形式的字面量，只要不会出现冲突能够定位到就行。布局和内存无关。直接引用：是指向目标的指针，偏移量或者能够直接定位的句柄。该引用是和内存中的布局有关的，并且一定加载进来的。

3.初始化 初始化是为类的静态变量赋予正确的初始值，准备阶段和初始化阶段看似有点矛盾，其实是不矛盾的，如果类中有语句：private static int a = 10，它的执行过程是这样的，首先字节码文件被加载到内存后，先进行链接的验证这一步骤，验证通过后准备阶段，给a分配内存，因为变量a是static的，所以此时a等于int类型的默认初始值0，即a=0,然后到解析（后面在说），到初始化这一步骤时，才把a的真正的值10赋给a,此时a=10。



### 27.类加载机制(三种)

- JVM的类加载机制主要有如下3种。
- 全盘负责：所谓全盘负责，就是当一个类加载器负责加载某个Class时，该Class所依赖和引用其他Class也将由该类加载器负责载入，除非显示使用另外一个类加载器来载入。
- 双亲委派：所谓的双亲委派，则是先让父类加载器试图加载该Class，只有在父类加载器无法加载该类时才尝试从自己的类路径中加载该类。通俗的讲，就是某个特定的类加载器在接到加载类的请求时，首先将加载任务委托给父加载器，依次递归，如果父加载器可以完成类加载任务，就成功返回；只有父加载器无法完成此加载任务时，才自己去加载。
- 缓存机制。缓存机制将会保证所有加载过的Class都会被缓存，当程序中需要使用某个Class时，类加载器先从缓存区中搜寻该Class，只有当缓存区中不存在该Class对象时，系统才会读取该类对应的二进制数据，并将其转换成Class对象，存入缓冲区中。这就是为什么修改了Class后，必须重新启动JVM，程序所做的修改才会生效的原因。

### 28.类加载机制(什么时候类会被加载)

1. 创建类的实例，也就是new一个对象
2. 访问某个类或接口的静态变量，或者对该静态变量赋值
3. 调用类的静态方法
4. 反射（Class.forName("com.lyj.load")）
5. 初始化一个类的子类（会首先初始化子类的父类）
6. JVM启动时标明的启动类，即文件名和类名相同的那个类

除此之外，下面几种情形需要特别指出：

对于一个final类型的静态变量，如果该变量的值在编译时就可以确定下来，那么这个变量相当于“宏变量”。Java编译器会在编译时直接把这个变量出现的地方替换成它的值，因此即使程序使用该静态变量，也不会导致该类的初始化。反之，如果final类型的静态Field的值不能在编译时确定下来，则必须等到运行时才可以确定该变量的值，如果通过该类来访问它的静态变量，则会导致该类被初始化。

### 29.HashMap 的长度为什么是 2 的幂次方

为了能让 HashMap 存取高效，尽量较少碰撞，也就是要尽量把数据分配均匀。我们上面也讲到了过了，Hash 值的范围值-2147483648 到 2147483647，前后加起来大概 40 亿的映射空间，只要哈希函数映射得比较均匀松散，一般应用是很难出现碰撞的。但问题是一个 40 亿长度的数组，内存是放不下的。所以这个散列值是不能直接拿来用的。用之前还要先做对数组的长度取模运算，得到的余数才能用来要存放的位置也就是对应的数组下标。这个数组下标的计算方法是“ (n - 1) & hash”。（n 代表数组长度）。这也就解释了 HashMap 的长度为什么是 2 的幂次方。

**这个算法应该如何设计呢？**

我们首先可能会想到采用%取余的操作来实现。但是，重点来了：**“取余(%)操作中如果除数是 2 的幂次则等价于与其除数减一的与(&)操作（也就是说 hash%length==hash&(length-1)的前提是 length 是 2 的 n 次方；）。”** 并且 **采用二进制位操作 &，相对于%能够提高运算效率，这就解释了 HashMap 的长度为什么是 2 的幂次方。**

### 30.LinkedList 为什么不能实现 RandomAccess 接口？

RandomAccess 是一个标记接口，用来表明实现该接口的类支持随机访问（即可以通过索引快速访问元素）。由于 LinkedList 底层数据结构是链表，内存地址不连续，只能通过指针来定位，不支持随机快速访问，所以不能实现 RandomAccess 接口。

### 31.HashMap的put 方法执行过程

HashMap 只提供了 put 用于添加元素，putVal 方法只是给 put 方法调用的一个方法，并没有提供给用户使用。

**对 putVal 方法添加元素的分析如下：**

1. 如果定位到的数组位置没有元素 就直接插入。
2. 如果定位到的数组位置有元素就和要插入的 key 比较，如果 key 相同就直接覆盖，如果 key 不相同，就判断 p 是否是一个树节点，如果是就调用e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value)将元素添加进入。如果不是就遍历链表插入(插入的是链表尾部)。

![img](https://pic.yupi.icu/5563/202403092042438.png)

------

### 32.ConcurrentHashMap的put方法

1. 根据 key 计算出 hashcode 。
2. 判断是否需要进行初始化。
3. 即为当前 key 定位出的 Node，如果为空表示当前位置可以写入数据，利用 CAS 尝试写入，失败则自旋保证成功。
4. 如果当前位置的 hashcode == MOVED == -1,则需要进行扩容。
5. 如果都不满足，则利用 synchronized 锁写入数据。
6. 如果数量大于 TREEIFY_THRESHOLD 则要执行树化方法，在 treeifyBin 中会首先判断当前数组长度 ≥64 时才会将链表转换为红黑树。

```java
public V put(K key, V value) {
    return putVal(key, value, false);
}

/** Implementation for put and putIfAbsent */
final V putVal(K key, V value, boolean onlyIfAbsent) {
    // key 和 value 不能为空
    if (key == null || value == null) throw new NullPointerException();
    int hash = spread(key.hashCode());
    int binCount = 0;
    for (Node<K,V>[] tab = table;;) {
        // f = 目标位置元素
        Node<K,V> f; int n, i, fh;// fh 后面存放目标位置的元素 hash 值
        if (tab == null || (n = tab.length) == 0)
            // 数组桶为空，初始化数组桶（自旋+CAS)
            tab = initTable();
        else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
            // 桶内为空，CAS 放入，不加锁，成功了就直接 break 跳出
            if (casTabAt(tab, i, null,new Node<K,V>(hash, key, value, null)))
                break;  // no lock when adding to empty bin
        }
        else if ((fh = f.hash) == MOVED)
            tab = helpTransfer(tab, f);
        else {
            V oldVal = null;
            // 使用 synchronized 加锁加入节点
            synchronized (f) {
                if (tabAt(tab, i) == f) {
                    // 说明是链表
                    if (fh >= 0) {
                        binCount = 1;
                        // 循环加入新的或者覆盖节点
                        for (Node<K,V> e = f;; ++binCount) {
                            K ek;
                            if (e.hash == hash &&
                                ((ek = e.key) == key ||
                                 (ek != null && key.equals(ek)))) {
                                oldVal = e.val;
                                if (!onlyIfAbsent)
                                    e.val = value;
                                break;
                            }
                            Node<K,V> pred = e;
                            if ((e = e.next) == null) {
                                pred.next = new Node<K,V>(hash, key,
                                                          value, null);
                                break;
                            }
                        }
                    }
                    else if (f instanceof TreeBin) {
                        // 红黑树
                        Node<K,V> p;
                        binCount = 2;
                        if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                       value)) != null) {
                            oldVal = p.val;
                            if (!onlyIfAbsent)
                                p.val = value;
                        }
                    }
                }
            }
            if (binCount != 0) {
                if (binCount >= TREEIFY_THRESHOLD)
                    treeifyBin(tab, i);
                if (oldVal != null)
                    return oldVal;
                break;
            }
        }
    }
    addCount(1L, binCount);
    return null;
}
```

### 33.hashmap子类知道多少?

![img](https://pic.yupi.icu/5563/202403092042647.png)

### 34.对于多态的理解是什么？

#### 1.父类的引用指向子类的对象

子类重写父类的方法：子类可以继承父类的方法，并对其进行重写。当通过父类的引用调用这个方法时，实际执行的是子类重写后的方法。

比如Person person = new Student Person是父类 Student ，都有一个工作的方法，student重写工作方法，比如上学。

**2.接口的引用指向实现类的对象**

\1) List list = new ArrayList(); 2) ArrayList list=  new ArrayList()

在第一种情况下，无法使用ArrayList特有的方法，因为声明的是一个List类型的变量，只能使用List接口中定义的方法。而在第二种情况下，声明了一个ArrayList类型的变量，可以使用ArrayList特有的方法。

#### 3.方法的重载

方法的重载：方法重载指的是在同一个类中定义多个同名但参数列表不同的方法。在调用这个方法时，编译器会根据参数的类型和数量来确定具体调用哪个方法。

**4.方法重写**

4.1子类中的方法必须与父类中的方法具有相同的名称。

4.2子类中的方法必须具有相同的参数列表（参数的类型、顺序和数量）。

4.3子类中的方法的返回类型可以是父类方法返回类型的子类型（也称为协变返回类型）。

4.4子类中的方法不能缩小父类方法的访问权限（即不能将父类方法的访问权限由public改为private),不能更加严格，但是可以扩大访问权限。

**5.向上转型和向下转型**

5.1向上转型（Upcasting）：将一个子类对象转换为父类类型。这是一个隐式的转型过程，不需要显式地进行类型转换。

5.2向下转型（Downcasting）：将一个父类对象转换为子类类型。这是一个显式的转型过程，需要使用强制类型转换符进行类型转换。需要注意进行类型检查，避免类型转换异常。

### 35.对于static变量的理解？static变量分配内存的时候发生在哪个环节?

static变量是一种静态变量，它在程序执行期间保持不变。它被所有类的对象所共享，这意味着无论创建多少个类的对象，静态变量只有**一个副本**。

静态变量在类定义时被声明，而不是在对象创建时分配内存。内存分配发生在程序加载时，在程序的生命周期内只会**分配一次**。

静态变量是在类加载阶段的第二阶段连接（第二阶段准备阶段）从方法区取出内存，进行静态变量的默认初始化，真正被赋值的时候是第三阶段初始化。

### 36.JDK1.8对于方法区的实现是？（元空间）元空间还会存放什么东西？

- 为什么永久代(HotSpot虚拟机)要改成方法区(元数据区)？

因为永久代的垃圾回收条件苛刻，所以容易导致内存不足，而转为元空间，使用本地内存，极大减少了 OOM 异常，毕竟，现在电脑的内存足以支持 Java 的允许。

- **只有 Hotspot 才有永久代**。BEA JRockit、IBMJ9 等来说，是不存在永久代的概念的。
- 必要性：1.为永久代设置空间大小是很难确定的 2.对永久代进行调优是很困难的。

**元数据区(MetaSpace)存放**：1.运行时常量池和静态常量池(字符串常量池放在堆中) 2.类元信息(类的二进制字节码文件,类的名称，父类，接口，成员变量，方法，静态变量，常量，常量池的符号引用和指向其他类的引用) 3.方法元信息（方法访问修饰符，返回类型，参数类型，异常类型）



### 37.为什么String要设计成Final类？

**只有当字符串是不可变的，字符串池才有可能实现**

字符串池的实现可以在运行时节约很多heap空间，因为不同的字符串变量都指向池中的同一个字符串。但如果字符串是可变的，那么String interning将不能实现(注：String interning是指对不同的字符串仅仅只保存一个，即不会保存多个相同的字符串。)，因为这样的话，如果变量改变了它的值，那么其它指向这个值的变量的值也会一起改变。

**如果字符串是可变的，那么会引起很严重的安全问题**

譬如，数据库的用户名、密码都是以字符串的形式传入来获得数据库的连接，或者在socket编程中，主机名和端口都是以字符串的形式传入。因为字符串是不可变的，所以它的值是不可改变的，否则黑客们可以钻到空子，改变字符串指向的对象的值，造成安全漏洞。

**因为字符串是不可变的，所以是多线程安全的**

同一个字符串实例可以被多个线程共享。这样便不用因为线程安全问题而使用同步。字符串自己便是线程安全的。

**类加载器要用到字符串，不可变性提供了安全性,以便正确的类被加载**

譬如你想加载java.sql.Connection类，而这个值被改成了myhacked.Connection，那么会对你的数据库造成不可知的破坏。

**作为Map的key，提高了访问效率**

因为字符串是不可变的，所以在它创建的时候hashcode就被缓存了，不需要重新计算。这就使得字符串很适合作为Map中的键，字符串的处理速度要快过其它的键对象。这就是HashMap中的键往往都使用字符串。因为Map使用得也是非常之多，所以一举两得

### 38.什么情况下会导致元数据区溢出？

**1.加载的类的数量过多（比如CGLIB不断生成代理类）**

**2.类的大小过大，包含大量静态属性或常量**

**3.元数据区的参数设置不当，内存给的太小**



**堆在什么时候会内存溢出？**

**堆内存存在大量对象，且对象都有被引用，创建对象不合理。**

**栈在什么时候会溢出？**

**递归调用没有写好结束条件，导致栈溢出。**



**频繁FullGC的几种原因？**

**拓展：新生代占比堆的三分之一，而老年代占比堆的三分之二。1:2**

**新生代占比：Eden8,S0,S1 各占1份，8:1:1**

**① 系统承载高并发请求，或者处理数据量过大，导致YoungGC很频繁，而且每次YoungGC过后存活对象太多，内存分配不合理，Survivor区域过小，导致对象频繁进入老年代，频繁触发FullGC**

**② 系统一次性加载过多数据进内存，搞出来很多大对象，导致频繁有大对象进入老年代，然后频繁触发FullGC**

**③ 系统发生了内存泄漏，创建大量的对象，始终无法回收，一直占用在老年代里，必然频繁触发FullGC**

**④ Metaspace 因为加载类过多触发FullGC**

**⑤ 误调用 System.gc() 触发 FullGC**



https://www.cnblogs.com/rerise/p/16202433.html

https://blog.csdn.net/weixin_43421392/article/details/127941263



### 39.Java的常量优化机制

#### 数值常量优化机制

1、给一个变量赋值，如果“=”号右边是常量的表达式没有一个变量，那么就会在编译阶段计算该表达式的结果。

2、然后判断该表达式的结果是否在左边类型所表示范围内。

3、如果在，那么就赋值成功，如果不在，那么就赋值失败。

```java
byte b1  = 1 + 2;
System.out.println(b1);
// 输出结果 3
```

这个就是典型的常量优化机制，1 和 2 都是常量，编译时可以明显确定常量结果，所以直接把 1 和 2 的结果赋值给 b1 了。(和直接赋值 3 是一个意思)

#### 编译器对 String 的常量也有优化机制

```java
String s1 = "abc";
String s2 = "a"+"b"+"c";
System.out.println(s1 == s2); // true
String a = "a1";
String b = "a" + 1; // 常量+基础数据类型
System.out.println((a == b));  //result = true

String a = "atrue";
String b = "a" + true;
System.out.println((a == b));  //result = true

String a = "a3.4";
String b = "a" + 3.4;
System.out.println((a == b));  //result = true
String s1 = "ab";
String s2 = "abc";
String s3 = s1 + "c"; // 变量+常量
System.out.println(s3 == s2); // false
final String s1 = "ab";
String s2 = "abc";
String s3 = s1 + "c"; //变量相加
System.out.println(s2 == s3); // true
```

### 40.Comparable和Comparator接口区别和对比使用场景

**1、Comparable 接口简介**

**Comparable<T>接口：位于java.lang包下，需要重写public int compareTo(T o);**

**Comparable 是排序接口。若一个类实现了Comparable接口，就意味着“该类支持排序”。**

**“实现Comparable接口的类的对象”可以用作“有序映射(如TreeMap)”中的键或“有序集合(TreeSet)”中的元素，而不需要指定比较器。**

**它强行将实现它的每一个类的对象进行整体排序-----称为该类的自然排序，实现此接口的对象列表和数组可以用Collections.sort(),和Arrays.sort()进行自动排序；**

**接口中通过compareTo（T o）来比较x和y的大小。若返回负数，意味着x比y小；返回零，意味着x等于y；返回正数，意味着x大于y。**

**2、Comparator 接口简介**

**Comparator<T>接口：位于java.util包下，需要重写int compare(T o1, T o2);**

**Comparator 是比较器接口。我们若需要控制某个类的次序，而该类本身不支持排序(即没有实现Comparable接口)；**

**它是针对一些本身没有比较能力的对象（数组）为它们实现比较的功能，所以它叫做比较器，是一个外部的东西，通过它定义比较的方式，再传到Collection.sort()和Arrays.sort()中对目标排序，而且通过自身的方法compare()定义比较的内容和结果的升降序；**

**int compare(T o1, T o2) 和上面的x.compareTo(y)类似，定义排序规则后返回正数，零和负数分别代表大于，等于和小于。**

**总结：**

**1.只要涉及对象大小的比较，就可以实现这两个接口的任意一个。**

Comparable接口：通过对象调用重写后的compareTo（）

Comparator接口：通过对象调用重写后的compare()

**2.如果要调用sort进行排序**

Comparable接口：自然排序

Comparator接口：定制排序-->属于临时性的排序规则

**3.compare和compareTo方法的对比**

compareTo：是拿调这个方法的对象和形参比大小

compare：直接让两个形参比大小



### 41.装饰器模式和代理模式的区别？

装饰器模式和代理模式的区别

代理是全权代理，目标根本不对外，全部由代理类来完成；装饰是增强，是辅助，目标仍然可以自行对外提供服务，装饰器只起增强作用。

装饰器模式强调的是：增强、新增行为；代理模式强调的是：对代理的对象施加控制，但不对对象本身的功能进行增强。

装饰器模式：生效的对象还是原本的对象；代理模式：生效的是新的对象（代理对象）

### 42.Java有哪些东西是在编译期做的，哪些是在运行期做的？

### 编译期（Compile Time）:

1. **语法检查：** 在编译期，Java编译器会检查源代码的语法，确保其符合Java语法规范。
2. **类型检查：** 编译器会进行类型检查，确保变量、表达式等的类型匹配，避免类型错误。
3. **生成字节码：** Java源代码被编译成字节码，这是一种中间代码，不是本地机器代码。
4. **优化：** 编译器可以对生成的字节码进行一些优化，以提高程序的性能。
5. **错误检查：** 编译器会检查潜在的编译时错误，例如未声明的变量、未捕获的异常等。

### 运行期（Runtime）:

1. **类加载：** 在运行期，Java虚拟机（JVM）负责加载字节码并将其转换成机器码。这个过程包括加载、链接（验证、准备、解析）、初始化等步骤。
2. **内存分配：** 运行期时，JVM负责在内存中为对象分配空间，管理堆内存和栈内存。
3. **字节码解释/编译执行：** JVM可以通过解释执行字节码，也可以选择将其编译成本地机器码执行（即时编译）。
4. **垃圾回收：** 运行期时，JVM负责垃圾回收，自动释放不再使用的内存。
5. **异常处理：** 运行期负责捕获和处理异常，包括运行时异常和编译时异常。
6. **动态链接：** 在运行期，Java支持动态链接，可以在运行时链接和加载一些类。

### 43.Java的通配符和上下界介绍一下？

无限定通配符:?

上限通配符：？extends 类型

下限通配符：？super 类型

### 44.为什么转换成红黑树 链表长度要>8，为什么 不> 7,为什么阈值为6要退化成链表？

当长度为 8 的时候，概率仅为 0.00000006。这是一个小于千万分之一的概率，通常我们的 Map 里面是不会存储这么多的数据的，所以通常情况下，并不会发生从链表向红黑树的转换。

红黑树转链表的阈值为6，主要是因为，如果也将该阈值设置于8，那么当hash碰撞在8时，会反生链表和红黑树的不停相互激荡转换，白白浪费资源。中间有个差值7可以防止链表和树之间的频繁转换，
假设一下：

如果设计成链表个数超过8则链表转换成树结构，链表个数小于8则树结构转换成链表，如果HashMap不停的插入，删除元素，链表个数在8左右徘徊，就会频繁的发生红黑树转链表，链表转红黑树，效率会很低下。

### 45.LinkedHashMap 底层实现？用在什么场景？

LinkedHashMap 是 Java 提供的一个集合类，它继承自 HashMap，并在 HashMap 基础上维护一条双向链表，使得具备如下特性:

1. 支持遍历时会按照插入顺序有序进行迭代。
2. 支持按照元素访问顺序排序,适用于封装 LRU 缓存工具。
3. 因为内部使用双向链表维护各个节点，所以遍历时的效率和元素个数成正比，相较于和容量成正比的 HashMap 来说，迭代效率会高很多。

场景：插入顺序和取出顺序相一致，可以用于封装LRU缓存。

### 46.TreeMap的特点，使用的场景

TreeMap是一个基于key有序的key value 散列表。

- map根据其键的自然顺序排序，或者根据map创建时提供的Comparator排序
- 不是线程安全的
- key 不可以存入null
- 底层是基于红黑树实现的

### 47.TreeMap底层是如何保证元素不重复的？

在TreeMap中，判断key是否重复的依据是根据 comparaTo 是否为0，如果为0，TreeMap 就认为key是重复的。

### 48.单例模式的写法注意点？

注意点：

1、用 Volatile 修饰实例，保证有序性和可见性，主存同步。

2、构造器私有化，防止外部直接去调用构造器进行实例化。

3、双重检索模式，两次检查实例是否创建完毕。

### 49.两个集合，如果取出相同的元素？

方法一：使用循环遍历

```java
Set<Integer> set1 = new HashSet<>(Arrays.asList(1, 2, 3, 4, 5));
Set<Integer> set2 = new HashSet<>(Arrays.asList(3, 4, 5, 6, 7));

Set<Integer> commonElements = new HashSet<>();
for (Integer element : set1) {
    if (set2.contains(element)) {
        commonElements.add(element);
    }
}
System.out.println(commonElements);
```

方法二：使用retainAll方法

```java
Set<Integer> set1 = new HashSet<>(Arrays.asList(1, 2, 3, 4, 5));
Set<Integer> set2 = new HashSet<>(Arrays.asList(3, 4, 5, 6, 7));

Set<Integer> commonElements = new HashSet<>(set1);
commonElements.retainAll(set2);
System.out.println(commonElements);
```

方法三：使用Java 8的stream和filter

```java
Set<Integer> set1 = new HashSet<>(Arrays.asList(1, 2, 3, 4, 5));
Set<Integer> set2 = new HashSet<>(Arrays.asList(3, 4, 5, 6, 7));

Set<Integer> commonElements = set1.stream()
        .filter(set2::contains)
        .collect(Collectors.toSet());
System.out.println(commonElements);
```

### 50.为什么 HashMap 的负载因子是0.75？底层算法是什么？

https://blog.csdn.net/qq_51824841/article/details/130215943

https://blog.csdn.net/qq_40299519/article/details/131423476

https://baijiahao.baidu.com/s?id=1656137152537394906&wfr=spider&for=pc

### 51.如何实现接口的幂等性？

https://zhuanlan.zhihu.com/p/507706427

https://blog.csdn.net/Tyson0314/article/details/128526868

### 52.说说你了解的设计模式和使用场景？

#### 装饰器模式：允许向一个现有的对象添加新的功能，同时不改变其结构，例如IO的缓冲流和普通的文件输入流。

#### 适配器模式：主要用于接口互不兼容的类的协调工作，你可以将其联想到我们日常经常使用的电源适配器。

1.InputStreamReader 和 OutputStreamWriter 就是两个适配器(Adapter)， 同时，它们两个也是字节流和字符流之间的桥梁。InputStreamReader 使用 StreamDecoder （流解码器）对字节进行解码，实现字节流到字符流的转换， OutputStreamWriter 使用StreamEncoder（流编码器）对字符进行编码，实现字符流到字节流的转换。

2.Spring Cloud GateWay 的网关过滤器在进行优先级排序的时候，使用到了适配器模式，底层用了适配器进行处理，将GloabalFilter适配成GateWayFilterAdapter，而GateWayFilterAdapter又实现了 GatewayFilter的接口，因此GlobalFilter适配了GateWayFilter。

```java
private static List<GatewayFilter> loadFilters(List<GlobalFilter> filters) {
	return filters.stream().map(filter -> {
	    // 把GlobalFilter适配成了GatewayFilterAdapter
		GatewayFilterAdapter gatewayFilter = new GatewayFilterAdapter(filter);
		// 并且这里计算顺序的时候，只是从Ordered接口取的 order注解不起作用
		if (filter instanceof Ordered) {
			int order = ((Ordered) filter).getOrder();
			return new OrderedGatewayFilter(gatewayFilter, order);
		}
		return gatewayFilter;
	}).collect(Collectors.toList());
}
// GatewayFilterAdapter 就实现了GatewayFilter接口
private static class GatewayFilterAdapter implements GatewayFilter {
	private final GlobalFilter delegate;
	GatewayFilterAdapter(GlobalFilter delegate) {
		this.delegate = delegate;
	}
	@Override
	public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
		return this.delegate.filter(exchange, chain);
	}
}
```

3.FutureTask 类使用了适配器模式，将 Runnable适配成Callable。

```java
public FutureTask(Runnable runnable, V result) {
    // 调用 Executors 类的 callable 方法
    this.callable = Executors.callable(runnable, result);
    this.state = NEW;
}
```

**适配器模式和装饰器模式有什么区别呢？**

**装饰器模式** 更侧重于动态地增强原始类的功能，装饰器类需要跟原始类继承相同的抽象类或者实现相同的接口。并且，装饰器模式支持对原始类嵌套使用多个装饰器。

**适配器模式** 更侧重于让接口不兼容而不能交互的类可以一起工作，当我们调用适配器对应的方法时，适配器内部会调用适配者类或者和适配类相关的类的方法，这个过程透明的。就比如说 StreamDecoder （流解码器）和StreamEncoder（流编码器）就是分别基于 InputStream 和 OutputStream 来获取 FileChannel对象并调用对应的 read 方法和 write 方法进行字节数据的读取和写入。

**适配器和适配者两者不需要继承相同的抽象类或者实现相同的接口。**

#### 工厂模式：用于创建对象。

工厂模式的好处：1.对象的创建和使用解耦2.隐藏创建对象的细节，用户只关注对象的使用。3.实现开闭原则，对修改关闭，对拓展开放。·

使用场景：

1.NIO 中大量用到了工厂模式，比如 Files 类的 newInputStream 方法用于创建 InputStream 对象（静态工厂）、 Paths 类的 get 方法创建 Path 对象（静态工厂）、ZipFileSystem 类（sun.nio包下的类，属于 java.nio 相关的一些内部实现）的 getPath 的方法创建 Path 对象（简单工厂）。

2.数据库连接池

3.Spring的IOC 工厂模式+XML解析+反射

#### 观察者模式：可以将一个对象的状态变化通知给多个观察者，而不需要让这些观察者直接依赖于该对象。

1.Spring 的事件监听机制

Spring事件机制是观察者模式的实现。ApplicationContext中事件处理是由ApplicationEvent类和ApplicationListener接口来提供的。如果一个Bean实现了ApplicationListener接口，并且已经发布到容器中去，每次ApplicationContext发布一个ApplicationEvent事件，这个Bean就会接到通知。ApplicationEvent 事件的发布需要显示触发，要么 Spring 触发，要么我们编码触发。spring内置事件由spring触发。我们先来看一下，如何自定义spring事件，并使其被监听和发布。

### 场景题：文件拷贝需要注意的点（大文件，缓存，出现异常如何解决）