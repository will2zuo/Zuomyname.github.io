---
title: Composer required 内存溢出解决办法
date: 2021-05-06 17:07:53
tags: Composer
---

#### 在使用 composer 过程中出现内存溢出错误，只需要暂时将内存设置为没有限制
```
COMPOSER_MEMORY_LIMIT=-1 composer required(install、update)
```