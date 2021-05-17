---
title: Docker 学习
date: 2021-05-06 17:06:03
tags: Docker
---

### Docker 概述

#### 1. docker 为什么会出现

- 环境不统一
- 环境配置十分麻烦，部署费时费力
- docker 的思想来源于集装箱
- docker 通过隔离机制，可以将服务器利用到极致

#### 2. docker 可以做什么

容器化技术不是模拟一个完整的操作系统

**比较 docker 和传统的虚拟技术的不同：**

- 传统虚拟机，虚拟出一个硬件，运行一个完整的操作系统，然后在这个系统上面安装运行软件
- 容器内的应用直接运行在宿主机的内容，容器没有自己的内核，也没有虚拟我们的硬件，所以轻便
- 每个容器间相互隔离，每个容器内部都有一个属于自己的文件系统，互不影响

**DevOps（开发、运维）**

- 应用更快捷的交付和部署

- 更便捷的升级和扩缩容

- 更简单的系统运维

  在容器化之后，我们的开发，测试环境都是高度的一致

- 更高效的计算资源利用

  docker 是内核级别的虚拟化，可以在一个物理机上运行很多的容器实例。服务器性能可以被压榨到极致

### Docker 安装

#### 1. Docker 的基本组成

![image-20200716140943286](/Users/zyyt/Library/Application Support/typora-user-images/image-20200716140943286.png)

**镜像（image）**：docker 镜像就好比一个模板，可以通过这个模板创建容器服务，通过镜像可以创建多个容器（最终项目运行在容器中）

**容器（container）**：利用容器技术，可以独立运行一个或者一组应用，通过镜像来创建

启动、停止、删除等基本命令（可以理解为一个简易的linux系统）

**仓库（repository）**：存放镜像的地方；分为公有仓库和私有仓库

docker hub（默认是国外的）

**安装 docker**

[安装地址]: http://note.youdao.com/s/IP1Ek5li

**docker run**的流程

1. 开始，docker 在本机寻找镜像，判断本机是否有镜像
2. 有，运行这个镜像；否，去docker hub 上下载
3. 在docker hub 上是否能找到这个镜像，找不到就返回错误
4. 下载镜像到本地，运行这个镜像

**底层原理**

docker 是怎么工作的

- docker 是一个 client-server结构的系统，docker的守护进程运行在主机，通过 socket从客户端访问
- dockerserver 接受到 docker-client的指令，执行命令

**docker 为什么比虚拟机快**

1. docker有比虚拟机更少的抽象层
2. docker 利用的是宿主机的内核，虚拟机需要 guest os
3. 新建一个容器的时候，docker 不需要像虚拟机重新加载一个操作系统内核，避免引导，虚拟机需要加载一个 guest os

### Docker 命令

1. 帮助命令

   ```shell
   docker version # 显示 docker 的版本信息
   docker info # 显示 docker 的系统信息，包括系统镜像和数量
   docker help 
   ```

2. 镜像命令

   **docker images** # 查看主机上的所有镜像

   ```shell
   [root@iZ8vbcrus31oj3y0u2w3svZ ~]# docker images
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
   hello-world         latest              bf756fb1ae65        6 months ago        13.3kB
   
   #解释
   REPOSITORY 镜像的仓库源
   TAG				 镜像的标签
   IMAGE ID	 镜像的ID
   CREATED		 镜像的创建时间
   SIZE			 镜像的大小
   
   # 可选项
   Options:
     -a 			 显示所有镜像
     -q			 只显示镜像的 ID
   ```

   **docker search** # 搜索镜像

   ```shell
   [root@iZ8vbcrus31oj3y0u2w3svZ ~]# docker search mysql
   NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
   mysql                             MySQL is a widely used, open-source relation…   9735                [OK]
   mariadb                           MariaDB is a community-developed fork of MyS…   3552                [OK]
   
   # 可选项
   --filter=start=3000 # 表示搜索收藏大于 3000 的镜像
   ```

   **docker pull** # 下载镜像

   ```shell
   # 下载镜像 docker pull 镜像名称[:tag]
   [root@iZ8vbcrus31oj3y0u2w3svZ ~]# docker pull mysql
   Using default tag: latest # 如果不写 tag，默认下载最新的 tag
   latest: Pulling from library/mysql
   8559a31e96f4: Pull complete # 分成下载，docker image 的核心，联合文件系统
   d51ce1c2e575: Pull complete
   c2344adc4858: Pull complete
   fcf3ceff18fc: Pull complete
   16da0c38dc5b: Pull complete
   b905d1797e97: Pull complete
   4b50d1c6b05c: Pull complete
   571e8a282156: Pull complete
   e7cc823c6090: Pull complete
   61161ba7d2fc: Pull complete
   74f29f825aaf: Pull complete
   d29992fd199f: Pull complete
   Digest: sha256:fe0a5b418ecf9b450d0e59062312b488d4d4ea98fc81427e3704f85154ee859c # 签名信息
   Status: Downloaded newer image for mysql:latest
   docker.io/library/mysql:latest # 真实地址 == docker pull docker.io/library/mysql:latest
   ```

   **docker rmi** # 删除镜像

   ```shell
   docker rmi -f 容器id # 按id 删除镜像
   docker rmi -f $(docker images -aq) # 删除全部镜像
   ```

