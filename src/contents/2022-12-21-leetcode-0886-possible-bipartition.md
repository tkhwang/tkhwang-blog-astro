---
author: tkhwang
datetime: 2022-12-21T00:00:00Z
title: "leetcode 886. Possible Bipartition | medium | bfs"
slug: 2022-12-21-leetcode-0886-possible-bipartition
featured: false
draft: false
tags:
  - medium
  - bfs
ogImage: ""
description: leetcode 886. Possible Bipartition | javascript | medium | bfs
---

## 🗒️ Problems

[(1) Possible Bipartition - LeetCode](https://leetcode.com/problems/possible-bipartition/)

```
We want to split a group of n people (labeled from 1 to n) into two groups of any size. Each person may dislike some other people, and they should not go into the same group.

Given the integer n and the array dislikes where dislikes[i] = [ai, bi] indicates that the person labeled ai does not like the person labeled bi, return true if it is possible to split everyone into two groups in this way.
```

## 🤔 First attempt

It doesn't work...

- Navigate the dislikes nodes and assign the alternating color.
- If it fails before the end, it fails.

```javascript
var possibleBipartition = function (N, dislikes) {
  const colors = Array(N + 1);
  colors[0] = -1;

  const BLACK = -1;
  const WHITE = 1;

  for (const [one, two] of dislikes) {
    if (colors[one] === undefined && colors[two] === undefined) {
      colors[one] = BLACK;
      colors[two] = WHITE;
    } else if (colors[one] === undefined && colors[two] !== undefined) {
      const taken = colors[two];
      colors[one] = taken * -1;
    } else if (colors[one] !== undefined && colors[two] === undefined) {
      const taken = colors[one];
      colors[two] = taken * -1;
    } else if (colors[one] !== undefined && colors[two] !== undefined) {
      if (colors[one] === colors[two]) return false;
    }
  }
  return true;
};
```

## 🍀 Intuition

How to solve this problem ?

```
The nodes in the same dislikes group should be split into two groups.
```

### 🍀💡 Think as the graph problem with coloring the node.

Let's think about this as the graph problem.

- `node`
- connections between dislikes group => `edge`
- node in the specific group => `colorize the node`
  - `RED` color
  - `BLUE` color

## ✨ Idea

- build the adjacency list.
- Initially set each node the default color `NONE`;
- Traverse the node from `1` to `n-1`.
  - If the node is not colored yet, do `BFS()` with alternating coloring.
  - if the node was already colored, check the color alternating.

## 🔥 My Solution

```javascript
/**
 * @param {number} n
 * @param {number[][]} dislikes
 * @return {boolean}
 */
var possibleBipartition = function (N, dislikes) {
  if (!dislikes || dislikes.length === 0) return true;

  const graph = {};
  for (const [one, two] of dislikes) {
    if (graph[one] === undefined) graph[one] = [];
    if (graph[two] === undefined) graph[two] = [];
    graph[one].push(two);
    graph[two].push(one);
  }

  const NONE = -1;
  const RED = 0;
  const BLUE = 1;

  const colors = Array(N + 1).fill(NONE);

  const bfs = start => {
    const queue = [start];

    colors[start] = RED;

    while (queue.length) {
      const len = queue.length;

      for (let i = 0; i < len; i += 1) {
        const cur = queue.shift();

        if (graph[cur] === undefined) continue;

        for (const next of graph[cur]) {
          if (colors[cur] === colors[next]) return false;

          if (colors[next] === NONE) {
            colors[next] = 1 - colors[cur];
            queue.push(next);
          }
        }
      }
    }
    return true;
  };

  for (let i = 1; i < N + 1; i += 1) {
    if (colors[i] === NONE) {
      if (!bfs(i)) return false;
    }
  }
  return true;
};
```
