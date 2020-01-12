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
