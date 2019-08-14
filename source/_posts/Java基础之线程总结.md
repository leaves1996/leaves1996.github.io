---
title: Java基础之线程总结
copyright: true
comments: true
date: 2019-08-14 09:57:26
tags:
 - Java基础
 - 线程
 - Thread
categories: Java基础
description: 本文主要概述Java基础中的线程相关知识点总结。
sticky:
---

@[TOC](线程面试题总结)

## 什么是线程？

&emsp;&emsp;线程是操作系统能够进行运算调度的最小单位，它被包含在进程之中，是进程中的实际运作单位，可以使用多线程对进行运算提速。

比如，如果一个线程完成一个任务要100毫秒，那么用十个线程完成改任务只需10毫秒

## 进程和线程的区别

 * 进程是程序运行和资源分配的基本单位
 * 一个程序至少有一个进程,一个进程至少有一个线程
 * 一个进程下也可以有多个线程来增加程序的执行速度
 * 进程在执行过程中拥有独立的内存单元,而多个线程共享内存资源,减少切换次数,从而效率更高
 * 线程是进程的一个实体,是cpu调度和分派的基本单位,是比程序更小的能独立运行的基本单位
 * 同一进程中的多个线程之间可以并发执行

## 并行和并发有什么区别？

&emsp;&emsp;**并行：** 多个处理器或者多核处理器同时处理多个任务。
&emsp;&emsp;**并发：** 多个任务在同一个 CPU 核上，按细分的时间片轮流（交替）执行，从逻辑上来看那些任务是同时执行。

请看下图：
![并行和并发.png](并行和并发.png)
并发 = 两个队列和一台咖啡机。
并行 = 两个队列和两个咖啡机。

## 守护线程是什么？
&emsp;&emsp;守护线程是运行在后台的一种特殊进程。它独立于控制终端并且周期性地执行某种任务或者等待处理某些发生的事件。在 Java 中垃圾回收线程就是特殊的守护线程。

&emsp;&emsp;一般来说，JVM(JAVA虚拟机）中一般会包括两种线程，分别是`用户线程`和`后台线程`。

 * 所谓后台线程（daemon)线程指的是：在程序运行的时候在后台提供的一种通用的服务的线程，并且这种线程并不属于程序中不可或缺的部分。因此，当所有的非后台线程结束的时候，也就是用户线程都结束的时候，程序也就终止了。同时，会杀死进程中的所有的后台线程。
 * 反过来说，只要有任何非后台线程还在运行，程序就不会结束。比如执行main()的就是一个非后台线程。

&emsp;&emsp;基于这个特点，当虚拟机中的用户线程全部退出运行时，守护线程没有服务的对象后，JVM也就退出了。
&emsp;&emsp;所有的用户线程一定会走完，但是守护线程随着用户线程的销毁而销毁
```java
boolean isDaemon() 
测试这个线程是否是守护线程。 
void setDaemon(boolean on) 
将此线程标记为 daemon线程或用户线程。 
```
## 创建线程有哪几种方式？

创建线程有三种方式：
 * 方式一：继承 Thread 重写 run 方法
 * 方式二：实现 Runnable 接口
 * 方式三：实现 Callable 接口

## 创建线程方式一和方式二的比较

 * 1、代码复杂程度：
  * 继承Thread方式简单。
  * 实现Runnable接口的方式比较复杂。
 * 2、实现原理：
  * 继承方式：调用 start 方法，调用 start0 方法，start0 是本地方法 `private native void start0();` ,由虚拟机实现，是C语言实现的方法，所以在java中看不到源码。本地方法 start0 返回来调用java中的run方法，run方法已经在子类中重写过了，所以最终运行的是子类重写了的run方法。
  * 实现方式：构造器中，将Runnable的实现类对象传入构造器中，经过一路init 方法的传递，最终，用于给Thread 类型中的某个成员变量（target）赋值；调用对象的start方法，最终也是返回来调用Thread 类中的run方法，判断当前的成员变量target是否为null，如果不为null，就调用target的run方法，而这个run方法我们已经重写了，最终执行的是我们重写的run方法。

## 使用匿名内部类开启线程

