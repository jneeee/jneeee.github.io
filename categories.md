---
layout: default
title: 分类 - Jneee
permalink: /categories/
---
<ul class="tags-box">
	<h2>文章分类</h2>
{% for cat in site.categories %}
<a href="#{{ cat[0] }}" title="{{ cat[0] }}" rel="{{ cat[1].size }}">{{ cat[0] | join: "/"}}<span class="size"> {{ cat[1].size }}</span></a> | 
{% endfor %}</ul>

<ul class="tags-box">
{% for cat in site.categories %}
<li id="{{ cat[0] }}">{{ cat[0]}}</li>
{% for post in cat[1] %}
<time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time> &raquo;
<a href="{{ site.baseurl }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a><br />
{% endfor %}{% endfor %}
</ul>