3. 容器命令

   - 新建容器并启动

     ```shell
     docker run[可选参数] image
     
     # 参数说明
     -name="Name" 	容器名称，用来区分容器
     -d 						后台方式运行
     -it 					使用交互方式运行，进入容器查看内容
     -p 						指定容器的端口 -p8080:8080
     -P dap   			随机映射端口
     	
     # 测试
     [root@iZ8vbcrus31oj3y0u2w3svZ ~]# docker run -it centos
     [root@ca30f0ae3fd6 /]# ls
     bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
     ```

   - 列出所有运行的容器

     ```shell
     # 正在运行的容器
     [root@iZ8vbcrus31oj3y0u2w3svZ /]# docker ps
     CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
     # 查看之前运行过的容器
     [root@iZ8vbcrus31oj3y0u2w3svZ /]# docker ps -a
     CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                          PORTS               NAMES
     ca30f0ae3fd6        centos              "/bin/bash"         2 minutes ago       Exited (0) About a minute ago                       compassionate_engelbart
     0ec2b1b3f3be        centos              "/bin/bash"         4 minutes ago       Exited (0) 4 minutes ago                            kind_ishizaka
     b9045bad84a7        bf756fb1ae65        "/hello"            3 hours ago         Exited (0) 3 hours ago                              trusting_payne
     
     -n 显示最近创建的容器
     -q 只显示容器的编号
     ```

   - 删除容器

     ```shell
     docker rm 容器 id # 删除指定的容器，不能删除正在运行的容器，如果要强制删除 rm -f
     docker rm -f $(docker ps -aq) # 删除所有的容器
     docker ps -a -q | xargs rm # 删除所有容器
     ```

   - 退出容器

     ```shell
     exit # 停止容器并退出
     ```

   - 启动和停止容器的操作

     ```shell
     docker start 容器id 			#启动容器
     docker restart 容器id			#重启容器
     docker stop 容器id				#停止当前正在运行的容器
     docker kill 容器id				# 强制停止的当前容器
     ```

