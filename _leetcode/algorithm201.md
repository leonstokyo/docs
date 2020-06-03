# LeetCode-算法（201-225）

> LeetCode 算法类型题目（201-225）。

# 206.反转链表

> 简单

##### 题目

反转一个单链表。

##### 示例
```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```
##### 思路

 1.迭代法：遍历链表，将迭代的每一个元素放入结果链表的头部即可。
 2.递归法：注意节点的位置即可。
 
##### 实现

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
const reverseList = function(head) {
    // 迭代方法实现，关键在于没迭代一个元素将其放入结果列表的头部即可。
    let result = null; // 结果链表
    while (head) {
        let current = head; // 获取到当前元素
        head = head.next; // 游标后翼
        current.next = result; // 将当前元素放到结果集的前面
        result = current; // 重新定位结果集的头部
    }
    return result;
};
```

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
const reverseList = function(head) {
    // 利用递归的方法
    if (!head || !head.next) return head; // 没有节点或者一个节点直接返回该节点。
    // 递归完成之后，实际上就是两个元素在操作
    // head 和 head.next，递归完成后，head.next处在末尾
    const newHead = reverseList(head.next);
    head.next.next = head;
    head.next = null; // 把head置于末尾
    return newHead;
};
```


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
