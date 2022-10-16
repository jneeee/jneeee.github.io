---
published: true
title: 各种图的用处整理（Markdown Typora）
layout: post
author: Jn
category: work
tags: 
- typora
---

> 刚跳槽还是轻松啊，加上本身新公司压力小，可以花时间（work time）学 ansible，之后 knowledge sharing 阶段需要做个PPT，里面的图学一下用typora画，语法后面应该都能用到。

mermaid 的官方文档：<https://mermaid-js.github.io/mermaid/#/?id=diagram-types> 各种图的简单示例比较详细，语法不废话了，整理一下各种图的使用场景。

### 1 Flowchart 流程图
所有流程图都由多种几何形状的节点、箭头或线条组成。可以有多种箭头类型、嵌套子图。  
![](https://mermaid-js.github.io/mermaid/img/flow.png)

### 2 Sequence diagram 时序图
时序是一种交互图，它显示了**进程**的交互信息和运行时序，以及这中间的内部逻辑。还可以写单进程的注释 Note；用嵌套框描述复杂逻辑（loop，case）（横轴是进程名，纵轴是时间）  
![](https://mermaid-js.github.io/mermaid/img/sequence.png)

### 3 Gantt chart 甘特图
描述项目的各个子任务的开始结束时间，横轴是时间，纵轴可以是人、子项目名称和消息队列/共享内存等。  
![](https://mermaid-js.github.io/mermaid/img/gantt.png)

### 4 Class diagrams 类图
描述类的内部信息和各个类之间的关系。整理代码必备。（不同线条定义了类之间的不同关系：继承、组合、关联、聚合、链接、依赖、实现）[defining-relationship](https://mermaid-js.github.io/mermaid/#/classDiagram?id=defining-relationship)
![](https://mermaid-js.github.io/mermaid/img/class.png)

### 5 Git graph 
一个横向的版本路径图，项目经理必备（版本规划）。
> 语法比较简单，比如没法定义commit的名称，只能是随机的hash值。最致命的是typora写这个图容易卡死。不推荐。

![](https://mermaid-js.github.io/mermaid/img/git.png)

### 6 Entity Relationship Diagram 实体关系图？
想不到用处

### 7 User Journey Diagram 人员经历图
个人工作总结可以用，横轴是时间，上面是任务名称，任务可以嵌套，任务上可以标注协作者；纵轴是自我评价。
![](https://mermaid-js.github.io/mermaid/img/user-journey.png)

### 8 Pie chart diagrams 饼图
圆形统计图，有占比数据时候能用到。

### 9 State Diagram 状态图
感觉和流程图区别不大。

### 10 Requirement Diagram 需求依赖关系图
想不到用处

