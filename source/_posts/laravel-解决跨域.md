---
layout: source/_posts
title: Laravel 解决跨域
date: 2021-04-30 15:42:45
tags: Laravel
category: PHP
---

#### 1. 安装扩展
```
composer require barryvdh/laravel-cors
```

#### 2. 发布配置文件
```
php artisan vendor:publish --provider="Barryvdh\Cors\ServiceProvider"
```

#### 3. 使用
```
如果需要全局使用，可以在 app/Http/Kernel.php 的 $middleware 中增加 \Barryvdh\Cors\HandleCors::class，
```