4. 常用的其他命令

   - 后台启动

     ```shell
     docker run -d 镜像名称
     
     # 常见的坑，docker 容器使用后台运行，就必须要有个前台进程，docker 发现没有应用，就会自动停止
     # nginx 容器启动后，发现自己没有提供服务，就会立刻停止，就是没有程序了
     ```

   - 查看日志

     ```shell
     docker logs -f -t --tail 10 容器id #显示指定行数的日志
     docker logs -f -t 容器id # 显示全部的日志
     ```

   - 查看容器中的进程信息

     ```shell
     docker top 容器id 
     ```

   - 查看元数据

     ```shell
     docker inspect 容器id
     ```

   - 进入当前正在运行的容器

     ```shell
     # 我们容器通常是使用后台方式运行的，现在需要进入容器
     # 命令
     docker exec 容器id # 进入容器后，开启一个新的终端，可以在里面操作（常用）
     docker arrach 容器 id #进入容器正在执行的终端，不会启动新的进程
     ```

   - 从容器中拷贝文件到主机上

     ```shell
     docker cp 容器id:容器内路径 目的地路径
     
     # 测试
     [root@iZ8vbcrus31oj3y0u2w3svZ home]# ll
     总用量 4
     drwx------ 2 www www 4096 3月  24 22:04 www
     [root@iZ8vbcrus31oj3y0u2w3svZ home]# docker ps -a
     CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
     [root@iZ8vbcrus31oj3y0u2w3svZ home]# docker run -it centos
     [root@753e672d39fd /]# cd /home
     [root@753e672d39fd home]# touch test.java
     [root@753e672d39fd home]# exit
     exit
     [root@iZ8vbcrus31oj3y0u2w3svZ home]# docker ps -a
     CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
     753e672d39fd        centos              "/bin/bash"         23 seconds ago      Exited (0) 7 seconds ago                       relaxed_gould
     [root@iZ8vbcrus31oj3y0u2w3svZ home]# docker cp 753e672d39fd:/home/test.java /home
     [root@iZ8vbcrus31oj3y0u2w3svZ home]# ll
     总用量 4
     -rw-r--r-- 1 root root    0 7月  16 18:15 test.java
     drwx------ 2 www  www  4096 3月  24 22:04 www
     [root@iZ8vbcrus31oj3y0u2w3svZ home]#
     ```

   - 常用命令小结

     ```shell
     attach 			# 当前 shell 下，attach 链接指定运行镜像
     build 			# 通过 Dockerfile 定制镜像
     commit 			# 提交当前容器为新的镜像
     cp 					# 从容器中拷贝指定文件或者目录到宿主机中
     create 			# 创建一个新的容器，同 run ，但不启动容器
     diff 				# 查看 docker 容器的变化
     events 			# 从docker 服务获取容器实时时间
     exec 				# 在已存在的容器上运行命令
     export 			# 到处容器的内容流作为一个 tar 归档文件
     history 		# 展示一个镜像形成历史
     images 			# 列出系统当前镜像
     import 			# 从 tar 包中的内容创建一个新的而文件系统镜像
     info 				# 显示系统相关信息
     inspect 		# 查看容器详细信息
     kill 				# kill 指定 docker 容器
     load 				# 从一个 tar 包中加载一个镜像
     login 			# 注册或者登陆一个 docker 源服务器
     logout 			# 从当前源登出
     logs 				# 查看当前容器日志信息
     port 				# 查看映射端口对应的容器内部源端口
     pause 			# 暂停容器
     ps 					# 列出容器列表
     pull 				# 从docker 镜像源服务器拉去指定镜像
     push 				# 推送指定镜像或者库镜像只 docker源服务器
     restart 		# 重启运行的容器
     rm 					# 删除一个或者多个容器
     rmi 				# 移除一个或者多个镜像
     run 				# 创建一个新的容器并运行一个命令
     save 				# 保存一个镜像为一个 tar 包（对应 load）
     search 			# 在docker hub 中搜索镜像
     start 			# 启动容器
     stop 				# 停止容器
     tag 				# 给源中镜像打标签
     top 				# 查看容器中运行的进程
     unpause 		# 取消暂停容器
     version 		# 查看 docker 版本信息
     wait 				# 截取容器停止时退出状态值
     ```

   - 安装 nginx

     ```shell
     # 1. 下载nginx镜像
     docker search nginx
     docker pull nginx
     
     # 2. 运行容器
     docker run -d --name nginx01 -p 3344:80 nginx
     
     # 3. 进入容器
     docker exec -it nginx01 /bin/bash
     ```

   - 安装 es

     ```shell
     docker run -d --name elasticsearch  -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
     
     #-e ES_JAVA_OPTS="-Xms64m -Xmx512m" 限制内存大小
     
     [root@iZ8vbcrus31oj3y0u2w3svZ ~]# curl localhost:9200
     {
       "name" : "ff85ed481a48",
       "cluster_name" : "docker-cluster",
       "cluster_uuid" : "FVGcHuJJTlaOr97RmCdJEQ",
       "version" : {
         "number" : "7.6.2",
         "build_flavor" : "default",
         "build_type" : "docker",
         "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
         "build_date" : "2020-03-26T06:34:37.794943Z",
         "build_snapshot" : false,
         "lucene_version" : "8.4.0",
         "minimum_wire_compatibility_version" : "6.8.0",
         "minimum_index_compatibility_version" : "6.0.0-beta1"
       },
       "tagline" : "You Know, for Search"
     }
     
     # 查看内存使用情况
     docker stats 容器id
     ```

   - 使用 kibana 连接 elasticsearch

     通过 linux 内网地址来进行连接；涉及到 docker 的网络原理

5. 可视化

   - portainer ，docker 的图形化管理工具，提供一个后台面板供我们操作

### Docker 镜像

**镜像是什么**

