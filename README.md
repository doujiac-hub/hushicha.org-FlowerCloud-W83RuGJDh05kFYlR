
在JavaScript中，new关键字的应用可以说是再平常不过了，最基础的有new Array()、new Set(),再而就是new一个自己创建的构造函数，也就是创建一个该构造函数的示例。如：var person1 \= new Person("一颗苹果", 18\);但你是否真的了解new以及它的底层原理呢，本文将举出几个例子并且手写一个 new 来带大家深入理解。


# **new**


## new的使用方法




```
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}

const car1 = new Car('Eagle', 'Talon TSi', 1993);

console.log(car1.make);   //输出: "Eagle"
```


## 那么new的过程发生了什么呢




* **创建一个空对象**：创建一个空的简单 JavaScript 对象。为方便起见，我们称之为 `newInstance`。
* **指定原型链**：将 `newInstance` 的 \[\[Prototype]] 指向构造函数的 `prototype` 属性，否则 `newInstance` 将保持为一个普通对象，其 \[\[Prototype]] 为 `Object.prototype`。
* **更改this指向**：使用给定参数执行构造函数，并将 `newInstance` 绑定为 this 的上下文。
* **返回值**：返回`newInstance。`



 


## 手写一个new



```


|  | function myNew(Fun, ...args) { |
| --- | --- |
|  | let obj = {} |
|  | obj.__proto__ = Fun.prototype |
|  | Fun.apply(obj, args) |
|  | return obj |
|  | } |
|  | function Person(name,age) { |
|  | this.name = name |
|  | this.age = age |
|  | } |
|  | let apple = myNew(Person, '一颗苹果','18') |
|  | console.log(apple.name);   //输出：一颗苹果 |
|  | console.log(apple.age);    //输出：18 |


```



* **...args**：将剩下的元素都放进`args`中
* **obj. \_\_ proto \_\_ \= Fun.prototype**：将 `obj` 的原型链指向构造函数的 `prototype` 属性
* **Fun.apply(obj, args)**：将构造函数内部的this绑定到`obj`上，并执行构造函数


*通过这一系列操作，我们就可以拿到构造函数中的 `name`和 `age`属性了*。


### 看似完成了，但其实落了很重要的一点


回想一下，我们通过new函数创建一个对象时，除了引用它的属性，我们还会做些什么呢？


```


|  | let arr = new Array() |
| --- | --- |
|  | arr.push(4) |
|  | arr.unshift(3) |
|  | console.log(arr); |


```

 当然是使用它自带的方法，但是上面并没有给出方法上的引用。这就得引入原型链这个知识点了，我们只需要给Person的原型链上增加我们想要的方法就可以了，实现代码如下：



```


|  | function myNew(Fun, ...args) { |
| --- | --- |
|  | let obj = {} |
|  | obj.__proto__ = Fun.prototype |
|  | Fun.apply(obj, args) |
|  | return obj |
|  | } |
|  | function Person(name,age) { |
|  | this.name = name |
|  | this.age = age |
|  | } |
|  | Person.prototype.who = function () { |
|  | console.log('我是Person的实例对象'); |
|  | } |
|  | Person.prototype.myname = function () { |
|  | console.log(this.name) |
|  | } |
|  | let p = myNew(Person, '一颗苹果') |
|  | p.who();      // 输出：我是Person的实例对象 |
|  | p.myname();   // 输出：一颗苹果 |
|  | console.log(p.name);  // 输出：一颗苹果 |


```


*这就是我们的最终答案了，把它交给面试官，perfect！！*


 本博客参考[veee加速器](https://liuyunzhuge.com)。转载请注明出处！
