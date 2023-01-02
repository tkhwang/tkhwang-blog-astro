---
author: tkhwang
title: "leetcode 36. Valid Sudoku | medium | hash-map"
slug: 2022-11-23-leetcode-36-valid-sudoku
datetime: 2022-11-23T00:00:00Z
description: "leetcode 36. Valid Sudoku | javascript | medium | hash-map"
tags:
  - medium
  - hashmap
---

One of the basic famous problem of Sudoku. <br />
And the advanced one is the [Sudoku solver](https://tkhwang.me/2022-10-12-leetcode-0037-sudoku-solver/)

## 🗒️ Problems

[Valid Sudoku - LeetCode](https://leetcode.com/problems/valid-sudoku/)

```
Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

1. Each row must contain the digits 1-9 without repetition.
2. Each column must contain the digits 1-9 without repetition.
3. Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.
```

#### Note:

- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.

## 🤔 First attempt

## ✨ Idea

- Navigate every dot in the board.
- According to the sudoku rule, check the wrong number using the hash map.
  - If you find the wrong place number, it fails.
- If it is not wrong, add to the corresponding (row, col, box) sets.
- Move the next until the end of the board.

## 🍀 Intuition

### ROW/COLS set

- The row set and col set are easy to use just using its index.

### 💡 BOX set

- What about box ?
- how to map box coordinate `(r, c)` to the one dimensional set array.

```javascript
const m = ROWS / 3;
const rc2box = (r, c) => Math.floor(r / m) * m + Math.floor(c / m);
```

## 🔥 My Solution

```javascript
/**
 * @param {character[][]} board
 * @return {boolean}
 */
var isValidSudoku = function (board) {
  const [ROWS, COLS] = [board.length, board[0].length];

  const setRows = Array(ROWS)
    .fill(null)
    .map(_ => new Set());
  const setCols = Array(COLS)
    .fill(null)
    .map(_ => new Set());
  const setBoxs = Array(ROWS)
    .fill(null)
    .map(_ => new Set());

  const m = ROWS / 3;
  const rc2box = (r, c) => Math.floor(r / m) * m + Math.floor(c / m);

  for (let r = 0; r < ROWS; r += 1) {
    for (let c = 0; c < COLS; c += 1) {
      const cur = board[r][c];
      if (cur === ".") continue;

      if (setRows[r].has(cur)) return false;
      setRows[r].add(cur);

      if (setCols[c].has(cur)) return false;
      setCols[c].add(cur);

      if (setBoxs[rc2box(r, c)].has(cur)) return false;
      setBoxs[rc2box(r, c)].add(cur);
    }
  }

  return true;
};
```
