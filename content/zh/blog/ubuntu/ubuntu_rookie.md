---
title: "Ubuntu 新手配置"
authors: [kiki]
tags: [ubuntu, linux]
categories: [blog]
draft: false
---

- [1 环境](#1-%e7%8e%af%e5%a2%83)
- [2 配置静态 ip](#2-%e9%85%8d%e7%bd%ae%e9%9d%99%e6%80%81-ip)
- [3 配置输入法和字体](#3-%e9%85%8d%e7%bd%ae%e8%be%93%e5%85%a5%e6%b3%95%e5%92%8c%e5%ad%97%e4%bd%93)
  - [中文界面](#%e4%b8%ad%e6%96%87%e7%95%8c%e9%9d%a2)
  - [配置搜狗输入法](#%e9%85%8d%e7%bd%ae%e6%90%9c%e7%8b%97%e8%be%93%e5%85%a5%e6%b3%95)
  - [配置 Consolas 雅黑字体](#%e9%85%8d%e7%bd%ae-consolas-%e9%9b%85%e9%bb%91%e5%ad%97%e4%bd%93)
- [4 安装与配置 Git](#4-%e5%ae%89%e8%a3%85%e4%b8%8e%e9%85%8d%e7%bd%ae-git)
- [5 安装 Qt](#5-%e5%ae%89%e8%a3%85-qt)
  - [Qt5.7.1](#qt571)
  - [Qt5.4.1](#qt541)
- [6 命令行安装工具包](#6-%e5%91%bd%e4%bb%a4%e8%a1%8c%e5%ae%89%e8%a3%85%e5%b7%a5%e5%85%b7%e5%8c%85)
  - [supervisor](#supervisor)
  - [VSCode](#vscode)
  - [Chrome 浏览器](#chrome-%e6%b5%8f%e8%a7%88%e5%99%a8)
  - [TeamViewer](#teamviewer)
  - [有道词典](#%e6%9c%89%e9%81%93%e8%af%8d%e5%85%b8)
  - [截图工具 Shutter](#%e6%88%aa%e5%9b%be%e5%b7%a5%e5%85%b7-shutter)

## 1 环境

默认 Ubuntu 16.04

## 2 配置静态 ip

- IP地址      192.168.1.81
- 子网掩码    255.255.0.0
- DNS 与网关  192.168.1.1

## 3 配置输入法和字体

### 中文界面

- 系统输入-语言支持-添加删除语言-选择 Chinese
- 将汉语中国拖动到第一栏

### 配置搜狗输入法

```sh
# 从 Sogou 官网下载 deb 安装包
sudo dpkg -i sougou-packet.deb
sudo apt-get install -f
```

- 系统输入-语言支持-键盘输入法系统选择 fctix
- 系统输入-文本输入-添加输入源-选择 sougo 输入法。
- 系统输入-文本输入-切换到下一个源改为 Super+空格（Ctrl+空格 与 Qt 提示冲突）
- 重启

### 配置 Consolas 雅黑字体

```sh
cd temp
wget http://www.mycode.net.cn/wp-content/uploads/2015/07/YaHeiConsolas.tar.gz
tar -zxvf YaHeiConsolas.tar.gz
sudo mkdir -p /usr/share/fonts/vista
sudo cp YaHeiConsolas.ttf /usr/share/fonts/vista/
sudo chmod 644 /usr/share/fonts/vista/*.ttf
cd /usr/share/fonts/vista/
sudo mkfontscale && sudo mkfontdir && sudo fc-cache -fv
sudo apt-get install unity-tweak-tool
reboot
# 终端中输入配置字体
unity-tweak-tool
```

## 4 安装与配置 Git

```sh
sudo apt-get install git
sudo apt-get install gitk
# 创建私钥与公钥，并把公钥放到gitlab上
cd ~/.ssh
ssh-keygen 或者 ssh-keygen -t rsa -C "kiki@ubuntu.com"
# 配置全局姓名与邮箱
git config --global user.name kiki
git config --global user.email kiki@ubuntu.com
```

## 5 安装 Qt

### Qt5.7.1

- 需要解决 Qt 中不能输入中文的 BUG

```sh
sudo apt-get install fcitx-frontend-qt5
cd usr/lib/x86_64-linux-gnu/qt5/plugins/platforminputcontexts/
cp -rp libfcitxplatforminputcontextplugin.so ~/Software/Qt5.7.1/Tools/QtCreator/lib/Qt/plugins/platforminputcontexts
cp -rp libfcitxplatforminputcontextplugin.so ~/Software/Qt5.7.1/5.7/gcc_64/plugins/platforminputcontexts
# 重启 Qt_Creator
# 双版本需要添加快捷方式
cd ~/.local/share/applications
touch DigiaQt-qtcreator-community-5.7.1.desktop
```

- 输入以下内容

```text
[Desktop Entry]
Type=Application
Exec=/local_path/Qt5.4.1/Tools/QtCreator/bin/qtcreator
Name=QtCreator5.4.1 (Community)
GenericName=The IDE of choice for Qt development.
Icon=QtProject-qtcreator
Terminal=false
Categories=Development;IDE;Qt;
MimeType=text/x-c++src;text/x-c++hdr;text/x-xsrc;application/x-designer;application/vnd.qt.qmakeprofile;application/vnd.qt.xml.resource;text/x-qml;text/x-qt.qml;text/x-qt.qbs;
```

### Qt5.4.1

- 需要解决 Qt 中不能输入中文的 BUG

```sh
sudo apt-get install fcitx-frontend-qt5
cd usr/lib/x86_64-linux-gnu/qt5/plugins/platforminputcontexts/
cp -rp libfcitxplatforminputcontextplugin.so ~/Software/Qt5.4.1/Tools/QtCreator/lib/Qt/plugins/platforminputcontexts
cp -rp libfcitxplatforminputcontextplugin.so ~/Software/Qt5.4.1/5.7/gcc_64/plugins/platforminputcontexts
# 重启Qt_Creator
# 双版本需要添加快捷方式
cd ~/.local/share/applications
touch DigiaQt-qtcreator-community-5.4.1.desktop
```

- 输入以下内容

```txt
[Desktop Entry]
Type=Application
Exec=/local_path/Qt5.4.1/Tools/QtCreator/bin/qtcreator
Name=QtCreator5.4.1 (Community)
GenericName=The IDE of choice for Qt development.
Icon=QtProject-qtcreator
Terminal=false
Categories=Development;IDE;Qt;
MimeType=text/x-c++src;text/x-c++hdr;text/x-xsrc;application/x-designer;application/vnd.qt.qmakeprofile;application/vnd.qt.xml.resource;text/x-qml;text/x-qt.qml;text/x-qt.qbs;
```

## 6 命令行安装工具包

### supervisor

```sh
cd ~/temp/
git clone git@192.168.1.36:ylrc/Supervisor.git
cd Supervisor
sudo dpkg -i python-meld3_1.0.2-2_all.deb supervisor_3.2.0-2_all.deb
supervisorctl -v
sudo service supervisor start
```

### VSCode

到`https://code.visualstudio.com/docs/?dv=linux64_deb`下载
使用 `sudo dpkg -i target.deb` 安装
有错误的话使用 `sudo apt-get install -f` 修复
修改首选项中 VSCode 的配置文件,配置字体 Consolas

### Chrome 浏览器

```sh
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome-stable_current_amd64.deb
sudo apt-get install -f
```

### TeamViewer

```sh
# https://www.teamviewer.com/zhcn/download/linux/ 下载
sudo dpkg -i teamviewerTarget.deb
sudo apt-get install -f
```

### 有道词典

```sh
# 有道官方下载ubuntu安装包
sudo apt update
sudo apt upgrade
sudo dpkg -i youdao.deb
sudo apt install -f
```

### 截图工具 Shutter

```sh
sudo apt-get install shutter
```

- 设置快捷键：系统设置->键盘->窗口->自定义快捷键: commond 输入 `shutter -s`, 然后在右侧点击输入想要设置的快捷键
