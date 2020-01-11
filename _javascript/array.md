# 数组

> 数组中一些常用的方法

## 检测数组

* `instanceof`

```javascript
const arr = [1, 2, 3, 4];
const obj = {name: 'Jean', age: '21'};
console.log(arr instanceof Array); // true
console.log(obj instanceof Array); // false
```

* `Array.isArray()`

```javascript
const arr = [1, 2, 3, 4];
const obj = {name: 'Jean', age: '21'};
console.log(Array.isArray(arr)); // true
console.log(Array.isArray(obj)); // false
```

## 转换方法
* `valueOf()`

数组的`valueOf()`方法返回其本身。
```javascript
const arr = [1, 2, 3, 4];
console.log(arr.valueOf()); // [ 1, 2, 3, 4 ]
```
* `toString()`和`toLocaleString()`

数组的`toString()`方法会返回数组中每个值的字符串形式拼接而成的一个以**逗号**分隔的字符串。实际上，为了创建
这个字符串会调用数组的每一项的`toString()`方法。

```javascript
const arr = [1, 2, 3, 4];
console.log(arr.toString());       // 1,2,3,4
console.log(arr.toLocaleString()); // 1,2,3,4

const people1 = {
  toLocaleString: function () {
    return 'Jean'
  },
  toString: function () {
    return 'Jean'
  }
};

const people2 = {
  toLocaleString: function () {
    return 'Mike'
  },
  toString: function () {
    return 'Micheal'
  }
};

const people = [people1, people2];
console.log(people.toString());       // Jean,Micheal
console.log(people.toLocaleString()); // Jean,Mike
```
`toLacaleString()`与`toString()`不同的是**字符串样式与当前语言环境有关。**

* `join()` 

接收一个参数，并以该参数作为分隔符返回字符串。

