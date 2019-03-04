## 基础
### 1.JDK和JRE的区别
- JDK：JAVA Development Kit简称，Java开发工具包，提供了Java的开发环境。
- JRE：Java Runtime Environment 简称，Java运行环境，为Java的运行提供了环境。

具体来说，JDK其实包含了JRE，同时还包含了编译Java源码的编译器Javac，还包含了很多Java程序调试和分析的工具，简单来说，如果需要运行Java程序，只需安装JRE即可，如果需要编写Java程序，需要安装JDK。
### 2.== 和 equals的区别

#### **==的作用**
- 基本类型：比较的是值是否相同。
- 引用类型：比较的是引用是否相同。

#### **euqals的作用** 
比较的都是值是否相同。

代码示例：
```Java
String x="String";
String y="String";
String z=new String("String");
System.out.println(x==y);// true
System.out.println(x==z);//false
System.out.println(x.equals(y));//true
System.out.println(x.equals(z));//true
```
代码解读：因为x和y指向了同一个引用，所以== 也是true，而new String()操作则是重新开辟了内存空间，所以==结果为false，而equals比较的一直是值，所以结果都为true。
### 3.两个对象的hashCode()相同，则equals()也一定为true对吗？
不对，两个对象的hashCode()相同，equals()不一定为true。
首先分析hashCode源码：
```Java
public int hashCode(){
    int h=hash;//hash值
    if(h==0&&value.length>0){
        char val[]=value;
        for(int i=0;i<val.length;i++){
            h=31*h+val[i];
            hash=h;
        }
    }
    return hash;
}
```
面试可能会问到为什么这里的系数是31。
- 31是质数，相乘之后冲突小，同时质数不能太大，不然会乘法溢出。
- 31可由i*31=(i<<5)-1来表示，虚拟机支持这一操作。

典型的hash算法，输入一个字符串，计算它的hash值，熟悉hash算法的都知道，会存在冲突，所以有两个对象的hashCode()相同，但是不一定值（内容）相同。
所以二者的关系可以总结为：
1. 若两个对象相同，则他们的hashCode一定相同。
2. 若两个对象的hashCode相同，他们并不一定equals。

### 4.final在Java中有什么作用？finally呢，finalize
- final修饰的类叫最终类，该类不能被继承。
- final修饰的方法不能被重写。
- final修饰的变量叫做常量，常量必须初始化，初始化之后，值就不能被修改。

finally作为异常处理的一部分，只能用在try/catch语句之中，并且附带了一个语句块，表示这段语句最终一定会被执行，经常被用在需要释放资源的情况下使用。

finalize： 是 Object 类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法。

### 5.String属于基础数据类型吗？
Java中基础数据类型有8种，分别为：int，short，long，double,float,boolean,char,byte.
String属于对象类型，注意数组也属于对象。

### 6.Java中操作字符串都有哪些类，他们之间有什么区别？
操作字符串的类有：String,StringBuffer,StringBuilder

都是final类，都不允许被继承。
1. String和StringBuffer，StringBuilder的区别在于 String声明的是不可变的对象，每次操作都会生成新的String对象，然后将指针指向新的String对象。而StringBuffer，StringBuilder可以在原有对象的基础上进行操作，所以在经常改变字符串内容的情况下最好不要用String。
2. StringBuffer和StringBuilder最大的区别在于前者是相乘安全的，而后者是非线程安全的，但StringBuilder的性能要高于StringBuffer。因为StringBuffer在StringBuilder的基础上加了synchronized修饰，造成了一定程度上性能的下降。单线程环境下推荐使用StringBuilder，多线程环境下推荐使用StringBuffer。

### 7.String str="i"与String str=new String("i")一样吗？

不一样，因为内存的分配方式不一样。str="i"的方式，Java虚拟机会将其分配到常量池中，而String str=new String("i")则会被分到堆内存中。

### 8. String类常用的方法都有哪些？
indexOf() 返回指定字符的索引
charAt() 返回指定索引处的字符
replace() 字符串替换
trim() 去掉字符串两端的空白
split() 分割字符串，返回一个分割后的字符数组
getBytes() 返回字符串的byte类型数组。
length() 返回字符串长度
toLowerCase() 将字符串转成小写字母
toUpperCase() 将字符串转成大写字符
substring() 截取字符串
equals() 字符串比较。

### 9.关于抽象类
1.抽象类不一定非要有抽象方法
例如：
```
abstract class Cat {
    public static void sayHi() {
        System. out. println("hi~");
    }
}
```
上面代码，抽象类并没有抽象方法但完全可以正常运行，大师不可以直接实例化。

对于普通类来说，普通类不能包含抽象方法，抽象类可以包含抽象方法。抽象类不能直接实例化，普通类可以直接实例化。

抽象类不能用final修饰，定义抽象类就是为了让其他类继承，如果定义为final该类就不能被继承，这样彼此就会产生矛盾，所以final不能修饰抽象类。

### 10 接口与抽象类的区别。
- 默认方法实现：抽象类可以有默认的方法实现，接口不能有默认的方法实现。
- 实现：抽象类的子类使用extends来继承，接口必须使用implements来实现接口。
- 构造函数:抽象类可以有构造函数，接口不能有
- main方法：抽象类可以有main方法，并且我们能运行它，接口不能有main方法。
- 实现数量：类可以实现很多个接口，但是只能继承一个抽象类。
- 访问修饰符：接口中的方法默认使用public修饰，抽象类中的方法可以是任意访问修饰符。

