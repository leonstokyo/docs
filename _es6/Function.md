# ES6 函数的扩展

## 函数的默认参数
ES6之前，不能为函数指定默认参数。
```javascript
function log(x, y) {
  y = y || 'world'; // 设置参数的默认值
  console.log(x, y);
}
log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello World
```
这样做是存在问题的。如果参数`y`被赋值了，但是其对应的布尔值为`flase`，则该赋值是不起作用的。如上最后一个例子。

一般为了避免这个问题，需要先判断参数是否被赋值。

```javascript
if (typeof y === 'undefined') {
  y = 'World';
} 
```

ES6允许函数为参数设置默认值，如下格式：

```javascript
function log(x, y = 'World') {
  console.log(x, y);
}
log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello
```
这样写的好处有两个：首先可以明显的看出哪些参数可以省略；其实有利于将来的代码优化，即使未来的版本在对外接口中，彻底拿掉这个参数，也不会导致以前的代码无法运行。

使用默认参数时，需要注意一下几个问题：

1. 参数是默认声明的，不能用`let`或`const`再次声明。
```javascript
function foo(x = 5) {
  let x = 1; // error
  const x = 1; // error
}
```
2. 使用参数的默认值时，不能有**同名**参数。
```javascript
// 不报错
function foo(x, x, y) {
  // ...
}
// 报错
function foo(x, x, y = 1) {
  // ...
}
// SyntaxError: Duplicate parameter name not allowed in this context
```
