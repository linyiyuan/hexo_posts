---
layout: 如何在PHP中解析Cron表达式
title:   如何在PHP中解析Cron表达式
date: 2019-12-30 14:23:42
categories: "PHP"
abbrlink: 001b
tags: 
- PHP
- PHP源码解析
---

<img src="https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_4/8c507e495c06858d1ffa9e70e4e870e5.jpg" style="width:900px;height:400px" />

# 导言
我们平时开发过程中，很多种时候都会使用到`Cron表达式` 来储存一个时间计划，通过`Cron表达式` 我们可以实现定时器的功能，我们通常在开发过程中也需要将一个`Cron表达式` 解析成时间格式，小编通过在Github找到了基于PHP实现的`Cron解析包`，下面小编将会来教会大家如何使用这个解析包

<!--less-->


## cron表达式简介

[Cron](http://en.wikipedia.org/wiki/Cron)利用_cron表达式_表示重复计划。Cron表达式由几个字段组成，每个字段代表时间的度量。cron表达式中的字段如下：分钟，小时，每月的某天，月份，一周的某天以及可选的年份。这是一个每分钟运行一次的cron表达式示例，该表达式下方是位置字段。

```bash

*    *    *    *    *    *
-    -    -    -    -    -
|    |    |    |    |    |
|    |    |    |    |    + year [optional]
|    |    |    |    +----- day of week (0 - 7) (Sunday=0 or 7)
|    |    |    +---------- month (1 - 12)
|    |    +--------------- day of month (1 - 31)
|    +-------------------- hour (0 - 23)
+------------------------- min (0 - 59)

```

有几个特殊字符可以修改cron表达式的计划，并且某些修饰符在不同字段中的行为也有所不同。您可以在[cron的Wikipedia页面](http://en.wikipedia.org/wiki/Cron#Special_characters)上找到所有可用特殊字符的列表。

## 安装

`Cron解析包`Github地址：
>> https://github.com/dragonmantank/cron-expression>

我们通过composer的方式来安装该包

```bash
composer require dragonmantank/cron-expression

```

如果你使用的是`Laravel框架的话`则无需安装此包，这是因为`Laravel`框架的已经引入该包


## 使用方法

```bash

require_once '/vendor/autoload.php';

$cronTab = '* * * * *';

//实例化Cron对象
$cron = \Cron\CronExpression::factory($cron);
//根据Cron表达式计算出下次实行时间 返回一个DateTime对象
$cron = $cron->getNextRunDate();
//转换时间为指定格式
echo $cron->format('Y-m-d H:i:s');

```

通过上面的代码我们可以轻易的就根据`Cron表达式` 计算出下次执行时间，是不是很方便呢，当然，这个包提供的方法不仅仅只是单纯计算出下次执行时间，同样我们也可以计算出下X次执行时间
 	
``` php
$cron = \Cron\CronExpression::factory($cron);

foreach($cron->getMultipleRunDates(5) as $date){ 
  echo $date->format('Y-m-d H:i:s') . PHP_EOL; 
}
```

运行结果：

```bash 

D:\php7.3\php.exe C:\Users\php\Desktop\local_test\index.php
2019-12-30 15:53:00
2019-12-30 15:54:00
2019-12-30 15:55:00
2019-12-30 15:56:00
2019-12-30 15:57:00

Process finished with exit code 0

```

同样我们可以获取上次的执行时间 以及上X次执行时间：

```
$cron = \Cron\CronExpression::factory($cron);
$cron = $cron->getPreviousRunDate();
echo $cron->format('Y-m-d H:i:s');

$cron = \Cron\CronExpression::factory($cron);
foreach($cron->getMultipleRunDates(5, 'now', true ) as $date){ 
  echo $date->format('Y-m-d H:i:s') . PHP_EOL; 
}
```
我们还可以通过`isDue` 方法来查看cron表达式是否与特定日期匹配，同时该库也支持一些宏

* `@yearly`，`@annually`-每年1月1日午夜运行一次-`0 0 1 1 *`
* `@monthly` -每个月的第一天午夜运行一次- `0 0 1 * *`
* `@weekly` -每周午夜在太阳上运行一次- `0 0 * * 0`
* `@daily` -每天半夜运行一次- `0 0 * * *`
* `@hourly` -第一分钟每小时运行一次- `0 * * * *`

我们可以通过这些宏来快速计算出执行时间，例如下面这个例子可以快速计算出每天半夜执行一次的时间：

``` php

$cron = Cron\CronExpression::factory('@daily');
echo $cron->getNextRunDate()->format('Y-m-d H:i:s');

```

## 参考文献

1. <http://mtdowling.com/blog/2012/06/03/cron-expressions-in-php/>
2. <https://github.com/mtdowling/cron-expression>

