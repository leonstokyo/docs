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
`push()`尾部添加，返回新数组的长度

`pop()`尾部移除一项，修改数组长度并返回该项

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

