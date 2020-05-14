---
title: "Manjaro 安装 Sublime Text"
date: 2020-05-13T09:58:14+08:00
draft: true
recommend: false

tags: ['Manjaro', 'Sublime Text']
categories: ['Programing']
hiddenFromHomePage: true

featuredImage: ""
toc: true
---

<!--more-->

## 安装 GPG key

```bash
$ curl -O https://download.sublimetext.com/sublimehq-pub.gpg && sudo pacman-key --add sublimehq-pub.gpg && sudo pacman-key --lsign-key 8A8F901A && rm sublimehq-pub.gpg
```

## 指定版本

### 稳定版

```bash
$ echo -e "\n[sublime-text]\nServer = https://download.sublimetext.com/arch/stable/x86_64" | sudo tee -a /etc/pacman.conf
```

### 开发版

```bash
$ echo -e "\n[sublime-text]\nServer = https://download.sublimetext.com/arch/dev/x86_64" | sudo tee -a /etc/pacman.conf
```

## 开始安装

```bash
$ yay -Syu sublime-text
```

## LICENSE

[Sublime Text 3 激活码](/sublime-text/license/)

