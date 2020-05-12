---
title: Laravel API 服务开发笔记
date: 2020-01-16 10:38:14
categories: Laravel
---



# API 基类

基类功能在于所有的 Api 控制器都继承这个基类。

```bash
$ php artisan make:controller Api/Controller
```

> 新增命名空间 `Api`，接口相关控制器同一放置，后续可根据需要在目录下增加 `V1`、`V2` 等目录，用来区分不同版本接口。

*app/Http/Controllers/Api/Controller.php*

```php
<?php

namespace App\Http\Controllers\Api;

use App\Http\Controllers\Controller as BaseController;
use Illuminate\Http\Request;

class Controller extends BaseController
{
}
```

> 通过 artisan 创建的控制器会自动继承 App\Http\Controllers\Controller，直接删除即可。

# API 路由

*routes/api.php*

```php
Route::prefix('v1')->namespace('Api')->name('api.v1.')->group(function() {
});
```

- `namespace('Api')`：自动到 `Api` 下找对应控制器。
- `name('api.v1.')`：自动给命名路由加前缀 `api.v1.`。

# API 验证

功能及备注同 API 基类。

```bash
$ php artisan make:request Api/FormRequest
```

*app/Http/Requests/Api/FormRequest.php*

```php
<?php

namespace App\Http\Requests\Api;

use Illuminate\Foundation\Http\FormRequest as BaseFormRequest;

class FormRequest extends BaseFormRequest
{
    public function authorize()
    {
        return true;
    }
}
```

# 响应格式

默认情况下，使用 Laravel Recource 返回的数据会包裹一层 `data`，当有数据嵌套时，数据嵌套的层数会比较多，可以选择去除：

*app/Providers/AppServiceProvider.php*

```php
<?php

namespace App\Providers;

use Illuminate\Http\Resources\Json\Resource;

class AppServiceProvider extends ServiceProvider
{
    public function boot()
    {
        Resource::withoutWrapping();
    }
}
```

# 节流处理

Laravel 提供了 `ThrottleRequests` 中间件用来限制访问。

不同的接口对调用频率可能会有不同的限制，可以通过在配置文件区分后在路由中使用：

*config/api.php*

```php
<?php

return [

    /*
    |--------------------------------------------------------------------------
    | 接口频率限制
    |--------------------------------------------------------------------------
    */

    'rate_limits' => [
        // 访问 次/分
        'access' => env('RATE_LIMITS', '60,1'),
        // 登录 次/分
        'sign' => env('SIGN_RATE_LIMITS', '10,1'),
    ]
];
```

*routes/api.php*

```php
Route::middleware('throttle:' . config('api.rate_limits.sign'))
    ->group(function() {});

Route::middleware('throttle:' . config('api.rate_limits.access'))
    ->group(function() {});
```

