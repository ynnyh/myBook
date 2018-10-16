---
layout: post
title: codewars每日一题 
tags: js 题
date: 2018-09-04
---


### 1. 以指定规则将指定字符串转换为指定格式如下
```
// 将'AAA'转换为'TTT'
// 将'TTT'转换为'AAA'
// 将'ATTGC'转换为'TAACG'

// 解法1
function DNAstrand(dna){
    const compire = {
	    A: 'T',
		T: 'A',
		C: 'G',
		G: 'C',
	};
	return dna.split('').map(i => compire[i]).join('');
}
// 解法2
function DNAstrand(dna){
    return dna.replace(/./g, i => ({A:'T',C:'G',G:'C',T:'A'}[i]));
}
// 解法3
function DNAstrand(dna) {
    return dna.replace(/./g, i => 'ACGT'['TGCA'.indexOf(i)]);
}
```
### 2. 将给定的两个数字相加，将最后的值转为二进制，并且为字符串（可以先转换后相加也可以先相加后转换）
```
function addBinary(a, b) {
return (a + b).toString(2);
}
// 此处使用了Number.prototype.toString(radix)方法，其中radix指定要用于数字到字符串的转换的基数（2-36），不写则默认为10
```
### 3. 计算求和（[附链接](https://www.codewars.com/kata/55fd2d567d94ac3bc9000064)）
```
// 解法1
function rowSumOddNumbers(n) {
    let a;
    let arr = [];
    if (n === 1) {
        return 1;
    }
    for (let i = 1; i < n; i++) {
        arr.push(i);
    }
    let num = arr.reduce((item, node) => {
        return item + node;
    });
    a = 2 * num + 1;
    if (n % 2 === 1) {
        let b = a + 2 * (n - 1);
        let c = (a + b) * ((n - 1) / 2);
        return c + (a + b) / 2;
    } else {
        let b = a + 2 * (n - 1);
        let c = (a + b) * (n / 2);
        return c;
    }
}

// 解法2
function rowSumOddNumbers(n) {
  return Math.pow(n, 3);
}
```
### 4. 给定一个字符串，如果其长度是奇数则返回中间字符，如果是偶数则返回中间两个字符如下
```
// 'test' return 'es'
// 'eat' return 'a'
// 'a' return 'a'
// 解法1
function getMiddle(s)
{
  if (s.length <= 2) {
    return s;
  } else {
    if (s.length % 2 === 1) {
      return s.split('')[(s.length - 1) / 2];
    } else {
      return s.split('')[s.length / 2 - 1] + s.split('')[s.length / 2];
    }
  }
}
// 解法2
function getMiddle(s)
{
  return s.substr(Math.ceil(s.length / 2 - 1), s.length % 2 === 0 ? 2 : 1);
}
```
### 5. 将给定字符转换为驼峰法，有可能是段划线-或下划线_
```
// 解法1
function toCamelCase(str){
  if (str.length === 0) {
    return '';
  } else {
    let arr = str.indexOf('_') === -1 ? str.split('-') : str.split('_');
    let f = arr.splice(0, 1);
    let newArr = [];
    arr.map(item => {
      let a = item.substr(0, 1).toUpperCase() + item.substr(1);
      newArr.push(a);
      return newArr;
    });
    return f + newArr.join('');
  }
}
// 解法2
function toCamelCase(str){
  return str.split(/-|_/g).map((w, i) => (i > 0 ? w.charAt(0).toUpperCase() : w.charAt(0)) + w.slice(1)).join('');
}
// 解法3
function toCamelCase(str){
  return str.replace(/[-_](.)/g, (_, c) => c.toUpperCase());
}
```
### 6. 按照如下规律进行解题
```
 persistence(39) === 3 // because 3*9 = 27, 2*7 = 14, 1*4=4
                       // and 4 has only one digit

 persistence(999) === 4 // because 9*9*9 = 729, 7*2*9 = 126,
                        // 1*2*6 = 12, and finally 1*2 = 2

 persistence(4) === 0 // because 4 is already a one-digit number
// 解法1
function persistence(num) {
   let a = num.toString().split('');
   let b = 0;
   function sum(sumNum) {
     if (sumNum.length <= 1) {
       return b;
     }
     b++;
     a = sumNum.reduce((accumulator, currentValue) => {
        return accumulator * currentValue;
    }).toString().split('');
    if (a.length > 1) {
      sum(a);
    }
    return b;
  }
  sum(a);
  return b;
}
// 解法2
function persistence(num) {
   var times = 0;
   
   num = num.toString();
   
   while (num.length > 1) {
     times++;
     num = num.split('').reduce((a, b) => a * b).toString();
   }
   
   return times;
}
```
### 7. 给定任意数字，求该数字的阶乘结果最后有几个0，如下
```
// 给定数字5，则得到5*4*3*2*1=120有一个0
// 解法（运用原理是质因数分解即仅2*5=10后面有0）
function zeros (n) {
  let count = 0;
  let a = 5;
  while (n / a >= 1) {
    count += Math.floor(n / a);
    a *= 5;
  }
  return count;
} 
```