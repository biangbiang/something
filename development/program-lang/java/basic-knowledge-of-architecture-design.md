# 架构设计基础知识整理

## I. 原则:

灵活运用，而非刻意遵循

### 1. 基础原则

尽量少的重复代码，低耦合(尽量小的影响)，高内聚

模块，可小到一个类，大到一个系统

#### 模块间耦合因素

构建架构时，需要谨慎耦合的因素

* 模块间调用
* 模块间传递的数据量
* 模块间控制
* 模块间接口复杂度

#### 模块间耦合从弱到强顺序

构建架构或简单的类时，需要根据实际情况尽量契合弱的模块间耦合关系

做到职责分明，简单轻量，尽量少的潜在性的数据流动，尽量少的相互影响，避免牵一发而动全身

* 非直接耦合: 相互之间没有直接关系，而是由第三方模块控制和调用
* 数据耦合: 通过传递java的内置数据类型通讯
* 标记耦合: 都引用了共同的数据结构，并且通过传递该数据结构通讯
* 控制耦合: 通过传递开关、标志、名字等控制信息，明显的控制选择另一个模块的功能
* 外部耦合: 都访问一个java的内置数据类型的全局变量
* 公共耦合: 都访问了一个公共代码块( 全局数据结构、公共通讯区、内存公共覆盖区等)
* 内容耦合: 一个模块直接修改另外一个模块的数据。

#### 降低耦合度的方法

* 少用类继承，多用类接口隐藏实现细节
* 模块功能尽量单一
* 拒绝重复代码
* 尽量不使用全局变量(Android中的全局变量会有一些坑，因为Attach在ClassLoader上的，因此根据不同ROM的优化，可能会在未预料的情况被unload，导致数据丢失)
* 类成员变量与方法少用public，多用private
* 尽量不用硬编码(如 字符串放到 res/string.xml，SQL语句做一层基于业务的封装供上层使用)
* 使用设计模式，尽量让模块间的耦合关系保证在数据耦合或更弱

### 2. 原则汇总

    原则	基本概念	解决问题	基本实现
    开闭原则	对扩展开发，对修改关闭	实现热拔插，解耦方式	接口、抽象
    里氏代换原则	子类是父类的具体抽象，抽象并可代表父类(Is-A)	解释抽象化的具体原则	继承，抽象
    依赖倒转原则	针对接口编程，依赖于抽象不依赖于具体	易于拓展	接口编程时类型使用基类，而不使用具体实现的子类
    接口隔离原则	使用多个隔离接口，比使用单个接口要好	降低耦合	封装接口的时候，尽量用不同接口解决不同问题，尽量不要合用一个接口
    迪米特法则	以实体为单位，实体之间的相互作用尽量的少	降低耦合	写一个系统架构，或模块的时候，尽量少的对外依赖
    合成复用原则	优先使用合成/聚合，而非继承	可以通过引入抽象类更加灵活，相互耦合变小，更加简单	尽量将已有对象纳入到新对象中，成为新对象的一部分，而不使用继承的方式进行复用，如 ClassLoader 中双亲委派架构

#### 使用组合而非继承的场景:

优先使用对象组合，而非继承

* Has-A的关系，而非Is-A的关系
* 子类的主要目的是拓展父类，而非override或final，如果存在大量这种情况，改用组合
* 引入工具类，而非继承自工具类
* 有可能或不确定 子类 有可能被替换为 另外一个类的子类的情况 ( 如果出现这种情况，就需要修改。因此还不如使用 组合，如果有类似需求，再 组合如新的对象，进行拓展即可)

#### 继承需要注意

当已经选择使用继承时，需要注意

* 实现抽象方法，拓展新的特性方法，尽量少的重载父类非抽象方法
* 重载父类非抽象方法时: 方法前置条件(方法形参)要比父类方法更宽松，方法后置条件(方法返回值)要比父类更严格

#### 类之间的关系与UML表示