&emsp;&emsp;如果我们同时实现 Runnable接口，并且继承Thread并重写run方法，该线程会以那种方式为主？两种方式都存在则以继承Thread类为主。请看如下代码：
```java
new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("实现Runnable接口并重写run方法。。。。。。");
    }
}){
    @Override
    public void run() {
        System.out.println("Thread类的匿名内部类子类，重写run方法。。。。。");
    }
}.start();
```

执行结果：
```xml
Thread类的匿名内部类子类，重写run方法。。。。。
```
## Runnable 和 Callable 有什么区别？

>* Runnable接口中的run()方法的返回值是void，它做的事情只是纯粹地去执行run()方法中的代码而已；
>* Callable接口中的call()方法是有返回值的，是一个泛型，和Future、FutureTask配合可以用来获取异步执行的结果。
## Thread类中的start()和run()方法有什么区别?
>* start()方法被用来启动新创建的线程，而且start()内部调用了run()方法，这和直接调用run()方法的效果不一样。
>* run() 方法用于执行线程的运行时代码。如果对象调用run() 方法，是普通方法色调用，并非线程的启动。
>* run() 可以重复调用。而 start() 只能调用一次，如果再次调用会抛异常。
## java中用到的线程调度算法是什么
**抢占式调度模型**
&emsp;&emsp;优先让优先级高的线程使用 CPU，如果线程的优先级相同，那么会随机选择一个，优先级高的线程获取的 CPU 时间片相对多一些。 

设置和获取线程的优先级
```java
public final int getPriority()
public final void setPriority(int newPriority)
```
线程优先级：
```xml
主线程的优先级:5
线程的优先级是1-10
默认线程都是5
```
如果设置优先级传入的参数不合法则会抛出如下异常：
```xml
java.lang.IllegalArgumentException
异常名称: 非法参数异常
产生原因: 参数不合法
解决办法: 如果是线程说明线程的优先级不在指定范围内
```

## 什么是线程安全和线程不安全？
通俗的说：加锁的就是是线程安全的，不加锁的就是是线程不安全的

**线程安全**
线程安全: 就是多线程访问时，采用了加锁机制，当一个线程访问该类的某个数据时，进行保护，其他线程不能进行访问，直到该线程读取完，其他线程才可使用。不会出现数据不一致或者数据污染。

**线程不安全**
线程不安全：就是不提供数据访问保护，有可能出现多个线程先后更改数据造成所得到的数据是脏数据。
## java中的++操作符线程安全么?
不是线程安全的操作。它涉及到多个指令，如读取变量值，增加，然后存储回内存，这个过程可能会出现多个线程交差
## 线程有哪些状态？
线程的状态：
>* NEW 尚未启动
>* RUNNABLE 正在执行中
>* BLOCKED 阻塞的 （被同步锁或者IO锁阻塞）
>* WAITING 永久等待状态
>* TIMED_WAITING 等待指定的时间重新被唤醒的状态
>* TERMINATED 执行完成

## 什么情况下导致线程阻塞？
&emsp;&emsp;阻塞指的是暂停一个线程的执行以等待某个条件发生（如某资源就绪），学过操作系统的同学对它一定已经很熟悉了。Java 提供了大量方法来支持阻塞，下面让我们逐一分析。
|方法|说明|
|:----:|:----|
sleep()|&emsp;&emsp;sleep() 允许 指定以毫秒为单位的一段时间作为参数，它使得线程在指定的时间内进入阻塞状态，不能得到CPU 时间，指定的时间一过，线程重新进入可执行状态。 <br>&emsp;&emsp;典型地，sleep() 被用在等待某个资源就绪的情形：测试发现条件不满足后，让线程阻塞一段时间后重新测试，直到条件满足为止
suspend() 和 resume()|&emsp;&emsp;两个方法配套使用，suspend()使得线程进入阻塞状态，并且不会自动恢复，必须其对应d的resume() 被调用，才能使得线程重新进入可执行状态。<br>&emsp;&emsp;典型地，suspend() 和 resume() 被用在等待另一个线程产生的结果的情形：测试发现结果还没有产生后，让线程阻塞，另一个线程产生了结果后，调用 resume() 使其恢复。
yield()|&emsp;&emsp;yield() 使当前线程放弃当前已经分得的CPU 时间，但不使当前线程阻塞，即线程仍处于可执行状态，随时可能再次分得 CPU 时间。<br>&emsp;&emsp;调用 yield() 的效果等价于调度程序认为该线程已执行了足够的时间从而转到另一个线程
wait() 和 notify()|&emsp;&emsp;两个方法配套使用，wait() 使得线程进入阻塞状态，它有两种形式，一种允许 指定以毫秒为单位的一段时间作为参数，另一种没有参数，前者当对应的 notify() 被调用或者超出指定时间时线程重新进入可执行状态，后者则必须对应的 notify() 被调用.

