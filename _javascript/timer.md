# 定时器

> JS中有两种定时器，`setTimeout`和`setInterval`。

# setTimeout

##### 语法

`setTimeout(code, millisec)`用于在指定的**毫秒数**后调用**函数或计算表达式。**
`code`必填参数，要调用的**函数**或**要执行的JavaScript代码**；`millisec`必填参数，在**执行代码前需等待的毫秒数**。

##### 注意

* `setTimeout`的延迟是**最小的**延迟时间，由于JS代码的运行机制，在制定参数延迟后定时器的回调未必可以得到执行。

* `code`可以是函数，也可以是代码块。建议使用函数而非代码块。因为使用代码块会造成**双重求值**。即，代码首先会以正常的方式求值，然后在执行过程中对包含于字符串中的代码发起另一个求值运算。
`setInterval`也存在该问题。

# setInterval

##### 语法

`setInterval(code, millisec)`用于按照指定的周期来循环调用函数或计算表达式，直到`clearInterval()`被调用或窗口关闭，由`setInterval()`返回的`ID`值可用作`clearInterval()`方法的参数。
`code`必填参数，要调用的**函数**或**要执行的JavaScript代码**；`millisec`必填参数，周期性执行的时间间隔，以毫秒计。

##### 注意

* `setInterval`未必可以按制定参数的时间间隔周期性执行代码。当代码执行的时间大于指定的时间间隔，则定时器下一次执行是要等待的。

* `setInterval`无视执行的代码，不管其所执行的代码是否报错均会一直调用。

##### 补充
使用`setTimeout`实现`setInterval`。

`setInterval`一直在执行代码。如果想让代码重复执行，一般有两种方式**循环**和**递归**。在这里循环是不可以的，因为循环无法保证时间间隔。

```javascript
function mySetInterval(callback, millisec) {
  function tmp() {
    setTimeout(tmp, millisec);
    callback()
  }
  setTimeout(tmp, millisec); // 递归体
}

mySetInterval(function () {
  console.log('mySetInterval')
}, 1000);
```

更好一点我们可以加上控制执行次数的条件
```javascript
function mySetInterval(callback, millisec, times) {
  function tmp() {
    if (typeof times === 'undefined' || times-- > 0) {
      setTimeout(tmp, millisec);
      try {
        callback();
      } catch (e) {
        times = 0; // 出错不再执行
        throw e.toString();
      }
    }
  }
  setTimeout(tmp, millisec);
}

mySetInterval(function () {
  console.log('advanced mySetInterval');
}, 1000, 2);
```
