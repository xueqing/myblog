---
title: "RAII 资源获取即初始化"
authors: [kiki]
tags: [c++]
categories: [blog]
draft: false
---

- [概念](#%e6%a6%82%e5%bf%b5)
- [使用 RAII](#%e4%bd%bf%e7%94%a8-raii)
- [例子](#%e4%be%8b%e5%ad%90)
- [标准库](#%e6%a0%87%e5%87%86%e5%ba%93)
- [注解](#%e6%b3%a8%e8%a7%a3)
- [参考](#%e5%8f%82%e8%80%83)

## 概念

资源获取即初始化（Resource Acquisition Is Initialization），或称 RAII，是一种 C++ 编程技术，它将必须在使用前请求的资源（分配的堆内存、执行线程、打开的套接字、打开的文件、锁定的互斥体、磁盘空间、数据库连接等——任何存在受限供给中的事物）的生命周期绑定与一个对象的生存期相绑定。

## 使用 RAII

- RAII 保证资源可用于任何会访问该对象的函数（资源可用性是一种[类不变式](https://en.wikipedia.org/wiki/Class_invariant)，这会消除冗余的运行时测试）。它亦保证所有资源在其控制对象的生存期结束时，以获取顺序的逆序释放。类似地，若资源获取失败（构造函数以异常退出），则为已构造完成的对象和基类子对象所获取的所有资源，会以初始化顺序的逆序释放。这有效地利用了语言特性（对象生存期、退出作用域、初始化顺序以及栈回溯）以消除内存泄漏并保证异常安全。根据 RAII 对象的生存期在退出作用域时结束这一基本状况，此技术的另一名称是作用域界定的资源管理（ Scope-Bound Resource Management，SBRM）。

- RAII 可总结如下:
  - 将每个资源封装入一个类，其中
    - 构造函数请求资源，并建立所有类不变式，或在它无法完成时抛出异常
    - 析构函数释放资源并决不抛出异常
  - 始终经由 RAII 类的实例使用满足要求的资源，该资源
    - 自身拥有自动存储期或临时生存期，或
    - 具有与自动或临时对象的生存期绑定的生存期
- 移动语义使得在对象间、跨作用域，以及在线程内外安全地移动所有权，而同时维护资源安全成为可能。

## 例子

拥有 open()/close()、lock()/unlock()，或 init()/copyFrom()/destroy() 成员函数的类是非 RAII 类的典型的例子

```cpp
std::mutex m;

void bad()
{
    m.lock();                    // 请求互斥体
    f();                         // 若 f() 抛异常，则互斥体永远不被释放
    if(!everything_ok()) return; // 提早返回，互斥体永远不被释放
    m.unlock();                  // 若 bad() 抵达此语句，互斥才被释放
}

void good()
{
    std::lock_guard<std::mutex> lk(m); // RAII类：互斥体的请求即是初始化
    f();                               // 若 f() 抛异常，则释放互斥体
    if(!everything_ok()) return;       // 提早返回，互斥体被释放
}                                      // 若 good() 正常返回，则释放互斥体
```

## 标准库

- C++ 标准库遵循 RAII 管理其自身的资源：`std::string`、`std::vector`、`std::thread`，以及多数其他类在构造函数中获取其资源（错误时抛出异常），并在其析构函数中释放之（决不抛出），而不要求显式清理。
- 另外，标准库提供几种 RAII 包装器以管理用户提供的资源：
  - `std::unique_ptr` 及 `std::shared_ptr` 用于管理动态分配的内存，或以用户提供的删除器管理任何以普通指针表示的资源
  - `std::lock_guard`、`std::unique_lock`、`std::shared_lock` 用于管理互斥体

## 注解

RAII 不适用于并非在使用前请求的资源：CPU 时间、核心，以及缓存容量、熵池容量、网络带宽、电力消费、栈内存等。

## 参考

- [RAII](https://zh.cppreference.com/w/cpp/language/raii)