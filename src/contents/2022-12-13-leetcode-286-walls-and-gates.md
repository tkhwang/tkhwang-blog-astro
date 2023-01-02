---
author: tkhwang
title: "leetcode 286. Walls and Gates | medium | bfs"
slug: 2022-12-13-leetcode-286-walls-and-gates
datetime: 2022-12-13T00:00:00Z
update: 2022-12-13
description: "leetcode 286. Walls and Gates | javascript | medium | bfs"
tags:
  - medium
  - bfs
---

## 🗒️ Problems

[Walls and Gates - LeetCode](https://leetcode.com/problems/walls-and-gates/)

```
You are given an m x n grid rooms initialized with these three possible values.

- `-1` A wall or an obstacle.
- `0` A gate.
- `INF` Infinity means an empty room. We use the value 231 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.
```

```
Input: rooms = [[2147483647,-1,0,2147483647],[2147483647,2147483647,2147483647,-1],[2147483647,-1,2147483647,-1],[0,-1,2147483647,2147483647]]
Output: [[3,-1,0,1],[2,2,1,-1],[1,-1,2,-1],[0,-1,3,4]]
```

## 🤔 First attempt

- Just BFS.
- If I found a gate, bfs from it.
- Therefore it should bfs twice.

But I got TLE.

## ✨ Idea

- Can it start from the both gates simultaneously ?
  - Why not...

## 🍀 💡 Intuition

### Gather the gate nodes.

```javascript
const queue = [];
for (let r = 0; r < ROWS; r += 1) {
  for (let c = 0; c < COLS; c += 1) {
    if (rooms[r][c] === 0) queue.push([r, c]);
  }
}

bfs(queue, 0);
```

### Start from the gates simultaneously

```javascript
bfs(queue, 0);
```

## 🔥 My Solution

```javascript
/**
 * @param {number[][]} rooms
 * @return {void} Do not return anything, modify rooms in-place instead.
 */
var wallsAndGates = function (rooms) {
  const [ROWS, COLS] = [rooms.length, rooms[0].length];
  const EMPTY = 2147483647;

  const genKey = (r, c) => `${r}:${c}`;

  const directions = [
    [1, 0],
    [-1, 0],
    [0, 1],
    [0, -1],
  ];
  const isValid = (r, c) => !(r < 0 || r >= ROWS || c < 0 || c >= COLS);

  const bfs = (queue, steps) => {
    const seen = new Set();

    for (const [r, c] of queue) {
      seen.add(genKey(r, c));
    }

    while (queue.length) {
      const len = queue.length;

      for (let i = 0; i < len; i += 1) {
        const [r, c] = queue.shift();

        rooms[r][c] = Math.min(rooms[r][c], steps);

        for (const [dR, dC] of directions) {
          const newR = r + dR;
          const newC = c + dC;

          if (!isValid(newR, newC)) continue;
          if (rooms[newR][newC] !== EMPTY) continue;
          if (seen.has(genKey(newR, newC))) continue;

          seen.add(genKey(newR, newC));
          queue.push([newR, newC]);
        }
      }

      steps += 1;
    }
  };

  const queue = [];
  for (let r = 0; r < ROWS; r += 1) {
    for (let c = 0; c < COLS; c += 1) {
      if (rooms[r][c] === 0) queue.push([r, c]);
    }
  }

  bfs(queue, 0);

  return rooms;
};
```
