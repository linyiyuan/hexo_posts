---
layout: 检测IP是否为国内IP
title: 检测IP是否为国内IP
date: 2019-08-10 14:42:45
categories: "PHP"
abbrlink: 004a
tags: 
- PHP
---

<img src="http://images.linyiyuan.top/timg.jpg" style="width:900px;height:400px" />

# 前言
最近项目有个需求，需要限制国外IP的访问，并且请求频率比较多，使用淘宝提供的接口检测的话 超过一定次数就会卡顿或者出现502的错误，所以这里通过计算IP以及网段判断IP是否处于国内的网段，根据Apnic分配给中国的IP网段可以知道所有国内的网段列表 ，这个网段列表是会持续更新，所以需要我们定期去获取更新

>  Apnic是全球5个地区级的Internet注册机构（RIR）之一，负责亚太地区的以下一些事务： 
（1）分配IPv4和IPv6地址空间，AS号 
（2）为亚太地区维护Whois数据库 
（3）反向DNS指派 
（4）在全球范围内作为亚太地区的Internet社区的代表

<!--less-->

# 定期获取IP网段
这里小编写了shell脚本并且结合crontab定期获取IP网段列表并写入Redis

1. 编写shell脚本获取IP断列表并写入redis中, 保存文件为shell.sh
```bash
!#/bin/bash
curl 'http://ftp.apnic.net/apnic/stats/apnic/delegated-apnic-latest' | grep ipv4 | grep CN | awk -F\| '{ printf("%s/%d\n", $4, 32-log($5)/log(2)) }' > /china_ip.txt

cat '/china_ip.txt' |while read p
  do
    redis-cli -h 127.0.0.1 -p 6379 hset "china_ip" $p $p >/dev/null
  done
```

2. 在crontab 加上以下命令 (每天00:00自动更新脚本)
```bash
00 00 * * * /data/shell.sh
```


# 使用函数判断指定IP是否存在指定网段中（Laravel框架）

```php
/**
 * Class Sms
 * @package App\Http\Services\Common
 * @Author YiYuan-LIn
 * @Date: 2019/2/28
 * 校验IP工具类
 */
class VerifyIp
{
    /**
     * @Author YiYuan-LIn
     * @Date: 2019/8/10
     * @enumeration:
     * @param $ip
     * @return bool
     * @description 校验IP是否处于国内的网段
     */
    public static function CheckWhetherIPIsDomestic($ip)
    {
        //校验IP参数是否合法
        if (empty($ip)) return false;
        if(filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4)) return false;
        $ip = (double) (sprintf("%u", ip2long($ip)));
        $china_ip = Redis::hkeys('china_ip');
        $china_ip = array_filter($china_ip);
        if (empty($china_ip)) return false;

        foreach ($china_ip as $key => $value) {
            $s = explode('/', $value);
            $network_start = (double) (sprintf("%u", ip2long($s[0])));
            $network_len = pow(2, 32 - $s[1]);
            $network_end = $network_start + $network_len - 1;

            if ($ip >= $network_start && $ip <= $network_end) return true;
            continue ;
        }

        return false;
    }
}
```

