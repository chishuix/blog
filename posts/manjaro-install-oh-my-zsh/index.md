# Manjaro 安装 Oh My Zsh


<!--more-->

## 切换终端

Manjaro 默认集成了 ZSH，切换一下终端就 OK 了。

```bash
# 查看当前已安装的shell种类
$ cat /etc/shells
# 切换当前用户
$ chsh -s /bin/zsh
# 修改文件切换 root 用户
$ sudo vim /etc/passwd
```

## 安装程序

```bash
# 下载不了时，直接另存为 .sh 文件，然后执行 bash install.sh 。
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## 语法高亮

```bash
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
$ echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
$ source ./zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

## 更改主题

*~/.zshrc* 

```
ZSH_THEME="ys"
```

## 参考文档

[oh-my-zsh 官方主题库](https://github.com/robbyrussell/oh-my-zsh/wiki/themes)
