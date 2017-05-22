 
## 泛型
---------
### 什么是泛型？

>"有许多的原因促成了泛型的出现，而最引人注目的地方的一个原因，就是为了创建容器"

>《Java编程思想 （第4版）》

泛型，顾名思义，就是要适应许多的类型，它是对Java类型系统的一种扩展，以支持创建按类型进行参数化的类。泛型的出现就是为了使程序具有广泛的表达能力。<br/>

### 什么是擦除机制？

>"擦除的正当理由是从非泛化代码到泛化代码的转变过程。"

>《Java编程思想 （第4版）》

简单来说，Java 的泛型在编译器有效，在运行期被擦除，所有泛型参数类型信息在编译后都会被清除掉。

```java
    public void Erased(List<String> strList) {

    }

    public void Erased(List<Integer> intList) {

    }
```
这段代码咋看没有什么问题的，但是在eclipse中会报一个错误：
>Erasure of method Erased(List<String>) is the same as another method in type ErasedType

这段代码编译过后是这样的：

```java
public void Erased(List<E> list)  {  

}
```
因为有了擦除机制，编译器会在编译含有泛型的代码的时候采取去泛型化的措施，所有代码里泛型信息都被擦除成为“它们”的原生类型。```List<String>``` 和 ```List<Ingeter>```的信息被编译器擦除为```List<E>```。



再来看一个例子：

```java

 public static void main(String args[]) {
        Class c1 = new ArrayList<String>().getClass();
        Class c2 = new ArrayList<Integer>().getClass();

        System.out.println("c1 == c2 : " + (c1 == c2));
    }

```
输出： ```c1 == c2 :true```


因为擦除只发生在编译阶段，编译的过程中，正确检验泛型结果后，会将泛型的相关信息擦出，并且在对象进入和离开方法的边界处添加类型检查和类型转换的方法。也就是说，成功编译过后的``class``文件中是不包含任何泛型信息的，泛型信息不会进入到运行时阶段。所以只要绕过了编译阶段，也就绕过了擦除机制。下面这段代码论证了这个问题：

```java
public class GenericsCompile {

    public static void main(String args[]) {
        ArrayList<String> a = new ArrayList<String>();
        a.add("You");
        Class clazz = a.getClass();
        try {
            Method method = clazz.getMethod("add", Object.class);
            method.invoke(a, 100);
            System.out.println(a);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
输出： ```[You, 100]```


### 泛型方法

>泛型既可以作用在整个类上也可以单独作用在某个方法上。泛型方法使得该方法可以独立于类而产生变化。以下是一个基本的指导原则是：
**无论何时，只要你能做到，你就应该尽量使用泛型方法。**也就是说，如果使用泛型方法可以取代将整个类泛化，那么应该有限采用泛型方法。<br/>

>《Java编程思想 （第4版）》


对于一个```static```的方法而言，是没有办法访问类的泛型变量的。所以，对于一个```static```的方法，若要使其具有泛型能力，则必须要变成泛型方法:

```java
public class Main {

    public static <T> void out(T t) {
        System.out.println(t);
    }

    public static void main(String[] args) {
        out("findingsea");
        out(123);
        out(11.11);
        out(true);
    }
}
```

```java
public <T> T hello(T t) {
        return t;
    }
```
如上面代码所示：，```<T>```是用来规范```T```的，例如```<T extends Object>```就规定了```T```边界，即规定了所有出现```T```的地方，```T```类型必须是```Object```的子类。而方法名```hello```前面的```T```表示的函数的返回值。

### 泛型使用注意事项

- 任何基本类型都不能作为泛型参数的类型
```List<int> ints = new ArrayList<int>(); //Syntax error, insert "Dimensions" to complete ReferenceType```
- 实现参数化接口
一个类不能同时实现泛型接口的两种变体。因为擦除机制的存在，会使得这两个接口没有任何显著的区别。
- 重载
当被擦除的参数不能产生唯一的参数列表的时候，必须提供显著有区别的方法名。
- 参数协变
ddd
- 异常
ddd


- **```<? extends T>```和 ```<? super T>```的区别**

```<? extends T>``` 表示该通配符所代表的类型是T类型的子类，上边界。
```<? super T>```   表示该通配符所代表的类型是T类型的父类，下边界。

- **泛型不能声明成为成员变量（没有使用类型参数的时候）。**
```java
public class ErasedType {
    private T t;
}
// error: T cannot be resolved to a type
```

- **泛型方法是静态的时候不能使用类类型参数**
```java
public class ErasedType<T> {

    private static T t; //root cause : Cannot make a static reference to the non-static type T
    
    public ErasedType(T t) {
        this.t = t;
    }
    
    public static void main(String args[]) {
        System.out.println(t);
    }
}
```

- **不能对确切的泛型类型使用```instanceof```操作。如下面的操作是非法的，编译时会出错。如果规定了边界，只能调用边界内的方法。**
- **不能创建一个确切的泛型类型的数组**
```java
T[] s = new T[100];  //Cannot create a generic array of T
```