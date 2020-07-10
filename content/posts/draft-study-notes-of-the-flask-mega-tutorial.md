---
title: "《The Flask Mega-Tutorial》学习笔记"
date: 2020-07-10T14:26:19+08:00
draft: true
recommend: false

tags: ['python']
categories: ['Programing']
hiddenFromHomePage: true

featuredImage: ""
toc: true
---

<!--more-->

## 项目准备

### 安装 Flask

切记**不要**使用全局安装 Flask，一定要使用**虚拟环境**。

**创建项目目录**

```bash
$ mkdir microblog && cd microblog
```

**搭建虚拟环境**

在 Python3 中，虚拟环境已成为内置模块，可以直接通过命令创建，但我在 Ubuntu (WSL2) 环境下使用时提示需要先安装依赖。

```bash
# 安装依赖
$ sudo apt install python3-venv
# 创建虚拟环境
# 第一个 venv 是虚拟环境包的名称
# 第二个 venv 是创建的虚拟环境的名称 可更改但没必要
$ python3 -m venv venv
```

如果使用的 Python 版本低于 *3.4*，则需要使用[ *virtualenv*](https://virtualenv.pypa.io/) 这个第三方工具来搭建虚拟环境，安装后执行 `$ virtualenv venv` 来搭建虚拟环境。

**激活虚拟环境**

无论使用哪种方式搭建虚拟环境，都需要使用下面命令激活后使用。

```bash
$ source venv/bin/activate
```

激活后命令行界面会显示当前激活的虚拟环境名称。

```bash
(venv) $ _
```

**安装 Flask**

```bash
(venv) $ pip install flask
```







## 参考文档

[The-Flask-Mega-Tutorial-zh](https://github.com/luhuisicnu/The-Flask-Mega-Tutorial-zh)

