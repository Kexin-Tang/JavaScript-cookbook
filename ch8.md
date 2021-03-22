## 对象，类与面向对象

一般而言，定义对象有以下两种方法
```js
let person = new Object();
person.name = "tom";
person.age = 18;
person.sayName = function () {
    console.log(this.name);
}
```
```js
let person = {
    name: "tom",
    age: 18,
    sayName: function () {
        console.log(this.name);
    }
}
```

### 理解对象
1. 属性值简写

如果属性的值是某一个变量，则可以使用变量名作为属性
```js
const name = "tom"
let person = {
    name        // 等同于 name: name，也就是name: "tom"
}
```

2. 方法简写

在定义中，每个方法都需要先写上函数名字，然后加上`:`，再是`function (params) {...}`，简写时可以直接使用函数名和参数列表进行定义
```js
let person = {
    sayName(name) {
        console.log(name);
    }
}
person.sayName("tom");  // "tom"
```

3. 解构

对于一个对象，如果想快速获取其内部的属性和方法并赋值给其他变量，就可以使用解构的方法
> * 可以通过`attributeName: newName`将*attributeName*的值赋给*newName*
> * 可以通过`attributeName=default`来设定预设值，如果有*attributeName*，则忽略预设值
```js
let person = {
    name: "tom",
    age: 18
}
const {name: personName, age, job="cat"} = person;
// personName = "tom"
// age = 18
// job = "cat"
```

---

### 创建对象

#### 工厂函数
```js
function Person(name, age){
    let o = new Object();
    o.name = name;
    o.age = age;
    o.showName = function () {
        console.log(o.name);
    }

    return o;
}
let tom = Person("tom", 18);
```

工厂函数虽然能快速帮助我们创建多个类似的对象，但是由于内部都是`new Object`，所以并不能很好的标识对象的归属。

#### 构造函数
```js
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.showName = function() {
        console.log(this.name);
    }
}
let tom = new Person("tom", 18);
```

调用构造函数时会执行的过程：
```js

```