### 11 Java中IO流的分类
按功能来分：输入流，输出流
按类型来分：字节流和字符流 二者区别：字节流按8位传输以字节为单位输入输出数据，字符流按16位传输以字符位单位输入输出数据。

### 12 BIO、NIO、AIO有什么区别
BIO：Block IO同步阻塞式IO，就是我们平常使用的传统IO，他的特点是模式简单，使用方便，并发处理能力低。
NIO：New IO 同步非阻塞IO，是传统IO的升级，客户端和服务器通过Channel(通道)通讯，实现了多路复用。
AIO：Asynchronous IO 是NIO的升级，也叫NIO2，实现了异步非阻塞IO，异步IO的操作基于事件和回调机制。

### 13 Files的常用方法都有哪些？
Files.exists()判断文件路径是否存在
Files.createFile()创建文件
Files.createDirectory()创建文件夹
Files.delete()删除一个文件或者目录
Files.copy()复制文件
Files.move()移动文件
Files.size()查看文件个数
Files.read()读取文件
Files.write()写入文件

## 容器

### 14.Java容器都有哪些？
Java容器分为Collection 和Map两大类，其下又有很多子类，如下所示：
1. Collection
- List
  - ArrayList
  - LinkedList
  - Vector
  - Stack
- Set
  - HashSet
  - LinkedHashSet
  - TreeSet
2.  Map
 - HashMap
  - LinkedHashMap 
 - TreeMap
 - ConcurrentHashMap
 - Hashtable

### 15. Collection和Collections有什么区别？
- Collection 是一个集合接口，它提供了对集合对象进行基本操作的通用接口方法，所有集合都是它的子类，比如List，set等。
- Collections是一个包装类，包含了很多静态方法，不能被实例化，就像一个工具类，比如提供排序的方法：Collections.sort(List);

### 16.List,Set,Map之间的区别

