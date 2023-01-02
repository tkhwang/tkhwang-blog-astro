---
author: tkhwang
title: "leetcode 115. Distinct Subsequences | medium | recursive"
slug: 2022-11-16-leetcode-115-distinct-subsequences
datetime: 2022-11-16T00:00:00Z
description: "leetcode 115. Distinct Subsequences | javascript | medium | recursive"
tags:
  - medium
  - recursive
---

## 🗒️ Problems

[Distinct Subsequences - LeetCode](https://leetcode.com/problems/distinct-subsequences/)

```
Given two strings s and t, return the number of distinct subsequences of s which equals t.

The test cases are generated so that the answer fits on a 32-bit signed integer.
```

```
Input: s = "rabbbit", t = "rabbit"
Output: 3
Explanation:
As shown below, there are 3 ways you can generate "rabbit" from s.
rabbbit
rabbbit
rabbbit
```

## 🤔 First attempt

- Sometimes bottom-up solution is easier to think.
- Sometimes top-down is better.

## ✨💡 Idea

- Handle the matching case `s[i]` and `t[j]` recursively.

## 🍀 Intuition

### ⬇️ top-down recursive function

- `recursive()` returns 1 (one case) when `t` can be found in `s`.

```javascript
const recursive = (i, j) => {};
```

### ⬇️ base case

- When we reach out the end of `t`, it means that we found one.
- But when we reach out the end of `s` first, it means that we failed to find.

```javascript
  const NS = s.length
  const NT = t.length

  const recursive = (i, j) => {
      if (i === NS || j === NT) {
          return j === NT
    }
```

- We will sum all of these findings.

### ⬇️ When `s[i]` is not same as `t[j]`

- We failed to find `t[j]` in `s[i]`.
- So we need to match the next position in source `s[i+1]` with the previous target `t[j]`.

```javascript
let res = 0;
res += recursive(i + 1, j);
```

### ⬇️ When `s[i]` is same as `t[j]`

- Because we found target `t[j]`, so that we can find the matching in next position of both of source and target.
- And it's also OK to find the previous target `t[j]` again in the next source position `s[i+1]`.

```javascript
  let res = 0
  res += recursive(i + 1, j)
  if (s[i] === t[j]) {
    res += recursive(i + 1, j + 1)
  }
}
```

## ⬇️🔥 My Solution

- Add the memoization for better performance.

```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {number}
 */
var numDistinct = function (s, t) {
  const NS = s.length;
  const NT = t.length;

  const cache = {};
  const genKey = (i, j) => `${i}:${j}`;

  const recursive = (i, j) => {
    const key = genKey(i, j);

    if (i === NS || j === NT) {
      return j === NT;
    }
    if (cache[key] !== undefined) return cache[key];

    let res = 0;
    res += recursive(i + 1, j);

    if (s[i] === t[j]) {
      res += recursive(i + 1, j + 1);
    }
    return (cache[key] = res);
  };

  return recursive(0, 0);
};
```
