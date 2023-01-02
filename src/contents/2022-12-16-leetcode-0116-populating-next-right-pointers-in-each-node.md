---
author: tkhwang
title: "leetcode 116. Populating Next Right Pointers in Each Node | medium | tree"
slug: 2022-12-16-leetcode-0116-populating-next-right-pointers-in-each-node
datetime: 2022-12-16T00:00:00Z
update: 2022-12-16
description: "leetcode 116. Populating Next Right Pointers in Each Node | javascript | medium | tree"
tags:
  - medium
  - tree
---

## 🗒️ Problems

[Populating Next Right Pointers in Each Node - LeetCode](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)

```
You are given a perfect binary tree where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.
```

```
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.

```

## 🤔🌲 First attempt

- I used the level traversal using BFS.
- During the level traversal, save those nodes in the same level and then populate the next pointer by handling those nodes in the same level.

```javascript
var connect = function (root) {
  if (!root) return null;

  const queue = [root];

  while (queue.length > 0) {
    const len = queue.length;

    const level = [];
    for (let i = 0; i < len; i += 1) {
      const cur = queue.shift();

      level.push(cur);

      if (cur.left) queue.push(cur.left);
      if (cur.right) queue.push(cur.right);
    }

    for (let i = 0; i < level.length; i += 1) {
      const curNode = level[i];
      if (i < level.length - 1) {
        const nextNode = level[i + 1];
        curNode.next = nextNode;
      } else {
        curNode.next = null;
      }
    }

    level.length = 0;
  }

  return root;
};
```

But, it seems that this node handling in the same level can be optimized better.

## ✨ Idea

![img](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

- Can I use the current processing node and the remaining nodes in the BFS queue ?

### 🌲💡 Level traversal result.

```
[
    [1],
    [2,3],
    [4,5,6,7]
]
```

### 🌲💡 What's in the BFS queue ?

- Let's check the current processing node and the remaining nodes in BFS queue.
  - The next right node is first node in the BFS queue if available.

```
[3]
processing 2  // cur.next = next => first in queue)

[]
processing 3  // cur.next = null => because queue is empty.
```

## 🔥🌲 My Solution

```javascript
var connect = function (root) {
  if (!root) return null;

  const queue = [root];

  while (queue.length > 0) {
    const len = queue.length;

    for (let i = 0; i < len; i += 1) {
      const cur = queue.shift();

      if (i < len - 1) {
        cur.next = queue.at(0);
      } else {
        cur.next = null;
      }

      if (cur.left) queue.push(cur.left);
      if (cur.right) queue.push(cur.right);
    }
  }

  return root;
};
```
