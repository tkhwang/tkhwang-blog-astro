---
author: tkhwang
title: "leetcode 236. Lowest Common Ancestor of a Binary Tree | medium | tree"
slug: 2022-11-02-leetcode-0236-lowest-common-ancestor-of-a-binary-tree
datetime: 2022-11-02T00:00:00Z
description: "leetcode 236. Lowest Common Ancestor of a Binary Tree | javascript | medium | tree"
tags:
  - medium
  - tree
---

## 🗒️ Problems

[Lowest Common Ancestor of a Binary Tree - LeetCode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

```
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”
```

## 🤔 First attempt

At first, I just tried the procedure approach.

- From the one node, find `ancestor` until the root.
- From the another node, find the common node among previously found ancestors from that node to the root.

```javascript
var lowestCommonAncestor = function (root, p, q) {
  const dfs = (node, parent) => {
    if (!node) return;

    node.parent = parent;

    dfs(node.left, node);
    dfs(node.right, node);
  };

  dfs(root, null);

  const ancestors = new Set();

  while (p) {
    ancestors.add(p);
    p = p.parent;
  }

  while (!ancestors.has(q)) {
    q = q.parent;
  }

  return q;
};
```

## ✨ Idea

- Let's think about the recursive way.
- Especially ⬆️ bottom-up postorder traversal.

## 🍀 Intuition

### 🌲💡 Recursive way : ⬆️ bottom-up postorder traversal

```javascript
const left = dfs(node.left, p, q);
const right = dfs(node.right, p, q);

// post-order
```

Can you proceed further based on the result ?

- the result of the children nodes : left, right
- current node.

## basecase

- If the node is null, just return null.

```javascript
const dfs = (node, p, q) => {
  if (!node) return null;
};
```

- If the current node is matching either the given two reference nodes, just return the current node.

```javascript
if (node === p || node === q) return node;
```

## 💡🌲 Case #1 : If the both `left` and `right` are found.

- If the both of `left` and `right` nodes are found, this means that this node is LCA.
- Now we are using **post-order recursion**.
- Therefore the first node which finds these both `left` and `right` will be LCA.

```javascript
if (left && right) return node;
```

## 💡🌲 Case #2 : If none of `left` and `right` are found.

- If the none of the `left` and `right` nodes are found, there is no LCA.

```javascript
if (!left && !right) return null;
```

## 💡🌲 Case #3 : If only either `left` or `right` is found

- If only one of `left` and `right` is found, that node is LCA.

```javascript
if (left || right) return left || right;
```

## 🔥🌲 My Solution

```javascript
/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function (root, p, q) {
  const dfs = (node, p, q) => {
    if (!node) return null;

    if (node === p || node === q) return node;

    const left = dfs(node.left, p, q);
    const right = dfs(node.right, p, q);

    if (left && right) return node;
    if (!left && !right) return null;
    if (left || right) return left || right;
  };

  return dfs(root, p, q);
};
```
