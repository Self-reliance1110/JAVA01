
## 多线程与多进程简介
### 多进程概念
* 当前操作系统都是OS
* 每个独立执行的任务就是一个进程
* OS将时间分为很多个时间片
* 每个时间片内将CPU分配给某一个任务，时间片介绍，CPU将自动回收，再分配给另外任务
### 多线程概念
* 一个程序可以包括多个子任务
* 每一个子任务可以成为一个线程
* 如果一个子任务堵塞，程序可以将CPU调度到另一个子任务进行工作。这样在这个时间片中CPU还是停留在这个程序当中，不会因为一个任务堵塞而释放CPU。
## Java多线程实现
### java多线程的创建
建议实现Runnable接口实现多线程编程
* java.lang.Thread类
线程继承Thread类，实现run()方法
* java.lang.Runnable接口
线程实现Runnable接口，实现run()方法
实现Runnable的对象必须包装在Thread类里面才能启动，例如
```
public class RunnableTest implements Runnable{
    @Override
    public void run() {
        System.out.println("Runnable is running");
    }

    public static void main(String[] args) {
        new Thread(new RunnableTest()).start();
    }
}
```
### java多线程的启动
启动
* start方法，会自动以新的进程运行线程的run方法
* 如果直接调用run方法，就不会起到线程该有的作用
* 一个线程只能执行一次start
* 程序的终止是所有线程都终止
## java多线程的信息共享问题
java现在还不支持点对点的信息发送
java通过共享变量来共享信息
* static变量
* 同一个Runnable类的成员变量
存在的问题及解决方法：
1.工作缓存副本:
* java提供了一个volatile关键字，可以及时在各线程里面通知
2.关键步骤缺乏加锁限制:
* 互斥：某一个线程在运行一个代码块是，其他线程不能运行这个代码块
* 同步：多个线程的运行，必须按照某一个规定的先后顺序来运行
* 互斥的关键字是synchronized,可以修饰代码块和函数
synchronized对性能负担重，但是简单方便
## java的多线程管理
### 线程状态
* NEW 刚刚创建
* 准备就绪(start)，可以运行但是需要CPU分配到这个进程
* 运行中(run)
* 堵塞和唤醒
    · sleep 定时睡眠，时间到了就会醒来
    · wait/notify/notifyAll 等待，需要别人唤醒
    · join 等待另一个线程结束
    · interrupt 向另一个线程发送中断信号，该线程收到信号会触发InterruptException(可解除堵塞)，并进行下一步处理
* 结束
### 
* 多线程死锁
预防死锁，对资源进行等级排序
* 守护线程seDeamon()方法
    ·普通线程的结束是run方法的结束
    ·守护线程的结束是run方法的结束，或main函数结束
    ·守护线程永远不要访问资源
# 作业
```
public class Main {
    public static void main(String[] args) {
        int m, n, threadNum;
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入希望运算的起始值：");
        m = sc.nextInt();
        System.out.println("请输入希望运算的截止值：");
        n = sc.nextInt();
        System.out.println("请输入希望存在的线程数：");
        threadNum = sc.nextInt();

        calculate ca = new calculate(m);
        int avgTimes = (n - m) / threadNum;//平均
        int extraTimes = (n - m) % threadNum;//平均分之后剩余
        for (int i = 0; i < threadNum; i++) {
            if (extraTimes == 0 ) {
                new add(ca, avgTimes).start();
                extraTimes--;
            } else
                new add(ca, avgTimes+1).start();

        }


    }
}

class add extends Thread {
    public static calculate ca;
    private int times = 0;
    private int max;

    public add(calculate ca,int max) {
        this.max = max;
        this.ca = ca;
    }

    @Override
    public void run() {

        while(times<max)
        {
            ca.addTo();
            times++;
            //System.out.println(Thread.currentThread().getName()+":"+times);
        }
    }
}
class calculate {
    private static long sum=0;
    private int m;

    public synchronized void  addTo()
    {
        sum+=m;
        System.out.println(Thread.currentThread().getName()+"sum:"+sum+" num:"+m);
        m++;
    }
    public calculate(int m)
    {
        this.m=m;
    }
}
```
## Java并发架构Executor
### 并行模式
* 主从模式
有一个主线程，多个从线程，主线程指挥从线程工作
* Worker模式
线程都是平等的
### Java并发编程

