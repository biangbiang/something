垃圾回收
========

在计算机科学中，垃圾回收（英语：Garbage Collection，缩写为GC）是一种自动的存储器管理机制。当一个电脑上的动态存储器不再需要时，就应该予以释放，以让出存储器，这种存储器资源管理，称为垃圾回收（garbage collection）。垃圾回收器可以让程序员减轻许多负担，也减少程序员犯错的机会。垃圾回收最早起源于LISP语言。目前许多语言如Smalltalk、Java、C#和D语言都支持垃圾回收器。

### 特征

垃圾回收器有两个基本的原理：

1. 考虑某个对象在未来的程序运行中，将不会被访问。

2. 向这些对象要求归回存储器。

### 实作

GC的来源可能是由编程语言本身内置（如Java、C#）或是经由外面的库所提供，而非建制于语言内部，例如贝姆垃圾收集器就是一种可支持C/C++语言的自动存储器管理工具。

现今的GC（如Java和.NET）使用分代收集（generation collection），依照对象存活时间的长短使用不同的垃圾收集算法，以达到最好的收集性能。

基础的算法有下面几种方式，参考记数、追踪收集、标记清除、复制收集、堆积压缩、标记压缩。

以Java为例，整个Java堆可以切割成为三个部分:

1. Young：

  Eden：存放新生对象。

  Survivor：存放经过垃圾回收没有被清除的对象。

  semi-Spaces：和Survivor做Copying collection。

2. Tenured：对象多次回收没有被清除，则移到该区块。

3. Perm:存放加载的类型还有方法对象。

Java不同的世代使用不同的GC算法。

1. Minor collection：

  YOUNG世代使用将Eden还有Survivor内的数据利用semi-space做复制收集（Copying collection），

  并将原本Survivor内经过多次垃圾收集仍然存活的对象移动到Tenured。

2. Major collection则会进行Minor collection，Tenured世代则进行标记压缩收集。
