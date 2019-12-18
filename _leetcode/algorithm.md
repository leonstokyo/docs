# LeetCode-算法

> LeetCode 算法类型题目。

# 1.两数之和

> 简单

##### 题目

给定一个整数数组`nums`和一个目标值`target`，请你在该数组中找出和为目标值的那**两个**整数，并返回他们的数组下标。你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

##### 示例

```
给定 nums = [2, 7, 11, 15], target = 9
因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

##### 实现

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
const twoSum = function(nums, target) {
    for (let i = 0; i < nums.length - 1; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] === target)
                return [i, j]
        }
    }
};
```

# 2.两数相加

> 中等

##### 题目

给出两个**非空**的链表用来表示两个非负的整数。其中，它们各自的位数是按照**逆序**的方式存储的，并且它们的每个节点只能存储**一位**数字。
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。您可以假设除了数字**0**之外，这两个数都不会以**0**开头。

##### 示例

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```
##### 思路
这里其实就是在模拟两个数相加的过程，只不过是用了链表而已。因为使用了链表，所以难点就在于遍历链表的指针，在移动的过程中不要指错就可以了。

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
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
const addTwoNumbers = function(l1, l2) {
    let p = l1;
    let q = l2;
    let carry = 0;
    let result = new ListNode();
    let dummy = result; // 需要一个移动的指针添加节点和一个固定的指针指在头部。
    while (p || q || carry) {
        let sum1 = p ? p.val : 0;
        let sum2 = q ? q.val : 0;
        let sum = 0;
        if (sum1 + sum2 + carry >= 10) {
            sum = sum1 + sum2 + carry - 10;
            carry = 1;
        } else {
            sum = sum1 + sum2 + carry;
            carry = 0;

        }
        dummy.next = new ListNode(sum);
        dummy = dummy.next;
        if (p) p = p.next;
        if (q) q = q.next;
    }
    return result.next;
};
```

# 3.无重复字符的最长子串

> 中等

##### 题目

给定一个字符串，请你找出其中不含有重复字符的**最长子串**的长度。

##### 示例

```
输入: "abcabcbb"
输出: 3
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为3。

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为1。

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为3。
     请注意，你的答案必须是子串的长度，"pwke" 是一个子序列，不是子串。
```

##### 实现

```javascript
/**
 * @param {string} s
 * @return {number}
 */
const lengthOfLongestSubstring = function(s) {
   let result = 0;
   for (let i = 0; i < s.length; i++) {
       let tmp = [];
       for (let j = i; j < s.length; j++) {
           if (tmp.indexOf(s.charAt(j)) === -1) {
               tmp.push(s.charAt(j))
           } else {
               break;
           }
       }
       result = Math.max(result, tmp.length);
   }
   return result;
};
```

# 5.最长回文子串

> 中等

##### 题目

给定一个字符串`s`，找到`s`中最长的回文子串。你可以假设`s`的最大长度为`1000`。

##### 示例

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。

输入: "cbbd"
输出: "bb"

```

##### 中心扩散法

**中心扩散法** 利用回文数的性质，从中心向外扩散，找的不同的中心下最大的回文数。


```javascript
/**
 * @param {string} s
 * @return {string}
 */
const longestPalindrome = function(s) {
  let result = '';
  if (s.length <= 1) {
    return s;
  }
  /**
   * 
   * @param left
   * @param right
   * @returns {string} 返回该中心下找到的最大长度的回文字符串
   * 
   */
  let extend = (left, right) => {
    while (left >= 0 && right < s.length && s[left] === s[right]) {
      left--;
      right++;
    }
    return s.substring(left + 1, right); // 注意子串截取的位置
  };
  for (let i = 0; i < s.length; i++) {
    let str1 = extend(i, i);
    let str2 = extend(i, i + 1);
    if (str1.length > str2.length && str1.length > result.length) {
      result = str1
    } else if (str2.length > str1.length && str2.length > result.length) {
      result = str2
    }
  }
  return result;
};
```

```javascript
/**
 * @param {string} s
 * @return {string}
 */
