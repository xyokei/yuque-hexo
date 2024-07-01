---
title: 一些Java问题
date: 2019-12-15T16:10:20.000Z
updated: '2024-07-01 20:31:53'
categories:
  - Java
tags:
  - Java
---

## java创建对象方式

   1. new 创建一个对象，

new的流程，jvm首先会找这个类是否被加载
声明一个变量存储new 出来对象地址，这个地址指向所引用的对象，声明类型引用
堆内存分配，jvm进行对象头的设置，包括Hash码，锁状态标志，锁指针，偏向线程ID
偏向时间戳，以及类型指针
![](/images/782bbd309e31265edc82f5ab91e96515.jpeg)
可以通过反射创建一个对象
通过克隆复制一个对象，深拷贝和浅拷贝
反序列化出一个对象

## synchronized的底层实现
|           Mark Word (32 bits)  |  |  |  |  | State |
| --- | --- | --- | --- | --- | --- |
| identity_hashcode:25 |  | age:4  | biased_lock:1  |  lock:2  | Normal |
| thread:23 | epoch:2 | age:4 | biased_lock:1  |  lock:2  | Biased |
| ptr_to_lock_record:30 |  |  |  |  lock:2  | Lightweight Locked |
| ptr_to_heavyweight_monitor:30 |  |  |  |  lock:2  | Heavyweight Locked |
|  |  |  |  |  lock:2  | Marked for GC |

**_biased_lock_**：对象是否启用偏向锁标记，只占1个二进制位。为1时表示对象启用偏向锁，为0时表示对象没有偏向锁。
**_age_**：4位的Java对象年龄。在GC中，如果对象在Survivor区复制一次，年龄增加1。当对象达到设定的阈值时，将会晋升到老年代。默认情况下，并行GC的年龄阈值为15，并发GC的年龄阈值为6。由于age只有4位，所以最大值为15，这就是-XX:MaxTenuringThreshold选项最大值为15的原因。
**_identity_hashcode_**：25位的对象标识Hash码，采用延迟加载技术。调用方法System.identityHashCode()计算，并会将结果写到该对象头中。当对象被锁定时，该值会移动到管程Monitor中。
**_thread_**：持有偏向锁的线程ID。
**_epoch_**：偏向时间戳。
**_ptr_to_lock_record_**：指向栈中锁记录的指针。
**_ptr_to_heavyweight_monitor_**：指向管程Monitor的指针。

## 序列化与反序列化
序列化：将java对象转换成字节码
反过来：将字节码转换成原先的java对象

用途，可以可持久化的保存java对象，
Serializable是个空接口，只是说明该类可以进行序列化，当个标记用
真正的序列化操作在输出文件的时候判断是否实现了该接口
SerialVersionUID
在反序列化时，用来做对比
static 和transient修饰符修饰的字段不会被序列化
序列化保存的是对象的状态，而不是类的

## 为什么面向接口编程
横向对比 ：PO OO
接口：协议标准，高度的抽象，规定一些将要实现的行为功能或者一种标记，
优点：可以实现代码灵活解耦
为什么接口可以继承呢
从接口组成的继承层级中，从上往下看的话，就是抽象到具体的过程（即减少其包含类的种类的同时给了这些类更为清晰的定义），层级分明，上一层是下一层共性的抽象，下层是对上层不同维度的演进
Iterable，Collection set/List/Queue
例子：插座

# 

