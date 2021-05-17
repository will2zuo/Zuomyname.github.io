---
title: Laravel passport OAuth 认证使用
date: 2021-05-17 09:47:03
tags: Laravel
---

#### 安装扩展包
```
composer require laravel/passport
```

#### 数据库迁移
```
php artisan migrate
```

#### 安装 Passport
```
php artisan passport:install
```

#### 在用户模型类中使用 HasApiTokens Trait
```
// 在 User Model 中
use HasApiTokens;
```

#### 在 AuthServiceProvider 中注册 Passport 路由
```
public function boot()
{
    $this->registerPolicies();

    Passport::routes();// 这里增加
}
```

#### 设置 Passport 在输入 API 请求中使用
```
// 位置 => config/auth.php
'api' => [
       'driver' => 'passport', // 这里是修改的
       'provider' => 'users',
],
```

#### 从 Web 浏览器访问认证 API
```
// 先要在 Http\Kernel.php 的 $middlewareGroups 属性中新增中间件 CreateFreshApiToken

protected $middlewareGroups = [
    'web' => [
        \App\Http\Middleware\EncryptCookies::class,
        \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
        \Illuminate\Session\Middleware\StartSession::class,
        // \Illuminate\Session\Middleware\AuthenticateSession::class,
        \Illuminate\View\Middleware\ShareErrorsFromSession::class,
        \App\Http\Middleware\VerifyCsrfToken::class,
        \Illuminate\Routing\Middleware\SubstituteBindings::class,
        CreateFreshApiToken::class // 这里是增加的
    ],

    'api' => [
        'throttle:60,1',
        'bindings',
    ],
];
```

#### 在 api.php 中使用
```
Route::prefix('v1')
    ->middleware('auth:api')
    ->group(function () {
        Route::get('/user', function (Request $request) {
           return $request->user();
        });
    });
```