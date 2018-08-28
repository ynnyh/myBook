---
title: call 和 apply 的区别
tags: JS 设计模式
grammar_cjkRuby: true
---


> call和apply的作用一模一样，区别在于传入参数形式不一样

* apply接受两个参数，第一个参数指定了函数体内this对象的指向，第二个参数为一个带下标的集合，这个集合可以是数组，也可以是类数组，apply方法把这个集合中的元素座位参数传递给被调用的函数。
```
const func = (a, b, c) => console.log([a, b, c]);
func.apply(null, [1, 2, 3]); // 输出[1, 2, 3]
```
* call传入的参数数量不固定,跟apply相同的是，第一个参数也是代表函数体内的this指向，从第二个参数开始往后，每个参数被依次传入函数。
```
const func = (a, b, c) => console.log([a, b, c]);
func.call(null, 1, 2, 3); 输出[1, 2, 3]
```