---
title: "结合 C 和 C++ 代码"
authors: [kiki]
tags: [c++]
categories: [blog]
draft: false
---

- [需要了解的知识](#%e9%9c%80%e8%a6%81%e4%ba%86%e8%a7%a3%e7%9a%84%e7%9f%a5%e8%af%86)
- [在 C++ 中调用 C 语言函数](#%e5%9c%a8-c-%e4%b8%ad%e8%b0%83%e7%94%a8-c-%e8%af%ad%e8%a8%80%e5%87%bd%e6%95%b0)
- [在 C 中调用 C++ 语言函数](#%e5%9c%a8-c-%e4%b8%ad%e8%b0%83%e7%94%a8-c-%e8%af%ad%e8%a8%80%e5%87%bd%e6%95%b0)
  - [在 C 中调用 C++ 非成员函数](#%e5%9c%a8-c-%e4%b8%ad%e8%b0%83%e7%94%a8-c-%e9%9d%9e%e6%88%90%e5%91%98%e5%87%bd%e6%95%b0)
  - [在 C 中调用 C++ 成员函数](#%e5%9c%a8-c-%e4%b8%ad%e8%b0%83%e7%94%a8-c-%e6%88%90%e5%91%98%e5%87%bd%e6%95%b0)
  - [在 C 中调用 C++ 重载函数](#%e5%9c%a8-c-%e4%b8%ad%e8%b0%83%e7%94%a8-c-%e9%87%8d%e8%bd%bd%e5%87%bd%e6%95%b0)
  - [C++ 名称重整](#c-%e5%90%8d%e7%a7%b0%e9%87%8d%e6%95%b4)
  - [C++ 链接器处理 C 符号](#c-%e9%93%be%e6%8e%a5%e5%99%a8%e5%a4%84%e7%90%86-c-%e7%ac%a6%e5%8f%b7)
  - [测试](#%e6%b5%8b%e8%af%95)
- [在 C++ 中包含一个标准的 C 头文件](#%e5%9c%a8-c-%e4%b8%ad%e5%8c%85%e5%90%ab%e4%b8%80%e4%b8%aa%e6%a0%87%e5%87%86%e7%9a%84-c-%e5%a4%b4%e6%96%87%e4%bb%b6)
- [在 C++ 中包含一个非系统的 C 头文件](#%e5%9c%a8-c-%e4%b8%ad%e5%8c%85%e5%90%ab%e4%b8%80%e4%b8%aa%e9%9d%9e%e7%b3%bb%e7%bb%9f%e7%9a%84-c-%e5%a4%b4%e6%96%87%e4%bb%b6)
- [修改自己的 C 头文件以便 C++ 容易使用 include 语句包含](#%e4%bf%ae%e6%94%b9%e8%87%aa%e5%b7%b1%e7%9a%84-c-%e5%a4%b4%e6%96%87%e4%bb%b6%e4%bb%a5%e4%be%bf-c-%e5%ae%b9%e6%98%93%e4%bd%bf%e7%94%a8-include-%e8%af%ad%e5%8f%a5%e5%8c%85%e5%90%ab)
- [在 C++ 中调用非系统的 C 函数 f(int,char,float)](#%e5%9c%a8-c-%e4%b8%ad%e8%b0%83%e7%94%a8%e9%9d%9e%e7%b3%bb%e7%bb%9f%e7%9a%84-c-%e5%87%bd%e6%95%b0-fintcharfloat)
- [创建一个可被 C 调用的 C++ 函数 f(int,char,float)](#%e5%88%9b%e5%bb%ba%e4%b8%80%e4%b8%aa%e5%8f%af%e8%a2%ab-c-%e8%b0%83%e7%94%a8%e7%9a%84-c-%e5%87%bd%e6%95%b0-fintcharfloat)
- [链接器在 C/C++ 调用 C++/C 函数时报错的原因](#%e9%93%be%e6%8e%a5%e5%99%a8%e5%9c%a8-cc-%e8%b0%83%e7%94%a8-cc-%e5%87%bd%e6%95%b0%e6%97%b6%e6%8a%a5%e9%94%99%e7%9a%84%e5%8e%9f%e5%9b%a0)
- [传递一个 C++ 类的对象给/从 C 函数](#%e4%bc%a0%e9%80%92%e4%b8%80%e4%b8%aa-c-%e7%b1%bb%e7%9a%84%e5%af%b9%e8%b1%a1%e7%bb%99%e4%bb%8e-c-%e5%87%bd%e6%95%b0)
- [C 函数是否可以直接访问 C++ 类对象的数据](#c-%e5%87%bd%e6%95%b0%e6%98%af%e5%90%a6%e5%8f%af%e4%bb%a5%e7%9b%b4%e6%8e%a5%e8%ae%bf%e9%97%ae-c-%e7%b1%bb%e5%af%b9%e8%b1%a1%e7%9a%84%e6%95%b0%e6%8d%ae)
- [使用 C++ 比 C 更觉得远离机器代码](#%e4%bd%bf%e7%94%a8-c-%e6%af%94-c-%e6%9b%b4%e8%a7%89%e5%be%97%e8%bf%9c%e7%a6%bb%e6%9c%ba%e5%99%a8%e4%bb%a3%e7%a0%81)
- [参考](#%e5%8f%82%e8%80%83)

## 需要了解的知识

- 有一些关键点(即使一些编译器尝试不要求，检查编译器厂商的文件)
  - 当编译 `main()` 时必须使用 C++ 编译器(比如为了静态初始化)
  - C++ 编译器应该管理链接过程(这样才可以得到指定的库)
  - C 和 C++ 编译器可能需要来自同一厂商，且具有兼容版本(这样才有相同的调用惯例)
- 除此之外，你需要阅读剩余的章节以找到 可被 C/C++ 调用的 C++/C 函数
- 有一个方式可以解决所有的问题：即使用 C++ 编译器编译所有的代码(即使是 C 风格的代码)
  - 优点：可以解决结合 C 和 C++ 代码的问题，也更容易发现 C 代码的错误
  - 缺点：需要更新 C 风格的代码，[原因](https://isocpp.org/wiki/faq/big-picture#back-compat-with-c)
  - 但是更细代码的代价可能比结合二者的代价更小，同时可以清除 C 风格的代码

## 在 C++ 中调用 C 语言函数

- 在 C++ 中用 `extern "C"` 声明 C 函数，然后在 C/C++ 中调用

```c++
extern "C" void f(int);//方式1
extern "C" {//方式2
  int g(double);
  double h();
};

void cppode(int i, double d)
{
  f(i);
  int ii = g(d);
  double dd = h();
  //...
};
```

```c
//c code
void f(int i)
{
  //...
}
int g(double d)
{
  //...
}
double h() {
  //...
}
```

## 在 C 中调用 C++ 语言函数

### 在 C 中调用 C++ 非成员函数

- 在 C++ 中用 `extern "C"` 声明 C++ 函数，然后在 C/C++ 中调用

```c++
extern "C" void f(int);

void f(int i)
{
  //...
}
```

```c
void f(int);

void ccode(int i)
{
  f(i);
  //...
}
```

### 在 C 中调用 C++ 成员函数

- 如果需要在 C 中调用成员函数(包括虚函数)，需要提供一个简单的封装

```c++
class C {
  //...
  virtual double f(int);
};

//封装函数
extern "C" double call_C_f(C* p, int i)
{
  return p->f(i);
}
```

```c
struct C
{
  //...
};
double call_C_f(struct C*, int);

void ccode(struct C* p, int i)
{
  double d = call_C_f(p, i);
  //...
}
```

### 在 C 中调用 C++ 重载函数

- 如果需要在 C 中调用重载函数，必须为 C 语言提供不同名称的封装函数

```c++
void f(int);
void f(double)

extern "C" void f_i(int);
extern "C" void f_d(double);
```

```c
void f_i(int);
void f_d(double);

void ccode(int i, double d)
{
  f_i(i);
  f_d(d);
  //...
}
```

- 这种方式适用于在 C 中调用 C++ 库，即使不能修改 C++ 的头文件

### C++ 名称重整

- C++ 支持函数重载。比如可以有多个函数名称相同，但参数不同。当生成目标代码时，C++ 编译器通过添加参数信息修改名称来区分不同的函数。这个添加额外信息到函数名的技术叫做名称重整(name mangling)
- C++ 标准没有对名称重整指定任何详细的技术，因此不同编译器可能添加不同信息到函数名

```c++
int f(void) {return 1;}
int f(int) {return 0;}
void g(void) (int i=f(), j=f(0);)
```

- 上述代码可能被 C++ 编译器改成

```c++
int __f_v(void) {return 1;}
int __f_i(int) {return 0;}
void __g_v(void) (int i=__f_v(), j=__f_i(0);)
```

### C++ 链接器处理 C 符号

- C 不支持函数重载，名称不会被重整。当把代码放到 `extern "C"` 块时，C++ 编译器确保函数名不会被重整，即编译器生成一个二进制文件，且没有修改函数名
- `extern "C"` 在 C++ 调用 C 时：告诉 g++ 预期得到 gcc 生成的未重整的符号
- `extern "C"` 在 C 调用 C++ 时：告诉 g++ 生成未重整的符号给 gcc 使用

### 测试

- 反编译一个 g++ 生成的二进制代码

```c++
void f() {}
void g();

extern "C" {
  void ef() {}
  void eg();
}

void h() { g(); eg();}
```

- 使用 g++ 编译 `g++ -c main.cpp`
- 反编译符号表 `readelf -s main.o`

```txt
Symbol table '.symtab' contains 13 entries:
   Num:    Value          Size Type    Bind   Vis      Ndx Name
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND
     1: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS cppcode.cpp
     2: 0000000000000000     0 SECTION LOCAL  DEFAULT    1
     3: 0000000000000000     0 SECTION LOCAL  DEFAULT    3
     4: 0000000000000000     0 SECTION LOCAL  DEFAULT    4
     5: 0000000000000000     0 SECTION LOCAL  DEFAULT    6
     6: 0000000000000000     0 SECTION LOCAL  DEFAULT    7
     7: 0000000000000000     0 SECTION LOCAL  DEFAULT    5
     8: 0000000000000000     7 FUNC    GLOBAL DEFAULT    1 _Z1fv
     9: 0000000000000007     7 FUNC    GLOBAL DEFAULT    1 ef
    10: 000000000000000e    17 FUNC    GLOBAL DEFAULT    1 _Z1hv
    11: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND _Z1gv
    12: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND eg
```

- 可以看到：`ef` 和 `eg` 在符号表存储的名字和代码中的名字相同；其他的名称被重整过
- 解开(unmangle)这些名字

```sh
kiki@ubuntu:/tmp/test$ c++filt _Z1fv
f()
kiki@ubuntu:/tmp/test$ c++filt _Z1hv
h()
kiki@ubuntu:/tmp/test$ c++filt _Z1gv
g()
```

## 在 C++ 中包含一个标准的 C 头文件

- 直接使用 `#include` 包含所需头文件，比如 `#include <cstdio>`

```c++
#include <cstdio>

int main()
{
  printf("Hello!\n");// or std::printf
  return 0;
}
```

- 如果使用 C++ 编译器编译 C 代码，又不希望将类似 `printf` 改成 `std::printf`，可以在 C 代码中使用旧风格的头文件 `stdio.h` 替换新风格的头文件 `cstdio`

```c
#include <stdio.h>

int main()
{
  printf("Hello!\n");
  return 0;
}
```

## 在 C++ 中包含一个非系统的 C 头文件

- 如果在 C++ 中包含一个非系统提供的 C 头文件，需要使用 `extern "C" {/**/}` 结构包裹 `#include` 语句，这个告诉 C++ 编译器此头文件声明的函数是 C 函数

```c++
extern "C" {
  //get declaration for void sum(int, int)
  #include "my-c-code.h"
}

int main()
{
  sum(1, 2);
  return 0;
}
```

## 修改自己的 C 头文件以便 C++ 容易使用 include 语句包含

- 如果在 C++ 中包含一个非系统提供的 C 头文件，且 C 头文件可以修改，强烈建议在头文件添加 `extern "C" {/**/}` 语句使得 C++ 开发者更加容易使用 `#include` 包含此头文件到 C++ 代码
- 因为 C 编译器不理解 `extern "C" {/**/}` 结构，必须使用 `#ifdef` 包裹 `extern "C" {` 和 `}`，以便 C 编译器看不到这个结构

```c
//my-c-code.h
//有且只有 C++ 编译器会定义 __cplusplus 符号
#ifdef __cplusplus
extern "C" {
#endif

//c code

#ifdef __cplusplus
}
#endif
}
```

```c++
#include "my-c-code.h"

int main()
{
  sum(1, 2);
  return 0;
}
```

## 在 C++ 中调用非系统的 C 函数 f(int,char,float)

- 如果需要调用一个 C 函数，但是不需要或者不想包含声明此函数的 C 头文件，可以在 C++ 代码中使用 `extern "C"` 语法声明这一个 C 函数。一般需要使用完整的函数原型

```c++
extern "C" void f(int,char,float);

int main()
{
  int i=1;
  char c='c';
  float ff=2;
  f(i, c, ff);
  return 0;
}
```

- 多个 C 函数可以使用 `extern "C" {/**/}`

## 创建一个可被 C 调用的 C++ 函数 f(int,char,float)

- C++ 编译器必须通过 `extern "C"` 知道 `f(int,char,float)` 会被 C 编译器调用

```c++
extern "C" void f(int,char,float);

//define f(int,char,float) in some c++ module
void f(int i, char c, float ff)
[
  //...
]
```

- `extern "C"` 告诉编译器发送给链接器的外部信息应该使用 C 的调用管理和名称重整(name-mangling)(比如前置一个下划线)。因为 C 不支持名称重载，你不能在 C 程序中同时调用多个重载的函数

## 链接器在 C/C++ 调用 C++/C 函数时报错的原因

- 如果 `extern "C" 语法不正确，会有一些链接错误而不是编译器错误。因为 C++ 编译器通常会重整(mangle) 函数名称(比如为了支持函数重载)而跟 C 编译器不同

## 传递一个 C++ 类的对象给/从 C 函数

```c++
// fred.h: this header can be read by c/c++ compilers
#ifndef FRED_H
#define FRED_H

#ifdef __cplusplus
class Fred {
public:
  Fred();
  void wilma(int);

private:
  int a_;
};
#else
typedef struct Fred Fred;
#endif

#ifdef __cplusplus
extern "C" {
#endif

#if defined(__STDC__) || defined(__cplusplus)
  extern void c_function(Fred*);///* ANSI C prototypes */
  extern Fred* cplusplus_callback_function(Fred*);
#else
  extern void c_function();///* K&R style */
  extern Fred* cplusplus_callback_function();
#endif

#ifdef __cplusplus
}
#endif

#endif
```

```cpp
// fred.cpp
#include "fred.h"

#include <stdio.h>

Fred::Fred() : a_(0)
{

}

void Fred::wilma(int a)
{
  a_ = a;
  printf("a=%d\n", a);
}

Fred* cplusplus_callback_function(Fred* fred)
{
  fred->wilma(123);
  return fred;
}
```

```c
// ccode.c
#include "fred.h"

void c_function(Fred* fred)
{
  cplusplus_callback_function(fred);
}
```

```c++
// cppcode.cpp
#include "fred.h"

int main()
{
  Fred fred;
  c_function(&fred);
  return 0;
}
```

- 编译命令 `gcc fred.h fred.cpp ccode.c cppcode.cpp` 或 `gcc fred.h fred.cpp ccode.c cppcode.cpp`，输出 `a=123`
- 和 C++ 代码不同，C 代码不能识别统一对象的两个指针，除非这两个指针完全是同一类型。比如，C++中容易检查指向同一对象的衍生类指针 dp 和基类指针 bp
  - 判断 `if(dp == bp)`，C++ 编译器会自动转化两个指针到同一类型，这种情况下到基类指针，然后进行比较。这取决于 C++ 编译器的具体实现，有时候这种转化会改动一个指针值的比特位
  - 技术层面上，大部分 C++ 编译器使用一个二进制对象格式以便多继承和/或虚继承的转换。但是 C++ 语言不会暴露对象格式，因此从原则上说，一个转化也会发生在非虚单继承
  - 关键点是 C 编译器不知道如何做指针转换，所以从衍生类到基类指针的转换必须发生在 C++ 编译器编译的代码，而不是 C 编译器编译的代码
  - **注意：**当转换衍生类和基类指针到 void* 时必须要小心，因为这个不支持 C/C++ 编译器做适合的指针调整

  ```cpp
  void f(Base* bp, Derived *dp)
  {
    if(bp ==dp) //valid to compare a Base* to Derived*
    {
      //...
    }
     void* xp = bp;
     void* yp = dp;
     if(x == y) //bad form! do not use this!
     {
       //...
     }
  }
  ```

  - 如上所述，上述指针转换会发生在多继承和/或虚继承
  - 使用 void* 指针的安全方式

  ```cpp
  void f(Base* bp, Derived *dp)
  {
    void* xp = bp;
    // If conversion is needed, it will happen in the static_cast<>
    void* yp = static_cast<Base*>(dp);
    if(x == y)//valid to compare a Base* to Derived*
    {
      //....
    }
  }
  ```

## C 函数是否可以直接访问 C++ 类对象的数据

- 如果一个 C++ 类满足下面的条件，C 函数可以安全访问 C++ 对象的数据
  - 没有虚函数(包括继承的虚函数)
  - 所有数据的访问权限相同(private/protected/public)
  - 不含带有虚函数的完全包含的子对象
- 如果 C++ 类由任何基类(或者其完全包含的子对象有基类)，对这些数据的访问都是不可移植的。因为语言不会暴露带有继承的类格式。但实际上，所以的 C++ 编译器的处理方式相同：基类对象在前面(多重继承时按照从左至右的顺序)，然后是成员对象
- 此外，如果该类(或任何基类)包含虚函数，几乎所有 C++ 编译器会在对象内放置一个 void*，或者是在第一个虚函数的位置，或者是在对象最开始的位置。这一点也不是语言要求的，但是每个语言都这样处理
- 如果类包含需基类，情况更加复杂且更难移植。一个通用的实现技术是在对象最后包含一个虚基类的对象(不管虚基类在继承中的层次结构)。对象的其他部分还是正常的顺序。每个衍生的类实际上有一个指向虚基类的指针？？？

## 使用 C++ 比 C 更觉得远离机器代码

- 作为面向对象(OO)的编程语言，C++ 支持模型化问题域，这支持在问题域的语言进行编程而不是使用解决方案域的语言编程
- C 的一个很大优势是没有隐藏机制：看到的就是得到的。可以阅读一个 C 程序并看到每一个时钟周期。但 C++ 不支持。虽然 C++ 为编程者隐藏了一些机制，它也提供了一个抽象层和表达上的经济，以便降低维护成本且不会破坏运行时性能

## 参考

- [mixing c and cpp](https://isocpp.org/wiki/faq/mixing-c-and-cpp)
- [What is the effect of extern “C” in C++](https://stackoverflow.com/questions/1041866/what-is-the-effect-of-extern-c-in-c)
