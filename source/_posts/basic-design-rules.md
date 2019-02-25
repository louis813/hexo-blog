---
title: 基本的设计原则
date: 2019-02-25 14:02:38
tags: design
---

# 简介

进行设计时，应该参考的原则和方法：

* SOLID
* KISS
* DRY
* YANGI
* CQS
* CQRS
* DDD

## SOLID
在程序设计领域， SOLID（**单一功能、开闭原则、里氏替换、接口隔离以及依赖反转**）是由罗伯特·C·马丁在21世纪早期引入的记忆术首字母缩略字，指代了面向对象编程和面向对象设计的五个基本原则。当这些原则被一起应用时，它们使得一个程序员开发一个容易进行软件维护和扩展的系统变得更加可能。SOLID所包含的原则是通过引发编程者进行软件源代码的代码重构进行软件的代码异味清扫，从而使得软件清晰可读以及可扩展时可以应用的指南。

|首字母|概念|说明|
|--|--|--|
|S (Single Responsibility Principle) <br>单一功能原则|规定每个类都应该有一个单一的功能，并且该功能应该由这个类完全封装起来。|例子：一个用于编辑和打印报表的模块。这样的模块存在两个改变行为。第一，报表的内容可以改变（编辑）。第二，报表的格式可以改变（打印）。这两个行为因为不同的起因而发生：一个是本质的修改，一个是表面的修改。单一功能原则认为这两个行为事实上是两个分离的功能，因此他们应该分离在不同的类或者模块里。把有不同的改变原因的事物耦合在一起的设计是糟糕的。|
|O (Open/Closed Principle) <br>开闭原则|软件体应该是对于扩展开放的，但是对于修改封闭的。|对扩展开放，意味着有新的需求或变化时，可以对现有代码进行扩展，以适应新的情况。对修改封闭，意味着类一旦设计完成，就可以独立完成其工作，而不要对类进行任何修改。|
|L (Liskov Substitution Principle) <br>里氏替换原则|程序中的对象应该是可以在不改变程序正确性的前提下被它的子类所替换的。|在很多情况下，在设计初期我们类之间的关系不是很明确，LSP则给了我们一个判断和设计类之间关系的基准：需不需要继承，以及怎样设计继承关系。|
|I (Interface Segregation Principle) <br>接口隔离原则|多个特定客户端接口要好于一个宽泛用途的接口。|指明客户（client）应该不依赖于它不使用的方法。接口隔离原则(ISP)拆分非常庞大臃肿的接口成为更小的和更具体的接口，这样客户将会只需要知道他们感兴趣的方法。|
|D (Dependency Inversion Principle) <br>依赖反转原则|依赖于抽象而不是一个实例。|高层次的模块不应该依赖于低层次的模块，两者都应该依赖于抽象接口。抽象接口不应该依赖于具体实现。而具体实现则应该依赖于抽象接口。|


## KISS

KISS 原则是英语 Keep It Simple, Stupid 的首字母缩略字，是一种归纳过的经验原则。KISS 原则是指在设计当中应当注重简约的原则。总结工程专业人员在设计过程中的经验，大多数系统的设计应保持简洁和单纯，而不掺入非必要的复杂性，这样的系统运作成效会取得最优；因此简单性应该是设计中的关键目标，尽量回避免不必要的复杂性。

## DRY

DRY 是 Do Not Repeat Yourself 的简称，不要写重复的代码，可以使用代码重构里的提取到方法，提取到类来做这事。

## YANGI

YANGI 是 You aren’t gonna need it 的简称，永久不要为某个假设去多写功能代码；用到了它，再去实现它。

## CQS

CQS 表示 Command-Query Separation Principle。CQS 是针对方法的经典面向对象设计原则。该原则指出，任何方法都可能是如下情况之一: 

1. 执行动作(更新,调整...)的命令方法，这种方法通常具有改变对象状态等副作用, 并且是 void 的。
2. 向调用者返回数据的查询，这种方法没有副作用，不会永久性的改变任何对象的状态。

关键是, 一个方法不应该同时属于以上两种类型。CQS 更像准则，而不是原则。

CQS 被公认为计算机科学理论中具有价值的原则。因为遵守该原则，你能够更容易的推测出程序的状态，在查询状态时不会同时发生变更。这样使得设计更便于理解和预见。

CQS 被视为 CQRS 的基本原则。

## CQRS

CQRS 表示 Command Query Responsibility Segregation，即命令和查询责任分离，是由 Greg Young 提出的一种将系统的读（查询）、写（命令）操作分离为两种独立子系统的架构模式。

CQRS 基于 CQS 原则，只是更为详细。它可以视为基于命令和事件的模式，另外在异步消息方面可选。

CQRS 让读模型和写模型完全分离，因此可以对读操作和写操作进行优化。这样会带来性能上的提升，同时也会让代码更清晰、更简单，代码反映了领域模型，提升了代码的可维护性。


不过，尽管 CQRS 可以让应用程序变得更健壮，但并不是说所有的应用程序都要使用 CQRS：我们应该在必要的时候选择正确的方案。


## DDD

TBC

# 参考

[CQRS - Martin Fowler](https://martinfowler.com/bliki/CQRS.html)
[正交设计，OO 与 SOLID - InfoQ](https://www.infoq.cn/article/orthogonal-design-oo-and-solid)
[使用聚合、事件溯源和 CQRS 开发事务型微服务 - InfoQ](https://www.infoq.cn/article/microservices-aggregates-events-cqrs-part-1-richardson)
[在微服务中应用简化后的 CQRS 和 DDD 模式 - Microsoft](https://docs.microsoft.com/zh-cn/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/apply-simplified-microservice-cqrs-ddd-patterns)