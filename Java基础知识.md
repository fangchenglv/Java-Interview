![image](pic/排序比较.jpg)
#
# Java基本数据类型总结
### &emsp;&emsp;byte：8位，最大存储数据量是255，存放的数据范围是-128~127之间。  
### &emsp;&emsp;short：16位，最大数据存储量是65536，数据范围是-32768~32767之间。  
### &emsp;&emsp;int：32位，最大数据存储容量是2的32次方减1，数据范围是负的2的31次方到正的2的31次方减1。  
### &emsp;&emsp;long：64位，最大数据存储容量是2的64次方减1，数据范围为负的2的63次方到正的2的63次方减1。  
### &emsp;&emsp;float：32位，数据范围在3.4e-45~1.4e38，直接赋值时必须在数字后加上f或F。  
### &emsp;&emsp;double：64位，数据范围在4.9e-324~1.8e308，赋值时可以加d或D也可以不加。  
### &emsp;&emsp;boolean：只有true和false两个取值。  
### &emsp;&emsp;char：16位，存储Unicode码，用单引号赋值。  

## Boolean占几个字节
### &emsp;&emsp;未精确定义字节。Java语言表达式所操作的boolean值，在编译之后都使用Java虚拟机中的int数据类型来代替，而boolean数组将会被编码成Java虚拟机的byte数组，每个元素boolean元素占8位。

#
# string 和 stringbuffer 和stringBuilder
### &emsp;&emsp;1. string由final修饰，不可修改。操作数量较少的字符串用String，不可修改的字符串；
### &emsp;&emsp;2. 在单线程且操作大量字符串用StringBuilder,速度快，但线程不安全，可修改；
### &emsp;&emsp;3. 在多线程且操作大量字符串用StringBuffer，线程安全，可修改。
### &emsp;&emsp;4. 两个字符串相加，原理是调用stringbuilder进行处理的。扩容为二倍+2
#
# 数组 (Array) 和列表 (ArrayList) 的区别
### &emsp;&emsp;1. Array可以包含基本类型和对象类型，ArrayList只能包含对象类型。
### &emsp;&emsp;2. Array大小是固定的，ArrayList的大小是动态变化的。
### &emsp;&emsp;3. ArrayList提供了更多的方法和特性，比如：addAll()，removeAll()，iterator()等等。
#
# 多态的必要条件
## 1. 重写   
## 2. 重载(参数个数或参数类型不一致)
## 3. 父类引用指向子类对象：先调用子类方法，super调用父类方法  
### 向上转型 
```java 
Person p = new student();  
```
### 方法先找子类再找父类，属性静态绑定  是父类。  
### 通过set、get 方法动态绑定  

#
# Object 通用方法

```java
public native int hashCode()
public boolean equals(Object obj)
protected native Object clone() throws CloneNotSupportedException
public String toString()
public final native Class<?> getClass()
protected void finalize() throws Throwable {}
public final native void notify()
public final native void notifyAll()
public final native void wait(long timeout) throws InterruptedException
public final void wait(long timeout, int nanos) throws InterruptedException
public final void wait() throws InterruptedException
```



#
# 深拷贝和浅拷贝
## 1. 浅拷贝 
### &emsp;&emsp;拷贝对象和原始对象的引用类型引用同一个对象。
## 2. 深拷贝  
### &emsp;&emsp;拷贝对象和原始对象的引用类型引用不同对象。
#
# equals和==的区别
### &emsp;&emsp;对于复合数据类型之间进行equals比较，在没有覆写equals方法的情况下，他们之间的比较还是内存中的存放位置的地址值，跟双等号（==）的结果相同；如果被复写，按照复写的要求来。   
## 1. == 的作用：  
 ### &emsp;&emsp;基本类型：比较的就是值是否相同  
 ### &emsp;&emsp;引用类型：比较的就是地址值是否相同   
