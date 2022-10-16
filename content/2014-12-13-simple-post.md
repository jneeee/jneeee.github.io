---
published: false
layout: post
title: Sample Post
excerpt: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
comments: true
---
kramdown markdown解析器的语法介绍
https://kramdown.gettalong.org/quickref.html

{:toc}

This is a sample post.

Below is just about everything you'll need to style in the theme. Check the source code to see the many embedded elements within paragraphs.

## 1 目录
```
{:toc}
```

## 2 Heading

~~~
# Heading 1

## Heading 2

### Heading 3

#### Heading 4
~~~



## 3 Body text

**This is strong**.

This is figure

![Elaphurus davidianus](https://i.imgur.com/Mdc4szJl.jpg "Père David's deer")

*This is emphasized*.

 53 = 125. Water is H<sub>2</sub>O. 

The New York Times <cite>(That’s a citation)</cite>. 

<u>Underline</u>. 


<del>HTML and CSS are our tools</del>. 

## 4 Blockquotes

> Justification:
> This species is listed as Extinct in the Wild, as all populations are still under captive management. The captive population in China has increased in recent years, and the possibility remains that free-ranging populations can be established some time in the near future. When that happens, its Red List status will need to be reassessed. 

> I follow up the quest. Despite of day and night and death and hell.
> <center> -- Idylls of the King, Tennyson </center>



## 5 List Types

### 5.1 Ordered Lists

1. Item one
   1. sub item one
   2. sub item two
   3. sub item three
2. Item two

### 5.2 Unordered Lists

* Item one
* Item two
* Item three

## Tables

| Header1 | Header2 | Header3 |
|:--------|:-------:|--------:|
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
| cell7   | cell8   | cell9   |


| Kingdom | Phylum  | Class | Order | Family |
|:------:|:------:|:------:|:------:|:------:| 
|ANIMALIA|CHORDATA|MAMMALIA|CETARTIODACTYLA|CERVIDAE|


## 6 Code Snippets

Syntax highlighting via Pygments

{% highlight css %}
#container {
  float: left;
  margin: 0 -240px 0 0;
  width: 100%;
}
{% endhighlight %}


### 6.1 highlight with line number.

{% highlight ruby linenos  %}
def foo
  puts 'foo'
end
{% endhighlight %}


### 6.2 highlight using triple backticks

```r
a=1:10
for(i in a)
{
  print(i)
}
```

## 7 $$\LaTeX$$ 

you can use latex with <q>double $$ </q>

$$e^{i\pi}+1=0$$


## 8 \<q\> tag

```
here is a <q> q tag </q>
```
here is a <q> q tag </q>

## 9 tips

\```tips
This is a tip
\```

```tips
This is a tip
```

## 10 缩写

This is an HTML example.

*[HTML]: Hyper Text Markup Language

```
*[HTML]: Hyper Text Markup Language
```
这里`HTML`应该会全篇搜索然后匹配关联

## 11 脚注

```
This is a text with a footnote[^2].

[^2]:
    And here is the definition.
    > With a quote!
```

This is a text with a footnote[^2].

[^2]:
    And here is the definition.
    > With a quote!
