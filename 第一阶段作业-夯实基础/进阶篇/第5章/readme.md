# Java多线程与并发编程
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
我对多线程的理解目前就是通过多个线程执行同一个任务