![](http://biangbiangpic.b0.upaiyun.com/blog/7c0b46716c799f085156f4c57e51ef6a.png)

## II. 常见的模式

### 1. MVC 与 MVP

![](http://biangbiangpic.b0.upaiyun.com/blog/82fb83d604e892aa4f75ab2939805bc4.gif)

From http://msdn.microsoft.com/en-us/library/ff647859.aspx

MVP(Model-View\_Presenter)是MVC(Model-View\_Controller)的一个子集。

* MVC中Controller控制全局事务，View将事件发送给Controller，Controller处理完事件同步给Model(数据库/数据模型)，View是通过所绑定的Model的改变来刷新自己。
* MVP中Presenter从View中获取数据，刷新Model，当Model中的数据发生改变后，Presenter读取Model并刷新View。

### 2. MVVM

![](http://biangbiangpic.b0.upaiyun.com/blog/b49721d350f8a9fb5f698ce292922a43.png)

    MVVM(View<->ViewModel->Model)

在Android中可以通过DataBinding，直接在Layout文件中绑定其ViewModel。

* View: 布局
* ViewModel: 负责显示数据(监听到Model中的数据变化进行显示)，以及处理用户交互(监听View布局中的用户Action)
* Model: 存储内容

### 3. MVVM-C

![](http://biangbiangpic.b0.upaiyun.com/blog/ef62c6c7bb75964a945b52d29aed122e.png)

    MVVM-C(View-ViewModel-Callback-Model)

* View: 布局
* Callback: 通常可以是Fragment或Activity，用于处理用户交互(监听View布局中的用户Action)
* ViewModel: 显示数据(监听Model中的数据变化进行显示)
* Model: 存储内容

## III. 设计模式

### 1. 工厂方法模式

![](http://biangbiangpic.b0.upaiyun.com/blog/1c0810f0713454924568146dae74bb06.png)

### 2. 单例模式

Initialization-on-demand holder idiom

Wiki

性能高，线程安全 基于JVM Class Loader保证Class唯一性线程安全的模型

    public class Something {
        private Something() {}
        private static class LazyHolder {
            private static final Something INSTANCE = new Something();
        }
        public static Something getInstance() {
            return LazyHolder.INSTANCE;
        }
    }

### 3. 建造者模式

与工厂模式区别是: 工厂模式关注构建单个类型类型；建造者模式关注构建符合类型对象。

![](http://biangbiangpic.b0.upaiyun.com/blog/dcc4fc52557e1deeeb17939a26652b13.png)

### 4. 原型模式

当前对象对外提供拷贝方法

#### 浅拷贝

除了基本数据类型外，其他类型的对象都只持有当前对象的引用，而非重新创建拷贝

#### Java中的Object#clone

* Object#clone()就已经提供了该对象的浅拷贝
* 如果需要使用Object#clone,需要类实现Clonable这个接口，来申明该类对象支持拷贝，否则会抛CloneNotSupportedException, 如果对象中存在队列成员变量，队列也需要实现Clonable

#### 深拷贝

所有成员变量都将重新创建

方式一:

直接序列化(Java中基于JVM层级最简单的让对象支持序列化的方式，实现Serializable），拷贝二进制流。

方式二(推荐）：

基于Object#clone()将非基本数据类型以外的元素都实现深拷贝，挨个深拷贝返回。

### 5. 适配器模式

![](http://biangbiangpic.b0.upaiyun.com/blog/15b25f620e2b5fe53ed28d70a3fff64a.png)

### 6. 装饰模式

![](http://biangbiangpic.b0.upaiyun.com/blog/3e30dca1ca0fce19bf7da32b92190928.png)

### 7. 代理模式

![](http://biangbiangpic.b0.upaiyun.com/blog/ccd56e23e889bed8b7bbabf05df42000.png)

### 8. 外观模式

![](http://biangbiangpic.b0.upaiyun.com/blog/2be7e57401bc29c2d401abd4c64cbf27.png)

### 9. 桥接模式

![](http://biangbiangpic.b0.upaiyun.com/blog/7d75d8587f1358c35890ec418d56f7f1.png)

### 10. 组合模式

![](http://biangbiangpic.b0.upaiyun.com/blog/13eff50a5105c9029de8e8f213c0943a.png)

### 11. 享元模式

![](http://biangbiangpic.b0.upaiyun.com/blog/0b8cd914170d8b592e229b230af0a109.png)

### 12. 策略模式

![](http://biangbiangpic.b0.upaiyun.com/blog/8752da3352961847a0ae5f589b87d873.png)

### 13. 模板方法模式

![](http://biangbiangpic.b0.upaiyun.com/blog/7c828a4cc0e45e2f5ccac19b18860ab8.png)

### 14. 观察者模式

![](http://biangbiangpic.b0.upaiyun.com/blog/34d7c76b4f64b3501c476e7fd4820e56.png)
