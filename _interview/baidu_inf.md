# 百度基础架构部-前端

> 2020.01.02 一面 电话面试

## 一面回顾

* Q1：你在做项目的过程中遇到过什么问题，你是怎么解决的？

* Q2：LeetCode算法第1题-两数之和。

 我先用了最简单的两个循环去做，代码如下：
 ```javascript
 const twoSum = function(nums, target) {
   for (let i = 0; i < nums.length - 1; i++) {
     for (let j = i + 1; j < nums.length; j++) {
       if (nums[i] + nums[j] === target)
         return [i, j]
     }
   }
 };
 ```
 然后让我分析了该算法的时间复杂度(O(n<sup>2</sup>))，并问我有没有优化的方法。然后我写了这样一段代码：
 ```javascript
 const twoSum = function(nums, target) {
      const map = {};
      for (let i = 0; i < nums.length - 1; i++) {
        map[i] = target - nums[i];
      }
      for (const item in map) {
        const lastIndex = nums.lastIndexOf(map[item]);
        // 这里要规避某一个元素所需的元素是它本身，通过indexOf查找的索引有误。
        // 可以倒着查，或者指定indexOf(map[index], Number(item) + 1)函数的起始位置，从当前元素的下一个开始查。
        // 这里要特别注意：item这里是字符串类型，注意类型的转换。
        if (lastIndex > item) {
          return [Number(item), lastIndex];
        } 
      }
    };
 ```
 但是这里的循环中由于用得到了`indexOf()`方法，我感觉时间复杂度其实是没有变的。
 后来仔细想想这样其实是走远了，可以直接在`map`对象中判断是否存在其对应的值，但是映射的结构需要改一下。
 
 ```javascript
 const twoSum = function(nums, target) {
       const map = {};
       for (let i = 0; i < nums.length; i++) {
         map[nums[i]] = i;
       }
       for (let i = 0; i < nums.length; i++) {
         const diff = target - nums[i];
         if (diff in map && map[diff] > i) {
           return [i, map[diff]];
         } 
       }
     };
 ```
 这样的算法实现了**O(n)**的时间复杂度。其主要的思想就是在**查询（获取索引）的时候不能去遍历数组。**
 还以用ES6提供的`Map`来做映射。
 
 ```javascript
 const twoSum = function(nums, target) {
       const map = new Map();
       for (let i = 0; i < nums.length; i++) {
         map.set(nums[i], i);
       }
       for (let i = 0; i < nums.length; i++) {
         const diff = target - nums[i];
         const result = map.get(diff);
         console.log(result);
         if (result !== undefined && result > i) {
           return [i, result];
         }
       }
     };
 ```
 
* Q3：实现一个**快排**。（请参考本博客中**Algorithm**模块-排序算法）。

* Q4：你在公司用Node.js多久了，用Node.js开发有什么**优势**？

 我们公司主要使用Node.js处理中间层业务。对公司内部不同的服务进行集中的管理，统一对外接口的提供服务。由于Node.js擅长处理高并发以及其非阻塞I/O的机制，所以使用Node.js做中间层还是很有优势的。

* Q4：说一说Node.js的**事件循环**。（请参考本博客中**Node**模块-事件循环）。

* Q5：**微任务**和**宏任务**有什么区别呢？

* Q6：**css**常用的**布局**有哪些？
 这个没答出来，后续补充。
    
* Q7：从浏览器发起请求，到用户接收到响应，都经历了哪些过程？

* Q8：你遇到过**跨域**的问题吗？有哪些解决方案？

* Q9：你对Ajax了解吗，你能实现一个Ajax吗？

* Q10：你对**ES6**了解吗？

* 11：**箭头函数**和普通函数的区别是什么？

* 12：Vue中**组件间通信**有哪些方式？

* 13：Vux中**action**和**mutation**有什么区别？

* 14：说一说Node.js的**文件模块**，它是如何实现的呢？

* 15：JS的**基本类型**有哪些？

* 16：你了解**闭包**吗？


## 二面准备

