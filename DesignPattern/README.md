# Desgin Pattern

---
## Principles

* 单一职责

就一个类而言，应该仅有一个引起它变化的原因

* 开放——封闭原则

软件实体（类，模块，函数等）应该支持扩展，但不支持修改(Open for extension, Close for modification)

* 依赖倒转原则

1. 高层模板不应该依赖底层模块，两个都依赖抽象
2. 抽象不应该依赖细节，细节应该依赖抽象

* 里氏替换原则

一个软件实体如果使用一个父类的话，那么也一定其子类而不应该有任何差别。

* 迪米特法则

如果两个类不必直接通信，那么这两个类不因该发生直接的相互引用。如果一个类需要调用另一个类的方法，可以通过第三者转发这个调用。

* 合成/聚合复用原则

如果可以，应尽量使用合成/聚合而不是继承

## Patterns

* [单例模式](Singleton.md)

* [观察者模式](Observe.md)

* [工厂方法](Factory.md)

* [简单工厂](SimpleFactory.md)

* [责任链模式](ChainofReponsibility.md)

* [建造者模式](Builder.md)

* [备忘录模式](Memento.md)

* [适配器模式](Adapter.md)

* [命令模式](Command.md)

* [代理模式](Proxy.md)

* [外观模式](Facade.md)

* [组合模式](Composite.md)


## Design Pattern in JDK
[Design Pattern in JDK](https://stackoverflow.com/questions/1673841/examples-of-gof-design-patterns-in-javas-core-libraries)
