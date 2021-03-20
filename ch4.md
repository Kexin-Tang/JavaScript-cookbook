## 变量、作用域与内存

### 原始值 &amp; 引用值

1. 原始值只有6中类型：*Number*, *String*, *Boolean*, *Null*, *Undefined*, *Symbol*
2. 由于JS限制不能访问内存，所以对于保存引用值的变量，我们都只能访问其**引用**
3. 引用值可以随时添加属性和方法；原始值虽然添加了不会报错，但是并不能访问和使用
```js
const ref = new Obejct();
const str = "abcd";
ref.str = "abcd";
str.num = 1;
console.log(ref.str);   // "abcd"
console.log(str.num);   // undefined
```
4. 注意，**原始值的初始化只能使用原始字面量，不能使用*new*!!**
```js
let p1 = new String("tom");
let p2 = "jerry";
p1.age = 10;
p2.age = 10;
console.log(p1.age);    // 10
console.log(p2.age);    // undefined
console.log(typeof p1); // object
console.log(typeof p2); // string
```

#### 复制
对于原始值进行复制，两个内容是相互独立的，修改其中一个不会影响另一个；对引用值进行复制，两个内容是对同一内存的不同引用，修改任意一个都会影响另一个
```js
let num1 = 10;
let num2 = num1;
num1 = 20;
console.log(num1, num2);    // 20 10
let s1 = new String();
let s2 = s1;
s1.name = "tom";
console.log(s1.name, s2.name);  // tom tom 
```

#### 传参
在JS中只有**按值传递**，传入函数时会将内容复制到一个临时空间。但是，既然是复制，那么如果传入的是原始值，则参考原始值的复制；如果是引用值，则参考引用值的复制。(所以JS在传递*Object*时看上去像按引用传递)
```js
function func(obj){
    obj.name = "tom";
};
let p = new Object();
p.name = "jerry";
func(p);
console.log(p.name);    // tom
```

#### 确定类型
对于原始值，可以使用`typeof`，而对于*null*和*object*而言，应该使用`instanceof`
```js
let person = new Object();
console.log(person instanceof Object);  // true
console.log(person instanceof Array);   // false
```

---

### 执行上下文与作用域

1. 变量或函数的上下文决定了他们可以访问哪些数据，以及他们的行为。每个上下文都有一个关联的**变量对象**，上下文中定义的函数和变量都存在于这个对象上
2. 上下文的代码在执行时会创建一个**作用域链**，该链决定了访问变量和函数的顺序(即如果要查找一个名为`a`的变量，先查找xx域，没找到再查找yy域)
3. 通过`try-catch`和`with`可以加强作用域链，即在某个作用域外再套一层额外的作用域

---

### 内存

1. JS内存回收的策略：(1)标记清理; (2)引用计数。
2. 优化内存的最佳手段就是保证执行代码时只保存必要的数据。如果数据不再需要，就设为*null*
3. 内存泄漏: (1)定义全局变量(未使用*var*, *let*或*const*限定某个变量); (2)使用闭包; (3)定时器

---
