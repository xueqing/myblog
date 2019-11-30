---
title: "6 以数据为中心的架构"
authors: [kiki]
tags: [sa]
categories: [translation]
weight: 7
draft: false
---

- 组件包括
  - 中心数据结构/数据存储器/数据仓库：负责提供长久的数据存储，表示当前的状态
  - 数据访问器/一系列独立的组件：操作中心数据结构存储，执行计算，存回结果
