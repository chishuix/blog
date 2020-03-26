---
title: "Laravel Api 服务器搭建笔记"
date: 2020-03-25T14:36:51+08:00
draft: true
recommend: false

tags: ['Laravel', 'API']
categories: ['Programing']
hiddenFromHomePage: true

featuredImage: ""
toc: true
---

<!--more-->

## 准备开始

### 创建项目

选择使用 Laravel 的 LTS 版本 6.* 。

```bash
$ composer create-project laravel/laravel laravel-shop --prefer-dist "6.*"
```

### 初始化

配置 *.env* 中项目基本参数及数据库信息。

```.env
APP_NAME=ELINE
APP_URL=http://eline.test
DB_DATABASE=eline
DB_USERNAME=homestead
DB_PASSWORD=secret
```

### HOSTS

修改 */etc/hosts* 将测试域名指向开发 IP。

```hosts
192.168.10.10 eline.test
```

### 辅助函数

创建辅助函数 *app/helpers.php*, 通过 *composer.json* 文件自动加载。

```json
"autoload": {
    "files": [
        "app/helpers.php"
    ]
},
```

刷新自动加载。

```bash
$ composer dump-autoload
```

## 路由设计

### 路由特性

使用 `API Resources` 格式化资源。

使用 `Restful HTTP API`。

使用 `Accept` 头和 `URL` 进行版本切换。

### 版本控制

所有 API 相关路由都需要注册到 *routes/api.php* 中，其会添加默认前缀 `api`。

通过 `prefix` 添加前缀来控制版本。

通过 `name` 统一版本中命名路由名称。

*routes/api.php*

```php
Route::prefix('v1')->name('api.v1.')->group(function () {
    // http://eline.test/api/v1/version
    Route::get('version', function () {
        return 'api v1';
    })->name('version');
});

Route::prefix('v2')->name('api.v2.')->group(function () {
    // http://eline.test/api/v2/version
    Route::get('version', function () {
        return 'api v2';
    })->name('version');
});
```

## 参考文档

[L03 Laravel 教程 - 实战构架 API 服务器 ( Laravel 6.x )](https://learnku.com/courses/laravel-advance-training/6.x)

[Github 的 Restful HTTP API 设计分解](https://learnku.com/courses/laravel-advance-training/6.x/follow-github-to-learn-restful-http-api-design/5697)

