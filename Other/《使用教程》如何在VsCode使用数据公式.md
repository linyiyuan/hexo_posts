---
layout: 《使用教程》如何在VsCode使用数据公式
title:   《使用教程》如何在VsCode使用数据公式
date: 2021-08-31 11:21
categories: "Other"
abbrlink: 020a
tags: 
- VsCode
- 工具
- MarkDown
- VsCode
---

<img src="https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_19/58d7d59cd228cfd9f9c5b88a25a8af9b.jpg" style="width:900px;height:400px" />

# 导言
在备战自考学习时，我常常需要记录一些数学公式到我的 `markdown` 笔记中，而我是在`vscode`上进行编辑markdown的，所以需要一个能够书写数学公式并能够解析的插件, 在查看了有关`vscode`插件后，可以通过安装`Markdown All in One` 这个插件进行公式的解析以及书写
<!--less-->

# Markdown All in One 介绍

`Markdown All in One` 一款在VSCode下,编辑md文件十分好用的扩展插件

### 特点：

1.  提供了常用操作便利的快捷键
2.  支持目录
3.  一边书写一边预览(Ctrl + Shift + V or Ctrl + K V)
4.  可轻松转换为HTML文件和PDF文件
5.  优化了List editing的编辑
6.  可格式化table (Alt + Shift + F) 以及Task list (use Alt + C to check/uncheck a list item)
7.  支持特殊数学符号渲染

而我们需要用到他的第七个特点，也就是支持特殊数学符号的渲染


# MarkDown All in One 安装

我们通过快捷键 `Ctrl + Shift + P` 调出VsCode控制面板，然后输入 `MarkDown All in One` 点击安装即可

# 数学公式的使用

我们可以通过以 `$$`  开头和结尾 作为数学公式的标记
```md
$$
  //数学公式
$$
```

然后我们在包裹里面书写公式即可，例如：

```md
$$
  \lim_{x \to \infty} 
$$

$$
  \begin{vmatrix}
     a & b \\
     c & d 
  \end{vmatrix}
$$

$$
\displaystyle\sum_{i=1}^{n+1} d_h^i
$$
```

$$
  \lim_{x \to \infty} 
$$

$$
  \begin{vmatrix}
     a & b \\
     c & d 
  \end{vmatrix}
$$

$$
\displaystyle\sum_{i=1}^{n+1} d_h^i
$$


然后我们通过在VsCode中 按下`Crtl + K V` 打开MD侧边预览款查看

![使用公式](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_19/75694620315b1daefb0c031da3dfca2e.png)

具体的公司列表可以查看下面这个文档链接：[katex官方文档](https://katex.org/docs/supported.html)， 里面记录了各种数学公式的书写方法，当然你可以通过这个LaTex在线公式编辑器去设置你的公式 

[ LaTeX公式编辑器](https://www.latexlive.com/home)

![在线公式编辑器](https://shmily-album.oss-cn-shenzhen.aliyuncs.com/photo_album_19/6b62a37898d23dc8e18eac51cb912f23.png)

# 在HTML页面上的解析

我的Markdown笔记最终也是要生成`docsify` 文档通过Web去访问的，所以最终访问的还是`HTML` 所以我就需要在`HTML` 去显示这些数学公式，在html页面上我们可以通过加以下代码

```js
  <script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" async></script>

  <script type="text/x-mathjax-config">

      MathJax.Hub.Config({ tex2jax: {inlineMath: [['$', '$']]}, messageStyle: "none" });

  </script>
```

使得`HTML`页面能够正确显示数学公式

# 参考资料
1. [LaTex公式编辑器](https://www.latexlive.com/home)、
2. [katex官方文档](https://katex.org/docs/supported.html)
