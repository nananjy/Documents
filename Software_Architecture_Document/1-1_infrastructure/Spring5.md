# Spring5

## 1 软件架构设计原则

### [开闭原则（Open-Cloesd Principle, OCP）](1-1_开闭原则.md)
&emsp;一个软件实体（如类、模块和函数）应该对扩展开放，对修改关闭。强调用抽象构建框架，用实现扩展细节，可以提高软件系统的可复用性及可维护性。
<br/>实现：价格优惠变动。接口ICourse中写有 `Double getPrice()` 方法，JavaCourse类实现接口ICourse的方法 `@Override public Double getPrice() {return this.price;}` ，JavaDiscountCourse类继承JavaCourse类，重写方法 ```public Double getPrice() {return super.getPrice() * 0.85;}``` 。

### [依赖倒置原则（Dependence Inversion Principle, DIP）](1-2_依赖倒置原则.md)
&emsp;高层模块不依赖低层模块，均依赖其抽象。有依赖注入、构造器方式和Setter方式。
<br/>实现：追加课程。接口ICourse中写有 `void study()` 方法，JavaCourse类实现接口ICourse的方法 `@Override public void study() {log.info("Tom在学习Java课程");}` ，PythonCourse类实现接口ICourse的方法 `@Override public void study() {log.info("Tom在学习python课程");}` ，Tom类中 `public void study(ICourse course) { course.study(); }` ，控制层Controller访问 `@GetMapping(value = "dependenceInversionPrinciple") public String dependenceInversionPrinciple() {tom.studyJavaCourse(); tom.studyPythonCourse(); tom.study(new JavaCourse()); tom.study(new PythonCourse()); return "成功依赖倒置原则";}` 。 构造器方式：Tom类中 `private ICourse course; public Tom(ICourse course) {this.course = course;} public void study() { course.study(); }` ，Controller访问 `Tom tom1 = new Tom(new PythonCourse()); tom1.study();` 。 Setter方式：如果Tom是全局单例，则我们就只能选择用Setter方式来注入。Tom类中 `public Tom() {} public void setCourse(ICourse course) {this.course = course;}` ，Controller访问 `Tom tom2 = new Tom(); tom2.setCourse(new PythonCourse()); tom2.study();` 。
<br/>切记：以抽象为基准比以细节为基准搭建起来的架构要稳定得多，因此在确定需求后，要面向接口编程，先顶层再细节地设计代码结构。

### [单一职责原则（Simple Responsibility Pinciple, SRP）](1-3_单一职责原则.md)
&emsp;一个类、接口或方法只负责一项职责。不要存在多于一个导致类变更的原因。假设我有一个类负责两个职责，一旦发生需求变更，修改其中一个职责的逻辑代码，有可能导致另一个职责的功能发生故障。这样一来，这个类就存在两个导致类变更的原因。

### [接口隔离原则（Interface Segregation Principle, ISP）](1-4_接口隔离原则.md)
&emsp;是指用多个专门的接口，而不使用单一总接口，客户端不应该依赖它不需要的接口。这个原则指导我们在设计接口时应当注意以下几点：
1. 一个类对另一个类的依赖应该建立在最小的接口之上。
2. 建立单一接口，不要建立庞大雕肿的接口。
3. 尽量细化接口，接口中的方法尽量少（不是越少越好，一定要适度）。
<br/>接口隔离原则符合我们常说的高内聚、低耦合的设计思想，可以使类具有很好的可读性、可扩展性和可维护性。

### [迪米特原则（Law of Demeter, LoD）](1-5_迪米特原则.md)
&emsp;指一个对象应该对其他对象保持最少的了解，又叫最少知道原则(Least Knowledge Principle, LKP)，尽量降低类与类之间的耦合度。迪米特原则主要强调只和朋友交流，不和陌生人说话。出现在成员变量、方法的输入、输出参数中的类都可以称为成员朋友类，而出现在方法体内部的类不属于朋友类。

### [里氏替换原则（Liskov Substitution Principle, LSP）](1-6_里氏替换原则.md)
&emsp;指如果对每一个类型为Tl的对象ol，都有类型为T2的对象o2，使得以Tl定义的所有程序P在所有的对象o1都替换成o2时，程序p行为没有发生变化，那么类型T2是类型Tl的子类型。引申含义为子类可以扩展父类的功能，但不能改变父类原有功能。
1. 子类可实现父类的抽象方法，但不能覆盖父类的非抽象方法。
2. 子类可以增加自己特有的方法。
3. 当子类的方法重载父类的方法时，方法的前置条件（即方法输入/入参）要比父类方法的输入参数更宽松。
4. 当子类的方法实现父类的方法（重写/重载或实现抽象方法），方法的后置条件（方法的输出/返回值）要比父类更严格或与父类一样。

