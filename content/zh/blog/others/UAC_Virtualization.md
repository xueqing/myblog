---
title: "UAC 虚拟化"
authors: [kiki]
tags: [uac]
categories: [blog]
draft: false
---

- [前言](#%e5%89%8d%e8%a8%80)
- [Windows 7 上的 UAC 虚拟化](#windows-7-%e4%b8%8a%e7%9a%84-uac-%e8%99%9a%e6%8b%9f%e5%8c%96)
- [UAC 虚拟化的现象](#uac-%e8%99%9a%e6%8b%9f%e5%8c%96%e7%9a%84%e7%8e%b0%e8%b1%a1)
- [思考](#%e6%80%9d%e8%80%83)
- [总结](#%e6%80%bb%e7%bb%93)
- [参考](#%e5%8f%82%e8%80%83)

## 前言

在早期的 Windows 版本(XP,NTY,95 等)，通常由管理员安装应用。这些应用可以自由地读写系统文件和注册表。但是，标准养护运行相同的应用会导致错误弹窗。

## Windows 7 上的 UAC 虚拟化

- 在 Windows 7 上通过重定向写操作到用户配置的一个特殊位置来改善标准账户的应用兼容性
- 例如：如果标准用户运行一个应用，尝试写`C:\Program Files\National Instruments\Settings.ini`，这个写操作会被重定向到`C:\User\Username\AppData\Local\VirtualStore\Program Files\National Instruments\Settings.ini`。同样地，尝试注册到`HKEY_LOCAL_MACHINE\Software\National Instruments\`文件会被重定向到`HKEY_CURRENT_USER\Software\Classes\VirtualStore\MACHINE\Software\National Instruments or HKEY_USERS\...`

## UAC 虚拟化的现象

- 如果有下面的现象表明应用可能是受到 UAC 虚拟化的影响
  - 应用写入 `Program Files`，`Windows`目录或系统根目录(通常是 C 盘)，但是在这些位置不能找到文件
  - 应用写入 Windows 注册表，特比是`HKLM/Software`，但是看不到注册表更新
  - 切换到另外一个用户账户时，应用不能找到之前写入 `Program Files`，`Windows`目录或系统根目录(通常是 C 盘)的文件，或者应用找到了这些文件的旧版本

## 思考

- UAC 虚拟化只用于辅助在 Windows Vista 之前开发的应用的兼容性。Windows 7 的新应用不应该执行写操作到敏感的系统文件，而且不应该依赖 UAC 虚拟化来提供必要的重定向
- 当更新已有代码再 Windows 7 上运行时，确保应用在运行时只存储数据在每个用户的位置
- 判断需要写数据文件到已有目录。被所有用户使用的通用数据应写入全局的公共位置，由所有用户共享。其他的数据应写入每个用户的位置
- 在确定合适的位置后，确保不会硬编码路径

## 总结

通过 UAC 虚拟化，Windows 7 支持标准用户运行开发的需要管理员权限的应用。

## 参考

- [UAC Virtualization and how it affects your Installers](https://forums.ni.com/t5/Windows-7/UAC-Virtualization-and-how-it-affects-your-Installers/gpm-p/3477163?profile.language=en)
