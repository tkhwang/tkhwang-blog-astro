---
author: tkhwang
title: "leetcode 931. Minimum Falling Path Sum | medium | dynamic-programming"
slug: 2022-10-21-leetcode-0931-minimum-falling-path-sum
datetime: 2022-10-21T00:00:00Z
description: "leetcode 931. Minimum Falling Path Sum | javascript | medium | dynamic-programming"
tags:
  - medium
  - dynamic-programming
---

## 🗒️ Problems

[Minimum Falling Path Sum - LeetCode](https://leetcode.com/problems/minimum-falling-path-sum/)

```
Given an n x n array of integers matrix, return the minimum sum of any falling path through matrix.

A falling path starts at any element in the first row and chooses the element in the next row that is either directly below or diagonally left/right. Specifically, the next element from position (row, col) will be (row + 1, col - 1), (row + 1, col), or (row + 1, col + 1).
```

## 🤔🔀 First attempt

At first, I thought that it can be solved by DFS traversal.

```javascript
var minFallingPathSum = function (matrix) {
  const [ROWS, COLS] = [matrix.length, matrix[0].length];

  const isValid = (r, c) => !(r < 0 || r >= ROWS || c < 0 || c >= COLS);
  const directions = [
    [1, -1],
    [1, 0],
    [1, 1],
  ];

  let min = Infinity;

  const dfs = (r, c, sum) => {
    sum += matrix[r][c];

    if (r === ROWS - 1) {
      if (min > sum) min = sum;
      return;
    }

    for (const [dR, dC] of directions) {
      const newR = r + dR;
      const newC = c + dC;

      if (!isValid(newR, newC)) continue;

      dfs(newR, newC, sum);
    }
  };

  for (let c = 0; c < COLS; c += 1) {
    dfs(0, c, 0);
  }

  return min;
};
```

하지만, TLE 발생합니다.

## ✨ Idea

- Can you think of dynamic programming in matrix ?

## 🍀 Intuition

### 🕸️💡 Dynamic programming pathing problem

- When there is a restriction of a cell to move in matrix, so that it cann't go backward, the problem can be solved using dynamic programming instead of as-is graph/matrix traversal.
  - For the state transfer equation, some directionality should exist.

### state

```javascript
const dp = Array(ROWS)
  .fill(null)
  .map(_ => Array(COLS).fill(0));
```

### basecase

The first row is set the same value in the given input `matrix[0]`.

```javascript
for (let c = 0; c < COLS; c += 1) {
  dp[0][c] = matrix[0][c];
}
```

### State transfer equation

We should find the minimum value.
For simplifying the out of bound problem, use the default value `Infinity` when the cell is out of bound.

```javascript
for (let r = 1; r < ROWS; r += 1) {
  for (let c = 0; c < COLS; c += 1) {
    dp[r][c] =
      matrix[r][c] +
      Math.min(
        dp[r - 1][c - 1] || Infinity,
        dp[r - 1][c] || Infinity,
        dp[r - 1][c + 1] || Infinity
      );
  }
}
```

## 🕸️⬆️🔥 My Solution

```javascript
/**
 * @param {number[][]} matrix
 * @return {number}
 */
var minFallingPathSum = function (matrix) {
  const [ROWS, COLS] = [matrix.length, matrix[0].length];

  const dp = Array(ROWS)
    .fill(null)
    .map(_ => Array(COLS).fill(0));
  for (let c = 0; c < COLS; c += 1) {
    dp[0][c] = matrix[0][c];
  }

  for (let r = 1; r < ROWS; r += 1) {
    for (let c = 0; c < COLS; c += 1) {
      dp[r][c] =
        matrix[r][c] +
        Math.min(
          dp[r - 1][c - 1] || Infinity,
          dp[r - 1][c] || Infinity,
          dp[r - 1][c + 1] || Infinity
        );
    }
  }

  return Math.min(...dp[ROWS - 1]);
};
```
