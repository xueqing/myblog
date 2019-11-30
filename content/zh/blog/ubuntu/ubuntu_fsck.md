---
title: "fsck 命令"
authors: [kiki]
tags: [ubuntu, linux]
categories: [blog]
draft: false
---

- Ubuntu16 在启动的时候出错 `fsck exited with status code 4`
- 出错原因：磁盘检测不能通过
- 解决方法：`fsck.ext4 -y /dev/sda1`