## sleep() 和 wait() 有什么区别
>* 所在的类不同：sleep() 来自 Thread，wait() 来自 Object。调用sleep()方法的过程中，线程不会释放对象锁。而 调用 wait 方法线程会释放对象锁
>* 释放锁：sleep() 不释放锁；wait() 释放锁。
>* 用法不同：sleep(milliseconds)需要指定一个睡眠时间，sleep() 时间到了会自动恢复；wait() 可以使用 notify() 、notifyAll()直接唤醒。
>* sleep()睡眠后不出让系统资源，wait让其他线程可以占用CPU

## stop和interrupt的区别
>* stop会直接将线程生命结束
>* interrupt会给对应的线程抛出一个异常，那么对应的线程可以通过InterruptedException来捕获这个异常，并且做出相应的异常处理

## 什么是线程加入
线程加入：主要是在开启某一个线程后，立即执行该方法，那么该线程会在执行完毕后其它线程才能抢占 CPU 的使用权。
```java
public final void join() 	优先让线程执行完毕
```
## 什么是线程礼让
线程礼让: 暂时让出CPU的执行权一小会，之后依然和其它线程抢占 CPU 的使用权。
```java
public static void yield()		暂时让出CPU的执行权一小会
```

## 线程同步
线程同步：保证同一资源同步共享。
同步方式：
方式一：同步代码块
```java
格式:synchronized(同步锁对象) {
}
```
方式二：同步方法
```java
格式:
访问权限修饰符 synchronized 返回值类型 方法名(参数列表) throws 异常类 {
	return 返回值;
}
```
`注意:`
a.锁必须是同一把锁
b.锁对象可以是任意对象
c.如果是同步方法，那么锁对象是this,当前对象作锁
				
方式三：利用Lock锁
void lock()  获取锁。 
void unlock()   试图释放此锁。

## notify() 和 notifyAll() 有什么区别
>* notifyAll() 会唤醒所有的线程，notify() 只会唤醒一个线程。
>* notifyAll() 调用后，会将全部线程由等待池移到锁池，然后参与锁的竞争，竞争成功则继续执行，如果不成功则留在锁池等待锁被释放后再次参与竞争。而 notify() 只会唤醒一个线程，具体唤醒哪一个线程由虚拟机控制。
## 产生死锁的条件
1.互斥条件：一个资源每次只能被一个进程使用。
2.请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
3.不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺。
4.循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。
## 为什么wait,nofity和nofityAll这些方法不放在Thread类当中
&emsp;&emsp;一个很明显的原因是JAVA提供的锁是对象级的而不是线程级的，每个对象都有锁，通过线程获得。如果线程需要等待某些锁那么调用对象中的wait()方法就有意义了。如果wait()方法定义在Thread类中，线程正在等待的是哪个锁就不明显了。简单的说，由于wait，notify和notifyAll都是锁级别的操作，所以把他们定义在Object类中因为锁属于对象。

## 怎么唤醒一个阻塞的线程
>* 如果线程是因为调用了wait()、sleep()或者join()方法而导致的阻塞，可以中断线程，并且通过抛出InterruptedException来唤醒它；
>* 如果线程遇到了IO阻塞，无能为力，因为IO是操作系统实现的，Java代码并没有办法直接接触到操作系统。
## 什么是Executors框架？
Executor框架同java.util.concurrent.Executor 接口在Java 5中被引入。

