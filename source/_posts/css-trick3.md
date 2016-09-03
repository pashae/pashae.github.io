---
title: 常见的CSS技巧（三）
date: 2016-07-31 00:07:23
categories: Skill
tags: [web前端,CSS]
---

### 一、辨识px,em,rem ###

1. px：1px在浏览器中即为1个像素

2. em：相对单位长度
	1.相对参照物为父元素的font-size；
	
	2.具有继承的特点；

	3.当所有的元素都没有设置font-size时，浏览器会有一个默认的em设置：1em = 16px。（这里的16px为chrome浏览器默认的字体大小）

3. rem：相对单位长度
	1.相对参照物为根元素html

	2.参照物相对固定，如果html没有设置font-size，则浏览器会有一个默认的rem设置：1rem = 16px。

4. 在chrome浏览器下，一般设置html元素的font-size为62.5%，即10px。在之后的样式代码书写中，如果需要用到em或者rem这样的相对单位长度设置，这样的安排会使后续的代码显得更为简单。

5. 这里有一个小知识点需要注意一下，**中文版**的chrome浏览器有一个<font color='red'>**最小字号限制**</font>，即在**中文版**的chrome浏览器下，默认显示的最小字号为<font color='red'>**12px**</font>；而在**英文版**的chrome下，这个限制为<font color='red'>**10px**</font>

### 二、diplay:none与visibility:hidden之间的区别 ###

&emsp;&emsp;通常情况下，我们有两种隐藏元素的方式，一种为`display:none`,另一种为`visibility:hidden`。虽然这两种方法的效果相同，但二者本质上还是有一定的区别的。以下面的代码为例（测试环境为chrome浏览器，51版）：

<!--more-->

html代码：
	```
	<div class="test">
	    <div class="item1">item1</div>
	    <div class="item2">item2</div>
	    <div class="item3">item3</div>
	</div>
	```
css代码：
```
.test {
    width: 200px;
    height: 200px;
    margin: 20px auto;
    border: 2px solid #ccc;
}
.item1 {
    width: 100px;
    height: 30px;
    margin: 5px auto;
    border: 2px solid #ffcc00;
    background-color: #ffcc00;
}
.item2 {
    width: 100px;
    height: 30px;
    margin: 5px auto;
    border: 2px solid #ffee00;
    background-color: #ffee00;
    visibility: hidden;
}
.item3 {
    width: 100px;
    height: 80px;
    margin: 5px auto;
    border: 2px solid #ff00cc;
    background-color: #ff00cc;
    display: none;
}
```
效果如下：

<img src="http://o8ptzqvlm.bkt.clouddn.com/image/css3/CSS3-ALL.PNG" title="CSS3-ALL" style="margin:0 auto;display:block">

&emsp;&emsp;接下来，我们去掉`.item3`的`display:none`样式以及`.item2`的`visibility: hidden`样式之后，各元素在页面中的展示为：

<img src="http://o8ptzqvlm.bkt.clouddn.com/image/css3/CSS3-none.PNG" title="CSS3-none" style="margin:0 auto;display:block">

&emsp;&emsp;此时，我们再将`.item2`的`visibility: hidden`样式添加至元素中，我们惊奇地发现：`.item2`元素不见了：

<img src="http://o8ptzqvlm.bkt.clouddn.com/image/css3/CSS3-2visibility.PNG" title="CSS3-2visibility" style="margin:0 auto;display:block">

&emsp;&emsp;紧接着，我们按下F12查看究竟出了什么问题，将鼠标至于`.item2`元素上，我们可以看到，本应出现在页面中属于`.item2`元素的内容没有了，但是`.item2`元素在页面中所占据的位置还保留着：

![CSS3-2visibility-cursor](http://o8ptzqvlm.bkt.clouddn.com/image/css3/CSS3-2visibility-cursor.png)

&emsp;&emsp;我们带着疑问继续下面的实验。为了方便做对比，我们使`.item2`元素显示，并设置`.item3`的`display`属性为`none`,“以防万一”，我们这次直接打开开发者工具并选中`.item3`,效果如下：

![CSS3-3display](http://o8ptzqvlm.bkt.clouddn.com/image/css3/CSS3-3display.png)

&emsp;&emsp;从上图中可以看出，设置了`display:none`样式的`.item3`元素已经彻底从页面中消失了！

&emsp;&emsp;通过上面的这个例子，我们可以得出以下结论：

> &emsp;&emsp;当我们为一个元素设置	`display:none`样式时，网页在渲染的过程中，不会将该元素加入到DOM树结构中，该元素在页面中彻底的消失了；而当我们设置`display`属性不为`none`时，该元素会重新被渲染到页面中，并且出现在该元素在原本DOM结构中的位置。

> &emsp;&emsp;当我们为一个元素设置	`visibility: hidden`样式时，网页在渲染的过程中会按照正常的DOM节点对该元素进行渲染，而该元素除了应有的位置结构会被保留在文档中外，其他一切样式都会被页面所“隐藏”，看起来就像是真的**hidden**了一样。
