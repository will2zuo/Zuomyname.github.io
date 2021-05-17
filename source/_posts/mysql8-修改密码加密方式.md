---
title: MySql8 修改密码加密方式
date: 2021-05-06 16:30:08
tags: MySql
---

#### 进入my.cnf 修改配置文件
```
vim /etc/my.cnf
[mysqld]
default_authentication_plugin=mysql_native_password
```

#### 进入 mysql 修改配置
```
ALTER USER 'root'@'localhost' IDENTIFIED BY '密码' PASSWORD EXPIRE NEVER;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '密码'; 
FLUSH PRIVILEGES;
```

#### 查看加密方式是否被改变
```
use mysql;
select user,plugin from user;
// 如果出现两个root用户且加密方式不对，删除不是mysql_native_password的一个
```