Executor框架是一个根据一组执行策略调用，调度，执行和控制的异步任务的框架。

无限制的创建线程会引起应用程序内存溢出。所以创建一个线程池是个更好的的解决方案，因为可以限制线程的数量并且可以回收再利用这些线程。

利用Executors框架可以非常方便的创建一个线程池，
## 创建线程池有哪几种方式？
 创建线程池的方式有七种，其中最核心的是最后一种：
 >* `newSingleThreadExecutor()` ： 创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行，最多会有一个任务处于活动状态，并且不允许使用者改动线程池实例，因此可以避免其改变线程数目。
 >* `newCachedThreadPool()` ：它是一种用来处理大量短时间工作任务的线程池，具有如下几个鲜明的特点：
 >&emsp;&emsp;1、它会试图缓存线程并重用，当无缓存线程可用时，就会创建新的工作线程；
 >&emsp;&emsp;2、如果线程闲置的时间超过 60 秒，则被终止并移出缓存；
 >&emsp;&emsp;3、长时间闲置时，这种线程池，不会消耗什么资源。其内部使用 SynchronousQueue 作为工作队列。
>* `newFixedThreadPool()` ：重用指定数目（nThreads） 的线程，其背后使用的是无界的工作队列，任何时候最多有 nThreads 个工作线程是活动的。这意味着，如果任务数量超过了活动队列数目，将在工作队列中等待空闲线程出现；如果有工作线程退出，将会有新的工作线程被创建，以补足指定的数目 nThreads。
>* `newSingleThreadScheduledExecutor()` ：创建单线程池，返回ScheduledExecutorService,可以进行定时或周期性的工作调度。
>* `newScheduledThreadPool(int  corePoolSize)` ：创建一个定长线程池，和 newSingleThreadScheduledExecutor() 类似，创建的是个ScheduledExecutorService，可以进行定时或周期性的工作调度，区别在于单一工作线程还是多个工作线程。
>* `newWorkStealingPool(int  parallelism)` ：这是一个经常被人忽略的线程池，Java8 才加入这个创建方法，其内部会构建 ForkJoinPool，利用 Work-Stealing 算法，并进行地处理任务，不保证处理顺序。
>* `ThreadPoolExecutor()` ：是最原始的线程池创建，上面的 1-3 创建方式都是对 ThreadPoolExecutor() 的封装。
## 为什么要使用线程池
避免频繁地创建和销毁线程，达到线程对象的重用。另外，使用线程池还可以根据项目灵活地控制并发的数目。
## 什么是阻塞队列？
JDK7提供了7个阻塞队列。（也属于并发容器）

>* ArrayBlockingQueue ：一个由数组结构组成的有界阻塞队列。
>* LinkedBlockingQueue ：一个由链表结构组成的有界阻塞队列。
>* PriorityBlockingQueue ：一个支持优先级排序的无界阻塞队列。
>* DelayQueue：一个使用优先级队列实现的无界阻塞队列。
>* SynchronousQueue：一个不存储元素的阻塞队列。
>* LinkedTransferQueue：一个由链表结构组成的无界阻塞队列。
>* LinkedBlockingDeque：一个由链表结构组成的双向阻塞队列。
## 线程池中 submit() 和 execute() 方法有什么区别？
submit() : 可以执行 Runnable 和 Callable 类型的任务。
execute() ：只能执行 Runnable 类型的任务。

## 在 Java 程序中怎么保证多线程的运行安全？
>* 方法一： 使用安全类，比如 Java.util.concurrent 下的类。
>* 方法二： 使用自动锁 synchronized。
>* 方法三： 使用手动锁。