#### Thread组管理(ThreadGroup类，JDK自带)
* 线程的集合
* 大线程组可以包含小线程组
* 可以通过enumerate方法遍历线程组，执行操作
* 可以管理多个线程，但是管理效率低
* 无法做到任务分配和执行过程的高度耦合
* 无法重用线程

## Executor框架
从JDK5开始Executor FrameWork(java.util.concurrent.*)
* 分离任务的创建和执行者的创建
* 可以重用线程
共享线程池
* 可以预设多个线程
* 多次执行很多很小的任务
Executor框架主要类：ExecutorService,ThreadPoolExecutor,Future
用例如下
```
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.*;

public class SubmitTask {
    public static void main(String[] args) {
        //创建线程数为4的线程池
        ThreadPoolExecutor executor =(ThreadPoolExecutor) Executors.newFixedThreadPool(4);
        List<Future<Integer>> resultList= new ArrayList<>();

        for (int i = 0; i < 10; i++) {
            Task task = new Task(i*100+1,(i+1)*100);
            Future<Integer> result= executor.submit(task);//提交任务并返回结果
            resultList.add(result);//将结果加入到列表中
        }

        do{

            System.out.println("已完成 "+executor.getCompletedTaskCount());
            for (int i = 0; i < resultList.size(); i++) {
                Future<Integer> result = resultList.get(i);
                System.out.println("Task "+i+" "+result.isDone());
            }

        }while (executor.getCompletedTaskCount()<resultList.size());
        Integer sum = 0;
        for (int i = 0; i < resultList.size(); i++) {
            Future<Integer> result = resultList.get(i);
            try {
                sum +=result.get();
            } catch (InterruptedException e) {
                e.printStackTrace();
            } catch (ExecutionException e) {
                e.printStackTrace();
            }
        }
        System.out.println("1~1000的累加和为："+sum);
        executor.shutdown();

    }
}
import java.util.concurrent.Callable;

public class Task implements Callable<Integer> {
    private int startNum;
    private int endNum;

    public Task(int startNum, int endNum) {
        this.startNum = startNum;
        this.endNum = endNum;
    }

    @Override
    public Integer call() throws Exception {
        int sum=0;
        for (int i = startNum; i <=endNum ; i++) {
            sum+=i;
            Thread.sleep(10);
        }
        return sum;
    }
}

```

