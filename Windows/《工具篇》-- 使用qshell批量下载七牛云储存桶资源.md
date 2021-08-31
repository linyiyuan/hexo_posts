---
layout: 《工具篇》-- 使用qshell批量下载七牛云储存桶资源
title:   《工具篇》-- 使用qshell批量下载七牛云储存桶资源
date: 2019-12-25 16:31:28
categories: "Windows"
abbrlink: 017a
tags: 
- Windows
- 工具
---

<img src="https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_4/b79518451f5913e791cd6ea019f552d2.png.png" style="width:900px;height:400px" />

# 导言
小编今天在整理旧博客项目的过程中，由于之前服务器过期没能及时续费，导致了博客部分数据的丢失，其中就包括旧博客中的图片资源数据，而这些资源都是储存在七牛云平台上，但是七牛云的后台没有批量下载图片的功能，而小编又需要将七牛云上的资源全部下载下来，所以小编使用了七牛云官方提供的`qshell` 命令行工具，使用这个工具能够下载整个空间的资源文件，下面，小编就来教大家如何使用这一利器

<!--less-->

## 介绍

引用官方的一句简介：“qshell是利用七牛文档上公开的API实现的一个方便开发者测试和使用七牛API服务的命令行工具。该工具设计和开发的主要目的就是帮助开发者快速解决问题 ”

## 下载

我们可以通过在七牛云官网上找到下载地址，里面有各个系统版本所对应的下载地址，这里小编选择的windwos amd64

[官方下载地址](https://developer.qiniu.com/kodo/tools/1302/qshell)

下载完成后我们将压缩包解压得到一个.exe 后缀的文件

## 使用

首先我们并不能直接打开该文件，直接打开该文件会提示`You need to open cmd.exe and run it from there`

[![](http://images.linyiyuan.top/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20191225114314.png)](http://images.linyiyuan.top/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20191225114314.png)

我们应该先打开windows 的命令行工具，可以使用 `Win + R` 打开 运行 然后输入 `cmd` 打开命令行工具

打开命令行工具后我们进入到刚刚解压完`qshell`的目录

[![](http://images.linyiyuan.top/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20191225114606.png)](http://images.linyiyuan.top/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20191225114606.png)

由图显示，小编已经进入该页面，为了方便操作，小编将下载的工具命名为`qshell`
这时候我们就可以使用`qshell` 提供的相关命令进行操作了

## [](http://www.linyiyuan.top/p/021a.html#%E4%B8%8B%E8%BD%BD%E8%B5%84%E6%BA%90 "下载资源")下载资源

1.  鉴权

在进行下载资源之前我们需要鉴权，我们只需要使用命令行配置好相应的key 跟 secret 即可

```cmd
qshell account AccessKey SecretKey AccountName
```


将`AccessKey` `SeretKey` `AccountName` 换成你的七牛账号下的 `AccessKey` 和 `SecretKey`以及账号名即可

运行完成后我们可以查看用户信息

```cmd
qshell user ls
```


1.  配置下载信息

我们需要单独配置一个下载配置，我们新建一个文件，命名为`download.conf`, 然后写入以下内容：

```json
{
 "dest_dir"   :   "C:\\Users\\sirui-php\\Desktop\\test\\image\\",
 "bucket"     :   "test",
 "prefix"     :   "photo/pic/",
 "suffixes"   :   ".png,.jpg",
 "cdn_domain" :   "",
 "referer"    :   "http://www.example.com",
 "log_file"   :   "download.log",
 "log_level"  :   "info",
 "log_rotate" :   1,
 "log_stdout" :   false
}
```

参数说明：

| 参数名 | 描述 | 可选参数 |
| --- | --- | --- |
| dest_dir | 本地数据备份路径，为全路径 | N |
| bucket | 空间名称 | N |
| prefix | 只同步指定前缀的文件，默认为空 | Y |
| suffixes | 只同步指定后缀的文件，默认为空 | Y |
| cdn_domain | 设置下载的CDN域名，默认为空表示从存储源站下载，【该功能默认需要计费，如果希望享受10G的免费流量，请自行设置cdn_domain参数，如不设置，需支付源站流量费用，无法减免！！！】 | N |
| referer | 如果CDN域名配置了域名白名单防盗链，需要指定一个允许访问的referer地址 | N |
| log_level | 下载日志输出级别，可选值为`debug`,`info`,`warn`,`error`,默认`info` | Y |
| log_file | 下载日志的输出文件，如果不指定会输出到qshell工作目录下默认的文件中，文件名可以在终端输出看到 | Y |
| log_rotate | 下载日志文件的切换周期，单位为天，默认为1天即切换到新的下载日志文件 | Y |
| log_stdout | 下载日志是否同时输出一份到标准终端，默认为false，主要在调试下载功能时可以指定为true | Y |

其中`dest_dir` 为你本地保存资源的路径，在Windows系统下面使用的时候，注意`dest_dir`的设置遵循`D:\\jemy\\backup`这种方式。也就是路径里面的`\`要有两个（`\\`）。

1.  运行命令

    配置好下载配置后我们运行以下命令即可下载该存储桶内的所有资源文件

```cmd
qshell qdownload test.conf
```


需要注意的时，如果运行命令报：
[![](http://images.linyiyuan.top/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20191225134430.png)](http://images.linyiyuan.top/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20191225134430.png)

表示我需要我们自己手动去创建保存资源的目录，我们新建相应目录即可

[![](http://images.linyiyuan.top/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20191225134631.png)](http://images.linyiyuan.top/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20191225134631.png)

## 参考文献

1.  [命令行工具(qshell)](https://developer.qiniu.com/kodo/tools/1302/qshell)
2.  [qdownload下载文档](https://github.com/qiniu/qshell/blob/master/docs/qdownload.md)
