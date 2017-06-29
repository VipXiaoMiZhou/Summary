# Singleton

---
> "Ensure a class only has one instance, and provide a global point of access to
it.

> -- *Design Patterns Elements of Reusable Object-Oriented Software*

单例模式，顾名思义，就是保证一个类只有一个实例并为这个类提供一个全局的访问点。
   
   

## Structure

* 单例模式结构图

![](img/singleton/SingletonStructure.png)

```Singleton```： 定义一个```getInstance```方法让外界调用其唯一的实例。

* 单例模式代码
```java
public class Singleton {

    private static Singleton instance = new Singleton();

    private Singleton() {
    }

    public Singleton getInstance() {
        return instance;
    }
	
	// other functions
}
```

## 时序图

略

## 使用场景

在java中，单例模式有很对种实现的方式，先面就列举我知道的一些。

* 饿汉式单例

```java
public class Singleton {

    private static Singleton instance = new Singleton();

    private Singleton() {
    }

    public static Singleton getInstance() {
        return instance;
    }
	
	// other functions
}
```

饿汉式是典型的空间换时间，当类装载的时候就会创建类的实例，不管用不用，先创建出来，这样每次调用的时候，就不需要再判断，节省了运行时间。
因为```static```变量在jvm中只会有一份，它的唯一性由jvm保证，所以饿汉式单例是线程安全的。

* 懒汉式单例

“懒汉”是一种形象的说法，既然比较“懒”，那就等用的时候在创建这个实例好了。

 同步方法的饿汉式单例：
 
 ```java
 package com.designpattern.singleton;

public class LazySingleton {

    private static LazySingleton instance = null;

    private LazySingleton() {
    }

    public static synchronized LazySingleton getInstance() {
        if (null == instance) {
            instance = new LazySingleton();
        }
        return instance;
    }

}

```
 
”双重检查锁“饿汉式单例：

```java

package com.designpattern.singleton;

public class LazySingleton {

    private static LazySingleton instance = null;
    static Object lock = new Object();

    private LazySingleton() {
    }

    public static LazySingleton getInstance() {
	
        if (null == instance) {
            synchronized (lock) {
                if (null == instance) {
                    instance = new LazySingleton();
                }
            }
        }
        return instance;
    }
}

```
这里给出了两种能够在多线程环境中安全运行的饿汉式单例模式，虽然它们都能安全的在多线程环境中安全使用，但是都有一定的性能消耗。

先来看第一种：

```synchronized```保证了同时刻只有一个线程访问```getInstance```方法，当第一个线程获得锁进入该方法，此时```instance```为```null```，所以该线程```new```了一个```LazySingleton```对象。等第一个线程执行完以后，下一个线程获得锁，进入方法，此时```instance```已经不为```null```了，直接返回。
这种实现方式的弊端在于，```synchronized```的本意是保证只让第一个获得锁的线程去```new```这个对象，等这个对象不为```null```时，这把锁也就失去了存在的意义。在多线程环境中，大量线程会阻塞在这里，降低了效率。

再来看第二种，”双重检查锁“实现的方式：

和第一种不同的是，线程并不是每次进入```getInstance```方法都需要同步，而是先不同步，进入方法后，先检查实例是否存在，如果不存在才进行下面的同步块，这是第一重检查;进入同步块过后，再次检查实例是否存在，如果不存在，就在同步的情况下创建一个实例，这是第二重检查。这样一来，就只需要同步一次了，减少了多次在同步情况下进行判断所浪费的时间。

这种实现方式确实是比第一种同步方法实现的方式效率要高，竞争只会发生在多个线程第一次同时调用```getInstance```的时候发生，后面再调用的时候就直接返回。但是，它的弊端也出现在这里，双重检查只发生在当```instance```为```null```调用，而且只会用一次。写那么多只是为了使用一次？似乎有点不太友好。有没有一种既不用同步也延迟加载就可以实现单例的方法呢 ？ 答案是肯定的，接着往下看。


* 静态内部类单例

```java
package com.designpattern.singleton;

public class InnerClassSingleton {

    private static class SingletonHolder {
        private static InnerClassSingleton instance = new InnerClassSingleton();
    }

    public static InnerClassSingleton getInstance() {
        return SingletonHolder.instance;
    }
}


```
和饿汉式单例一样，内部类单例也是通过采用静态初始化的方式，由jvm来保证线程的安全性的。内部类单例模式还有一个好处就是并不是在类初始化的时候就去加载类，而是将类的初始化延迟到内部类中,只有当代码真正运行到```SingletonHolder.instance```的时候，才会去加载。这样，不仅可以弥补饿汉式单例模式的不足，也保证了线程安全。



