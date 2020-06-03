# LeetCode-算法（551-575）

> LeetCode 算法类型题目（551-575）。

# 572.另一个树的子树

> 简单

##### 题目

给定两个非空二叉树 **s** 和 **t**，
检验 **s** 中是否包含和 **t** 具有相同结构和节点值的子树。 **s** 的一个子树包括 **s** 的一个节点和这个节点的所有子孙。
 **s** 也可以看做它自身的一棵子树。

##### 示例
```
给定的树 s:
     3
    / \
   4   5
  / \
 1   2
给定的树 t:
   4 
  / \
 1   2
返回 true, 因为 t 和 s 的一个子树拥有相同的结构和节点值。
```

##### 示例
```
给定的树 s:
     3
    / \
   4   5
  / \
 1   2
    /
   0
给定的树 t:
   4 
  / \
 1   2
返回 false。
```

##### 思路



##### 实现

```javascript
/**
 * @param {number} capacity
 */
const LRUCache = function(capacity) {
  this.capacity = capacity; // 缓存的容量
  this.cache = new Map(); // 缓存通过Map来实现，Map所存数据的顺序按插入的顺序
};

/** 
 * @param {number} key
 * @return {number}
 */
LRUCache.prototype.get = function(key) {
  // 如果获取到值，需要更新数据缓存的位置到最后
  const cache = this.cache;
  if (cache.has(key)) {
    const value = cache.get(key);
    cache.delete(key);
    cache.set(key, value);
    return value;
  } else {
    return -1;
  }
};

/** 
 * @param {number} key 
 * @param {number} value
 * @return {void}
 */
LRUCache.prototype.put = function(key, value) {
  // 插入值主要时要考虑容量不够的情况，需要把最前面（最远使用）的数据删除。
  // 如果插入的数据已经存在，则需要吧其位置更新
  const cache = this.cache;
  if (cache.has(key)) {
    cache.delete(key); // 删除一个，容量肯定不会超出
  } else { // 此时缓存可能是满的
    if (cache.size === this.capacity) {
      cache.delete(cache.keys().next().value);
    } 
  } 
  cache.set(key, value);
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * var obj = new LRUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */
```
