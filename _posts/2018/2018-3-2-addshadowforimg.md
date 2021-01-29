---
layout: post
title: css - 为图片添加阴影
categories: 代码
tags: css 
---
如果网页底色和图片颜色相同（白色），加个阴影是个非常好的选择。
## Show me the code
```css
p img {
	box-shadow: 0 0 10px rgba(0,0,0,0.5);
	overflow: visible;
	padding: 10px;
}
```
写`p img`意思是对 p 标签下的 img 标签生效，防止用错对象。
## 图片效果测试
![img](http://p2l510m4q.bkt.clouddn.com/aubucuo_180118_1707.png)