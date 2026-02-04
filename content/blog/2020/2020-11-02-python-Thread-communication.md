---
date: 2020-11-02
title: Python 中的线程通信
author: Jn
categories: 工作
tags: 
- Python
---


题目起的宽了，心有点虚。放个链接吧 [Python 多线程threading][1]
多线程编程必须操作int str等变量的话只能加锁。如果是计数的需求，**Python的list.append list.pop是线程安全的**。因此可以用list.append('type1') 这种来计数，使用list.count(type1)来达到计数目的。

### 题外话
双11啥也没买，印证了那句话：多打几份工不会让你变有钱，但可以让你没时间花钱。苦笑

杭州渐渐变冷了，反应有点迟滞的我没添衣服，骑电动车通勤被冻地瑟瑟发抖。

[1]: https://www.liujiangblog.com/course/python/79

