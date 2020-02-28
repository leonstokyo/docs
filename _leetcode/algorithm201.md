# LeetCode-算法（201-225）

> LeetCode 算法类型题目（201-225）。

# 215.数组中第k个最大元素

> 中等

##### 题目

在未排序的数组中找到第**k**个最大的元素。请注意，你需要找的是数组排序后的第**k**个最大的元素，而不是第**k**个不同的元素。

##### 示例
```
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```

##### 示例
```
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```

##### 实现

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
const findKthLargest = function(nums, k) {
  // 给数组逆序排序，然后找到第k个元素
  nums.sort((a, b) => b - a);
  return nums[k - 1];
};
```
明显地，上述方法对数组进行了完全的排序。优化一点的方法，则可以不必对数组进行完全的排序。
```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
const findKthLargest = function(nums, k) {
  const end = nums.length - 1;
  let index = partition(nums, 0, end);
  while(index !== k -1) {
    while (index > k - 1) {
      index = partition(nums, 0, index - 1);
    } 
    while (index < k - 1) {
      index = partition(nums, index + 1, end);
    } 
  }
  return nums[k - 1];
};

// 通过快排找到基准点的位置，由于基准点的位置是准确的。所以可以通过找到处在k-1位置上的基准点，即可找到所求。
const partition = function(nums, start, end) {
  const key = nums[start];
  while (start < end) {
    while (start < end && nums[end] <= key) {
      end--;
    } 
    nums[start] = nums[end];
    while (start < end && nums[start] > key) {
      start++;
    } 
    nums[end] = nums[start];
  }
  nums[start] = key;
  return start;
} 
```
