## 线程
-----

### 线程和进程
#### 进程
>进程是正在运行的程序的实例（an instance of a computer program that is being executed)。它是操作系统动态执行的基本单元，在传统的操作系统中，进程既是基本的分配单元，也是基本的执行单元。

#### 线程
>线程，有时被称为轻量级进程(Lightweight Process，LWP），是程序执行流的最小单元。线程是进程中的一个实体，是被系统独立调度和分派的基本单位，线程自己不拥有系统资源，只拥有一点儿在运行中必不可少的资源（堆栈，指令寄存器等）。

### 线程的创建
java创建线程的方式有两种,一种是继承```Thread```类,另外一种是实现```Runnable```接口。

继承```Thread```类：
```java
public class MyThread extends Thread {

    @Override
    public void run() {
        // do something
        System.out.println(this.getName());
    }

    public static void main(String[] args) {
        new MyThread().start();
    }
}
```

实现```Runnable```接口:
```java
 public static void main(String[] args) {

        Thread t = new Thread(new Runnable() {

            public void run() {
                // do something
                System.out.println("Thread t");
            }
        });

        t.start();
    }
```


### 线程的状态以及状态之间的转换
线程的状态

New:

当线程被创建时，它只会短暂的处于这种状态，这时系统已经为它分配了必要的资源，并执行了初始化。此时已经具备了获取系CPU时间的资格。

Runnable:
这种状态是介于已经调用```start()```方法但是还有获得系统时间的状态。在这种状态下，只要调度器把时间分给这个线程，该线程立马就可以开始执行。简单来说，在这种状态下线程可运行也可不运行，这取决与是否得到了系统时间。

Blocked :
线程能够运行，但是某个条件阻止了它的运行。当线程处于这个状态的时候，CPU调度器不会把系统时间分给它直到线程重新进入Runnable的状态，它才有可能执行。

Dead：
当线程正常执行完任务从```run()```返回或者意外终止时，线程处于死亡状态，线程不在有机会获得任何CPU时间。
<br>
![](img/threadstatus.jpg)
（图片来源于网络，侵删）

### 捕获异常
>由于线程的v本质，使得你你不能捕获从线程中逃逸的异常，一旦异常逃逸出```run()```方法，它就会传播到控制台。
《Java编程思想》

如下面程序所示，即使添加try-catch也没有作用：
```java
public class CatchThreadException {

    class ExceptionRunnable implements Runnable {

        public void run() {
            throw new RuntimeException();
        }

    }

    public void threadStart() {
        new Thread(new ExceptionRunnable()).start();
    }

    public static void main(String[] args) {

        try {
            new CatchThreadException().threadStart();
        } catch (RuntimeException e) {
            System.out.println("runtime exception!");
        }
    }

}
```
为了解决这个问题，java SE5中增加了一个新接口```UncaughtExceptionHandler```：
```java
 public interface UncaughtExceptionHandler {
        /**
         * Method invoked when the given thread terminates due to the
         * given uncaught exception.
         * <p>Any exception thrown by this method will be ignored by the
         * Java Virtual Machine.
         * @param t the thread
         * @param e the exception
         */
        void uncaughtException(Thread t, Throwable e);
    }
```
正如注释中描述的那样，当运行在线程中的代码抛出异常导致线程终止的时候，这个方法就会被调用。
使用的话也非常简单，只需要在线程中调用```setUncaughtExceptionHandler```就可以了。

```java
public void setUncaughtExceptionHandler(UncaughtExceptionHandler eh) {
        checkAccess();
        uncaughtExceptionHandler = eh;
}
```
调用```setUncaughtExceptionHandler```捕获线程异常

```java
    class ExceptionRunnable implements Runnable {

        public void run() {
            throw new RuntimeException("exception in run() ");
        }

    }

    class ThreadExceptionHandler implements UncaughtExceptionHandler {

        public void uncaughtException(Thread t, Throwable e) {
            System.out.println("uncaughtException : " + " " + t.getName() + "  " + e.getMessage());
        }
    }

    public void threadStart() {
        Thread task = new Thread(new ExceptionRunnable());
        task.setUncaughtExceptionHandler(new ThreadExceptionHandler());
        task.start();
    }

    public static void main(String[] args) {
        new CatchThreadException().threadStart();
    }

    //output : uncaughtException :  Thread-0  exception in run()
```



* main thread 停止以后其他线程不受影响
```java
    public static void main(String[] args) {
        Thread t = new Thread(new Runnable() {
            public void run() {
                try {
                    Thread.sleep(2000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName());
            }
        });
        t.setName("thread t");
        t.start();
        System.out.println("main thread over!");
    }

    // output :main thread over! , thread t
```

* ```sleep()``` 和 ```wait()```的区别

 ```wait()```是```Objectde```方法，```sleep()```是线程里面的方法。
 线程调用```wait()```的时候会让出锁，```sleep()```不会。
 ```sleep()```会抛出异常，```wait()```不会。
Thread.Sleep(0)的作用是“触发操作系统立刻重新进行一次CPU竞争”。

* ```Runnable``` 和 ```Callable```

```Runnable```

线程驱动任务，那任务就需要一种描述方式，在java中，这也就是```Runnable```的作用。在实现多线程的过程中，只需要任务体放在```run()```方法中并将这个任务附着在一个线程上，就可以实现多线程。```Runnable```中的```run()```方法不带返回值


```Callable```

```Callable```的功能和```Runnable```类似，只是```Callable```中的方法是```call()```,这个方法是带有返回值的。



* ```Executor``` 

```java
public interface Executor {

    /**
     * Executes the given command at some time in the future.  The command
     * may execute in a new thread, in a pooled thread, or in the calling
     * thread, at the discretion of the <tt>Executor</tt> implementation.
     *
     * @param command the runnable task
     * @throws RejectedExecutionException if this task cannot be
     * accepted for execution.
     * @throws NullPointerException if command is null
     */
    void execute(Runnable command);
}
```
> An object that executes submitted Runnable tasks. This interface provides a way of decoupling task submission from the mechanics of how each task will be run, including details of thread use, scheduling, etc. An Executor is normally used instead of explicitly creating threads. For example, rather than invoking new Thread(new(RunnableTask())).start() for each of a set of tasks, you might use:

根据 java doc的说法，这是一个用来执行提交的Runnable 任务。```Executor```提供了一种将任务提交与每个任务如何运行的机制分离的方法，包括线程使用、调度等细节。```Executor```通常被用来代替显示的创建线程。

* ```ExecutorService```




* ```Executors```



* ```CountDownLatch``` 和 ```CyclicBarrier```







[ConcurrentHashMap](https://www.ibm.com/developerworks/cn/java/java-lo-concurrenthashmap/index.html)

[volatile](http://www.cnblogs.com/dolphin0520/p/3920373.html)

[ThreadLocal](http://www.iteye.com/topic/103804)

[ThreadLocal](http://www.cnblogs.com/dolphin0520/p/3920407.html)
