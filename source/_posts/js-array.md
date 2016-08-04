---
title: javascript中的数组(Array)对象的使用
categories:
    - javascript
tags: [javascript]
---
# Js Array的使用

今天系统性的重温下Array对象的知识

## 定义
数组的定义：一个存储元素的线性集合，元素可以通过索引来任意存取，索引通常是数字，用来计算元素之间存储位置的偏移量。即数组对象用来在单独的变量名中存储一系列的值。

## 使用数组

### 创建数组
- 通过 ``var arr1 = [1,2,3]``
- 通过 new ``var arr2 = new Array(1,2,3)`` (注意：如果里面传入的是一个数字，比如10，是定义一个length为10的数组，每一个元素都是``undefined``)

### 读写数组

```js
    // 读取数组
    var arr = [1,2,3,4];
    for(var i=0;i<arr.length;i++){
        console.log(arr[i]);
    }

    //改变数组值
    var arr1 = [];
    for(var i=0;i<10;i++){
        arr1[i] = i;
    }
```
我们可以用``[]``读取到索引的数组，同时也可以对他进行赋值改变的操作。

### 由字符串生成数组
调用字符串对象``split()``方法通过一个字符串生成一个数组

```js
var str = 'hello world stone';
var arr = str.split(' ');
console.log(arr)  //["hello", "world", "stone"]
```


http://qiutc.me/post/javascript-array.html
https://github.com/TongchengQiu/blog/edit/source/source/_posts/javascript-array.md
