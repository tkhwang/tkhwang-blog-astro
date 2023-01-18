---
title: "leetcode Maximum (104), Minimum (111) Depth of Binary Tree | easy | tree"
datetime: 2022-12-27T00:00:00Z
description: "leetcode Maximum (104), Minimum (111) Depth of Binary Tree | javascript  | easy | tree"
slug: "2022-12-27-leetcode-104-maximum-minimum-depth-of-binary-tree"
featured: false
draft: false
tags:
  - easy
  - tree
ogImage: ""
---

## 🤔 First attempt

For solving tree problem, the meaning and difference of tree traversals is important.

- pre-order : ⬇️ top-down
- in-order
- post-order : ⬆️ bottom-up

## 🗒️ Problems

[104. Maximum Depth of Binary Tree - LeetCode](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

```
Given the root of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.
```

```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

### ✨⬆️🌲 Idea

- If you know the answer of its children, can you calculate the answer of that node?

If the answer is yes, solving the problem recursively using a bottom up approach might be a good idea.

- Using post-order traversal, traverse from the leaf node
- the depth of current node is the maximum depth of the left node or the right node plus one.

### 🔥⬆️🌲 My Solution

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var maxDepth = function (root) {
  const dfs = node => {
    if (!node) return 0;

    const left = dfs(node.left);
    const right = dfs(node.right);

    return 1 + Math.max(left, right);
  };

  return dfs(root);
};
```

## 🗒️ Problems

[111. Minimum Depth of Binary Tree - LeetCode](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

```
Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Note: A leaf is a node with no children.
```

```
Input: root = [3,9,20,null,null,15,7]
Output: 2
```

### ✨⬇️🌲 Idea

- Can you determine some parameters to help the node know its answer?
- Can you use these parameters and the value of the node itself to determine what should be the parameters passed to its children?

If the answers are both yes, try to solve this problem using a "top-down" recursive solution.

- Parent node provides the depth information.
- At each node, check whether it's a leaf node and if so, calculate the min depth.

### 🔥⬇️🌲 My Solution

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var minDepth = function (root) {
  if (!root) return 0;

  let min = Infinity;

  const dfs = (node, depth) => {
    if (!node) return 0;

    if (!node.left && !node.right) {
      min = Math.min(min, depth);
    }

    dfs(node.left, 1 + depth);
    dfs(node.right, 1 + depth);
  };

  dfs(root, 1);

  return min;
};
```