手动锁 Java 示例代码如下：
```java
Lock lock = new ReentrantLock();
lock.lock();
try{
	System.out.println("获取锁");
} catch (Exception e) {
	//TODO: handle exception
} finally {
	System.out.println("释放锁")；
	lock.unlock();
}
```
## 多线程中 synchronized 锁升级的原理是什么？
&emsp;&emsp;synchronized 锁升级原理：在锁对象的对象头里面有一个 threadid 字段，在第一次访问的时候 threadid 为空，JVM让其持有偏向锁，并将 threadid 设置为其线程 id，再次进入的时候会先判断 threadid 是否与其线程 id 一致，如果一致则可以直接使用此对象，如果不一致，则升级偏向锁为轻量级锁，通过自旋循环一定次数来获取锁，执行一定次数之后，如果还没有正常获取到要使用的对象，此时就会把锁从轻量级为重量级锁，此过程就构成了 synchronized 锁的升级。
&emsp;&emsp;锁的升级的目的：锁升级是为了减低了锁带来的性能消耗。在 Java6 之后优化synchronized 的实现方式，使用了偏向锁升级为轻量级锁再升级到重量级锁的方式，从而减低了锁带来的性能消耗。

## 什么是死锁？
当线程 A 持有独占锁a，并尝试去获取独占锁 b 的同时，线程 B 持有独占锁 b ，并尝试获取独占锁 a 的情况下，就会发生 AB 两个线程由于互相持有对方需要的锁，而发生的阻塞现象，我们称为死锁。

死锁示例代码：
```java
//代码示例一：
public class DieLock extends Thread{
	private boolean flag;
	
	public DieLock() {}
	
	public DieLock(boolean flag) {
		this.flag = flag;
	}
	
	@Override
	public void run() {
		if (flag) {
			// dl1进来
			synchronized (MyLock.lockA) {
				System.out.println("if 语句中 LockA锁"); // 就在输出完这句话之后被dl2抢到了资源
				synchronized (MyLock.lockB) {
					System.out.println("if 语句中 LockB锁");
				}
			}
		} else {
			// dl走else
			synchronized (MyLock.lockB) {
				System.out.println("else 语句中 lockB锁");
				synchronized (MyLock.lockA) {
					System.out.println("else 语句中 lockA锁");
				}
			}
		}
	}
}

/**
 * 创建锁对象
 */
class MyLock{
	public static final Object lockA = new Object();
	public static final Object lockB = new Object();
}

/**
 * 测试
 */
public class LockDemo {
	public static void main(String[] args) {
		DieLock dl1 = new DieLock(true);
		DieLock dl2 = new DieLock(false);
		
		dl1.start();
		dl2.start();
	}
}
```

```java
//代码示例二：

public class DeadLock {

    private static final String A = "A";
    private static final String B = "B";

    public static void main(String[] args) {

        Thread t1 = new Thread("a"){
            @Override
            public void run() {
                synchronized (A) {
                    System.out.println(Thread.currentThread().getName() + " == A");
                    synchronized (B) {
                        System.out.println(Thread.currentThread().getName() + " == B");
                    }
                }
            }
        };
        Thread t2 = new Thread("b"){
            @Override
            public void run() {
                synchronized (B) {
                    System.out.println(Thread.currentThread().getName() + " == B");
                    synchronized (A) {
                        System.out.println(Thread.currentThread().getName() + " == A");
                    }
                }
            }
        };
        t1.start();
        t2.start();
    }
}
```
## 怎么防止死锁？
	1.	尽量使用 tryLock(long  timeout,  TimeUnit  unit) 的方法 （ReentrantLock、ReentrantReadWriteLock），设置超时时间，超时可以退出防止死锁。(Lock锁中就使用了这种方式)
	2.	尽量使用 Java.util.concurrent 并发类代替自己手写锁。
	3.	尽量降低锁的使用粒度，尽量不要几个功能用同一把锁。
	4.	尽量减少同步代码块。
	5.	银行家算法
## ThreadLocal 是什么？ 有哪些使用场景？
&emsp;&emsp;ThreadLocal 为每个使用该变量的线程提供独立的变量副本，所以每个线程都可以独立地改变自己的副本，而不会影响到其它线程所对应的副本。

>* ThreadLocal 的使用场景：
1、数据库连接
2、session 管理
3、spring 声明式事务管理

