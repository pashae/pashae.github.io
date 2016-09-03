---
title: 常见的CSS技巧（四）
date: 2016-08-16 23:35:22
categories: Skill
tags: [web前端,CSS]
---

### 一、inline-block

&emsp;&emsp;display属性设置为inline-block的元素，会在每一个元素中间产生一个4px的间距（chrome下为8px），各个浏览器之间产生的这些空白并不相同，具体情况具体讨论。产生这个间距的原因不是浏览器出现了bug，而是浏览器将代码中的空白符渲染出来导致的。

&emsp;&emsp;接下来我们上代码吧：

html代码：
```html
<ul>
    <li>我是一号</li>
    <li>我是二号</li>
    <li>我是三号</li>
    <li>我是四号</li>
</ul>
```
css代码：
```css
* {
    margin: 0;
    padding: 0;
}
ul {
    width: 500px;
    height: 200px;
    margin: 50px auto;
    background-color: #ccc;
}
ul li {
    display: inline-block;
    width: 70px;
    height: 60px;
    margin-top: 50px;
    background-color: #ffcc00;
    text-align: center;
    line-height: 60px;
}
ul li:first-child {
    margin-left: 80px;
}
ul li + li {
    border-left: 1px solid #FFF;
    margin-left: -8px;
}
```

&emsp;&emsp;在浏览器中打开我们写好的页面（本例为chrome浏览器中文版，版本号为51）,并打开开发者调试工具。

&emsp;&emsp;通常情况下，页面为我们展示的效果是下面这样的：

<!--more-->

![CSS4-initial](http://o8ptzqvlm.bkt.clouddn.com/image/css4/CSS4-initial.PNG)

&emsp;&emsp;从上图中可以看到，在未设置任何margin与padding的情况下，display属性为inline-block的li元素之间出现了间隔。上面我们已经提到，这个间隔是由于浏览器在渲染页面的过程中将各个li元素之间的空白间隔也渲染至页面中所导致的。

&emsp;&emsp;那如何去除这些间隔呢？通常有以下两种途径：

&emsp;&emsp;&emsp;1. 消除各个li元素之间的空白符，有以下几种方式可供选择：
```html
<ul>
    <li>我是一号
    <li>我是二号
    <li>我是三号
    <li>我是四号
</ul>

<ul>
    <li>我是一号
    </li><li>我是二号
    </li><li>我是三号
    </li><li>我是四号
    </li>
</ul>

<ul>
    <li>我是一号</li><li>我是二号</li><li>我是三号</li><li>我是四号</li>
</ul>
```
&emsp;&emsp;&emsp;上述这种消除空白符的方法简单有效但是所写的代码异常混乱，对后期代码的维护十分不利。当然，如果代码结构并不复杂，也可以使用这种方式。

&emsp;&emsp;&emsp;另外还有一种方式，即设置包裹li的ul的font-size为0即可。

&emsp;&emsp;&emsp;2. 采用css技巧来消除li元素之间的间隔。具体操作如下：为li元素添加样式`ul li + li {margin-left: -8px;}`，其中-8px为chrome浏览器下，而在safari浏览器中则需要设置`letter-space:-Npx`。（没有水果机的博主不知道这个值是多少:joy: :joy: :joy:）效果如下：

![CSS4-8px](http://o8ptzqvlm.bkt.clouddn.com/image/css4/CSS4-8px.PNG)

&emsp;&emsp;&emsp;为了打消大家的疑虑，下面为大家展示一组设置为`margin-left: -7px`，为了增加对比度，特意把li元素的背景颜色改为白色，结果如下：

![CSS4-7px](http://o8ptzqvlm.bkt.clouddn.com/image/css4/CSS4-7px.PNG)

&emsp;从上图中可以清晰的看到，li元素之间依然存在着<font style="color:red">**1px**</font>（经实际测量）的间隔。

&emsp;&emsp;在平常的开发过程中，我们通常会遇到在各个li元素之间插入一个竖直线条的设计。解决这种设计的方法通常有以图片的方式插入，或者用span标签等来代替。经过我们上面的探讨，在这里提出一种新的方式解决这种设计：为li元素设置样式为`ul li + li {margin-left: -8px;border-left:1px solid #fff} `，效果如下：

![CSS4-final](http://o8ptzqvlm.bkt.clouddn.com/image/css4/CSS4-final.PNG)

&emsp;可以看到，我们已经成功地在各个li之间添加了宽度为1px的白色线条。:smile: :smile: :smile:

---
>&emsp;&emsp;以上这个小知识点可能大家平时写代码的时候不是特别注意，我也是偶然间发现的，现在贴出来，供大家一起交流学习。:muscle: :muscle: :muscle: