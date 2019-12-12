# 作用域和闭包

> 闭包是面试中经常被问到的一个概念。究竟什么是闭包，闭包有什么好处，闭包又有什么缺点呢？

### 作用域
对于JS来说，作用域有两点需要注意
* 内层作用域可以使用外部作用域的资源
* 当作用域内的资源有（可能）被外部使用时，则该作用域内的资源不会被释放。

``` javascript
let title = 'Node.js成长记录';
function show() {
  console.log(title);
}
show(); // Node.js成长记录
```
相同的情况，对于php来说，则是不可行的。
``` php
$title = 'php';
function show() {
    echo $title; // 未定义
}
show(); // 无输出
```
看下面一个例子
```javascript
function hd() {
    let n = 1;
    function sum() {
        console.log(++n);
    }
    sum();
}
console.log(n); // ReferenceError: n is not defined 变量n只属于函数内部
hd(); // 2
hd(); // 2
```
每次调用`hd()`时，都会在内存中开辟一块新的空间。并且当函数执行完毕后这个空间就会被释放。这两块空间是独立的。

```javascript
function hd() {
    let n = 1;
    return function() {
        console.log(++n);
    }
}
let b = hd();
b(); // 2
b(); // 3
```
这次就不一样了，函数`hd()`返回一个匿名函数的引用。则在函数执行完后，其内部的作用域资源不会被释放。那我们就可以通过操作匿名函数来操作变量`n`。

下面再来看一个复杂的情况
```javascript
function hd() {
    let n = 1;
    return function() {
        let m = 1;
        function show() {
            console.log(++m);
        }
        show();
    }
}
let a = hd();
a(); // 2
a(); // 2
```
和上面的例子很像，`hd()`同样是返回了一个匿名函数的引用。但这次，返回的函数中，并未使用到自身函数体以外的资源。所以每次函数执行时，其占用的资源都是独立的。

如过还想出现之前的累加效果，可以这样做
```javascript
function hd() {
    let n = 1;
    return function() {
        let m = 1;
        return function() {
            console.log('m:' + ++m);
            console.log('n:' + ++n);
        }
    }
}
let a = hd()();
a(); // m:2 n:2
a(); // m:3 n:3
```
`a`接收的是第二个匿名函数的引用，其使用的变量均是函数外部的。每次函数在执行时，无法独立创建变量。所以每次函数都是操作的外部变量。

我们可以用一句话总结：**函数每次在执行时，都是开辟新的内存空间。如果变量是在函数内声明的，则每次创建新的变量。如果操作的是外不变量，则每次执行都操作该变量。**

关于作用域还有下面的例子--**块级作用域**
```javascript
for (var i = 0; i < 4; i++) { // 全局每次都被覆盖
    setTimeout(function() {
        console.log(i);
    }, 1000)
}
// 4 4 4 4

for (let i = 0; i < 4; i++) {
    setTimeout(function() { // 块级单独创建
        console.log(i);
    }, 1000)
}
// 0 1 2 3 
```
对于`var`的声明方式，可以稍加改进也有`let`声明方式的作用。
```javascript
for (var i = 0; i < 4; i++) {
    (function(a){setTimeout(function() {
        console.log(a);
    }, 1000)})(i);
}
```
通过一个**立即执行函数**将变量`i`做为函数的参数，则在每个函数内`i`是独立的。

### 闭包
> 一个函数可以访问其他函数作用域内的变量。

闭包的特性，php是没有的。
```php
function hd() {
    $n = 1;
    function sum() {
        echo $n;

    }
    sum();
}
hd(); // 变量n是无法被输出的
```

##### 闭包的作用
* 利用闭包封装函数
数组的`filter`函数过滤元素
```javascript
let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let hd = arr.filter(function(v) {
    return v >= 4 && v <= 6
});
console.log(hd); // [4, 5, 6]
```
 如果我们想更换过滤的范围，那么我们只能修改函数体内的内容。我们可以利用闭包将其做为参数传入。
 ```javascript
 let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
 function between(a, b) {
     return function(v) {
         return v >= a && v <= b;
     }
 }
 let hd = arr.filter(between(3, 7));
 console.log(hd); // [3, 4, 5, 6, 7]
 ```
数组的`sort`排序
```javascript
const data = [
  {
    age: 10,
    grade: 99
  },
  {
    age: 16,
    grade: 90
  },
  {
    age: 13,
    grade: 95
  }
];
data.sort(function(a, b) {
  return a.age < b.age ? -1 : 1;
});
console.log(data);
```
同样的，如果我们需要根据不同的字段排序则很麻烦，于是可以
```javascript
function compare(field, type = 'asc') {
  return function (a, b) {
    if (type === 'asc') {
      return a[field] < b[field] ? -1 : 1;
    } else if (type === 'desc') {
      return a[field] < b[field] ? 1 : -1;
    }
  }
}
data.sort(compare('grade', 'desc'));
```
通过闭包设置其**排序字段**和**顺序。**
