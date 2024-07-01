---
title: Java基础回顾
date: 2019-07-20T13:08:35.000Z
updated: '2024-07-01 20:34:46'
categories:
  - Java
tags:
  - Java
---

## 数据类型
1，四类八种
基本单位 byte = 字节
1字节= 8 位，1位 = 1 bites
整数：byte short int long
浮点：float double
字符：char【16位Unicode字符】 【min：\u0000=0,max：\uffff=65535】
布尔：

## 基础语法

#### 1，运算符

   1. + - * / %
   2. ++i ，i++
   3. < >  == >= <= !=
   4. &&    ||    !     &    |    ^
   5. 位运算符：& | ~ ^
   6. 移位运算： >>  << 
   7. 三元运算： 【条件】？式1：式2

#### 2，控制流程

   1. if {} else if {}
   2. switch (变量) case value ： break;.... default ： break；
   3. while （条件）{} do{..}while{}
   4. for（）{}，for(Object ot ：list){ ...}
   5. break，continue，return

## 面向对象

#### 1，类

   1. 属性
      1. 私有
      2. 公有
      3. 常量
      4. 变量
   2. 方法
      1. 构造
      2. 功能
      3. 方法名，参数，（返回值）方法体
   3. 子类父类
   4. 接口
   5. 抽象类
   6. 关键字：static，final
   7. 内部类
   8. 匿名类

#### 2，对象创建

   1. new 

#### 3，方法

   1. 子类父类
      1. 重写
      2. @Override
   2. 重载
      1. 方法名同
      2. 参数不同

#### 4，初始化

   1. 类
      1. 构造函数
   2. 属性
      1. 未指定时默认值
      2. 指定初始值
   3. 顺序
      1. 静态属性
      2. 静态方法块
      3. 普通属性
      4. 普通方法块
      5. 构造函数
      6. 普通方法

#### 5，对象销毁

   1. 理解java虚拟机
   2. 对象作用域 以 { } 决定
   3. this：当前对象本身[调方法时必须放第一位]
   4. super：父类引用

#### 6，多态

   1. 继承 （基础）
   2. 重写 （基础）
   3. 父类引用指向子类对象（表现）

#### 7，组合

   1. 单例模式
   2. 松耦合
   3. A 类中通过创建B类对象，调用B类功能

#### 8，代理

   1. A 类中通过创建B类对象的代理，调用B类功能（类似套娃）

## 概念

#### 1，接口

   1. 约定和标准
   2. interface

#### 2，抽象类

#### 3，异常

   1. Exception
   2. Throwable
   3. Error
   4. 常见RuntimeException
   5. UncheckedException
   6. throw 方法体内
   7. throws 方法名后

## 数据结构

#### 1，集合

   1. 遍历
      1. Iterable
   2. ArrayList
      1. 可扩容动态数组（扩容时容量增加一半）
      2. 线程不安全
      3. 线程安全：Collections.synchronizedList(new ArrayList())
      4. fail-fast机制 -ConcurrentModificationException （遍历时修改了内容报错）
   3. Vector
      1. 可扩容动态数组（扩容时容量增加一倍）
      2. 线程安全
   4. LinkedList
      1. 双向链表
      2. 线程不安全
      3. 线程安全：Collections.synchronizedList(new LinkedList())
   5. stack
      1. 先进后出
      2. 继承Vector，不常用
      3. push（）pop（）peek（）
   6. HashSet
      1. 无序不重复（是HashMap的键）
      2. 线程不安全
      3. Collection.synchronizedSet()
      4. fail-fast
   7. TreeSet
      1. add(),remove(),contains()
      2. 线程不安全
      3. Collection.synchronizedSet()
      4. fail-fast
   8. LinkedHashSet
      1. 继承HashSet
      2. 散列表+双向链表（数组+链表）
      3. 线程不安全
      4. Collection.synchronizedSet()
      5. fail-fast
   9. queue
      1. PriorityQueue
         1. 不允许null
         2. 自然排序
         3. 线程不安全
         4. PriorityBlockingQueue
   10. HashMap
      1. key-value 都可为空
      2. 线程不安全
      3. fail-fast
      4. ConCurrentHashMap
      5. Collection.synchronizedMap()
   11. TreeMap
      1. key自然排序
      2. comparator定制排序
      3. 线程不安全
      4. fail-fast
      5. Collection.synchronizedMap()
   12. LinkedHashMap
      1. 哈希表+链表
      2. 线程不安全
      3. fail-fast
      4. Collection.synchronizedMap()
   13. HashTable
      1. 线程安全
      2. fail-fast
   14. IdentityHashMap
   15. WeakHashMap

#### 2，Collections

## 泛型

   1. 参数化的集合，限制了添加集合的类型
   2. 多态也是泛型的机制
   3. 希望对象或方法具有最广泛的表达能力
   4. 可用于类，接口，抽象类，方法
   5. 通配符
      1. 上界通配符<? extends ClassType>
      2. 下界通配符<? superClassType>

## 反射
1，包

   1. Java.lang.reflect

2，功能

   1. 可在运行时，判断任意一个对象所属类，任意一个类所有成员变量和方法
   2. 可在运行时，构造任意一个类的对象，调用任意一个对象的方法
   3. 对象
      1. getFields() 获取对象的所有公有属性
      2. getDeclaredFields()  获取对象的所有属性不包含继承
      3. getMethods();
      4. getDeclaredMethods();
      5. getConstructors();
      6. getDeclaredConstructors();
      7. .getClass().getName()返回全限定名称
   4. 类
      1. Class c = Class.forName("全限定名称")
      2. c.newInstance()
      3. 类名.class.toGenericString()
   5. Class
      1. newinstance（）
      2. getClassLoader（）
      3. getTypeParameter（）
      4. ...
   6. Field
      1. get（Object oj）
      2. set（Object oj，Object，value）
   7. Method
      1. invoke（）
   8. ClassLoader
      1. 类加载器
      2. 把类加载到jvm的
      3. 双亲委派模型进行搜索加载类

## 枚举

## I/O
![](/images/19e38bf17e67a66bb3a128d69aaa514f.jpeg)

![](/images/7d221ce11b4779d02332052b89b7c04d.jpeg)
1，类

   1. File
   2. InputStream
      1. read(byte b[], int off ,int len)
      2. read()
      3. skip()
      4. close()
   3. OutputStream
      1. write()
      2. flush()
      3. close()
   4. Reader
      1. read()
      2. skip()
      3. close()
   5. Writer
      1. write()
      2. flush()
      3. append()
      4. close()

## 注解
元数据
3个在java.lang
7个在java.lang.annotation

#### 1，代码中用

   1. @Override
   2. @Deprecated
   3. @SuppressWarnings

#### 2，元注解

   1. @Retention
   2. @Documented
   3. @Target
   4. @Inherited

#### 3，1.7增3个

   5. @SafeVarargs
   6. @FunctionalInterface
   7. @Repeatable

#### 4，后续新增

   8. 


## JUC

### 1，线程池
![](/images/8cbb3c37affde360be26ad57482d2976.jpeg)
