---
title: 常见的CSS技巧（二）
date: 2016-06-14 16:21:59
categories: Skill
tags: [web前端,CSS]
---

### 使用CSS实现三角形 ###
#### 1.CSS实现三角形的原理 ####

首先我们先来看一段代码：

html代码:

```html
    <div class="triangle">CSS实现三角形原理</div>
```

css代码:
```css
.triangle {
    width: 200px;
    height: 100px;
    text-align: center;
    line-height: 100px;
    margin: 10px auto;
    border-width: 30px;
    border-style: solid;
    border-top-color: #8ef26e;
    border-bottom-color: #4a56f2;
    border-left-color: #f24ae8;
    border-right-color: #e2464a;
}
```
效果如下如所示：

<img src="http://o8ptzqvlm.bkt.clouddn.com/image/css-triangle/triangle-principle.PNG" title="CSS实现三角形原理" style="display:block;margin:0 auto">

从图中我们可以看到，当一个元素的`border-width`达到一定宽度时，在每条边的交界处会出现**均分现象**，即这个元素的每个角都被形成这个角的两条边框所平分。产生这个现象的原因为：**矩形框的四个角并不单独属于某条边，而是由形成它的两条边所共有**。正因为有了这几条外边框，才会形成各个角。

<!--more-->

我们还可以看一个更极端的例子：

<img src="http://o8ptzqvlm.bkt.clouddn.com/image/css-triangle/triangle-unusual.PNG" title="triangle-unusual" style="display:block;margin:0 auto">
这个例子的实现代码如下：

```
<div class="triangle-unusual"></div>
/*CSS样式*/
.triangle-unusual {
    width: 0;
    height: 0;
    border: 50px;
    margin: 0 auto;
    border-style: solid;
    border-top-color: #8ef26e;
    border-bottom-color: #4a56f2;
    border-left-color: #f24ae8;
    border-right-color: #e2464a;
}
```
现在我们已经了解了CSS生成三角形的原理，接下来就看几个实例吧。
#### 2.第一类三角形 ####
html代码：
```html
<div class="triangle1 .clearfix">
    <div class="triangle1-1 fl"></div>
    <div class="triangle1-2 fl"></div>
    <div class="triangle1-3 fl"></div>
    <div class="triangle1-4 fl"></div>
</div>
```
CSS代码：
```css
/*清除浮动*/
.clearfix {
    zoom: 1; 
}
.clearfix:after {	
    content: '';
    display: block;
    clear: both;
}
/*左浮动*/
.fl {
    float:left;
}
/*三角形样式*/
.triangle1 {
    width: 400px;
    height: 120px;
    margin: 10px auto;
    border: 5px solid #f0f0f0;
    border-radius: 10px;
}
.triangle1-1 {
    width: 0;
    height: 0;
    margin: 10px;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
    border-bottom: 50px solid #434353;
}
.triangle1-2 {
    width: 0;
    height: 0;
    margin: 10px;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
    border-top: 50px solid #434353;
}
.triangle1-3 {
    width: 0;
    height: 0;
    margin: 10px;
    border-top: 50px solid transparent;
    border-bottom: 50px solid transparent;
    border-right: 50px solid #434353;
}
.triangle1-4 {
    width: 0;
    height: 0;
    margin: 10px;
    border-top: 50px solid transparent;
    border-bottom: 50px solid transparent;
    border-left: 50px solid #434353;
}
```
效果如下所示：

<img src="http://o8ptzqvlm.bkt.clouddn.com/image/css-triangle/triangle-implement1.PNG" title="第一类三角形实现" style="display:block;margin:0 auto">

这里我们以triangle1-1（上图中的一个，等腰三角形）为例来阐述这一类三角形的实现原理：
首先大家需要**注意**的是：使用CSS实现三角形时，你所看到的这些三角形都是由该元素的`border`构成的。

接下来为大家介绍一下样式中各个部分设置的具体原因：

- 设置元素的`width`与`height`为0，保证实现过程中页面中元素所呈现的都是`border`部分的内容
- `border-bottom: 50px solid #434353`: 设定了这个三角形的高度
- `border-left: 50px solid transparent`与`border-right: 50px solid transparent`： 设定了这个三角形的宽度

<img src="http://o8ptzqvlm.bkt.clouddn.com/image/css-triangle/triangle1-1-explaination.png" title="triangle1-1-explaination" style="display:block;margin:0 auto">