## 说一下 synchronized 底层实现原理？
&emsp;&emsp;synchronized 是由一对 monitorenter/monitorexit 指令实现的，monitor 对象是同步的基本实现单元。 在 Java6 之前，monitor 的实现完全是依靠操作系统内部的互斥锁，因为需要进行用户态到内核态的切换，所以同步操作是一个无差别的重量级操作，性能也很低。但在 Java6 的时候，Java 虚拟机 对此进行了大刀阔斧地改进，提供了三种不同的 monitor 实现，也就是常说的三种不同的锁： 偏向锁 （Biased Locking）、轻量级锁和重量级锁，大大改进了其性能。

## Java当中有哪几种锁
>* 自旋锁: 自旋锁在JDK1.6之后就默认开启了。基于之前的观察，共享数据的锁定状态只会持续很短的时间，为了这一小段时间而去挂起和恢复线程有点浪费，所以这里就做了一个处理，让后面请求锁的那个线程在稍等一会，但是不放弃处理器的执行时间，看看持有锁的线程能否快速释放。为了让线程等待，所以需要让线程执行一个忙循环也就是自旋操作。在jdk6之后，引入了自适应的自旋锁，也就是等待的时间不再固定了，而是由上一次在同一个锁上的自旋时间及锁的拥有者状态来决定

>* 偏向锁: 在JDK1.之后引入的一项锁优化，目的是消除数据在无竞争情况下的同步原语。进一步提升程序的运行性能。 偏向锁就是偏心的偏，意思是这个锁会偏向第一个获得他的线程，如果接下来的执行过程中，改锁没有被其他线程获取，则持有偏向锁的线程将永远不需要再进行同步。偏向锁可以提高带有同步但无竞争的程序性能，也就是说他并不一定总是对程序运行有利，如果程序中大多数的锁都是被多个不同的线程访问，那偏向模式就是多余的，在具体问题具体分析的前提下，可以考虑是否使用偏向锁。

>* 轻量级锁: 为了减少获得锁和释放锁所带来的性能消耗，引入了“偏向锁”和“轻量级锁”，所以在Java SE1.6里锁一共有四种状态，无锁状态，偏向锁状态，轻量级锁状态和重量级锁状态，它会随着竞争情况逐渐升级。锁可以升级但不能降级，意味着偏向锁升级成轻量级锁后不能降级成偏向锁

## synchronized 和 volatile 的区别是什么？
>* volatile修饰的变量,jvm每次都从主内run读取,不会从工作内存读取；
>&emsp;而synchronized是锁住当前变量,同一时刻只有一个线程能访问当前变量；
>&emsp;volatile只是作用于变量,synchronized可以用在变量，方法，类，代码段中
>* valatile仅能实现变量修改的可见性,无法保证原子性；synchronized可以实现变量修改的可见性和原子性。
>* volatile 不会造成线程的阻塞；synchronized 可能会造成线程的阻塞。
>* volatile 标记的变量不会被编译器优化；synchronized 标记的变量可以被编译器优化。
## synchronized 和 Lock 有什么区别？
	1.  synchronized 是 java 内置的关键字，Lock 是个接口
	2.	synchronized 无法判断是否获取锁的状态，Lock 可以判断是否获取到锁.
	3.	synchronized 不需要手动获取锁和释放锁，使用简单，发生异常会自动释放锁，不会造成死锁；而Lock需要手动加锁和释放锁，如果使用不当，没有 unLock() 去释放锁就会造成死锁。
	4.	synchronized 可重入(同一个类中两个同步方法,获取到锁后不用每次都去获取),不可中断,非公平;Lock可中断可公平.
	5. 	synchronized 获得锁的线程阻塞,其他线程都会无限等待,Lock不会
## synchronized 和 ReentrantLock 区别是什么？
synchronized 早期的实现比较低效，对比 ReentrantLock，大多数场景性能都相差较大，但是在Java6 中对synchronized 进行了非常多的改进。
>* 1.synchronized遇到异常不catch,锁会自动释放；ReentrantLock 使用起来比较灵活，但是必须需要手动释放锁。
>* 2.synchronized是非公平锁,ReentrantLock可以实现公平锁.
>* 3.前者无法获取锁的状态,后者可以,tryLock()方法可以返回是否获得了锁.
>* 4.ReentrantLock可以精确唤醒线程,Synchronized要么随机唤醒一个,要么全部唤醒.
## 说一下 atomic 的原理？
>* atomic包中的类可以实现多线程环境下的变量操作,底层是调用CPU的CAS指令来进实现线程安全的。
>* atomic 主要利用 CAS（Compare And Wwap）乐观锁和 volatile 和 native 方法来保证原子操作，通过自旋保证当次修改的最终修改成功，通过降低锁粒度（多段锁）增加并发性能。
>* CAS相对于其他锁，不会进行内核态操作，有着一些性能的提升。但同时引入自旋，当锁竞争较大的时候，自旋次数会增多。cpu资源会消耗很高。
> 换句话说，CAS+自旋适合使用在低并发有同步数据的应用场景。