## Fork-Join框架
* JDK7提供的并行框架：分解、治理、合并
* 适合用于整体任务量不确定的场合
关键类
* ForkJoinPool任务池
* RecursiveAction
* RecursiveTask
总得来说，ForkJion框架可以将任务分解然后通过递归的方式来执行
用例
```
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.ForkJoinTask;

public class SubmitTask {
    public static void main(String[] args) {
        ForkJoinPool pool = new ForkJoinPool(4);
        //ForkJoinPool pool = new ForkJoinPool();
        Task task = new Task(1,10000000);
        ForkJoinTask<Long> result = pool.submit(task);
        do {
            System.out.println("Thread Num:"+pool.getActiveThreadCount());
            System.out.println("Thread Parallelism"+pool.getParallelism());
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }while (!task.isDone());
        try {
            System.out.println(result.get().toString());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }

        pool.shutdown();
    }
}
import java.util.concurrent.RecursiveTask;

public class Task extends RecursiveTask<Long> {
    private int startNum;

    public Task(int startNum, int endNum) {
        this.startNum = startNum;
        this.endNum = endNum;
    }

    private int endNum;
    @Override
    protected Long compute() {
        Long sum = 0L;
        Boolean canCompute = (endNum - startNum)<5;
        if(canCompute)
        {
            for (int i = startNum; i <= endNum; i++) {
                sum+=i;
            }
        }
        else
        {
            int middle = (endNum +startNum)/2;
            Task task1 = new Task(startNum, middle);
            Task task2 = new Task(middle+1, endNum);
            invokeAll(task1,task2);
            Long sum1 = task1.join();
            Long sum2 = task2.join();
            sum = sum1+sum2;
        }
        return sum;
    }
}
```
## 并发数据结构
* 常用的数据结构是线程不安全的
如ArrayList,HashMap,HashSet都是非同步的,多个线程同时读写可能会抛出异常或数据错误
* 传统Vector，HashTable同步性能过差
* 并发数据结构
- 阻塞式集合：当集合为空或满的时候，等待
- 非阻塞式集合：当集合为空或满的时候，不等待，返回null或异常
* List
– Vector 同步安全，写多读少
– ArrayList 不安全
– Collections.synchronizedList(List list) 基于synchronized，效率差
– CopyOnWriteArrayList 读多写少，基于复制机制，非阻塞
* Set 
– HashSet 不安全
– Collections.synchronizedSet(Set set) 基于synchronized，效率差
– CopyOnWriteArraySet (基于CopyOnWriteArrayList实现) 读多写少，非阻塞
* Map 
– Hashtable 同步安全，写多读少
– HashMap 不安全
– Collections.synchronizedMap(Map map) 基于synchronized，效率差
– ConcurrentHashMap 读多写少，非阻塞
* Queue & Deque (队列，JDK 1.5 提出) 
– ConcurrentLinkedQueue 非阻塞
– ArrayBlockingQueue/LinkedBlockingQueue 阻塞
## Java并发协作控制
### synchronized
* 限定只有一个线程才能进入关键区
* 简单粗暴，性能损失有点大
### Lock
* Lock也可以实现同步的效果
* 实现更复杂的临界区结构
* tryLock方法可以预判锁是否空闲
* 允许分离读写的操作，多个读，一个写
* ReentrantLock类，可重入的互斥锁
* ReentrantReadWriteLock类，可重入的读写锁
readlock，读锁，可多个线程共享
writelock，写锁，只能有一个线程拥有
* 主要方法是lock和unlock函数

### Semaphore信号量
* 本质上是一个计数器
* 计数器大于0，可以使用，等于0不能使用
* 可以设置多个并发量，例如限制10个访问，synchronized只能一个
* Semaphore主要方法
– acquire获取
– release释放

### Latch
* 等待锁，是一个同步辅助类
* 用来同步执行任务的一个或者多个线程
* 不是用来保护临界区或者共享资源
* CountDownLatch
– countDown() 计数减1
– await() 等待latch变成0

### Barrier
* 集合点，也是一个同步辅助类
* 允许多个线程在某一个点上进行同步
* CyclicBarrier
– 构造函数是需要同步的线程数量
– await等待其他线程，到达数量后，就放行

