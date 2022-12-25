---
title: "leetcode 931. Minimum Falling Path Sum | medium | dynamic-programming"
datetime: 2022-10-21T00:00:00Z
description: "leetcode 931. Minimum Falling Path Sum | javascript | medium | dynamic-programming"
tags:
  - dynamic-programming
  - medium
---

## 🗒️ Problems

[Minimum Falling Path Sum - LeetCode](https://leetcode.com/problems/minimum-falling-path-sum/)

Given an n x n array of integers matrix, return the minimum sum of any falling path through matrix.

A falling path starts at any element in the first row and chooses the element in the next row that is either directly below or diagonally left/right. Specifically, the next element from position (row, col) will be (row + 1, col - 1), (row + 1, col), or (row + 1, col + 1).

## 🤔🔀 First attempt

이 문제는 딱 보면 matrix에서 DFS traverse 다.

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

- matrix 문제를 traverse 문제가 아니라
- dynamic programming의 path problem 으로 생각하여 DP 로 해결한다.

## 🍀 Intuition

### 🕸️💡 Dynamic programming pathing problem

- 일반적인 matrix traverse 문제에서 **움직이는 방향에 제약**이 걸려져 있어서 backward 돌아가는 일이 없는 경우에는 단순 traverse 가 아니라 DP 로서 문제 해결이 가능함.
  - DP의 상태 전이 방정식이 성립하려면 일정한 방향성이 존재해야 하니깐...

### state

```javascript
const dp = Array(ROWS)
  .fill(null)
  .map(_ => Array(COLS).fill(0));
```

### basecase

첫 row는 해당 값 그대로 `dp[0]` 설정됨.

```javascript
for (let c = 0; c < COLS; c += 1) {
  dp[0][c] = matrix[0][c];
}
```

### 상태 전이 방정식

최저값을 찾아야 하므로 윗줄에서 boundary 를 벗어나는 경우를 undefined value 대신에 Infinity 로 설정해서 값이 invalid 하도록 함.

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
