---
layout: 《开源项目篇》-- Swoole-webcoket 小型公共聊天系统
title:   《开源项目篇》-- Swoole-webcoket 小型公共聊天系统
date:  2020-01-02 15:23:54
categories: "开源项目"
abbrlink: 061b
tags: 
- PHP
- Laravel
- 开源项目
- Github
- Swoole
- Websocket
---

<img src="https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_14/caa7c7a6a9206432df2918c036ff593b.jpg" style="width:900px;height:400px" />

# 导言
该项目是小编在学习`Swoole`而衍生的一个项目，在接触了`Swolle` 的 `Websocket`之后，利用其相关知识点构建了这么一个即时通讯聊天系统，目前该系统只支持在一个群聊公共聊天，前端支持QQ，微博，GIthub, google快速登录，也可以使用邮箱注册登录，暂未支持好友聊天，群组聊天等场景。

<!--less-->

## 项目部署

由于该项目是基于`Laravel 5.6`, `Swoole` 开发的，首先你得确保你服务器符合以下要求：

*   PHP >= 7.1.3
*   OpenSSL PHP扩展
*   PDO PHP扩展，注意需要php_mysql
*   Mbstring PHP扩展
*   Tokenizer PHP扩展
*   XML PHP扩展
*   Swoole PHP扩展

项目地址：

> [https://github.com/linyiyuan/swoole-webchat.git](https://github.com/linyiyuan/swoole-webchat.git)

项目演示地址：

> [http://webchat.linyiyuan.top](http://webchat.linyiyuan.top/)

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

打开配置文件，可以看到如下的部分配置：

```bash
SERVER_URL=当前服务器IP地址（如果是云服务器请填内网地址）
SERVER_PORT=启动服务的端口（一般填9501即可）
SERVER_ADMIN_PORT=启动服务的内网端口（可忽略不填）
```


紧接着我们打开`public/dist/js/example.js` 并修改：

```bash
ws = new WebSocket("ws://服务器地址:Websocket服务端口");//连接服务器
```

需要注意的是服务器地址如果有域名解析则需要填写域名，这是由于`cookie`可能会导致数据不同步
配置完上面还需配置一下第三方登录的一些参数：

```bash
//GITHUB授权登录秘钥
GITHUB_CLIENT_ID=
GITHUB_CLIENT_SECRET=
GITHUB_REDIRECT=http://webchat.linyiyuan.top/OAuth/callback/github

//微博授权登录秘钥
WEIBO_KEY=
WEIBO_SECRET=
WEIBO_REDIRECT_URI=http://webchat.linyiyuan.top/OAuth/callback/weibo

//QQ授权登录秘钥
QQ_APP_ID=
QQ_APP_KEY=
QQ_REDIRECT=http://webchat.linyiyuan.top/OAuth/callback/qq

//谷歌授权登录秘钥
GOOGLE_APP_ID=
GOOGLE_APP_KEY=
GOOGLE_REDIRECT=http://webchat.linyiyuan.top/OAuth/callback/google
```

2、根据.env文件修改各配置项，如果.env文件中没有存在key值则运行命令：

```bash
php artisan key:generate
```

3、配置stroage bootstrap 可写

```bash
chmod -R 777 stroage bootstrap
```


**第4步：启动服务**

在项目目录下运行一下命令

```bash
php artisan server:build Websocket
```


控制台显示`The WebSocket Server is Started` 说明启动成功，由于该命令需要后台常驻，我们可以使用`supervisor` 来进行管理，至此项目部署已经基本以完成，配置相应apache或者nginx 域名指向即可访问网站

## 项目展示

[![登录页面](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_14/b71f4a907186bb9915934c4522a4100e.jpg)](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_14/b71f4a907186bb9915934c4522a4100e.jpg "登录页面")

[![注册页面](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_14/d765f586b988b4014dfb806b705edf8d.jpg)](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_14/d765f586b988b4014dfb806b705edf8d.jpg "注册页面")

[![加入聊天室](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_14/17543dfced3ed6474b6879ec29eb590b.jpg)](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_14/17543dfced3ed6474b6879ec29eb590b.jpg "加入聊天室")

[![多人聊天](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_14/0c2efe0685655d9a1809cb07c6031d93.jpg)](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_14/0c2efe0685655d9a1809cb07c6031d93.jpg "多人聊天")

[![夜晚模式](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_14/caa7c7a6a9206432df2918c036ff593b.jpg)](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_14/caa7c7a6a9206432df2918c036ff593b.jpg "夜晚模式")
