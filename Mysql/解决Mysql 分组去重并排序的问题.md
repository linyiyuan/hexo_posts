---
title: Mysql 的事务四个特性以及事务的四个隔离级别
tags: 
	  - Mysql
categories: "Mysql"
abbrlink: 011a
date: 2019-12-13 14:37:57
---

<img src="http://images.linyiyuan.top/staff_1024.jpg" style="width:900px;height:400px" />

## 前言

今天在处理一个数据汇总的时候，需要写一条mysql语句用来获取根据最新日期排序并且根据用户去重的一个列表数据，当执行这条语句后，发现并没有按照预期的结果显示数据，显示的是未排序的数据，但是去重已经完成了，在查阅了相关资料后发现mysql5.7更新一些特性导致这个语句失效。

<!--less-->


## 问题解决

假设我们现在有一张用户钱包表 该表包含着四个字段 气质ID字段是递增的 还有用户名(username), 货币数量(currency), 添加时间(addTime), 用户的货币数量随着时间的变化而更新

![img](http://images.linyiyuan.top/snipaste_20191214_143831.jpg)

现在我们需要查询每个用户的最新货币数量，首先我们需要对列表进行排序并对用户去重，所以我们就简单的写了一条sql语句并执行：

```mysql
select distinct username, currency, addTime from user_wallet order by addTime desc;
```


结果显示：

![img](http://images.linyiyuan.top/snipaste_20191214_144255.jpg)

结果显示去重并未生效，在网上查看的资料后，其原因是`distinct`只能返回他的目标字段，而无法返回其他字段，所以我们使用`group by` 进行去重 在使用`group by`之前，由于我本地的MySQL 版本是5.7，而MySQL5.7 默认的 MySQL 配置中 `sql_mode` 配置了 `only_full_group`，需要 `GROUP BY` 中包含所有 在 SELECT 中出现的字段, 所以我们需要在MySQL 的配置中去掉这个配置:

在 配置文件（my.cnf）中 修改 sql\_mode 的配置为：

```bash
$ vim /usr/local/etc/my.cnf
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
```


然后重启MYSQL服务即可：

![img](http://images.linyiyuan.top/snipaste_20191214_145646.jpg)

结果显示，去重已经生效，但是排序未生效，这是因为我们是先去重拿到结果后再进行的排序，所以排序也是针对于去重后的结果进行排序，所以我们对这条Sql语句进行修改：

```mysql
select username, currency, addTime FROM (select username, currency, addTime from user_wallet order by addTime DESC) alias GROUP BY username;
```

![img](http://images.linyiyuan.top/snipaste_20191214_150133.jpg)

结果发现排序还是未能生效，

小编在查阅了相关资料后，发现在mysql 5.7 中 ，这种语法并没有效果，正确的写法是：

```mysql
select username, currency, addTime FROM (select username, currency, addTime from user_wallet order by addTime DESC limit 0, 10000) alias GROUP BY username;
```


通过 explain 查看执行计划，可以看到没有 limit 的时候，少了一个 DERIVED 操作。DERIVED用于派生表的SELECT(FROM子句的子查询)，所以没有它就相当于子句里的排序并没有被执行。至此问题解决

## 注意事项
1. 采用子句查询时，别名（alias）不可少，会报语法错误
2. 如果有分页，分页条件要写在子句里面
3. 需要去重mysql的严格模式

## 参考文章

1. [Mysql5.7分组排序](https://jingyan.baidu.com/article/4ae03de3df93cc3eff9e6b37.html)
2. [解决MySQL：1055 - Expression #2 of SELECT list is not in GROUP BY clause and contains nonaggregated](https://www.cnblogs.com/chuanqi1995/p/11436993.html)
