---
author: tkhwang
title: "leetcode 37. Sudoku Solver | hard | backtrack"
slug: 2022-10-12-leetcode-0037-sudoku-solver
datetime: 2022-10-12T00:00:00Z
description: "leetcode 37. Sudoku Solver | javascript | hard | backtrack"
tags:
  - hard
  - backtrack
---

## 🗒️ Problems

[Sudoku Solver - LeetCode](https://leetcode.com/problems/sudoku-solver/)

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

## ✨ Idea

- 비어있는 칸에 1부터 9까지 하나씩 backtrack 으로 적용.
- backtrack 에서는 하나씩 오른쪽으로 진행하고, col 끝까지 가면 다음 row 처음부터 진행.
- 진행하면서 valid 하지 않으면 return 하면서 backtrack 에서 변경을 복원해서 다른 변경을 적용함.
- valid 한 변경의 경우 계속 진행해서 끝까지 도달하면 return true 하여 종료함.

## 🍀 Intuition

### 9x9 matrix 안에서 3x3 box indexing

```javascript
const rc2box = (r, c) => 3 * Math.floor(r / 3) + Math.floor(c / 3);
```

### hashset 을 이용한 validity check

- n-queens 와 같이 row, column, box 단위로 valid 한 숫자를 hash set 으로 구성하여 validity 를 체크함.

```javascript
const possible = (r, c, value) => {
  if (setRow[r].has(value)) return false;
  if (setCol[c].has(value)) return false;
  if (setBox[rc2box(r, c)].has(value)) return false;
  return true;
};
```

### 🔀 기본 backtrack 을 이용한 숫자 생성

- 숫자 생성 -> backtrack 다음 진행.
- 진행 하다가 실패 시 return 하여 기 적용되었던 변경 사항을 제거하고 다른 변경 사항 적용.

```javascript
for (let i = 1; i <= 9; i += 1) {
  if (!possible(r, c, i)) continue;

  // apply change
  board[r][c] = "" + i;
  addSet(r, c, i);

  // go further
  if (backtrack(r, c + 1)) return true;

  // remove change
  board[r][c] = ".";
  deleteSet(r, c, i);
}
```

### 🔀 💡 Backtrack 시 종료 처리

- backtrack 을 통한 진행을 하면서 마지막 종료 처리를 어떻게 해야할지 어려웠다...
- backtrack 최종 종료 한 이후에 backtrack 변경 사항 실행되지 않도록 하기 위해서 boolean return 하도록 해서 이에 따라서 true return 하는 경우에는 전체 return 하도록 함.

```javascript
    const backtrack = (r, c) => {
        ...
        for (let i = 1; i <= 9; i += 1) {
            if (!possible(r, c, i)) continue;

            board[r][c] = "" + i;
            addSet(r, c, i);

            if (backtrack(r, c+1)) return true;

            board[r][c] = ".";
            deleteSet(r, c, i);
        }
        return false;
    }

    backtrack(0, 0);
```

## 🔀 🔥 My Solution

```javascript
/**
 * @param {character[][]} board
 * @return {void} Do not return anything, modify board in-place instead.
 */
var solveSudoku = function (board) {
  const [ROWS, COLS] = [board.length, board[0].length];

  const setRow = Array(ROWS)
    .fill(null)
    .map(_ => new Set());
  const setCol = Array(COLS)
    .fill(null)
    .map(_ => new Set());
  const setBox = Array(ROWS)
    .fill(null)
    .map(_ => new Set());

  const rc2box = (r, c) => 3 * Math.floor(r / 3) + Math.floor(c / 3);

  for (let r = 0; r < ROWS; r += 1) {
    for (let c = 0; c < COLS; c += 1) {
      if (board[r][c] === ".") continue;

      setRow[r].add(+board[r][c]);
      setCol[c].add(+board[r][c]);
      setBox[rc2box(r, c)].add(+board[r][c]);
    }
  }

  const addSet = (r, c, value) => {
    setRow[r].add(+value);
    setCol[c].add(+value);
    setBox[rc2box(r, c)].add(+value);
  };

  const deleteSet = (r, c, value) => {
    setRow[r].delete(+value);
    setCol[c].delete(+value);
    setBox[rc2box(r, c)].delete(+value);
  };

  const possible = (r, c, value) => {
    if (setRow[r].has(value)) return false;
    if (setCol[c].has(value)) return false;
    if (setBox[rc2box(r, c)].has(value)) return false;
    return true;
  };

  const backtrack = (r, c) => {
    if (c === COLS) {
      r += 1;
      c = 0;
    }
    if (r === ROWS) {
      return true;
    }

    if (board[r][c] !== ".") return backtrack(r, c + 1);

    for (let i = 1; i <= 9; i += 1) {
      if (!possible(r, c, i)) continue;

      board[r][c] = "" + i;
      addSet(r, c, i);

      if (backtrack(r, c + 1)) return true;

      board[r][c] = ".";
      deleteSet(r, c, i);
    }
    return false;
  };

  backtrack(0, 0);

  return board;
};
```
