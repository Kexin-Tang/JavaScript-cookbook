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

---

### 数据类型

JS中基本数据类型有7种：*Number*, *String*, *Null*, *Undefined*, *Object*, *Boolean*, *Symbol*

#### *Undefined* 

当声明了一个变量却没有初始化时，该变量的值即为*undefined*。
```js
let a;
console.log(typeof a);  // undefined
```

#### *Null*

*null* 表示空对象指针。如果要定义一个变量用来在后面保存数据，但是初始时不确定具体值时，可以使用 *null* 来赋值，以区分 *undefined*。

> 注意，`null == undefined`是成立的，而且`typeof null`返回的是 *Object*

#### *Boolean*

有`true`和`false`两种结果，可以使用`Boolean()`对某些数据进行转换：

| 数据类型  |   true   |   false   |
| :-------: | :------: | :-------: |
|  Boolean  |   true   |   false   |
|  String   |   非空   |    ""     |
|  Number   |   非0    | 0 和 NaN  |
|  Object   | 非空对象 |   null    |
| Undefined |    无    | undefined |

#### *Number*

1. 八进制使用`0o`，十六进制使用`0x`
2. 如果出现`1.`和`12.0`这种的，会自动存储为整数
3. 浮点数是有精度的，会出现`0.1+0.2 != 0.3`
4. 使用`Number.MIN_VALUE`和`Number.MAX_VALUE`可以获取最小/最大值，使用`Number.NEGATIVE_INFINITY`和`Number.POSITIVE_INFINITY`可以获取负无穷/正无穷
5. *NaN* 代表 *Not a Number*，用于表示本来要返回数值的操作失败
   1. 任何带有 *NaN* 的操作返回都是 *NaN*
   2. `NaN != NaN`
   3. 使用`isNaN()`来判断是否为 *NaN*，该函数会想办法先将传入的数据转成数字，如`isNaN('10')`返回为`true`，因为先将`'10'`&rarr;`10`
6. 数值转换
   1. *Number()*
      1. true &rarr; 1; false &rarr; 0
      2. null &rarr; 0
      3. undefined &rarr; NaN
      4. "" &rarr; 0
      5. "00112" &rarr; 112
      6. "1.3415" &rarr; 1.3415
      7. "0xF" &rarr; 15
      8. "123abc" &rarr; NaN
   2. *parseInt() & parseFloat()*: 从第一个非空格开始，如果不是+，-以及数字，则直接返回NaN，依次检查，直到遇到非数字。
      1. true/false &rarr; NaN
      2. null &rarr; NaN
      3. undefined &rarr; NaN
      4. "" &rarr; NaN
      5. "00123" &rarr; 123
      6. "1.34" &rarr; 1.34
      7. "1.34.5" &rarr; 1.34
      8. "123abc" &rarr; 123

#### *String*

1. 字符串是不可变的，一旦创建就不能改变。如要修改某个值，需要把原始字符串全部销毁，然后再赋予新值。
2. 转换字符串使用`toString()`方法(*null*与*undefined*没有该方法)
3. `String()`函数会先判断，如果有`toString()`，则调用；否则返回*null*或者*undefined*
4. 标签函数可以让用户根据字符串中插入的数据进行处理
```js
// strings 保留除去${}剩下的部分，此处strings=["", " is a "]
// person, age分别对应剩余的${}中内容
const myTag(strings, person, age){
    let info = null;
    if(age>18){
        info = "adult";
    } else {
        info = "child";
    }
    return strings[0] + person + strings[1] + info;
};

let person = "tom";
let age = 19;
const res = myTag(`${person} is a ${age}`); // tom is a adult
```

#### *Symbol*

1. *Symbol* 类似于DB中的主键，其创建一个独一无二的标识，用于区分不同的内容
2. 可以在 `Symbol()` 中传入`str`作为注释，但是即使传入的`str`相同，仍为两个不同的 *Symbol*
```js
const s1 = Symbol('symbol');
const s2 = Symbol('symbol');
console.log(s1 === s2); // false
```
3. 使用`Symbol.for(str)`可以注册一个全局*symbol*，全局*symbol*中使用相同的`str`会得到相同的*symbol*
```js
const s1 = Symbol.for('symbol');
const s2 = Symbol.for('symbol');
console.log(s1 === s2); // true
const s3 = Symbol('symbol');
console.log(s1 === s3); // false
```
4. 使用`Symbol.keyfor(obj)`可以查找`obj`对象对应的*symbol*
```js
const s = Symbol.for('symbol');
console.log(Symbol.keyfor(s));  // symbol
```

#### *Object*

1. 对象是一组功能和属性的集合，使用 *new* 进行实例化
2. *Object* 类是所有派生类的基类，其有如下属性和方法：
   1. *constructor* 用于创建当前对象
   2. *hasOwnProperty(propertyName)* 用于查询当前类是否含有 *propertyName* 的属性
   3. *isPrototypeOf(objectName)* 用于判断当前类是否为 *objectName* 的原型
   4. *propertyIsEnumerable(propertyName)* 用于属性可否用`for-in`进行枚举
   5. *toString()* 用于返回字符串表示
   6. *valueOf()* 返回对应的字符串、boolean和数值

---

### 操作符

1. 在进行运算符操作的时候，如果运算的不是*Number*，则会先调用各种方法转换成*Number*再进行运算

2. `+`比较奇特，如果有一方为*String*，则会将另一方转变成*String*，然后进行*String*的追加

3. `==`会先将两边进行类型转换，再判断；`===`会直接判断
```js
NaN == NaN;         // false
"10" == 10;         // true
"10" === 10;        // false
0 == false;         // true
0 === false;        // false
undefined == null;  // true
undefined === null; // false
```

---

### 语句

#### if

#### while

#### for
`for(initialization; condition; update)`
* *for-of* 用于遍历**可迭代对象**
```js
const nums = [1, 2, 3];
for (const num of nums){
    console.log(num);
}
```
* *for-in* 用于遍历**对象中的key**
```js
const info = {
    name: "tom",
    age : 18
};
for (const i in info){
    console.log(i + ': ' + info[i]);    // name: tom
                                        // age: 18
}
```


#### switch
与C语言中*switch*类似，有几个不同点：
1. switch的选择条件可以是非数字，如*String*甚至是表达式
2. case的标签也可以是变量或表达式
3. switch中比较case标签时，使用的是`===`，不会做类型转换

#### with
*with* 用于将代码作用域设定成指定的对象
```js
// let qs = location.search.substring(1)
// let host = location.hostname;
// let url = location.url;
with (location){
    let qs = search.substring(1);
    let host = hostname;
    let url = url;
}
```

---
