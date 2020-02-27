# LeetCode-算法（51-75）

> LeetCode 算法类型题目（51-75）。

# 56.合并区间

> 中等

##### 题目

给出一个区间的集合，请合并所有重叠的区间。

##### 示例
```
输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```
##### 示例
```
输入: [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
```

##### 思路
首先需要对数组进行排序（对元素的左值排序），然后对数组遍历，合并能合并的元素。

能合并的又有两种情况：
1.两个区间只是有交集，但不包含
2.两个集合是包含关系

##### 实现

```javascript
/**
 * @param {number} n
 * @return {number}
 */
/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */
const merge = function(intervals) {
  // 首先一定要记得判断特殊情况
  const ans = [];
  if (intervals.length === 0) {
    return ans;
  } 
  // 确定数组不为空时，再对数组进行排序
  intervals.sort((a, b) => a[0] - b[0]);
  // 利用数组reduce函数对数组遍历。该函数返回的是最后一个归并的结果（即最后一次执行的返回值）。
  const lastValue = intervals.reduce((pre, cur) => {
    // 判断两个区间是有集合
    if (pre[1] >= cur[0]) {
      // 是否存在重合的情况
      if (pre[1] >= cur[1]) {
        return pre
      } else {
        pre[1] = cur[1];
        return pre;
      }
    } else {
      ans.push(pre);
      return cur;
    }
  });
  ans.push(lastValue);
  return ans;
};
```

# 70.爬楼梯

> 简单

##### 题目

假设你正在爬楼梯。需要`n`阶你才能到达楼顶。每次你可以爬`1`或`2`个台阶。你有多少种不同的方法可以爬到楼顶呢？

**注意：**给定`n`是一个正整数。

##### 示例
```
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
```
##### 示例
```
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

##### 思路

对于第`n`层的楼梯，可以从第`n-1`层或者第`n-2`层达到。

设`f(n)`为到达第`n`层的不同方法，则有`f(n) = f(n-1) + f(n-2)`。

##### 实现

```javascript
/**
 * @param {number} n
 * @return {number}
 */
const climbStairs = function(n) {
  const std = {
    1: 1,
    2: 2
  };
  if (std[n]) return std[n];
  for (let i = 3; i <= n; i++) {
    std[i] = std[i - 1] + std[i - 2];
  }
  return std[n];
};
```

```php
class Solution {

    /**
     * @param Integer $n
     * @return Integer
     */
    function climbStairs($n) {
        $std = [1 => 1, 2 => 2];
        if ($std[$n]) return $std[$n];

        for($i = 3; $i <= $n; $i++) {
            $std[$i] = $std[$i - 1] + $std[$i - 2];
        }
        return $std[$n];
    }
}
```