```javascript
const colors = ['red', 'green', 'black'];
console.log(colors.join());     // red,green,black
console.log(colors.join(','));  // red,green,black
console.log(colors.join('-'));  // red-green-black
````

## 栈方法
* `push()`尾部添加，返回新数组的长度

* `pop()`尾部移除一项，修改数组长度并返回该项

```javascript
const colors = [];
let count = colors.push('red', 'green');
console.log(count); // 2
count = colors.push('black');
console.log(count); // 3
const item = colors.pop();
console.log(item); // black
console.log(colors.length); // 2
```
## 队列方法

* `shift()`取出第一项
```javascript
const colors = ['red', 'green', 'black'];
const item = colors.shift();
console.log(item);  // red
```
* `unshift()`头部添加，返回新数组的长度
```javascript
const colors = ['red', 'green', 'black'];
const count = colors.unshift('yellow', 'gray');
console.log(count); // 5
console.log(colors); // [ 'yellow', 'gray', 'red', 'green', 'black' ]
```

## 重排序方法

* `reverse()`反转数组顺序
```javascript
const colors = ['red', 'green', 'black'];
colors.reverse();
console.log(colors); // [ 'black', 'green', 'red' ]
```

* `sort()` 数组排序，默认升序排列数组项。该方法会调用每个数组项的`toString()`转型方法，然后比较得到的字符串。

```javascript
const arr = [0, 1, 5, 10 ,15];
arr.sort();
console.log(arr); // [0, 1, 10, 15, 5] 比较的字符串的顺序
```

于是`sort()`方法接收一个比较函数。比较函数接收两个参数，如果第一个参数位于第二个之前则返回一个负数，两个参数相等
则返回0，如果第一个参数位于第二个参数之后则返回一个正数。升序的比较函数可以如下：

```javascript
function compare(value1, value2) {
  if (value1 < value2) {
    return -1;
  } else if (value1 > value2) {
    return 1;
  } else {
    return 0;
  }
}
```
上面例子则有：
```javascript
const arr = [0, 1, 5, 10 ,15];
arr.sort(compare);
console.log(arr); // [ 0, 1, 5, 10, 15 ]
```
## 操作方法

* `contact()` 基于**当前数组**的所有项创建一个新的数组。具体来说，该方法会创建当前数组的一个副本，然后再将
接收到的参数添加到这个副本的末尾。最后返回新构建的数组。在没有参数的情况下，它只**复制当前数组并返回副本**。如果
传递的参数是一个或者多个数组，则该方法会将这些数组的每一项都添加到结果数组中。如果传递的值不是数组，这些值
会被简单地添加到结果数组的末尾。

```javascript
const colors = ['red', 'green', 'blue'];
const colors2 = colors.concat('yellow', ['black', 'brown']);
console.log(colors);  // [ 'red', 'green', 'blue' ]
console.log(colors2); // [ 'red', 'green', 'blue', 'yellow', 'black', 'brown' ]
```
* `slice()` 基于当前数组中的**一个或者多个项**创建一个新数组。接收一个或者两个参数，即要**返回项的起始和结束**位置。在只有
一个参数的情况下，返回从**指定开始**到当前数组末尾的所有项。如果有两个参数，则返回**起始**和**结束**位置之间的所有项**但不包括结束位置的项**。**`slice()`不会影响原始数组**。

```javascript
const colors = ['red', 'green', 'blue', 'yellow', 'brown'];
const colors2 = colors.slice(1);
const colors3 = colors.slice(1, 4);
console.log(colors);  // [ 'red', 'green', 'blue', 'yellow', 'brown' ]
console.log(colors2); // [ 'green', 'blue', 'yellow', 'brown' ]
console.log(colors3); // [ 'green', 'blue', 'yellow' ]
```

* `splice()` 主要是向数组中部插入项，该方法的使用方式有如下**3**种。
    * **删除：** 可以删除任意数量的项，只需指定**2**个参数：要删除的第一项的位置和要删除的项数。例如，`splice(0, 2)`为删除数组中的前两项。

    * **插入：** 可以向指定位置插入任意数量的项，只需提供**3**个参数：起始位置、0（要删除的项数）和要插入的项。如果要插入多个项，只需参数列表往后延伸。例如，`splice(2, 0, 'red', 'green')`会从位置2插入字符串 **'red'** 和 **'green'**。
    
    * **替换：** 可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定**3**个参数：起始位置、要删除的项数和要插入的任意数量的项。**插入和删除的项不必相等**。例如；`splice(2, 1, 'red', 'green')`会删除数组位置2的项，然后再从位置2开始插入字符串 **'red'** 和 **'green'**。

`splice()`方法始终都会返回一个数组，该数组中包含从原始数组中删除的项（没有删除任何项则返回空数组）。
```javascript
const colors = ['red', 'green', 'blue'];
let removed = colors.splice(0, 1); // 删除第一项
console.log(colors); // [ 'green', 'blue' ]
console.log(removed);// [ 'red' ]

removed = colors.splice(1, 0, 'yellow', 'orange'); // 从位置1插入两项
console.log(colors); // [ 'green', 'yellow', 'orange', 'blue' ]
console.log(removed);// []

removed = colors.splice(1, 1, 'red', 'purple'); // 插入两项，删除一项
console.log(colors); // [ 'green', 'red', 'purple', 'orange', 'blue' ]
console.log(removed);// ['yellow']
```
## 位置方法

* `indexOf()` 和 `lastIndexOf()` 这两个方法都接收两个参数：要查找的项和（可选）表示查找起点位置的索引。其中，`indexOf()`方法从数组的开头(位置0)向后查找，`lastIndexOf()`方法则从数组的末尾开始向前查找。
这两个方法都返回要查找的项在数组中的位置，或者在没找到的情况下返回-1。在查找比较时**使用全等`===`操作符**。

```javascript
const numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
console.log(numbers.indexOf(4)); // 3
console.log(numbers.indexOf('4')); // -1
console.log(numbers.lastIndexOf(4)); // 5
console.log(numbers.indexOf(4, 4)); // 5
console.log(numbers.lastIndexOf(4, 4)); // 3

