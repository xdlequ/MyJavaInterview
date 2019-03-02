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