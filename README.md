# 简介

本文基于《JavaScript高级程序设计》(第四版)，作为学习过程中的笔记。希望我能学会JavaScript。

## 什么是JavaScript

JavaScript包括三个主要部分：
1. ECMAScript: 主要定义了JS的语法和关键字等信息；
2. DOM: 文档对象模型，提供了与网页内容进行交互的API；
3. BOM: 浏览器对象模型，提供了与浏览器进行交互的API；

---

## HTML中的JavaScript

1. HTML中包含JS可以使用`<script></script>`，可以在内部写入JS代码，也可以设置`src`连接外部文件(该文件可以是存储在本机的文件，也可以是网络上的代码仓库)
2. 在不设置`defer`和`async`的情况下，所有的`script`会按顺序被解释
3. 可以把`<script>`放在`<head>`中，但是由于网页在解析到`<body>`前都不会进行页面渲染，所以可以通过把`<script>`放到`<body>`的尾部
4. 使用`defer`属性，让页面先下载script但是等待页面渲染完成后再解释
5. 使用`async`可以实现类似功能，但是此时script就不一定按照预设的顺序执行
6. 使用`<noscript>`可以指定在浏览器不支持脚本时的内容；如果浏览器支持脚本，则`<noscript>`会被无视

---

## 语言基础

### 变量声明

#### var

1. *var* 的作用域是**函数作用域**，这并不意味着无法在`{}`外访问到变量
```js
// 函数作用域
function func(){
    var a = 10;
    console.log(a); // ok
}
console.log(a); // error

// 块级作用域，此时var不会被阻碍
if(true){
    var b = 10;
}
console.log(b); // ok
```

2. 提升(hoist)：变量的声明会被提前到作用域开头，但是赋值仍停留在原地
```js
// var a;
// console.log(a);
// a = 10;
console.log(a); // undefined
var a = 10;
```

3. *var* 可以重复定义
```js
var a = 10;
var a = 20;
```

#### let

1. *let* 的作用域是**块级作用域**
2. *let* 不能被重复定义
```js
// error
let a = 10;
let a = 20; 
// error，重复定义并不会跟使用什么声明关键字有关，关键字不同不代表类型有别
let b = 10;
var b = 20; 
```
3. *var* 在全局作用域定义的变量会成为 *window* 的属性，而 *let* 不会
```js
var a = 1;
console.log(window.a);  // 1
let b = 2;
console.log(window.b);  // undefined
```

> for循环泄露问题
> ```js
> // 最终输出: 0 1 2 3 4 | 5 5 5 5 5
> // 因为setTimeout是异步函数，会放入任务队列中
> // 所以每次放入任务队列后，执行console.log(i)可以正常输出i
> // 当setTimeout从队列中取出时，var i已经为5，所以输出5个5
> for (var i=0; i<5; i++){
>     setTimeout(()=>console.log(i), 0);
>     console.log(i);
> };
> // 最终输出: 0 1 2 3 4 | 0 1 2 3 4
> // 因为let不能重复定义，所以每次给setTimeout的都是一个数据备份
> // 然后先放入队列，再执行console.log(i)，注意每一个循环的i其实都是不一样的
> // 当setTimeout从队列中取出时，里面存储的仍然是数据备份0 1 2 3 4
> for (let i=0; i<5; i++){
>     setTimeout(()=>console.log(i), 0);
>     console.log(i);
> }
> ```


#### const

1. *const* 在声明时就要赋值，且不允许更改指向。

注意是不允许更改指向，比如说`const a=[1,2,3]`，那么 *a* 就和该 *Array([1,2,3])* 绑定，对 *Array([1,2,3])* 进行处理不会报错，但是更换 *a* 的指向则报错。
```js
const a = [1,2,3];
a.push(4);  // ok
a = [5,6];  // error，因为更改了指向
```

2. *const* 声明在循环中不可以作为迭代变量，但是可以用在`for-of`与`for-in`中，代表该变量在函数内部不可被更改指向
```js
// error
for(const i =0; i<5; i++){...};
// ok
for(const i of [1,2,3]){...};
for(const key in {a: 1, b: 2}){...};
```
