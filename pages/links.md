---
layout: mypost
title: 友情链接
---

<ul>
  {%- for link in site.links %}
  <li>
    <p><a href="{{ link.url }}" title="{{ link.desc }}" target="_blank" >{{ link.title }}</a></p>
  </li>
  {%- endfor %}
</ul>

```
如果你发现最近网站访问增加了，那是因为我的百万并发的博客添加了你的友链
```
