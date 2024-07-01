---
title: C++常用写法
date: 2022-08-20T18:12:25.000Z
updated: '2024-07-01 20:57:45'
categories:
  - C/C++
tags:
  - 做题
---

## 头文件（.h）与源文件（.cpp）
一般.h文件文件写
类的声明（包括类里面的成员和方法的声明）、
函数原型、#define常数等，但一般来说不写出具体的实现。
```cpp
#ifndef FILENAME_H

#define FILENAME_H
//代码写在这里
class ClassName
...{
private:
     int temp;
public:
     ClassName();
     ClassName(int temp);;
     int func();
};
#endif
```
源文件主要写实现头文件中
已经声明的那些函数的具体代码。
需要注意的是，开头必须#include一下实现的头文件，以及要用到的头文件。
那么当你需要用到自己写的头文件中的类时，只需要#include进来就行了
```cpp
#include <iostream>


```

## 指针与数组

- 变量 引用 指针 
- 常量引用 常量指针 
- 引用常量，指针常量
```cpp
1.数组名即是指针 且是指针常量，指向数组首地址
2.arr 和 arr[0] ？
```

## 引用
```cpp
//a 是原名， b是引用a的别名， b 和 a指向相同内存地址
//引用即变量别名
//1. 引用必须初始化，且初始化后不可更改
//2. 引用于指针常量？
int &b =a  //左边是引用 不能建立数组的引用
int *p =&a //在右边是地址
//此时是赋值操作 b 和 a 指向不同的地址，但值相同
int b = a

//3. 
//    值传递：
    int test（int a）{}
//    地址传递
    int test（int *a）{}
//    引用传递
    int test（int &a）{}
//变量 引用 指针 常量引用 常量指针 
//指针 p 值 就是地址
//*p 取地址的值

```

## 函数返回值
```cpp
1. 指针返回
2. 引用返回
3. 数组不可作返回值
--可做左值

```

## 内联函数与宏
```cpp
目的：便于组织，方便重复利用，（感觉就是代码的复用）

使用场景：
宏，一般常量定义，也可以是有参宏，或无参宏
#define MaxSize
#define add(x,y) (x+y)
```
**详细解释**
使用带参数的宏定义可完成函数调用的功能，又能减少系统开销，提高运行效率。
正如C语言中所讲，函数的使用可以使程序更加模块化，便于组织，而且可重复利用，
但在发生函数调用时，需要保留调用函数的现场，以便子函数执行结束后能返回继续执行，
同样在子函数执行完后要恢复调用函数的现场，这都需要一定的时间，如果子函数执行的操作比较多，
这种转换时间开销可以忽略，但如果子函数完成的功能比较少，甚至于只完成一点操作，
如一个乘法语句的操作，则这部分转换开销就相对较大了，但使用带参数的宏定义就不会出现这个问题，
因为它是在预处理阶段即进行了宏展开，在执行时不需要转换，即在当地执行。
宏定义可完成简单的操作，但复杂的操作还是要由函数调用来完成，
而且宏定义所占用的目标代码空间相对较大。所以在使用时要依据具体情况来决定是否使用宏定义。
内联函数，几乎可以这样认为：内联函数就是带了参数静态类型检查的宏。

## 浅拷贝与深拷贝

- 默认拷贝函数 浅拷贝 深拷贝需重写
- 浅拷贝构造函数
   1. 成员一一复制
   2. 若类中无 指针成员 拷贝对象值修改 不影响 原有值
   3. 有 指针成员 由下两个原因 此时须用深拷贝
      1. 拷贝对象值修改 影响 原有值
      2. 对象销毁时，调两次 析构函数，导致 指针悬挂 
- 深拷贝构造函数（需重写）
> 以下测试不用看了，看上面总结就够了，实际使用时 只需 判断自己是需要怎样的拷贝就用哪个就行
> 基本有指针的，或者有引用类型的都需要重写，只有基本类型的就不用了
> 引用类型的变量名就是指针 ，相当于数组名

```cpp
#include <iostream>
#include "Student.h"

using namespace std;

int main()
{
    Student stu1;
    Course cour;
    cour.setCh('a')
    stu1.setAge(100); //基本类型测试
    stu1.setSize(3);  //数组类型测 
    stu1.setName("test");  //字符串测试
    char gr[5] ="abc";
    stu1.setGrade(gr);  
    stu1.setCourse(cour);  
    cout<<stu1.getAge()<<endl;
    cout<<stu1.getSize()<<endl;
    cout<<stu1.getName()<<endl;
    cout<<stu1.getGrade()<<endl;
    cout<<stu1.getCourse().getCh<<endl;
    
    //测试浅copy
    Student stu2(stu1);
    stu2.setAge(200);
    stu2.setSize(5);
    stu2.setName("nai"); //测试 方法错了，不能用set改指针，数组，字符串
    char grtt[5] ="ttt";
    stu2.setGrade(grtt);
    cour.setCh('b');
    cout<<stu1.getAge()<<endl;
    cout<<stu2.getAge()<<endl;
    cout<<stu1.getSize()<<endl;
    cout<<stu2.getSize()<<endl;
    cout<<stu1.getName()<<endl;
    cout<<stu2.getName()<<endl;
    cout<<stu1.getGrade()<<endl;
    cout<<stu2.getGrade()<<endl;
    cout<<stu1.getCourse().getCh<<endl;
    cout<<stu2.getCourse().getCh<<endl;
}
```
```cpp
#include <iostream>
#include <cstring>
#include "Course.h"

using namespace std;
class Student{
    private:
        int age;
        int course[3];
        string name;
        char* grade;
        Course cour;
    public:
        void setAge(int age);
        int getAge();
        
        void setSize(int size);
        int getSize();
        
        void setName(string arg);
        string getName();
        
        void setGrade(char* grade);
        char* getGrade();
        
        void setCourse(char* grade);
        Course getCourse();
};
```
```cpp
#include "Student.h"

void Student::setAge(int age){
    this->age = age;
}

int Student::getAge(){
    return this->age;
}

void Student::setSize(int size){
    this->course[0] = size;
}

int Student::getSize(){
    return this->course[0];
}

void Student::setName(string arg){
    this->name = arg;
}

string Student::getName(){
    return this->name;
}

void Student::setGrade(char* grade){
    this->grade = grade;
}

char* Student::getGrade(){
    return this->grade;
}

void Student::setCourse(Course cou){
    this->cour = cou;
}

Course Student::getCourse(){
    return this->cour;
}

```
```cpp
class{
    private:
        char ch;
    public:
        void setCh(char c){
            this->ch = c;
        }
        char getCh(){
            return ch;
        }
}
```

## C中怎样做到实现字符串的

- 字符数组与字符串的区别
```cpp
#include <stdio.h>

int main(){
    char str[] = "this is string"; //c系统内部会自动填充长度 此时长度
    char str[3]={'a','b','c'}
    char strr[10]; strr = "this is"; //错误，初始化必须何赋值在一起
    

}
```

## 实现文件的读写（io）
三个类：
ofstream  ：文件写操作 写到磁盘
ifstream ：文件读操作 读到内存
fstream ：读写操作

### open函数
`open("filename",mode,prot)`

| mode | 描述 |
| --- | --- |
| ios::in | 打开文件 |
| ios::out | 输出文件 |
| ios::ate |  |
| ios::app | 输出追加到文件 |
| ios::trunc | 若文件已存在先删除文件 |
| ios::binary | 二进制 |

> mode可组合使用

`open("test.txt",ios::in|ios::out)`

## 多线程的实现
