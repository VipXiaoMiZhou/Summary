# SimpleFactory

## Structure

![](img/simplefactory/simplefactory.png)

```Productor``` : 定义产品的统一操作

```java

package com.designpattern.simplefactory;

public interface Productor {
    public void operation();
}

```


```ConcreteProductorA``` : 具体产品A

```java

package com.designpattern.simplefactory;

public class ConcreteProductorA implements Productor {

    public void operation() {
        System.out.println(this.getClass().getName());
    }
}

```

```ConcreteProductorB``` ：具体产品B

```ConcreteProductorC``` ：具体产品C

```ConcreteCreator``` : 简单工厂，负责生产各种产品

```java

package com.designpattern.simplefactory;

public class ConcreteCreator {
    public static Productor create(String choice) {
        if (choice == null || choice.equals("")) {
            return null;
        }

        switch (choice) {
        case "A":
            return new ConcreteProductorA();
        case "B":
            return new ConcreteProductorB();
        case "C":
            return new ConcreteProductorC();
        default:
            return null;
        }
    }
}

```

```Client``` :


```java

package com.designpattern.simplefactory;

public class Client {
    public static void main(String[] args) {
        Productor productorA = ConcreteCreator.create("A");
        productorA.operation();

        Productor productorB = ConcreteCreator.create("B");
        productorB.operation();

        Productor productorC = ConcreteCreator.create("C");
        productorC.operation();
    }
}

```

