# LeetCode-算法（26-50）

> LeetCode 算法类型题目（26-50）。

# 31.下一个排列

> 中等

##### 题目

实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须**原地**修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。

##### 示例
```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

##### 思路
要找到下一个排列，那首先要知道什么是下一个排列。就是当前序列字典序中后面序列最小的那个。
比如1-3-4-5，很明显当5和4换位置之后的1-3-5-4是下一个序列。再如1-2-7-6-5-4，对于1-2开头的序列，
该序列已经是最大的了，故需要找出1-4开头的最小的序列，即1-4-2-5-6-7。

一般地，我们需要从后往前找到第一个逆序对前一个数的位置，并把该数与后面数字序列中的最小数换位置，然后将后面的
序列顺序排列即可得到下一个序列。

##### 实现

```javascript
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
const nextPermutation = function(nums) {
  // 倒序对数组遍历
  for (let i = nums.length - 1; i >= 0; i--) {
    if (i === 0) {
      nums.reverse();
    } else {
      // 判断第一个逆序的位置
      if (nums[i] > nums[i - 1]) {
        // 利用数组的splice()函数对数组进行分割处理
        let arr = nums.splice(i);
        const value = nums[i - 1];
        let target = 0;
        // 对切割下的数组排序，找到其中第一个比value大的值
        arr.sort((a, b) => a - b);
        for (let j = 0; j < arr.length; j++) {
          if (arr[j] > value) {
            target = arr[j];
            arr[j] = value;
            break;
          } 
        } 
        nums[i - 1] = target;
        arr.sort((a, b) => a - b);
        // 再利用splice()函数拼接数组
        nums.splice(nums.length, 0, ...arr);
        break;
      } 
    }
  }
};
```

# 46.全排列

> 中等

##### 题目

给定一个没有重复数字的序列，返回其所有可能的全排列。

##### 示例
```
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

##### 思路
递归实现全排列，核心的思想就是缩小问题的规模。以 3 个数的全排列为例，是 3 个数字中的每一个数加上剩下
 2 个数的全排列的组合。即为 3 个数的全排列。并且，当只有 1 个数时，那么它本身就是全排列。

##### 实现

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
const permute = function(nums) {
  const ans = [];
  // 递归函数run
  const run = function(index) { // 从数组的起始位置开始计算后续数列的全排列
      if (index === nums.length - 1) {
        ans.push([...nums]);
      }
      for (let i = index; i < nums.length; i++) {
        // 缩小问题规模，拿出每一个数，与剩下的数的全排列结合。
        [nums[index], nums[i]] = [nums[i], nums[index]];
        run(index + 1);
        [nums[i], nums[index]] = [nums[index], nums[i]];
      } 
  };
  run(0);
  return ans;
};
```

