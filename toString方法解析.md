---
layout: post
title: toString方法解析 
tags: js toString
date: 2018-09-04
---


### 1. Boolean.prototype.toString()，不需要参数
将布尔值转换为字符串
### 2. Array.prototype.toString()，不需要参数
Array对象覆盖了Object的toString方法，对于数组对象，toString方法可以连接数组并返回一个字符串，其中包含用逗号分隔的每个数组元素
```
const a = [1, 2, 'a', 'b'];
console.log(a.toString()); // 输出'1,2,a,b'
```
### 3. RegExp.prototype.toString()，不需要参数
RegExp 对象覆盖了 Object 对象的 toString() 方法，并没有继承 Object.prototype.toString()。对于 RegExp 对象，toString 方法返回一个该正则表达式的字符串形式。
```
myExp = new RegExp("a+b+c");
alert(myExp.toString());       // 显示 "/a+b+c/"

foo = new RegExp("bar", "g");
alert(foo.toString());         // 显示 "/bar/g"
```
### 4. Number.prototype.toString(radix)
radix 指定要用于数字到字符串的转换的基数(从2到36)。如果未指定 radix 参数，则默认值为 10。
RangeError 如果 toString() 的 radix 参数不在 2 到 36 之间，将会抛出一个 RangeError。
```
var count = 10;

console.log(count.toString());    // 输出 '10'
console.log((17).toString());     // 输出 '17'
console.log((17.2).toString());   // 输出 '17.2'

var x = 6;

console.log(x.toString(2));       // 输出 '110'
console.log((254).toString(16));  // 输出 'fe'

console.log((-10).toString(2));   // 输出 '-1010'
console.log((-0xff).toString(2)); // 输出 '-11111111'
```