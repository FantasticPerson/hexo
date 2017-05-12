---
title: 5分钟上手react
date: 2016-10-23 15:26:22
tags: web
---
git地址：[react-starter][1]

运行步骤：
---

 1. 安装node
 
 2. 进入项目根目录，运行
    `npm install`
    有可能这条命令运行会很慢，我建议先安装 cnpm
    `npm install cnpm -g`
    然后再运行
    `cnpm install`

 3. 运行
    `npm run dev`
    通过浏览器打开  http://localhost:3999/#/demoPage
    下面就是运行效果：
    ![此处输入图片的描述][2]

好了，上面就是怎么把项目跑起来的全过程！！！

内容分析：
---
![此处输入图片的描述][3]
源代码主要放在web这个目录底下

***下面主要介绍一下 router 和 redux***
## router ##
主要看routes这个目录
![此处输入图片的描述][4]
index.js
![此处输入图片的描述][5]
`path:'/'`对用渲染的组件为 indexApp ,对应的URL为http://localhost:3999/#/
`onEnter`：进入这个路由之前需要干什么写在这里面，最后调一下`cb()`真正进入这个路由
`onLeave`：自然表示的就是离开这个路由之后做的事情
`childRoutes`：表示当前这个路由底下的子路由
我们看一下demoPage里面怎么定义的：
![此处输入图片的描述][6]
根据里面的定义 http://localhost:3999/#/demoPage 就是子路由对应的URL
## redux ##
简单的说redux就是用来管理数据流的一个工具
首先我们来看两张图
![此处输入图片的描述][7]
![此处输入图片的描述][8]
store是一个单一对象
![此处输入图片的描述][9]
![此处输入图片的描述][10]

actions可以理解为向store传递数据信息
![此处输入图片的描述][11]
![此处输入图片的描述][12]
reducer实际上就是一个函数，根据action来更新state
![此处输入图片的描述][13]
![此处输入图片的描述][14]
通过console里面的log也可以看到
![此处输入图片的描述][15]

下面看一个具体的例子：
![此处输入图片的描述][16]
通过上面这段将数据绑定到当前的组件上面
![此处输入图片的描述][17]
上面的log显示title是'origin title'
所以当前界面显示为
![此处输入图片的描述][18]
通过点击按钮  click to change title
出发dispatch操作 
![此处输入图片的描述][20]
此时更新的数据问 'new title' + 当前时间戳
如底下log所示
![此处输入图片的描述][19]
最后界面更新为如下所示：
![此处输入图片的描述][21]

好的 差不多就这样了吧。。。


  [1]: https://github.com/wdd119/react-starter
  [2]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog1.PNG
  [3]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog2.PNG
  [4]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog4.PNG
  [5]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog3.PNG
  [6]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog5.PNG
  [7]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog6.jpg
  [8]: http://odpdls7ab.bkt.clouddn.com/blog/blog2/blog7.jpg
  [9]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog9.PNG
  [10]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog10.PNG
  [11]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog13.PNG
  [12]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog14.PNG
  [13]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog11.PNG
  [14]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog12.PNG
  [15]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog15.PNG
  [16]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog19.PNG
  [17]: http://odpdls7ab.bkt.clouddn.com/bog/blog1/blog17.PNG
  [18]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog16.PNG
  [19]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog22.PNG
  [20]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog20.PNG
  [21]: http://odpdls7ab.bkt.clouddn.com/blog/blog1/blog21.PNG