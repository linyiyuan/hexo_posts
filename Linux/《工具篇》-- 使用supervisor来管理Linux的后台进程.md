---
layout: 《工具篇》-- 使用supervisor来管理Linux的后台进程
title:   《工具篇》-- 使用supervisor来管理Linux的后台进程
date: 2019-12-19 16:33
categories: "Linux"
abbrlink: 014a
tags: 
- Linux
- 工具
---

<img src="http://images.linyiyuan.top/klahsudkihajhskdjhk45641564.jpg" style="width:900px;height:400px" />

# 导言
我们在日常开发过程中，我们服务器都会经常跑一些程序，而这些程序都是放在后台去运行，但是有些程序运行过程中可能会出现崩溃，或者提前退出等情况，所以我们需要有一个用于管理进程的工具，当进程中断或者奔溃的时候能自动重新启动它，`Supervisor` 就是这么一个用于管理进程的工具

<!--less-->

## Supervisor 介绍

`Supervisor` 是一个进程管理工具，当进程中断的使用`Supervisor`能自动重新启动它，该工具使用`Python`语言开发，支持Linux/Unix系统，不支持Windows系统，它可以很方便的监听、启动、停止、重启一个或多个进程。

## Supervisor 安装

以下安装均在`Ubuntu16.04`系统下进行

1.  安装 `Python`

由于`Supervisor`是由`Python`语言开发，自然而然我们的系统就需要安装`Python`语言环境, 一般情况下`Ubuntu`都自带`Python`语言环境

```bash
apt-get install python3.7
```

1.  安装`Supervisor`

```bash
apt-get install supervisor
```


安装成功后 运行：
```bash
services supervisor status
```

查看`supervisor` 运行状态

## Supervisor 配置

首先查看`Supervisor` 的主配置文件，一般文件位于`/etc/supervisor/`目录下

```bash
vim /etc/supervisor/supervisord.conf

//文件内容
; supervisor config file 
[unix_http_server]
file=/var/run/supervisor.sock   ; (the path to the socket file)
chmod=0700                       ; sockef file mode (default 0700)

[supervisord]
logfile=/var/log/supervisor/supervisord.log ; (main log file;default $CWD/supervisord.log)
pidfile=/var/run/supervisord.pid ; (supervisord pidfile;default supervisord.pid)
childlogdir=/var/log/supervisor            ; ('AUTO' child log dir, default $TEMP)

; the below section must remain in the config file for RPC
; (supervisorctl/web interface) to work, additional interfaces may be
; added by defining them in separate rpcinterface: sections
[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[supervisorctl]
serverurl=unix:///var/run/supervisor.sock ; use a unix:// URL  for a unix socket

; The [include] section can just contain the "files" setting.  This
; setting can list multiple files (separated by whitespace or
; newlines).  It can also contain wildcards.  The filenames are
; interpreted as relative to this file.  Included files *cannot*
; include files themselves.

[include]
files = /etc/supervisor/conf.d/*.conf
```

从配置文件最后一行可以看出子配置文件位于`/etc/supervisor/conf.d/` 目录下，如果没有则新建相应目录即可，我们打开该目录下`conf.d`目录并新建一个文件 `test.conf`

```bash
vim test.conf
```

并写入以下内容：

```bash
[program:websocket]
command= //需要被监听的进程路径
autostart=true  //是否随着supervisord的启动而启动
autorestart=unexpected //是否自动重启
exitcodes=0 //正常退出代码
stopsignal=KILL //用来杀死进程的信号
user=root //执行命令用户
redirect_stderr=true //重定向stderr到stdout
stdout_logfile= //日志路径
```

紧接着我们重启 `Supervisor` 服务

```bash
service supervisor restart
```

`Supervisord`启动成功后，可以通过`Supervisorct`l客户端控制进程，启动、停止、重启。运行`supervisorctl`命令


```bash
supervisorctl 
test  RUNNING   pid 16350, uptime 1:20:44
supervisor> status
test  RUNNING   pid 16350, uptime 1:20:45
supervisor>
```


然后我们可以看到我们监听的进程`test`已经 在运行了，我们可以通过以下命令对其进行控制：

```bash
supervisorctl restart <application name> ;重启指定应用
supervisorctl stop <application name> ;停止指定应用
supervisorctl start <application name> ;启动指定应用
supervisorctl restart all ;重启所有应用
supervisorctl stop all ;停止所有应用
supervisorctl start all ;启动所有应用
```

## [](http://www.linyiyuan.top/p/031a.html#%E5%8F%82%E8%80%83%E6%96%87%E7%AB%A0 "参考文章")参考文章

1.  [linux学习(四) – supervisor守护进程](https://www.cnblogs.com/redirect/p/6599489.html)
2.  [Supervisor-守护进程工具](https://www.jianshu.com/p/39b476e808d8)