### Phaser
* 允许执行并发多阶段任务，同步辅助类
* 在每一个阶段结束的位置对线程进行同步，当所有的线程都到达这步，再进行下一步
* Phaser
– arrive()
– arriveAndAwaitAdvance(


### Exchanger
* 允许在并发线程中互相交换消息
* 允许在2个线程中定义同步点，当两个线程都到达同步点，它们交换数据结构
* Exchanger
– exchange(), 线程双方互相交互数据
– 交换数据是双向
当两个线程都执行到同一个exchanger的exchange方法，两个线程就会互相交换数据


## 定时任务
* 某一个时间点执行
* 周期

### 简单定时器机制
* 设置计划任务，也就是在指定的时间开始执行某一个任务。
* TimerTask 封装任务
* Timer类 定时器

### Executor +定时器机制
* ScheduledExecutorService
– 定时任务
– 周期任务

### Quartz
* Quartz是一个较为完善的任务调度框架
* 解决程序中Timer零散管理的问题
* 功能更加强大
* Timer执行周期任务，如果中间某一次有异常，整个任务终止执行
* Quartz执行周期任务，如果中间某一次有异常，不影响下次任务执行
# 作业
```
import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) {

        Random random = new Random();
        int[] array = new int[10000];
        for (int i = 0; i < 10000; i++) {
            array[i] = random.nextInt(100) + 1;
        }
        method01(array);
        method02(array);
        method03(array);


    }

    public static void method01(int[] array)//串行搜索
    {
        int total = 0;
        for (int i = 0; i < array.length; i++) {
            if (array[i] == 50) {
                total++;
            }
        }
        System.out.println("串行搜索得到50的个数是" + total + "个");
    }

    public static void method02(int[] array) {//Executor搜索
        int total = 0;
        ThreadPoolExecutor pool = (ThreadPoolExecutor) Executors.newFixedThreadPool(4);
        List<Future<Integer>> futureList = new ArrayList<>();

        Future<Integer> result1 = pool.submit(new Callable<Integer>() {
            @Override
            public Integer call() throws Exception {
                int total = 0;
                for (int i1 = 0; i1 < 2500; i1++) {
                    if (array[i1] == 50) {
                        total++;
                    }
                }
                return total;
            }
        });
        futureList.add(result1);
        Future<Integer> result2 = pool.submit(new Callable<Integer>() {
            @Override
            public Integer call() throws Exception {
                int total = 0;
                for (int i1 = 0; i1 < 2500; i1++) {
                    if (array[i1+2500] == 50) {
                        total++;
                    }
                }
                return total;
            }
        });
        futureList.add(result2);

        Future<Integer> result3 = pool.submit(new Callable<Integer>() {
            @Override
            public Integer call() throws Exception {
                int total = 0;
                for (int i1 = 0; i1 < 2500; i1++) {
                    if (array[i1+5000] == 50) {
                        total++;
                    }
                }
                return total;
            }
        });
        futureList.add(result3);

        Future<Integer> result4 = pool.submit(new Callable<Integer>() {
            @Override
            public Integer call() throws Exception {
                int total = 0;
                for (int i1 = 0; i1 < 2500; i1++) {
                    if (array[i1+7500] == 50) {
                        total++;
                    }
                }
                return total;
            }
        });
        futureList.add(result4);

        for (int i = 0; i < futureList.size(); i++) {
            Future<Integer> result=futureList.get(i);
            try {
                total +=result.get();
            } catch (InterruptedException e) {
                e.printStackTrace();
            } catch (ExecutionException e) {
                e.printStackTrace();
            }
        }

        System.out.println("Executors搜索得到50的个数是"+total+"个");
    }

    public static void method03(int[] array)
    {

        ForkJoinPool pool = new ForkJoinPool();
        ToSum toSum = new ToSum(array,0,array.length-1);
        ForkJoinTask<Integer> result = pool.submit(toSum);
        try {
            Integer integer = result.get();
            System.out.println("Fork-Join搜索得到50的个数是" +integer+"个");

        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }


    }
}
class ToSum extends RecursiveTask<Integer>
{

    private int[] array;

    private int start;

    public ToSum(int[] array, int start, int end) {
        this.array = array;
        this.start = start;
        this.end = end;
    }

    private int end;


    @Override
    protected Integer compute() {
        int total =0;
        if((end - start)>=10)
        {
            int middle = (end+start)/2;
            ToSum to1 = new ToSum(array,start,middle);
            ToSum to2 = new ToSum(array,middle+1,end);
            invokeAll(to1,to2);
            return to1.join()+to2.join();
        }else
        {
            for (int i = start; i <= end; i++) {
                if(array[i]==50)
                    total++;
            }
        }
        return total;
    }
}
```
