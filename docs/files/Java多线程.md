# Java多线程

## 1. 基本概念

- **程序（program）**

  程序是为完成特定任务、用某种语言编写的一组指令的集合。即指==一段静态的代码==（还没有运行起来），静态对象。

- **进程（process）**

  进程是程序的一次执行过程，也就是说程序运行起来了，加载到了内存中，并占用了cpu的资源。这是一个动态的过程：有自身的产生、存在和消亡的过程，这也是进程的生命周期。

  ==进程是系统资源分配的单位==，系统在运行时会为每个进程分配不同的内存区域。

- **线程（thread）**

  进程可进一步细化为线程，是一个程序内部的执行路径。

  若一个进程同一时间并行执行多个线程，那么这个进程就是支持多线程的。

  ==线程是cpu调度和执行的单位，每个线程拥有独立的运行栈和程序计数器（pc）==，线程切换的开销小。

  一个进程中的多个线程共享相同的内存单元/内存地址空间——》他们从同一堆中分配对象，可以访问相同的变量和对象。这就使得相乘间通信更简便、搞笑。但索格线程操作共享的系统资源可能就会带来==安全隐患==（隐患为到底哪个线程操作这个数据，可能一个线程正在操作这个数据，有一个线程也来操作了这个数据v）。

  - 配合JVM内存结构了解（只做了解即可）

    ![](https://i.vgy.me/karuEk.png)

    class文件会通过类加载器加载到内存空间。

    其中内存区域中每个线程都会有虚拟机栈和程序计数器。

    每个进程都会有一个方法区和堆，多个线程共享同一进程下的方法区和堆。

- **CPU单核和多核的理解**

  **单核**的CPU是一种假的多线程，因为在一个时间单元内，也只能执行一个线程的任务。同时间段内有多个线程需要CPU去运行时，CPU也只能交替去执行多个线程中的一个线程，但是由于其执行速度特别快，因此感觉不出来。

  **多核**的CPU才能更好的发挥多线程的效率。

  对于**Java**应用程序java.exe来讲，至少会存在三个线程：main()主线程，gc()垃圾回收线程，异常处理线程。如过发生异常时会影响主线程。

- **Java线程的分类**：用户线程 和 守护线程
    - Java的gc()垃圾回收线程就是一个守护线程
    - 守护线程是用来服务用户线程的，通过在start()方法前调用thread.setDaemon(true)可以吧一个用户线程变成一个守护线程。

- **并行和并发**

  - **并行**：多个cpu同时执行多个任务。比如，多个人做不同的事。
  - **并发**：一个cpu（采用时间片）同时执行多个任务。比如，渺少、多个人做同一件事。

- **多线程的优点**

  1. 提高应用程序的响应。堆图像化界面更有意义，可以增强用户体验。
  2. 提高计算机系CPU的利用率。
  3. 改善程序结构。将既长又复杂的进程分为多个线程，独立运行，利于理解和修改。

- **何时需要多线程**

  - 程序需要同时执行两个或多个任务。
  - 程序需要实现一些需要等待的任务时，如用户输入、文件读写操作、网络操作、搜索等。
  - 需要一些后台运行的程序时。



## 2. 线程的创建和启动

### 2.1. 多线程实现的原理

- Java语言的JVM允许程序运行多个线程，多线程可以通过Java中的==java.lang.Thread==类来体现。
- Thread类的特性
  - 每个线程都是通过某个特定的Thread对象的run()方法来完成操作的，经常吧run()方法的主体称为线程体。
  - 通过Thread方法的start()方法来启动这个线程，而非直接调用run()。

### 2.2.多线程的创建，方式一：继承于Thread类

1. 创建一个继承于Thread类的子类。
2. 重写Thread类的run()方法。
3. 创建Thread类的子类的对象。
4. 通过此对象调用start()来启动一个线程。

**代码实现：**多线程执行同一段代码

```java
package com.broky.multiThread;

/**
 * @author 13roky
 * @date 2021-04-19 21:22
 */
public class ThreadTest extends Thread{
    @Override
    //线程体,启动线程时会运行run()方法中的代码
    public void run() {
        //输出100以内的偶数
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0){
                System.out.println(Thread.currentThread().getName()+":\t"+i);
            }
        }
    }

    public static void main(String[] args) {
        //创建一个Thread类的子类对象
        ThreadTest t1 = new ThreadTest();
        //通过此对象调用start()启动一个线程
        t1.start();
        //注意:已经启动过一次的线程无法再次启动
        //再创建一个线程
        ThreadTest t2 = new ThreadTest();
        t2.start();

        //另一种调用方法,此方法并没有给对象命名
        new ThreadTest().start();

        System.out.println("主线程");
    }
}
```

**多线程代码运行图解**

![](https://i.vgy.me/at2IMI.png)

**多线程执行多段代码**

```java
package com.broky.multiThread.exer;

/**
 * @author 13roky
 * @date 2021-04-19 22:43
 */
public class ThreadExerDemo01 {
    public static void main(String[] args) {
        new Thread01().start();
        new Thread02().start();
    }
}

class Thread01 extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0) System.out.println(Thread.currentThread().getName() + ":\t" + i);
        }
    }
}

class Thread02 extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 != 0) System.out.println(Thread.currentThread().getName() + ":\t" + i);
        }
    }
}
```

### 2.3.多线程的创建，方式一：创建Thread匿名子类（也属于方法一）

```java
package com.broky.multiThread;

/**
 * @author 13roky
 * @date 2021-04-19 22:53
 */
public class AnonymousSubClass {
    public static void main(String[] args) {

        new Thread(){
            @Override
            public void run() {
                for (int i = 0; i < 100; i++) {
                    if (i % 2 == 0) System.out.println(Thread.currentThread().getName() + ":\t" + i);
                }
            }
        }.start();

    }
}
```

### 2.4. 多线程的创建，方式二：实现Runnable接口

1. 创建一个实现Runnable接口的类。
2. 实现类去实现Runnable接口中的抽象方法：run()。
3. 创建实现类的对象。
4. 将此对象作为参数传到Thread类的构造器中，创建Thread类的对象。
5. 通过Thread类的对象调用start()方法。

```java
package com.broky.multiThread;

/**
 * @author 13roky
 * @date 2021-04-20 23:16
 */
public class RunnableThread {
    public static void main(String[] args) {
        //创建实现类的对象
        RunnableThread01 runnableThread01 = new RunnableThread01();
        //创建Thread类的对象,并将实现类的对象当做参数传入构造器
        Thread t1 = new Thread(runnableThread01);
        //使用Thread类的对象去调用Thread类的start()方法:①启动了线程 ②Thread中的run()调用了Runnable中的run()
        t1.start();

        //在创建一个线程时，只需要new一个Thread类就可,不需要new实现类
        Thread t2 = new Thread(runnableThread01);
        t2.start();
    }
}

//RunnableThread01实现Runnable接口的run()抽象方法
class RunnableThread01 implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0) System.out.println(Thread.currentThread().getName() + ":\t" + i);
        }
    }
}
```

#### 2.4.1. 比较创建线程的两种方式

- Java中只允许单进程，以卖票程序TiketSales类来说，很有可能这个类本来就有父类，这样一来就不可以继承Thread类来完成多线程了，但是一个类可以实现多个接口，因此==实现的方式没有类的单继承性的局限性==，用实现Runnable接口的方式来完成多线程更加实用。
- 实现Runnable接口的方式天然具有共享数据的特性（不用static变量）。因为继承Thread的实现方式，需要创建多个子类的对象来进行多线程，如果子类中有变量A，而不使用static约束变量的话，每个子类的对象都会有自己独立的变量A，只有static约束A后，子类的对象才共享变量A。而实现Runnable接口的方式，只需要创建一个实现类的对象，要将这个对象传入Thread类并创建多个Thread类的对象来完成多线程，而这多个Thread类对象实际上就是调用一个实现类对象而已。==实现的方式更适合来处理多个线程有共享数据的情况。==
- 联系：Thread类中也实现了Runnable接口
- 相同点两种方式都需要重写run()方法，线程的执行逻辑都在run()方法中

### 2.5. 多线程的创建，方式三：实现Callable接口

**与Runnable相比，Callable功能更强大**

1. 相比run()方法，可以有返回值
2. 方法可以抛出异常
3. 支持泛型的返回值
4. 需要借助FutureTask类，比如获取返回结果

```java
package com.broky.multiThread;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

/**
 * 创建线程的方式三：实现Callable接口。 ---JDK5新特性
 * 如何理解Callable比Runnable强大？
 * 1.call()可以有返回值
 * 2.call()可以抛出异常被外面的操作捕获
 * @author 13roky
 * @date 2021-04-22 21:04
 */

//1.创建一个实现Callable的实现类
class NumThread implements Callable<Integer>{
    //2.实现call方法，将此线程需要执行的操作声明在call()中
    @Override
    public Integer call() throws Exception {
        int sum = 0;
        for (int i = 1; i < 100; i++) {
            if(i%2==0){
                System.out.println(i);
                sum += i;
            }
        }
        return sum;
    }
}

public class ThreadNew {
    public static void main(String[] args) {
        //3.创建Callable接口实现类的对象
        NumThread numThread = new NumThread();
        //4.将此Callable接口实现类的对象作为参数传递到FutureTask构造器中，创建FutureTask对象
        FutureTask<Integer> futureTask = new FutureTask(numThread);
        //5.将FutureTask的对象作为参数传递到Thread类的构造器中，创建Thread对象，并调用start()
        new Thread(futureTask).start();

        try {
            //6.获取Callable中Call方法的返回值
            Integer sum = futureTask.get();
            System.out.println("总和为"+sum);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }

    }
}
```

### 2.6. 多线程的创建，方式四：线程池

**背景：**

​	经常创建和销毁、使用量特别大的资源、比如并发情况下的线程、对性能影响很大。

**思路：**

​	提前创建好多个线程，放入线程池中，使用时直接获取，使用完放回池中。可以避免频繁创建销毁、实现重复利用。类似生活中的公共交通工具。

**优点：**

​	提高响应速度（减少了创建新线程的时间）

​	降低资源消耗（重复利用线程池中线程，不需要每次都创建）

​	便于线程管理

```java
package com.broky.multiThread;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;

/**
 * 创建线程的方式四：使用线程池
 * <p>
 * 面试题：创建多线程有几种方式
 *
 * @author 13roky
 * @date 2021-04-22 21:49
 */

class NumberThread implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ":\t" + i);
            }
        }
    }
}

public class ThreadPool {
    public static void main(String[] args) {

        //1.提供指定线程数量的线程池
        ExecutorService service = Executors.newFixedThreadPool(10);
        ThreadPoolExecutor service1 = (ThreadPoolExecutor) service;
        //设置线程池的属性
        //        System.out.println(service.getClass());
        //        service1.setCorePoolSize(15);
        //        service1.setKeepAliveTime();

        //2.执行指定的线程的操作。需要提供实现Runnable接口或Callable接口实现类的对象。
        service.execute(new NumberThread()); //适合用于Runnable
        //        service.submit(); 适合适用于Callable
        //关闭线程池
        service.shutdown();
    }
}
```





## 3. Thread类的常用方法

- start() : 启动当前线程, 调用当前线程的run()方法
- run() : 通常需要重写Thread类中的此方法, 将创建的线程要执行的操作声明在此方法中
- currentThread() : 静态方法, 返回当前代码执行的线程
- getName() : 获取当前线程的名字
- setName() : 设置当前线程的名字
- yield() : 释放当前CPU的执行权
- join() : 在线程a中调用线程b的join(), 此时线程a进入阻塞状态, 知道线程b完全执行完以后, 线程a才结束阻塞状态
- stop() : 已过时. 当执行此方法时,强制结束当前线程.
- sleep(long militime)  : 让线程睡眠指定的毫秒数，在指定时间内，线程是阻塞状态
- isAlive() ：判断当前线程是否存活

## 4. 线程的调度

### 4.1. cpu的调度策略

- **时间片：**cpu正常情况下的调度策略。即CPU分配给各个程序的时间，每个线程被分配一个时间段，称作它的时间片，即该进程允许运行的时间，使各个程序从表面上看是同时进行的。如果在时间片结束时进程还在运行，则CPU将被剥夺并分配给另一个进程。如果进程在时间片结束前阻塞或结束，则CPU当即进行切换。而不会造成CPU资源浪费。在宏观上：我们可以同时打开多个应用程序，每个程序并行不悖，同时运行。但在微观上：由于只有一个CPU，一次只能处理程序要求的一部分，如何处理公平，一种方法就是引入时间片，每个程序轮流执行。

- **抢占式：**高优先级的线程抢占cpu。

### 4.2. Java的调度算法：

- 同优先级线程组成先进先出队列（先到先服务），使用时间片策略。
- 堆高优先级，使用优先调度的抢占式策略。

**线程的优先级等级**（一共有10挡）

- MAX_PRIORITY：10
- MIN_PRIORITY：1
- NORM_PRIORITY：5 (默认优先级)

**获取和设置当前线程的优先级**

- `getPriority();` 获取
- `setPriority(int p);` 设置

**说明：高优先级的线程要抢占低优先级线程cpu的执行权。但是只是从概率上讲，高优先级的线程高概率的情况下被执行。并不意味着只有高优先级的线程执行完成以后，低优先级的线程才执行。**

## 5. 线程的生命周期

- JDk中用Thread.State类定义了线程的几种状态

想要实现多线程，必须在主线程中创建新的线程对象。Java语言使用Thread类及其子类的对象来表示线程，在他的一个完整的生命周期中通常要经历如下的**五种状态**：

1. 新建：当一个Thread类或其子类的对象被声明并创建时，新的线程对象处于新建状态。
2. 就绪：处于新建状态的线程被start()后，将进入线程队列等待CPU时间片，此时它已具备了运行的条件，只是没分配到CPU资源。
3. 运行：当就绪的线程被调度并获得CPU资源时，便进入运行状态，run()方法定义了线程的操作和功能。
4. 阻塞：在某种特殊情况下，被认为挂起或执行输入输出操作时，让出CPU并临时中止自己的执行，进入阻塞状态。
5. 死亡：线程完成了它的全部工作或线程被提前强制性的中止或出现异常倒置导致结束。

![](https://i.vgy.me/qiV0by.png)



## 6. 线程的同步

### 6.1. 多线程的安全性问题解析

- 线程的安全问题
  - 多个线程执行的不确定性硬气执行结果的不稳定性
  - 多个线程对账本的**共享,** 会造成操作的不完整性, 会破坏数据.
  - 多个线程访问共享的数据时可能存在安全性问题
- 线程的安全问题Demo: 卖票过程中出现了重票和错票的情况 (以下多窗口售票demo存在多线程安全问题)

```java
package com.broky.multiThread.safeThread;

/**
 * @author 13roky
 * @date 2021-04-21 20:39
 */
public class SafeTicketsWindow {
    public static void main(String[] args) {
        WindowThread ticketsThread02 = new WindowThread();
        Thread t1 = new Thread(ticketsThread02);
        Thread t2 = new Thread(ticketsThread02);
        Thread t3 = new Thread(ticketsThread02);

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }
}

class WindowThread implements Runnable {
    private int tiketsNum = 100;

    public void run() {
        while (true) {
            if (tiketsNum > 0) {
                try {
                    //手动让线程进入阻塞,增大错票概率
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + ":\t票号:" + tiketsNum);
                /*try {
                    //手动让线程进入阻塞,增大重票的概率
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }*/
                tiketsNum--;
            } else {
                break;
            }
        }
    }
}
```

**错票分析:**

当票数为1的时候，三个线程中有线程被阻塞没有执行票数-1的操作，这是其它线程就会通过if语句的判断，这样一来就会造成多卖了一张票，出现错票的情况。

极端情况为，当票数为1时，三个线程同时判断通过，进入阻塞，然后多执行两侧卖票操作。

![](https://i.vgy.me/WYm0AK.png)

**重票分析**：

如果t1在输出票号22和票数-1的操作之间被阻塞，这就导致这时候t1卖出了22号票，但是总票数没有减少。在t1被阻塞期间，如果t2运行到输出票号时，那么t2也会输出和t1相同的票号22.

**通过以上两种情况可以看出，线程的安全性问题时因为多个线程正在执行代码的过程中，并且尚未完成的时候，其他线程参与进来执行代码所导致的。**

### 6.2. 多线程安全性问题的解决

**原理：**

当一个线程在操作共享数据的时候，其他线程不能参与进来。知道这个线程操作完共享数据的时候，其他线程才可以操作。即使当这个线程操作共享数据的时候发生了阻塞，依旧无法改变这种情况。

在Java中，我们通过同步机制，来解决线程的安全问题。

![](https://i.vgy.me/3Lo8QW.png)

#### 6.2.1. 多线程安全问题的解决方式一：同步代码块

`synchronized(同步监视器){需要被同步的代码}`

说明：

1. 操作共享数据（多个线程共同操作的变量）的代码，即为需要被同步的代码。 不能多包涵代码（效率低，如果包到while前面就变成了单线程了），也不能少包含代码
2. 共享数据：多个线程共同操作的变量。
3. 同步监视器：俗称，锁。任何一个类的对象都可以充当锁。但是所有的线程都必须共用一把锁，共用一个对象。

锁的选择：

1. 自行创建，共用对象，如下面demo中的Object对象。

2. 使用this表示当前类的对象

   继承Thread的方法中的锁不能使用this代替，因为继承thread实现多线程时，会创建多个子类对象来代表多个线程，这个时候this指的时当前这个类的多个对象，不唯一，无法当作锁。

   实现Runnable接口的方式中，this可以当作锁，因为这种方式只需要创建一个实现类的对象，将实现类的对象传递给多个Thread类对象来当作多个线程，this就是这个一个实现类的对象，是唯一的，被所有线程所共用的对象。

3. 使用类当作锁，以下面demo为例，其中的锁可以写为`WindowThread.class`, 从这里可以得出结论，==类也是一个对象==

优点：同步的方式，解决了线程安全的问题

缺点：操作同步代码时，只能有一个线程参与，其他线程等待。相当于时一个单线程的过程，效率低。

**Demo**

```java
package com.broky.multiThread.safeThread;

/**
 * @author 13roky
 * @date 2021-04-21 20:39
 */
public class SafeTicketsWindow {
    public static void main(String[] args) {
        WindowThread ticketsThread02 = new WindowThread();
        Thread t1 = new Thread(ticketsThread02);
        Thread t2 = new Thread(ticketsThread02);
        Thread t3 = new Thread(ticketsThread02);

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }
}

class WindowThread implements Runnable {
    private int tiketsNum = 100;
    
    //由于，Runnable实现多线程，所有线程共用一个实现类的对象，所以三个线程都共用实现类中的这个Object类的对象。
    Object obj = new Object();
    //如果时继承Thread类实现多线程，那么需要使用到static Object obj = new Object();
    
    public void run() {
        
        //Object obj = new Object();
        //如果Object对象在run()方法中创建，那么每个线程运行都会生成自己的Object类的对象，并不是三个线程的共享对象，所以并没有给加上锁。
        
        while (true) {
            synchronized (obj) {
                if (tiketsNum > 0) {
                    try {
                        //手动让线程进入阻塞,增大安全性发生的概率
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + ":\t票号:" + tiketsNum + "\t剩余票数:" + --tiketsNum);
                } else {
                    break;
                }
            }
        }
    }
}
```

#### 6.3.2. 多线程安全问题的解决方式二：同步方法

将所要同步的代码放到一个方法中，将方法声明为synchronized同步方法。之后可以在run()方法中调用同步方法。

**要点：**

1. 同步方法仍然涉及到同步监视器，只是不需要我们显示的声明。
2. 非静态的同步方法，同步监视器是：this。
3. 静态的同步方法，同步监视器是：当前类本身。

**Demo**

```java
package com.broky.multiThread.safeThread;

/**
 * @author 13roky
 * @date 2021-04-21 22:39
 */
public class Window02 {
    public static void main(String[] args) {
        Window02Thread ticketsThread02 = new Window02Thread();
        Thread t1 = new Thread(ticketsThread02);
        Thread t2 = new Thread(ticketsThread02);
        Thread t3 = new Thread(ticketsThread02);

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }
}

class Window02Thread implements Runnable {
    private int tiketsNum = 100;

    @Override
    public void run() {
        while (tiketsNum > 0) {
            show();
        }
    }

    private synchronized void show() { //同步监视器：this
        if (tiketsNum > 0) {
            try {
                //手动让线程进入阻塞,增大安全性发生的概率
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + ":\t票号:" + tiketsNum + "\t剩余票数:" + --tiketsNum);
        }
    }
}
```

```java
package com.broky.multiThread.safeThread;

/**
 * @author 13roky
 * @date 2021-04-21 22:59
 */
public class Window03 {
    public static void main(String[] args) {
        Window03Thread t1 = new Window03Thread();
        Window03Thread t2 = new Window03Thread();
        Window03Thread t3 = new Window03Thread();
        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");
        t1.setPriority(Thread.MIN_PRIORITY);
        t3.setPriority(Thread.MAX_PRIORITY);
        t1.start();
        t2.start();
        t3.start();
    }
}

class Window03Thread extends Thread {
    public static int tiketsNum = 100;

    @Override
    public void run() {
        while (tiketsNum > 0) {
            show();
        }
    }

    public static synchronized void show() {//同步监视器：Winddoe03Thread.class  不加static话同步监视器为t1 t2 t3所以错误
        if (tiketsNum > 0) {
            try {
                //手动让线程进入阻塞,增大安全性发生的概率
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + ":\t票号:" + tiketsNum + "\t剩余票数:" + --tiketsNum);
        }
    }
}
```

**使用同步解决懒汉模式的线程安全问题**

```java
package com.broky.multiThread.safeThread;

/**
 * @author 13roky
 * @date 2021-04-22 7:24
 */
public class BankTest {
}

class Bank {
    private Bank() {
    }

    private static Bank instance = null;

    public static Bank getInstance() {
        //方式一：效率性差，每个等待线程都会进入同步代码块
        //        synchronized (Bank.class) {
        //            if (instance == null) {
        //                instance = new Bank();
        //            }
        //        }

        //方式二：在同步代码块外层在判断一次，就防止所有线程进入同步代码块。
        if (instance == null) {
            synchronized (Bank.class) {
                if (instance == null) {
                    instance = new Bank();
                }
            }
        }
        return instance;
    }
}
```

#### 6.2.3. 多线程安全问题的解决方式二：Lock锁	-JDK5.0新特性

JDK5.0之后，可以通过实例化ReentrantLock对象，在所需要同步的语句前，调用ReentrantLock对象的lock()方法，实现同步锁，在同步语句结束时，调用unlock()方法结束同步锁

**synchronized和lock的异同：(面试题)**

	1. Lcok是显式锁（需要手动开启和关闭锁），synchronized是隐式锁，除了作用域自动释放。
 	2. Lock只有代码块锁，synchronized有代码块锁和方法锁。
 	3. 使用Lcok锁，JVM将花费较少的时间来调度线程，性能更好。并且具有更好的拓展性（提供更多的子类）

建议使用顺序：Lock—》同步代码块（已经进入了方法体，分配了相应的资源）—》同步方法（在方法体之外）

**Demo:**

```java
package com.broky.multiThread.safeThread;

import java.util.concurrent.locks.ReentrantLock;

/**
 * @author 13roky
 * @date 2021-04-22 9:36
 */
public class SafeLock {
    public static void main(String[] args) {
        SafeLockThread safeLockThread = new SafeLockThread();
        Thread t1 = new Thread(safeLockThread);
        Thread t2 = new Thread(safeLockThread);
        Thread t3 = new Thread(safeLockThread);

        t1.start();
        t2.start();
        t3.start();
    }
}

class SafeLockThread implements Runnable{
    private int tickets = 100;
    private ReentrantLock lock = new ReentrantLock();

    @Override
    public void run() {
        while (tickets>0) {
            try {
                //在这里锁住，有点类似同步监视器
                lock.lock();
                if (tickets > 0) {
                    Thread.sleep(100);
                    System.out.println(Thread.currentThread().getName() + ":\t票号:" + tickets + "\t剩余票数:" + --tickets);
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                //操作完成共享数据后在这里解锁
                lock.unlock();
            }
        }
    }
}

```





### 6.3. 线程同步的死锁问题

**原理：**

​	不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃自己需要的同步资源，就形成了死锁。

​	出现死锁后，并不会出现异常，不会出现提示，只是所有的线程都处于阻塞状态，无法继续。

​	使用同步时应避免出现死锁。

**Java中死锁最简单的情况：**

​	一个线程T1持有锁L1并且申请获得锁L2，而另一个线程T2持有锁L2并且申请获得锁L1，因为默认的锁申请操作都是阻塞的，所以线程T1和T2永远被阻塞了。导致了死锁。这是最容易理解也是最简单的死锁的形式。但是实际环境中的死锁往往比这个复杂的多。可能会有多个线程形成了一个死锁的环路，比如：线程T1持有锁L1并且申请获得锁L2，而线程T2持有锁L2并且申请获得锁L3，而线程T3持有锁L3并且申请获得锁L1，这样导致了一个锁依赖的环路：T1依赖T2的锁L2，T2依赖T3的锁L3，而T3依赖T1的锁L1。从而导致了死锁。

​	从这两个例子，我们可以得出结论，产生死锁可能性的最根本原因是：**线程在获得一个锁L1的情况下再去申请另外一个锁L2，也就是锁L1想要包含了锁L2，也就是说在获得了锁L1，并且没有释放锁L1的情况下，又去申请获得锁L2，这个是产生死锁的最根本原因**。另一个原因是**默认的锁申请操作是阻塞的**。

**死锁的解决方法：**

	1. 专门的算法、原则。
 	2. 尽量减少同步资源的定义。
 	3. 尽量避免嵌套同步。

```java
package com.broky.multiThread.safeThread;

/**
 * @author 13roky
 * @date 2021-04-22 8:34
 */
public class DeadLock {
    public static void main(String[] args) {
        StringBuffer s1 = new StringBuffer();
        StringBuffer s2 = new StringBuffer();

        new Thread() {
            public void run() {
                synchronized (s1) {
                    s1.append("a");
                    s2.append("1");

                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                    synchronized (s2) {
                        s1.append("b");
                        s2.append("2");

                        System.out.println(s1);
                        System.out.println(s2);
                    }
                }
            }
        }.start();

        new Thread(new Runnable() {
            public void run() {
                synchronized (s2) {
                    s1.append("c");
                    s2.append("3");

                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                    synchronized (s1) {
                        s1.append("d");
                        s2.append("4");

                        System.out.println(s1);
                        System.out.println(s2);
                    }
                }
            }
        }).start();
    }
}
```



## 7. 线程的通信

很多情况下，尽管我们创建了多个线程，也会出现几乎一个线程执行完所有操作的时候，这时候我们就需要让线程间相互交流。

**原理：**

​	当一个线程执行完成其所应该执行的代码后，手动让这个线程进入阻塞状态，这样一来，接下来的操作只能由其他线程来操作。当其他线程执行的开始阶段，再手动让已经阻塞的线程停止阻塞，进入就绪状态，虽说这时候阻塞的线程停止了阻塞，但是由于现在正在运行的线程拿着同步锁，所以停止阻塞的线程也无法立马执行。如此操作就可以完成线程间的通信。

**所用的到方法：**

​	wait()：一旦执行此方法，当前线程就会进入阻塞，一旦执行wait()会释放同步监视器。

​	notify()：一旦执行此方法，将会唤醒被wait的一个线程。如果有多个线程被wait，就唤醒优先度最高的。

​	notifyAll() ：一旦执行此方法，就会唤醒所有被wait的线程

​	说明：

​		这三个方法必须在同步代码块或同步方法中使用。

​		三个方法的调用者必须是同步代码块或同步方法中的同步监视器。

​		这三个方法并不时定义在Thread类中的，而是定义在Object类当中的。因为所有的对象都可以作为同步监视器，而这三个方法需要由同步监视器调用，所以任何一个类都要满足，那么只能写在Object类中。

**sleep()和wait()的异同：(面试题)**

1. 相同点：两个方法一旦执行，都可以让线程进入阻塞状态。

2. 不同点：1) 两个方法声明的位置不同：Thread类中声明sleep(),Object类中声明wait()

   ​				2) 调用要求不同：sleep()可以在任何需要的场景下调用。wait()必须在同步代码块中调用。

   ​				2) 关于是否释放同步监视器：如果两个方法都使用在同步代码块呵呵同步方法中，sleep不会释放锁，wait会释放锁。

**Demo：**

```java
package com.broky.multiThread;

/**
 * @author 13roky
 * @date 2021-04-22 13:29
 */
public class Communication {
    public static void main(String[] args) {
        CommunicationThread communicationThread = new CommunicationThread();
        Thread t1 = new Thread(communicationThread);
        Thread t2 = new Thread(communicationThread);
        Thread t3 = new Thread(communicationThread);

        t1.start();
        t2.start();
        t3.start();
    }
}

class CommunicationThread implements Runnable {
    int Num = 1;

    @Override
    public void run() {
        while (true) {
            synchronized (this) {
                notifyAll();
                if (Num <= 100) {
                    System.out.println(Thread.currentThread().getName() + ":\t" + Num);
                    Num++;

                    try {
                        wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }else{
                    break;
                }
            }

        }
    }
}
```




## 练习

- 练习1：

银行有一个账户。

有两个储户分别向同一个账户存3000元，每次存1000，存3次。每次存完打印账户余额。

```java
package com.broky.multiThread.exer;

/**
 * 练习1
 * 银行有一个账户
 * 有两个储户分别向同一个账户存3000元，每次存1000，存3次。每次存完打印账户余额。
 * 分析：
 * 1.是否有多个线程问题？ 是，有两个储户线程。
 * 2.是否有共享数据？ 是，两个储户向同一个账户存钱
 * 3.是否有线程安全问题： 有
 *
 * @author 13roky
 * @date 2021-04-22 12:38
 */
public class AccountTest {
    public static void main(String[] args) {
        Account acct = new Account();
        Customer c1 = new Customer(acct);
        Customer c2 = new Customer(acct);

        c1.setName("储户1");
        c2.setName("储户2");

        c1.start();
        c2.start();

    }
}

class Account {
    private double accountSum;

    public Account() {
        this.accountSum = 0;
    }

    public Account(double accountSum) {
        this.accountSum = accountSum;
    }

    //存钱
    public void deppsit(double depositNum) {
        synchronized (this) {
            if (depositNum > 0) {
                accountSum = accountSum + depositNum;
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + ": 存钱成功，当前余额为：\t" + accountSum);
            }
        }

    }

}

class Customer extends Thread {
    private Account acct;

    public Customer(Account acct) {
        this.acct = acct;
    }

    @Override
    public void run() {
        for (int i = 0; i < 3; i++) {
            acct.deppsit(1000);
        }
    }
}
```

- 经典例题：生产者和消费着问题

生产者( Productor)将产品交给店员( Clerk),而消费者( (Customer)从店员处取走产品, 店员一次只能持有固定数量的产品(比如:20),如果生产者试图生产更多的产品,店员会叫生产者停一下,如果店中有空位放产品了再通知生产者继续生产; 如果店中没有产品了,店员会告诉消费者等一下,如果店中有产品了再通知消费者来取走产品。

```java
package com.broky.multiThread.exer;

/**
 * - 经典例题：生产者和消费着问题
 * 生产者( Productor)将产品交给店员( Clerk),而消费者( (Customer)从店员处取走产品,
 * 店员一次只能持有固定数量的产品(比如:20),如果生产者试图生产更多的产品,店员会叫生产者停一下,
 * 如果店中有空位放产品了再通知生产者继续生产; 如果店中没有产品了,店员会告诉消费者等一下,
 * 如果店中有产品了再通知消费者来取走产品。
 *
 * 分析：
 * 1.是多线程问题，可以假设多个消费这和多个生产者是多线程的
 * 2.存在操作的共享数据，生产和购买时都需要操作经销商的库存存量。
 * 3.处理线程安全问题。
 * 4.三个类：生产者，经销商，消费者。经销商被生产者和消费者共享。生产者读取经销商库存，当库存不够时，生产产品
 * 并发给经销商，操作经销商库存+1。消费者读取经销商库存，当有库存时，方可进行购买，购买完成后，经销商库存-1.
 * @author 13roky
 * @date 2021-04-22 14:36
 */
public class ProductTest {
    public static void main(String[] args) {
        Clerk clerk = new Clerk();
        Producer p1 = new Producer(clerk);
        Producer p2 = new Producer(clerk);
        p1.setName("生产者1");
        p2.setName("生产者2");

        Consumer c1 = new Consumer(clerk);
        Consumer c2 = new Consumer(clerk);
        c1.setName("消费者1");
        c2.setName("消费者2");

        p1.start();
        c1.start();
    }
}

class Clerk {
    private int productNum;

    public Clerk() {
        this.productNum = 0;
    }

    public int getProductNum() {
        return productNum;
    }

    public void setProductNum(int productNum) {
        this.productNum = productNum;
    }
}

class Producer extends Thread {
    private Clerk clerk;

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "开始生产......");

        while(true){
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            produce();
        }
    }

    public Producer(Clerk clerk) {
        if (clerk != null) {
            this.clerk = clerk;
        }
    }

    private void produce() {
        synchronized (ProductTest.class) {
            ProductTest.class.notify();
            if (clerk.getProductNum() < 20) {
                clerk.setProductNum(clerk.getProductNum() + 1);
                System.out.println(Thread.currentThread().getName() + ":\t生产完成第 " + clerk.getProductNum() + " 个产品");
            }else {
                try {
                    ProductTest.class.wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }

}

class Consumer extends Thread {
    private Clerk clerk;

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "开始消费......");

        while(true){
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            buy();
        }
    }

    public Consumer(Clerk clerk) {
        if (clerk != null) {
            this.clerk = clerk;
        }
    }

    private void buy(){
        synchronized (ProductTest.class) {
            ProductTest.class.notify();
            if (clerk.getProductNum() > 0) {
                System.out.println(Thread.currentThread().getName() + ":\t购买完成第 " + clerk.getProductNum() + " 个产品");
                clerk.setProductNum(clerk.getProductNum() - 1);
            }else {

                try {
                    ProductTest.class.wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

