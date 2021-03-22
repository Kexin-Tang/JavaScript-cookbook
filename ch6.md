## 集合引用类型

### *Object*

1. 定义*Object*类型时有多种方法，如*new*和对象字面量方法
```js
// 下面两种方法等效
let o1 = new Object();
o1.name = "tom";

let o2 = {
    name: "tom"
}
```
2. 除了可以使用`.`来访问属性和方法，还可以使用`[]`，但是内部的标记一定是*string*类型
```js
console.log(o1.name);       // tom
console.log(o2['name']);    // tom
```

---

### *Array*

#### 创建*Array*

1. 构造函数法
```js
let s1 = new Array("tom", "jerry");
let s2 = Array("jack"); // 同上
let s3 = Array(3);      // 此处要注意，这样的语法并不是创建[3]，而是创建一个长度为3的数组！
```
2. 数组字面量
```js
let s = ["jack", 3, "rose", true];
```
3. `from`

用于从类数组结构生成数组。其还可以增加第二个参数，用于对每个数组的元素进行增强。
```js
// 从string直接拆分每个char组成数组
console.log(Array.from("abc")); // ['a', 'b', 'c']

// 对array进行浅复制
let a1 = [1,2,3];
console.log(Array.from(a1));    // [1,2,3]

// 对函数的参数进行组合
function func(person, age){
    console.log(Array.from(arguments));
}
func("tom", 20);    // ['tom', 20]

// 对数组进行增强
console.log(Array.from(a1, (x)=>x**2)); // [1,4,9]
```

4. `of`

`of`是用来代替`Array.prototype.slice.call(arguments)`的，用于将传入的参数组合成一个数组
```js
console.log(Array.of("jack", 3));   // ["jack", 3]
```

#### 数组长度

注意，`length`不仅是可读的，还是可写的。如果显示对`length`修改，会抹除元素/增加*undefined*元素
```js
let a = [1,2,3];
a.length = 2;   // a = [1,2]
a.length = 4;   // a = [1,2,undefined,undefined]
```

#### 迭代器方法

*keys*用于得到索引迭代器; *values*用于得到值迭代器; *entries*用于得到 *[索引，值]* 迭代器
```js
const a = ['a', 'b', 'c'];

const aKeys = Array.from(a.keys()); // [0,1,2]
const aValues = Array.from(a.values()); // ['a', 'b', 'c']
const aEntries = Array.from(a.entries());   // [[0, 'a'], [1, 'b'], [2, 'c']]

// 还可以在for循环中得到索引和值，类似Python的enumerate()
for(const [idx, val] of a.entries()){
    ...
}
```

#### 复制和填充方法

1. `fill()`方法用于填充数据
```js
let a, reset = () => a = [0,0,0,0,0];

a.fill(5);  // [5,5,5,5,5]
reset();

a.fill(5, 2);   // [0,0,5,5,5]
reset();

a.fill(5, 2, -1); // [0,0,5,5,0]
reset();

// 如果表示起始和结束的值超出范围，则会将在数组范围内的元素进行修改
a.fill(5, 2, 10);   // [0,0,5,5,5]
reset();
a.fill(5, -10, -8); // [0,0,0,0,0]
```

2. `copyWithin()`用于将某一部分进行浅复制，并替换一部分内容
```js
let a, reset = () => a = [0,1,2,3];

// 如果只有一个参数，则代表插入的位置
a.copyWithin(2);    // [0,1,0,1]
reset();

// 如果有两个参数，则第一个是插入的位置，第二个是开始copy的位置
a.copyWithin(1, 2); // [0, 2, 3, 3]
reset();

// 如果有三个参数，则第一个是插入位置，第二三个是copy的范围
a.copyWithin(1, 0, 2);  // [0, 0, 1, 3]
reset();
```

#### 转换方法

`join(分割符)`可以将数组中每个元素取出，并用分隔符进行分割，返回新的*string*
```js
const a = [1,2];
console.log(a.join('-'));    // "1-2"
```

