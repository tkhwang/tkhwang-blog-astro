---
title: "leetcode 1631. Path With Minimum Effort | medium | binary-search"
datetime: 2023-01-07T00:00:00Z
description: "leetcode 1631. Path With Minimum Effort | javascript  | medium | binary-search"
slug: "2023-01-07-leetcode-1631-path-with-minimum-effort"
featured: false
draft: false
tags:
  - medium
  - binary-search
ogImage: ""
---

## 🗒️ Problems

[1631. Path With Minimum Effort - Leetcode](https://leetcode.com/problems/path-with-minimum-effort/)

```
You are a hiker preparing for an upcoming hike. You are given heights, a 2D array of size rows x columns, where heights[row][col] represents the height of cell (row, col). You are situated in the top-left cell, (0, 0), and you hope to travel to the bottom-right cell, (rows-1, columns-1) (i.e., 0-indexed). You can move up, down, left, or right, and you wish to find a route that requires the minimum effort.

A route's effort is the maximum absolute difference in heights between two consecutive cells of the route.

Return the minimum effort required to travel from the top-left cell to the bottom-right cell.
```

```
Input: heights = [[1,2,2],[3,8,2],[5,3,5]]
Output: 2
Explanation: The route of [1,3,5,3,5] has a maximum absolute difference of 2 in consecutive cells.
This is better than the route of [1,2,2,2,5], where the maximum absolute difference is 3.
```

### Constraints:

```
rows == heights.length
columns == heights[i].length
1 <= rows, columns <= 100
1 <= heights[i][j] <= 106
```

## 🤔 First attempt

What do you see ?

- It seems that a typical matrix traversal problem, isn't it ?
- But Oh, no. I got TLE. Why ?

```javascript
/**
 * @param {number[][]} heights
 * @return {number}
 */
var minimumEffortPath = function (heights) {
  const [ROWS, COLS] = [heights.length, heights[0].length];

  const directions = [
    [1, 0],
    [-1, 0],
    [0, 1],
    [0, -1],
  ];
  const isValid = (r, c) => !(r < 0 || r >= ROWS || c < 0 || c >= COLS);
  const genKey = (r, c) => `${r}:${c}`;

  let min = Infinity;

  const dfs = (r, c, effort, seen) => {
    if (r === ROWS - 1 && c === COLS - 1) {
      if (min > effort) min = effort;
      return;
    }

    for (const [dR, dC] of directions) {
      const newR = r + dR;
      const newC = c + dC;

      if (!isValid(newR, newC)) continue;
      if (seen.has(genKey(newR, newC))) continue;

      seen.add(genKey(newR, newC));

      dfs(
        newR,
        newC,
        Math.max(effort, Math.abs(heights[newR][newC] - heights[r][c])),
        seen
      );

      seen.delete(genKey(newR, newC));
    }
  };

  const seen = new Set();
  seen.add(genKey(0, 0));
  dfs(0, 0, 0, seen);

  return min;
};
```

### 🙅‍♂️ Failed time complexity : `O(n)`

## ✨ Idea

## 🍀 Intuition

### ⬇️ top-down

### ⬆️ bottom-up

### 🔀 backtrack

### 🕸️ dynamic programming

### 🔗 Linked list

### 📏 Binary search

## 🪜 Steps

## 🌲 tree

## 🥞 stack

## ♻️ recursive

## ➡️ iterative

### 💡

## 🔥 My Solution

```javascript

```

### 🙆‍♂️ Time complexity: `O(n)`
