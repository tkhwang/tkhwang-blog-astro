---
author: tkhwang
title: "leetcode 872. Leaf-Similar Trees | easy | tree"
slug: 2022-12-08-leetcode-872-leaf-similar-trees
datetime: 2022-12-08T00:00:00Z
description: "leetcode 872. Leaf-Similar Trees | javascript | easy | tree"
tags:
  - easy
  - tree
---

This problem is the basic tree problem, but I come to know one thing.

## 🗒️ Problems

[Leaf-Similar Trees - LeetCode](https://leetcode.com/problems/leaf-similar-trees/)

```
Consider all the leaves of a binary tree, from left to right order, the values of those leaves form a leaf value sequence.
```

```
Input: root1 = [3,5,1,6,2,9,8,null,null,7,4], root2 = [3,5,1,6,7,4,2,null,null,null,null,null,null,9,8]
Output: true
```

## 🤔 First attempt

OK. Basic tree traversing problem.

```javascript
var leafSimilar = function (root1, root2) {
  const leaf1 = [];
  const leaf2 = [];

  const dfs = (node, arr) => {
    if (!node) return;

    if (!node.left && !node.right) arr.push(node.val);

    dfs(node.left, arr);
    dfs(node.right, arr);
  };

  dfs(root1, leaf1);
  dfs(root2, leaf2);

  return leaf1.join("") === leaf2.join("");
};
```

But it fails is some test cases. Why ???

```
[3,5,1,6,2,9,8,null,null,7,14]
[3,5,1,6,71,4,2,null,null,null,null,null,null,9,8]
```

## 🍀 Intuition

Let's check the failed testcase more in detail.

```
//[ 6, 7, 14, 9, 8 ]
[3,5,1,6,2,9,8,null,null,7,14]

//[ 6, 71, 4, 9, 8 ]
[3,5,1,6,71,4,2,null,null,null,null,null,null,9,8]
```

### 💡 `join` delimiter

If the wrong numbers are joined to be the same number like...

- [ 6, 7, 14, 9, 8 ] => 671498
- [ 6, 71, 4, 9, 8 ] => 671498

There two sequences are different, but they has the same result because I didn't use the **delimiter for join operator**.

## 🔥 My Solution

```javascript
/**
 * @param {TreeNode} root1
 * @param {TreeNode} root2
 * @return {boolean}
 */
var leafSimilar = function (root1, root2) {
  const leaf1 = [];
  const leaf2 = [];

  const dfs = (node, arr) => {
    if (!node) return;

    if (!node.left && !node.right) arr.push(node.val);

    dfs(node.left, arr);
    dfs(node.right, arr);
  };

  dfs(root1, leaf1);
  dfs(root2, leaf2);

  return leaf1.length === leaf2.length && leaf1.every((v, i) => leaf2[i] === v);
};
```
