# 1.线程

**什么是线程？**

· 线程是一个程序内部的一条执行流程

· 程序中如果只有一条执行流程，那这个程序就是单线程程序

**什么是多线程？**

·多线程是指从软硬件上实现的多条执行流程的技术(多条线程由cup负责调度执行)



# 2.创建和使用

## 2.1继承Tread类

```java
/**
 * 1.让子类继承Tread线程类
 */
public class MyThread extends Thread{
    //2.重写Tread的run方法


    @Override
    public void run() {
        //描述线程的执行任务
        for (int i = 0;i < 5;i++){
            System.out.println("字线程执行: "+i);
        }
    }
}
```

```java
public class Demo1 {
    //main方法是由一条默认的主线程执行的
    public static void main(String[] args) {
        //3.创建线程对象代表一个线程
        Thread t = new MyThread();
        //4.启动多线程(自动执行run方法)
        t.start();
        for (int i = 0;i < 5;i++){
            System.out.println("主线程线程执行: "+i);
        }
    }
}
```

**优点**：编码简单

**缺点**：线程类已经继承了Thread，无法继承别的类，不利于功能的拓展 

**注意**：     启动一定用start，用run无法启动线程，相当于方法调用，还是单线程    **start向cup注册了线程**

​		**不要把主线程的任务放到子线程中去**

## 2.2实现Runnable接口

### 2.2.1普通写法

```java
//1.定义一个任务类，实现Runnable接口
public class MyRunnable implements Runnable{
    //2.重写Runnable的run方法
    @Override
    public void run() {
        //线程要执行的任务
        for (int i = 1;i <= 5;i++){
            System.out.println("子线程输出: "+i);
        }
    }
}
```

```java
public class Demo2 {
    public static void main(String[] args) {
        //3.创建任务对象
        Runnable target = new MyRunnable();
        //4.把任务对象交给线程对象
        new Thread(target).start();

        for (int i = 1;i <= 5;i++){
            System.out.println("主线程输出: "+i);
        }

    }
}
```

**优点**：任务类只是实现接口，可以继续继承其他类，实现其他接口，拓展性更强

**缺点**：需要多创建一个Runnable对象

### 2.2.2匿名内部类的写法

```java
public class Demo3 {
    public static void main(String[] args) {
        //1.直接创建Runnable接口的匿名内部类形式（任务对象）
        Runnable target = new Runnable() {
            @Override
            public void run() {
                //线程要执行的任务
                for (int i = 1;i <= 5;i++){
                    System.out.println("子线程输出: "+i);
                }
            }
        };
        new Thread(target).start();
        //2.直接在Tread中创建
        new Thread(new Runnable() {
            @Override
            public void run() {
                //线程要执行的任务
                for (int i = 1;i <= 5;i++){
                    System.out.println("子线程输出: "+i);
                }
            }
        }).start();
        //3.lambda方式
        new Thread(() -> {
                //线程要执行的任务
            for (int i = 1;i <= 5;i++){
                System.out.println("子线程输出: "+i);
            }
        }).start();
    }
}
```

## 2.3实现Callable接口

**前两种方式实现的多线程由一个共同问题：假如线程执行完毕后有一些数据需要返回，他们重写的run方法均不能直接返回结果**

​	

```java
public class MyCallable implements Callable<String> {//泛型内为返回值类型
    //2.重写call方法
    private int n;
    public MyCallable(int n){
        this.n = n;
    }
    @Override
    public String call() throws Exception {
        //描述线程任务，返回线程执行返回后的结果
        int sum = 0;
        for (int i =1;i < n;i++){
            sum += i;
        }
        return "sum: " + sum;
    }
}
```

```java
public class demo4 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        //3.创建一个Callable的对象
        Callable<String> myCallable = new MyCallable(3 );
        //4.把Callable对象封装成一个FutureTask对象（任务对象）
        FutureTask<String> f1 = new FutureTask<>(myCallable);
        //这是一个任务对象，实现类Runnable对象
        //可以在线程执行完毕后，用未来任务对象调用get方法获取线程执行完的结果
        //5.把任务对象交给线程对象
        new Thread(f1).start();
        //6.获取线程执行完成后的结果
        String s = f1.get();//如果代码执行到这里，加入上面的线程还没有执行完毕，代码会暂停，等待上面线程执行完毕
        System.out.println(s);
    }
}
```