通过F12在Chrome浏览器的控制台中可以看到：**triangle1-1**这个三角形是生成于一个**100x50**的矩形之内的。这里的 **100**是由`border-left`与`border-right`的`width`之和构成。由之前所阐述的原理可知：**triangle1-1** 这个三角形的**左下角**是由`border-bottom`与`border-left`形成；**右下角**由`border-bottom`与`border-right`形成；顶角由`border-bottom`、`border-left`与`border-right`共同形成。最后在页面中看到的这个三角形，其实只是`.triangle1-1`这个`div`元素的`border-bottom`在页面中的显示。

如果想要改变三角形的形状，可以通过修改样式中各个`border`的`width`来实现。

了解了`triangle1-1`三角形的实现原理，其余三个三角形的实现原理也很简单，大家可以动手尝试一番。
#### 3.第二类三角形 ####
html代码：
```html
<div class="triangle2 .clearfix">
    <div class="triangle2-1 fl"></div>
    <div class="triangle2-2 fl"></div>
    <div class="triangle2-3 fl"></div>
    <div class="triangle2-4 fl"></div>
</div>
```
CSS代码：
```
/*清除浮动*/
.clearfix {
    zoom: 1; 
}
.clearfix:after {	
    content: '';
    display: block;
    clear: both;
}
/*左浮动*/
.fl {
    float:left;
}
/*三角形样式*/
.triangle2 {
    width: 300px;
    height: 80px;
    margin: 10px auto;
    border: 5px solid #f26e6e;
    border-radius: 10px;
}
.triangle2-1 {
    width: 0;
    height: 0;
    margin: 10px;
    border-top: 50px solid transparent;
    border-left: 50px solid #434353;
    /*
    另一种实现方式:
    border-bottom: 50px solid #434353;
    border-right: 50px solid transparent;
    */
}
.triangle2-2 {
    width: 0;
    height: 0;
    margin: 10px;
    border-bottom: 50px solid transparent;
    border-left: 50px solid #434353;
    /*
    另一种实现方式:
    border-top: 50px solid #434353;
    border-right: 50px solid transparent;
    */
}
.triangle2-3 {
    width: 0;
    height: 0;
    margin: 10px;
    border-top: 50px solid transparent;
    border-right: 50px solid #434353;
    /*
    另一种实现方式:
    border-bottom: 50px solid #434353;
    border-left: 50px solid transparent;
    */
}
.triangle2-4 {
    width: 0;
    height: 0;
    margin: 10px;
    border-bottom: 50px solid transparent;
    border-right: 50px solid #434353;
    /*
    另一种实现方式:
    border-top: 50px solid #434353;
    border-left: 50px solid transparent;
    */
}
```
效果图如下：

<img src="http://o8ptzqvlm.bkt.clouddn.com/image/css-triangle/triangle-implement2.PNG" title="第二类三角形实现" style="display:block;margin:0 auto">

这里我们以`triangle2-1`（上图中第一个）为例阐述这类三角形（等腰直角三角形）的实现原理：

- 设置元素的`width`与`height`为0，保证实现过程中页面中元素所呈现的都是`border`部分的内容
- `border-top: 50px solid transparent`: 设定了这个三角形的高度
- `border-left: 50px solid #434353`：设定了这个三角形的宽度
- 这是一个直角边为50px的等腰直角三角形

<img src="http://o8ptzqvlm.bkt.clouddn.com/image/css-triangle/triangle2-1-explaination.png" title="triangle2-1-explaination" style="display:block;margin:0 auto">

通过F12在Chrome浏览器的控制台中可以看到：**triangle2-1**这个三角形是生成于一个**50x50**的矩形之内的。这个三角形的直角是由于`border-left`被缺失的`border-bottom`截去了一半所生成的；而三角形的两个锐角则是由`border-left`与`border-top`所生成的。（这里大家可能理解起来比较困难，之后我会补充一个综合图例给大家看，相信大家看完之后再回过头来看这个就会好懂许多）。 **triangle2-1**这个三角形的另一种实现方式（代码中注释部分）原理为：`border-bottom`与被缺失的`border-left`截取生成直角；而两个锐角则是由`border-bottom`与`border-right`生成。

剩余的三个等腰直角三角形的实现原理与`triangle2-1`是类似的，且每一种三角形都有两种实现方法。

修改三角形的形状依旧是通过修改样式中`border`的`width`来实现。
#### 4.组合实例 ####

最后，通过一个组合实例，来辅助大家理解等腰直角三角形的实现。

