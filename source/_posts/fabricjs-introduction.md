---
title: 上手canvas库fabricjs
date: 2018-04-24 15:06:34
tags: fabricjs JavaScript
intro: 简单介绍上手fabricjs的方法，fabricjs能帮我们从繁琐的canvas操作中解放出来，从而快速的实现自己的canvas页面效果。
---

fabricjs是一个canvas的库，原本canvas的操作主要基于上下文，需要使用者自己从0开始去实现一些基本功能。而canvas库文件封装好了许多便利的对象，这些canvas库使得用户可以在比较高级的层面上进行绘制。fabricjs就是其中之一。

> 上手建议：官方api写得比较简洁，建议先看看官方[快速教程][1]，也可以参考这部[中文教程][2]



**使用fabricjs的新手套路**：
    1. 创建fabric的canvas对象。
    2. 创建fabric的各种对象，包括：rect，circle，line，image等。
    3. 将创建好的对象添加到canvas。
    4. 可以对每一个fabric对象使用动画，克隆，修改属性，事件监听，碰撞检测等。修改属性需及时渲染canvas才能展示出效果。   
**fabricjs使用注意要点**：
1. 每次修改对象的参数后需要执行canvas.renderAll()重新渲染，否则不会出效果。
2. fabric对象用acoords参数（bl，br，tl，tr）【大概是topleft，bottomright的意思】表达自己所在canvas中的位置。 每次更改了对象的位置，大小时（比如left，width），若要canvas感知该对象的具体位置（用于对象间碰撞边缘检测，或事件响应），需要使用setCoords方法重设对象的四个角坐标。

**介绍下常用的配置参数，这些配置参数可用来配置fabric对象**
width宽：number,
height高：number,
fill填充颜色：string,
stroke描边颜色:string,
strokeWidth描边宽度：number,
angle角度:number,
left左边距:number,
top上边距:number,
originX横轴中心点:['center'|'left'|'right],
originY纵轴中心点:['center|'top'|'bottom'],
flipX水平翻转:boolean,
flipY垂直翻转：boolean,

更多配置参数请参考：[fabric.Object][3]，以及其子类

**介绍下官方相应的demo**
[事件][4]
[动画][5]
[克隆][6]
[边缘检测][7]
[精灵动画][8]
[分组和取消分组][9]


  [1]: http://fabricjs.com/articles/
  [2]: http://jspang.com/2017/01/11/fabricjsbasic/
  [3]: http://fabricjs.com/docs/fabric.Object.html
  [4]: http://fabricjs.com/events
  [5]: http://fabricjs.com/animation-easing
  [6]: http://fabricjs.com/copypaste
  [7]: http://fabricjs.com/intersection
  [8]: fabricjs.com/animated-sprite
  [9]: http://fabricjs.com/manage-selection