---
title: "Manjaro 安装 Yay"
date: 2020-05-12T14:41:54+08:00
draft: false
recommend: false

tags: ['Manjaro']
categories: ['Programing']
hiddenFromHomePage: true

featuredImage: ""
toc: true
---

<!--more-->

## 程序安装

```bash
$ sudo pacman -S yay
```

## 配置 AUR 镜像

```bash
$ yay --aururl "https://aur.tuna.tsinghua.edu.cn" --save
```

查看修改过的配置

```bash
$ yay -P -g
```

## 基本命令

| 用途 | 命令                |
| :--- | :------------------ |
| 搜索 | `$ yay -Ss package` |
| 安装 | `$ yay -S package`  |
| 卸载 | `$ yay -R package`  |
| 列表 | `$ yay -Qs package` |

## 参考文档

[AUR 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/AUR/)