const longestPalindrome = function(s) {
  if (s.length <= 1) {
    return s;
  }
  let start = 0;
  let end = 0;  // 记录已找到的最大的子串的起止坐标
  /**
   *
   * @param left
   * @param right
   * @returns {number} 返回该中心下找到的最大子串的长度
   */
  let extend = (left, right) => {
    while (left >= 0 && right < s.length && s[left] === s[right]) {
      left--;
      right++;
    }
    return right - left - 1;
  };
  for (let i = 0; i < s.length; i++) {
    const length1 = extend(i, i);
    const length2 = extend(i, i + 1);
    const maxLength = Math.max(length1, length2);
    if (maxLength > (end - start + 1)) { // 新的子串更长
      console.log(maxLength);
      // 计算新子串的起止坐标，其中心在i或者(i, i + 1)
      start = i - ((maxLength - 1) >> 1); 
      // >> 向右移位相当于丢掉后面的位数。此处丢调最后1位，相当于除以2并向下取整
      end = i + (maxLength >> 1);
      console.log(start, end);
    }
  }
  return s.substring(start, end + 1);
};
```

# 7.整数反转

> 简单

##### 题目

给出一个**32**位的有符号整数，你需要将这个整数中每位上的数字进行反转。

##### 示例

```
输入: 123
输出: 321

输入: -123
输出: -321

输入: 120
输出: 21
```
##### 思路
注意临界值数字。

##### 实现

```javascript
/**
 * @param {number} x
 * @return {number}
 */
const reverse = function(x) {
    if (x === 0) {
        return 0
    }
    let flag = false;
    if (x < 0) {
        x = -1 * x;
        flag = true
    }
    let result = parseInt(x.toString().split('').reverse().join(''));
    // 这里用到了字符分割为数组、数字的反转和拼接为字符串的方法
    if (result >= Math.pow(2, 31)) return 0;
    if (flag) result = -1 * result;
    return result
};
```

# 9.回文数

> 简单

##### 题目

给出一个**32**位的有符号整数，你需要将这个整数中每位上的数字进行反转。

##### 示例

```
输入: 121
输出: true

输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```
##### 进阶
你能不将整数转为字符串来解决这个问题吗？

##### 实现

```javascript
/**
 * @param {number} x
 * @return {boolean}
 */
const isPalindrome = function(x) {
    if (x === 0) return true;
    if (x < 0) return false;
    let tmp = parseInt(x.toString().split('').reverse().join(''));
    if (tmp === x) {
        return true
    } else {
        return false
    }
};
```

# 11.盛最多水的容器

> 中等

##### 题目

给定`n`个非负整数`a1，a2，...，an`，每个数代表坐标中的一个点`(i, ai)` 。在坐标内画`n`条垂直线，垂直线`i`的两个端点分别为`(i, ai)`和`(i, 0)`。
找出其中的两条线，使得它们与`x`轴共同构成的容器可以容纳最多的水。

##### 说明

你不能倾斜容器，且`n`的值至少为2。

##### 思路
容器的最大体积是由**最矮的边**和**两条边之间的距离**决定的。
两条边之间的距离是容易控制的（从最大到最小），那么只需要解决在两条边变化的过程中什么时候的面积最大。

##### 实现

```javascript
/**
 * @param {number[]} height
 * @return {number}
 */
const maxArea = function(height) {
  let result = 0; // 初始默认面积
  let start = 0;  // 最前面的指针
  let end = height.length - 1; // 最后面的指针
  while (start < end) {
    // 先计算一下当前指针所在位置的面积
    let area = Math.min(height[start], height[end]) * (end - start);
    result = Math.max(result, area);
    // 指针移动的逻辑
    if (height[start] < height[end]) { // 哪个边矮移动哪个边，因为是它决定了高度
      start++;
    } else {
      end--;
    }
  }
  return result;
};
```
# 13.罗马数字转整数

> 简单

##### 题目

罗马数字包含以下七种字符: `I，V，X，L，C，D`和`M`。

```
    字符      数值
    I         1
    V         5
    X         10
    L         50
    C         100
    D         500
    M         1000
```
例如，罗马数字`2`写做`II`，即为两个并列的`1`。`12`写做`XII`，即为`X + II`。
`27`写做`XXVII`, 即为`XX`+`V`+`II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。
但也存在特例，例如`4`不写做`IIII`，而是`IV`。数字`1`在数字`5`的左边，所表示的数等于大数`5`减小数`1`得到的数值`4`。
同样地，数字`9`表示为`IX`。这个特殊的规则只适用于以下六种情况：