所有的应用，直接打包 docker 镜像，就可以直接跑起来

**如何得到镜像：**

1. 从远程仓库下载
2. 自己制作
3. 别人给

**docker 镜像加载原理** 

联合文件系统

分层下载

#### Commit 镜像

```shell
docker commit 提交容器成为一个新的副本
docker commit -m='提交的描述信息' -a='作者' 容器 id 目标镜像名称;[tag]
```

### 容器数据卷

**什么是容器数据卷**：需要数据可以持久化，容器之间可以有一个数据共享的技术，docker 产生的数据，同步到本地，就是将容器内的目录挂在在 Linux 上面

容器的持久化和同步操作，容器间也是可以数据共享的



**使用数据卷**

1. 使用命令来挂载

   ```shell
   docker run -it -v 主机目录：容器内目录
   ```

2. 具名挂载和匿名挂载

   ```shell
   # 匿名挂载 -v 的时候只写了容器内的路径
   -v 容器内录路径
   docker rn -d -p -v /etc/nginx nginx
   
   # 查看所有卷的情况
   docker volume ls
   
   # 具名挂载
   docker rn -d -p -v jump-nginx:/etc/nginx nginx
   
   [root@iZ8vbcrus31oj3y0u2w3svZ ~]# docker volume ls
   DRIVER              VOLUME NAME
   local               jump-nginx
   
   # 查看卷详细信息
   docker volume inspect jump-nginx
   
   [root@iZ8vbcrus31oj3y0u2w3svZ ~]# docker volume inspect jump-nginx
   [
       {
           "CreatedAt": "2020-07-17T15:52:03+08:00",
           "Driver": "local",
           "Labels": null,
           "Mountpoint": "/var/lib/docker/volumes/jump-nginx/_data",
           "Name": "jump-nginx",
           "Options": null,
           "Scope": "local"
       }
   ]
   
   # 指定路径挂载
   docker rn -d -p -v /宿主机地址:/etc/nginx nginx
   
   # 在没有指定目录情况下，默认路径在 /var/lib/docker/volumes/xxx/_data
   
   docker rn -d -p -v jump-nginx:/etc/nginx:ro nginx
   docker rn -d -p -v jump-nginx:/etc/nginx:rw nginx
   
   # ro rw 改变读写权限
   ro readonly 只读，说明这个路径只能通过宿主机来操作，在容器内部无法操作
   rw readwrite 可读可写
   
   # 一旦设置了容器的权限，容器对我们挂载出来的内容就有限定了
   ```

### Docker File

就是用来构建docker 镜像的构建文件，命令脚本，通过这个脚本可以生成一个镜像，脚本就是一个个的命令，每个命令都是一层

```shell
FROM centos

VOLUME ["volume01", "volume02"]

CMD echo "++++++ end +++++"
CMD /bin/bash
```

```shell
docker build dockerfile
```

#### Dockerfile 的构建过程

基础知识：

1. 每个保留关键字（指令）都必须是大写字母
2. 执行从上到小顺序执行
3. #表示注释
4. 每个指令都会创建提交一个新的镜像层，并提交

dockerfile 是面向开发的，我们以后要发布项目，做镜像，就需要编写dockerfile 文件，这个文件十分简单

docker 镜像逐渐成为企业交付的标准，必须要掌握

**步骤：**

Dockerfile：构建文件，定义了一切的步骤，源代码

DockerImgaes：通过dockerfile构建生成的镜像，最终发布和运行的产品

Docker容器：容器就是镜像运行起来供服务的



#### Dockerfile 的一些指令

![image-20200720134234025](/Users/zyyt/Library/Application Support/typora-user-images/image-20200720134234025.png)

![image-20200720134302424](/Users/zyyt/Library/Application Support/typora-user-images/image-20200720134302424.png)

```shell
FROM 					# 基础镜像，一切从这里开始构建
MAINTAINER 		# 镜像是谁写的，姓名+邮箱
RUN 					# 镜像构建的要运行的命令
ADD 					# 步骤，tomcat 镜像，这个 tomcat 的压缩包，添加内容
WORDDIR 			# 镜像的工作目录
VOLUME 				# 挂载目录
EXPOSE 				# 暴露端口有配置 -p
CMD 					# 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT 		# 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD 			# 当构建一个被继承 dockerfile，这个时候就会运行 ONBUILD的指定 ，出发指令
COPY 					# 将我们的文件拷贝到镜像中
ENV 					# 构建的时候设置环境变量
```

