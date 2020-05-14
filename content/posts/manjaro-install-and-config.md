---
title: "Manjaro 安装和配置"
date: 2020-05-12T11:45:41+08:00
draft: false
recommend: true

tags: ['Manjaro']
categories: ['Programing']
hiddenFromHomePage: false

featuredImage: "//static.u1s3.com/images/f28e80b85dad82eac291f789056b1fca.jpg"
toc: true
---

<!--more-->

## 分区方案

硬件条件：120G SSD

| 挂载点    | 大小        | 标记 | 文件系统  |
| --------- | ----------- | ---- | --------- |
| /boot/efi | 1024M~2048M | ESP  | fat32     |
| /         | 60G         |      | ext4      |
| /home     | 40G         |      | ext4      |
| swap      | 8192M       |      | linuxswap |

## 配置国内镜像

### 官方镜像

```bash
# 更新排序国内镜像 在弹窗中选择一个镜像
# 建议选择清华或交大
# 清华：https://mirrors.tuna.tsinghua.edu.cn/manjaro/
# 交大：https://mirrors.sjtug.sjtu.edu.cn/manjaro/
$ sudo pacman-mirrors -i -c China -m rank

# 查看当前选择的源
$ sudo vim /etc/pacman.d/mirrorlist
```

### archlinuxcn 镜像

*/etc/pacman.conf*

注意，这里只能选择使用其中一个。

```
# 清华
[archlinuxcn]
SigLevel = Optional TrustAll
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch

# 交大
[archlinuxcn]
SigLevel = Optional TrustAll
Server = https://mirrors.sjtug.sjtu.edu.cn/archlinux-cn/$arch
```

### 更新镜像及系统

```bash
# 更新数据源
$ sudo pacman -Syy
# 导入公钥
$ sudo pacman -S archlinuxcn-keyring
# 更新系统
$ sudo pacman -Syyu
```

## 常用软件安装

[Manjaro 安装 Yay](/posts/manjaro-install-yay)

[Manjaro 安装 Oh My Zsh](/posts/manjaro-install-oh-my-zsh/)

[Manjaro 安装 搜狗拼音输入法](/posts/manjaro-sogoupinyin)





[Manjaro 常用软件名称]

## 启用 SSH 访问

```bash
# 开机启动
$ systemctl enable sshd.service 
# 立即启动
$ systemctl start sshd.service 
# 立即重启
$ systemctl restart sshd.service 
```





## 使用微软雅黑



## 英文文件夹名称

### 更改配置文件

*~/.config/user-dirs.dirs*

```
XDG_DESKTOP_DIR="$HOME/Desktop"
XDG_DOWNLOAD_DIR="$HOME/Downloads"
XDG_TEMPLATES_DIR="$HOME/Templates"
XDG_PUBLICSHARE_DIR="$HOME/Public"
XDG_DOCUMENTS_DIR="$HOME/Documents"
XDG_MUSIC_DIR="$HOME/Music"
XDG_PICTURES_DIR="$HOME/Pictures"
XDG_VIDEOS_DIR="$HOME/Videos"
```

### 更改文件夹名称

```bash
$ mv 桌面 Desktop && mv 下载 Downloads && mv 模板 Templates && mv 公共 Public && mv 文档 Documents && mv 视频 Videos && mv 图片 Pictures && mv 音乐 Music
```

### 重启电脑





## 建议字体

安装文泉驿黑体 `wqy-microhei`，通过系统字体管理修改字体。





## 参考文档

[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/)