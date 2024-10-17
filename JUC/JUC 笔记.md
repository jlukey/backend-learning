# 1. synchronized与lock
1. 多线程交互中，必须防止多线程的虚假唤醒，也即 ( 判断只用while，不能用if ) 。

      * 什么是虚假唤醒：当一个条件满足时，很多线程都被唤醒了，但是只有其中部分是有用的唤醒，其它的唤醒都是无用功。
      
      * 为什么 if 会出现虚假唤醒：因为 if 只会执行一次，执行完会接着向下执行 if（）外边的，而 while 不会，直到条件满足才会向下执行 while（）外边的。

2. synchronized 的新写法为 lock

```java
private Lock lock = new ReentrantLock();
private Condition condition = lock.newCondition();
// 其中 wait()被condition.await()代替，notifyAll()被condition.signalAll()代替.
```

lock的使用方式：

```java
Lock lock = new ReentrantLock();
lock.lock();
try{
    //处理任务
}catch(Exception ex){
     
}finally{
    lock.unlock();   //释放锁
}
```

3. 为什么要用lock代替synchronized？  
lock可以定义多个condition（可以理解为一把锁有多个钥匙），精准通知，精准唤醒。

通过设置多个condition精确通知需要唤醒的线程，可以指定线程顺序调用。



# 2. 8锁理解
<font style="color:#4D4D4D;">① 非静态方法的默认锁是 this， 静态方法的默认锁是 class 类。</font>

<font style="color:#4D4D4D;">②某一时刻内，只能有一个线程有锁，无论几个方法。</font>

```java
/*
1、标准的访问情况下，先执行 sendEmail 还是 sendSMS

   答案：sendEmail
   被 synchronized 修饰的方法，锁的对象是方法的调用者也就是实际new的对象，所以说这里两个方法调用的对象是同一个
   先调用的先执行！
 */
public class LockDemo01 {
    public static void main(String[] args) throws InterruptedException {
        Phone phone = new Phone();

        new Thread(()->{phone.sendEmail();},"A").start();
        TimeUnit.SECONDS.sleep(2);
        new Thread(()->{phone.sendSMS();},"B").start();
    }
}
class Phone{
    public synchronized void sendEmail(){
        System.out.println("sendEmail");
    }
    public synchronized void sendSMS(){
        System.out.println("sendSMS");
    }
}
```

```java
/*
2、sendEmail休眠4秒后 ，先执行 sendEmail 还是 sendSMS

   答案：sendEmail
   被 synchronized 修饰的方法，锁的对象是方法的调用者，所以说这里两个方法调用的对象是同一个。 先调用的先执行！执行sleep（）方法的线程并不会释放锁。
   
   阳哥笔记: 
   一个对象里面如果有多个synchronized方法, 某一个时刻内，只要一个线程去调用其中的一个synchronized方法了，其他线程都只能等待，换句话说，某一个时刻内，只能有唯一一个线程去访问这些synchronized方法，锁的是当前对象this，被锁定后，其他线程都不能进入到当前对象的其他的 synchronized方法。
 */
public class LockDemo02 {
    public static void main(String[] args) throws InterruptedException {
        Phone2 phone = new Phone2();

        new Thread(()->{
            try {
                phone.sendEmail();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"A").start();

        //Thread.sleep(200);
        TimeUnit.SECONDS.sleep(2);

        new Thread(()->{
            phone.sendSMS();
        },"B").start();
    }
}

class Phone2{
    public synchronized void sendEmail() throws InterruptedException {
        TimeUnit.SECONDS.sleep(4);
        System.out.println("sendEmail");
    }

    public synchronized void sendSMS(){
        System.out.println("sendSMS");
    }
}
```

```java
/*
3、增加一个普通方法，请问先打印那个 sendEmail 还是 hello

   答案：hello
   新增加的这个方法没有 synchronized 修饰，不是同步方法，不受锁的影响！
 */
public class LockDemo03 {
    public static void main(String[] args) throws InterruptedException {
        Phone3 phone = new Phone3();

        new Thread(()->{
            try {
                phone.sendEmail();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"A").start();
        TimeUnit.SECONDS.sleep(1);
        
        new Thread(()->{
            phone.hello();
        },"B").start();
    }
}

class Phone3{
    public synchronized void sendEmail() throws InterruptedException {
        TimeUnit.SECONDS.sleep(4);
        System.out.println("sendEmail");
    }

    // 没有 synchronized 没有 static 就是普通方法
    public void hello(){
        System.out.println("hello");
    }
}
```

```java
/*
4、两个手机，请问先执行sendEmail 还是 sendSMS
    答案：sendSMS
    被 synchronized  修饰的方式，锁的对象是调用者；我们这里有两个调用者，两个方法在这里是两个锁
 */
public class LockDemo04 {
    public static void main(String[] args) throws InterruptedException {
        Phone4 phone1 = new Phone4();
        Phone4 phone2 = new Phone4();

        new Thread(()->{
            try {
                phone1.sendEmail();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"A").start();

        //Thread.sleep(200);
        TimeUnit.SECONDS.sleep(1);

        new Thread(()->{
            phone2.sendSMS();
        },"B").start();
    }
}

class Phone4{
    public synchronized void sendEmail() throws InterruptedException {
        TimeUnit.SECONDS.sleep(3);
        System.out.println("sendEmail");
    }

    public synchronized void sendSMS(){
        System.out.println("sendSMS");
    }
}
```

```java
/*
5、两个静态同步方法，同一个手机请问先执行sendEmail 还是 sendSMS

    答案：sendEmail
    只要方法被 static 修饰，锁的对象就是 Class模板对象,这个则全局唯一！所以说这里是同一个锁
    并不是因为synchronized
 */
public class LockDemo05 {
    public static void main(String[] args) throws InterruptedException {
        Phone5 phone = new Phone5();


        new Thread(()->{
            try {
                phone.sendEmail();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"A").start();

        //Thread.sleep(200);
        TimeUnit.SECONDS.sleep(1);

        new Thread(()->{
            phone.sendSMS();
        },"B").start();
    }
}

class Phone5{

    public static synchronized void sendEmail() throws InterruptedException {
        TimeUnit.SECONDS.sleep(3);
        System.out.println("sendEmail");
    }

    public static synchronized void sendSMS(){
        System.out.println("sendSMS");
    }

}
```

```java
/*
6、两个静态同步方法，两个手机，请问先执行sendEmail 还是 sendSMS

    答案：sendEmail
    只要方法被 static 修饰，锁的对象就是 Class模板对象,这个则全局唯一！所以说这里是同一个锁
    并不是因为synchronized
 */
public class LockDemo06 {
    public static void main(String[] args) throws InterruptedException {
        Phone6 phone = new Phone6();
        Phone6 phone2 = new Phone6();

        new Thread(()->{
            try {
                phone.sendEmail();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"A").start();

        //Thread.sleep(200);
        TimeUnit.SECONDS.sleep(1);

        new Thread(()->{
            phone2.sendSMS();
        },"B").start();
    }
}

class Phone6{

    public static synchronized void sendEmail() throws InterruptedException {
        TimeUnit.SECONDS.sleep(3);
        System.out.println("sendEmail");
    }

    public static synchronized void sendSMS(){
        System.out.println("sendSMS");
    }

}
```

