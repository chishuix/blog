---
title: "Windows Terminal 配置 Powershell 7"
date: 2020-06-30T10:29:37+08:00
draft: false
recommend: false

tags: ['Windows Terminal', 'Powershell']
categories: ['Programing']
hiddenFromHomePage: true

featuredImage: ""
toc: true
---

<!--more-->

## 环境准备

既然是使用 Windows Terminal 来配置 Powershell 7，那首先需要安装它们。

### Windows Terminal

可以 [点击这里](microsoft.com/zh-cn/p/windows-terminal/9n0dx20hk701) 通过网页跳转到安装界面，或者直接在 **Microsoft Store** 里搜索安装。

### Fira Code

Fira Code 是一款支持 ligature 连字功能，而且是一款专门为代码显示准备的字体，[点击这里](https://github.com/tonsky/FiraCode/releases) 获取最新的 Releases，Windows 下安装 `.ttf`格式的字体。

### Powershell Core

这里的 Powershell 和 Windows 自带的 Powershell 不可同日而语，[点击这里](https://github.com/PowerShell/PowerShell/releases) 获取最新的 Releases，下载并安装 `.msi` 格式的安装包。

## 安装 Powershell 插件

使用上面安装的新版 Powershell 运行下面命令。

```powershell
# PSReadline 
# 类似 zsh，让命令行很好用 
> Install-Module -Name PSReadLine -AllowPrerelease -Force

# posh-git 
# 让 git 更好用
> Install-Module posh-git -Scope CurrentUser

# oh-my-posh 
# 让命令行更酷炫、优雅
> Install-Module oh-my-posh -Scope CurrentUser
```

安装过程稍慢，等待即可，如提示包来源不可信，输入 `Y`。

## 配置 Windows Terminal

*settings.json*

```json
// 使用新版 Powershell 作为默认命令行工具
"defaultProfile": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",

// Powershell 配置
"profiles":
{
    // ...
    "list":
    [
        // ...
        {
            // 看准 guid
            "guid": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
            "hidden": false,
            "name": "pwsh",
            // 这里的路径可能会由变动，原因是安装的是稳定版还是预览版
            "commandline": "C:/Program Files/PowerShell/7/pwsh.exe -nologo",
            "source": "Windows.Terminal.PowershellCore",
            "startingDirectory": ".",
            "fontFace": "Fira Code",
            "fontSize": 11,
            "historySize": 9001,
            "padding": "5, 5, 20, 25",
            "snapOnInput": true,
            "useAcrylic": false,
            "colorScheme": "Homebrew"
        }
    ]
},

// 色彩主题
"schemes": [
    {
        "name": "Homebrew",
        "black": "#282828",
        "red": "#FC5275",
        "green": "#00a600",
        "yellow": "#b8b87a",
        "blue": "#6666e9",
        "purple": "#b200b2",
        "cyan": "#00a6b2",
        "white": "#bfbfbf",
        "brightBlack": "#666666",
        "brightRed": "#e50000",
        "brightGreen": "#00d900",
        "brightYellow": "#e5e500",
        "brightBlue": "#0000ff",
        "brightPurple": "#e500e5",
        "brightCyan": "#00e5e5",
        "brightWhite": "#e5e5e5",
        "background": "#283033",
        "foreground": "#7ab87a"
    }
],
```

## 添加右键菜单

[点击这里](https://github.com/lextm/windowsterminal-shell/) 获取最新的脚本，然后 **以管理员身份运行** Windows Terminal，并使用新版 Powershell 执行脚本。

```powershell
> .\install.ps1 mini
```

如果修改了 Profile，可能需要先 `uninstall.ps1` 一下，然后重新执行上面的安装命令。

## 添加 Powershell 启动参数

```powershell
> notepad.exe $Profile
```

*Microsoft.PowerShell_profile.ps1*

```
<#
 * FileName: Microsoft.PowerShell_profile.ps1
 * Author: 刘 鹏
 * Email: littleNewton6@outlook.com
 * Date: 2020, May. 1
 * Copyright: No copyright. You can use this code for anything with no warranty.
#>

#------------------------------- Import Modules BEGIN -------------------------------
# 引入 posh-git
Import-Module posh-git

# 引入 oh-my-posh
Import-Module oh-my-posh

# 设置 PowerShell 主题
Set-Theme Paradox
#------------------------------- Import Modules END   -------------------------------

#-------------------------------  Set Hot-keys BEGIN  -------------------------------
# 设置 Tab 键补全
# Set-PSReadlineKeyHandler -Key Tab -Function Complete

# 设置 Tab 为菜单补全和 Intellisense
Set-PSReadLineKeyHandler -Key "Tab" -Function MenuComplete

# 设置 Ctrl+d 为退出 PowerShell
Set-PSReadlineKeyHandler -Key "Ctrl+d" -Function ViExit

# 设置 Ctrl+z 为撤销
Set-PSReadLineKeyHandler -Key "Ctrl+z" -Function Undo

# 设置向上键为后向搜索历史记录
Set-PSReadLineKeyHandler -Key UpArrow -Function HistorySearchBackward

# 设置向下键为前向搜索历史纪录
Set-PSReadLineKeyHandler -Key DownArrow -Function HistorySearchForward
#-------------------------------  Set Hot-keys END    -------------------------------

#-------------------------------    Functions BEGIN   -------------------------------
# Python 直接执行
$env:PATHEXT += ";.py"

# 更新 pip 的方法
function Update-Packages {
    # update pip
    Write-Host "Step 1: 更新 pip" -ForegroundColor Magenta -BackgroundColor Cyan
    $a = pip list --outdated
    $num_package = $a.Length - 2
    for ($i = 0; $i -lt $num_package; $i++) {
        $tmp = ($a[2 + $i].Split(" "))[0]
        pip install -U $tmp
    }

    # update TeX Live
    $CurrentYear = Get-Date -Format yyyy
    Write-Host "Step 2: 更新 TeX Live" $CurrentYear -ForegroundColor Magenta -BackgroundColor Cyan
    tlmgr update --self
    tlmgr update --all
}
#-------------------------------    Functions END     -------------------------------

#-------------------------------   Set Alias Begin    -------------------------------
# 1. 编译函数 make
function MakeThings {
    nmake.exe $args -nologo
}
Set-Alias -Name make -Value MakeThings

# 2. 更新系统 os-update
Set-Alias -Name os-update -Value Update-Packages

# 3. 查看目录 ls & ll
function ListDirectory {
    (Get-ChildItem).Name
    Write-Host("")
}
Set-Alias -Name ls -Value ListDirectory
Set-Alias -Name ll -Value Get-ChildItem
#-------------------------------    Set Alias END     -------------------------------
```

## 参考文档

[Windows Terminal 完美配置 PowerShell 7.1](https://zhuanlan.zhihu.com/p/137595941)