#### 栈方法

1. `push()`可以放入任意数量参数至末尾，并返回最新长度
2. `pop()`可以弹出末尾元素，并返回弹出的元素值

#### 队列方法

1. `shift()`可以弹出头部的元素，并返回弹出的元素值
2. `push()`可以在尾部增加一个元素
3. `unshift()`可以在头部增加一个元素
4. `pop()`可以弹出尾部的元素

综上，`shift()`和`push()`构成常规队列；`unshift()`和`pop()`构成反向队列；四者合起来使用，可以构成双向队列。

#### 排序方法

1. `arr.reverse()`用于将数组反序
2. `arr.sort([method])`在默认情况下会将arr中内容先转变成*string*，再进行比较，所以可能会出现奇怪的错误
```js
// 奇怪的排序
const a = [2, 10];
a.sort();   // [10, 2]，因为"10"中'1'排在'2'前面
```
为此，可以给`sort`传入一个函数，用于定义排序原则。该函数有两个参数，当第一个参数要排在前面时，需要返回负数；当第一个参数要排在后面时，需要返回正数。
```js
// 正确定义由小到大排列
// 如果num1>num2，那么num1-num2>0，num1自然就在num2后面；反之亦然
a.sort((num1, num2) => num1-num2);  
```
**注意，这两个方法都会永远的改变原始的数组！！**

#### 操作方法

1. `concat()`用于将多个数组合并到一起
2. `slice()`用于切片得到数组的子串
3. `splice()`
   1. 删除：传入两个参数，要删除的第一个元素的位置和删除的元素数量
   2. 插入：传入三个参数，开始位置，0(代表删除0个)，插入的元素
   3. 替换：传入三个参数，开始位置，删除的元素个数，插入的元素

#### 查找方法

1. `indexOf()`和`includes()`用于查找元素在数组中的位置/是否存在
2. `find()`和`findIndex()`使用了断言函数，这两个方法的第一个参数是一个函数，第二个参数可选，用于指定第一个函数中*this*的指向
```js
const nums = [1,2,3,4,5];
// 三个参数：元素，索引，数组
nums.find((element, index, array) => element>3);
```

#### 迭代方法

1. `every()`对数组每一项都运行传入的函数，如果返回均为true，则返回true
2. `some()`对数组每一项都运行传入的函数，如果有至少一项返回为true，则返回true
3. `filter()`对数组每一项都运行传入的函数，将返回true的项组成为数组后返回
4. `forEach()`对数组每一项都运行传入的函数
5. `map()`对数组每一项都运行传入的函数，返回输出的结构构成的数组

#### 归并方法

`reduce()`方法用于将一串数组压缩成某一个输出
```js
// 实现累加
const nums = [1,2,3];
console.log(nums.reduce((prev, cur) => prev + cur));
```

---

### *Map*

#### 操作

1. `set()`用于添加一个新的键值对
2. `get()`和`has()`用于查询
3. `size`属性用于查询键值对的个数
4. `delete()`和`clear()`用于清除值。其中`clear()`是将整个*map*删除，`delete()`是删除对应的键值对

```js
let m = new Map([['jack', 20]]);
m.set('tom', 18);
m.get('tom');       // 18
m.has('jerry');     // false
m.size;             // 2
```
5. `keys()`，`values()`和`entries()`分别可以得到键、值和键值对
6. `forEach()`中的函数有三个参数，分别是(key, value, map)

#### 特性

与*Object*不同，*Map*的键可以使用任何数据类型！

同样，*Map*在插入元素的时候是有顺序的，所以可以使用各种迭代器等方法遍历。


### *Set*

#### 操作
1. `add()`用于添加一个元素
2. `has()`用于查询
3. `size`属性用于查询键值对的个数
4. `delete()`和`clear()`用于清除值。其中`clear()`是将整个*set*删除，`delete()`是删除元素
5. 由于*set*中只有元素，所以一般只会只用`values()`去遍历
6. 集合可以做交并补等数学操作

---

