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

## ποΈ Problems

[Sudoku Solver - LeetCode](https://leetcode.com/problems/sudoku-solver/)

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

## β¨ Idea

- λΉμ΄μλ μΉΈμ 1λΆν° 9κΉμ§ νλμ© backtrack μΌλ‘ μ μ©.
- backtrack μμλ νλμ© μ€λ₯Έμͺ½μΌλ‘ μ§ννκ³ , col λκΉμ§ κ°λ©΄ λ€μ row μ²μλΆν° μ§ν.
- μ§ννλ©΄μ valid νμ§ μμΌλ©΄ return νλ©΄μ backtrack μμ λ³κ²½μ λ³΅μν΄μ λ€λ₯Έ λ³κ²½μ μ μ©ν¨.
- valid ν λ³κ²½μ κ²½μ° κ³μ μ§νν΄μ λκΉμ§ λλ¬νλ©΄ return true νμ¬ μ’λ£ν¨.

## π Intuition

### 9x9 matrix μμμ 3x3 box indexing

```javascript
const rc2box = (r, c) => 3 * Math.floor(r / 3) + Math.floor(c / 3);
```

### hashset μ μ΄μ©ν validity check

- n-queens μ κ°μ΄ row, column, box λ¨μλ‘ valid ν μ«μλ₯Ό hash set μΌλ‘ κ΅¬μ±νμ¬ validity λ₯Ό μ²΄ν¬ν¨.

```javascript
const possible = (r, c, value) => {
  if (setRow[r].has(value)) return false;
  if (setCol[c].has(value)) return false;
  if (setBox[rc2box(r, c)].has(value)) return false;
  return true;
};
```

### π κΈ°λ³Έ backtrack μ μ΄μ©ν μ«μ μμ±

- μ«μ μμ± -> backtrack λ€μ μ§ν.
- μ§ν νλ€κ° μ€ν¨ μ return νμ¬ κΈ° μ μ©λμλ λ³κ²½ μ¬ν­μ μ κ±°νκ³  λ€λ₯Έ λ³κ²½ μ¬ν­ μ μ©.

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

### π π‘ Backtrack μ μ’λ£ μ²λ¦¬

- backtrack μ ν΅ν μ§νμ νλ©΄μ λ§μ§λ§ μ’λ£ μ²λ¦¬λ₯Ό μ΄λ»κ² ν΄μΌν μ§ μ΄λ €μ λ€...
- backtrack μ΅μ’ μ’λ£ ν μ΄νμ backtrack λ³κ²½ μ¬ν­ μ€νλμ§ μλλ‘ νκΈ° μν΄μ boolean return νλλ‘ ν΄μ μ΄μ λ°λΌμ true return νλ κ²½μ°μλ μ μ²΄ return νλλ‘ ν¨.

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

## π π₯ My Solution

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
