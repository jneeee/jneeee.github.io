---
layout: post
title: TeXt 主题探究
categories: 代码
---
> 对于好看的网页前端，我是非常喜欢的，尤其是在自己的博客上。因此本着“知其然知其所以然”的精神，我一边看一边学习这个主题的语法。目的是到最后可以再本主题的基础上修改成自己喜欢的样子。

## 一、布局
在首页和文章页，我希望文章日期是在标签前面的（左对齐）。因此需要修改 
`_layouts/home.html`。发现引用了`components/article-data.html`文件，这是现实文章标签和日期的文件（还包含一个 key 用来统计浏览次数的，被我删了）。data 文件的 css 在`_sass/components/_article_data.scss`，修改如下：
```css
.view-wrapper {
    @include split-line(left);
```
## 二、全局参数
## 三、插件