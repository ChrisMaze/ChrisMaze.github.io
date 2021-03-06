---
layout: single
title:  "7 Design Principles"
date:   2021-03-28 11:35:10 +0800
toc: true
tags:
  - design-pattern
  - Backend
---

# 设计模式中常用的7大设计原则

如何写出高质量的代码呢？

通常我们在code review时，评判一段代码的是否是好代码的维度非常多，
例如，是否有code smell，是否易维护、可读、可扩展、可复用，是否灵活性高，是否可测等。

在OO设计中，有一些常见的设计原则可以帮助指导我们尽量写出优秀的代码， 
例如 SOLID原则，DRY原则, KISS原则等等。

本文会简单介绍7个在设计模式中常见的设计原则：

- [SRP-单一职责原则](#srp-单一职责原则)
- [OCP-开闭原则](#ocp-开闭原则)
- [LSP-里式替换原则](#lsp-里式替换原则)
- [ISP-接口隔离原则](#isp-接口隔离原则)
- [DIP-依赖倒置原则](#dip-依赖倒置原则)
- [LOD-迪米特法则](#lod-迪米特法则)
- [CRP-合成复用原则](#crp-合成复用原则)

## SRP-单一职责原则
### Definition
单一职责原则(Single Responsibility Principle，SRP)又称单一功能原则，
由 Uncle Bob(Robert C. Martin)于《敏捷软件开发：原则、模式和实践》一书中提出的。  
> 一个类或者模块只负责完成一个职责(或者功能)。  
> A class or module should have a single responsibility  

### Gist
1. 如何理解单一职责原则(SRP)？
> 一个类只负责完成一个职责或者功能。不要设计大而全的类，要设计粒度小、功能单一的类。  
> SRP是为了实现代码高内聚、低耦合，提高代码的复用性、可读性、可维护性。  
> SRP不仅适用于module、class中，还适用于function、file中

2. 如何判断类的职责是否足够单一？
> 不同的应用场景、不同阶段的需求背景、不同的业务层面，对同一个类的职责是否单一，可能会有不同的判定结果。  
> 实际上，一些侧面的判断指标更具有指导意义和可执行性，比如，出现下面这些情况就有可能说明这类的设计不满足SRP：
- 类中的代码行数、函数或属性过多，会影响代码的可读性和可维护性，我们就需要考虑对类进行拆分；
- 类依赖的其他类过多，或者依赖类的其他类过多，不符合高内聚、低耦合的设计思想，我们就需要考虑对类进行拆分；
- 私有方法过多，我们就要考虑能否将私有方法独立到新的类中，设置为 public 方法，供更多的类使用，从而提高代码的复用性；
- 比较难给类起一个合适名字，很难用一个业务名词概括，或者只能用一些笼统的 Manager、Context 之类的词语来命名，这就说明类的职责定义得可能不够清晰；
- 类中大量的方法都是集中操作类中的某几个属性，考虑是否可以将这几个属性和对应的方法拆分到新的类中。

3. 类的职责是否设计得越单一越好？
> 单一职责原则通过避免设计大而全的类，避免将不相关的功能耦合在一起，来提高类的内聚性。  
> 同时，类职责单一，类依赖的和被依赖的其他类也会变少，减少了代码的耦合性，以此来实现代码的高内聚、低耦合。  
> 但是，如果拆分得过细，实际上会适得其反，反倒会降低内聚性，也会影响代码的可维护性。

## OCP-开闭原则
### Definition
开闭原则(Open Closed Principle，OCP)由勃兰特·梅耶(Bertrand Meyer)提出。  
他在 1988 年的著作《面向对象软件构造》(Object Oriented Software Construction)中提出：

>软件实体(模块、类、方法等)应该 “对扩展开放、对修改关闭”。  
>Software entities(modules, classes, functions, etc.)should be open for extension，but closed for modification，

开闭原则的含义是：添加一个新的功能应该是，在已有代码基础上扩展代码(新增模块、类、方法等)，而非修改已有代码(修改模块、类、方法等)。  
而不破坏原有代码，对原有代码的侵入最低，我们就可以说是基于扩展而不是修改。

### Gist
1. 如何理解“对扩展开放、对修改关闭”？  
>添加一个新的功能，应该是通过在已有代码基础上扩展代码(新增模块、类、方法、属性等)，而非修改已有代码(修改模块、类、方法、属性等)的方式来完成。  
关于定义，我们有两点要注意。  
- 开闭原则并不是说完全杜绝修改，而是以最小的修改代码的代价来完成新功能的开发。
- 同样的代码改动，在粗代码粒度下，可能被认定为“修改”；在细代码粒度下，可能又被认定为“扩展”。

2. 如何做到“对扩展开放、修改关闭”？  
>我们要时刻具备扩展意识、抽象意识、封装意识。  
在写代码的时候，我们要多花点时间思考一下，这段代码未来可能有哪些需求变更，如何设计代码结构，事先留好扩展点，以便在未来需求变更的时候，
在不改动代码整体结构、做到最小代码改动的情况下，将新的代码灵活地插入到扩展点上。  
很多设计原则、设计思想、设计模式，都是以提高代码的扩展性为最终目的的。特别是 23 种经典设计模式，大部分都是为了解决代码的扩展性问题而总结出来的，都是以开闭原则为指导原则的。  
最常用来提高代码扩展性的方法有：多态、依赖注入、基于接口而非实现编程，以及大部分的设计模式(比如，装饰、策略、模板、职责链、状态)。  

## LSP-里式替换原则
### Definition
里氏替换原则(Liskov Substitution Principle，LSP)由麻省理工学院计算机科学实验室的里斯科夫(Liskov)女士
在 1987 年的“面向对象技术的高峰会议”(OOPSLA)上发表的一篇文章《数据抽象和层次》(Data Abstraction and Hierarchy)里提出来的，她提出：
  
>继承必须确保超类所拥有的性质在子类中仍然成立。  
>Inheritance should ensure that any property proved about supertype objects also holds for subtype objects.

简单理解一下就是：子类对象能够替换程序中父类对象出现的任何地方，并且保证原来程序的逻辑行为不变及正确性不被破坏。

里氏替换原是继承复用的基础，它反映了基类与子类之间的关系，是对开闭原则的补充，是对实现抽象化的具体步骤的规范。

### Gist
> 里式替换原则是用来指导，继承关系中子类该如何设计的一个原则。    
理解里式替换原则，最核心的就是理解“design by contract，按照协议来设计”这几个字。  
父类定义了函数的“约定”(或者叫协议)，那子类可以改变函数的内部实现逻辑，但不能改变函数原有的“约定”。这里的约定包括：函数声明要实现的功能；对输入、输出、异常的约定；甚至包括注释中所罗列的任何特殊说明。
  
> 理解这个原则，我们还要弄明白里式替换原则跟多态的区别。  
虽然从定义描述和代码实现上来看，多态和里式替换有点类似，但它们关注的角度是不一样的。  
多态是面向对象编程的一大特性，也是面向对象编程语言的一种语法。它是一种代码实现的思路。  
而里式替换是一种设计原则，用来指导继承关系中子类该如何设计，子类的设计要保证在替换父类的时候，不改变原有程序的逻辑及不破坏原有程序的正确性。


## ISP-接口隔离原则
### Definition
接口隔离原则(Interface Segregation Principle，ISP)要求程序员尽量将臃肿庞大的接口拆分成更小的和更具体的接口，让接口中只包含客户感兴趣的方法。

Robert Martin 在 SOLID 原则中是这样定义它的：  
> 客户端不应该被迫依赖于它不使用的方法。  
> Clients should not be forced to depend on methods they do not use.  

该原则还有另外一个定义：

> 一个类对另一个类的依赖应该建立在最小的接口上。  
> The dependency of one class to another one should depend on the smallest possible interface.

以上两个定义的含义是：要为各个类建立它们需要的专用接口，而不要试图去建立一个很庞大的接口供所有依赖它的类去调用。


### Gist
1. 接口隔离原则与单一职责原则的区别  
> 接口隔离原则和单一职责都是为了提高类的内聚性、降低它们之间的耦合性，体现了封装的思想，但两者是不同的：
> - SRP注重的是职责，而ISP注重的是对接口依赖的隔离。
> - SRP主要是约束类，它针对的是程序中的实现和细节；ISP主要约束接口，主要针对抽象和程序整体框架的构建。 
> - ISP提供了一种判断接口是否职责单一的标准：通过调用者如何使用接口来间接地判定。如果调用者只使用部分接口或接口的部分功能，那接口的设计就不够职责单一。 

2. 如何理解“接口隔离原则”？  
> 对“接口隔离原则”的“接口”二字，这里有三种不同的理解。
> - 如果把“接口”理解为一组接口集合，可以是某个微服务的接口，也可以是某个类库的接口等。  
> 如果部分接口只被部分调用者使用，我们就需要将这部分接口隔离出来，单独给这部分调用者使用，而不强迫其他调用者也依赖这部分不会被用到的接口。
> - 如果把“接口”理解为单个 API 接口或函数，部分调用者只需要函数中的部分功能，那我们就需要把函数拆分成粒度更细的多个函数，让调用者只依赖它需要的那个细粒度函数。
> - 如果把“接口”理解为 OOP 中的接口，也可以理解为面向对象编程语言中的接口语法。
>那接口的设计要尽量单一，不要让接口的实现类和调用者，依赖不需要的接口函数。

## DIP-依赖倒置原则
### Definition
依赖倒置原则(Dependence Inversion Principle，DIP)是 Robert C.Martin于 1996 年在 C++ Report 上发表的文章。

依赖倒置原则的原始定义为：  
> 高层模块不应该依赖低层模块，两者都应该依赖其抽象；抽象不应该依赖实现细节，具体的实现细节应该依赖抽象。  
> High level modules should not depend upon low level modules.Both should depend upon abstractions.Abstractions should not depend upon details. Details should depend upon abstractions.

其核心思想是：
> 要面向接口编程，不要面向实现编程

依赖倒置原则是实现开闭原则的重要途径之一，它降低了客户与实现模块之间的耦合。

由于在软件设计中，细节具有多变性，而抽象层则相对稳定，因此以抽象为基础搭建起来的架构要比以细节为基础搭建起来的架构要稳定得多。这里的抽象指的是接口或者抽象类，而细节是指具体的实现类。

使用接口或者抽象类的目的是制定好规范和契约，而不去涉及任何具体的操作，把展现细节的任务交给它们的实现类去完成。

### Gist
1. 控制反转
> 实际上，控制反转是一个比较笼统的设计思想，并不是一种具体的实现方法，一般用来指导框架层面的设计。  
> 这里所说的“控制”指的是对程序执行流程的控制，而“反转”指的是在没有使用框架之前，程序员自己控制整个程序的执行。  
> 在使用框架之后，整个程序的执行流程通过框架来控制。流程的控制权从程序员“反转”给了框架。
2. 依赖注入
> 依赖注入和控制反转恰恰相反，它是一种具体的编码技巧。  
> 我们不通过 new 的方式在类内部创建依赖类的对象，而是将依赖的类对象在外部创建好之后，
> 通过构造函数、函数参数等方式传递(或注入)给类来使用。
3. 依赖注入框架
> 我们通过依赖注入框架提供的扩展点，简单配置一下所有需要的类及其类与类之间依赖关系，
> 就可以实现由框架来自动创建对象、管理对象的生命周期、依赖注入等原本需要程序员来做的事情。
4. 依赖反转原则
> 依赖反转原则也叫作依赖倒置原则。  
> 这条原则跟控制反转有点类似，主要用来指导框架层面的设计。  
> 高层模块不依赖低层模块，它们共同依赖同一个抽象。
> 抽象不要依赖具体实现细节，具体实现细节依赖抽象。
5. 高层模块和底层模块
> 所谓高层模块和低层模块的划分，简单来说就是，在调用链上，调用者属于高层，被调用者属于低层。  
> 在平时的业务代码开发中，高层模块依赖底层模块是没有任何问题的。  
> 实际上，这条原则主要还是用来指导框架层面的设计，跟前面讲到的控制反转类似。
>
> 我们拿 Tomcat 这个 Servlet 容器作为例子来解释一下。
> 
> Tomcat 是运行 Java Web 应用程序的容器。
> 我们编写的 Web 应用程序代码只需要部署在 Tomcat 容器下，便可以被 Tomcat 容器调用执行。
> 按照之前的划分原则，Tomcat 就是高层模块，我们编写的 Web 应用程序代码就是低层模块。  
> Tomcat 和应用程序代码之间并没有直接的依赖关系，两者都依赖同一个“抽象”，也就是 Servlet 规范。
> Servlet 规范不依赖具体的 Tomcat 容器和应用程序的实现细节，而 Tomcat 容器和应用程序依赖 Servlet 规范。

## LOD-迪米特法则
### Definition
迪米特法则(Law of Demeter，LoD)又叫作最少知识原则(Least Knowledge Principle，LKP)，产生于 1987 年美国东北大学(Northeastern University)的一个名为迪米特(Demeter)的研究项目，由伊恩·荷兰(Ian Holland)提出，被 UML 创始者之一的布奇(Booch)普及，后来又因为在经典著作《程序员修炼之道》(The Pragmatic Programmer)提及而广为人知。

迪米特法则的定义是：
> 只与你的直接朋友交谈，不跟“陌生人”说话。  
> Talk only to your immediate friends and not to strangers.  

其含义是：
> 如果两个软件实体无须直接通信，那么就不应当发生直接的相互调用，可以通过第三方转发该调用。

其目的是:
> 降低类之间的耦合度，提高模块的相对独立性。

迪米特法则中的“朋友”是指：
> 当前对象本身、当前对象的成员对象、当前对象所创建的对象、当前对象的方法参数等，这些对象同当前对象存在关联、聚合或组合关系，可以直接访问这些对象的方法。

### Gist
1. 如何理解“高内聚、松耦合”？
> “高内聚、松耦合”是一个非常重要的设计思想，能够有效提高代码的可读性和可维护性，缩小功能改动导致的代码改动范围。  
>“高内聚”用来指导类本身的设计，“松耦合”用来指导类与类之间依赖关系的设计。  
>  
> 所谓高内聚，就是指相近的功能应该放到同一个类中，不相近的功能不要放到同一类中。相近的功能往往会被同时修改，放到同一个类中，修改会比较集中。  
> 所谓松耦合指的是，在代码中，类与类之间的依赖关系简单清晰。即使两个类有依赖关系，一个类的代码改动也不会或者很少导致依赖类的代码改动。
2. 如何理解“迪米特法则”？
> 不该有直接依赖关系的类之间，不要有依赖；有依赖关系的类之间，尽量只依赖必要的接口。  
> 迪米特法则是希望减少类之间的耦合，让类越独立越好。  
> 每个类都应该少了解系统的其他部分。一旦发生变化，需要了解这一变化的类就会比较少。

## CRP-合成复用原则
### Definition
合成复用原则(Composite Reuse Principle，CRP)又叫组合/聚合复用原则(Composition/Aggregate Reuse Principle，CARP)。  

它要求在软件复用时，要尽量先使用组合或者聚合等关联关系来实现，其次才考虑使用继承关系来实现。

如果要使用继承关系，则必须严格遵循里氏替换原则。合成复用原则同里氏替换原则相辅相成的，两者都是开闭原则的具体实现规范。

### Gist
1. 继承复用
> 继承复用虽然有简单和易实现的优点，但它存在以下缺点:
- 继承复用破坏了类的封装性。因为继承会将父类的实现细节暴露给子类，父类对子类是透明的，所以这种复用又称为“白箱”复用。
- 子类与父类的耦合度高。父类的实现的任何改变都会导致子类的实现发生变化，这不利于类的扩展与维护。
- 它限制了复用的灵活性。从父类继承而来的实现是静态的，在编译时已经定义，所以在运行时不可能发生变化。
2. 组合/聚合 复用
> 采用组合/聚合复用时，可以将已有对象纳入新对象中，使之成为新对象的一部分，新对象可以调用已有对象的功能，它有以下优点:  
- 它维持了类的封装性。因为成分对象的内部细节是新对象看不见的，所以这种复用又称为“黑箱”复用。
- 新旧类之间的耦合度低。这种复用所需的依赖较少，新对象存取成分对象的唯一方法是通过成分对象的接口。
- 复用的灵活性高。这种复用可以在运行时动态进行，新对象可以动态地引用与成分对象类型相同的对象。
