---
title: "Node.js 环境搭建"
authors: [kiki]
tags: [ubuntu, linux]
categories: [blog]
draft: false
---

- [1 版本要求](#1-%e7%89%88%e6%9c%ac%e8%a6%81%e6%b1%82)
- [2 提供以下四种方式安装(PS: 建议第四种方式安装)](#2-%e6%8f%90%e4%be%9b%e4%bb%a5%e4%b8%8b%e5%9b%9b%e7%a7%8d%e6%96%b9%e5%bc%8f%e5%ae%89%e8%a3%85ps-%e5%bb%ba%e8%ae%ae%e7%ac%ac%e5%9b%9b%e7%a7%8d%e6%96%b9%e5%bc%8f%e5%ae%89%e8%a3%85)
  - [2.1 调用官网脚本自动安装](#21-%e8%b0%83%e7%94%a8%e5%ae%98%e7%bd%91%e8%84%9a%e6%9c%ac%e8%87%aa%e5%8a%a8%e5%ae%89%e8%a3%85)
  - [2.2 使用二进制包文件](#22-%e4%bd%bf%e7%94%a8%e4%ba%8c%e8%bf%9b%e5%88%b6%e5%8c%85%e6%96%87%e4%bb%b6)
    - [2.2.1 设置文件路径](#221-%e8%ae%be%e7%bd%ae%e6%96%87%e4%bb%b6%e8%b7%af%e5%be%84)
    - [2.2.2 建立软连接](#222-%e5%bb%ba%e7%ab%8b%e8%bd%af%e8%bf%9e%e6%8e%a5)
  - [2.3 apt 安装](#23-apt-%e5%ae%89%e8%a3%85)
- [3 查看nodejs的版本](#3-%e6%9f%a5%e7%9c%8bnodejs%e7%9a%84%e7%89%88%e6%9c%ac)
- [4 查看npm的版本](#4-%e6%9f%a5%e7%9c%8bnpm%e7%9a%84%e7%89%88%e6%9c%ac)

## 1 版本要求

- node >= 8.x 目前稳定版本为 10.x
- npm >= 5.x

## 2 提供以下四种方式安装(PS: 建议第四种方式安装)

### 2.1 调用官网脚本自动安装

- [参考](https://github.com/nodesource/distributions/blob/master/README.md#debinstall)

```sh
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### 2.2 使用二进制包文件

#### 2.2.1 设置文件路径

```sh
# 按下载版本修改 VERSION
VERSION=v10.15.1
DISTRO=linux-x64
sudo mkdir -p /usr/local/lib/nodejs
sudo tar -xJvf node-$VERSION-$DISTRO.tar.xz -C /usr/local/lib/nodejs
```

#### 2.2.2 建立软连接

```sh
VERSION=v10.15.1
DISTRO=linux-x64
sudo ln -s /usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin/node /usr/bin/node
sudo ln -s /usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin/npm /usr/bin/npm
sudo ln -s /usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin/npx /usr/bin/npx
```

### 2.3 apt 安装

安装版本较低，需手动更新

```sh
# 安装 nodejs
sudo apt install node
# 安装 nodejs 的依赖库管理工具 npm
sudo apt install npm
# 安装 nodejs 版本管理工具 n
sudo npm install n -g
# 升级 nodejs 的版本
sudo n stable
```

或者

```sh
sudo apt install node
sudo apt install npm
# 升级 npm
sudo npm install npm@latest -g
# 升级 node
sudo npm install -g n
sudo n stable
```

## 3 查看nodejs的版本

```sh
node
# 或
node -v
```

## 4 查看npm的版本

```sh
npm -v
```
