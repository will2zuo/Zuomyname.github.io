---
title: CentOs 7 下安装 wkhtmltopdf
date: 2021-05-06 17:08:54
tags: wkhtmltopdf
---

### 两种方式安装，其中方法一比较顺利

#### 方法一
1. 安装依赖
```
yum install -y fontconfig libX11 libXext libXrender libjpeg libpng xorg-x11-fonts-75dpi xorg-x11-fonts-Type1
```
2. 下载 wkhtmltopdf
```
wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox-0.12.6-1.centos7.x86_64.rpm
```
3. 解压 wkhtmltopdf
```
rpm -ivh wkhtmltox-0.12.6-1.centos7.x86_64.rpm
```
4. 查看 wkhtmltopdf
```
whereis wkhtmltopdf
```

##### 方法二：在 laravel 项目中使用 composer 安装

1. composer 安装
```
$ composer require h4cc/wkhtmltopdf-amd64 0.12.x
$ composer require h4cc/wkhtmltoimage-amd64 0.12.x
```

2. 接下来将安装好的 wkhtmltopdf 复制到 Linux 系统可执行命令的目录中
```
cp vendor/h4cc/wkhtmltoimage-amd64/bin/wkhtmltoimage-amd64 /usr/local/bin/
cp vendor/h4cc/wkhtmltopdf-amd64/bin/wkhtmltopdf-amd64 /usr/local/bin/
//并使其可执行：
chmod +x /usr/local/bin/wkhtmltoimage-amd64 
chmod +x /usr/local/bin/wkhtmltopdf-amd64
```

#### 可能存在的问题
1. 中文字体无法显示
```
// 下载 windows 宋体字体，将 simsunbd.ttf 文件导入到 centos 系统 /usr/share/fonts/chinese/TrueType 文件目录下
```
