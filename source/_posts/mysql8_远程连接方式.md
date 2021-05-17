---
layout: mysql8
title: MySql8 远程连接方式
date: 2021-05-06 16:59:06
tags: MySql
---

mysql8 默认配置是不开启远程连接的，这里我们需要修改 mysql 数据库的配置来主动开启

```
mysql -uroot -p
use mysql;
select host,user from user;
update user set host='%' where user=root;
// 更新数据库
flush privileges;
```

然后就可以远程登录了