## 2. equals 的作用:  
### &emsp;&emsp;引用类型：默认情况下，比较的是地址值，重写该方法后比较对象的成员变量值是否相同  
## 3.代码分析
```java
public static void main(String[] args) {
	Integer a = new Integer(3);
	Integer b = 3;                 
	int c = 3;
	System.out.println(a == b);    
	System.out.println(a == c);   
}
```
### 1. a == b分析  
### &emsp;&emsp;Integer b = 3; 自动调用Integer.valueOf(3) 返回一个Integer的对象。 这个对象是存放到cache中的。 而 Integer a = new Integer(3);这里创建了一个新的对象Integer。所以 a == b 返回的是false
### 2. a == c 分析  
### &emsp;&emsp;一个Integer 与 int比较，先将Integer转换成int类型，再做值比较，所以返回的是true。  
### &emsp;&emsp;java中还有与Integer类似的是Long，它也有一个缓存，在区间[-128,127]范围内获取缓存的值，而Long与long比较的时候先转换成long类型再做值的比较。Double类型，它没有缓存，但是当Double与double比较的时候会先转换成double类型，再做值的比较  
#
# HashMap和ConcurrentHashMap

### &emsp;&emsp;由于HashMap是线程不同步的，虽然处理数据的效率高，但是在多线程的情况下存在着安全问题，因此设计了CurrentHashMap来解决多线程安全问题。  
### &emsp;&emsp;HashMap最多只允许一条记录的键为Null;允许多条记录的值为 Null; HashMap底层数组+链表+红黑树  2倍扩容!
***
## HashMap为什么线程不安全
***
### HashMap的线程不安全主要体现在下面两个方面：
### &emsp;&emsp;1.在JDK1.7中，当并发执行扩容操作时会造成环形链和数据丢失的情况。
### &emsp;&emsp;2.在JDK1.8中，在并发执行put操作时会发生数据覆盖的情况。
---
https://blog.csdn.net/swpu_ocean/article/details/88917958
### &emsp;&emsp;1.7中，在resize时，转移元素的transfer方法，由于采用的头插法和直接覆盖新数组位置，在多线程情况下，有可能造成循环链表，形成死循环。（扩容逆序和环形）线程1正向标记，线程2完成扩容变成逆序，线程1再进入时形成环形链表
### &emsp;&emsp;1.8中，通过尾插法和先链接到指定节点上，最后再覆盖新数组的情况，在并发执行put操作时会发生数据覆盖的情况。
### &emsp;&emsp;1.7和1.8共有的问题是，多线程情况下，线程的操作可能被覆盖或者被忽视，导致数据丢失。其余也有例如size只是采用transient修饰，并没有采用volatile修饰，所以size也可能产生问题。
---
## **HashMap的环**：
---
https://blog.csdn.net/swpu_ocean/article/details/88917958   
头插法逆序

