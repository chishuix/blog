# Manjaro 安装 搜狗拼音输入法


我真是的是服了，我用了这么长时间的 Manjaro，也一直在安装搜狗输入法，愣是没发现我启用的输入法不是搜狗，真是蠢到爆。

<!--more-->

## 程序安装

Tips：安装前添加 `archlinuxcn` 源会提高下载速度。

```bash
$ yay -S fcitx-im fcitx-configtool fcitx-sogoupinyin
$ yay -S aur/fcitx-qt4
```

安装时如果提示选择安装，选择 **全部安装**，如果提示冲突需要删除某些包就 **同意**。

## 配置文件

*~/.pam_environment*

```
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
```

配置完成后重启一下就能愉快的使用搜狗输入法了。

## 参考文档

[manjaro Linux，有什么较为简单的方法，可安装中文输入法? ](https://www.zhihu.com/question/330715155/answer/731357066)
