---
title: C基本格式
date: 2020-09-10T10:10:57.000Z
updated: '2024-07-01 20:53:33'
categories:
  - C/C++
tags:
  - 做题
---

## 必写基础格式
```cpp
#include <stdio.h>

int main(){
    //单行注释、
    
    /*
    *多行
    */
    
    return 0;
}
```

## 输入输出
```cpp
#include <stdio.h>

int main(){
    type element;
    printf("content \n"); //换行符
    printf("%d",element); // 整数
    printf("%c",element); // 字符
    printf("%f",element); // 浮点
    
    return 0;
}
```

## 数据类型
```cpp
1.整数类型（有符号 无符号）暂且没搞懂无符号，默认有符号
位数记得！！！！
char int short long
2.浮点类型
float double
3.变量
char ch ='a';
4.常量
#define WIDTH 5（无分号）
const int a = 10;
5.存储类
auto 局部变量不写默认
register 不常用
static 函数调用期间值不变 和java一样
extern 此变量/函数是在别处定义的，要在此处引用

```

## 自定义数据结构（结构体）
```cpp
1. 指针返回
2. 引用返回

--可做左值

```

## 运算符
```cpp
1.运算符
&
0&0=0;   
0&1=0;    
1&0=0;     
1&1=1;
|
0|0=0;   
0|1=1;   
1|0=1;    
1|1=1;
^
0^0=0;   
0^1=1;   
1^0=1;  
1^1=0;
~
~1=-2;   
~0=-1;
<< 左移右补0
>> 右移左补0
```

## 总结

- 总的来说，对每一个语言，基础都是，变量，条件，循环，字符串，数组，函数，【结构体】
