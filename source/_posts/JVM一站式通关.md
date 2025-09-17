---
categories: 整理输出
tags:
  - jvm
sticky: ''
description: ''
permalink: ''
title: JVM一站式通关
date: '2025-09-03 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/037bc1b7-d838-4ee6-9c51-f60489686735/%E3%80%90%E5%93%B2%E9%A3%8E%E5%A3%81%E7%BA%B8%E3%80%91%E4%BA%8C%E6%AC%A1%E5%85%83-%E5%8A%A8%E6%BC%AB.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SH7BQ564%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T070049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJHMEUCIFbk2J5%2B0fvu5VaxgMZW1giGPNARWdKvLUT4AxYJx4qOAiEAn9I5oCPWTcbmXZNuuQgrlUmb8gwHQ4%2B2kxef6cgE9rkqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOuF%2Bj1EvTA%2BcLcYKSrcA31jr9pS5uDqlnwN0uX2ciocbbjB3sDNRTydw0LYCDMGXy88tn7P8rksekFgGwH3CWH2TktFzFubdQOGnQeVhPL5LFq%2FUbeqXhRCgtrtSVX43Hvd8VAF%2BwAOWXw2QHZXJ9PaKxCcjI8F9HGk28RlBhbV7Pf78s61QENzYslMIBBtSYq0RCpiXPRlTadDInWpcKklZKwxOs74AnDry1aWrL0msMvhT44mh98JBSwef%2FmGQXltAOpsNQTiQ6pD8laZKoUxaPHgEeB01NXxzWLHs0sVZM5ljOsriV1BZD3whWB7hNm1rLHn5f8PmAIEGbbl6YkdYCH9cjJp34zWPwZZX8nDVpeEJ4vc%2FoAod3iSyRFI1asmDQeTIeHLqSAcR2lAeSK1OOEWP7Bk2%2B2dS0eSlK1FSGL7%2FXKGZ9ZCFoBm9BB79WVss5uiMyqage2ECPXZX7Bfv5BQAn4Sn27Px0K9LE1t23i4wA7DSTPdfORjHgFEWl0%2FnAQNmnKd9BRl5a8rp8yqiFV3JFRxjZyLuJxpnRI5nQQG8UrDaFzeObFeBAya3fiy3b5DXsCkogMv%2BVCm1i8Hii0iKLTW5G8bSxX1y7Me9zytHz86G%2Bb2y0cPuv4ucgj%2BlCHCPbw8V35QMLikqcYGOqUB7Tc9yp4LfCR%2FOHxM4o2i5cQ5AdJ6QEOEHe86ylCHD9dGjpB0hCoihSPcZe55F0OptK5m1CzeodHSoq%2FBdUHSp%2F7JBpt9GfuhvTHNRLtigY0WM5zdV9HSsN4OC%2FxtZg%2Fya%2BBVlUr1LMcqvgZBGmS3sZz1nJOyjXEp4lYbHnpVumOFX8fSLNouAppIMKzxkoI%2Bwg%2FxFjzHHa408F8RgjJNqvq%2FKt7u&X-Amz-Signature=838420d191e7177e4468f7f7360fb84b9e06c112a171d74008afbca9a8379460&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/1f26c2a33d77896146f171a3c45f7c4e.png
banner_img: /images/1f26c2a33d77896146f171a3c45f7c4e.png
---

# 垃圾收集器


主要分为两类：

- 分代回收器
    - CMS
- 分区回收器
    - G1
    - ZGC

![images2c1751ad14c6855e72003abf68ba4767.png](/images/697af6f27197058a12edfdb84c8a5c04.png)


## CMS


