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

### 4.final在Java中有什么作用？finally呢
- final修饰的类叫最终类，该类不能被继承。
- final修饰的方法不能被重写。
- final修饰的变量叫做常量，常量必须初始化，初始化之后，值就不能被修改。

finally作为异常处理的一部分，只能用在try/catch语句之中，并且附带了一个语句块，表示这段语句最终一定会被执行，经常被用在需要释放资源的情况下使用。

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


