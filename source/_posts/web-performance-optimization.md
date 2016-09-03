---
title: 前端性能优化知识总结（一）
date: 2016-07-31 20:24:56
categories: Summary
tags: [web前端,性能优化]
---

### 一.DNS寻址以及IP地址解析 ###

1.URI：资源标识符，URL和URN是其子集。URL(统一资源定位符)通过描述资源的位置来表示资源；URN通过描述资源的名字来表示资源。

&emsp;URL由三部分组成，以 <a cursor="normal">http://www.hans-leesun.com/xx/xx/index.html</a> 为例:

&emsp;&emsp;1. URL方案(http)：告知web客户端怎样访问资源
&emsp;&emsp;2. (www.hans-leesun.com)：指的是服务器的位置
&emsp;&emsp;3. (/xx/xx/index.html)：资源路径

&emsp;即“**方案://服务器位置/路径**”。

2.下面我们以www.baidu.com为例：

&emsp;&emsp;当你在浏览器中输入上述这个域名的时候，我们先解析(www.baidu.com.):域名是从右向左解析的.先解析的是.，.代表公网，之后解析到了.com 然后是子域名baidu,最后是www。

&emsp;&emsp;很多人都想知道，当在浏览器中输入一个url到浏览器为我们呈现出页面的这个过程中究竟发生了什么，以下是我个人对此的理解：

&emsp;&emsp;当你在浏览器中输入一个url的时候，这个url会在你的浏览器里种下一个cookie。每当你在该页面登录的时候，该页面会发送的请求会带着这个cookie一起到服务器，服务器会根据这些信息帮你返回你个性化定制的界面。如果你是第一次打开这个网页，则浏览器会发送请求到存储该url的DNS的服务器，服务器会返回一个响应，浏览器则根据这个响应确定接下来应该如何渲染页面或者进行其他的操作。

