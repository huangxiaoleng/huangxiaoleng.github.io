---
layout: post
title: 单行文本两端对齐
---

# 需求

曾经遇到过一个单行文本两端对齐的需求，具体情况如下：

条件：

1. 单行显示（高度固定）；
2. 每行的字数不固定（但不会超过宽度）；

要求：

1. 两端对齐；

# 方案

一开始找到一个方案，基本能满足需求，但是，得在每行文字的末尾加入一个空`span`元素。后面，找到一个新的方案，克服了空`span`元素这个缺点：

CSS：
```css
div.justify {
    text-align: justify;/* required */
    height: 18px;/* required */
    width: 200px;
    margin-bottom: 20px;
    font-size: 14px;
    color: red;
    border: 1px solid blue;
}

/* required */
.justify:after {
    content: "";
    display: inline-block;
    width: 100%;
    height:0;
    font-size:0;
    line-height: 0;
}
```
HTML：
```html
<div class="justify">中 文 两 端 对 齐</div>
```

## 兼容性：

ie8+

## 缺点：

1. 为了兼容ie，中文文本得加入空格
2. 得根据具体的使用场景处理好高度问题，而这个高度的问题[本质原因](https://stackoverflow.com/a/23038536/8762513)如下：

> text-align: justify doesn't justify the last line. Then, you can add a pseudo-element with width:100% which will fall to the next line, so the line you want to justify won't be the last one and it will be justified. – Oriol Apr 13 '14 at 3:09

引文大意：justify 对最后一行文本不起作用。

所以，单行文本被认为是最后一行，直接对其使用`text-align:  justify`是没有两端对齐效果的。我们希望单行文本两端对齐的话，就要让它变成不是最后一行，so，添加一个宽度100%的`after`伪类来充当这最后一行。无法避免地，高度被撑开了，所以如果使用场景能对容器设置固定高度的话，倒是可以克服缺点2。

【之所以对最后一行文本不起作用，这个原理其实很容易理解。正常的一行，单词都是有一定数量的，碰巧遇到换行的情况，尾部剩余的空间一般不会特别大，使用两端对齐的话，空间平均给整行的内容，整体的效果不会变化太大。但是，如果最后一行恰好只有很少的的几个单词，尾部剩余大量的空间，此时还渲染成两端对齐，那这一行的效果就很难看了。所以，不对最后一行使用两端对齐，是合情合理的。】

# 参考来源：
- [https://jsfiddle.net/leaverou/h37zk/](https://jsfiddle.net/leaverou/h37zk/) 【这个是leaverou（《CSS揭秘》一书的作者）的方案】