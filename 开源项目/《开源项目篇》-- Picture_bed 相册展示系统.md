---
layout: 《开源项目篇》-- Picture_bed 相册展示系统
title:   《开源项目篇》-- Picture_bed 相册展示系统
date:  2019-12-26 16:33:22
categories: "开源项目"
abbrlink: 018a
tags: 
- PHP
- Laravel
- 开源项目
- Github
---

<img src="https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/f897b405e0b1955aa58f5bd3ef9be31e.png.png" style="width:900px;height:400px" />

# 导言
Picture_bed 是基于Laravel5.6 + Bootstrap4.0开发的一个小型相册展示系统，编写该项目的意义在于可以更加可视化的去管理自己储存在云上的图片，同时后台也支持批量下载，批量上传功能。该系统只是前端展示，其后台使用的是[vue-admin-template](http://www.linyiyuan.top/[https://github.com/linyiyuan/vue-admin-template](https://github.com/linyiyuan/vue-admin-template)) + [laravel-admin-template](https://github.com/linyiyuan/laravel-admin-template) 构建的crm系统。

<!--less-->

## 项目部署

首先你得确保你服务器符合以下要求：

*   PHP >= 7.1.3
*   OpenSSL PHP扩展
*   PDO PHP扩展，注意需要php_mysql
*   Mbstring PHP扩展
*   Tokenizer PHP扩展
*   XML PHP扩展

项目地址：

> [https://github.com/linyiyuan/picture_bed.git](https://github.com/linyiyuan/picture_bed.git)

项目演示地址：

> [http://album.linyiyuan.top](http://album.linyiyuan.top/)

**第1步：克隆代码**

```bash
git clone https://github.com/linyiyuan/picture_bed.git
```

**第2步：安装composer包**

```bash
composer install
```

**第3步：配置文件**

1、在项目中找到`.env.example`文件，该文件作为项目的全局配置文件，在部署时需要复制成`.env`，执行以下命令

```bash
cp -f .env.example ./.env
```


2、根据.env文件修改各配置项，如果.env文件中没有存在key值则运行命令：

```bash
php artisan key:generate
```


3、配置stroage bootstrap 可写

```bash
chmod -R 777 stroage bootstrap
```


**第4步：初始化数据库**

在根路径上执行以下命令来实现初始化数据库结构。注意执行该命令前请检查项目是否已依赖`doctrine/dbal`

```bash
php artisan migrate
```


至此项目部署已经基本以完成，配置相应apache或者nginx 域名指向即可

## 项目更新

1.  增加了密码相册，允许在相册上设置问题访问，回答问题访问成功后自动保存访问信息，下次访问不需要进行密码认证，有效期为60分钟。
2.  增加了_live2d_模型，可以切换各种角色，目前有10中角色的切换，里面配置了一部分快捷键。

## 项目展示

![相册首页](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/06e45d780c22b1818f38f35ffe1b9d10.jpg)

![相册列表](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/f897b405e0b1955aa58f5bd3ef9be31e.png.png "相册列表")

[![图片列表页](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/c37a55e8586dbc059550f6960c7f009a.jpg)](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/c37a55e8586dbc059550f6960c7f009a.jpg "图片列表页")

[![图片列表页](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/81ca6c9a2f40659b1e650f47530670fe.jpg)](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/81ca6c9a2f40659b1e650f47530670fe.jpg "图片列表页")

[![密码相册展示](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/dd5f76877f84078b0f49d7236b28bf1d.jpg)](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/dd5f76877f84078b0f49d7236b28bf1d.jpg "密码相册展示")

[![图片详情页](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/1f8cb5ba592b3a230fc4e5801775bcc7.jpg)](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/1f8cb5ba592b3a230fc4e5801775bcc7.jpg "图片详情页")

[![个人信息页](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/dfae36e2bea944e7d6a8a21ae84f8f7a.png.png)](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/dfae36e2bea944e7d6a8a21ae84f8f7a.png.png "个人信息页")

[![后台展示](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/931ed5c6e3ac7420c6d4142b993b0ae6.jpg)](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/931ed5c6e3ac7420c6d4142b993b0ae6.jpg "后台展示")

[![后台上传页面](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/293f0596c4900cfa1ee5669d3b1d479e.jpg)](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_9/293f0596c4900cfa1ee5669d3b1d479e.jpg "后台上传页面")
