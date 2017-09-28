---
title: Angular实践
date: 2017-09-24 11:13:55
tags:
---
项目地址：[JD][1]
---
项目截图
---
![此处输入图片的描述][2]
---
![此处输入图片的描述][3]
---
![此处输入图片的描述][4]
---
![此处输入图片的描述][5]


---
最近react许可证风波闹的沸沸扬扬，虽然最终facebook改了许可证，但是我还是乘势搞了一波Angular

---
简单的对比
```
Angular

首先Angular的背后是Google（难道这就是官网被墙的原因？），所以社区基础是不用担心的，整个生态也已经是非常的完整了，从最基本的Tutorial到StackOverflow的问题数到框架本身的剖析都有非常非常多，所以从这个角度看起来Angular应该算是上手比较容易的。不过Angular目前的问题看起来也很明显1. 性能   同样是TODOMVC的Sample，Angular完全载入用了1.1s（WebPagetest - Visual Comparison）。目前我用到的基于Angular的就是Kibana，不得不说，确实挺慢的。。2. Angular 2.0  Angular的2.0几乎是一个推翻重做的框架，估计不会有1.X的upgrade方案。所以如果现在新开始的项目采用Angular的话，会是一个很尴尬的时机。同样，如此大的改动似乎也反面印证了1.X并不是那么好。

但是现在angular也已经升级到了4.0  而且基本用法也几乎与angular2.0一致，性能问题以不再是我们不使用angular的借口了

react

React很大的特点就是“轻”，再加上VDOM这个很好的idea让React非常非常快（在上面那个测试里面0.3s左右就载入完毕）。另外React和Angular一个很大的不同就是React采用的是one-way data flow。React的缺点嘛，大概就是现在还太新了很难说将来有没有大的API变化，目前在大的稳定的项目上采用React的，我也就只知道有Yahoo的Email。所以现在很少有批评React的声音也许不是他真的就没有坑，而是那些坑还没有被踩出来而已。还有就是React本身只是一个V而已，所以如果是大型项目想要一套完整的框架的话，也许还需要引入Flux和routing相关的东西。React的routing我没有研究过，但是Flux的话已经有出现一些批评的声音了。

总结
Angular是真正的大而全的framework，他有自己一套思路，基本你follow这个思路往里面填代码就OK。
React是一个简短有力的library，他只负责解决你某个单一的“痛点”。
```

---
接下来介绍一下用法

```
初始化
先要全局安装 angular-cli
npm install angular-cli -g

生成项目
ng new XXX(给你的项目取个名字)

然后全部安装好后
ng serve 启动项目
然后就能在浏览器中打开 查看你的项目
```

---
具体用法

```
angular提供了  ng g c xxx 命令用来生成template
它会自动生成相应的文件，并进行引用，这样就不用你手动创建文件，并一一去引用，有助于提高效率
```

---
常用的指令，值的绑定，以及事件的监听和组件的交互

---
ngIf
```
<user-center *ngIf="showUserCenter()"></user-center>

用来控制组件的显示与隐藏，这里要提一点，这里的显示隐藏式通过类似于设置 display:none的方式实现的，而不是visibility:hidden;
```

---
ngFor
```
<div class="goods_item" *ngFor="let item of itemArr">
这样可以创建一组组件，语法很简洁，但是 遍历的对象只能是一个数组 而不能是对象，当然对象你可以自己写个方法也是可以实现的
```

---
值绑定

```
首先在js里面定义值
@Input() data:any;
然后就可以在模板里面进行值绑定了
<div class="attr-name">{{data.choices.name}}</div>

这里的值绑定也可以是一个方法返回的值
同样现在js里面定义一个方法
public getBtnStr():string{
    console.log(this.mode);
    return this.mode == "count" ? "编辑" : "完成";
}
然后进行绑定
<span>{{this.getBtnStr()}}</span>
```

---
事件监听

```
(click)="onBtnClick()"

括号里面是事件的名称 后面的方法就是用来处理事件，事件的名称跟原生的是一样的，当然你可以用原生的写法，但是那样就太麻烦了
```

---
组件通信

----
父组件  传消息给子组件

```
这个就没什么好讲的了 最简单就是  通过改变 你 绑定 给子组件的值进行通讯

还有一个就是 使用viewChild

先通过viewChild获取到对应的子组件
@ViewChild(ModRecommendComponent)
private modComponent:ModRecommendComponent;

然后你就可以在父组件中 使用子组件了
```

---
子组件传递消息给父组件
```
这个需要用到EventEmmiter

先是要在子组件将EventEmmiter定义好
@Output()
public eventEmmiter:EventEmitter<any> = new EventEmitter<any>();

发送事件
public onBtnClick(){
    this.eventEmmiter.emit({type:'changeMode',data:{}});
}

在父组件模板写上对应的事件监听
<mod-address [(mode)]="mode" (eventEmmiter)="eventFromAddress($event)"></mod-address>
```

----
兄弟，祖宗节点之间的监听
```
有时候需要需要交互的组件并非父子，并且嵌套的层次可能比较多，再用上面的方式去做就会显得很麻烦

这个时候我们就用到了service

定义service
import {Injectable} from '@angular/core';
import {Observable} from 'rxjs/Observable';
import {Subject} from 'rxjs/Subject';

@Injectable()
export class EventBusService{
    public eventBus:Subject<any> = new Subject<any>();

    constructor(){
        
    }
}

在需要发送事件的组件中传入service
constructor(public eventBusService:EventBusService) { }

发送事件
this.eventBusService.eventBus.next({
      describe:this.describe,
      price:this.price,
      imgSrc:this.imgSrc,
      weight:this.weight,
      choices:this.choices
    })

在其他组件中进行监听
ngOnInit(){
    this.eventServiceBus.eventBus.subscribe((value)=>{
        this.cModToBuy = value;
    })
}
这个我们可以写在 钩子函数 ngOnInit中，我也推荐大家这么做，至于为什么我也不知道，我随便写的，哈哈
```

---
未完待续

```
今天就先介绍到这里，这些都是最基本的用法，足够你去做一些简单的页面
以后有时间，我会再介绍 rxjs router module http等模块及技术点
```

[1]: https://github.com/FantasticPerson/JD
[2]:http://odpdls7ab.bkt.clouddn.com/blog/blog4/img1.png
[3]:http://odpdls7ab.bkt.clouddn.com/blog/blog4/img2.png
[4]:http://odpdls7ab.bkt.clouddn.com/blog/blog4/img3.png
[5]:http://odpdls7ab.bkt.clouddn.com/blog/blog4/img4.png