本质就是对【可达性分析】的改进，即三色标记法[三色标记法](https://www.yuque.com/fanglikuai/blog/hxxaxf4e3d6f4gm6)


以获取最短回收停顿时间为目标，采用`标记-清除`算法。 jdk14被移除


cms是第一个关注停顿时间（stw）的垃圾收集器


![images657d9ea0229824e0e515af5cec09055e.png](/images/57840813da4d90a17e3da16859c3c101.png)


具体流程：


cms的四个回收步骤比较好理解，主要为四个步骤：


1初始标记：这个过程十分快速，需要stop the world，遍历所有的对象并且标记初始的gc root


2并发标记：这个过程可以和用户线程一起并发完成，所以对于系统进程的影响较小，主要的工作为在系统线程运行的时候通过gc root对于对象进行根节点枚举的操作，标记对象是否存活，注意这里的标记也是较为迅速和简单的，因为下一步还需要重新标记


3重新标记：需要stop the world，这个阶段会继续完成上一个阶段的动作，对于上一个步骤标记的对象进行二次遍历，重新标记是否存活。


4并发清理：和用户线程一起并发，负责将没有Gc root引用的垃圾对象进行回收。


从上面的步骤描述可以看到，cms的垃圾收集器已经有了很大的进步，可以实现并发的标记和并发的整理阶段做到和用户线程并发执行（但是比较吃系统资源），不干扰用户线程的对象分配操作，但是需要注意初始标 记和重新标记阶段依然需要停顿。


### 


初始标记


初始标记阶段：需要暂停用户线程， 开启垃圾收集线程， 但是仅仅是收集当前老年代的GC ROOT对象，整个运行过程的速度非常快，用户几乎感知不到。


这里需要注意的是哪些对象会作为GC ROOT，而哪些则不会，比如实例变量不是GC ROOT的对象，同时在根节点枚举当中如果发现没有被引用也会标记为垃圾对象。


![images13812b2fc149e733f4dcad86b77fd6ca.webp](/images/502f96c314f2798d2143522d8a97a2e8.webp)


哪些节点可以作为gc root


●**局部变量本身就可以作为GC ROOT
●静态变量可以看作是Gc Root
●Long类型index的遍历循环会作为GT ROOT**
总结：当有方法局部变量引用或者类的静态变量引用，就不会被垃圾线程回收。


### 并发标记


并发标记阶段：可以和用户线程一起并发执行，此时系统进程会不断往虚拟机中分配对象，而垃圾收集线程则会根据gc root对于老年代中的对象进行有效性检测，将对象标记为存活对象或者垃圾对象，这个阶段是最为耗时的，但是由于是和用户线程并发执行，影响不是很大。


注意这个这个阶段并不能完成标记出需要垃圾回收的对象，因为此时可能存在存活对象变为垃圾对象，而垃圾对象也可能变为存活对象。


![imagese7b9cdab86eff2c4a59f3e2991c0360a.webp](/images/ba29192928a98c1234fc39c4a98bd9a5.webp)


补充 - 并发关系和并行关系在jvm的区别：


并行：指的是多条垃圾收集线程之间的关系


并发：垃圾收集器和用户线程之间的关系


### 重新标记


重新标记阶段：这个阶段同样需要stop world，作用是会继续完成上一个阶段的动作，其实是对第二个阶段已经标记的对象再次进行对象是否存活的标记和判断，这个过程是十分快的，因为是对上一个步骤的扫尾工作。


![images526595ee2ba8b083f866e18af0be5000.webp](/images/562931b2c2b97166eded507d061e8120.webp)


### 


并发清理


并发清理阶段：这个阶段同样是和用户线程并发执行的，此时用户线程可以继续分配对象，而垃圾回收线程则进行垃圾的回收动作，这个阶段也是比较耗时的，但是由于是并发执行所以影响不是很大。


![images3b334d4ba3bfff518d59725f4936e707.webp](/images/e7c9bf1758d8cd6cbad4c430bcd3aeaf.webp)


### 优缺点


CMS 的优点是：并发收集、低停顿。但缺点也很明显：


①、对 CPU 资源非常敏感，因此在 CPU 资源紧张的情况下，CMS 的性能会大打折扣。


默认情况下，CMS 启用的垃圾回收线程数是`（CPU数量 + 3)/4`，当 CPU 数量很大时，启用的垃圾回收线程数占比就越小。但如果 CPU 数量很小，例如只有 2 个 CPU，垃圾回收线程占用就达到了 50%，这极大地降低系统的吞吐量，无法接受。


②、CMS 采用的是「**标记-清除**」算法，会产生大量的内存碎片，导致空间不连续，当出现大对象无法找到连续的内存空间时，就会触发一次 Full GC，这会导致系统的停顿时间变长。


③、CMS 无法处理浮动垃圾，当 CMS 在进行垃圾回收的时候，应用程序还在不断地产生垃圾，这些垃圾会在 CMS 垃圾回收结束之后产生，这些垃圾就是浮动垃圾，CMS 无法处理这些浮动垃圾，只能在下一次 GC 时清理掉。（**其实问题不大**）


## G1


在jdk9成为默认的收集器。G1有五个属性：分代，增量，并行，标记征集，stw。


①分代


G1将堆内存分为多个大小相等的区域（Region），每个区域都可以是 Eden 区、Survivor 区或者 Old 区。


![images78831b5b65ce33dbac10179634ff4e49.png](/images/28543177878e5d7dfbef856fc491472e.png)


G1 有专门分配大对象的 Region 叫 Humongous 区，而不是让大对象直接进入老年代的 Region 中。在 G1 中，大对象的判定规则就是一个大对象超过了一个 Region 大小的 50%，比如每个 Region 是 2M，只要一个对象超过了 1M，就会被放入 Humongous 中，而且一个大对象如果太大，可能会横跨多个 Region 来存放。


G1 会根据各个区域的垃圾回收情况来决定下一次垃圾回收的区域，这样就避免了对整个堆内存进行垃圾回收，从而降低了垃圾回收的时间。


②增量：可以用增量式的方式执行垃圾回收，不需要一次性回收整个堆空间，有利于控制停顿时间。


③并行：多个cpu


④标记整理：进行老年代的整理时使用的算法


**年轻代使用的是复制算法， 因为年轻代的对象通常是朝生夕死的。**

> GC的三种收集方法：标记清除、标记整理、复制算法的原理与特点，分别用在什么地方，优化收集方法的思路_标记清除 标记整理-CSDN博客

⑤、STW：G1 也是基于「标记-清除」算法，因此在进行垃圾回收的时候，仍然需要「Stop the World」。不过，G1 在停顿时间上添加了预测机制，用户可以指定期望停顿时间


## ZGC


ZGC（The Z Garbage Collector）是 JDK11 推出的一款低延迟垃圾收集器，适用于大内存低延迟服务的内存管理和回收，SPEC jbb 2015 基准测试，在 128G 的大堆下，最大停顿时间才 1.68 ms，停顿时间远胜于 G1 和 CMS。


前面讲 G1 垃圾收集器的时候提到过，Young GC 和 Mixed GC 均采用的是[复制算法](https://javabetter.cn/jvm/gc.html)，复制算法主要包括以下 3 个阶段：


①、标记阶段，从 GC Roots 开始，分析对象可达性，标记出活跃对象。


②、对象转移阶段，把活跃对象复制到新的内存地址上。


③、重定位阶段，因为转移导致对象地址发生了变化，在重定位阶段，所有指向对象旧地址的引用都要调整到对象新的地址上。


标记阶段因为只标记 GC Roots，耗时较短。但转移阶段和重定位阶段需要处理所有存活的对象，耗时较长，并且转移阶段是 STW 的，因此，G1 的性能瓶颈就主要卡在转移阶段。


与 G1 和 CMS 类似，ZGC 也采用了复制算法，只不过做了重大优化，ZGC 在标记、转移和重定位阶段几乎都是并发的，这是 ZGC 实现停顿时间小于 10ms 的关键所在。


ZGC 是怎么做到的呢？

- **指针染色（Colored Pointer）：一种用于标记对象状态的技术。**
    - **通过在指针中嵌入这些信息，ZGC 在标记和转移阶段会更快，因为通过指针上的颜色就能区分出对象状态，不用额外做内存访问。**
- **读屏障（Load Barrier）：一种在程序运行时插入到对象访问操作中的特殊检查，用于确保对象访问的正确性。**
