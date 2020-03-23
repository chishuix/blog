# 使用 Homestead 搭建 Laravel 开发环境


<!--more-->

## 准备开始

安装 Homestead 需要先安装 VirtualBox 、VMWare 之类的虚拟机环境以及 Vagrant。

### Windows

Windows 下安装直接下载安装包一把梭就行了，但是安装时有个顺序，需要先安装 [VirtualBox](https://www.virtualbox.org/wiki/Downloads) 再安装 [Vagrant](https://www.vagrantup.com/downloads.html)。

### Manjaro

Manjaro 下安装需要先查一下 Linux Kernel 版本，安装对应版本的 VirtualBox。

```bash
$ uname -r # 查看内核版本
$ yay -S virtualbox # 安装相应内核版本
$ yay -S vagrant
```

安装完最好重启一下系统。

## 安装 Homestead Vagrant Box

### 下载

如果网速可以的话，通过下面的命令就可以了。

```bash
$ vagrant box add laravel/homestead
```

但是，一般是不可以的，你懂的，这时候可通过上面命令的输出文本得到下载地址，使用下载工具下载就行了，**留意一下下载的是哪个版本**。

### 导入

通过命令行直接下载的无需这一步，使用下载工具下载的需要先将文件重命名为 *virtualbox.box*，然后放到一个**非中文**目录内，并在目录内新建文件 *metadata.json* 填入下面内容，`version` 属性要修改成上一步的版本。

```json
{
    "name": "laravel/homestead",
    "versions": 
    [
        {
            "version": "9.3.0",
            "providers": [
                {
                  "name": "virtualbox",
                  "url": "virtualbox.box"
                }
            ]
        }
    ]
} 
```

## 安装 Homestead 

克隆 Homestead 仓库到本地，并切换到当前版本，注意，`master` 并不一定是最新版本，可以先到 [Release](https://github.com/laravel/homestead/releases) 页面看一下最新的版本号，然后 `git checkout` 切换过去，也可以直接切换到 `release` 版本，它一直是最新版本。

```bash
$ git clone https://github.com/laravel/homestead.git ~/Homestead
$ cd ~/Homestead
$ git checkout release
```

仓库拉下来之后进入目录然后执行初始化命令。

```bash
$ bash init.sh
```

## 参数配置

*～/Homestead/Homestead.yaml*

```yaml
---
ip: "192.168.10.10"
memory: 4096
cpus: 2
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa
    - ~/.ssh/id_rsa.pub
    
folders:
    - map: ~/Websites/eline
      to: /home/vagrant/eline
      type: "rsync"
      options:
          rsync__args: ["--verbose", "--archive", "--delete", "-zz"]
          rsync__exclude: ["node_modules"]

sites:
    - map: eline.test
      to: /home/vagrant/eline/public

databases:
    - eline

features:
    - mariadb: false
    - ohmyzsh: false
    - webdriver: false

# ports:
#     - send: 50000
#       to: 5000
#     - send: 7777
#       to: 777
#       protocol: udp

# variables:
#     - key: APP_ENV
#       value: local
```

## 初始化配置

### 启动登录

```bash
$ vagrant up && vagrant ssh
```

### 配置国内镜像

```bash
$ npm config set registry https://registry.npm.taobao.org
$ yarn config set registry https://registry.npm.taobao.org
$ composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
```

### Git 配置

**用户信息**

```bash
$ git config --global user.name "name"
$ git config --global user.email "email@example.com"
$ git config --global push.default simple
```

**添加 `ssh-agent`**

```bash
$ eval "$(ssh-agent -s)"
$ ssh-add ~/.ssh/id_rsa
```

## 使用方法

### 启动

```bash
$ vagrant up
```

### 登录

```bash
$ vagrant ssh
$ ssh vagrant@127.0.0.1	-p 2222 # 如果上面登陆失败, 还可以通过此方式
```

### 重启

```bash
$ vagrant reload
```

### 关闭

```bash
$ vagrant halt
```

### 刷新配置

```bash
$ vagrant reload --provision
```

## 局域网访问

*～/Homestead/Homestead.yaml*

```yaml
# 修改 ip 为当前局域网同网段
ip: "192.168.0.77"
```

*~/Homestead/scripts/homestead.rb*

```yaml
# 修改 private_network 参数为 public_network
# Configure A Private Network IP
...
config.vm.network :public_network, ip: settings['ip'] ||= '192.168.10.10'
...
```

## 性能优化 (Windows)

### NFS 模式

**安装插件**

```bash
$ vagrant plugin install vagrant-winnfsd
```

*~/Homestead/scripts/homestead.rb*

```rb
# 将下面内容
# Register All Of The Configured Shared Folders
if settings.include? 'folders'
...
end

# 替换为
# Register All Of The Configured Shared Folders
if settings.include? 'folders'
    settings["folders"].sort! { |a,b| a["map"].length <=> b["map"].length }

    settings["folders"].each do |folder|
        config.vm.synced_folder folder["map"], folder["to"], 
        id: folder["map"],
        :nfs => true,
        :mount_options => ['nolock,vers=3,udp,noatime']
    end
end
```

*～/Homestead/Homestead.yaml*

```yaml
folders:
    - map: ~/Code
      to: /home/vagrant/Code
      # 添加这个
      type: nfs
```

### SMB 模式

*～/Homestead/Vagrantfile*

```yaml
...
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
#SMB
config.vm.synced_folder ".", "/vagrant", type: "smb",
    smb_username: "",
    smb_password: "",
    owner: "vagrant",
    group: "root"
...
```

*～/Homestead/Homestead.yaml*

```yaml
...
folders:
    - map: ~/Code
      to: /home/vagrant/Code
      # 添加这个
      type: smb
...
```

*/etc/nginx/nginx.conf*

```conf
# 需要 sudo 权限
sendfile: off
```

## 科学上网

科学上网的客户端需要开启"允许来自局域网的链接"。

**安装插件**

```bash
$ vagrant plugin install vagrant-proxyconf
```

*～/Homestead/Vagrantfile*

```yaml
if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.http = "http://127.0.0.1:1080/"
    config.proxy.https = "http://127.0.0.1:1080/"
    config.proxy.no_proxy = "localhost,127.0.0.1"
end
```

测试连接

```bash
$ curl -v  "www.google.com"
```

## 容器管理

### 查看列表

```bash
$ vagrant box list
```

### 删除容器

```bash
$ vagrant box remove boxname
```

### 删除虚拟机

```bash
$ vagrant destroy
```

## 参考文档

[Laravel Homestead](https://laravel.com/docs/6.x/homestead#optional-software)

[开发环境搭建 - Windows](https://learnku.com/docs/laravel-development-environment/5.8/development-environment-windows/4373)

[如何升级 Homestead 盒子？](https://learnku.com/docs/laravel-development-environment/5.8/upgrade-homestead-box/4376)

[Vagrant入门](http://www.cnblogs.com/davenkin/p/vagrant-virtualbox.html)

[Laravel-Homestead 环境搭建的各种坑](https://laravel-china.org/topics/2990/laravel-homestead-environment-to-build-a-variety-of-pits)

[Homestead 安装问题汇总](https://laravel-china.org/topics/4796/ssh-auth-method-private-key-solution-summary-when-homestead-starts)