List,Set,Map的区别主要体现在两个方面：元素是否有序，是否允许元素重复。
三者之间的区别，如下表：
![image](https://i.loli.net/2019/03/02/5c7a450024cf4.jpg)

### 17. HashMap和Hashtable区别
- 存储： HashMap允许key和value为null，而Hashtable不允许
- 线程安全： Hashtable是线程安全的，但HashMap是非线程安全的

在Hashtable的类注释中可以看到，Hashtable是保留类不建议使用，推荐在单线程环境下使用HashMap替代，如果需要多线程，则使用ConcurrentHashMap替代。

### 18.HashMap与TreeMap
对于在Map中插入，删除，定位一个元素这类的操作，HashMap是最好的选择，因为相对而言，HashMap的插入会很快，但如果需要对一个key集合进行有序的遍历，那么TreeMap是更好地选择。TreeMap的底层是红黑树。

### 19.HashMap实现原理
HashMap基于Hash算法实现，通过put(key,value)存储，当传入key时，HashMap会根据key.hashCode()计算出hash值，根据hash值将value保存在bucket里。当计算出的hash值相同时，我们称为hash冲突，HashMap的做法是用链表和红黑树存储相同hash值的value,当hash冲突的个数比较少时，使用链表否则使用红黑树。

### 20.HashSet实现原理
HashSet是基于HashMap实现的，HashSet底层使用HashMap来保存所有元素，因此Hashset的实现比较简单，相关HashSet的操作，基本上都是直接调用底层HashMap的相关方法来完成，HashSet不允许重复的值。

### 21 ArrayList和LinkedList的区别是什么？

- 数据结构实现：ArrayList是动态数组的数据结构实现，而LinkedList是双向链表的数据结构实现。
- 随机访问效率：ArrayList比LinkedList在随机访问的时候效率要高，因为LinkedList是线性的数据存储方式，所以需要移动指针从前往后依次查找。
- 增加和删除效率：在非首尾的增加和删除操作，LinkedList要比ArrayList效率要高，因为ArrayList增加操作要影响数组内的其他数据的下标。
综合来说，在需要频繁读取集合中的元素时，更推荐使用ArrayList，而在插入和删除操作较多时，更推荐使用LinkedList.

### 22 如何实现数组和List之间的转换？
- 数组转List 使用Arrays.asList(array)进行转换。
- List转数组：使用List自带的toArray()方法。
```Java
// list to array
List<String> list = new ArrayList<String>();
list. add("王磊");
list. add("的博客");
list. toArray();
// array to list
String[] array = new String[]{"王磊","的博客"};
Arrays. asList(array);
```
### 23.ArrayList 和Vector的区别是什么？

- 线程安全：Vector 使用了Synchronized来实现线程同步，是线程安全的，而ArrayList是非线程安全的。
- 性能： ArrayList在性能方面要优于Vector。
- 扩容：ArrayLsit和Vector都会根据实际的需要动态的调整容量，只不过在Vector扩容每次会增加1倍，而ArrayList只会增加50%

### 24 Array和ArrayList有何区别？
- Array可以存储基本数据类型和对象，ArrayList只能存储对象。
- Array是指定固定大小的，而ArrayList大小是自动扩展的。
- Array内置方法没有ArrayList多，比如addAll,removeAll,iteration等方法只有ArrayList有。

### 25 在Queue中poll()和remove()有什么区别？
相同点：都是返回第一个元素，并在队列中删除返回的对象。

不同点：如果没有元素poll()会返回null，而remove()会直接抛出NoSuchElementException异常。
```
Queue<String> queue = new LinkedList<String>();
queue. offer("string"); // add
System. out. println(queue. poll());
System. out. println(queue. remove());//抛异常
System. out. println(queue. size());
```

### 26. 哪些集合类是线程安全的？


Vector、Hashtable、Stack都是线程安全的，而像HashMap则是非线程安全的，不过在jdk1.5之后，随着JUC并发包的出现，他们也有了自己对应的线程安全类，比如HashMap对应的线程安全类就是ConcurrentHashMap。

### 27. 迭代器Iterator是什么？
Iterator接口提供遍历任何Collection的接口。我们可以从一个Collection中使用迭代器方法来获取迭代器实例。迭代器取代了Java集合框架中的Enumeration，迭代器允许调用者在迭代过程中移除元素。
使用代码如下：
```Java
List<String>list=new ArrayList<>();
Iterator<String>it=list.iterator();
while(it.hasNext()){
    String obj=it.next();
    System.out.println(obj);
}
```
Iterator的特点是更加安全，因为它可以确保，在当前遍历的集合元素被更改的时候，就会抛出ConcurrentModificationException异常。

### 28.Iterator和ListIterator有什么区别？

- Iterator可以遍历Set和List集合，而LsitIterator只能遍历List。
- Iterator只能单向遍历，而ListIterator可以双向遍历。
- ListIterator从Iterator接口继承，然后添加了一些额外的功能，比如添加一个元素，替换一个元素，获取前面或后面元素的索引位置。

### 29.怎么确保一个集合不能被修改？
可以使用Collections.unmodifiableCollection(Collection c)方法来创建一个只读集合，这样改变集合的操作都会抛出Java.lang.UnsupportedOperationException异常。
示例代码：
```
List<String> list = new ArrayList<>();
list. add("x");
Collection<String> clist = Collections. unmodifiableCollection(list);
clist. add("y"); // 运行时此行报错
System. out. println(list. size());
```
## 多线程
### 30.并行与并发有什么区别？
- 并行：一个处理器同时处理多个任务。
- 并发：多个处理器或多核处理器同时处理多个不同的任务。

### 31.线程和进程区别？
- 进程是资源分配的基本单位，它是程序执行时的一个实例。程序运行时系统就会创建一个进程，并为它分配资源，然后把该进程放入进程就绪队列，进程调度器选中它的时候就会为它分配CPU时间，程序开始真正运行。
- 线程是程序执行时的最小单位，它是进程的一个执行流，是CPU调度和分派的基本单位，一个进程可以由很多个线程组成，线程间共享进程的所有资源，每个线程有自己的堆栈和局部变量。线程由CPU独立调度执行，在多CPU环境下就允许多个线程同时运行。同样多线程也可以实现并发操作，每个请求分配一个线程来处理。
- 进程有自己的独立地址空间，每启动一个进程，系统就会为它分配地址空间，建立数据表来维护代码段、堆栈段和数据段，这种操作非常昂贵。而线程是共享进程中的数据的，使用相同的地址空间，因此CPU切换一个线程的花费要远比进程要小很多，同时创建一个线程的开销也比进程要小很多。
- 线程之间的通信更方便，同一进程下的线程共享全局变量、静态变量等数据，而进程之间的通信需要以通信的方式（IPC）进行，不过如何处理好同步与互斥是编写多线程程序的难点。
- 多进程程序更加健壮，多线程只要有一个线程死掉，整个进程也死掉了，而一个进程死掉并不会对另外一个进程造成影响，因为进程有自己独立的地址空间。

### 32.守护线程是什么？
守护线程是运行在后台的一种特殊进程，
**它独立于控制终端并且周期性的执行某种任务或等待处理某些发生的事件。**
在Java中垃圾回收线程就是特殊的守护线程。

### 33.创建线程的方式
创建线程的三种方式：
- 继承Thread并重写run方法
- 实现Runnable接口
- 实现Callable接口

### 34. Runnable和callable有什么区别？
runnable没有返回值，callable可以拿到有返回值，callable可以看做是runnable的补充。

### 35.线程有哪些状态？
- NEW 尚未启动
- RUNNABLE 正在执行中
- BLOCKED 阻塞的（被同步锁或者IO锁阻塞）
- WAITING 永久等待状态
- TIMED_WAITING 等待指定的事件重新被唤醒的状态
- TERMINATED 执行完成

### 36. sleep()和wait()有什么区别？
类的不同：sleep来自Thread类，而wait来自Object类
释放锁： sleep不释放锁，wait释放锁
用法不同： sleep时间到会自动恢复，wait可以使用notify/notifyAll()直接唤醒。

### 37.notify和notifyAll有啥区别？
notifyAll()会唤醒所有的线程，notify()只会唤醒一个线程。notifyAll()调用后，会将全部线程由等待池转移到锁池，然后参与锁的竞争。竞争成功则继续执行，如果不成功则留在锁池等待被释放后再次参与竞争。而notify()只会唤醒一个线程，具体唤醒哪一个线程，由虚拟机控制。

### 38.线程的run和start有什么区别？

start方法用于启动线程，run方法用于执行线程的运行时代码，run可以重复调用，而start只能调用一次。

### 39.线程池相关问题
**1. 什么是线程池？**

顾名思义就是实现创建若干个可以执行的线程放入一个(容器)池中，需要的时候从池中获取线程不用自行创建，使用完毕不需要销毁线程，而是放入池中，从而减少创建和销毁线程对象的开销。线程池也是一种多线程处理形式，处理过程中将任务添加到队列，然后在创建线程后自动启动这些任务。

**2. 线程池创建的方式**
- newSingleThreadExecutor()：它的特点在于工作线程数目被限制为1，操作一个无界的工作队列，所以它保证了所有任务的都是被顺序执行，最多会有一个任务处于活动状态，并且不允许使用者改动线程池实例，因此可以避免其改变线程数目
- newCachedThreadPool()：它是一种用来处理大量短时间工作任务的线程池，具有几个鲜明特点：它会试图缓存线程并重用，当无缓存线程可用时，就会创建新的工作线程；如果线程闲置的时间超过 60 秒，则被终止并移出缓存；长时间闲置时，这种线程池，不会消耗什么资源。其内部使用 SynchronousQueue 作为工作队列；
- newFixedThreadPool(int nThreads)：重用指定数目（nThreads）的线程，其背后使用的是无界的工作队列，任何时候最多有 nThreads 个工作线程是活动的。这意味着，如果任务数量超过了活动队列数目，将在工作队列中等待空闲线程出现；如果有工作线程退出，将会有新的工作线程被创建，以补足指定的数目 nThreads；
- newSingleThreadScheduledExecutor()：创建单线程池，返回 ScheduledExecutorService，可以进行定时或周期性的工作调度；
- newScheduledThreadPool(int corePoolSize)：和newSingleThreadScheduledExecutor()类似，创建的是个 ScheduledExecutorService，可以进行定时或周期性的工作调度，区别在于单一工作线程还是多个工作线程；
- newWorkStealingPool(int parallelism)：这是一个经常被人忽略的线程池，Java 8 才加入这个创建方法，其内部会构建ForkJoinPool，利用Work-Stealing算法，并行地处理任务，不保证处理顺序；
- ThreadPoolExecutor()：是最原始的线程池创建，上面1-3创建方式都是对ThreadPoolExecutor的封装。

**3.线程池的基本组成**

线程管理器(ThreadPool)：用于创建并管理线程池，包括创建线程，销毁线程池，添加新任务等。

工作线程(PollWorker) 线程池中的线程，在没有任务时，处于等待状态，可以循环的执行任务。

任务接口(task) 每个任务必须实现的接口，以供工作线程调度任务的执行，它主要规定了任务的入口，任务执行完成的收尾工作，任务的执行状态等。

任务队列(taskQueue)用于存放没有处理的任务，提供一种缓冲机制。

**4.线程池的好处**

1.重用存在的线程池，较少对象的创建以及消亡的开销，性能好。

2.可有效地控制最大并发线程数，提高系统资源利用率，同时避免过多的资源竞争，避免阻塞。

3.可以提供定期执行，定时执行，单线程，并发控制等功能。

**5.线程池中 submit() 和 execute() 方法有什么区别？**

execute()只能执行Runnable类型的任务。

submit() 可以执行Runnable和Callable类型的任务。

Callable类型的任务可以获取执行的返回值，而Runnable执行无返回值。

### 40.Java程序中怎么保证多线程的运行安全？

1.使用安全类，比如java.util.concurrent的类。

2.使用自动锁 synchronized

3.使用手动锁Lock

手动锁代码如下：
```
Lock lock=new ReentrantLock();//
lock.lock();
try{
    System.out.println("获得锁");
}catch(Exception e){
    //TODO handle exception
}finally{
    System.out.println("释放锁");
    lock.unlock();
}
```

### 41.多线程中synchronized锁升级的原理是什么？
synchronized 锁升级原理：在锁对象的对象头里有一个thread id 字段，在第一次访问的时候，thread id为空，jvm让其持有偏向锁，并将thread id 设置为其线程id，再次进入的时候会先判断thread id是否与其线程id一致，如果一致则可以直接使用此对象，如果不一致，则升级偏向锁为轻量级锁，通过自旋循环一定次数来获取锁，执行一定次数后，如果还没有正常获取到要使用的对象，此时就会把锁从轻量级升级为重量级锁，此过程就构成了synchronized锁的升级。

锁升级的目的：锁升级是为了降低锁带来的性能消耗，在Java6之后优化synchronized的实现方式，使用了偏向锁升级为轻量级锁再升级为重量级锁的方式，从而减低锁带来的性能消耗。

### 42.什么是死锁？

当线程A持有独占锁a，并尝试去获取独占锁b的同时，线程B持有独占锁b，并尝试获取独占锁a的情况下，就会发生AB两个线程由于互相持有对方需要的锁，而发生的阻塞现象，我们称为死锁。

### 43.怎么防止死锁？

- 尽量使用tryLock(Long timeout,TimeUnit unit)的方法(ReentrantLock,ReentrantReadWriteLock)设置超时时间，超时可以退出防止死锁。
- 尽量使用java.util.concurrent并发类代替自己手写锁.
- 尽量降低锁的使用粒度，尽量不要几个功能共用同一把锁。
- 尽量减少同步的代码块。

### 44.ThreadLocal是什么？有哪些使用场景？
ThreadLocal为每个使用该变量的线程提供独立的变量副本，所以每个线程都可以独立的改变自己的副本，而不会影响其他线程所对应的副本。
ThreadLocal的经典使用场景是数据库连接和session管理等。

### 45. synchronized底层实现原理
synchronized是由一对monitorenter/monitorexit指令实现的，monitor对象是同步的基本实现单元。在java6之前，monitor的实现是依靠操作系统内部的互斥锁，因为需要进行用户态到内核态的切换，所以同步操作时一个无差别的重量级操作，性能也很低。但在java6的时候，Java虚拟机做出了改进，提供了三种不同的monitor实现，即偏向锁，轻量级锁，和重量级锁，大大改进了其性能。

### 46. synchronized 和volatile的区别是什么？

- volatile 是变量修饰符;synchronized是修饰类、方法、代码段。
- volatile 仅能实现变量的修改可见性，不能保证原子性；而synchronized则可以保证变量的修改可见性和原子性。
- volatile不会造成线程的阻塞，synchronized可能会造成线程的阻塞。
- 性能方面，synchronized可以防止多个线程同时执行一段代码，会影响程序执行效率。而volatile在某些情况下，性能要优于synchronized。但volatile无法取代synchronized。
-  volatile标记的变量不会被编译器优化，synchronized变量可以被编译器优化。

### 47 synchronized与Lock有什么区别？
- synchronized 可以给类，方法，代码块加锁；而lock只能给代码块加锁。
- synchronized不需要手动获取锁和释放锁，使用简单，发生异常会自动释放锁，不会造成死锁；而lock需要自己加锁和释放锁，如果使用不当没有unlock()去释放锁就会造成死锁。
- 通过Lock可以知道有没有成功获取锁，而synchronized却无法办到。

### 48. synchronized和ReentrantLock区别是什么？
synchronized早起的实现比较低效，对比ReentrantLock，大多数场景性能都相差较大，但是在Java6对synchronized进行了很多的改进。主要区别如下：
- ReentrantLock使用起来比较灵活，但是必须有释放锁的配合动作；
- ReentrantLock必须手动获取与释放锁，而synchronized可用于修饰方法，代码块等。

### 49 atomic原理
atomic主要利用CAS(Compare and swap)和volatile和native方法来保证原子操作，从而避免synchronized的高开销，执行效率大为提升。

## 反射

### 50.什么是反射？
反射是在运行状态中，对于任意一个雷，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性，这种动态获取的信息以及动态调用对象的方法的功能称为Java语言的反射机制。

### 51.什么是Java序列化？什么情况下需要序列化？
Java序列化为了保存各种对象在内存中的状态，并且可以把保存的对象状态在读出来。以下情况需要Java序列化：
- 想把内存中的对象状态保存到一个文件中或者数据库中。
- 想用套接字在网络上传送对象的时候。
- 想通过远程方法调用传输对象的时候。

### 52 动态代理是什么？有哪些应用？

动态代理是运行时动态生成代理类。
动态代理的应用有spring aop，hibernate数据查询、测试框架的后端mock，rpc，Java注解对象获取等。

### 53 怎么实现动态代理
Jdk原生动态代理和cglib动态代理。JDK原生动态代理是基于接口实现的，而cglib是基于继承当前类的子类实现的。

## 对象拷贝

### 54.为什么要使用克隆？
克隆的对象可能包含一些已经修改过的属性，而new出来的对象的属性都还是初始化时候的值，所以当需要一个新的对象来保存当前对象的状态，就靠克隆的方法。

### 55.如何实现对象克隆
- 实现cloneable接口并重写Object类中的clone()方法。
- 实现Serializable接口，通过对象的序列化和反序列化实现克隆，可以实现真正的深度克隆。

### 56.深拷贝和浅拷贝的区别是什么？

- 浅拷贝：当对象被复制时，只复制它本身和其中包含的值类型的成员变量。而引用类型的成员对象并没有复制。
- 深拷贝：除对象本身被复制外，对象所包含的所有成员变量也将复制。

## JavaWeb

### 57. JSP和servlet有什么区别
JSP是servlet技术的扩展，本质上就是servlet的简易方式。servlet和JSP最大的不同的在于servlet的应用逻辑是在Java文件中，并且完全从表示层中的html里分离开来，而JSP的情况是Java和html可以组合成一个扩展名为JSP的文件，JSP侧重于视图，servlet主要用于控制逻辑。

### 58. session和cookie有什么区别？
- 存储位置不同：session存储在服务器端，cookie存储在浏览器端。
- 安全性不同：cookie安全性一般，在浏览器存储，可以被伪造和修改。
- 容量和个数限制：cookie有容量限制，每个站点下的cookie也有个数限制。
- 存储的多样性：session可以存储在Redis中、数据库中，应用程序中，而cookie只能存储在浏览器中。

### 59.session工作原理
session的工作原理是客户端登录完成之后，服务器会创建对应的session，session创建完之后，会把session的id发送给客户端，客户端再存储到浏览器中。这样客户端每次访问服务器时，都会带着session id，服务器拿到session id之后，在内存找到与之对应的session这样就可以正常工作了。

### 60.如果客户端禁止cookie能实现session还能用吗？
可以用，session只是依赖cookie存储session id，如果cookie被禁用了，可以使用url中添加session id的方式保证session能正常使用。

### 61.springmvc和struts的区别是什么？

拦截级别：struts2是类级别的拦截，springmvc是方法级别的拦截。

数据独立性： springmvc的方法之间基本上独立的，独享request和response数据，请求数据通过参数获取，处理结果通过ModelMap交回给框架，方法之间不共享变量，而struts2虽然方法之间也是独立的，但其所有action变量是共享的，这不会影响程序运行，但会带来一定麻烦

拦截机制：struts2有自己的拦截机制，springmvc用的是独立的aop方式，这样导致struts2的配置文件量比springmvc大。

### 62.如何避免sql注入？
- 使用预处理Preparedstatement
- 使用正则表达式过滤掉字符串中的特殊字符。

### 63.什么是XSS攻击，如何避免？
XSS攻击：即跨站脚本攻击，它是Web程序中常见的漏洞，原理是攻击者往Web页面里插入恶意的脚本代码，当用户浏览该页面时，嵌入其中的脚本代码会被执行。从而达到恶意攻击用户的目的，例如盗取用户cookie，破坏页面结构、重定向到其他网站等。

预防XSS的核心是必须对输入的数据做过滤处理。

### 64.什么是CSRF攻击，如何避免？

CSRF：Cross-Site Request Forgery（中文：跨站请求伪造），可以理解为攻击者盗用了你的身份，以你的名义发送恶意请求，比如：以你名义发送邮件、发消息、购买商品，虚拟货币转账等。
防御手段：
- 验证请求来源地址；
- 关键操作添加验证码；
- 在请求地址添加 token 并验证。

## 异常

### 65.throw和throws的区别
throw 真实抛出一个异常

throws：是声明可能会抛出一个异常。

### 66.try-catch-finally 中，如果 try或catch 中 return 了，finally 还会执行吗？
try-catch-finally 其中 catch 和 finally 都可以被省略，但是不能同时省略，也就是说有 try 的时候，必须后面跟一个 catch 或者 finally。

finally 一定会执行，即使是 try/catch 中 return 了，catch 中的 return 会等 finally 中的代码执行完之后，才会执行。需要注意的是，最终return的值以try或catch中为准。

### 67.常见的异常类有哪些？
- NullPointerException 空指针异常
- ClassNotFoundException 指定类不存在
- NumberFormatException 字符串转换为数字异常
- IndexOutOfBoundsException 数组下标越界异常
- ClassCastException 数据类型转换异常
- FileNotFoundException 文件未找到异常
- NoSuchMethodException 方法不存在异常
- IOException IO 异常
- SocketException Socket 异常

## 网络

### 68.http常见响应码

- 200 (成功) 服务器已成功处理了请求。 通常，这表示服务器提供了请求的网页。
- 301 (永久移动) 请求的网页已永久移动到新位置。 服务器返回此响应(对 GET 或 HEAD 请求的响应)时，会自动将请求者转到新位置。
- 304 (未修改) 自从上次请求后，请求的网页未修改过。 服务器返回此响应时，不会返回网页内容。
- 404 (未找到) 服务器找不到请求的网页。
- 500 (服务器内部错误) 服务器遇到错误，无法完成请求。

### 69.forward和redirt的区别？

forward是转发，redirect是重定向。
地址栏url现实：forward url不会发生改变，redirect url会发生变化

数据共享： forward可以共享request里面的数据，redirect不能共享。

效率： forward比redirect效率高。

### 70.tcp和udp的区别。
tcp 和 udp 是 OSI 模型中的运输层中的协议。tcp 提供可靠的通信传输，而 udp 则常被用于让广播和细节控制交给应用的通信传输。

两者的区别大致如下：
- tcp 面向连接，udp 面向非连接即发送数据前不需要建立链接；
- tcp 提供可靠的服务（数据传输），udp 无法保证；udp使用尽最大努力交付，即不保证可靠交付，同时也不使用拥塞控制。
- udp支持一对一，一对多，多对一，多对多的交互通信。
- UDP的首部开销小，只有8个字节。TCP首部最低20个字节。
- 每条tcp连接只能有两个端点（endpoint）每一条TCP连接只能是点对点的（即一对一），所以tcp提供全双工通信
- tcp 面向字节流，udp 面向报文；
- tcp 数据传输慢，udp 数据传输快；
- TCP提供拥塞控制和流量控制，而UDP不提供。

### 71.tcp为什么要三次握手，挥手却是四次。两次握手不行吗？为什么。以及为什么要四次挥手。

首先介绍**三次握手**的过程：
1. 第一次握手：建立连接时，客户端A发送SYN包(SYN=j)到服务器B，并进入SYN_SEND状态，等待服务器B确认。
2. 第二次握手：服务器B收到SYN包，必须确认客户A的SYN(ACK=j+1),同时自己也发送一个SYN包(SYN=k)即SYN+ACK包，此时服务器B进入SYN_RECV状态。
3. 第三次握手：客户端A收到服务器B的SYN+ACK包，向服务器B发送确认包ACK(ack=k+1)此包发送完毕，完成三次握手。客户端与服务器开始传送数据。

**四次握手**过程：
由于TCP连接是全双工的，因此每个方向都必须单独进行关闭。这个原则是当一方完成他的数据发送任务后就能发送一个FIN来终止这个方向的连接(主动关闭)。收到一个FIN只意味着这一方向上没有数据流动。一个TCP连接在收到一个FIN后仍能发送数据。首先进行关闭的一方将执行主动关闭，而另一方执行被动关闭。
1. 客户端A发送一个FIN，用来关闭客户A到服务器B的数据传送。
2. 服务器B收到这个FIN，它发回一个ACK，确认序号为收到的序号+1.
3. 服务器B关闭与客户端A的连接，发送一个FIN给客户端A。
4. 客户端A发回ACK报文确认，并将确认序号设置为收到序号+1。

如果采用两次握手，那么只要服务器发出确认数据包就会建立连接，但由于客户端此时并未响应服务器端的请求，那此时服务器端就会一直在等待客户端，这样服务器端就白白浪费了一定的资源。若采用三次握手，服务器端没有收到来自客户端的再次确认，则就会知道客户端并没有要求建立请求，就不会浪费服务器资源。

在TCP连接中，服务器端的SYN和ACK向客户端发送时一次性发送的，而在断开连接的过程中，B端向A端发送的ACK和FIN是分开发送的。因为在B端接收到A端的FIN后，B端可能还有数据要传输，所以先发送ACK，等B端处理完自己的事情就可以发送FIN断开连接了。
### 72.tcp粘包是怎么产生的？

tcp粘包可能发生在发送端或者接收端，分别来看两端各种产生粘包的原因。

- 发送端粘包：发送端需要等缓冲区满才发送出去，造成粘包。
- 接收方粘包： 接收方不及时接收缓冲区的包，造成多个包接收。

### 73.OSI七层模型都有哪些？
- 物理层:利用传输介质为数据链路层提供物理连接。实现比特流的透明传输。
- 数据链路层：负责建立和管理节点间的链路。
- 网络层：通过路由选择算法，为报文或分组通过通信子网选择最适当的路径。
- 传输层：向用户提供可靠的端对端的差错和流量控制，保证报文的正确传输。
- 会话层：向两个实体的表示层提供建立和使用连接的方法。
- 表示层：处理用户信息的表示问题，如编码，数据格式转换和加密解密等。
- 应用层：直接向用户提供服务，完成用户希望在网络上完成的各种工作。


### 74.get和post请求有何区别？
- get被强制服务器支持。
- 浏览器对URL的长度有限制，所以GET请求不能代替POST请求发送大量数据。
- get 请求会被浏览器主动缓存，而 post 不会。
- get 传递参数有大小限制，而 post 没有。
- get请求是幂等的
- post 参数传输更安全，get 的参数会明文限制在 url 上，post 不会。


### 75.http与https的区别？
- http是HTTP协议运行在TCP之上，所有传输的内容都是明文，客户端和服务器端都无法验证对方的身份。
- https是HTTP运行在SSL之上，SSL运行在TCP之上，所有的传输内容都经过加密，加密采用对称加密。
- http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。
- http和https使用的是完全不同的连接方式用的端口也不一样，前者是80端口，后者是443端口。
- http的连接很简单，是无状态的。
- https协议是由SSL+http协议构建的可进行加密传输，身份认证的网络协议，要比http协议安全。

## 设计模式
### 76.常用的设计模式：
- 单例模式：保证被创建一次，节省系统开销(常考必须记住。)
- 工厂模式(简单工厂，抽象工厂)：解耦代码.
- 观察者模式： 定义了对象之间的一对多的依赖，这样一来，当一个对象改变时，它的所有的依赖者都会收到通知并自动更新。
- 待更新

### 77.简单工厂和抽象工厂有什么区别？
- 简单工厂：用来生成同一等级结构中的任意产品，对于增加新的产品，无能为力。
- 抽象工厂：


## Spring/SpringMVC

### 78.为什么要使用Spring？
- spring 提供 ioc 技术，容器会帮你管理依赖的对象，从而不需要自己创建和管理依赖对象了，更轻松的实现了程序的解耦。
- spring 提供了事务支持，使得事务操作变的更加方便。
- spring 提供了面向切片编程，这样可以更方便的处理某一类的问题。
更方便的框架集成，spring 可以很方便的集成其他框架，比如 MyBatis、hibernate 等。

### 79.简单解释什么是aop，什么是ioc？
aop是面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。
简单来说就是统一处理某一“切面”的问题的编程思想，比如统一处理日志，异常等。

ioc：Inversionof Control（中文：控制反转）是 spring 的核心，对于 spring 框架来说，就是由 spring 来负责控制对象的生命周期和对象间的关系。

简单来说，控制指的是当前对象对内部成员的控制权；控制反转指的是，这种控制权不由当前对象管理了，由其他（类,第三方容器）来管理。

### 80.Spring有哪些模块？
- spring core：框架最基础的部分，提供ioc和依赖注入特性。
- spring context: 构建与core封装包基础上的context封装包，提供了一种框架式的对象访问方法。
- spring dao： Data Access Object提供了JDBC的抽象层。
- spring aop：提供了面向切面的编程实现，让你可以自定义拦截器、切点等。
- spring web：提供了针对web开发的集成特性，例如文件上传，利用servlet listener进行ioc容器初始化和针对Web的ApplicationContext。
- spring Web mvc：spring中的mvc封装包提供了web应用的Model-View-Controller（MVC）的实现。

### 81.spring常用的注入方式？
- setter属性注入
- 构造方法注入
- 注解方式注入

### 82.spring中的bean是线程安全的吗？

spring中的bean默认是单例模式，spring框架并没有对单例bean进行多线程的封装处理。实际上大部分时候，spring bean是无状态的，所以某种程度上来说bean是安全的。但如果bean是有状态的，那需要开发者自己去保证线程安全，最简单的就是改变bean的作用域，把singleton变更为prototype，这样请求bean相当于new Bean()了，所以就可以保证线程安全了。
- 有状态就是有数据存储功能。
- 无状态就是不会保存数据。

### 83.spring支持几种bean的作用域？
spring支持5种作用域，如下：
- singleton ：spring ioc容器中只存在一个bean实例，bean以单例模式存在，是系统默认值。
- prototype：每次从容器调用bean时都会创建一个新的实例，即每次getBean()相当于执行new Bean()操作。
- Web环境下的作用域：
 - request：每次http请求都会创建一个bean；
 - session：同一个http session 共享一个bean实例。
 - global-session 用于portlet容器，因为每个portlet有单独的session，globalsession提供一个全局性的http session。
 
  **注意**： 使用 prototype 作用域需要慎重的思考，因为频繁创建和销毁 bean 会带来很大的性能开销。

### 84.Spring事务以及隔离级别
首先，需要明白事务的概念。
事务就是一组操作数据库的动作集合。事务是现代数据库理论中的核心概念之一。如果一组=处理步骤或者全部发生或者一步也不执行，我们称该组处理步骤为一个事务。
当所有的步骤像一个操作一样被完整的执行，我们称该事务被提交。由于其中的一部分或多步执行失败，导致没有步骤被提交，则事务必须回滚到最初的系统状态。

在Spring中，有五大隔离级别，七个事务传播属性。
七个事务传播属性：
- PROPAGATION_REQUIRED 支持当前事务，如果当前没有事务，就新建一个事务，这是常见的选择。
- PROPAGATION_SUPPORTS 支持当前事务，如果当前没有事务，就以非事务的方式执行。
- PROPAGATION_MANDATORY 支持当前事务，如果当前没有事务，就抛出异常。
- PROPAGATION_NOT_SUPPORTED 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
- PROPAGATION_NEVER  以非事务方式执行，如果当前存在事务，则抛出异常。
- PROPAGATION_NESTED  如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则进行与PROPAGATION_REQUIRED类似的操作。

备注：常用的两个事务传播属性是1和4，即PROPAGATION_REQUIRED，PROPAGATION_REQUIRES_NEW

五个隔离级别：
- ISOLATION_DEFAULT 这是一个PlatfromTransactionManager默认的隔离级别，使用数据库默认的事务隔离级别，另外四个与JDBC的隔离级别相对应。
- ISOLATION_READ_UNCOMMITTED （未提交读） 这是事务最低的隔离级别，它允许另外一个事务可以看到这个事务提交的数据。这个隔离级别会产生脏读，不可重复读和幻读。
- ISOLATION_READ_COMMITTED （提交读）
保证一个事务修改的数据提交后才能被另外一个事务读取，另外一个事务不能读取该事务未提交的数据。这种事务隔离级别可以避免脏读出现，但是可能会出现不可重复读和幻读。
- ISOLATION_REPEATABLE_READ（可重复读）
这种事务隔离级别可以防止脏读，不可重复读。但是可能出现幻读。它除了保证一个事务不能读取另一个事务未提交的数据外，还保证了避免不可重复读的产生。

ISOLATION_SERIALIZABLE(序列化)
这是花费最高代价，但是最可靠的事务隔离级别。事务被处理为顺序执行。可避免脏读，不可重复读，幻读。

脏读 ：表示一个事务能够读取另一个事务中还未提交的数据。比如，某个事务尝试插入记录 A，此时该事务还未提交，然后另一个事务尝试读取到了记录 A。

不可重复读 ：是指在一个事务内，多次读同一数据。

幻读 ：指同一个事务内多次查询返回的结果集不一样。比如同一个事务 A 第一次查询时候有 n 条记录，但是第二次同等条件下查询却有 n+1 条记录，这就好像产生了幻觉。发生幻读的原因也是另外一个事务新增或者删除或者修改了第一个事务结果集里面的数据，同一个记录的数据内容被修改了，所有数据行的记录就变多或者变少了

### 85.Spring mvc运行流程
- spring mvc 先将请求发送给DispatcherServlet.
- DispatcherServlet查询一个或多个HandlerMapping，找到处理请求的Controller。
- DispatcherServlet再把请求提交到对应的Controller。
- Controller进行业务逻辑处理后，会返回一个ModelAndView。
- Dispatcher查询一个或多个ViewResolver视图解析器，找到ModelAndView对象指定的视图对象。
- 视图对象负责渲染返回给客户端。

### 86.spring mvc有哪些组件？
- 前置控制器 DispatcherServlet。
- 映射控制器 HandlerMapping。
- 处理器 Controller。
- 模型和视图 ModelAndView。
- 视图解析器 ViewResolver。

### 87.@RequestMapping的作用是什么？
将http请求映射到相应的类或方法上。

### 88.@Autowired的作用是什么？
@Autowired它可以对类成员变量、方法及构造函数进行标注，完成自动装配的工作，通过@Autowired的使用来消除set/get方法。