---
title: angular实现动态表单
date: 2018-02-22 15:15:17
tags: angular js dynamicform typescript fullstack
intro: angular动态表单适用场景，以及实现思路。
---
> 这边文章主要介绍angular动态表单的实现思路。具体实现细节可以参考社区里semlinker的[动态创建表单](https://segmentfault.com/a/1190000009186703)这篇文章，以及他推荐的参考资源 [Configurable Reactive Forms in Angular with dynamic components](https://toddmotto.com/angular-dynamic-components-forms)，笔者这篇文章主要是以上文章的部分翻译和思考。

##### [查看demo源码](https://github.com/Karin001/ngx-dynamic-form)
##### [查看demo演示](http://ngx-dynamic-form-demo.herokuapp.com/)
![](https://ws1.sinaimg.cn/large/bbad0914gy1fopc0h1d1pj20ku08q74h.jpg)
------
### <a name="1">动态表单使用场景</a>
有时候我们需要一个灵活的表单，这个表单可以根据用户的选择，或者服务器返回的信息进行重新配置，比如：增加或删除一组input元素、一组select元素，等等。

在这样的情况下，如果一开始就在模板里写下所有的表单，利用一个ngif树状结构进行选择控制，程序会变得比较冗余。

这时。程序最好是能够根据用户的选择（driven by configuration）或者服务器的响应，自动生成所需要的表单。这就是动态表单要处理的业务。

----------------

### 组件生成的相关概念
- #### 组件的两个构成
要动态生成表单，需要先理解组件是如何生成的。

一个angular组件由两部分所组成。
1. **Wrapper**  
Wrapper能够与组件进行交互，当一个Wrapper初始化完成后，就已经帮我们实例化了一个组件。同时，它也负责组件的change detection，以及触发钩子函数比如ngOnInit，ngOnChanges。
2. **View**  
View负责呈现渲染过后的模板，将组件的外貌展示出来，并且能够触发Wrapper的change detection。一个组件可以有多个view，每一个view可以通过调用angular提供的两个函数自行生成和销毁，这个过程不用顶层的视图参与。
- #### 组件的通常加载方式（非动态加载方式）

通常情况下，我们都是把组件内嵌到根组件或者另一个组件当中使用。嵌入的组件称为子组件，被嵌入的称为父组件。这时，当我们的子组件代码在被编译时，会生成一个组件工厂**component factory**（这是angular核心类ComponentFactory的一个实例），和一个**hsot view**，host view负责本组件在父组件视图内生成该组件的dom节点，以及生成该组件的wrapper和view。
- #### 动态加载组件
而当我们想要将一个动态组件插入某个组件视图时，则无法取得这个动态组件的实例，因为这些是非动态组件编译器做的事。

-------------------


### 实现动态组件
angular提供了一些函数解决上面的难题，要使用这些函数我们需要注入两个对象。

```
constructor(
    private componentFactoryResolver: ComponentFactoryResolver,
    private viewcontainerRef: ViewContainerRef,
  ) {
      
  }
```
我们注入了ComponentFactoryResolver，和ViewContainerRef。

**ComponentFactoryResolver**上提供了一个方法（resolveComponentFactory()），该方法接收一个组件类作为参数，生成一个基于该组件类的组件工厂，也就是我们之前提到的那个组件工厂。

**ViewContainerRef**提供了一个方法（createComponent()），该方法接收组件工厂作为参数，在该视图中生成子组件。（我个人的理解是它处理了host view所做的事，为组件生成了wrapper和view）

-------------

### 实现动态表单
上文简要的介绍了实现动态组件的一些技术，现在开始思考如何做一个动态表单。

- #### 具体思路 
我们想要做出一个独立的动态表单模块，当我们想要使用动态表单时，只需简单引入这个模块，稍加配置即可使用。

我们希望这个模块做好了后，在顶层使用者的角度会是这样一个工作流程：
![](https://ws1.sinaimg.cn/large/bbad0914gy1fop4wr7sicj20n203ldg0.jpg)

我们可以很容易的做出一个具有输入属性的组件，问题的核心在于这个组件是如何根据输入属性生成我们想要的表单。

也就是说，是它自己调用ComponentFactoryResolver和ViewContainerRef进行组件的动态生成，还是交给别人处理。

下图是实现思路：

![](https://ws1.sinaimg.cn/large/bbad0914gy1fop45fyugcj20j909vjsa.jpg)

1. 实际上我们把动态表单拆分成了一个个小的动态组件（不预先加载），由外层的一个组件充当一个容器，所有的动态组件都会在里面进行生成和销毁，他们共同组成了一个动态表单。

2. 调用ComponentFactoryResolver和ViewContainerRef生成组件的的这部分逻辑没有集成在外层容器中，而是交给了一个自定义的指令和ng-container。因为指令没有视图，他通过注入ViewContainerRef获取到的是宿主的视图容器。由于ng-container不会被渲染，所以获取到的视图容器就是外层组件容器的视图容器。

这么处理的好处就是不需要由外层组件统一对各个拆分的动态组件进行管理，相当于是由动态组件自己进行管理。

外层组件容器大概会是下面这样：
```
<form>
  <ng-container *ngFor="let config of configs" [自定义指令] >
 </ng-container>
</form>
```

> configs是用户的配置数据，自定义指令寄宿在ng-container中，根据config渲染出各自的动态组件，而ng-container是透明的。

看一下代码目录结构，最后会是这个样子

![](https://ws1.sinaimg.cn/large/bbad0914gy1fop76qqx3fj20f608sq3o.jpg)

> 以上就是大体的实现思路了，具体还有许多细节可以关注文章开头提到的那两篇文章，讲的很详细。