**优点**：线程任务类只是实现接口，可以继续继承类和实现接口，拓展性强，可以在线程执行完毕后去获取线程执行的结果

**缺点**：编码复杂

# 3.常用方法和构造器

| Thread的常用方法                         | **说明**                                      |
| ---------------------------------------- | --------------------------------------------- |
| **public void run()**                    | 线程的任务方法                                |
| **public void start()**                  | 启动线程                                      |
| **public String getName()**              | 获取当前线程的名称，线程名称默认是Thread-索引 |
| **public void setName(String name)**     | 为线程设置名称                                |
| **public static Thread currentThread()** | 获取当前执行的线程对象                        |
| **public static void sleep(long time)**  | 让当前执行的线程休眠多少毫秒后，再继续执行    |
| **public final void join()**             | 让调用当前这个方法的线程先执行完              |

| Thread常用的构造器                         | 说明                                       |
| ------------------------------------------ | ------------------------------------------ |
| public Thread(String name)                 | 可以为当前线程指定名称                     |
| public Thread(Runnable target)             | 封装Runnable对象成为线程对象               |
| public Thread(Runnable target,String name) | 封装Runnable对象为线程对象，并指定线程名称 |

```java
public class MyThread extends Thread{
    //2.重写Tread的run方法
    
    public MyThread(String name){
        super(name);
    }

    @Override
    public void run() {
        //描述线程的执行任务
        for (int i = 0;i < 5;i++){
            System.out.println("字线程执行: "+i);
        }
    }
}
```

```java
public class Demo1 {
    //main方法是由一条默认的主线程执行的
    public static void main(String[] args) {
        //3.创建线程对象代表一个线程
        Thread t = new MyThread("name1");
        //4.启动多线程(自动执行run方法)
        t.start();
        for (int i = 0;i < 5;i++){
            System.out.println("主线程线程执行: "+i);
        }
    }
}
```

# 4.线程安全

**多个线程，同时操作同一个共享资源的时候，可能出现业务安全问题**

(存在多个线程同时执行，同时访问一个共享资源，存在修改该共享资源)

# 5.线程同步

**解决线程安全问题的方案**

*让多个线程实现先后依次访问共享资源*



**加锁**：

**每次只允许一个线程加锁，加锁后才能进行访问，访问完毕后自动解锁，然后其他线程才能再加锁进来**

## 5.1同步代码块

把访问共享资源的核心代码块给上锁，以此保证安全

```java
synchronized(同步锁){
		访问共享资源的核心代码
}
```

**每次只允许一个线程加锁后进入，执行完毕后自动解锁，其他线程才可以进来执行**



**注意事项**：

对于当前同时执行的线程来说，同步锁必须是同一把（同一个对象），否则会出bug



```java
public void withdraw(double money){
    String name = Thread.currentThread().getName();
  
    //this正好代表这个对象本身，比如说账户对象
    synchronized (this){ //🔒是一个对象，确定锁了同一个对象
        if (this.money >= money){
            System.out.println(name+"来取钱"+money);
            this.money -= money;
            System.out.println("取钱后余额: "+this.money);
        }else{
            System.out.println(name+"余额不足");
        }
    }
}
```

**当一个对象去获取这个锁的时候，在对象底层注入一个标记，代表已经被加锁了**



```java
public static void test(){
	synchronized(Account.class){
		//对于静态方法，使用类名.class保证唯一性
	}
}
```

## 5.2同步方法

把访问共享资源的核心方法上锁，以此保证线程安全

```java
修饰符 synchronized 返回值类型 方法名称（形参列表）{
		操作共享资源的代码
}
```

```java
//同步方法
public synchronized void withdraw2(double money){
    String name = Thread.currentThread().getName();
   
    if (this.money >= money){
        System.out.println(name+"来取钱"+money);
        this.money -= money;
        System.out.println("取钱后余额: "+this.money);
    }else{
        System.out.println(name+"余额不足");
    }
    
}
```





**其底层也是由隐式锁对象的，只是锁的范围是整个方法代码**



**可读性方面，同步方法更好**

**同步代码块的范围更小，性能更好**

## 5.3Lock锁

通过它可以创建出锁对象进行加锁和解锁

更灵活，更方便，更强大

Lock是一个接口，采用其实现类 reentranLock来构建Lock对象



```java
//创建一个锁对象,每一个账户对象都应该有一个自己的锁
private final Lock lk = new ReentrantLock();
```

