---
title: "leetcode 146. LRU Cache | medium | hashmap"
datetime: 2023-01-02T00:00:00Z
description: "leetcode 146. LRU Cache | javascript  | medium | hashmap"
slug: 2023-01-02-leetcode-146-lru-cache"
featured: false
draft: false
tags:
  - medium
  - hashmap
  - design
ogImage: ""
---

## 🗒️ Problems

[LRU Cache - LeetCode](https://leetcode.com/problems/lru-cache/)

```
Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.
Implement the LRUCache class:

* LRUCache(int capacity) Initialize the LRU cache with positive size capacity.
* int get(int key) Return the value of the key if the key exists, otherwise return -1.
* void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.
The functions get and put must each run in O(1) average time complexity.
```

```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1); // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2); // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1); // return -1 (not found)
lRUCache.get(3); // return 3
lRUCache.get(4); // return 4
```

## 🤔 First attempt

- It seems that the linked list is quite good data-structure for the quick update.
- But I like the hashmap better.

## 🍀 Intuition : What is the difference between object and hashmap?

### Object

- In latest browser, ascending order for number-like keys

```javascript
const map = {};
map[1] = 1;
map[2] = 2;
map[3] = 3;

// original key sequence
Object.keys(map);
(3)[("1", "2", "3")];

delete map[2];
map[2] = 2;

// key sequence after delete
Object.keys(map);
(3)[("1", "2", "3")];
```

Originally

```
An object is a member of the type Object. It is an unordered collection of properties each of which contains a primitive value, object, or function. A function stored in a property of an object is called a method.
```

[recently](https://stackoverflow.com/a/5525820/2453632)

```
The iteration order for objects follows a certain set of rules since ES2015, but it does not (always) follow the insertion order. Simply put, the iteration order is a combination of the insertion order for strings keys, and ascending order for number-like keys:
```

### Hashmap

- guarantees the keys to be iterated in order of insertion, without exception:

```javascript
const map = new Map();
map.set(1,1);
map.set(2,2);
map.set(3,3);

// original key sequence
map.keys();
MapIterator {1, 2, 3}

map.delete(2);
map.set(2,2);

// key sequence after delete
map.keys()
MapIterator {1, 3, 2}
```

[link](https://stackoverflow.com/a/5525820/2453632)

```
Using an array or a Map object can be a better way to achieve this. Map shares some similarities with Object and guarantees the keys to be iterated in order of insertion, without exception:
```

## ✨ Idea

- If the hasp map keys guarantee the order of insertion, then we can use the insertion order to get the order of iteration.
  - Whenever the latest access, delete and again set for making being latest one.

## 🔥 My Solution

```javascript
/**
 * @param {number} capacity
 */
var LRUCache = function (capacity) {
  this.cache = new Map();
  this.length = capacity;
};

/**
 * @param {number} key
 * @return {number}
 */
LRUCache.prototype.get = function (key) {
  if (!this.cache.has(key)) return -1;

  const value = this.cache.get(key);
  this.cache.delete(key);
  this.cache.set(key, value);
  return value;
};

/**
 * @param {number} key
 * @param {number} value
 * @return {void}
 */
LRUCache.prototype.put = function (key, value) {
  if (this.cache.has(key)) this.cache.delete(key);
  this.cache.set(key, value);
  if (this.cache.size > this.length) {
    const oldest = this.cache.keys().next().value;
    this.cache.delete(oldest);
  }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * var obj = new LRUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */
```