const person = {name: 'Jean'};
const people = [{name: 'Jean'}];

const morePeople = [person];
console.log(people);
console.log(morePeople);
console.log(people.indexOf(person)); // -1 
console.log(morePeople.indexOf(person)); // 0

```

## 迭代方法
ECMAScript5 为数组提供了**5**个迭代方法。每个方法都接收**两个参数**：要在每一项上运行的函数和（可选）运行该函数的作用域对象--影响``this``的值。传入这些方法中的函数会接收**三个参数**：数组项的值、该项在数组中的位置和数组对象本身。

* `every()`：对数组中的每一项运行给定函数，如果该函数对**每一项**都返回true，则返回true。
* `some()`：对数组中的每一项运行给定函数，如果该函数对**任一项**返回true，则返回true。
* `filter()`：对数组中的每一项运行给定函数，返回函数会返回true的项组成的数组。
* `forEach()`：对数组中的每一项运行给定函数。**没有返回值**。
* `map()`：对数组的每一项运行给定函数，返回每次函数调用结果组成的数组。

**以上方法不会修改数组中包含的值。**

`some()`和`every()`最为相似，它们都用于查询数组中的项**是否满足某个条件**。
 ```javascript
 const numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
 const everyResult = numbers.every((item, index, array) => {
   return item > 2;
 });
 console.log(everyResult); // false
 const someResult = numbers.some((item, index, array) => {
   return item > 2;
 });
 console.log(someResult); // true
 ```
 
 `filter()`利用指定函数确定是否在返回数组中包含某一项。
```javascript
const numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
const filterResult = numbers.filter((item, index, array) => {
  return item > 2;
});
console.log(filterResult); // [ 3, 4, 5, 4, 3 ]
```
`map()`返回一个数组，而这个数组的每一项都是在原数组中的对应项上运行传入函数的结果。例如，可以给数组中的每一项
乘以2，然后返回这些乘积组成的数组。
```javascript
const numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
const mapResult = numbers.map((item, index, array) => {
  return item * 2;
});
console.log(mapResult); // [ 2, 4, 6, 8, 10, 8, 6, 4, 2 ]
```
`forEach()`只是对数组中的每一项运行传入函数，这个方法没有返回值。**本质上与使用`for`循环迭代数组一样**。
```javascript
const numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
numbers.forEach((item, index, array) => {
  // 执行某些操作
});
```

## 归并方法
ECMAScript5还新增了两个归并数组的方法：`reduce()`和`reduceRight()`。这两个方法都会**迭代数组的所有项，然后构建一个最终的返回值**。
其中，`reduce()`方法从数组的**第一项**开始，逐个遍历到最后。而`reduceRight()`则从**最后一项**开始，向前遍历到第一项。

这两个方法都接收两个参数：一个在每一项上调用的函数和（可选的）作为归并基础的初始值。传给`reduce()`和`reduceRight()`的函数接收**4**个参数：前一个值、当前值、项的索引和数组对象。**这个函数的返回的任何值都会作为第一个参数传给下一项。**第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数是数组的第二项。

```javascript
const values = [1, 2, 3, 4, 5];
const sum = values.reduce((pre, cur, index, array) => {
  return pre + cur;
});
/**
 * 迭代4次
 * 每次迭代的pre：1, 3, 6, 10
 * 每次迭代的cur：2, 3, 4, 5
 */
console.log(sum); // 15
```


```javascript
const values = [1, 2, 3, 4, 5];
const sum = values.reduceRight((pre, cur, index, array) => {
  return pre + cur;
});
/**
 * 迭代4次
 * 每次迭代的pre：5, 9, 12, 14
 * 每次迭代的cur：4, 3, 2, 1
 */
console.log(sum); // 15
```