* 枚举类单例

```java
package com.designpattern.singleton;

public enum EnumSingleton {

    instance;

    public void singletonOperation() {
	
        System.out.println("hello, java");
    }

    public static void main(String[] args) {
	
        instance.singletonOperation();
    }
}


```
为什么枚举类能够保证单例和线程安全呢 ？ 我们先不妨将这段代码先编译一下，然后在反编译看看是什么样子的。

 编译```javac EnumSingleton.java```，生成一个```EnumSingleton.class```
 
 反编译```javap -p EnumSingleton.class```
 
 
 ```java
public final class com.designpattern.singleton.EnumSingleton extends java.lang.Enum<com.designpattern.singleton.EnumSingleton> {
  public static final com.designpattern.singleton.EnumSingleton instance;
  private static final com.designpattern.singleton.EnumSingleton[] $VALUES;
  public static com.designpattern.singleton.EnumSingleton[] values();
  public static com.designpattern.singleton.EnumSingleton valueOf(java.lang.String);
  private com.designpattern.singleton.EnumSingleton();
  public void singletonOperation();
  public static void main(java.lang.String[]);
  static {};
}

```
可以看到，在反编译的代码中，多了一个私有化的构造函数，即使在源码中并没有声明。这样这也就禁止了外界通过调用构造函数创建枚举对象的可能。
其次，```instance``` 被编译成了一个```static final```的对象，也就是说，```instance```的唯一性和线程安全性由jvm保证，也可以保证它是唯一的。

甚至还可以像下面这样，在同一个枚举类里面定义不同的单例。

```java
package com.designpattern.singleton;

public enum EnumSingleton {

    instance1 {
        @Override
        public void singletonOperation() {
            System.out.println("instance1");
        }

    },
    instance2 {

        @Override
        public void singletonOperation() {
            System.out.println("instance2");
        }

    };

    public abstract void singletonOperation();

    public static void main(String[] args) {
        instance1.singletonOperation();
        instance2.singletonOperation();
    }
}

```

使用枚举来实现单例除了简洁之外，还有一个其它方式无法比拟的优点：使用枚举可防止反射强行调用构造器，而且还提供了自动序列化机制，防止反序列化的时候创建新的对象。

还是上面的反编译代码，在编译的时候，编译器会让枚举类去继承```java.lang.Enum```，那里面有什么呢？

```java
public abstract class Enum<E extends Enum<E>>

        implements Comparable<E>, Serializable {
		
	//...	省略一些不必要的

    /**
     * Throws CloneNotSupportedException.  This guarantees that enums
     * are never cloned, which is necessary to preserve their "singleton"
     * status.
     *
     * @return (never returns)
     */
    protected final Object clone() throws CloneNotSupportedException {
	
        throw new CloneNotSupportedException();
    }

    /**
     * prevent default deserialization
     */
    private void readObject(ObjectInputStream in) throws IOException,
        
		ClassNotFoundException {
        throw new InvalidObjectException("can't deserialize enum");
    }

    private void readObjectNoData() throws ObjectStreamException {
	
        throw new InvalidObjectException("can't deserialize enum");
    }
}

```
从```java.lang.Enum```可以看到，当调用```readObjectNoData```或```readObject```，都会抛异常。

## jdk中的单例

```java.lang.Runtime``` (饿汉式单例)

```java
public class Runtime {
    private static Runtime currentRuntime = new Runtime();

    /**
     * Returns the runtime object associated with the current Java application.
     * Most of the methods of class <code>Runtime</code> are instance
     * methods and must be invoked with respect to the current runtime object.
     *
     * @return  the <code>Runtime</code> object associated with the current
     *          Java application.
     */
    public static Runtime getRuntime() {
        return currentRuntime;
    }

    /** Don't let anyone else instantiate this class */
    private Runtime() {}
	
	//...
}

```


## 总结

从上面这些不同的单例实现方式来看，使用单例的时候需要注意的点：

1. 效率问题
2. 线程安全问题
3. 序列化和反序列化问题


虽然在文中说了很多不同实现方式之间的优缺点，但对于软件开发来说，没有绝对好的和绝对不好的，只有合适的才是对的。我们所做的种种分析，都是基于理论的情况，属于理论上的好，具体要使用那种实现方式还是要由具体需求来决定的。