```java
   public void withdraw3(double money){
        String name = Thread.currentThread().getName();
        lk.lock();//加锁
     
        try {
            if (this.money >= money) {
                System.out.println(name + "来取钱" + money);
                this.money -= money;
                System.out.println("取钱后余额: " + this.money);
            } else {
                System.out.println(name + "余额不足");
            }
        }catch (Exception e){
            throw new RuntimeException(e);
        }finally {
            lk.unlock();//解锁
        }
    }
```

# 6.线程通信

## 6.1定义

**当多个线程共同操作共享的资源的时候，线程间通过某种方式相互告知自己的状态，以相互协调，并避免无效的资源争夺**

## 6.2常见模型

**生产者与消费者模型**



生产者线程负责生产数据

消费者线程负责消费生产者生产的数据

生产者生产完数据应该等待自己，通知消费者消费，

消费者消费完数据，也应该等待自己，再通知生产者生产



### **Object类的等待和唤醒方法**

| 方法名称         | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| void wait()      | 让当前线程等待并释放所占的🔒，直到另一个线程调用notify()方法，或者notifyAll()方法 |
| void notify()    | 唤醒正在等待的单个线程                                       |
| void notifyAll() | 唤醒正在等待的所有线程                                       |

**应该使用锁对象进行调用**



```java
public class Desk {
    private final ArrayList<String> list = new ArrayList<>();
    
    public synchronized void put(){
        try{
            String name = Thread.currentThread().getName();
            
            if (list.isEmpty()){
                list.add(name+"做的肉包子");
                System.out.println(name+"做了一个肉包子");
                Thread.sleep(2000);
            } //唤醒别人，等待自己
            this.notifyAll();
            this.wait();
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }
    
    public synchronized void get(){
        try {
            String name = Thread.currentThread().getName();
            if (!list.isEmpty()) {
                System.out.println(name + "吃了" + list.get(0));
                list.clear();
                Thread.sleep(1000);
            }
            this.notifyAll();
            this.wait();
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }
}
```

# 7.线程池

## 7.1认识线程池

线程池就是一个可以复用线程的技术



**如果不创建线程池，比如用户每发起一次请求，后台就需要创建一个新线程来处理，下次新任务来了肯定又要创建新线程处理，而创建新线程的开销是很大的，并且请求过多时，肯定会产生大量的线程出来，这样会严重影响系统的性能**

![截屏2024-04-15 17.42.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-15%2017.42.39.png)

## 7.2创建线程池

**代表线程池的接口：ExecutorService**

*ThreadPoolExecutor --> 实现类*



### 7.2.1threadPoolExecutor构造器

```java
public ThreadPoolExecutor(int corePoolSize, //核心线程的数量（可互用）
                          int maximumPoolSize,//最大线程数量
                          long keepAliveTime,//临时线程的存活时间
                          TimeUnit unit,//指定临时线程存活的时间单位(second,min,hour,day)
                          BlockingQueue<Runnable> workQueue,//指定线程池的任务队列
                          ThreadFactory threadFactory,//指定线程工厂（为线程池创建线程）
                          RejectedExecutionHandler handler) //指定线程池的任务拒绝策略（线程都在忙，任务队列也满了的时候，新任务来了怎么办）{
    if (corePoolSize < 0 ||
        maximumPoolSize <= 0 ||
        maximumPoolSize < corePoolSize ||
        keepAliveTime < 0)
        throw new IllegalArgumentException();
    if (workQueue == null || threadFactory == null || handler == null)
        throw new NullPointerException();
    this.acc = System.getSecurityManager() == null ?
            null :
            AccessController.getContext();
    this.corePoolSize = corePoolSize;
    this.maximumPoolSize = maximumPoolSize;
    this.workQueue = workQueue;
    this.keepAliveTime = unit.toNanos(keepAliveTime);
    this.threadFactory = threadFactory;
    this.handler = handler;
}
```



```java
public static void main(String[] args) {
    ExecutorService s = new ThreadPoolExecutor(3,5,8, TimeUnit.MINUTES,
            new LinkedBlockingDeque<>());
    //LinkedBlockingDeque大小不限
    ExecutorService ss = new ThreadPoolExecutor(3,5,8, TimeUnit.MINUTES,
            new ArrayBlockingQueue<>(4), Executors.defaultThreadFactory(),new ThreadPoolExecutor.AbortPolicy());
    //拒绝策略为新任务来了抛异常
}
```



