## 基本引用类型

1. 虽然在JS中定义引用类型的时候很像别的语言中的*class*，但是并不是类
2. 对象是某个特定引用类型的实例

### 原始值包装类型

在JS中，提供了*Number*,*Boolean*和*String*这三种原始值的引用类型。每当使用这三种原始值的方法和属性时，都相当于在后台新建了对应的原始值包装类型，然后调用这些包装类型的方法和属性。
```js
// 下列第二行语句等效于三个操作：
// let temp = new String(s1);
// let s2 = temp.substring(2);
// temp = null;
let s1 = "tom";
let s2 = s1.substring(2);
```

引用类型的对象在*new*过后，生命周期是整个上下文；而原始值不同，其生命周期只在其调用的那一行，也就是说**不能给原始值添加新的属性和方法，但是可以调用其预设的属性和方法**

#### *String* 常用的属性和方法

1. `str.length` -- 返回`str`的长度
2. `str.concat(newStr0[, newStr1, ...])` -- 用于拼接多个字符串
3. 提取字符串
   1. `slice(start[, end])` -- 两个参数表示范围
      1. `"abcd".slice(2)` &rarr; `"cd"`
      2. `"abcd".slice(1, 3)` &rarr; `"bc"`
      3. `"abcd".slice(-2)` &rarr; `"cd"`
      4. `"abcd".slice(0, -2)` &rarr; `"ab"`
   2. `substr(start[, charNum])` -- 第一个参数是起始点，第二个参数是起始点之后的字符数
      1. `"abcd".substr(1)` &rarr; `"bcd"`
      2. `"abcd".substr(1, 2)` &rarr; `"bc"`
      3. `"abcd".substr(-2)` &rarr; `"cd"`
      4. `"abdc".substr(1, -2)"` &rarr; `""`
   3. `substring(start[, end])` -- 两个参数表示范围(负数会视为0)
      1. `"abcd".substring(2)` &rarr; `"cd"`
      2. `"abcd".substring(1, 3)` &rarr; `"bc"`
      3. `"abcd".substring(-2)` &rarr; `"abcd"`
      4. `"abcd".substring(2, -2)` &rarr; `"ab"`(会从较小值开始，在较大值停止，该语句等同于`"abcd".substring(0, 2)"`)
4. `str.indexOf(substr)` -- 返回字符串位置(从头开始查找)
5. `str.includes(substr)` -- 返回是否包含*substring*
6. `str.split(char)` -- 将字符串根据*char*进行划分
7. `str.replace(oldstr, newstr)` -- 将第一个找到的*oldstr*替换成*newstr*

### 内置对象

#### Global

#### Math

1. `Math.random()`可以生成0~1内的随机数
2. `Math.min()`以及`Math.max()`可以求出最大最小值，对于数组，需要使用扩展操作符`...nums`
3. 舍入方法
   1. `Math.floor()` -- 向下取整
   2. `Math.round()` -- 四舍五入
   3. `Math.fround()` -- 返回最近的单精度浮点数

---