## 什么是CAS
CAS，全称为Compare and Swap，即比较-替换。假设有三个操作数：内存值V、旧的预期值A、要修改的值B，当且仅当预期值A和内存值V相同时，才会将内存值修改为B并返回true，否则什么都不做并返回false。当然CAS一定要volatile变量配合，这样才能保证每次拿到的变量是主内存中最新的那个值，否则旧的预期值A对某条线程来说，永远是一个不会变的值A，只要某次CAS操作失败，永远都不可能成功

## ConcurrentHashMap的并发度是什么?
ConcurrentHashMap的并发度就是segment的大小，默认为16，这意味着最多同时可以有16条线程操作ConcurrentHashMap，这也是ConcurrentHashMap对Hashtable的最大优势，任何情况下，Hashtable能同时有两条线程获取Hashtable中的数据吗？

## ConcurrentHashMap的工作原理
ConcurrentHashMap在jdk 1.6和jdk 1.8实现原理是不同的：
**jdk 1.6：**
>* ConcurrentHashMap是线程安全的，但是与Hashtablea相比，实现线程安全的方式不同。Hashtable是通过对hash表结构进行锁定，是阻塞式的，当一个线程占有这个锁时，其他线程必须阻塞等待其释放。
>* ConcurrentHashMap是采用分离锁的方式，它并没有对整个hash表进行锁定，而是局部锁定，也就是说当一个线程占有这个局部锁时，不影响其他线程对hash表其他地方的访问。
>* 具体实现:ConcurrentHashMap内部有一个Segment

**jdk 1.8：**
>* 在jdk 8中，ConcurrentHashMap不再使用Segment分离锁，而是采用一种乐观锁CAS算法来实现同步问题，但其底层还是“数组+链表->红黑树”的实现。

## 你有哪些多线程开发良好的实践?
>* 给线程命名
>* 最小化同步范围
>* 优先使用volatile
>* 尽可能使用更高层次的并发工具而非wait和notify()来实现线程通信,如BlockingQueue,Semeaphore
>* 优先使用并发容器而非同步容器.
>* 考虑使用线程池
## 线程的生命周期
>* 新建状态：创建一个线程对象  Thread t = new Thread();
>* 就绪状态（可运行状态）：线程对象创建后，其它线程调用了该对象的 start() 方法。 该状态的线程位于可运行线程池中，变得可运行（具备执行资格，没有执行权），等待获取CPU的使用权。
>* 运行状态：线程被分配到了CPU的时间片，执行程序代码。（具备执行资格，具备执行权）
>* 阻塞状态：阻塞状态是线程因为某种原因放弃CPU的使用权，暂时停止运行。直到线程进入就绪状态，才有机会转到运行状态。
>>阻塞状态分三种：
>>>等待阻塞：运行的线程执行wait() 方法，JVM 会把该线程放入等待池中。
>>>同步阻塞：运行的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM 会把该线程放入到锁池中。
>>>其它阻塞：运行的线程执行 sleep() 或者 join() 方法，或者发出了 IO 请求，JVM 会把该线程设置为阻塞状态。当 sleep() 状态超时，join() 等待线程终止或者超时，或者 IO 处理完毕时，线程重新转入就绪状态。
>* 死亡状态：当 main 方法结束、线程执行完了、调用stop 方法、或者因异常退出run() 方法，该线程结束生命周期，线程对象成为垃圾对象，等待垃圾回收器回收。

如下图分析：
![线程生命周期.png](线程生命周期.png)