```java
/*
7、一个普通同步方法，一个静态同步方法，只有一个手机，请问先执行sendEmail 还是 sendSMS

    答案：sendSMS
    synchronized 锁的是这个调用的对象
    static 锁的是这个类的Class模板
    这里是两个锁！互不影响
 */
public class LockDemo07 {
    public static void main(String[] args) throws InterruptedException {
        Phone7 phone = new Phone7();

        new Thread(()->{
            try {
                phone.sendEmail();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"A").start();

        //Thread.sleep(200);
        TimeUnit.SECONDS.sleep(1);

        new Thread(()->{
            phone.sendSMS();
        },"B").start();
    }
}

class Phone7{

    public static synchronized void sendEmail() throws InterruptedException {
        TimeUnit.SECONDS.sleep(3);
        System.out.println("sendEmail");
    }

    public synchronized void sendSMS(){
        System.out.println("sendSMS");
    }

}
```

```java
1 /*
 2 8、一个普通同步方法，一个静态同步方法，两个手机，请问先执行sendEmail 还是 sendSMS
 3 
 4     答案：sendSMS
 5     synchronized 锁的是这个调用的对象
 6     static 锁的是这个类的Class模板
 7     这里是两个锁！
 8  */
 9 public class LockDemo08 {
10     public static void main(String[] args) throws InterruptedException {
11         Phone8 phone = new Phone8();
12         Phone8 phone2 = new Phone8();
13 
14         new Thread(()->{
15             try {
16                 phone.sendEmail();
17             } catch (InterruptedException e) {
18                 e.printStackTrace();
19             }
20         },"A").start();
21 
22         //Thread.sleep(200);
23         TimeUnit.SECONDS.sleep(1);
24 
25         new Thread(()->{
26             phone2.sendSMS();
27         },"B").start();
28     }
29 }
30 
31 class Phone8{
32 
33     public static synchronized void sendEmail() throws InterruptedException {
34         TimeUnit.SECONDS.sleep(3);
35         System.out.println("sendEmail");
36     }
37 
38     public synchronized void sendSMS(){
39         System.out.println("sendSMS");
40     }
41 
42 }
```

**8锁小结**

1、new this 调用的这个对象，是一个具体的对象！

2、static class 唯一的一个模板！

在我们编写多线程程序得时候，只需要搞明白这个到底锁的是什么就不会出错了！



synchronized(Demo.class){

}//等同于static synchronized 方法



synchronized(this){

}//等同于synchronized普通方法



# 3. List不安全
## 举例说明list线程不安全  
<font style="color:#F5222D;">注意：ArrayList不是为并发情况而设计的集合类</font>

举例：多个线程操作ArraList，边写边读，会出现<font style="color:#F5222D;">并发修改异常</font>.

**故障现象**：

java.util.**<font style="color:#F5222D;">ConcurrentModificationException </font>**并发修改异常.

**导致原因**：



**解决方法**：

1） List<String> list = new Vector<>();  // vector 线程安全，add方法有synchronized，读写性能低,同一时间段只能一个人读/写.

2）List<String> list = Collections.synchronizedList(new ArrayList<>()); // Collections工具类提供，将ArrayList转换为线程安全.

3）List<String> list = new CopyOnWriteArrayList();  // JUC包提供的 写时复制(读写分离思想).

## CopyOnWriteArrayList浅析
<font style="color:#000000;">和ArrayList一样，其底层数据结构也是数组，加上transient不让其被序列化，加上volatile修饰来保证多线程下的其可见性和有序性。</font>

<font style="color:#000000;">其修改操作是基于fail-safe机制，像我们的String一样，不在原来的对象上直接进行操作，而是复制一份对其进行修改，另外此处的修改操作是利用Lock锁进行上锁的，所以保证了线程安全问题。</font>

## CopyOnWriteArrayList的几个要点
+ 实现了List接口
+ 内部持有一个ReentrantLock lock = new ReentrantLock();
+ 底层是用<font style="color:#FF0000;">volatile</font> <font style="color:#FF0000;">transient</font>声明的数组 array
+ 读写分离，写时复制出一个新的数组，完成插入、修改或者移除操作后将新数组赋值给array

**volatile**<font style="color:#333333;"> （挥发物、易变的）：变量修饰符，只能用来修饰变量。volatile修饰的成员变量在每次被线程访问时，都强迫从共享内存中重读该成员变量的值。而且，当成员变量发生变 化时，强迫线程将变化值回写到共享内存。这样在任何时刻，两个不同的线程总是看到某个成员变量的同一个值。</font>

**transient （**<font style="color:#333333;">暂短的、临时的</font>**）**<font style="color:#333333;">：修饰符，只能用来修饰字段。在对象序列化的过程中，标记为transient的变量不会被序列化。</font>

## CopyOnWrite的缺点　
CopyOnWrite容器有很多优点，但是同时也存在两个问题，即内存占用问题和数据一致性问题。所以在开发的时候需要注意一下。

**　　内存占用问题**。因为CopyOnWrite的写时复制机制，所以在进行写操作的时候，内存里会同时驻扎两个对象的内存，旧的对象和新写入的对象（注意:在复制的时候只是复制容器里的引用，只是在写的时候会创建新对象添加到新容器里，而旧容器的对象还在使用，所以有两份对象内存）。如果这些对象占用的内存比较大，比如说200M左右，那么再写入100M数据进去，内存就会占用300M，那么这个时候很有可能造成频繁的Yong GC和Full GC。之前我们系统中使用了一个服务由于每晚使用CopyOnWrite机制更新大对象，造成了每晚15秒的Full GC，应用响应时间也随之变长。

　　针对内存占用问题，可以通过压缩容器中的元素的方法来减少大对象的内存消耗，比如，如果元素全是10进制的数字，可以考虑把它压缩成36进制或64进制。或者不使用CopyOnWrite容器，而使用其他的并发容器，如ConcurrentHashMap。

**　　数据一致性问题**。CopyOnWrite容器只能保证数据的最终一致性，不能保证数据的实时一致性。所以如果你希望写入的的数据，马上能读到，请不要使用CopyOnWrite容器。

## CopyOnWriteArrayList为什么并发安全且性能比Vector好
**　**我们知道Vector是增删改查方法都加了synchronized，保证同步，但是每个方法执行的时候都要去获得锁，性能就会大大下降，而CopyOnWriteArrayList 只是在增删改上加锁，但是读不加锁，在读方面的性能就好于Vector，CopyOnWriteArrayList支持读多写少的并发情况。

# 4. Set 不安全
HashSet底层数据结构是HashMap。 hashSet的add方法底层调用的就是hashMap的put方法，为什么hashMap的put是2个参数，而hashSet的add是一个参数呢，因为add进去的一个值就是put的key，value永远是一个Object类型的常量: PRSENT，固定写死的。

Set<String> set =  new CopyOnWriteArraySet();  // new HashSet<>();  Collections.synchronizedArraySet(new HashSet());



# 5. Map 不安全
Map<String,Object> map = new ConCurrentHashMap<>();   // new HashMap<>();  Collections.synchronizedMap();



# 6. Callable 

**面试题：callable 接口与 runnable 接口的区别？**

- 是否有返回值
- 是否抛异常
- 落地方法不一样，一个是run，一个是call

```java
/**
* 使用 Callable 和 FutureTask 来实现多线程编程，并获取线程执行的结果
*/
public class CallableDemo {

    public static void main(String[] args) throws ExecutionException, InterruptedException{

        // 创建一个 FutureTask 对象，传入 MyThread 实例
        FutureTask<Integer> futureTask = new FutureTask<>(new MyThread());

        // 创建启动一个新线程，将futureTask 作为任务传入,线程启动后，会执行futureTask中的任务。
        new Thread(futureTask, "A").start();

        // 获取并打印 futureTask 的结果.(futureTask.get()方法会阻塞当前线程，直到futureTask中的任务执行完毕并返回结果)
      	// 这里会等待 `MyThread` 的 `call` 方法执行完毕，并返回结果 `1024`。
        System.out.println(futureTask.get());
    }
}

class MyThread implements Callable<Integer>{
    @Override
    public Integer call() throws Exception {
        System.out.println("******come in callable");
        return 1024;
    }
}

// >>> 控制台输出：
// >>> ******come in callable
// >>> 1024
```

