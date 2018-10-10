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