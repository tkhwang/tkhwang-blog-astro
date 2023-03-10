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

## ποΈ Problem

- [Longest Increasing Path in a Matrix - LeetCode](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

![img](https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg)

```
Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
Output: 4
Explanation: The longest increasing path is [1, 2, 6, 9].
```

## π Intuition

### cache κ΅¬ν

- graph traverse νλ©΄μ λͺ¨λ  μ§μ μμ DFS λ‘ νμνλ©΄μ increasing pathλ₯Ό κ°κ° μ°ΎμμΌ νλ―λ‘ κ·Έλ₯νλ©΄ TLE λ°μν¨.
- top level recursion with memoization

### recursive DFS νμ

- BFSμμλ traverse μμλ μ΄μ μ ν΄λΉ μ‘°κ±΄μ νμΈνμ¬ λ§λ κ²½μ°μλ traverse νλ κ²½μ°κ° λ§μλ°
- DFSμμλ μΌλ¨ traverse ν λ€μμ κ·Έ node μμ μ‘°κ±΄μ νμΈν΄μ invalidνλ©΄ return μ²λ¦¬νλ κ²½μ°κ° λ§λ€μ.

### recursive DFS μμμ return value μ²λ¦¬ λ°©λ²

- recursive DFS μμ return valueλ₯Ό λͺ¨μμ μ²λ¦¬νλ λ°©λ².

### Graph DFS μμ visited κ΄λ¦¬ λ°©λ²

- Graph traverse μμλ visited κ΄λ¦¬λ₯Ό νκΈ° λ§λ ¨μΈλ°, μ΄λ²μλ increasing path λ₯Ό μ°Ύμκ°λ κ³Όμ μ΄λ―λ‘ λλμκ°λ κ²½μ°μλ increasing pathκ° λ§μ‘±νμ§ μκΈ° λλ¬Έμ λ°λ‘ κ΄λ¦¬νμ§ μμλ λ¨.

## π₯ My Solution

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
