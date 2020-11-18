---
layout: Mysql汇总查询将两个结果集合并一行展示
title:   Mysql汇总查询将两个结果集合并一行展示
date: 2019-12-13 14:37:57
categories: "MYSQL"
abbrlink: 011c
tags: 
- MYSQL
- SQL语句
---

<img src="http://images.linyiyuan.top/zval_sep.png" style="width:900px;height:400px" />

# 导言
  今天在处理一个数据查询时，遇到一个问题，需要将同一张表不同条件的分组结果合并成一个结果集，最初以为使用union与union all 就能解决问题，后来发现事情远远没那么简单，因为他这里出现了两个不同字段，而使用union的前提就是要保证字段的相同，在查阅网上相关资料后，对该问题进行了一次总结

<!--less-->

## 初始化

建表语句：

```sql
CREATE TABLE `test` (
  `time` varchar(255) DEFAULT NULL,
  `money` varchar(255) DEFAULT NULL,
  `date` varchar(255) DEFAULT NULL
) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC;


INSERT INTO `test`(`time`, `money`, `date`) VALUES ('1', '100', 'today');
INSERT INTO `test`(`time`, `money`, `date`) VALUES ('2', '1000', 'today');
INSERT INTO `test`(`time`, `money`, `date`) VALUES ('3', '10000', 'today');
INSERT INTO `test`(`time`, `money`, `date`) VALUES ('4', '100000', 'today');
INSERT INTO `test`(`time`, `money`, `date`) VALUES ('1', '100', 'yestoday');
INSERT INTO `test`(`time`, `money`, `date`) VALUES ('2', '1000', 'yestoday');
INSERT INTO `test`(`time`, `money`, `date`) VALUES ('3', '10000', 'yestoday');
INSERT INTO `test`(`time`, `money`, `date`) VALUES ('4', '100000', 'yestoday');

```

预期输出：

![img](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_15/6f92ca792276ad43950d22e0ffd3acec.png)

## 解决方法

1. 方法1：

```sql
select 
time,
sum(if(date='today',money,0)) as today_money,
sum(if(date='yestoday',money,0)) as yestoday_money
from test
group by time;
```

2. 方法2：

```sql
select 
time,
sum(case when date='today' then money end) as today_money,
sum(case when date='yestoday' then money end) as yestoday_money
from test
group by time
```

3. 方法3：

```sql
select
  a.time,
  a.today_money,
  b.yestoday_money
from
  ((
    select
      time,
      sum( money ) as today_money
    from
      test
    where
      date = 'today'
    group by
      time
      ) as a
  left join ( select time, sum( money ) as yestoday_money from test where date = 'yestoday' group by time ) as b on a.time= b.time
  )
    
```

## 结果：

![img](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_15/3b64b201f46f3d16f98e61f3f8e66c45.png)


## 参考文章
1. [mysql sql汇总查询将两个结果集合并一行展示](https://jingyan.baidu.com/article/4ae03de3df93cc3eff9e6b37.html)