html代码:

```
<div class="triangle-example">
    <div class="triangle-example-item triangle-en en1"></div>
    <div class="triangle-example-item triangle-en en2"></div>
    <div class="triangle-example-item triangle-ws ws1"></div>
    <div class="triangle-example-item triangle-ws ws2"></div>
    <div class="triangle-example-item triangle-wn wn1"></div>
    <div class="triangle-example-item triangle-wn wn2"></div>
    <div class="triangle-example-item triangle-es es1"></div>
    <div class="triangle-example-item triangle-es es2"></div>
</div>
```
css代码:
```
.triangle-example {
    width: 100px;
    height: 100px;
    position: relative;
    margin: 10px auto;
}
.triangle-example-item {
    position: absolute;
    width: 0;
    height: 0;
}
.triangle-en {
    /* 直角指向东北方向 */
    border-top: 50px solid #8ef26e;
    border-left: 50px solid transparent;
}
.triangle-ws {
    /* 直角指向西南方向 */
    border-left: 50px solid #4a56f2;
    border-top: 50px solid transparent;
}
.triangle-wn {
    /* 直角指向西北方向 */
    border-top: 50px solid #f24ae8;
    border-right: 50px solid transparent;
}
.triangle-es {
    /* 直角指向东南方向 */
    border-bottom: 50px solid #e2464a;
    border-left: 50px solid transparent;
}
.en1, .ws1 {
    top: 0;
    left: 0;
}
.en2, .ws2 {
    top: 50px;
    left: 50px;
}
.wn1, .es1 {
    top: 0;
    left: 50px;
}
.wn2, .es2 {
    top: 50px;
    left: 0;
}
```
效果图如下：

<img src="http://o8ptzqvlm.bkt.clouddn.com/image/css-triangle/triangle-example.PNG" title="triangle-example" style="display:block;margin:0 auto">

其中，wn(west-north)表示西北，ws(west-south)表示西南，en(east-north)表示东北，es(east-south)表示东南。这里的方位均指的是三角形的直角所朝向的方位。

掌握了上面这些小技巧，下面我们就看一看两种网页中常见的三角形的应用。
#### 5.带有三角形的凸起 ####

html代码:

```
    <div class="triangle-bulge">我有一个小凸起</div>
```

css代码:
```
/* 带有凸起的块 */
.triangle-bulge {
    width: 200px;
    height: 100px;
    margin: 60px auto;
    background-color: #d2362d;
    color: #fff;
    text-align: center;
    line-height: 100px;
    position: relative;
}
.triangle-bulge::before {
    width: 0;
    height: 0;
    content: '';
    position: absolute;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
    border-bottom: 50px solid #b92dd2;
    top: -50px;
    left: 50px;
}
```

效果图如下：

<img src="http://o8ptzqvlm.bkt.clouddn.com/image/css-triangle/triangle-bulge.PNG" title="triangle-bulge" style="display:block;margin:0 auto">

#### 6.带有三角形的凹陷 ####

html代码:

```
    <div class="triangle-depressed">我有一个小凹陷</div>
```

css代码:
```
/* 带有凹陷的块 */
.triangle-depressed {
    width: 200px;
    height: 100px;
    line-height: 80px;
    margin: 20px auto;
    background-color: #143dd7;
    color: #fff;
    text-align: center;
    position: relative;
}
.triangle-depressed:after {
    width: 0;
    height: 0;
    content: '';
    position: absolute;
    left: 0;
    bottom: 0;
    border-left: 100px solid transparent;
    border-right: 100px solid transparent;
    border-bottom: 20px solid #fff;
}
```

效果图如下：

<img src="http://o8ptzqvlm.bkt.clouddn.com/image/css-triangle/triangle-drepressed.PNG" title="triangle-drepressed" style="display:block;margin:0 auto">

上面的两个例子中只用到了一个`div`元素，通过`before`和`after`伪对象构造三角形结构，调整三角形的形状以及伪对象的位置，就可以让我们的小三角出现在任何你想出现的位置了。

--------

以上便是我个人总结的使用CSS实现三角形的一些技巧。

下面是以上源码的DEMO下载地址：<a href="http://o8ptzqvlm.bkt.clouddn.com/web/triangle/triangle.rar">点我下载</a>

相较于后端，前端更容易看到自己代码的运行结果。结合Chrome控制台以及FireBug等调试工具，开动你的双手，相信你很快就能掌握这个小技巧！加油！