> 创建一个自己的 centos

```shell
[root@iZ8vbcrus31oj3y0u2w3svZ dockerfile]# cat mydockerfile-centos
FROM centos

MAINTAINER will.zuo<17683115201@163.com>

ENV MYPATH /usr/local
WORDDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "+++++++ end ++++++++"
CMD /bin/bash

# 运行构建文件
docker build -f mydockerfile-centos -t mycentos .

# 构建成功返回
Successfully built c54058230aa3
Successfully tagged mycentos:latest

# 查看是否存在镜像
[root@iZ8vbcrus31oj3y0u2w3svZ dockerfile]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mycentos            latest              c54058230aa3        10 seconds ago      295MB
will/centos         1.0                 0237c263acb8        2 days ago          215MB
will.tomcat         1.0                 bccad863bff2        2 days ago          652MB
tomcat              9.0                 df72227b40e1        3 days ago          647MB
tomcat              latest              df72227b40e1        3 days ago          647MB
mysql               5.7                 d05c76dbbfcf        6 days ago          448MB
nginx               latest              0901fa9da894        9 days ago          132MB
centos              latest              831691599b88        4 weeks ago         215MB
elasticsearch       7.6.2               f29a1ee41030        3 months ago        791MB

# 测试运行
```

对比：之前原生的 centos

我们之后增加的

![image-20200720140737677](/Users/zyyt/Library/Application Support/typora-user-images/image-20200720140737677.png)



> CMD 和 ENTRYPOINT 的区别

1. cmd 后面的命令会把前面命令覆盖
2. entrupoint 命令会直接在后面追加

**tomcat 镜像**

1. 准备好镜像文件， tomcat 压缩包，jdk压缩包
2. 编写 dockerfile 文件，官方命名 Dockerfile， build 会自动寻找这个文件，不需要 -f 指定文件



#### 发布镜像

```shell
docker login -u -p

docker push 镜像名称
```

#### 发布到阿里云

1. 创建命名空间
2. 创建容器镜像
3. 本地仓库
4. 参考官方地址

### 数据卷容器

```shell
--volumes-from 继承的容器name
```



容器之间配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用为止，一旦持久化到本地，这个时候本地数据就不会被删除

### Docker 网络

#### 理解 docker0

```shell
# docker 是怎么处理容器网络访问的

# 创建一个新的 tomcat 容器
docker run -d -P --name tomcat01 tomcat

# 查看容器的内部网络地址 ，容器启动会得到  eth0@if55 ip 地址，docker分配
docker exec tomcat01 ip addr

[root@iZ8vbcrus31oj3y0u2w3svZ ~]# docker exec tomcat01 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
54: eth0@if55: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
       
# linux 可以ping通 docker 容器内部
```

>原理

1. 我们每启动一个 docker 容器，docker 都会给 docker 容器分配一个 ip，只要电脑上安装了 docker ，都会有docker0网卡，桥接模式，使用的技术是 evth-pair 技术

   ```shell
   [root@iZ8vbcrus31oj3y0u2w3svZ ~]# ip addr
   1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
       link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
       inet 127.0.0.1/8 scope host lo
          valid_lft forever preferred_lft forever
   2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
       link/ether 00:16:3e:0d:8c:bf brd ff:ff:ff:ff:ff:ff
       inet 172.26.187.10/20 brd 172.26.191.255 scope global dynamic eth0
          valid_lft 305183217sec preferred_lft 305183217sec
   3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
       link/ether 02:42:ca:12:cd:e5 brd ff:ff:ff:ff:ff:ff
       inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
          valid_lft forever preferred_lft forever
   55: veth2fece64@if54: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default
       link/ether 4e:f4:f1:c4:64:24 brd ff:ff:ff:ff:ff:ff link-netnsid 0
   
   ```

   ```shell
   # 我们发现容器带来的网卡，都是一对对的
   # evth-pair 就是一对虚拟设备接口，他们都是成对出现的，一段连接协议，一段彼此相连
   # 正因为有这个特性，eth-pair 充当一个桥梁，连接各种虚拟网络设备
   # 
   ```

2. 启动另一个 tomcat 容器

