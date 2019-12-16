# 基础问题

> 基础1

# 逻辑等（==）与全等（===）

##### 逻辑等 (==)

在比较之前，== 和 != 操作符都会进行强制类型转换。在转换不同类型时遵循下列原则:

* 如果有一个操作数是布尔值，则在比较之前先将操作数转化为数值，即调用`Number()`函数。

* 如果有一个操作数是字符串，另一个是数值，则在比较之前先将字符串转化为数值，即调 用`Number()`函数。

* 如果有一个操作数是对象，另一个不是，则调用对象的`valueOf()`方法，用得到的基本类型按前面的规则进行比较。

在转换类型之后，在进行比较时又遵循以下原则:

* JS 规定`null`和`undefined`是逻辑相等的。

* 如果有一个操作数是`NaN`则返回`false`(NaN不与任何操作数逻辑等或全等，包括NaN)。

* 如果两个操作符都是对象，则比较它们是不是一个对象。是则返回`true`，否则返回`false`。

##### 全等 (===)

全等比较简，**基本类型**要比较**类型**和**值**。**引用类型**要比较**引用值**是否相等。

##### 例子

```javascript
undefined == false // false
/**
 * Number(false) = 0
 * Number(undefined) = NaN
 */

null == false // false
/**
 * Number(false) = 0
 * Number(null) = 0
 * 值得注意的是，null是对象类型
 */

undefined == null // true

undefined === null // false

[1] == [1] // false

```

# 深拷贝与浅拷贝

> **浅拷贝**和**深拷贝**都只针对于**引用数据类型**，浅拷贝**只复制**指向某个对象的**指针**，
而不复制对象本身，新旧对象还是共享同一块内存；但深拷贝会另外创造一个一模一样的对象，新对象跟原对象**不共享内存**，修改新对象不会改到原对象。

```javascript
function shallowCopy(object) {
  let obj = Array.isArray(object) ? [] : {};
  for (let i in object) {
      obj[i] = object[i];
  }
  return obj;
}

const obj1 = {
  a: 1,
  b: 2,
  c: {
      d: 3
  }
};

const obj2 = shallowCopy(obj1);
obj2.a = 3;
obj2.c.d = 4;
console.log(obj1.a); // 1
console.log(obj2.a); // 3
console.log(obj1.c.d); // 4
console.log(obj2.c.d); // 4
```
基本类型可以直接通过`==`赋值，而引用类型（这里是对象）则不可以。`obj1.c`接收的是`obj2.c`的引用。
修改`obj1.c`的值会影响到`obj2.c`


```javascript
const obj1 = {
   a: {
     b: 1
   },
   c: 2
};
const obj2 = Object.assign({},obj1);
obj2.a.b = 3;
obj2.c = 3;
console.log(obj1.a.b); // 3
console.log(obj2.a.b); // 3
console.log(obj1.c); // 2
console.log(obj2.c); // 3
```
通过`Object.assign`方法，对引用类型依然是**浅拷贝**。

```javascript
const obj1 = {
  a: 1,
  b: 2,
  c: {
    d: 3
  }
};
const obj2 = Object.create(obj1);
obj2.a = 3;
obj2.c.d = 4;
console.log(obj1.a); // 1
console.log(obj2.a); // 3
console.log(obj1.c.d); // 4
console.log(obj2.c.d); // 4
```
通过`Object.create`方法，依然是**浅拷贝**。

```javascript
function deepCopy(object) {
  const obj = Array.isArray(object) ? [] : {};
  if (object && typeof object === 'object') {
      for (const i in object) {
          if (object.hasOwnProperty(i)) {
              if (object[i] && typeof object[i] === 'object') {
                  obj[i] = deepCopy(object[i]);
              } else {
                  obj[i] = object[i];
              }
          }
      }
  }
  return obj;
}

const obj1 = {
  a: 1,
  b: 2,
  c: {
    d: 3
  }
};
const obj2 = deepCopy(obj1);
obj2.a = 3;
obj2.c.d = 4;
console.log(obj1.a); // 1
console.log(obj2.a); // 3
console.log(obj1.c.d); // 3
console.log(obj2.c.d); // 4
```
通过**递归**的方式，可以实现引用类型的**深拷贝**。


```javascript
function deepCopy(obj1){
    let _obj = JSON.stringify(obj1);
    let obj2 = JSON.parse(_obj);
    return obj2;
  }
    const a = [1, [1, 2], 3, 4];
    const b = deepCopy(a);
    b[1][0] = 2;
    console.log(a); // 1,1,2,3,4
    console.log(b); // 2,2,2,3,4
```
通过`JSON.stringify`和`JSON.parse`的转换操作，可以实现引用类型的**深拷贝**。

