---
title: 连接 docker mysql 容器
date: 2021-05-06 17:01:53
tags: Docker
---

#### 创建新 mysql 容器
```
docker pull mysql
```

#### 启动 mysql 容器
```
sudo docker run -p 63306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=root -d mysql
// 将容器的 3306 端口映射到主机的 63306 端口，密码为root
```

#### 进入mysql容器
```
docker exec -it mysql bash
```

#### 在主机连接容器
```
mysql -uroot -proot -P63306 -h127.0.0.1
```