### [合成复用原则（Composite/Aggregate Reuse Principle, CARP）](1-7_合成复用原则.md)
&emsp;指尽量使用对象组合（has-a)/聚合（contains-a）而不是继承关系达到软件复用的目的。可以使系统更加灵活，降低类与类之间的耦合度，一个类的变化对其他类造成的影响相对较小。

### 原则设计总结
&emsp;在适当的场景遵循设计原则，这体现的是一种平衡取舍，可以帮助我们设计出更加优雅的代码结构。

## 2 Spring中常用的设计模式

### 为什么要从设计模式开始


### 工厂模式详解
#### 简单工厂模式
&emsp;指由一个工厂对象决定创建哪一种产品类型的实例，但它不属于GoF的23种设计模式。适用于工厂类负责创建的对象较少的场景，且客户端只需要传入工厂类的参数，对于如何创建对象不需要关心。

### 单例模式详解
&emsp;

### 原型模式详解


### 代理模式详解


### 委派模式详解


### 策略模式详解


### 模板模式详解


### 适配器模式详解


### 装饰者模式详解


### 观察者模式详解


### 各设计模式的总结与对比


### Spring中的编程思想总结


## 3 Spring的前世今生


### 一切从Bean开始

### Spring的设计初衷

### BOP编程伊始

### 理解BeanFactory

### AOP编程理念


## 4 Spring5系统架构


### 核心容器

### AOP和设备支持

### 数据访问和集成

### Web组件

### 通信报文

### 集成测试

### 集成兼容

### 各模块之间的依赖关系

## 5 Spring版本命名规则
## 6 Spring源码下载及构建技巧
## 7 300行代码手写提炼Spring核心原理


### 自定义配置

### 容器初始化

### 运行效果演示

## 8 一步一步手绘Spring IoC运行时序图


### Spring核心之IoC容器初体验

### 基于XML的IoC容器初始化

### 基于注解的IoC容器初始化

### IoC容器初始化小结

## 9 一步一步手绘Spring DI运行时序图


### Spring自动装配之依赖注入

### Spring IoC容器中哪些鲜为人知的细节

## 10 一步一步手绘Spring AOP运行时序图


### Spring AOP初体验

### Spring AOP源码分析

## 11 一步一步手绘Spring MVC运行时序图


### 初探Spring MVC请求处理流程

### Spring MVC九大组件

### Spring MVC源码分析

### Spring MVC优化建议

## 12 环境准备


### IDEA集成LomBok插件

### 从Servlet到ApplicationContext 

### 准备基础配置

## 13 IoC顶层结构设计


### Annotation(自定义配置)模块

### core(顶层接口)模块

### beans(配置封装)模块

### context(IoC容器)模块

## 14 完成DI模块的功能


## 15 完成MVC模块的功能


### MVC顶层设计

### 业务代码实现

### 定制模板页面

### 运行效果演示

## 16 完成AOP代码植入


### 基础配置

### 完成AOP顶层设计

### 设计AOP基础实现

### 植入业务代码

### 运行效果演示

## 17 数据库事务原理详解
## 18 Spring JDBC源码初探
## 19 基于Spring JDBC手写ORM框架


### 实现思路概况

### 搭建基础架构

### 基于Spring JDBC实现关键功能

### 动态数据源切换的底层原理

### 运行效果演示

## 20 Spring5新特性总结
## 21 基于Spring的经典高频面试题


### 什么是Spring框架，Spring框架有哪些主要模块

### 使用Spring框架能带来哪些好处

### 什么是控制反转（IoC），什么是依赖注入

### 在Java中依赖注入有哪些好处

### BeanFactory和ApplicationContext有什么区别

### Spring提供几种配置方式来设置元数据

### 如何使用XML配置方式配置Spring

### Spring提供哪些配置形式

### 怎样用注解的方式配置Spring

### 请解释Spring Bean的生命周期

### Spring Bean作用域的区别是什么 

### 什么是Spring Inner Bean

### Spring中的单例Bean是线程安全的吗

### 请举例说明如何在Spring中注入一个Java集合

### 如何向Spring Bean中注入java.util.Properties

### 请解释Spring Bean的自动装配

### 自动装配有哪些局限性

### 请解释各种自动装配模式的区别

### 请举例解释@Required注解

### 请举例说明@Qualifier注解
表明哪个Service实现类才是我们所需要的。注意需要搭配@Autowired使用，二者并非包含关系。

### 构造方法注入和设值注入有什么区别

### Spring中有哪些不同类型的事件

### FileSystemResource和ClassPathResource有什么区别

### Spring中用到了哪些设计模式

### 在Spring中如何更有效地使用JDBC

### 请解释Spring中的IoC容器

### 在Spring可以注入null或空字符串吗

