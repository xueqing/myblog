---
title: "4 面向对象泛型"
authors: [kiki]
tags: [sa]
categories: [translation]
weight: 5
draft: false
---

- [4 面向对象泛型](#4-%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E6%B3%9B%E5%9E%8B)
  - [4.1 面向对象泛型介绍](#41-%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E6%B3%9B%E5%9E%8B%E4%BB%8B%E7%BB%8D)
  - [4.2 面向对象分析](#42-%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E5%88%86%E6%9E%90)
  - [4.3 面向对象设计](#43-%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E8%AE%BE%E8%AE%A1)

## 4.1 面向对象泛型介绍

面向对象系统的基础概念和术语包括

- 对象：在面向对象环境中，对象是真实的元素，在物理(比如一个客户、一辆车)或概念(比如一个项目、一个进程)上是存在的
  - 每个对象都有
    - 身份：与系统中其他对象区分
    - 状态：决定一个对象的特点属性，已经持有的属性的值
    - 行为：表示一个对象执行的外部可见的行为，和对象的状态变化相关
  - 对象可以根据应用的需求被模型化
- 类：类表示拥有相同的特点属性(表现相同的行为)的一组对象
  - 创建一个类的一个对象作为成员称作实例化。因此，对象是类的一个实例
  - 一个类包括
    - 一组属性：对象从类实例化得到的属性
      - 一般来说，一个类的不同对象在这些属性上有一些不同
      - 属性常表示成类的数据
    - 一组操作：描绘该类的对象的行为
      - 操作常表示成类的函数或方法
- 封装：封装是在类中将属性和方法绑定在一起的过程
  - 通过封装，类可以向外部隐藏内部的细节
  - 类只允许外部通过接口访问类内的元素
- 多态：多态暗示操作的方式不同，取决于操作针对的实例
  - 多态使得内部结构不同的对象可以向外部提供统一的接口
  - 多态主要通过继承实现
  - 关系：系统包括动态(行为)指标和静态(逻辑)指标
    - 动态指标描述对象之间的关系(比如消息传递)
    - 静态指标描述类之间的关系(比如聚合、关联、继承)
- 消息传递：系统内的对象彼此使用消息传递通信
  - 一个对象如果想要另一个对象执行一个方法，必须给该对象发送一个消息
- 复合或聚合：复合或聚合表示类间的关系，指一个类可以通过其他类对象的组合生成
  - 聚合指的是 part-of 或 has-a 关系
  - 一个聚合类由一个或多个其他的对象组成
- 关联：关联是拥有共同的结构和行为的一组链接(link)。关联描述了一个或多个类的对象之间的关系
  - 一个链接(link)可以定义成一个关联的一个实体
  - 关联的度表示参与到一个连接的类的数目。度可以是单元、二元或三元的
    - 一个单元关系连接同一个类内的对象
    - 一个二元关系连接两个类的对象
    - 一个三元关系连接三个或多个类的对象
- 继承：继承机制允许基于已有的类创建类，创建的类可以扩展或重定义能力
  - 已有的类称作基类/父类/超类(super-class)，新类成为衍生类/子类/亚类(subclass)
  - 亚类可以继承或衍生超类提供的属性和方法
  - 亚类也可以添加自己的属性和方法，且方法可能会改变超类的方法
  - 继承定义了 is-a 的关系

## 4.2 面向对象分析

- OO(object-oriented，面向对象)分析的目的是理解系统的应用领域和特定的需求
- OO 分析的输出是系统的需求规格文档、逻辑结构的初始分析和可行性
- OO 分析常用的分析技术包括
  - 对象建模：开发软件系统对象的静态结构。对象建模的步骤
    - 标识对象，组合成类
    - 标识对象间的关系
    - 创建一个用户对象模型图
    - 定义一个用户对象的属性
    - 定义类上应该执行的操作
  - 动态建模：动态建模的目的是检验系统的时间和外部变化
    - 动态建模可以定义为“描述一个单独的对象如何响应事件的一种方式，事件可以是其他对象触发的内部事件，或者外部世界触发的外部事件”
    - 动态建模的步骤
      - 标识每个对象的状态
      - 标识事件，分析行为的适用性
      - 构建动态模型图，包括状态转换图
      - 表示对象属性的每个状态
      - 验证画的状态转换图
  - 功能建模：功能模型展示了对象内部执行的流程，以及对象在方法之间移动时数据如何变化
    - 功能建模详细说明了对象建模操作和动态建模行为的意义
    - 功能建模和传统的结构化分析的数据流图对应
    - 功能建模的步骤
      - 标识所有的输入和输出
      - 构建数据流图，展示功能依赖性
      - 说明每个函数的目的
      - 标识约束
      - 具体说明优化标准

## 4.3 面向对象设计

- 面向对象设计(OOD, object-oriented design)阶段，面向对象分析中的技术独立性概念被映射到实现类，约束被标识，接口被设计，生成解决方案域的模型
- OOD 的主要目的是开发系统的结构化架构
- OOD 的步骤
  - 定义系统的上下文
  - 设计系统架构
  - 标识系统的对象
  - 构造设计模型
  - 规范对象接口
- OOD 总结为两个步骤
  - 概要设计：也叫高层设计，定义系统所需所有类，详细描述每个类的功能
    - 使用类图描述类间的关系
    - 交互图展示事件流
  - 详细设计：也叫低层设计，基于交互图给每个类赋予属性和操作
    - 状态图描述设计进一步的细节
- 需要遵循下面的设计原则
  - 解耦原理：低耦合，可以通过引入新的类或继承消除耦合
  - 确保内聚：高内聚，一个内聚的类执行一组紧密相关的功能
  - 开放封闭原则：系统应该可以扩展满足新的需求
    - 系统已有的实现和代码不应该被修改以扩展系统
    - 下面是指导方针
      - 对于每一个具体类，应该维护独立的接口和实现
      - 在多线程环境，保持属性是私有的
      - 最小化全局变量和类变量