---
title: js-prototype
date: 2018-06-12 13:53:10
tags: js
intro: 本文介绍js原型链实现继承细节以及与es6的class语法对比。
---
> 要理解js的原型链主要就是理清楚以下三者的关系：
- 构造函数的protitype属性
- 对象的\__proto__属性
- 对象的constructor属性



在js中，函数作为一等公民，它是一个对象，可以拥有自己的属性，可以像其他类型（比如string）一样作为参数进行传递，作为返回值进行返回。

我们首先写一个名为Animal的函数。我们将从这个Animal函数开始，通过原型链，构建一条“动物-狗-警犬”的继承关系。

```
 function Animal() {}

```

首先把目光放在这个Animal函数身上。这是一个普通的空函数，当函数被关键字new调用时，我们就说他此刻是作为一个构造函数，通常用首字母大写表示。
接下来实例化这个Animal类

    var animal = new Animal();

这时，js发生了以下过程：
1. 从内存中获取一个空对象{}交给animal。
2. 设置animal.__proto__为Animal.prototype
3. 执行构造函数中的其他操作，比如利用this设置animal的属性等，因为此时this指向的是animal。我们这里是空函数,因此没有操作。
打印animal到控制台。
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/30401125.jpg)

可以看到animal只有一个__proto__属性，并且在__proto__中还有一个__proto__属性，这就是一条原型链，由__proto__组成的原型链。

当在自身属性上没有查找到某个属性时，js就会尝试查找__proto__上有无该属性，不断的向着上级__proto__爬，一直到找到那个属性为止，或者没有找到，返回undefined。

animal的第二级__proto__指向的就是Object。因为所有的对象的原型链顶端都是Object。

在_propto_属性中我们还能看到constructor属性，该属性指向一个构造函数。他们之间的关系即是：构造函数的prototype属性指向了原型，而原型上的constructor又回指构造函数Animal。

**构造函数，实例，原型三者的关系如下图**：
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/1143738.jpg)


构造函数Animal是构成整个原型链的关键，是他利用prototype将原型传给了后代。因此，通过操纵构造函数的prototype，就能够操纵原型链，从而对原型链进行自在的拼接。

现在我们就开始打造Animal->Dog->PoliceDog的原型链。

```
 Animal.prototype.breathe = function() {
    console.log('breathe~');
}
 function Dog(name){
    this.name = name;
}

 function PoliceDog(name,id) {
    Dog.call(this, name);
    this.id = id;
}
 
```

我们在Animal的prototype上设置了一个breathe方法，之后又新建了Dog和PoliceDog这两个构造函数，在构造函数中利用call和this设置了一些自己独有的属性。

现在, Animal,Dog,PoliceDog彼此还没有交集。当然他们的顶级原型都是Object。

    Dog.prototype = new Animal();
    PoliceDog.prototype = new Dog();

上两句代码就将三者的原型撺在了一起。接下来还需要将原型上的constructor进行回指。

    Dog.prototype.constructor = Dog;
    PoliceDog.Prototype.constructor = PoliceDog;

下面继续为Dog和PoliceDog添加一些方法

```
Dog.prototype.constructor = Dog;
Dog.prototype.bark = function(){
     console.log('汪！汪！');
}
PoliceDog.prototype.manhunt = function() {
     console.log(this.name + '向犯罪份子疯狂发动进攻！')
}
PoliceDog.prototype.checkId = function() {
     console.log('警犬'+this.name +'的id是：'+this.id);
}
```

此时我们new一个PoliceDog,并将其打印至控制台。
可以清晰的看到这条原型链。
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/69795339.jpg)


但是我们也注意到在policeDog的第二级原型上继承了name属性，但该属性我们已经在构造函数中为自己设置了，我们不希望在原型上也继承该属性。

会出现这种情况的原因是因为我们在拼接原型时用的是new出来的一个实例。Dog的实例中存在name属性。

**因此可以采用另一种拼接方法。**
PoliceDog.prototype = Object.create(Dog.prototype);

Object.create函数接受一个对象A，并返回一个对象，返回的对象的__proto__为对象A。
形如这个样子：Object.create(对象A)，返回{\__proto__:对象A}
关于Object.create具体的请移步[这里][4]。

修改之后再次打印policeDog的实例
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/83969116.jpg)

可以看到name属性没有出现在原型里。


----------
----------

使用es6实现继承，底层也是使用原型链

```
class Animal {
    breathe() {
        console.log('breathe')
   };
}
class Dog extends Animal {
    constructor(name){
        super();
        this.name = name;
    }
    bark(){
        console.log('汪！汪！');
    }
}
class PoliceDog extends Dog {
    constructor(name,id){
        super(name);
        this.id = id;
    }
    manhunt(){
        console.log(this.name + '向犯罪份子疯狂发动进攻！');
    }
    checkId(){
        console.log('警犬'+this.name +'的id是：'+this.id);
    }
}

// 测试
let policeDog_1 = new PoliceDog('peter','k0302');
console.log(policeDog_1);
```
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/33392944.jpg)

与我们使用Object.create修改原型链达到一样的效果

 
  [4]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create
 