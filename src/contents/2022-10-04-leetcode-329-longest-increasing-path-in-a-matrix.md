---
author: tkhwang
title: "leetcode 329. Longest Increasing Path in a Matrix | hard | graph"
slug: 2022-10-04-leetcode-329-longest-increasing-path-in-a-matrix
datetime: 2022-10-04T00:00:00Z
description: "leetcode 329. Longest Increasing Path in a Matrix | javascript  | hard | graph"
tags:
  - hard
  - graph
---

## 🗒️ Problem

- [Longest Increasing Path in a Matrix - LeetCode](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

![img](https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg)

```
Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
Output: 4
Explanation: The longest increasing path is [1, 2, 6, 9].
```

## 🍀 Intuition

### cache 구현

- graph traverse 하면서 모든 지점에서 DFS 로 탐색하면서 increasing path를 각각 찾아야 하므로 그냥하면 TLE 발생함.
- top level recursion with memoization

### recursive DFS 탐색

- BFS에서는 traverse 에서는 이전에 해당 조건을 확인하여 맞는 경우에는 traverse 하는 경우가 많은데
- DFS에서는 일단 traverse 한 다음에 그 node 에서 조건을 확인해서 invalid하면 return 처리하는 경우가 많네요.

### recursive DFS 에서의 return value 처리 방법

- recursive DFS 에서 return value를 모아서 처리하는 방법.

### Graph DFS 에서 visited 관리 방법

- Graph traverse 에서는 visited 관리를 하기 마련인데, 이번에는 increasing path 를 찾아가는 과정이므로 되돌아가는 경우에는 increasing path가 만족하지 않기 때문에 따로 관리하지 않아도 됨.

## 🔥 My Solution

```javascript
var longestIncreasingPath = function (matrix) {
  const [ROWS, COLS] = [matrix.length, matrix[0].length];

  let max = -Infinity;
  const cache = new Map();

  const directions = [
    [1, 0],
    [-1, 0],
    [0, 1],
    [0, -1],
  ];
  const isValid = (r, c) => !(r < 0 || r >= ROWS || c < 0 || c >= COLS);
  const genKey = (r, c) => `${r}:${c}`;

  const dfs = (r, c, parent) => {
    if (!isValid(r, c)) return 0;
    if (parent >= matrix[r][c]) return 0;
    if (cache.has(genKey(r, c))) return cache.get(genKey(r, c));

    let localMax = -Infinity;

    for (const [dR, dC] of directions) {
      const newR = r + dR;
      const newC = c + dC;

      localMax = Math.max(localMax, 1 + dfs(newR, newC, matrix[r][c]));
    }

    cache.set(genKey(r, c), localMax);
    return localMax;
  };

  for (let r = 0; r < ROWS; r += 1) {
    for (let c = 0; c < COLS; c += 1) {
      max = Math.max(max, dfs(r, c, -Infinity));
    }
  }

  return max;
};
```
