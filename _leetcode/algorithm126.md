# LeetCode-算法（126-150）

> LeetCode 算法类型题目（126-150）。

# 146.LRU缓存机制

> 简单

##### 题目

运用你所掌握的数据结构，设计和实现一个`LRU(最近最少使用)`缓存机制。它应该支持以下操作： 获取数据`get`和写入数据`put`。

获取数据`get(key)`- 如果密钥 (key) 存在于缓存中，则获取密钥的值（总是正数），否则返回 -1。

写入数据`put(key, value)`- 如果密钥不存在，则写入其数据值。当缓存容量达到上限时，它应该在写入新数据之前删除最近最少使用的数据值，从而为新的数据值留出空间。

##### 进阶
你是否可以在 O(1) 时间复杂度内完成这两种操作？

##### 示例
```
LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回  1
cache.put(3, 3);    // 该操作会使得密钥 2 作废
cache.get(2);       // 返回 -1 (未找到)
cache.put(4, 4);    // 该操作会使得密钥 1 作废
cache.get(1);       // 返回 -1 (未找到)
cache.get(3);       // 返回  3
cache.get(4);       // 返回  4
```

##### 思路
最近最少使用，最重要的一点就是当我们操作一个值时，它最变成了最近使用的项。需要把它按指定的顺序放在最前。

1.可以利用ES6 Map进行数据的存储。

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
