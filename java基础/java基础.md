# java基础



## Java中Runnable和Thread以及Callable的区别

### 继承Thread

- 一个类只要继承了Thread类同时覆写了本类中的run()方法就可以实现多线程操作了，但是一个类只能继承一个父类。

### 实现Runnable接口

- 一个类可以实现多个接口
- 适合于资源的共享

### 实现Callable接口

- Callable使用call()方法，Runnable使用run()方法。
- call()可以返回值，run()不能返回。
- call()可以抛出受检查的异常。

```java
class TaskWithResult implements Callable<String> {  
    private int id;  

    public TaskWithResult(int id) {  
        this.id = id;  
    }  

    @Override  
    public String call() throws Exception {  
        return "result of TaskWithResult " + id;  
    }  
}  

public class CallableTest {  
    public static void main(String[] args) throws InterruptedException,  
            ExecutionException {  
        ExecutorService exec = Executors.newCachedThreadPool();  
       //Future 相当于是用来存放Executor执行的结果的一种容器  
        ArrayList<Future<String>> results = new ArrayList<Future<String>>();    
        for (int i = 0; i < 10; i++) {  
            results.add(exec.submit(new TaskWithResult(i)));  
        }  
        for (Future<String> fs : results) {  
            if (fs.isDone()) {  
                System.out.println(fs.get());  
            } else {  
                System.out.println("Future result is not yet complete");  
            }  
        }  
        exec.shutdown();  
    }  
} 
```

## Stream

### 操作符

操作符就是对数据进行的一种处理工作，一道加工程序；就好像工厂的工人对流水线上的产品进行一道加工程序一样。

### 中间操作符

- map(mapToInt,mapToLong,mapToDouble) 转换操作符，把比如A->B，这里默认提供了转int，long，double的操作符。
- flatmap(flatmapToInt,flatmapToLong,flatmapToDouble) 拍平操作比如把 int[]{2,3,4} 拍平 变成 2，3，4 也就是从原来的一个数据变成了3个数据，这里默认提供了拍平成int,long,double的操作符。
- limit 限流操作，比如数据流中有10个 我只要出前3个就可以使用。
- distint 去重操作，对重复元素去重，底层使用了equals方法。
- filter 过滤操作，把不想要的数据过滤。
- peek 挑出操作，如果想对数据进行某些操作，如：读取、编辑修改等。
- skip 跳过操作，跳过某些元素。
- sorted(unordered) 排序操作，对元素排序，前提是实现Comparable接口，当然也可以自定义比较器。

### 终止操作符

数据经过中间加工操作，就轮到终止操作符上场了；终止操作符就是用来对数据进行收集或者消费的，数据到了终止操作这里就不会向下流动了，终止操作符只能使用一次。

- collect 收集操作，将所有数据收集起来，这个操作非常重要，官方的提供的Collectors 提供了非常多收集器，可以说Stream 的核心在于Collectors。
- count 统计操作，统计最终的数据个数。
- findFirst、findAny 查找操作，查找第一个、查找任何一个 返回的类型为Optional。
- noneMatch、allMatch、anyMatch 匹配操作，数据流中是否存在符合条件的元素 返回值为bool 值。
- min、max 最值操作，需要自定义比较器，返回数据流中最大最小的值。
- reduce 规约操作，将整个数据流的值规约为一个值，count、min、max底层就是使用reduce。
- forEach、forEachOrdered 遍历操作，这里就是对最终的数据进行消费了。
- toArray 数组操作，将数据流的元素转换成数组。

## Java并发编程三个性质：原子性、可见性、有序性

- 原子性：一个操作或多个操作要么全部执行且执行过程不被中断，要么不执行
- 可见性：多个线程修改同一个共享变量时，一个线程修改后，其他线程能马上获得修改后的值
- 有序性 :  程序执行的顺序按照代码的先后顺序执行
- 可以通过 **synchronized**和**Lock**实现原子性，因为synchronized和Lock能够保证任一时刻**只有一个线程**访问该代码块；Java提供了**volatile关键字**保证可见性；synchronized和lock也可保证可见性，在加锁时其他线程无法访问共享资源；可以通过**volatile**关键字来保证一定的“有序性”

## Cookie和Session

**会话（Session）**跟踪是Web程序中常用的技术，用来**跟踪用户的整个会话**。常用的会话跟踪技术是Cookie与Session。**Cookie通过在客户端记录信息确定用户身份**，**Session通过在服务器端记录信息确定用户身份**。

### Cookie

一个用户请求操作都属于同一个会话，不同用户请求不属于不同会话。而Http协议是无状态协议。数据交互完毕后，客户端与服务端的连接就会关闭，再次交换数据需要建立新的连接。

- Cookie是一小段文本信息，如果服务器需要记录用户状态，使用response向客户端浏览器颁发一个Cookie，浏览器把Cookie存起来。当浏览器再次请求，浏览器把Cookie提交给服务器。
- Cookie具有不可跨域名性。
- 　Cookie是不可跨域名的。域名www.google.com颁发的Cookie不会被提交到域名www.baidu.com去。这是由Cookie的隐私安全机制决定的。需要设置Cookie的domain参数。
- 如果不设定时间，它的生命周期为浏览器会话的期间，只要关闭浏览器，cookie就消失了。
- 如果设置cookie的过期时间，浏览器会把cookie保存到硬盘中，再次打开浏览器有效。

### Session

Session是服务器端使用的一种记录客户端状态的机制

- Session在用户第一次访问服务器的时候自动创建。
- 设置Session有效期防止Session过多。
- Tomcat中Session默认失效时间为20分钟。
- 调用Session的invalidate方法。

## Java对象的大小

```java
Object ob = new Object();
```

在java中一个空Object对象的大小是8byte，那么它所占的空间为：4byte+8byte。4byte为Java栈中保存引用的所需要的空间。8byte为Java堆中对象的信息。Java非基本类型对象都默认继承Object对象，大小必须大于8byte。

```java
Class NewObject {
    int count;
    boolean flag;
    Object ob;
}
    //其大小为：空对象大小(8byte)+int大小(4byte)+Boolean大小(1byte)+空Object引用的大小(4byte)=17byte。但是因为Java在对对象内存分配时都是以8的整数倍来分，因此大于17byte的最接近8的整数倍的是24，因此此对象的大小为24byte。
```

- 包装类已经成为对象，至少12byte（声明Object最小空间），java对象是8的整数倍，因此一个基本类型包装类至少16byte。

## 引用类信息

- 强引用：一般声明对象是虚拟机生成的引用，强引用下，垃圾回收需要严格判断当前对象是否被强引用，如果被强引用，则不会被垃圾回收。
- 软引用：一般作为缓存使用。软引用在垃圾回收时，虚拟机根据当前系统剩余内存来决定是否堆软引用进行回收。**发生OutOfMemory时，肯定没有软引用存在。**
- 弱引用：弱引用与软引用类似，弱引用在进行垃圾回收时，是一定会被回收掉的。







## Java内存模型

Java线程之间的通信由Java内存模型（Java Memory Model，简称JMM）控制。JMM决定一个线程对共享变量的写入何时对另一个线程可见。

