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

 **1.浏览器对URL拆分解析**。
    
  基本形式**scheme://host.domain:port/path/filename**解释为：

  * **scheme** - 定义因特网服务的类型。最常见的类型是**http**
  * **host** - 定义域主机（http 的默认主机是 www）
  * **domain** - 定义因特网域名，比如 baidu.com
  * **:port** - 定义主机上的端口号（http 的默认端口号是 80）
  * **path** - 定义服务器上的路径（如果省略，则文档必须位于网站的根目录中）
  * **filename** - 定义文档/资源的名称
     
 **2.解析域名获取IP**。
    
  * 浏览器现在本地的**hosts**文件中检查是否有相应的**域名**和**IP**的对应关系，如果有则向其IP地址发送请求。
  * 如果没有，浏览器将domain(域名)发送给DNS进行解析，将IP返回浏览器。
    
 **3.通过IP建立网络通信**。
    
  一般采用四层模型，从上往下依次为：**应用层**、**传输层**、**网络层**和**数据链路层**。发送端从应用层往下走，
  接收端从数据链路层往上走。大致的模型图如下：
  <img style="height:500px" src="_media/network1.png">
  
  **网络协议的内容会有专题详解**
 
 **4.页面渲染**。


* Q8：你遇到过**跨域**的问题吗？有哪些解决方案？

 **浏览器同源策略**约定请求的url地址，必须与浏览器的url地址处于同域上，也就是**域名**，**端口**，**协议**都相同。
 实际上的结果是，请求已经被发送过去了，目标服务器也对请求做出了响应，只是浏览器对非同源请求的返回结果做了拦截。
 **跨域**就是由于浏览器的同源策略产生的。

 如何解决跨越问题：
 
  **1.JSONP**
  全称是**JSON with Padding**是为了解决跨域请求资源而产生的解决方案,是一种依靠开发人员创造出的一种非官方跨域数据交互协议。
  
  **2.CORS**
  
  **Cross-origin resource sharing,跨域资源共享**，是一个W3C标准，定义了在必须跨越访问资源时，浏览器和服务器应该
  如何沟通。
  
  CORS有两种请求，**简单请求**和**非简单请求**。
  
  同时满足下面两大条件的，就是**简单请求**：
    1. 请求方法是下面三种之一：
     * HEAD
     * GET
     * POST
    2. HTTP的头信息不超出以下几种字段：
     * Accept
     * Accept-Language
     * Content-Language
     * Last-Event-ID
     * Content-Type：只限于三个值**application/x-www-form-urlencoded、multipart/form-data、text/plain**
   
   **后端需要加入请求头Access-Control-Allow-Origin**
   
   **非简单请求**在发送数据之前会先发第一次请求做预检，只有预检通过后再发一次请求作为数据传输。
    * 请求方式：**OPTIONS**
    * 如何预检
     * Access-Control-Allow-Origin（必含）
     * 如果复杂请求是**PUT**等请求，则服务端需要设置允许某请求，否则“预检”不通过 **Access-Control-Request-Method**
     * 如果复杂请求设置了请求头，则服务端需要设置允许某请求头，否则“预检”不通过 **Access-Control-Request-Headers**
     
* Q9：你对Ajax了解吗，你能实现一个Ajax吗？

* Q10：你对**ES6**了解吗？

* 11：**箭头函数**和普通函数的区别是什么？

* 12：Vue中**组件间通信**有哪些方式？

* 13：Vux中**action**和**mutation**有什么区别？
 ```javascript
 const store = new Vuex.Store({
   state: {
     count: 0
   },
   mutations: {
     increment (state) {
       state.count++
     }
   },
   actions: {
     increment (context) {
       context.commit('increment')
     }
   }
 })
 ```
 1.流程顺序
 
 '相应视图->修改State'实则为两部分，**视图触发Action**，**Action再触发Mutation**。
 
 2.角色定位
 
 基于流程顺序，二者扮演的角色也不同。
 
 **Mutation:** 专注于修改State，**理论上是修改State的唯一途径**。
 
 **Action:** 业务代码，异步请求。
 
 3.限制
 
 角色不同所以二者的限制不同
 
 **Mutation:** 必须同步执行。
 
 **Action:** 可以异步，但不能操作State。
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



