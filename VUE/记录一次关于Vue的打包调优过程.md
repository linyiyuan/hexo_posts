---
layout: 记录一次关于Vue的打包调优过程
title: 记录一次关于Vue的打包调优过程
date: 2019-08-30 14:42:45
categories: "Vue"
abbrlink: 001b
tags: 
- Vue
- Webpack
---

<img src="http://images.linyiyuan.top/webpack.png" style="width:900px;height:400px" />

# 前言

在使用Vue-admin-template作为基础框架进行后台开发的时候，发现每次打包出来的文件很大，最大的有4MB左右, 导致第一次加载页面的十分缓慢，需要很长时间才能进去后台, 影响用户体验，所以现在对其进行优化。

<!--less-->

# 分析问题
在进行优化的前提下，首先要做得是对整个框架进行分析，看看具体哪个模块以及那个插件打包体积过大，针对性的进行优化，在这里我们通过 `webpack-bundle-analyzer` 进行分析

步骤1：
```bash
  npm install --save-dev webpack-bundle-analyzer //安装插件
```

步骤2:
```javascript
//在`build/webpack.prod.config.js`中的`module.exports = webpackConfig`这句话的上面增加

if (config.build.bundleAnalyzerReport) {
  const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin
  webpackConfig.plugins.push(new BundleAnalyzerPlugin())
}
```

步骤3:
```shell
  npm run build --report
```

结果：
![](http://images.linyiyuan.top/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190830155242.png)

通过结果我们不难发现，体积过大的文件有一下几个：
1. element-ui
2. vue
3. echarts
4. xlsx
5. tinymce
6. vCharts

# 优化问题

优化主要目的有：

1.  加快资源加载速度，减少用户等待的时间和首页白屏时间，提高用户体验。
2.  加快打包速度，不要将时间浪费在等待打包上。

## 更换CDN：

解决第一个问题，很多人都会想到资源文件放在 CDN 上就好了，没错，这次我们就是通过 CDN 来解决加载问题。

在 `index.html` 中引入 vue, element-ui tinymce echarts。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <!-- 同时也要引入对应版本的 css -->
    <link href="https://cdn.bootcss.com/element-ui/2.3.2/theme-chalk/index.css" rel="external nofollow" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/v-charts/lib/style.min.css">
    <title>Vue-admin-template</title>
  </head>
  <body>
    <script src="./static/tinymce4.7.5/tinymce.min.js"></script>
     <!-- 先引入 Vue -->
    <script src="https://cdn.bootcss.com/vue/2.4.2/vue.min.js"></script>
    <!-- 引入组件库 -->
    <script src="https://cdn.bootcss.com/element-ui/2.3.2/index.js"></script>
    <script src="https://cdn.bootcss.com/element-ui/2.3.2/locale/zh-CN.min.js"></script>
    <script src="https://cdn.bootcss.com/echarts/3.7.0/echarts.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/v-charts/lib/index.min.js"></script>
    <script src="https://cdn.bootcss.com/xlsx/0.15.1/cpexcel.js"></script>
    <script src="https://cdn.bootcss.com/xlsx/0.15.1/xlsx.core.min.js"></script>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/v-charts/lib/style.min.css">
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>


```

因为依赖是从外部引入的，所以需要告知 webpack 在打包时，依赖的来源。

修改 `webpack.base.conf.js` 在最后一行加上一下代码：

```javascript
  externals: {
    vue: 'Vue',
    'element-ui':'ELEMENT',
    "echarts": "echarts",
    "vCharts": "v-charts",
    'xlsx': "xlsx"
 }
```

删除所有引入/导入Vcharts , echarts, tinymce 的地方

再次 npm run build --report 

![](http://images.linyiyuan.top/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190902113040.png)

发现其打包文件体积比之前足足少了2MB，在实际开发过程中，可能会使用到其他的插件，我们可以根据插件来更换其CDN从而减少其打包体积大小，这样页面在加载的时候也会更快

## 服务器端开启 gzip
使用 gzip 可以进一步压缩文件，使得服务器传递给浏览器的文件是经由压缩之后的，待浏览器收到之后再解压缩。要使用这一方式，需要服务器端的支持，这里以 Nginx 为例。

在 `nginx.conf` 中，添加如下配置：

```shell
gzip on;
gzip_min_length 1k;
gzip_buffers 4 16k;
#gzip_http_version 1.0;
gzip_comp_level 2;
gzip_types text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png application/javascript;
gzip_vary off;
```

之后刷新页面（ 注意禁用缓存 ），观察 js、css 等资源文件的请求中是否包含 `Content-Encoding: gzip`，如果存在，则表明 gzip 已成功。

注意，在 `gzip_types` 中规定了哪些请求类型会使用 gzip 进行压缩。对于没有使用 gzip 的资源文件，可将其 `Content-type` 类型加入 `gzip_types` 之中。

## 参考

1.  [实例 PK ( Vue服务端渲染 VS Vue 浏览器端渲染 ) - segmentfault](https://segmentfault.com/a/1190000008795113)
2.  [使用vue-cli生成的vendor.js文件太大，有办法减少体积吗？ - segmentfault](https://segmentfault.com/q/1010000008832754)
3.  [Webpack 打包优化之体积篇](http://jeffjade.com/2017/08/06/124-webpack-packge-optimization-for-volume/)
4.  [babel-plugin-lodash](https://www.npmjs.com/package/babel-plugin-lodash)
5.  [Nginx开启Gzip压缩大幅提高页面加载速度 - 博客园](http://www.cnblogs.com/mitang/p/4477220.html)
6.  [Nginx启用Gzip压缩js无效的原因 - 博客园](http://www.cnblogs.com/youlechang123/p/5345852.html)
7.  [Wabpack系列：在webpack+vue开发环境中使用echarts导致编译文件过大怎么办？ - 博客园](http://www.cnblogs.com/strinkbug/p/5786222.html)
8.  [Webpack 打包优化之速度篇](http://jeffjade.com/2017/08/12/125-webpack-package-optimization-for-speed/)
9.  [如何写一手漂亮的 Vue](http://jeffjade.com/2017/03/11/120-how-to-write-vue-better/)
