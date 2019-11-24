---
title: "关于 const"
authors: [kiki]
tags: [c++]
categories: [blog]
draft: false
---

- [1 常量正确性(const correctness)](#1-%e5%b8%b8%e9%87%8f%e6%ad%a3%e7%a1%ae%e6%80%a7const-correctness)
  - [1.1 概述](#11-%e6%a6%82%e8%bf%b0)
  - [1.2 const 和 *](#12-const-%e5%92%8c)
  - [1.3 const 和 &](#13-const-%e5%92%8c)
  - [1.4 成员函数](#14-%e6%88%90%e5%91%98%e5%87%bd%e6%95%b0)
- [2 二重指针](#2-%e4%ba%8c%e9%87%8d%e6%8c%87%e9%92%88)
- [3 修改 const](#3-%e4%bf%ae%e6%94%b9-const)
- [4 X**->const X**](#4-x-const-x)
- [5 reference](#5-reference)

## 1 常量正确性(const correctness)

### 1.1 概述

- 常量正确性：即使用关键字`const`避免常量对象被修改
- 函数接收一个参数，且在调用过程中不能修改参数，实现方式有三种
  - f1/f2 中不能修改传入参数，否则会有编译警告
  - f3 值传递，只修改局部拷贝
  - f1/f2 中不能调用 g1/g2，否则会有编译警告

```cpp
void f1(const std::string& s);  //pass by reference-to-const
void f2(const std::string* sp); //pass by pointer-to-const
void f3(std::string s);         //pass by value

void g1(std::string& s);        //pass by reference-to-non-const
void g2(std::string* sp);       //pass by pointer-to-non-const
```

- 常量正确性：避免意外修改不希望修改的东西
- 一般来说，`const` 作用于离它最近的左侧的类型，否则，作用于离它最近的右侧的类型
- 规则：`read it backwards`，即倒着读
- 建议：X 放在修饰符的右边

### 1.2 const 和 *

- `const X * ptr`：ptr is a pointer to an X that is const
  - ptr 是一个指针变量，指向一个 X 的对象，但不能通过指针修改 X 对象, *ptr 只读
  - 不能通过 ptr 调用 X 非 const 的成员方法，否则会有编译警告

| 声明 | 解释 | 描述 |
| --- | --- | --- |
| `const int const0=96;` | int is const | const1 是整型常量，不可再赋值 |
| `X * ptr` | ptr is a pointer to an X | X 对象实例的指针 |
| `const X * ptr` | ptr is a pointer to an X that is const | ptr 是一个指针变量，指向一个 X 的对象，但不能通过指针修改 X 对象, *ptr 只读 |
| `X const * ptr` | 同上 | 同上 |
| `X const const * ptr` | 同上 | 同上 |
| `const X const * ptr` | 同上 | 同上 |
| `X * const ptr` | ptr is a const pointer to an X | ptr 是一个常量指针，指向一个 X 的对象，不能给指针再赋值，但是可以通过指针修改 X 对象，ptr 只读 |
| `X const * const ptr` | ptr is a const pointer to a const X | ptr 是一个常量指针，指向一个 X 的对象，但不能给指针再赋值，也不能通过指针修改 X 对象 |
| `const X * const ptr` | 同上 | 同上 |

```cpp
const char *Function1() { return "Some text"; }
Function1()[1]='a'; //error
```

- 常量指针指向变量：变量可修改，且未从内存移除
- 指向常量的指针变量：函数返回常量字符串数组，防止修改返回值错误

### 1.3 const 和 &

- `const X & obj`：obj is a reference to an X that is const
  - obj 是一个 X 对象的引用，但不能通过 obj 修改 X 对象
  - 不能通过 obj 调用 X 非 const 的成员方法，否则会有编译警告

| 声明 | 解释 | 描述 |
| --- | --- | --- |
| `const X & obj` | obj is a reference to an X that is const | obj 是一个 X 对象的引用，但不能通过 obj 修改 X 对象, obj 只读 |
| `X const & obj` | 同上 | 同上 |

### 1.4 成员函数

- 在成员函数后加`const`避免在内部修改成员变量

```cpp
class MyClass{
  int m_var;
  
  // modify m_var is not allowed
  void SomeMethod() const;

  // the var pointed to by returned pointer and returned pointer is not allowed to altered
  // the var pointed to by given pointer and given pointer is const
  // modify m_var is not allowed
  const int * const AnotherMethod(const int * const &) const;

  // the return value must not be reference to a member of MyClass
  std::string& BadMethod() const;

  // the return value can be reference to a member of MyClass
  const std::string& GoodMethod() const;
}
```

## 2 二重指针

| 声明 | 解释 | 描述 |
| --- | --- | --- |
| `int ** pp` | pp is a pointer to a pointer to an int | - |
| `int ** const pp` | pp is a const pointer to a pointer to an int | - |
| `int * const * pp` | pp is a pointer to a const pointer to an int | - |
| `int const ** pp` | pp is a pointer to a pointer to a const int | - |
| `int * const * const pp` | pp is a const pointer to a const pointer to an int |

## 3 修改 const

- 一个对象或变量被声明成`const`，后续可以使用`const_cast`修改其为可变
- 将类的某些成员变量声明为`mutable`，可在`const`成员函数中修改它们

## 4 X**->const X**

- 将`X*`转成`const X*`是安全的
- 将`X**`转成`const X**`会有编译警告，因为可能会不经 cast 操作修改一个`const X`对象

```cpp
class MyClass {
  public:
  void Modify();
}

int main()
{
  const MyClass obj;
  MyClass * ptr;
  const MyClass ** pptr = &ptr; // compile error
  *pptr = &obj; // ptr points to obj
  ptr->Modify();  // ptr modifies const obj
  return 0;
}
```

## 5 reference

- [const correctness](https://isocpp.org/wiki/faq/const-correctness)
- [What is the difference between const int\*, const int \* const, and int const \*](https://stackoverflow.com/questions/1143262/what-is-the-difference-between-const-int-const-int-const-and-int-const)
- [The C++ 'const' Declaration: Why & How](http://duramecho.com/ComputerInformation/WhyHowCppConst.html)