* Q1：数组的去重方法。

 * 双层`for`循环
 
 ```javascript
 const arr = [1, 2, 3, 5, 5, 4, 4, 6];
 
 const distinctArray = function (arr) {
   for (let i = 0; i < arr.length; i++) {
     for (let j = i + 1; j < arr.length; j++) {
       if (arr[i] === arr[j]) {
         arr.splice(j, 1);
         j--;
       }
     }
   }
   return arr;
 };
 console.log(distinctArray(arr)); // [ 1, 2, 3, 5, 4, 6 ]
 ```
 很明显，该方法的时间复杂度为**O(n<sup>2</sup>)**。
 
 * 元素放入新数组，重复元素忽略
 
 ```javascript
 const arr = [1, 2, 3, 5, 5, 4, 4, 6];
 
 const distinctArray = function (arr) {
   const result = [];
   for (const item of arr) {
     if (result.indexOf(item) === -1) {
       result.push(item);
     }
   }
   return result;
 };
 
 console.log(distinctArray(arr)); // [ 1, 2, 3, 5, 4, 6 ]
 ````
 
 * 利用数组的`filter()`和`indexOf()`方法
 
 ```javascript
 const arr = [1, 2, 3, 5, 5, 4, 4, 6];
 
 const distinctArray = function (arr) {
   return arr.filter((item, index) => {
     return arr.indexOf(item) === index;
   })
 };
 
 console.log(distinctArray(arr)); // [ 1, 2, 3, 5, 4, 6 ]
 ```
 `indexOf()`方法返回数组元素第一次出现的索引。如果元素当前索引与其第一次的出现的索引相同，那么可以说明只有一个元素。
 
 * 利用数组`sort()`方法和一次遍历
 
 ```javascript
 const arr = [1, 2, 3, 5, 5, 4, 4, 6];
 
 const distinctArray = function (arr) {
   const compare = (a, b) => {
     return a - b;
   };
   const sortedArr = arr.concat().sort(compare);
   const result = [sortedArr[0]];
   for (let i = 1; i < sortedArr.length; i++) {
     if (sortedArr[i] !== sortedArr[i - 1]) {
       result.push(sortedArr[i])
     }
   }
   return result;
 };
 
 console.log(distinctArray(arr)); // [ 1, 2, 3, 4, 5, 6 ]
 ```
 该方法的明显的一个缺点是**改变了数组的元素的顺序**。
 
 * 利用ES6`Set`集合
 
 ```javascript
 const arr = [1, 2, 3, 5, 5, 4, 4, 6];
 
 const distinctArray = function (arr) {
   return [...new Set(arr)];
 };
 
 console.log(distinctArray(arr)); // [ 1, 2, 3, 5, 4, 6 ]
 ```
 
 * 利用ES6`Map`
 
 ```javascript
 const arr = [1, 2, 3, 5, 5, 4, 4, 6];
 
 const distinctArray = function (arr) {
   const map = new Map();
   const result = [];
   for (let i = 0; i < arr.length; i++) {
     if (!map.has(arr[i])) {
       result.push(arr[i]);
       map.set(arr[i], 1);
     }
   }
   return result;
 };
 
 console.log(distinctArray(arr)); // [ 1, 2, 3, 5, 4, 6 ]
 ```

 > 这里的方法均以数字示例，以后会做进阶的方法分析。

* Q2：**cookie**和**session**的区别。

 **cookie**和**session**都是用来存储用户信息的。由于HTTP协议是无状态协议，每次建立连接时是无法识别身份的。
 于是就有了**cookie**，把用户的一些信息记录到**cookie**中，当再一次打开网页和服务器建立连接时，把**cookie**中记录的
 信息一起发送给服务器，这样服务器可以识别用户身份。
    
 **cookie**的特点：
    
  * 存储在客户端，**安全性不高**，任何人都可以查看。
  * **cookie**的**数量**及其存储的**字符数量**都有限制（存储几十个每个最大为4096个字符）。
     
 当用户和服务器建立连接时，服务器也会存一份用户的数据。这就是**session（会话控制）**。并且会向客户端发送不同的**sessionId**
 并保存在**cookie**中。当用户再次访问时，会向服务端发送**sessionID**，服务端根据**sessionId**找到对应的**session**就可以识别用户。

 **session**的特点：
      
  * 存储在服务端，安全性高。
  * 占用服务端资源。
  
* Q3：**this**绑定机制（参考**javascript**模块-this）。

## 二面回顾

> 2020.01.13 电话面试

* Q1：为什么考虑换工作。

* Q2：你现在的公司在上海，为什么要考虑北京呢？

* Q3：你之前是做后端开发的，为什么要改做前端呢？

* Q4：挑一个简历上的项目具体说说。

* Q5：JS如何获取某个元素的绝对位置？

* Q6：说一下css的盒模型。

* Q7：你对RESTful风格了解吗？

* Q8：POST和GET请求的区别有哪些？

* Q9：你对TCP协议了解吗？

* Q10：你对Http状态码了解多少？

* Q11：301，302，304有什么区别？

* Q12：XML、HTML、XHTML有什么区别？

* Q13：原型链你了解吗？如何实现一个继承？

* Q14：你都是通过哪些方式提升你的编程能力的？

* Q15：你最近在看什么书？

* Q16：给定一个数和一个节点，找到从跟节点到给定节点的路径。