### 7.3.2临时线程什么时候创建

​	新任务提交时发现核心线程都在忙，任务队列也满了，并且还可以创建临时线程，此时才会创建临时线程

### 7.3.3什么时候开始拒绝新任务

​	核心线程和临时线程都在忙，任务队列也满了，新的任务过来的时候才会开始拒绝任务

### 7.3.4ExecutorService常用方法

| 方法名称                           | 说明                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| void execute(Runnable command)     | 执行Runnable方法                                             |
| Future<T> submit(Callable<T> task) | 执行Callable任务，返回未来任务对象，用于获取线程返回的结果   |
| void shutdown()                    | 等全部任务执行完毕后，再关闭线程池                           |
| List<Runnable> shutdownNow()       | 立刻关闭线程池，停止正在执行的任务，并返回队列中未执行的任务 |

### 7.3.5拒绝策略

| 策略                                   | 详解                                                   |
| -------------------------------------- | ------------------------------------------------------ |
| ThreadPoolExecutor.AbortPolicy         | 丢弃任务并抛出RejectedExecutionException异常，默认策略 |
| ThreadPoolExecutor.DiscardPolicy       | 丢弃任务，但是不抛出异常，不推荐                       |
| ThreadPoolExecutor.DiscardOldestPolicy | 抛弃队列中等待最久的任务，然后把当前任务加入队列       |
| ThreadPoolExecutor.CallerRunsPolicy    | 由主线程负责调用任务的run()方法从而绕过线程池直接执行  |

## 7.3处理Runnable任务

```java
public static void main(String[] args) {
	//创建线程和复用线程的例子
    ExecutorService pool = new ThreadPoolExecutor(3,5,8, TimeUnit.MINUTES,
            new ArrayBlockingQueue<>(4), Executors.defaultThreadFactory(),new ThreadPoolExecutor.AbortPolicy());
    //拒绝策略为新任务来了抛异常
    Runnable target =  new MyRunnable();
    pool.execute(target);//线程池自动创建一个新线程，自动处理这个任务
    pool.execute(target);//线程池自动创建一个新线程，自动处理这个任务
    pool.execute(target);//线程池自动创建一个新线程，自动处理这个任务
    pool.execute(target);//复用前面的核心线程
    pool.execute(target);//复用前面的核心线程
    pool.shutdown();
}
```

```java
public static void main(String[] args) {
		//演示创建临时线程和拒绝策略
    ExecutorService pool = new ThreadPoolExecutor(3,5,8, TimeUnit.MINUTES,
            new ArrayBlockingQueue<>(4), Executors.defaultThreadFactory(),new ThreadPoolExecutor.AbortPolicy());
    //拒绝策略为新任务来了抛异常
    Runnable target =  new MyRunnable();
    pool.execute(target);//线程池自动创建一个新线程，自动处理这个任务
    pool.execute(target);//线程池自动创建一个新线程，自动处理这个任务
    pool.execute(target);//线程池自动创建一个新线程，自动处理这个任务
    pool.execute(target);
    pool.execute(target);
    pool.execute(target);
    pool.execute(target);//队列加满了
    //到了临时线程创建的时机了
    pool.execute(target);
    pool.execute(target);
    //拒绝时机
    pool.execute(target);//这里用的是抛异常
    pool.shutdown();
}
```

## 7.4处理Callable任务

```java
public static void main(String[] args) throws ExecutionException, InterruptedException {
    ExecutorService pool = new ThreadPoolExecutor(3,5,8, TimeUnit.MINUTES,
            new ArrayBlockingQueue<>(4), Executors.defaultThreadFactory(),new ThreadPoolExecutor.AbortPolicy());

    Callable<String> myCallable = new MyCallable(100);
    
    Future<String> f1 = pool.submit(myCallable);
    
    System.out.println(f1.get());
}
```

## 7.5Executors工具类实现线程池

### 7.5.1定义

**Executors是一个线程池的工具类，提供了很多静态方法用于返回不同特点的线程池对象**

### 7.5.2常用方法

