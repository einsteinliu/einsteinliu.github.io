---
layout: post
title: Effective Modern C++:Template和auto的类型推断
description: "Some tricks for modern C++"
tags: [C++]
categories: [Tech, Program]

---

Template 和 auto 的类型推断

**template类型推断会忽略一层引用和指针类型：**

示例一：

传入参数按引用传入

```c++
template<typename T>
void f(T& param); 

int x = 27; // x is an int
const int cx = x; // cx is a const int
const int& rx = x; // rx is a reference to x as a const int 
f(x);// T is int
f(cx);// T is const int, param's typeis const int&
f(rx);// T is const int, param's type is const int& 
```



<!-- more -->

示例二：

传入参数按指针传入

```c++
template<typename T>
void f(T* param); // param isnow a pointer

int x = 27; // as before
const int *px = &x; // px is a ptr to x as a const int 
f(&x); // T is int, param's typeis int*
f(px); // T is const int, param's type is const int* 
```

示例三：

传入参数按双层引用传入，则减掉一层

```c++
template<typename T>
void f(T&& param); // param isnow a universal reference 

int x = 27; // as before
const int cx = x; // as before
const int& rx = x; // as before
f(x); // x is lvalue, so T is int&, param's type is also int&
f(cx); // cx is lvalue, so T is const int&, param's type is also const int&
f(rx); // rx is lvalue, so T is const int&, param's type is also const int&
f(27); // 27 is rvalue, so T is int, param's type istherefore int&& 
```

示例四：

传入参数按值传入，cont和&通通减去

```c++
template<typename T>
void f(T param); // param is now passed by value 

int x = 27; // as before
const int cx = x; // as before
const int& rx = x; // as before
f(x); // T's and param's types are both int
f(cx); // T's and param's types are again both int
f(rx); // T's and param's types are stillboth int 
```

斜杠后面的类型是T的类型/斜杠前面的类型是param的类型 ![type_deduction]({{ site.url }}\images\cpp\type_deduction.png)

