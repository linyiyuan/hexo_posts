---
layout: 《插件篇》-- 使用Vue + ElementUI 搭建的Cron表达式组件
title: 《插件篇》-- 使用Vue + ElementUI 搭建的Cron表达式组件
date: 2019-12-20 14:42:45
categories: "Vue"
abbrlink: 041b
tags: 
- Vue
- ElementUI
- 插件
---

<img src="http://images.linyiyuan.top/cron1.png" style="width:900px;height:400px" />

# 前言

学习过`Linux` 的同学都知道，`Linux`自带了一个`Crontab` 的定时器，用于设置周期性被执行的指令，用户所建立的crontab文件中，每一行都代表一项任务，每行的每个字段代表一项设置，它的格式共分为六个字段，前五段是时间设定段，第六段是要执行的命令段，一般而言我们我们程序员书写该命令段是比较相对容易，只需要理解其中字段代表意思即可，但实际项目中，我们有些功能或许需要使用到该字段格式，例如一个活动配置中的开启关闭时间，这类时间比较灵活，我们可以根据`Crontab`表达式来进行设定其时间，而当我们使用这种表达式时，对运营小伙伴就比较不友好，他们需要去学习理解这种表达式，现在，我们可以使用`vue-cron` 表达式组件，支持多种特性，可以方便、快捷直观地定义cron表达式。
<!--less-->

## 安装

该项目是由 [ldy](https://gitee.com/lindeyi) 作者编写，以下文档说明大部分都是从作者项目下转载，非原创

项目地址：

*   [https://gitee.com/lindeyi/vue-cron](https://gitee.com/lindeyi/vue-cron)

首先我们将该项目本地克隆下来，然后将 `src/components` 里面的两个文件 `cron.vue`, `cron` 复制到我们的项目中的`components`中即可。

## 使用方式

在我们需要使用到该插件的页面中去引入该插件, 并注册

```js
import cron from '@/components/cron'

export default {
name: "App",
components: {
 cron
},
....
```

然后我们在指定显示组件的地方粘贴下以下代码：

```vue
<el-form-item style="margin-top: -10px; margin-bottom:0px;">
 <span style="color: #E6A23C; font-size: 12px;">corn从左到右（用空格隔开）：秒 分 小时 月份中的日期 月份 星期中的日期 年份</span>
 <cron v-if="showCronBox" v-model="cronParams"></cron>
</el-form-item>
 <el-form-item label="cron表达式">
 <el-input size="medium" v-model="cronParams" auto-complete="off" placeholder="请填写cron表达式">
 <el-button slot="append" v-if="!showCronBox" icon="el-icon-arrow-up" @click="showCronBox= true" title="打开图形配置"></el-button>
 <el-button slot="append" v-else icon="el-icon-arrow-down" @click="showCronBox= false" title="关闭图形配置"></el-button>
 </el-input>
</el-form-item>
```

然后我们同样需要在`data` 数据块中加上以下数据即可：

```vue
data() {
 return {
 cronParams: null,
 showCronBox: false,
 }
}

```


## 页面演示

[![](http://images.linyiyuan.top/cron1.png)](http://images.linyiyuan.top/cron1.png)

[![](http://images.linyiyuan.top/cron2.png)](http://images.linyiyuan.top/cron2.png)

[![](http://images.linyiyuan.top/cron3.png)](http://images.linyiyuan.top/cron3.png)

[![](http://images.linyiyuan.top/cron4.png)](http://images.linyiyuan.top/cron4.png)

## 参考文献

1.  [vue-cron 基于Vue的Cron表达式组件](https://gitee.com/lindeyi/vue-cron)
2.  [超级好用的Cron表达式组件easy-cron](http://www.easysb.cn/2019/04/210.html)