&emsp;&emsp;如果看官们觉得不够清楚明晰，可以移步[这里](http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/ "Answer")或者[这里](http://stackoverflow.com/questions/2092527/what-happens-when-you-type-in-a-url-in-browser/ "Stackoverflow Answer")。

### 二. 前端页面渲染流程 ###

1.HTML页面请求：
&emsp;1. HTML文档下载
&emsp;2. HTML文档解析：
&emsp;&emsp;1. DOM树生成：elements窗口展现的（所有的元素）
&emsp;&emsp;2. Render树生成：（页面中见到的）
&emsp;&emsp;3. 逐级解析DOM树：尽量减少页面的reflow
&emsp;&emsp;&emsp;1. 回流：
&emsp;&emsp;&emsp;2. 重绘：
&emsp;&emsp;&emsp;3. document.write会阻塞通道，不建议使用。（通道是指每个浏览器本身的能力）
&emsp;&emsp;&emsp;4. 图片加载可以显示用分辨率较低的图片
2.浏览器的JS引擎：
&emsp;1. V8（C++）是谷歌发布的开源JS引擎
&emsp;2. SpiderMonkey是Mozilla项目的一部分
&emsp;3. rhino(Java)使用纯Java写成的JS开源代码实现。它最常用于嵌入Java应用程序，一边为终端用户提供脚本的能力。

<!--more-->

### 三. Yahoo军规 ###
1.尽可能减少HTTP请求。
&emsp;HTTP请求：从客户端到服务器端的请求信息，包括消息首行中，对资源的请求方法、资源的标识符以及使用的协议。
2.使用CDN（内容分发网络）
&emsp;内容分发网络：意思是尽可能避开户粮网上有可能影响数据出书速度和稳定性的瓶颈和环节，使内容传输的更快、更稳定。
3.添加Expire/Cache-Control头
&emsp;Expire头的内容是一个时间值，值就是资源在本地的过期时间、存储在本地。在本地缓存阶段，找到一个对应的资源值，当前时间还没超过资源的国企时间，就直接使用这一个资源，不会发送HTTP请求。

&emsp;Cache-Control是HTTP协议中常用的头部之一，顾名思义：它是负责控制页面的缓存机制。如果该头部指示缓存，缓存的内容也会存在本地，操作流程和Expire相似，但也有不同的地方。Cache-Control有更多的选项，而且也有更多的处理方式。

&emsp;总的来说是一个依靠一个返回值来建立一个定时间的缓存，以避免用户重复发送HTTP请求。
4.启用Gzip压缩
&emsp;在服务器端对文件进行压缩，减小文件的体积
5.将CSS放在页面的最上面
6.将JS放在页面最下面
7.避免在CSS中使用Expressions
8.把JS和CSS放到外部文件中
9.减少DNS查询
10.压缩JS和CSS
11.避免重定向：
&emsp;1. 301 永久重定向 被移动到了另外的位置
&emsp;2. 302 临时重定向 将来的访问依旧会用到老的URL
12.移除重复的脚本
13.配置实体标签（ETag）
&emsp;Etag：实体标签（支持HTTP协议，受WEB服务支持），使用特殊的字符串来表示某个请求资源版本。
&emsp;为资源绑定Etag标签，当请求发送时，服务器会检查该标签已查明请求的资源是否与本地缓存的资源相同。若相同，则调用本地缓存即可，这样一来可以大大减轻服务器端的压力。
14.使用AjAx缓存
&emsp;1. POST：每次都执行、不被缓存
&emsp;2. GET：同一地址不重复执行、可以被缓存

### 四. 加载方式（**提升用户体验**） ###
1.同步加载：
&emsp;1. 可能很少的TCP连接就能完成页面的加载
&emsp;2. 都加载完成才能展示给用户想看的
&emsp;3. 重要的东西同步加载，不重要的东西异步加载
2.分级加载：
&emsp;1. 同步加载和异步加载想结合
&emsp;2. 先给用户加载重要信息比如logo/核心功能，后面加载不重要的。
3.按需加载：
&emsp;1. 用户不触发我们就不加载
&emsp;2. 用户不触发，但是带宽限制或者页面的主要元素都加载完了，我们可以后加载；但是如果用户触发该功能，模块的加载时间就提前

### 五. BigPipe ###
&emsp;使用页面元素标签作占位符，与后端进行配合添加内容
&emsp;同步加载时，返回一些空的占位符，在源代码中可查。依据占位符中的id，发起请求将返回的内容显示到占位符中。

缺点：
  
&emsp;1. 异步请求较多
&emsp;2. SEO引擎较难抓取
&emsp;3. 模块之间相互通信引用
&emsp;4. 模板引擎重复渲染

### 六. 优化术语 ###

1.技术类
&emsp;1. 首屏时间（加载到第一屏的功能点，所消耗的时间点）
&emsp;2. 白屏时间（从进入页面到head解析的时间）
&emsp;3. 可操时间（与模块相关，主要是测试核心模块的使用率，以及用户感知）
&emsp;4. 连通率（多为视频站点。时间为纵轴，主要是对应时间用户看到视屏或者听到声音的比例）
&emsp;5. 前两个都是针对整个页面的，第三个是针对模块的
2.产品类
&emsp;1. pv一次访问一次pv（地址栏中输入一次url就是一次pv）
&emsp;2. uv多次访问同一个人一次uv（uv是去重的pv）
&emsp;3. day日活跃用户数量（Google Analytics）
&emsp;4. MAU月活跃用户人数
&emsp;5. 跳出率（跳出时间留下来的人/pv）

### 七. SEO ###
**Search Engine Optimization：搜索引擎优化**
1.白帽SEO：
&emsp;&emsp;1. 网站标题、关键字、描述
&emsp;&emsp;2. 网站内容优化
&emsp;&emsp;3. Robot.txt文件
&emsp;&emsp;4. 网站地图
&emsp;&emsp;5. 增加外链作用
2.黑帽SEO 不提倡的

对于前端工程师，在工作中主要是对网站结构以及代码进行SEO优化
 
1.扁平化结构：
&emsp;&emsp;1. 控制首页链接数量
&emsp;&emsp;2. 扁平化的目录层次
&emsp;&emsp;3. 导航SEO优化：面包屑导航（让用户了解当前所处位置、使用户可以了解网站的那内容形式）
&emsp;&emsp;4. 一个页面最好不要超过100k
2.代码SEO优化：
&emsp;&emsp;1. &lt;title&gt;标题：关键词放在前面
&emsp;&emsp;2. &lt;meta keywords&gt;关键词
&emsp;&emsp;3. &lt;meta description&gt;网页描述
&emsp;&emsp;4. H1~H6标签多用于标题，文章标题采用H1
&emsp;&emsp;5. UL标签多用于无序列表；OL标签多用于有序列表；DL标签用于定义数据列表
&emsp;&emsp;6. em,strong表示强调
&emsp;&emsp;7. &lt;br&gt;用&lt;p&gt;包裹
&emsp;&emsp;8. 为&lt;a&gt;添加rel="nofollow"
&emsp;&emsp;7. 重要内容HTML代码放在最前面
&emsp;&emsp;8. 重要内容不要用JS输出
&emsp;&emsp;9. 尽少使用iframe框架
&emsp;&emsp;10. 谨慎使用display:none;
&emsp;&emsp;11. 不断精简代码