![image](https://km.sankuai.com/api/file/cdn/129208842/129309678)
---
## ConcurrentHashMap底层实现原理，为什么线程安全
---
https://blog.csdn.net/qq_43568704/article/details/108406763
### &emsp;&emsp;在JDK1.7版本中，ConcurrentHashMap维护了一个Segment数组，Segment这个类继承了重入锁ReentrantLock，并且该类里面维护了一个 HashEntry<K,V>[] table数组，在写操作put，remove，扩容的时候，会对Segment加锁，所以仅仅影响这个Segment，不同的Segment还是可以并发的，所以解决了线程的安全问题，同时又采用了分段锁也提升了并发的效率。
### &emsp;&emsp;在JDK1.8版本中，ConcurrentHashMap摒弃了Segment的概念，而是直接用Node数组+链表+红黑树的数据结构来实现，并发控制使用Synchronized和CAS来操作，整个看起来就像是优化过且线程安全的HashMap。（（分段锁、乐观锁CAS、violate关键字）

![image](pic/java集合框架.png)
---
## Hashtable与 HashMap
---
### &emsp;&emsp;Hashtable与 HashMap类似，不同的是:它不允许记录的键或者值为空;它支持线程的同步，即任一时刻只有一个线程能写Hashtable(Synchronize关键字锁住整张表),因此也导致了 Hashtable在写入时会比较慢。
---
## LinkedHashMap HashMap
---
### &emsp;&emsp;LinkedHashMap 是HashMap的一个子类，保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的。按添加顺序遍历，多了前后一个的指针。
---
## TreeMap
---
### &emsp;&emsp;TreeMap实现SortMap接口，能够把它保存的记录根据键排序,默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator 遍历TreeMap时，得到的记录是排过序的。按key排序、红黑树、key必须是同一个类创建的对象!
#
# 红黑树
![image](pic/红黑树.jpeg)
11

#
# 值传递和引用传递？
### &emsp;&emsp;java中只有值传递    
### &emsp;&emsp;值传递是对基本型变量而言的,传递的是该变量的一个副本,改变副本不影响原变量.   
### &emsp;&emsp;引用传递一般是对于对象型变量而言的,传递的是该对象地址的一个副本, 并不是原对象本身 。 所以对引用对象进行操作会同时改变原对象。 
#
# 为什么重写equals还要重写hashcode？   
### &emsp;&emsp;HashMap中的比较key是这样的，先求出key的hashcode(),比较其值是否相等，若相等再比较equals(),若相等则认为他们是相等的。若equals()不相等则认为他们不相等。
### &emsp;&emsp;如果只重写hashcode()不重写equals()方法，当比较equals()时只是看他们是否为同一对象（即进行内存地址的比较）,所以必定要两个方法一起重写。HashMap用来判断key是否相等的方法，其实是调用了HashSet判断加入元素 是否相等。
### &emsp;&emsp;重载hashCode()是为了对同一个key，能得到相同的Hash Code，这样HashMap就可以定位到我们指定的key上。重载equals()是为了向HashMap表明当前对象和key上所保存的对象是相等的，这样我们才真正地获得了这个key所对应的这个键值对。
#
# Java中Iterator用法整理
### &emsp;&emsp;1. 使用next()获得序列中的下一个元素。  
### &emsp;&emsp;2. 使用hasNext()检查序列中是否还有元素。    
### &emsp;&emsp;3. 使用remove()将迭代器新返回的元素删除。    


#
# Java比较器
## 对象之间的比较  
---
### 1. Comparable   
### &emsp;&emsp;包装类实现了comparable接口，可自定义比较方法，重写compareTo()  
### 2. Comparator  
---
## 重写compare  
---
### &emsp;&emsp;1. Comparable的compareTo(T)方法只有1个参数.  
### &emsp;&emsp;2. Comparator接口的compare(T o1, T o2)方法有两个参数, 有@FunctionalInterface接口,所以有Lambda表达式的用法.Comparable只能在类内部实现比较功能,让想让实现比较功能的类自身实现Comparable接口  
### &emsp;&emsp;3. Comparator可以做成比较器类,让比较器类实现Comparator接口.  
#
# 反射：通过对象看到类的结构
### &emsp;&emsp;指在运行状态中,对于任意一个类,都能够知道这个类的所有属性和方法,对于任意一个对象,都能调用它的任意一个方法.这种动态获取信息,以及动态调用对象方法的功能叫java语言的反射机制。
#
# Java动态代理与静态代理的定义与区别
## 1. 静态代理
### &emsp;&emsp;由程序员创建或由特定工具自动生成源代码，再对其编译。在程序运行前，代理类的.class文件就已经存在了。
## 2. 动态代理类
### &emsp;&emsp;在程序运行时，运用反射机制动态创建而成。与静态代理类对照的是动态代理类，动态代理类的字节码在程序运行时由Java反射机制动态生成，无需程序员手工编写它的源代码。动态代理类不仅简化了编程工作，而且提高了软件系统的可扩展性，因为Java 反射机制可以生成任意类型的动态代理类。

#
# 接口和抽象函数、抽象类

### 1. 抽象类来捕捉子类的通用特性，子类需要实现所有方法的实现。
### 2. 包含抽象方法的一定是抽象类，但是抽象类不一定含有抽象方法；
### 3. 抽象类可以包含属性、方法
### 4. 接口是抽象方法的集合，实现类需要实现声明的所有方法
### 5. 接口成员变量默认为public static final，必须赋初值，不能被修改

#
# volatile关键字
## 作用:内存可见性、有序性(防止指令重排) 
## 1. 内存可见性
### &emsp;&emsp;volatile在多处理器开发中保证了共享变量的“ 可见性”。可见性的意思是当一个线程修改一个共享变量时，另外一个线程能读到这个修改的值。(共享内存，私有内存)  
## 2. 有序性
### &emsp;&emsp;volatile之所以能够禁止指令重排，是因为它构建了happens-before语义。所谓happens-before，指的是某一句代码，一定会在另外一句代码前执行完成。
---
## happens-before 原则内容如下
---
- 程序次序规则(Pragram Order Rule)：在一个线程内，按照程序代码顺序，书写在前面的操作先行发生于书写在后面的操作。准确地说应该是控制流顺序而不是程序代码顺序，因为要考虑分支、循环结构。
- 管程锁定规则(Monitor Lock Rule)：一个unlock操作先行发生于后面对同一个锁的lock操作。这里必须强调的是同一个锁，而”后面“是指时间上的先后顺序。

- volatile变量规则(Volatile Variable Rule)：对一个volatile变量的写操作先行发生于后面对这个变量的读取操作，这里的”后面“同样指时间上的先后顺序。

- 线程启动规则(Thread Start Rule)：Thread对象的start()方法先行发生于此线程的每一个动作。

- 线程终于规则(Thread Termination Rule)：线程中的所有操作都先行发生于对此线程的终止检测，我们可以通过Thread.join()方法结束，Thread.isAlive()的返回值等作段检测到线程已经终止执行。

- 线程中断规则(Thread Interruption Rule)：对线程interrupt()方法的调用先行发生于被中断线程的代码检测到中断事件的发生，可以通过Thread.interrupted()方法检测是否有中断发生。

- 对象终结规则(Finalizer Rule)：一个对象初始化完成(构造方法执行完成)先行发生于它的finalize()方法的开始。

- 传递性(Transitivity)：如果操作A先行发生于操作B，操作B先行发生于操作C，那就可以得出操作A先行发生于操作C的结论。


#
# Atomic类的CAS操作

### CAS是英文单词CompareAndSwap的缩写，中文意思是：比较并替换。CAS需要有3个操作数：**内存地址V**，**旧的预期值A**，即将要更新的**目标值B**。CAS指令执行时，当且仅当内存地址V的值与预期值A相等时，将内存地址V的值修改为B，否则就什么都不做。整个比较并替换的操作是一个原子操作。
#
# CAS操作ABA问题：

### 如果在这段期间它的值曾经被改成了B，后来又被改回为A，那CAS操作就会误认为它从来没有被改变过。为了解决这个问题，提供了一个带有标记的原子引用类“AtomicStampedReference”，它可以通过控制变量值的版本来保证CAS的正确性。
#
# synchronized底层原理
### &emsp;&emsp;在HotSpot虚拟机里，对象在堆内存中的存储布局可以划分为三个部分：对象头（Header）、实例数据（Instance Data）和对齐填充（Padding）。
## 1. **对象头**:
### &emsp;&emsp;java中的对象在内存中存储的时候有一个重要的组成部分，就是对象头，对象头中主要包括两部分数据：**类型指针和标记字段**，通过类型指针可以知道该对象是什么类型的，标记字段用来存储对象运行时的数据，其中就有锁对象的指针，它是我们理解Synchronized的关键，因为Synchronized使用到了各种锁。
### &emsp;&emsp;对象头部分包含两类信息。第一类是自身运行时数据，如何哈希码（hashcode）、GC分代年龄、锁状态标志、线程持有的锁、偏向线程ID等，这部分数据官方称它为“Mark Word”。第二类是类型指针，即对象指向它的类型元数据的指针，虚拟机通过它来确定对象是哪个类型的实例。
## 2. synchronized加锁原理
### &emsp;&emsp;标记字段中的锁对象指针指向了一个monitor对象，每个对象都有一个对应的monitor对象，monitor对象是同步工具，线程在执行加了Synchronized的代码段的时候要先去获取对象的monitor,执行完毕释放monitor。此过程是互斥的，一次只能有一个线程获取monitor,只有该线程释放monitor以后其它线程才能再获取它。
![image](https://user-images.githubusercontent.com/30047055/114560239-128e9780-9c9f-11eb-8074-2bf2b5b3cd42.png)
### Sychronized加锁过程  偏向锁--->轻量级--->自旋10次？--->重量级
#
# Synchronized的四种使用方式

### 1. synchronized(this)：当a线程执行到该语句时，锁住该语句所在对象object，其它线程无法访问object中的所有synchronized代码块。
### 2. synchronized(obj)：锁住对象obj，其它线程对obj中的所有synchronized代码块的访问被阻塞。
### 3. synchronized method()：与（1）类似，区别是（1）中线程执行到某方法中的该语句才会获得锁，而对方法加锁则是当方法调用时立刻获得锁。
### 4. synchronized static method()：当线程执行到该语句时，获得锁，所有调用该方法的其它线程阻塞，但是这个类中的其它非static声明的方法可以访问，即使这些方法是用synchronized声明的，但是static声明的方法会被阻塞；注意，这个锁与对象无关。

### 前三种方式加的是对象锁，但是如果（2）中obj是一个class类型的对象，那么加的是类锁，并且锁的范围比（4）还要大；如果该class类型的某个实例对象获得了类锁，那么该class类型的所有实例对象的synchronized代码块均无法访问。
#
# Synchronized和Lock的区别

![image](https://user-images.githubusercontent.com/30047055/114558660-7f089700-9c9d-11eb-9ca1-0237d84e718d.png)

### 1. 首先synchronized是java内置关键字在jvm层面，Lock是个java接口。
### 2. synchronized无法判断是否获取锁的状态，Lock可以判断是否获取到锁，并且可以主动尝试去获取锁。
### 3. synchronized会自动释放锁(a 线程执行完同步代码会释放锁 ；b 线程执行过程中发生异常会释放锁)，Lock需在finally中手工释放锁（unlock()方法释放锁），否则容易造成线程死锁。
### 4. 用synchronized关键字的两个线程1和线程2，如果当前线程1获得锁，线程2线程等待。如果线程1阻塞，线程2则会一直等待下去，而Lock锁就不一定会等待下去，如果尝试获取不到锁，线程可以不用一直等待就结束了。
### 5. synchronized的锁可重入、不可中断、非公平，而Lock锁可重入、可判断、可公平（两者皆可）
### 6. Lock锁适合大量同步的代码的同步问题，synchronized锁适合代码少量的同步问题。
#
# volatile和synchronized区别

### 1. volatile是变量修饰符，而synchronized则作用于一段代码或方法。
### 2. volatile只是在线程内存和“主”内存间同步某个变量的值；而synchronized通过锁定和解锁某个监视器同步所有变量的值, 显然synchronized要比volatile消耗更多资源。
### 3. volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。
### 4. volatile保证数据的可见性，但不能保证原子性；而synchronized可以保证原子性，也可以间接保证可见性，因为它会将私有内存中和公共内存中的数据做同步。
### 5. volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化。
#
# AQS理论的数据结构

### 1. AQS内部有3个对象，一个是state（用于计数器，类似gc的回收计数器），一个是线程标记（当前线程是谁加锁的），一个是阻塞队列。  
### 2. AQS是自旋锁，在等待唤醒的时候，经常会使用自旋的方式，不停地尝试获取锁，直到被其他线程获取成功。  
### 3. AQS使用一个int类型的成员变量state来表示同步状态，当state>0时表示已经获取了锁，当state = 0无锁。它提供了三个方法（getState()、setState(int newState)、compareAndSetState(int expect,int update)）来对同步状态state进行操作，可以确保对state的操作是安全的。
### 4. AQS使用一个Volatile的int类型的成员变量来表示同步状态，通过内置的FIFO队列来完成资源获取的排队工作，通过CAS完成对State值的修改。
### 5. AQS是通过一个CLH队列实现的（CLH锁即Craig, Landin, and Hagersten (CLH) locks，CLH锁是一个自旋锁，能确保无饥饿性，提供先来先服务的公平性。CLH锁也是一种基于链表的可扩展、高性能、公平的自旋锁，申请线程只在本地变量上自旋，它不断轮询前驱的状态，如果发现前驱释放了锁就结束自旋。）
#
# Java多线程实现的四种方式：
### 1. 继承Thread类，重写run方法
### 2. 实现Runnable接口，重写run方法，实现Runnable接口的实现类的实例对象作为Thread构造函数的target
### 3. 通过Callable和FutureTask创建线程
### 4. 通过线程池创建线程
### &emsp;&emsp;前面两种可以归结为一类：无返回值，原因很简单，通过重写run方法，run方式的返回值是void，所以没有办法返回结果。  
### &emsp;&emsp;后面两种可以归结成一类：有返回值，通过Callable接口，就要实现call方法，这个方法的返回值是Object，所以返回的结果可以放在Object对象中。  
## **第一种：继承Thread类，重写该类的run()方法。**  
```java
 class MyThread extends Thread {
      
      private int i = 0;
  
      @Override
      public void run() {
          for (i = 0; i < 100; i++) {
              System.out.println(Thread.currentThread().getName() + " " + i);
          }
     }
 }

  public class ThreadTest {
  
      public static void main(String[] args) {
          for (int i = 0; i < 100; i++) {
              System.out.println(Thread.currentThread().getName() + " " + i);
              if (i == 30) {
                  Thread myThread1 = new MyThread();     // 创建一个新的线程  myThread1  此线程进入新建状态
                  Thread myThread2 = new MyThread();     // 创建一个新的线程 myThread2 此线程进入新建状态
                  myThread1.start();                     // 调用start()方法使得线程进入就绪状态
                 myThread2.start();                     // 调用start()方法使得线程进入就绪状态
             }
         }
     }
 }
```
### &emsp;&emsp;如上所示，继承Thread类，通过重写run()方法定义了一个新的线程类MyThread，其中run()方法的方法体代表了线程需要完成的任务，称之为线程执行体。当创建此线程类对象时一个新的线程得以创建，并进入到线程新建状态。通过调用线程对象引用的start()方法，使得该线程进入到就绪状态，此时此线程并不一定会马上得以执行，这取决于CPU调度时机。
	
## **第二种：实现Runnable接口，并重写该接口的run()方法。**  

### &emsp;&emsp;创建Runnable实现类的实例，并以此实例作为Thread类的target来创建Thread对象，该Thread对象才是真正的线程对象。
```java
  class MyRunnable implements Runnable {
      private int i = 0;
      @Override
      public void run() {
          for (i = 0; i < 100; i++) {
              System.out.println(Thread.currentThread().getName() + " " + i);
          }
      }
 }

  public class ThreadTest {
      public static void main(String[] args) {
          for (int i = 0; i < 100; i++) {
              System.out.println(Thread.currentThread().getName() + " " + i);
              if (i == 30) {
                  Runnable myRunnable = new MyRunnable(); // 创建一个Runnable实现类的对象
                  Thread thread1 = new Thread(myRunnable); // 将myRunnable作为Thread target创建新的线程
                  Thread thread2 = new Thread(myRunnable);
                 thread1.start(); // 调用start()方法使得线程进入就绪状态
                 thread2.start();
             }
         }
     }
 }
```
## **第三种：使用Callable和Future接口创建线程。**  
### a:创建Callable接口的实现类 ，并实现Call方法 
### b:创建Callable实现类的实现，使用FutureTask类包装Callable对象，该FutureTask对象封装了Callable对象的Call方法的返回值  
### c:使用FutureTask对象作为Thread对象的target创建并启动线程  
### d:调用FutureTask对象的get()来获取子线程执行结束的返回值  

## **第四种：线程池。**   
#
# Java中sleep()和wait()的区别
## 1. 区别一
### &emsp;&emsp;sleep()线程控制自身流程。wait()用来线程间通信，使拥有该对象锁的线程等待直到指定时间或notify()。  
### &emsp;&emsp;Thread线程的方法，很简单，让线程休眠。使cpu让出该线程，等待时间过后，线程又重新进入排队恢复运行。  
### &emsp;&emsp;但是Object的方法就有问题了，Object是java的根基类，所有的对象都来源于此。所以wait方法的目的是让出对象锁，并进入线程等待池，等待一段时间或notify的唤醒并同时得到对象锁后运行。一般与notify一起使用。  
## 2. 区别二
### &emsp;&emsp;sleep()方法的线程不会释放对象锁。wait（）方法的线程会释放对象锁。

#
# 为什么要使用线程池
### &emsp;&emsp;1. 减少创建和销毁线程的次数，每个工作线程都可以被重复利用，可执行多个任务。
### &emsp;&emsp;2. 可以根据系统的承受能力，调整线程池中工作线程的数目，放置因为消耗过多的内存，而把服务器累趴下。
## 线程创建的开销
### &emsp;&emsp;对操作系统来说,创建一个线程的代价是十分昂贵的, 需要给它分配内存、列入调度，同时在线程切换的时候还要执行内存换页，CPU 的缓存被清空,切换回来的时候还要重新从内存中读取信息,破坏了数据的局部性。【分配内存、列入调度、内存换页、清空缓存和重新读取】
## 关于内存开销
### &emsp;&emsp;Java线程的线程栈区别于堆，它是不受Java程序控制的，只受系统资源限制。默认一个线程的线程栈大小是1M，别小看这1M的空间，如果每个用户请求都新建线程的话，1024个用户光线程就占用了1个G的内存，如果系统比较大的话，一下子系统资源就不够用了，最后程序就崩溃了。
#
# 核心线程池ThreadPoolExecutor内部参数

### 1. corePoolSize：指定了线程池中的线程数量
### 2. maximumPoolSize：指定了线程池中的最大线程数量
### 3. keepAliveTime：线程池维护线程所允许的空闲时间
### 4. unit: keepAliveTime 的单位。
### 5. workQueue：任务队列，被提交但尚未被执行的任务。
### 6. threadFactory：线程工厂，用于创建线程，一般用默认的即可。
### 7. handler：拒绝策略。当任务太多来不及处理，如何拒绝任务。
#
# 线程池的执行流程

### 1. 如果正在运行的线程数量小于 corePoolSize，那么马上创建线程运行这个任务
### 2. 如果正在运行的线程数量大于或等于 corePoolSize，那么将这个任务放入队列
### 3. 如果这时候队列满了，而且正在运行的线程数量小于 maximumPoolSize，那么还是要创建非核心线程立刻运行这个任务
### 4. 如果队列满了，而且正在运行的线程数量大于或等于 maximumPoolSize，那么线程池会抛出异常RejectExecutionException
#
# 线程池都有哪几种工作队列
### 1. ArrayBlockingQueue：底层是数组，有界队列，如果我们要使用生产者-消费者模式，这是非常好的选择。
### 2. LinkedBlockingQueue：底层是链表，可以当做无界和有界队列来使用，所以大家不要以为它就是无界队列。
### 3. SynchronousQueue：本身不带有空间来存储任何元素，使用上可以选择公平模式和非公平模式。
### 4. PriorityBlockingQueue：无界队列，基于数组，数据结构为二叉堆，数组第一个也是树的根节点总是最小值。

举例 ArrayBlockingQueue 实现并发同步的原理：原理就是读操作和写操作都需要获取到 AQS 独占锁才能进行操作。如果队列为空，这个时候读操作的线程进入到读线程队列排队，等待写线程写入新的元素，然后唤醒读线程队列的第一个等待线程。如果队列已满，这个时候写操作的线程进入到写线程队列排队，等待读线程将队列元素移除腾出空间，然后唤醒写线程队列的第一个等待线程。
#
# 线程池的拒绝策略

### 1. ThreadPoolExecutor.AbortPolicy:丢弃任务并抛出RejectedExecutionException异常。
### 2. ThreadPoolExecutor.DiscardPolicy：丢弃任务，但是不抛出异常。
### 3. ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列最前面的任务，然后重新提交被拒绝的任务
### 4. ThreadPoolExecutor.CallerRunsPolicy：由调用线程（提交任务的线程）处理该任务
#
# 线程池的正确创建方式

### 不能用Executors，newFixed和newSingle，因为队列无限大，容易造成耗尽资源和OOM，newCached和newScheduled最大线程数是Integer.MAX_VALUE，线程创建过多和OOM。应该通过ThreadPoolExecutor手动创建。
#
# 线程提交submit()和execute()有什么区别

### 1. submit()相比于excute()，支持callable接口，也可以获取到任务抛出来的异常
### 2. 可以获取到任务返回结果
### 3. 用submit()方法执行任务，用Future.get()获取异常

# 线程池的线程数量怎么确定
### 1. 一般来说，如果是CPU密集型应用，则线程池大小设置为N+1。
### 2. 一般来说，如果是IO密集型应用，则线程池大小设置为2N+1。
### 3. 在IO优化中，线程等待时间所占比例越高，需要越多线程，线程CPU时间所占比例越高，需要越少线程。这样的估算公式可能更适合：最佳线程数目 = （（线程等待时间+线程CPU时间）/线程CPU时间 ）* CPU数目

## 如何实现一个带优先级的线程池

### 利用priority参数，继承 ThreadPoolExecutor 使用 PriorityBlockingQueue 优先级队列。
#
# ThreadLocal的原理和实现
### ThreadLocal 不是 Thread，是一个线程内部的数据存储类，通过它可以在指定的线程中存储数据，对数据存储后，只有在线程中才可以获取到存储的数据，对于其他线程来说是无法获取到数据。  
### ThreadLocal提供线程本地变量，每个线程拥有本地变量的副本，各个线程之间的变量互不干扰。ThreadLocal实现在多线程环境下去保证变量的安全。


---
## 1、ThreadLoal 变量
---
### &emsp;&emsp;线程局部变量，同一个 ThreadLocal 所包含的对象，在不同的 Thread 中有不同的副本。ThreadLocal 变量通常被private static修饰。当一个线程结束时，它所使用的所有 ThreadLocal 相对的实例副本都可被回收。
---
## 2、ThreadLocal的数据结构
---
### &emsp;&emsp;Thread类中有个变量threadLocals，这个类型为ThreadLocal中的一个内部类ThreadLocalMap，这个类没有实现map接口，就是一个普通的Java类，但是实现的类似map的功能。

![avatar][ThreadLocal]
 
### &emsp;&emsp;每个线程都要自己的一个map，map是一个数组的数据结构存储数据，每个元素是一个Entry，entry的key是threadlocal的引用，也就是当前变量的副本，value就是set的值。

## 3、ThreadLocal的应用场景 

### &emsp;&emsp;数据库连接  
### &emsp;&emsp;session管理：  

## 4、ThreadLocal为什么要使用弱引用和内存泄露问题

### &emsp;&emsp;在ThreadLocal中内存泄漏是指ThreadLocalMap中的Entry中的key为null，而value不为null。因为key为null导致value一直访问不到，而根据可达性分析导致在垃圾回收的时候进行可达性分析的时候,value可达从而不会被回收掉，但是该value永远不能被访问到，这样就存在了内存泄漏。
### &emsp;&emsp;如果 key 是强引用，那么发生 GC 时 ThreadLocalMap 还持有 ThreadLocal 的强引用，会导致 ThreadLocal 不会被回收，从而导致内存泄漏。弱引用 ThreadLocal 不会内存泄漏，对应的 value 在下一次 ThreadLocalMap 调用 set、get、remove 方法时被清除，这算是最优的解决方案。

### &emsp;&emsp;Map中的key为一个threadlocal实例.如果使用强引用，当ThreadLocal对象（假设为ThreadLocal@123456）的引用被回收了，ThreadLocalMap本身依然还持有ThreadLocal@123456的强引用，如果没有手动删除这个key，则ThreadLocal@123456不会被回收，所以只要当前线程不消亡，ThreadLocalMap引用的那些对象就不会被回收，可以认为这导致Entry内存泄漏。


#
# java异常
![image](https://user-images.githubusercontent.com/30047055/114562369-086d9880-9ca1-11eb-8b15-9699baeb5414.png)

## Java 的 I/O 大概可以分成以下几类：

### &emsp;&emsp;磁盘操作：File
### &emsp;&emsp;字节操作：InputStream 和 OutputStream
### &emsp;&emsp;字符操作：Reader 和 Writer
### &emsp;&emsp;对象操作：Serializable
### &emsp;&emsp;网络操作：Socket
### &emsp;&emsp;新的输入/输出：NIO

## 字符流与字节流的区别
### &emsp;&emsp;1. 字节流在操作时本身不会用到缓冲区（内存），是文件本身直接操作的，而字符流在操作时使用了缓冲区，通过缓冲区再操作文件
### &emsp;&emsp;2. 在从字节流转化为字符流时，实际上就是byte[]转化为String时，
### &emsp;&emsp;3. 而在字符流转化为字节流时，实际上是String转化为byte[]时，
#
# Java如何实现序列化,有什么意义?
	
### &emsp;&emsp;1)让类实现Serializable接口,该接口是一个标志性接口,标注该类对象是可被序列
	
### &emsp;&emsp;2)然后使用一个输出流来构造一个对象输出流并通过writeObect(Obejct)方法就可以将实现对象写出
	
### &emsp;&emsp;3)如果需要反序列化,则可以用一个输入流建立对象输入流,然后通过readObeject方法从流中读取对象
	
## 1、作用:
	
### &emsp;&emsp;1)序列化就是一种用来处理对象流的机制,所谓对象流也就是将对象的内容进行流化,可以对流化后的对象进行读写操作,也可以将流化后的对象传输与网络之间;
	
### &emsp;&emsp;2)为了解决对象流读写操作时可能引发的问题(如果不进行序列化,可能会存在数据乱序的问题)
	
### &emsp;&emsp;3）序列化除了能够实现对象的持久化之外，还能够用于对象的深度克隆

## 2、transient

### &emsp;&emsp;transient 关键字可以使一些属性不会被序列化。
### &emsp;&emsp;反序列化时按默认值

### &emsp;&emsp;ArrayList 中存储数据的数组 elementData 是用 transient 修饰的，因为这个数组是动态扩展的，并不是所有的空间都被使用，因此就不需要所有的内容都被序列化。通过重写序列化和反序列化方法，使得可以只序列化数组中有内容的那部分数据。

```java
private transient Object[] elementData;
```