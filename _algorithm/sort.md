# 排序算法

> **排序** 是最常使用到的算法之一。下面是常见的一些排序算法的思路和实现。

# 快速排序

##### 简介
快速排序之所以被称作**'快速'**，是因为它是我们接触到的最快的**通用**排序算法。

##### 算法思想
快排是基于**分治**的思想。首先找一个基准值(一般取待排序列的第一个值)，然后通过操作把小于等于基准值的元素放到一边，
大于等于基准值的元素放到另一边。首先用指针标记首尾两个元素，后面的标记点先从后向前遍历，如果标记点的元素大于等于基准值，那么标记点前移;
如果小于就把两个标记点的元素互换位置。前面的标记点从前向后遍历，如果标记点的元素小于等于基准值，标记点就后移;
如果大于就把两个标记点的元素换位置。最后两个标记点会重合，排序结束。

下面看一个例子：

<img src="_media/quick-sort.jpg">

##### 算法实现
```javascript
const quickSort = function (nums, start, end) {
  if (start >= end) {
    return;
  }
  // 获取到一趟排序后的基准值位置
  const index = partition(nums, start, end);
  quickSort(nums, start, index - 1);
  quickSort(nums, index + 1, end);
};

// 每次迭代，找到中心点
const partition = function (nums, start, end) {
  const pivot = nums[start]; // 取第一个值作为排序基准值
  while (start < end) {
    while (nums[end] >= pivot && end > start) {
      end--;
    }
    [ nums[end], nums[start] ] = [ nums[start], nums[end] ]; // 交换两个游标的元素
    while (nums[start] <= pivot && end > start) {
      start++;
    }
    [ nums[end], nums[start] ] = [ nums[start], nums[end] ]; // 交换两个游标的元素
  }
  return end;
};

const a = [4, 3, 6, 1, 2, 11, -2, 5, 5];
quickSort(a, 0, a.length - 1);
console.log(a); // [ -2, 1, 2, 3, 4, 5, 5, 6, 11 ]
```

对于partition函数还有其他的写法，比如：
```javascript
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

##### 复杂度分析

递归算法的时间复杂度公示：

**T(n) = aT(n/b) + f(n)** 该公式中，**a**是子问题的个数，**n/b**是子问题的规模。**f(n)**包含了问题分解和子问题解合并的代价。

###### 最优情况

快排的最好的情况是每次取到的元素刚好平分整个数组。根据上面的公式则有：

**T(n) = 2T(n/2) + n**

令**n = n/2** （第二次递归）则有：

**T(n) = 2(2T(n/4) + n/2) + n**

**=2^2T(n/(2^2)) + 2n**

令**n = n/(2^(m-1))**(第m次递归，假设此时递归完成)则有：

**=2^mT[1] + mn**

假设，m+1次递归完成，我们知道递归的最后一步为**T[1]**, 固有：

**T[n/(2^m)] = T[1]**, 即**m=logn**

又由上知：**T(n) = 2^mT[1] + mn**

**=nT(1) + nlogn**

**=n + logn**

所以，最有的情况下，快速排序的时间复杂度为：**O(nlogn)**。

###### 最差情况

最差情况，每次都取到数组中最大或者最小的元素，那其实质就是冒泡排序。时间复杂度为**O(n^2)**。



# 选择排序

##### 算法思想

在一个长度为 N 的无序数组中，第 1 趟遍历 N 个元素，找出最小的元素与第 1 个元素交换;
第 2 趟遍历剩下的 N − 1 个元素，找出其中最小的元素与第 2 个元素交换; ... ;
第 N − 1 趟遍历剩下 2 个元素，找出其中最小的与第 N − 1 个元素交换，至此排序完成。

##### 算法实现
```javascript
const selectionSort = function (nums) {
  for (let i = 0; i < nums.length; i++) {
    let value = nums[i];
    let index = -1;
    for (let j = i + 1; j < nums.length; j++) {
      if (nums[j] < value) {
        value = nums[j];
        index = j;
      }
    }
    if (index > -1) {
      [nums[i], nums[index]] = [nums[index], nums[i]]
    }
  }
  return nums;
}
selectionSort([2,3,4, 3, 7, 11, 34, -3,1, 56, 87, 4, 23, -10]);
// [ -10, -3, 1, 2, 3, 3, 4, 4, 7, 11, 23, 34, 56, 87 ]
```
