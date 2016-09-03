---
title: 常见的CSS技巧（一）
date: 2016-06-14 14:37:49
categories: Skill
tags: [web前端,CSS]
---

### 常见的水平垂直居中方法 ###
1. 水平居中：
	1.内联元素：
	``` 
		text-align: center
	```
	2.块级元素：
	```
		margin: 0 auto
	```
	3.绝对定位的元素:
	```
		position:absolute;
		left: 50%;
		margin-left: -width/2;
	```
2. 垂直居中:
	1.line-height: 在css中设置`line-height:height`,即可让元素中的单行文本垂直居中
	2.table-cell:在css中设置元素的`display:table-cell`以及`vertical-align:middle`
	3.绝对定位的元素:
	```
		position:absolute;
		top: 50%;
		margin-top: -height/2;
	```
<!--more-->

### 常见的reset样式 ###
```css
body,div,dt,dd,ul,ol,li,input,h1,h2,h3,h4,h5,h6,textarea,a,p,form {
    margin: 0;
    padding: 0;
    font-family: "Helvetica Neue",Helvecita Arial,sans-serif,Microsoft Yahei;
}
a {
    text-decoration: none;
    color: "选择PSD中的颜色"
}
input, select, textarea {
    outline: none;
    border: none;
}
textarea {
    resize: none;
}
ul, ol, li {
    list-style: none;
}
img {
    border: none;
}
/*清除浮动*/
.clearfix {
    /*这里的设置是为了兼容IE6以及IE7*/
    zoom: 1;
}
.clearfix:after {
    content: "";
    display: block;
    clear: both;
}
/*全新的清除浮动技术*/
/*这里的:before伪类是为了防止元素与元素之间出现的margin重叠现象*/
/*即：在页面中，一些元素的margin-bottom会与另一些元素的margin-top重叠*/

.clearfix:before,
.clearfix:after {
    content: " ";
    display: table;
}
.clearfix:after {
    clear: both;
}
.clearfix {
    /*这里的设置是为了兼容IE6以及IE7*/
    zoom: 1;
}
/*推荐使用上述新技术*/
```
&emsp;&emsp;随着技术的不断进步，已经有了更好的方式代替reset.css,即[Normalize.css](http://necolas.github.io/normalize.css/ "Normalize.css")解决方案。Normalize相对平和，注重通用的方案，重置掉该重置的样式，保留有用的user agent样式，同时进行一些bug的修复。推荐在以后的开发中使用这种新的技术。

### 文本超出元素框后显示省略号 ###
```
	display: block;
	overflow: hidden;
	white-space: nowrap;
	text-overflow: ellipsis;
```