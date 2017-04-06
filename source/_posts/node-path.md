---
layout: post
title:  "node.js的path模块"
categories:
    - node
tags: [node,nodejs]
permalink: node-path
---
## node之path模块
在node API中的`path`对象使用频率很高，掌握了有助于我们处理好一些文件或文件夹的路径。以此记录，共勉。
```js
//引用该模块
var path = require("path");
```
## 1、路径解析，得到规范化的路径格式

```js
//对window系统，目录分隔为'\', 对于UNIX系统，分隔符为'/'，针对'..'返回上一级；/与\\都被统一转换
//path.normalize(p);

var myPath = path.normalize(__dirname + '/test/a//b//../c/utilyou.mp3');
console.log(myPath); //windows: E:\workspace\NodeJS\app\fs\test\a\c\utilyou.mp3
```

## 2、路径结合、合并，路径最后不会带目录分隔符
```js
//path.join([path1],[path2]..[pathn]);
/**
 * [path1] 路径或表示目录的字符，
 */

var path1 = 'path1',
    path2 = 'path2//pp\\',
    path3 = '../path3';

var myPath = path.join(path1, path2, path3);
console.log(myPath); //path1\path2\path3
```

## 3、获取绝对路径
```js
//path.resolve(path1, [path2]..[pathn]);

//以应用程序为起点，根据参数字符串解析出一个绝对路径

/**
 * path 必须至少一个路径字符串值
 * [pathn] 可选路径字符串
 */

var myPath = path.resolve('path1', 'path2', 'a/b\\c/');
console.log(myPath);//E:\workspace\NodeJS\path1\path2\a\b\c
```

## 4、获取相对路径

```js
//path.relative(from, to);
//获取两路径之间的相对关系

/**
 * from 当前路径，并且方法返回值是基于from指定到to的相对路径
 * to   到哪路径，
 */

var from = 'c:\\from\\a\\',
    to = 'c:/test/b';

var _path = path.relative(from, to);
console.log(_path); //..\..\test\b; 表示从from到to的相对路径
```

## 5、path.dirname(p)

```js
// 获取路径中目录名

var myPath = path.dirname(__dirname + '/test/util you.mp3');
console.log(myPath);
```

## 6、path.basename(path, [ext])
```js
// 获取路径中文件名,后缀是可选的，如果加，请使用'.ext'方式来匹配，则返回值中不包括后缀名；

var myPath = path.basename(__dirname + '/test/util you.mp3', '.mp3');
console.log(myPath);
```

## 6、path.basename(path, [ext])
```js
// 获取路径中文件名,后缀是可选的，如果加，请使用'.ext'方式来匹配，则返回值中不包括后缀名；

var myPath = path.basename(__dirname + '/test/util you.mp3', '.mp3');
console.log(myPath);
```

## 7、path.extname(path)
```js
// 返回路径文件中的扩展名
var myPath = path.extname('/foo/bar/baz/file.txt');
console.log(myPath); // .txt
```

## 8、path.sep
```js
//返回对应平台下的文件夹分隔符，win下为'\'，*nix下为'/'：
var url1 = path.sep;
var url2 = 'foo\\bar\\baz'.split(path.sep);
var url3 = 'foo/bar/baz'.split(path.sep);

console.log('url1:',url1);  // win下为\，*nix下为/
console.log('url2:',url2);  // [ 'foo', 'bar', 'baz' ]
console.log('url3:',url3);  // win下返回[ 'foo/bar/baz' ]，但在*nix系统下会返回[ 'foo', 'bar', 'baz' ]
```

## 9、path.delimiter
```js
//返回对应平台下的路径分隔符，win下为';'，*nix下为':'：
var path = require('path');
var env = process.env.PATH; //当前系统的环境变量PATH

var url1 = env.split(path.delimiter);

console.log(path.delimiter); //win下为“;”，*nix下为“:”
console.log('env:',env);  // C:\ProgramData\Oracle\Java\javapath;C:\Program Files (x86)\Intel\iCLS Client\;
console.log('url1:',url1);  // ['C:\ProgramData\Oracle\Java\javapath','C:\Program Files (x86)\Intel\iCLS Client\']
```