* `I`可以放在`V`(5)和`X`(10)的左边，来表示`4`和`9`。
* `X`可以放在`L`(50)和`C`(100)的左边，来表示`40`和`90`。
* `C`可以放在`D`(500)和`M`(1000)的左边，来表示`400`和`900`。

给定一个罗马数字，将其转换成整数。输入确保在`1`到`3999`的范围内。

##### 示例
```
输入: "III"
输出: 3

输入: "IV"
输出: 4

输入: "IX"
输出: 9

输入: "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.

输入: "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

##### 思路
对于上述给的6种做减法的情况，都是两个的组合。所以最多判断两个就可以确定是做加法还是减法。

还有一点要注意的是**最后一个元素有可能在循环中漏掉**。因为循环条件中没有最后一个。

##### 实现

```javascript
/**
 * @param {string} s
 * @return {number}
 */
const romanToInt = function(s) {
    // 映射关系
    const map = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    }
    let sum = 0;
    let i = 0;
    while (i < s.length - 1) {
        let first = s.charAt(i);
        let second = s.charAt(i + 1);
        if (map[first] >= map[second]) {
            sum += map[first];
            i++;
        } else {
            sum += (map[second] - map[first]);
            i = i + 2;
        }
    }
    if (i === s.length - 1) {
        sum += map[s.charAt(i)]
    }
    return sum;
};
```

# 14.最长公共前缀

> 简单

##### 题目

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串`""`。

##### 示例
```
输入: ["flower","flow","flight"]
输出: "fl"

输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```

##### 说明
所有输入只包含小写字母**a-z**。

##### 实现

```javascript
/**
 * @param {string[]} strs
 * @return {string}
 */
const longestCommonPrefix = function(strs) {
    if (strs.length === 0) {
        return ''
    }
    let prefix = '';
    let count = strs[0].length;
    let tmp = strs[0];
    for (let i = 1; i < strs.length; i++) {
        if (count > strs[i].length) {
            count = strs[i].length;
            tmp = strs[i];
        }
    }
    for (let j = 0; j < count; j++) {
        let char = tmp.charAt(j);
        let flag = false;
        for (const item of strs) {
            if (item.charAt(j) !== char) {
                flag = true
            }
        }
        if (!flag) {
            prefix += char;
        } else {
            break;
        }
    }
    return prefix;
};
```
# 20.有效的括号

> 简单

##### 题目
给定一个只包括`'('，')'，'{'，'}'，'['，']'` 的字符串，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。

2. 左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。


##### 示例
```
输入: "()"
输出: true

输入: "()[]{}"
输出: true

输入: "(]"
输出: false

输入: "([)]"
输出: false

输入: "{[]}"
输出: true
```

##### 思路

对于都是括号的字符串，如果是符合题目条件的。那么，在字符串的最中间一定是一对匹配的括号。

##### 实现

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
const isValid = function(s) {
    if (s.length % 2 !== 0) {
          return false
      }
      // s中一定存在'{}','[]','()' 中的某一个形式
      while(s.length) {
          let tmp = s;
          s = s.replace('{}', '');
          s = s.replace('[]', '');
          s = s.replace('()', '');
          if (tmp === s) {
              return false
          }
      }
      return true;
};
```

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
const isValid = function(s) {
    if (s.length % 2 !== 0) {
        return false
      }
      let stack = [];
      const map = {
        '}': '{',
        ']': '[',
        ')': '('
      }
      const left = ['{', '(', '['];
      for (let i = 0; i < s.length; i++) {
        const char = s.charAt(i);
        if (left.indexOf(char) !== -1) {
          stack.push(char)
        } else {
          const tmp = stack.pop();
          if (tmp !== map[char]) {
            return false;
          }
        }
      }
      return !stack.length;
};
```

# 21.合并两个有序链表

> 简单

##### 题目

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

##### 示例
```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

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
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
const mergeTwoLists = function(l1, l2) {
    let p = l1;
    let q = l2;
    let dumy = new ListNode(1);
    let result = dumy;
    while (p || q) {
        let val1 = p ? p.val : Number.MAX_VALUE;
        let val2 = q ? q.val : Number.MAX_VALUE;
        if (val1 <= val2 && p) {
            result.next = new ListNode(val1);
            result = result.next;
            p = p.next
        } else {
            result.next = new ListNode(val2);
            result = result.next;
            q = q.next
        }
    }
    return dumy.next;
};
```