| name of function                                             | description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| public static ExecutorService newFixedThreadPool(int nThreads) | 创建固定线程数量的线程池，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程替代它 |
| public static ExecutorService newSingleThreadExecutor()      | 创建只有一个线程的线程池对象，如果该线程出现异常而结束，那么线程池会补充一个新线程 |
| public static ExecutorService newCachedThreadPool()          | 线程数量随着任务增加而增加，如果线程任务执行完毕且空闲了60s则会被回收掉 |
| public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize) | 创建一个线程池，可以实现在给定的延迟后运行任务，或者定期执行任务 |



**计算密集型任务：核心线程数量 = CPU核数量 + 1 // 一直在计算**

**IO密集型任务：核心线程数量 = CPU核数 * 2  //通信**



**大型并发系统环境中使用Executors如果不注意可能会出现系统风险**



 无限创建线程 和 根据数量创建线程的静态线程池创建方法 可能会因为堆积大量请求或者创建大量线程，从而导致OOM（内存溢出）

# 8.进程和并行

## 8.1进程

**正在运行的程序(软件)就是一个独立的进程**



**线程是属于进程的，一个进程可以同时执行很多个线程**



**进程中的多个线程其实是并发和并行执行的**

## 8.2并发

**进程中的线程是由CPU负责执行调度的，但CPU能同时处理线程的数量有限，为了保证全部线程都能往前执行，CPU会轮询为系统的每个线程服务，由于CPU切换的速度很快，给我们的感觉这些线程在同时执行，这就是并发**

## 8.3并行

**在同一时刻上，同时有多个线程在被CP调度执行**



多线程是并发和并行的共同作用

# 9.线程的生命周期

**Java总共定义了6种状态，定义在Thread类的内部枚举类中**

![截屏2024-04-15 19.54.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-15%2019.54.01.png)

![截屏2024-04-15 19.54.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-15%2019.54.26.png)

# 10.悲观锁和乐观锁

悲观锁：一上来就加锁，没有安全感，每次只能一个线程进入访问完毕后再解锁，线程安全，性能较差

乐观锁：一开始不上锁，认为是没有问题的，等要出现线程安全问题的时候才开始控制，线程安全，性能较好

```java
public class PessimisticDemo {
    public static void main(String[] args) {
        Runnable target = new PessimisticLockRunnable();

        for (int i = 1;i <= 100;i++){
            new Thread(target).start();
        }
    }
}
```

```java
public class PessimisticLockRunnable implements Runnable{

    private int count;
    @Override
    public void run() {
        synchronized (this) {
            for (int i = 0; i < 100; i++) {
                System.out.println(Thread.currentThread().getName() + "---->" + (++count));
            }
        }
    }
}
```



```java
//整数修改的乐观锁：原子类实现
private final AtomicInteger count = new AtomicInteger();
@Override
public void run() {
    for (int i = 0;i < 100;i++){
        System.out.println(Thread.currentThread().getName()+count.incrementAndGet());
    }
}
```

乐观锁比较对象获取前后的版本号，没有变过才进行，效率高   **CAS比较和修改算法**





**上锁的时候一定要保证对象唯一**

# 11.练习

100份礼物，小明，小红分别是一个线程派送，比较最后谁多

```java
public class WorkDemo {
    public static void main(String[] args) throws Exception {
        List<String> gift = new ArrayList<>();
        Random random = new Random();
        String[] gifts = {"口红","包包","橡皮","池子","浴缸","鱼缸"};
        for (int i = 0;i < 100;i++){
            gift.add(gifts[random.nextInt(gifts.length)]+(i+1));
        }

       SendThread xh =  new SendThread("xh",gift);
        xh.start();
       SendThread xm =  new SendThread("xm",gift);
       xm.start();


       xh.join();
       xm.join();//确保两个线程都执行完，一个对象join另外一个对象不会停止，这里只是主线程停止

        System.out.println(xh.getCount());
        System.out.println(xm.getCount());


    }
}
```

```java
public class SendThread extends Thread{

    private final List<String> gift;
    private int count;

    public SendThread(String name, List<String> gift) {
        super(name);
        this.gift = gift;
    }

    @Override
    public void run() {
        Random random = new Random();
        String name = Thread.currentThread().getName();
        while (true){
            synchronized (gift){
                if (gift.size() < 10){
                    //System.out.println(name + "->" + count);
                    break;
                }
                String rs = gift.remove(random.nextInt(gift.size()));
                System.out.println(name+"->"+rs);
                count++;
            }
        }
    }

    public int getCount(){
        return count;
    }
}
```
