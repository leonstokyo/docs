# this绑定机制

> 每个函数的**this**是在调用时被绑定的，完全取决于函数的调用位置。

## 调用位置

**调用位置**就是函数在代码中被调用的位置（而不是声明的位置）。最重要的是分析**调用栈**（就是为了达到当前执行位置所调用的所有函数）。调用位置就在当前
正在执行的函数的前一个**调用**中。

下面以一个例子来看**调用栈**和**调用位置**：

```javascript
function baz() {
  // 当前调用栈是：baz
  // 因此，当前调用位置是全局作用域
  
  console.log('baz');
  bar(); // <-- bar 的调用位置
}

function bar() {
  // 当前调用栈是：baz --> bar
  // 因此，当前调用位置在baz中
  
  console.log('bar');
  foo(); // <-- foo的调用位置
}

function foo() {
  // 当前调用栈是：baz --> bar --> foo
  // 因此，当前调用位置在bar中
  
  console.log('foo');
}

baz(); // <-- baz的调用位置
```

## 绑定规则

首先找到调用位置，然后判断需要应用下面**4**条规则中的哪一条。

### 默认绑定

函数的**独立调用**。

```javascript
global.a = 2;

function foo() {
  console.log(this.a);
}

foo(); // 2
```
上述情况下，函数调用时应用了**this**的默认绑定，因此**this**指向了**全局对象**。
需要注意的是，如果使用**严格模式（strict mode）**，全局对象则无法使用默认绑定，因此**this**
会绑定到**undefined**。

### 隐式绑定

调用位置是否有上下文对象，或者说是否被某个对象拥有或者包含。

```javascript
function foo() {
  console.log(this.a);
}

const obj = {
  a: 3,
  foo: foo
};

obj.foo(); // 3
```

当**函数引用**有上下文对象时，**隐式绑定**规则会把函数调用中的**this**绑定到这个上下文对象。因此，此处的
`this.a`和`obj.a`是一样的。

对象属性引用链中只有**最顶层**或者说**最后一层**会影响调用位置。

```javascript
function foo() {
  console.log(this.a);
}

const obj2 = {
  a: 42,
  foo: foo
};

const obj1 = {
  a: 2,
  obj2: obj2
};

obj1.obj2.foo(); // 42
```
##### 隐式丢失

一个最常见的**this**绑定问题就是被**隐式绑定**的函数会**丢失绑定对象**，也就是它会应用**默认绑定**，从而把**this**绑定
到**全局对象**或者**undefined**上，取决于是否是**严格模式**。

```javascript
global.a = 'oops, global'; // 全局对象属性

function foo() {
  console.log(this.a);
}

const obj = {
  a: 2,
  foo: foo
};

const bar = obj.foo; // 函数别名！

bar(); // oops, global
```
虽然`bar`是`obj.foo`的一个引用，但是实际上，它引用的是`foo`函数本身，因此此时的`bar()`其实是一个**独立函数**调用，应用了**默认绑定**。

一种更微妙、更常见并且更出乎意料的情况发生在**传入回调函数**时：

```javascript
global.a = 'oops, global'; // 全局对象属性

function foo() {
  console.log(this.a);
}

function doFoo(fn) {
  // fn 其实引用的是foo
  fn(); // <-- 调用位置！
}
const obj = {
  a: 2,
  foo: foo
};

doFoo(obj.foo); // oops, global
```
**参数传递**其实就是一种**隐式赋值**，因此我们传入函数时也会被隐式赋值，所以结果和上一个例子一样。
个人感觉这样利用引用'构造'的函数和如下代码是相通的。
```javascript
global.a = 'oops, global'; // 全局对象属性

function doFoo() {
  console.log(this.a);

}

doFoo(); // oops, global
```

把函数传给语言内置函数而不是自己声明的函数，结果是一样的。

这里需要注意一下，下面以`setTimeout()`方法作为示例，在**浏览器**环境中是**成立**的，在**Node.js**环境中则**不成立**。

```javascript
// node环境下，setTimeout中执行函数的this并不指向全局对象
function foo() {
  console.log(this.a);
}

const obj = {
  a: 2,
  foo: foo
};

global.a = 'oops, global';

setTimeout(obj.foo, 1000); // undefined
```
```javascript
// 浏览器环境下，setTimeout中执行函数的this是指向全局对象的
function foo() {
  console.log(this.a);
}

const obj = {
  a: 2,
  foo: foo
};

var a = 'oops, global';

setTimeout(obj.foo, 1000); // oops, global


```

## 显示绑定

在**隐式绑定**时，必须在一个对象内部包含一个指向函数的属性，并且通过这个属性间接