3. 我们来测试 tomcat01 和 tomcat02 是否能ping通

   ```shell
   [root@iZ8vbcrus31oj3y0u2w3svZ ~]# docker exec -it tomcat02 ping 172.17.0.2
   PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
   64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.128 ms
   64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.106 ms
   64 bytes from 172.17.0.2: icmp_seq=3 ttl=64 time=0.110 ms
   ^C
   --- 172.17.0.2 ping statistics ---
   3 packets transmitted, 3 received, 0% packet loss, time 1000ms
   rtt min/avg/max/mdev = 0.106/0.114/0.128/0.015 ms
   
   # 结论，容器之间是可以相互ping通的，容器都是共用的一个路由器，所有容器不指定网络情况下，都是 docker0路由的，docker 会给我们的容器分批一个默认的可用 ip
   ```

> 小结

docker 使用的是linux的桥接，宿主机中是一个docker

docker 中所有的网络接口都是虚拟的，虚拟转发效率高

只要容器删除，对应网桥一堆就没了



### --link

```shell
docker run -d -P --name tomcat03 --link tomcat02 tomcat

# 测试
[root@iZ8vbcrus31oj3y0u2w3svZ ~]# docker exec tomcat03 ping tomcat02
PING tomcat02 (172.17.0.3) 56(84) bytes of data.
64 bytes from tomcat02 (172.17.0.3): icmp_seq=1 ttl=64 time=0.129 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=2 ttl=64 time=0.108 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=3 ttl=64 time=0.110 ms

```

探究：inspect

--link ：就是我们在 hosts 匹配里增加了 

```shell
[root@iZ8vbcrus31oj3y0u2w3svZ ~]# docker exec tomcat03 cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.17.0.3	tomcat02 1a989e21dfaa
172.17.0.4	51220b767658
```



### 自定义网路

```shell
# 查看所有 docker 网络

[root@iZ8vbcrus31oj3y0u2w3svZ ~]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
0c75c9a7bf94        bridge              bridge              local
ffecf92be639        host                host                local
9b9c16be0510        none                null                local

```

网络模式：

1. bridge：桥接（默认，自己创建也是使用桥接）
2. none：不配置网络
3. host：和宿主机共享网络
4. container：容器网络连通（少用，局限性很大）

测试

```shell
# 自定义一个网络
docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet

# 通过自定义网络启动容器
docker run -d -P --network mynet --name tomcat01 tomcat

# 自定义网络可以通过容器名称来进行网络连通

```

好处：

1. 不同的集群使用不容的网络，保证集群是安全和健康的



#### 网络连通

![image-20200720174733412](/Users/zyyt/Library/Application Support/typora-user-images/image-20200720174733412.png)

```shell
# 将容器连接到自定义网络
docker network connect mynet tomcat03

# 将 tomcat03 这个容器放入到 mynet 这个网络中
[root@iZ8vbcrus31oj3y0u2w3svZ ~]# docker inspect mynet
[
    {
        "Name": "mynet",
        "Id": "a327bd0d4d5374b14f3abfa95a3a32e164b9305131a99e6847966386285b25c4",
        "Created": "2020-07-20T17:41:12.217307542+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "01e420d4390278b31030a060fd8aff77d749cbb6794ab085f4d4d9ecf4896613": {
                "Name": "tomcat03",
                "EndpointID": "ab8c1ac0018293aa2f4e28ab94e157e15ec295b610da74dece83535a3c788610",
                "MacAddress": "02:42:c0:a8:00:04",
                "IPv4Address": "192.168.0.4/16",
                "IPv6Address": ""
            },
            "9765811f0c13b92fe3aa7e8e13f1ef8c32d124a3f02d2c2183a61154f11f7c80": {
                "Name": "tomcat02",
                "EndpointID": "c231ffef794e3034ca6965f65c67c976475d60ab5f8a60baa58c3f7db61ae7a6",
                "MacAddress": "02:42:c0:a8:00:03",
                "IPv4Address": "192.168.0.3/16",
                "IPv6Address": ""
            },
            "aaafbaf9706abb26de1fc30796aec29611343495e4c5422e3c38ed249ddd2e09": {
                "Name": "tomcat01",
                "EndpointID": "d1defe4f0bf4ea1b193413c08fa7a265c68b6c8b0a4e4ea74ba6e1296a9d983b",
                "MacAddress": "02:42:c0:a8:00:02",
                "IPv4Address": "192.168.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

结论：

1. 假设要跨网络操作别人，就需要使用 docker network connect
2. 

### 查看 docker 容器 ip 地址
```
docker inspect 容器名称或 id
```