执行流程

1. `main` 方法启动后，创建一个 `FutureTask` 对象，包装 `MyThread` 实例。
2. 启动一个新线程 `A`，执行 `futureTask` 中的任务。
3. 新线程 `A` 执行 `MyThread` 的 `call` 方法，打印 `"******come in callable"`，并返回 `1024`。
4. `main` 线程调用 `futureTask.get()`，等待 `MyThread` 的 `call` 方法执行完毕，并获取返回值 `1024`。
5. `main` 线程打印 `futureTask.get()` 的结果 `1024`。



_**<font style="color:#F5222D;background-color:#FCFCCA;">并发线程栅栏：CyclicBarrier 和CountDownLatch  区别：</font>**_

+ CountDownLatch：一个或者多个线程，<font style="color:#FF0000;">等待其他</font>多个线程完成某件事情之后才能执行；
+ CyclicBarrier：多个线程<font style="color:#FF0000;">互相等待</font>，直到到达同一个同步点，<font style="color:#FF0000;">再继续</font>**<font style="color:#FF0000;">一起执行</font>**。



<font style="color:#000000;">对于CountDownLatch来说，</font><font style="color:#FF0000;">重点是“一个线程（多个线程）等待</font><font style="color:#000000;">”，而其他的N个线程在完成“某件事情”之后，可以终止，也可以等待。而对于</font><font style="color:#FF0000;">CyclicBarrier，重点是多个线程，在任意一个线程没有完成，所有的线程都必须互相等待，然后继续一起执行</font><font style="color:#000000;">。</font>

<font style="color:#000000;"></font>

# 7. 同步锁工具类

## 7.1 CountDownLatch概念

CountDownLatch又名**<font style="color:#F5222D;">倒计时计算器</font>**，它是一个同步工具类，用来协调多个线程之间的同步，或者说起到线程之间的通信（而不是用作互斥的作用）。

CountDownLatch能够使一个线程在等待另外一些线程完成各自工作之后，再继续执行。使用一个计数器进行实现。计数器初始值为线程的数量。当每一个线程完成自己任务后，计数器的值就会减一。当计数器的值为0时，表示所有的线程都已经完成一些任务，然后在CountDownLatch上等待的线程就可以恢复执行接下来的任务。



## 7.2 CountDownLatch的用法

1、某一线程在开始运行前等待n个线程执行完毕。将CountDownLatch的计数器初始化为new CountDownLatch(n)，每当一个任务线程执行完毕，就将计数器减1 countdownLatch.countDown()，当计数器的值变为0时，在CountDownLatch上await()的线程就会被唤醒。一个典型应用场景就是启动一个服务时，<font style="color:#F5222D;">主线程需要等待多个组件加载完毕</font>，<font style="color:#F5222D;">之后再继续执行</font>。



> <font style="color:#FA8C16;">countDownLatch是采取阻塞主线程的方法实现了线程的统一</font>



2、实现多个线程开始执行任务的最大并行性。注意是并行性，不是并发，强调的是多个线程在某一时刻同时开始执行。类似于赛跑，将多个线程放到起点，等待发令枪响，然后同时开跑。做法是初始化一个共享的CountDownLatch(1)，将其计算器初始化为1，多个线程在开始执行任务前首先countdownlatch.await()，当主线程调用countDown()时，计数器变为0，多个线程同时被唤醒。



## 7.3 CountDownLatch的不足

CountDownLatch是一次性的，计算器的值只能在构造方法中初始化一次，之后没有任何机制再次对其设置值，当CountDownLatch使用完毕后，它不能再次被使用。



## 7.4 方法说明

- [x] **public void countDown()**

递减锁存器的计数，如果计数到达零，则释放所有等待的线程。如果当前计数大于零，则将计数减少.



- [x] **<font style="color:#000000;">public boolean await(long timeout,TimeUnit unit) throws InterruptedException</font>**

使当前线程在锁存器倒计数至零之前一直等待，除非线程被中断或超出了指定的等待时间。如果当前计数为零，则此方法立刻返回true值。

　　如果当前计数大于零，则出于线程调度目的，将禁用当前线程，且在发生以下三种情况之一前，该线程将一直出于休眠状态：

1. 如果计数到达零，则该方法返回true值。
2. 如果当前线程，在进入此方法时已经设置了该线程的中断状态；或者在等待时被中断，则抛出InterruptedException，并且清除当前线程的已中断状态。
3. 如果超出了指定的等待时间，则返回值为false。如果该时间小于等于零，则该方法根本不会等待。

**参数**：

　　timeout-要等待的最长时间

　　unit-timeout 参数的时间单位

**返回**：

　　如果计数到达零，则返回true；如果在计数到达零之前超过了等待时间，则返回false

**抛****出**：

　　InterruptedException-如果当前线程在等待时被中断



## 7.5 CyclicBarrier

<font style="color:#4D4D4D;">在CyclicBarrier类的内部有一个计数器，每个线程在到达屏障点的时候都会调用await方法将自己阻塞，此时计数器会减1，当计数器减为0的时候所有因调用await方法而被阻塞的线程将被唤醒。这就是实现一组线程相互等待的原理。</font>

<font style="color:#4D4D4D;"></font>

> <font style="color:#FA541C;">CountDownLatch是计数器，只能使用一次，而CyclicBarrier的计数器提供reset功能，可以多次使用。</font>

<font style="color:#4D4D4D;"></font>

<font style="color:#4D4D4D;">代码示例：</font>

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612453876882-74684952-54d7-4827-8e4e-ce71bff2a60e.png)

<font style="color:#4D4D4D;">打印输出：</font>

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612454008204-1f563c5e-f1a0-4302-9195-3e15f65cc157.png)

<font style="color:#4D4D4D;"></font>



## 7.6 Semaphore 

<font style="color:#121212;">Semaphore 通常我们叫它信号量， 可以用来控制同时访问特定资源的线程数量，通过协调各个线程，以保证合理的使用资源</font>

<font style="color:#121212;"></font>

<font style="color:#121212;">可以把它简单的理解成我们停车场入口立着的那个显示屏，每有一辆车进入停车场显示屏就会显示剩余车位减1，每有一辆车从停车场出去，显示屏上显示的剩余车辆就会加1，当显示屏上的剩余车位为0时，停车场入口的栏杆就不会再打开，车辆就无法进入停车场了，直到有一辆车从停车场出去为止。</font>

<font style="color:#121212;"></font>

<font style="color:#121212;">主要用于那些资源有明确访问数量限制的场景，常用于限流 。</font>

