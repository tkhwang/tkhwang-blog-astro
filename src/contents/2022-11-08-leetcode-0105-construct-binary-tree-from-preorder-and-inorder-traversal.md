---
author: tkhwang
title: "leetcode 105. Construct Binary Tree from Preorder and Inorder Traversal | medium | tree"
slug: 2022-11-08-leetcode-0105-construct-binary-tree-from-preorder-and-inorder-traversal
datetime: 2022-11-08T00:00:00Z
description: "leetcode 105. Construct Binary Tree from Preorder and Inorder Traversal | javascript | medium | tree"
tags:
  - tree
  - medium
---

## 🗒️ Problems

[Construct Binary Tree from Preorder and Inorder Traversal - LeetCode](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

```
Given two integer arrays preorder and inorder where preorder is the preorder traversal of a binary tree and inorder is the inorder traversal of the same tree, construct and return the binary tree.
```

```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```

## 🤔 First attempt

I couldn't find the clue how to solve this problem. <br />

- The characteristics of preorder and inorder traversal.

## ✨ Idea

- Using the characteristics of preorder and inorder traversal,
- find root, left tree and right tree
- build the tree recursively.

## 🍀 Intuition

### 💡🌲 Root. Where is it ?

- The first element of preorder is the very root.

```
            |
            \/
preorder = [3,9,20,15,7]
inorder  = [9,3,15,20,7]
```

### Where is the left tree and the right tree ?

I don't know.

### Could you find the root in inorder tree ?

```
            |
            \/
preorder = [3,9,20,15,7]
inorder  = [9,3,15,20,7]
              /\
               |
```

### Where is the left tree and the right tree, again ?

```
            |
            \/
preorder = [3,9,20,15,7]
inorder  = [9,3,15,20,7]
              /\
               |
  [ left tree ] [ right tree]
          [ 9 ] [ 15, 27, 7 ]
```

### 💡🌲 Could you find the preorder and inorder for the left tree and right tree ?

```javascript
const root = new TreeNode(preorder[0]);
const mid = inorder.indexOf(preorder[0]);
```

- left tree
  - `inorder` : `[0, mid)`
  - `prorder` : `[1, mid + 1)`
- right tree
  - `inorder` : `[mid+1, N)`
  - `preorder` : `[mid+1, N)`

```
            |
            \/
preorder = [3,9,20,15,7]
inorder  = [9,3,15,20,7]
              /\
               |
              mid
  [ left tree ] [ right tree]
          [ 9 ] [ 15, 27, 7 ]
```

## 🔥🌲 My Solution

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
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function (preorder, inorder) {
  const dfs = (preorder, inorder) => {
    if (preorder.length === 0 && inorder.length === 0) return null;

    const root = new TreeNode(preorder[0]);

    const mid = inorder.indexOf(preorder[0]);

    root.left = dfs(preorder.slice(1, mid + 1), inorder.slice(0, mid));
    root.right = dfs(preorder.slice(mid + 1), inorder.slice(mid + 1));

    return root;
  };

  return dfs(preorder, inorder);
};
```