<font style="color:#121212;">参考资料：</font>[https://zhuanlan.zhihu.com/p/98593407](https://zhuanlan.zhihu.com/p/98593407)

<font style="color:#121212;"></font>

# 8. 读写锁

<font style="color:#121212;">在 Java 中通过</font>`ReadWriteLock`<font style="color:#121212;">来实现读写锁。ReadWriteLock是一个接口，</font>`ReentrantReadWriteLock`<font style="color:#121212;">是 ReadWriteLock 接口的具体实现类。在 ReentrantReadWriteLock 中定义了两个内部类</font>`ReadLock`<font style="color:#121212;">、</font>`WriteLock`<font style="color:#121212;">，分别来实现读锁和写锁</font>

<font style="color:#121212;"></font>

读-读  不互斥

读-写  互斥

写-写  互斥

<font style="color:#404040;"></font>

<font style="color:#404040;">Java并发包中ReadWriteLock是一个接口，</font><font style="color:#404040;">主要有两个方法，如下</font>

```java
public interface ReadWriteLock {
    /**
     * Returns the lock used for reading.
     *
     * @return the lock used for reading
     */
    Lock readLock();

    /**
     * Returns the lock used for writing.
     *
     * @return the lock used for writing
     */
    Lock writeLock();
}
```

资料： [https://www.jianshu.com/p/9cd5212c8841](https://www.jianshu.com/p/9cd5212c8841)



# 9. 阻塞队列

BlockingQueue即阻塞队列，它是基于ReentrantLock.

> 在Java中，BlockingQueue是一个接口，它的实现类有ArrayBlockingQueue、DelayQueue、 LinkedBlockingDeque、LinkedBlockingQueue、PriorityBlockingQueue、SynchronousQueue等，它们的区别主要体现在存储结构上或对元素操作上的不同，但是对于take与put操作的原理，却是类似的。

**种类分析** 

+ <font style="color:#F5222D;">ArrayBlockingQueue：由数组结构组成的有界阻塞队列。</font>
+ <font style="color:#F5222D;">LinkedBlockingQueue：由链表结构组成的有界（但大小默认值为integer.MAX_VALUE）阻塞队列。</font>
+ PriorityBlockingQueue：支持优先级排序的无界阻塞队列。
+ DelayQueue：使用优先级队列实现的延迟无界阻塞队列。
+ <font style="color:#F5222D;">SynchronousQueue：不存储元素的阻塞队列，也即单个元素的队列。</font>
+ LinkedTransferQueue：由链表组成的无界阻塞队列。
+ LinkedBlockingDeque：由链表组成的双向阻塞队列。

备注： 掌握红色部分即可，其他了解即可。



# 10. 线程池

## 10.1 线程池简介

**线程池的概念**<font style="color:#333333;">：</font>

线程池就是首先创建一些线程，它们的集合称为线程池。使用线程池可以很好地提高性能，线程池在系统启动时即创建大量空闲的线程，程序将一个任务传给线程池，线程池就会启动一条线程来执行这个任务，执行结束以后，该线程并不会死亡，而是再次返回线程池中成为空闲状态，等待执行下一个任务。它的<font style="color:#F5222D;">主要特点</font>为：**<u>线程复用、控制最大并发数、管理线程</u>**。

------

**线程池的工作机制：**

在线程池的编程模式下，任务是提交给整个线程池，而不是直接提交给某个线程，线程池在拿到任务后，就在内部寻找是否有空闲的线程，如果有，则将任务交给某个空闲的线程。

一个线程同时只能执行一个任务，但可以同时向一个线程池提交多个任务。

------

**使用线程池的原因**

多线程运行时，系统不断的启动和关闭新线程，成本非常高，会过渡消耗系统资源，以及过度切换线程的危险，从而可能导致系统资源的崩溃。这时，线程池就是最好的选择了。

<font style="color:#F5222D;">1）降低资源消耗。重复利用已创建好的线程。</font>

<font style="color:#F5222D;">2）提高响应速度。不需要等待线程创建。</font>

<font style="color:#F5222D;">3）提高线程的管理性。统一分配、调优，监控。</font>



## 10.2 继承图


![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612769983751-96fdb845-0e5e-4ec1-864d-9b8f1effdec8.png)

## 10.3 线程池3大方法：

```java
// 固定容量线程池，类似银行5个窗口，每次只能5个人办理，其他人阻塞等待
ExecutorService threadPool = Executors.newFixedThreadPool(5);
// 单例线程池，线程池只有一个线程，类似银行只有1个窗口，每次只能1个人办理，其他人阻塞等待。
ExecutorService threadPool = Executors.newSingleThreadExecutor();
// 缓存线程池, 线程池随着线程的数量扩容，类似银行N个窗口，有N个线程，同时执行。线程多的时候自动扩容。
ExecutorService threadPool = Executors.newCachedThreadPool(); 
```

**接下来看看线程池的源码：**

<font style="color:#4D4D4D;">首先点开newFixedThreadPool()的源码可以看到：</font>

<font style="color:#4D4D4D;">  
</font>![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612770203880-a83d044d-2bae-4eb9-ac1a-04159ad99b42.png)

<font style="color:#4D4D4D;">接下来点进去newSingleThreadExecutor()的源码可以看到：</font>

<font style="color:#4D4D4D;">  
</font>![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612770247116-edf50d82-5a0f-47ad-a8f2-42c67b9ad19a.png)

<font style="color:#4D4D4D;">接下来点进去newCachedThreadPool()的源码可以看到：</font>

<font style="color:#4D4D4D;">  
</font>![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612770257316-4711a4d3-5bf9-4ed7-909c-09a523037112.png)

<font style="color:#4D4D4D;">综上所述，返回的实际上只是一个</font>`ThreadPoolExecutor`<font style="color:#4D4D4D;">（可以看看继承图），利用构造器传入的不同的参数而已，而且我们也能发现底层是阻塞队列。</font>



<font style="color:#4D4D4D;">同时说明我们也可以通过ThreadPoolExecutor`来创建线程池，Executors只是一个创建线程池的工具类，实际上返回的还是ThreadPoolExecutor。</font>



<font style="color:#4D4D4D;">接着我们继续点开</font>`ThreadPoolExecutor`<font style="color:#4D4D4D;">：</font>

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612795132689-cd941417-a894-4769-8241-f096bf076365.png)

**接着再点进这this，我们可以看到它有七个参数：**

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612795206859-df21d5a6-a3da-4f22-9203-6b3e6b204a0d.png)

## 10.4 threadPoolExecutor七大参数

**七大参数的解释：**

1. **`corePoolSize`**：线程池中的常驻核心线程数。
2. **`maximumPoolSize`**：线程池中能够容纳同时执行的最大线程数，此值必须大于等于 1。
3. **`keepAliveTime`**：多余的空闲线程的存活时间；当前池中线程数超过 corePoolSize 时，当空闲时间达到keepLiveTime 时，多余线程会被销毁直到剩下 corePoolSize 个线程为止。
4. **`unit`**：keepLiveTime 的单位（年月日时分秒）。
5. **`workQueue`**：任务队列，被提交但尚未被执行的任务。
6. **`threadFactory`**：表示生成线程池中工作线程的线程工厂，用于创建线程，一般默认的即可。
7. **`handler`**：拒绝策略，表示当队列满了，并且工作线程大于等于线程池的最大线程数（maximumPoolSize）时如何来拒绝请求执行的 runnable 的策略。



通俗理解为：<font style="color:#4D4D4D;">线程池相当于一家银行，窗口相当于线程</font>，<font style="color:#4D4D4D;">银行有最多有 5 个受理窗口，但是常用的却只有 2 个，而候客区就相当于我们的阻塞队列（</font>**BlockingQueue**<font style="color:#4D4D4D;">），那当我们的阻塞队列满了之后，</font>**handle** <font style="color:#4D4D4D;">拒绝策略出来了，相当于银行门口立了块牌子，上面写着不办理业务了！然后当客户都办理的差不多了，此时多出来 (在 corePool 的基础上扩容的窗口) 的窗口在经过</font> **keepAliveTime** <font style="color:#4D4D4D;">的时间后就关闭了，重新恢复到</font> **corePool** 个受理窗口。</font>



## 10.5 线程池的底层工作原理图

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612797450665-04237b97-ef1b-4edb-8056-33df157813f1.png)



## 10.6 线程池的工作流程

首先线程池接收到任务，先判断核心线程数是否满了，没有满则继续接客，满了就放到阻塞队列，如果阻塞队列没满，这些任务放在阻塞队列，如果满了，就扩容线程数到最大线程数，如果最大线程数也满了，就是我们的拒绝策略。

<font style="color:#4D4D4D;">这就是线程池四大步骤。 </font>**<font style="color:#F5222D;">接客、放入队列，扩容线程，拒绝策略</font>**

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612795919118-5d3bdec9-e0b4-4507-bd1b-fd3d45c79ae7.png)



<font style="color:#4D4D4D;">那我们实际开发中怎么进行配置呢？</font>

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612797931705-a4c2174f-25fe-48f2-9fed-a028ce985eab.png)

**为什么呢？**

<font style="color:#4D4D4D;">对于</font> **newSingleThreadExecutor()**<font style="color:#4D4D4D;">; 而言 LinkedBlockQueue 的长度是 </font>**Integer.MAX_VALUE**<font style="color:#4D4D4D;">，</font>

<font style="color:#4D4D4D;">对于</font> **newCachedThreadPool()**而言，**maximumPool** 的值竟然为 **Integer.MAX_VALUE**<font style="color:#4D4D4D;">！！</font>

<font style="color:#4D4D4D;">两者均会导致</font> **OOM** <font style="color:#4D4D4D;">异常！</font>



## 10.7 自定义线程池

```java
    public static void main(String[] args) {
        ExecutorService threadPool = new ThreadPoolExecutor(
                2, // 常驻线程数
                5, // 最大线程数
                2L, // 空闲回收线程的时间
                TimeUnit.SECONDS, // 空闲时间单位
                new LinkedBlockingQueue<>(3), // 使用Executors工具类自带的线程池实现中的阻塞队列，
                                              // 阻塞队列的数量不定义则使用默认值Integer.MAX_VALUE。
                Executors.defaultThreadFactory(), // 使用Executors工具类自带的线程池实现中的工厂
                new ThreadPoolExecutor.AbortPolicy()); //JDK默认的拒绝策略

        try {
            //模拟10个用户办理业务，但是只有5个受理窗口
            for (int i = 0; i < 9; i++) {
                threadPool.execute(() -> {
                    System.out.println(Thread.currentThread().getName() + "\t" + "办理业务");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            threadPool.shutdown(); //关闭线程池
        }
    }
```

<font style="color:#4D4D4D;">threadPool 是我们自定义的线程池，连接过上面的参数的应该都知道，该线程池最大支持的并发量就应该是maximumPool+Queue的大小，即5+3=8，而超过了大小之后就会报错：</font>`java.util.concurrent.RejectedExecutionException`<font style="color:#4D4D4D;"> 拒绝执行异常。</font>

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612800862388-55502d41-217d-4330-ae55-7a69d96a6686.png)



## 10.8 拒绝策略（4种）

当线程池达到最大线程数，并且等待队列也全部满了，无法处理新的任务请求，就需要合理的拒绝策略来处理。

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612800321428-516486d8-f5b0-4489-8b60-c9e4c1c65dfa.png)

注，第四种应该是直接丢弃提交的这个新任务。



<font style="color:#4D4D4D;">将上面代码的拒绝策略改成第二种</font>`new ThreadPoolExecutor.CallerRunsPolicy()，`<font style="color:#4D4D4D;">返回结果如下：</font>

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612800804237-c75dfd6d-4c38-4c04-b891-b6b22e525645.png)

将多余的无法处理的任务回退给调用者。



<font style="color:#4D4D4D;">第三种</font>`new ThreadPoolExecutor.DiscardOldestPolicy()`<font style="color:#4D4D4D;">：</font>

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612800986127-bf593ebf-01a0-4c3e-b719-da73284714ce.png)

丢弃无法处理的任务，不报错。



<font style="color:#4D4D4D;">第四种 </font>`new ThreadPoolExecutor.DiscardPolicy()`<font style="color:#4D4D4D;">：</font>

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612801181269-6b9f03ad-fbd4-461d-9ba8-a8ee7ea00a50.png)



以上策略均继承自 `RejectedExecutionHandler` 接口。

最后提一句怎么设置 **maximumPoolSize** 合理，

```java
// 打印机器cpu核数
System.out.println(Runtime.getRuntime().availableProcessors()); //8核
```

一般设置为 CPU 核数加 1，例如 8 核的话，设置为 9

# 11. 函数式接口

** java.util.faction 有四大函数式接口**

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612863419661-4024dbb7-9cba-4416-964a-9e009fd0ad58.png)

## 11.1 Function 函数型接口

输入一个参数，内部做一系列处理之后，返回一个新的值



示例1,匿名内部类写法:

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612970123593-27b0d2e0-996d-41c2-8df6-680bd0dc1341.png)

输出：1024



示例2：lambda写法：

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612970245129-343d49b0-fcd2-40e2-acf6-c66bb243117f.png)

输出：3



## 11.2 Predicate 断定型接口

输入一个参数，在内部判定该参数是否满足某种约束，并返回boolean值。



示例1,匿名内部类写法:

    ![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612971611523-ba8bd9b2-4dcb-4362-b15d-13a8acb3545e.png)![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612970868969-a1197dd6-155a-4c54-84e4-d344fa4cee26.png)

输出：fasle



示例2：lambda写法：

    ![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612970918079-33d7ba6d-8d85-48d7-8dae-1c8b748f8828.png)

输出：false



## 11.3 Supplier 供给型接口

没有输入参数，但是有返回值。



示例1,匿名内部类写法:

   ![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612971347458-1ae1156c-3c1d-4861-88e5-f96240dca6f3.png)

输出：null



示例2：lambda写法：

  ![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612971427921-5e45cc0d-4ec4-4377-af81-367407f3b6a1.png)

输出：java0222





## 11.4 Consumer 消费型接口

输入参数，内部做操作，没有返回值。



示例1,匿名内部类写法:

    ![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612971040942-0c3fcdb3-0823-4882-9214-ab2bfdfb99be.png)

输出：方法没有返回值





示例2：lambda写法：

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1612971180417-8057df8f-2450-447b-a6e0-bce702a45cf6.png)

输出：方法没有返回值，但是方法内部打印语句执行。打印出：xiass



# 12. AQS

## 12.1 AQS底层实现

**AbstractQueuedSynchronizer，抽象队列同步器**

JUC 包下面很多 API 都是基于 AQS 来实现的加锁和释放锁等功能的，AQS 是 java 并发包的基础类。

ReentrantLock、ReentrantReadWriteLock 底层都是基于 AQS 来实现的。

ReentrantLock 内部包含了一个 AQS 对象。

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1619586759459-7b892e8d-a012-40cd-bf50-9a2d9fb12d55.png)

<font style="color:#505050;">AQS 定义两种资源共享方式：</font>

+ **<font style="color:#505050;">Exclusive</font>**<font style="color:#505050;">（</font>**<font style="color:#505050;">独占</font>**<font style="color:#505050;">，在特定时间内，只有一个线程能够执行，如ReentrantLock）</font>
+ **<font style="color:#505050;">Share</font>**<font style="color:#505050;">（</font>**<font style="color:#505050;">共享</font>**<font style="color:#505050;">，多个线程可以同时执行，如ReadLock、Semaphore、CountDownLatch）</font>

<font style="color:#4D4D4D;">AQS对象内部有一个核心的变量叫做</font>**<font style="color:#F5222D;">state</font>**<font style="color:#4D4D4D;">，是int类型的，代表了</font>**<font style="color:#F5222D;">加锁的状态</font>**<font style="color:#F5222D;">。初始值为0</font>

<font style="color:#4D4D4D;">另外，这个AQS内部还有一个</font>关键变量 **<font style="color:#F5222D;">exclusiveOwnerThread</font>**<font style="color:#4D4D4D;">，用来记录</font> **<font style="color:#F5222D;">获得锁的线程</font>**<font style="color:#4D4D4D;">，初始化状态下，这个变量是 null。</font>

<font style="color:#4D4D4D;">AQS 中还有一个用来</font><font style="color:#F5222D;">存</font><font style="color:#F5222D;">储获取锁失败线程的队列</font><font style="color:#4D4D4D;">，以及</font> **head**<font style="color:#4D4D4D;"> 和 </font>**tail**<font style="color:#4D4D4D;"> 结点。</font>**<font style="color:#4D4D4D;">是一个</font>**<font style="color:#F5222D;">**双向链表**</font>。



<font style="color:#4D4D4D;">线程1跑过来进行加锁，这个加锁的过程，直接就是通过</font><font style="color:#F5222D;"> CAS 操作将 state 值从 0 变为 1 </font><font style="color:#4D4D4D;">。</font><font style="color:#4D4D4D;">同时</font><font style="color:#F5222D;">设置当前加锁线程是自己</font><font style="color:#4D4D4D;">。</font>

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1619586855582-8de7faca-8337-4286-92ad-2d158dd0ac80.png)



<font style="color:#4D4D4D;">每次线程 1 可重入加锁一次，会判断一下当前加锁线程就是自己，那么他自己就可以可重入多次加锁，每次加锁就是把 state 的值给累加 1。</font><font style="color:#4D4D4D;"> </font>**<font style="color:#F5222D;">state+1 = 2，这就是可重入的核心原理！</font>**



一旦 state 小于 0 表示可重入的次数大于int型最大值，产生溢出了。



接着线程 2 过来加锁，<font style="color:#F5222D;">判断 </font><font style="color:#F5222D;">exclusiveOwnerThread</font> 发现不是自己加的锁，所以线程2此时就是加锁失败。



加锁失败后 ，此时就要<font style="color:#F5222D;">将自己放入队列中来等待</font>，等待线程 1 释放锁之后，自己就可以重新尝试加锁了。

> 如果等待队列里有其他的线程在等待了，就将自己的 node 设置为尾结点。**<font style="color:#F5222D;">基于 CAS 设置尾节点。并且通过“死循环”来保证节点的正确添加。</font>**
>
> 如果之前的等待队列没有等待的线程，那么new一个node，让head和tail指向这个new出来的结点。
>
> 
>
> 判断之前的结点是不是头结点 head，如果是头结点就尝试去获取锁，获取锁成功的话，就把当前线程设置为head
>
> 如果之前的不是头结点，那么就要等待了，等待过程中阻塞当前线程，并且<font style="color:#8C8C8C;">通过</font> **<font style="color:#F5222D;">自旋</font>** <font style="color:#8C8C8C;">来一直尝试获取同步状态，只有前驱节点是头节点才能够尝试获取同步状态</font>，等候之前的线程释放锁后，调用 LockSupport来唤醒。  



**线程锁释放 : **<font style="color:#4D4D4D;">其实很简单 就是将 state-- 直到state = 0，</font> <font style="color:#4D4D4D;">则彻底释放锁，会将“加锁线程”变量也设置为null</font><font style="color:#4D4D4D;">，</font><font style="color:#4D4D4D;">然后通过 LockSupport.unpark() 来唤醒等待队列中的下一个结点。</font>

> 这块唤醒的是头结点的后继结点而不是头结点，
>
> 因为我们上面调用addWaiter方法的时候，如果等待队列里面没有等待线程，那么直接new 了一个Node 然后 head 和 tail 都指向这个 node，换句话说这个头结点只是用来占位的，所以要从头结点的下一个结点开始唤醒。
>



<font style="color:#F5222D;background-color:#FFFB8F;"> ReentrantLock又分为 **公平锁** 和 **非公平锁**。 ReentrantLock 是基于独占锁实现。</font>

<font style="color:#4D4D4D;"></font>


## 12.2 AQS的非公平锁与同步队列的FIFO冲突吗？

<font style="color:#4D4D4D;"></font>

<font style="color:#4D4D4D;">公平锁与非公平锁的含义都很明白，</font>`公平锁必须排队获取锁，锁的获取顺序完全根据排队顺序而来，而非公平就是谁抢到是谁的`<font style="color:#4D4D4D;">。</font>但是！同步队列不是 FIFO 的吗，它们入队排好顺序并且按照顺序一个一个的醒来，只有醒来的才有机会去获取锁，才能去执行 try 方法，这不还是公平的吗？



**解答：**

线程在 do 方法中获取锁时，会先加入同步队列，之后根据情况再陷入阻塞。当阻塞后的节点一段时间后醒来时，这时候来了新的更多的线程来抢锁，这些新线程还没有加入到同步队列中去，也就是在 try 方法中获取锁。

在公平锁下，这些新线程会发现同步队列中存在节点等待，那么这些新线程将无法获取到锁，乖乖去排队；

而在非公平锁下，这些新线程会跟排队苏醒的线程进行锁争抢，失败的去同步队列中排队。

因此这里的公平与否，针对的其实是苏醒线程与还未加入同步队列的线程

而对于已经在同步队列中阻塞的线程而言，它们内部自身其实是公平的，因为它们是按顺序被唤醒的，这是根据 AQS 节点唤醒机制和同步队列的 FIFO 特性决定的。

<font style="color:#4D4D4D;"></font>

## 12.3 共享锁与独占锁的对比

与AQS的独占功能不同，当锁被头节点获取后，独占功能是只有头节点获取锁，其余节点的线程继续沉睡，等待锁被释放后，才会唤醒下一个节点的线程，而共享功能是只要头节点获取锁成功，就在唤醒自身节点对应的线程的同时，继续唤醒 AQS 队列中的下一个节点的线程，每个节点在唤醒自身的同时还会唤醒下一个节点对应的线程，以实现共享状态的“向后传播”，从而实现共享功能。



<font style="color:#4D4D4D;">CountDownLatch、</font><font style="color:#4D4D4D;">emaphore、 ReentrantReadWriteLock 都是共享锁的实现</font>



# 13.CAS 底层实现

<font style="color:#4D4D4D;">CAS的全称为 </font>**Compare-And-Swap** <font style="color:#4D4D4D;"> ,它是一条 CPU 并发原语。它的功能是判断内存某个位置的值是否为预期值,如果是则更新为新的值, 这个过程是原子的。</font>

<font style="color:#4D4D4D;"></font>

**<font style="color:#F33B45;">CAS 操作包含三个操作数 —— 内存位置（V）、预期原值（A）和新值(B)。</font>**<font style="color:#4D4D4D;"> 如果内存位置的值与预期原值相匹配，那么处理器会自动将该位置值更新为新值 。否则，处理器不做任何操作。</font>

<font style="color:#4D4D4D;"></font>



+ **<font style="color:#F33B45;">变量ValueOffset</font>**<font style="color:#3399EA;">：</font><font style="color:#3399EA;">它是该变量在内存中的偏移地址,因为UnSafe就是根据内存偏移地址获取数据的。</font>
+ **<font style="color:#F33B45;">变量value</font>**<font style="color:#3399EA;">：</font><font style="color:#3399EA;">被volatile修饰, 保证了多线程之间的可见性。</font>

<font style="color:#4D4D4D;"></font>

<font style="color:#4D4D4D;">CAS通过调用 JNI 的代码实现的。JNI：`Java Native Interface` 为 JAVA 本地调用，允许 java 调用其他语言。</font>



<font style="color:#4D4D4D;">Unsafe 类中的 compareAndSwapInt，是一个本地方法，该方法的实现位于</font> `unsafe.cpp` <font style="color:#4D4D4D;">中，而compareAndSwapInt 就是借助 C 来调用 CPU 底层指令实现的。</font>

<font style="color:#4D4D4D;"></font>

1. 先想办法拿到变量 value 在内存中的地址。
2. 通过 `Atomic::cmpxchg` 实现比较替换，其中参数 x 是即将更新的值，参数 e 是原内存的值。



<font style="color:#4D4D4D;">CAS 的原子性实际上是 CPU 实现的. 其实在这一点上还是有排他锁的. 只是比起用 synchronized , 这里的排他时间要短的多. 所以在多线程情况下性能会比较好.</font>

<font style="color:#4D4D4D;"></font>

**缺点：**

<font style="color:#4D4D4D;">CAS 虽然很高效的解决原子操作，但是 CAS 仍然存在三大问题。ABA 问题，循环时间长开销大和只能保证一个共享变量的原子操作。</font>

<font style="color:#4D4D4D;"></font>

# 14. synchronized底层原理

## synchronized 作用

+ 原子性：synchronized 保证语句块内操作是原子的（只能有一个线程去访问）。
+ 可见性：synchronized 保证可见性（在执行 unlock 之前，必须先把此变量刷回**主内存**）。
+ 有序性：synchronized 保证有序性（一个变量在同一时刻只允许一条线程对其进行 lock 操作）。



## synchronized 使用

+ 修饰实例方法，对当前实例对象加锁
+ 修饰静态方法，多当前类的Class对象加锁
+ 修饰代码块，对synchronized括号内的对象加锁



## 实现原理

Synchronized是通过对象内部的一个叫做监视器锁（**<font style="color:#F5222D;">monitor</font>**）来实现的，监视器锁本质又是依赖于底层的操作系统的 Mutex Lock（互斥锁）来实现的。而操作系统实现线程之间的切换需要从用户态转换到核心态，这个成本非常高，状态之间的转换需要相对比较长的时间，这就是为什么 Synchronized 效率低的原因。因此，这种依赖于操作系统 Mutex Lock 所实现的锁我们称之为“重量级锁”。



<font style="color:#4D4D4D;">jvm 基于进入和退出 Monitor 对象来实现</font>**<font style="color:#4D4D4D;">方法同步</font>**<font style="color:#4D4D4D;">和</font>**<font style="color:#4D4D4D;">代码块同步</font>**<font style="color:#4D4D4D;">。</font>



**<font style="color:#FA541C;">方法级的同步 </font>**是隐式，即无需通过字节码指令来控制的，它实现在方法调用和返回操作之中。JVM可以从方法常量池中的<font style="background-color:#FFFB8F;">方法表结构中的 </font><font style="color:#F5222D;background-color:#FFFB8F;">ACC_synchronized</font><font style="color:#F5222D;background-color:#FFFB8F;"> </font><font style="color:#F5222D;background-color:#FFFB8F;">访问标志</font><font style="background-color:#FFFB8F;">区分一个方法是否同步方法</font>。当方法调用时，调用指令会检查方法的 ACC_synchronized 访问标志是否被设置，如果设置了，执行线程将先持有 monitor（虚拟机规范中用的是管程一词）， 然后再执行方法，最后再方法完成时释放 monitor。

![](https://cdn.nlark.com/yuque/0/2021/jpeg/12493416/1620380947005-8f7b5ed7-99bf-4166-91e2-008bdca83513.jpeg)



**<font style="color:#FA541C;">代码块的同步 </font>**是<font style="background-color:#FFFB8F;">利用 </font>**<font style="color:#F5222D;background-color:#FFFB8F;">monitorenter</font><font style="color:#F5222D;background-color:#FFFB8F;">  </font>**<font style="background-color:#FFFB8F;">和 </font><font style="color:#F5222D;background-color:#FFFB8F;">**monitorexit**</font><font style="background-color:#FFFB8F;"> 这两个</font><font style="color:#F5222D;background-color:#FFFB8F;">字节码指令</font>。它们分别位于同步代码块的开始和结束位置。当 jvm 执行到 monitorenter 指令时，当前线程试图获取 monitor 对象的所有权，如果未加锁或者已经被当前线程所持有，就把锁的计数器+1；当执行 monitorexit 指令时，锁计数器 -1；当锁计数器为 0 时，该锁就被释放了。如果获取 monitor 对象失败，该线程则会进入阻塞状态，直到其他线程释放锁。

![](https://cdn.nlark.com/yuque/0/2021/jpeg/12493416/1620381080390-f9238bb7-633d-4d53-aeb0-ec5b64e35bf3.jpeg)



## 理解 Java 对象头

在JVM中，对象在内存中的布局分为三块区域：**对象头、实例数据、对齐填充**

![](https://cdn.nlark.com/yuque/0/2021/jpeg/12493416/1620381331413-e92087b6-19aa-4945-8301-38276751f6b1.jpeg)



**通过 JOL 分析 Java 对象布局**：

一个Java对象，通过 ClassLayout._parseInstance_(test).toPrintable()；打印出来的对象信息如下：

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1620381222684-f16a0ee5-5b93-478e-92fe-de4636e55fa5.png)



**实例变量**：存放类的属性数据信息，包括父类的属性信息，如果是数组的实例部分还包括数组的长度，这部分内存按4字节对齐。



**填充数据**：由于虚拟机要求<font style="color:#F5222D;background-color:#FFFB8F;">对象起始地址必须是</font><font style="color:#F5222D;background-color:#FFFB8F;">8个字节的整数倍</font>。填充数据不是必须存在的，仅仅是为了字节对齐。



> <font style="color:#F5222D;">在64位Java虚拟机上面，对象大小必须是8的倍数。</font>



**对象头：**

<font style="color:#000000;">32 位和 64 位虚拟机表现不同，这里以主流的 64 位进行说明。一个对象在内存中存储必须是 8 字节的整数倍，其中对象头占了 12 字节。</font>

<font style="color:#000000;">对象头分为了两部分：</font>

+ <font style="color:#000000;">第一部分是</font><font style="color:#000000;">：</font><font style="color:#F5222D;background-color:#FFFB8F;">指向方法区元数据的</font>**<font style="color:#F5222D;background-color:#FFFB8F;">类型指针</font>**<font style="color:#F5222D;background-color:transparent;">（</font><font style="color:#000000;">klass point），固定占用4字节32位；</font>
+ <font style="color:#000000;">第二部分是：</font><font style="color:#F5222D;background-color:#FFFB8F;">用于存储对象hashcode、分代年龄、同步状态、线程id等信息的</font>**<font style="color:#F5222D;background-color:#FFFB8F;">mark word</font>**<font style="color:#000000;">，占用8字节64位。</font>

> 对象头里面包含了 GC 状态，用 4bit 的二进制位存储，因为 4bit 最大是 1111，也就是 15，所以 GC 幸存区的复制算法是 15 次之后进入老年代。

**<font style="color:#F5222D;"></font>**

**<font style="color:#F5222D;">每一个 Java 对象自打娘胎里出来就带了一把看不见的锁，它叫做内部锁或者 Monitor 锁。</font>**

<font style="color:#121212;">Monitor 是线程私有的数据结构，每一个线程都有一个可用 monitor record 列表，同时还有一个全局的可用列表。每一个被锁住的对象都会和一个monitor关联（对象头的MarkWord中的LockWord指向monitor的起始地址），同时monitor中有一个Owner字段存放拥有该锁的线程的唯一标识，表示该锁被这个线程占用。</font><font style="color:#121212;">其结构如下：</font>

![](https://cdn.nlark.com/yuque/0/2021/jpeg/12493416/1620438685749-58f77d00-4ae7-4adf-9da6-051565c5972e.jpeg)

+ Owner：初始时为NULL表示当前没有任何线程拥有该monitorrecord，当线程成功拥有该锁后保存线程唯一标识，当锁被释放时又设置为NULL；
+ EntryQ:关联一个系统互斥锁（semaphore），阻塞所有试图锁住monitorrecord失败的线程。
+ RcThis:表示blocked或waiting在该monitorrecord上的所有线程的个数。
+ Nest:用来实现重入锁的计数。
+ HashCode:保存从对象头拷贝过来的HashCode值（可能还包含GCage）。
+ <font style="color:#121212;">Candidate:用来避免不必要的阻塞或等待线程唤醒，因为每一次只有一个线程能够成功拥有锁，如果每次前一个释放锁的线程唤醒所有正在阻塞或等待的线程，会引起不必要的上下文切换（从阻塞到就绪然后因为竞争锁失败又被阻塞）从而导致性能严重下降。Candidate只有两种可能的值0表示没有需要唤醒的线程1表示要唤醒一个继任线程来竞争锁。</font>





<font style="color:#121212;">我们知道 synchronized 是重量级锁，效率不怎么滴，同时这个观念也一直存在我们脑海里，不过在 jdk1.6 中对 synchronize 的实现进行了各种优化，使得它显得不是那么重了，那么JVM采用了那些优化手段呢？</font>

<font style="color:#121212;"></font>

<font style="color:#121212;">jdk1.6 对锁的实现引入了大量的优化，如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销。</font>

<font style="color:#121212;"></font>

锁主要存在四种状态，依次是：无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态，他们会随着竞争的激烈而逐渐升级。注意锁可以升级不可降级，这种策略是为了提高获得锁和释放锁的效率。



**<font style="color:#F5222D;">Java对象的状态：</font>**

1. **无锁：对象刚new出来的时候**
2. **偏向锁：对象只有一个线程获取锁的时候，并且没有其他线程使用。**
3. **轻量锁：多线程交替执行。但不存在锁竞争。**
4. **重量锁：多个线程发生锁竞争。**
5. GC标记 ：标记垃圾回收。

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1620384819844-52aaa141-2495-4c56-b5b2-a7d558f2994b.png)





<font style="color:#4D4D4D;">当锁对象第一次被线程获取的时候，虚拟机会把对象头中的标志位设置为“01”，即偏向模式。同时使用 CAS 操作把获取到这个锁的线程 ID 记录在对象的 Mark Word 中，如果 CAS 操作成功。持有偏向锁的线程以后每次进入这个锁相关的同步块时，虚拟机都可以不再进行任何同步操作。</font>

<font style="color:#4D4D4D;"></font>

## 自旋锁

<font style="color:#121212;">所谓自旋锁，就是让该线程等待一段时间，不会被立即挂起，看持有锁的线程是否会很快释放锁。怎么等待呢？执行一段无意义的循环即可（自旋）。</font>

<font style="color:#121212;"></font>

<font style="color:#121212;">自旋等待不能替代阻塞，</font><font style="color:#121212;">如果持有锁的线程很快就释放了锁，那么自旋的效率就非常好，反之，自旋的线程就会白白消耗掉处理的资源，反而会带来性能上的浪费。所以说，自旋等待的时间（自旋的次数）必须要有一个限度，如果自旋超过了定义的时间仍然没有获取到锁，则应该被挂起。</font>

<font style="color:#121212;"></font>

<font style="color:#121212;">自旋锁在JDK1.4.2中引入，默认关闭，但是可以使用-XX:+UseSpinning开开启，在JDK1.6中默认开启。同时自旋的默认次数为10次，可以通过参数-XX:PreBlockSpin来调整；</font>

<font style="color:#121212;"></font>

<font style="color:#121212;">如果通过参数-XX:preBlockSpin来调整自旋锁的自旋次数，会带来诸多不便。假如我将参数调整为10，但是系统很多线程都是等你刚刚退出的时候就释放了锁（假如你多自旋一两次就可以获取锁），你是不是很尴尬。于是JDK1.6引入自适应的自旋锁，让虚拟机会变得越来越聪明。</font>

<font style="color:#121212;"></font>

## <font style="color:#121212;">适应自旋锁</font>

<font style="color:#121212;">所谓自适应就意味着自旋的次数不再是固定的，它是由前一次在同一个锁上的自旋时间及锁的拥有者的状态来决定。它怎么做呢？线程如果自旋成功了，那么下次自旋的次数会更加多，因为虚拟机认为既然上次成功了，那么此次自旋也很有可能会再次成功，那么它就会允许自旋等待持续的次数更多。反之，如果对于某个锁，很少有自旋能够成功的，那么在以后要或者这个锁的时候自旋的次数会减少甚至省略掉自旋过程，以免浪费处理器资源。</font>

<font style="color:#121212;"></font>

## <font style="color:#121212;">锁消除</font>

<font style="color:#121212;">为了保证数据的完整性，我们在进行操作时需要对这部分操作进行同步控制，但是在有些情况下，JVM 检测到不可能存在共享数据竞争，这是 JVM 会对这些同步锁进行锁消除。</font>

<font style="color:#121212;"></font>

<font style="color:#121212;">我们在使用一些 JDK 的内置 API 时，如 StringBuffer、Vector、HashTable 等，这个时候会存在隐形的加锁操作，</font><font style="color:#121212;">比如 StringBuffer 的 append() 方法，Vector 的 add() 方法。</font>

<font style="color:#121212;"></font>

### <font style="color:#121212;">锁粗化</font>

<font style="color:#121212;">就是将多个连续的加锁、解锁操作连接在一起，扩展成一个范围更大的锁。如上面实例：vector 每次 add 的时候都需要加锁操作，JVM 检测到对同一个对象（vector）连续加锁、解锁操作，会合并一个更大范围的加锁、解锁操作，即加锁解锁操作会移到 for 循环之外。</font>



# 15. volatile 底层实现

volatile修饰的共享变量进行写操作的时候会多出 **<font style="color:#F5222D;">Lock前缀的指令</font>**，该指令在多核处理器下会引发两件事情：

1. 将当前处理器缓存行数据刷写到系统主内存。
2. 这个刷写回主内存的操作会使其他 CPU 缓存的该共享变量内存地址的数据无效。



这样就保证了多个处理器的缓存是一致的，对应的处理器发现自己缓存行对应的内存地址被修改，就会将当前处理器缓存行设置无效状态，当处理器对这个数据进行修改操作的时候会重新从主内存中把数据读取到缓存里。<font style="color:#4D4D4D;"></font>

> <font style="color:#8C8C8C;">指令重排序对单线程没有什么影响，它不会影响程序的运行结果，反而会优化执行性能，但会影响多线程的正确性。</font>
>
> <font style="color:#8C8C8C;"></font>
>
> <font style="color:#8C8C8C;">Java 因为指令重排序，优化我们的代码，让程序运行更快，也随之带来了多线程下，指令执行顺序的不可控。</font>
>
> <font style="color:#8C8C8C;"></font>
>
> **<font style="color:#FA541C;">volatile的底层是通过lock前缀指令、内存屏障来实现的。</font>**

**<font style="color:#FA541C;"></font>**

lock 前缀指令实际上相当于一个内存屏障（也称内存栅栏），内存屏障会提供 3 个功能：

+ 它确保指令重排序时不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面；即在执行到内存屏障这句指令时，在它前面的操作已经全部完成；
+ 它会强制将对缓存的修改操作立即写入主存；
+ 如果是写操作，它会导致其他CPU中对应的缓存行无效。





























