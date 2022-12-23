---
title: "leetcode 46. Permutations | medium | backtrack"
datetime: 2022-12-30T00:00:00Z
description: "leetcode 46. Permutations | javascript  | medium | backtrack"
slug: "2022-12-30-leetcode-46-permutations"
featured: false
draft: false
tags:
  - medium
  - backtrack
ogImage: ""
---

## 🗒️ Problems

[Permutations - LeetCode](https://leetcode.com/problems/permutations/)

```
Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.
```

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

## ✨ Idea

- How to generate the difference sequences ?

## 🤔 First attempt

- Select the specific number from the remaining array.

```javascript
var permute = function (nums) {
  const result = [];

  const dfs = (cur, remains) => {
    if (remains.length === 0) {
      result.push(cur);
      return;
    }

    for (let i = 0; i < remains.length; i += 1) {
      dfs(
        [...cur, remains[i]],
        [...remains.slice(0, i), ...remains.slice(i + 1)]
      );
    }
  };

  dfs([], nums);

  return result;
};
```

- Select the current num which is not included before.

```javascript
var permute = function (nums) {
  const N = nums.length;
  const res = [];

  const backtrack = cur => {
    if (cur.length >= N) {
      res.push([...cur]);
      return;
    }

    for (let i = 0; i < N; i += 1) {
      if (cur.includes(nums[i])) continue;

      cur.push(nums[i]);

      backtrack(cur);

      cur.pop();
    }
  };

  backtrack([]);

  return res;
};
```

But the above solutions seem there is a lot of unnecessary copy and deletion.

## 🍀 Intuition

- Minimize the unnecessary copy and deletion of items in the given array.

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

### 💡➡️

```
index
|
0 1 2 3 4
| | | | | change one

    index
      |
0 1 2 3 4
      | |
```

## 🔥 My Solution

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permute = function (nums) {
  const N = nums.length;
  const res = [];

  const dfs = index => {
    if (index >= N) {
      res.push([...nums]);
      return;
    }

    for (let i = index; i < N; i += 1) {
      [nums[index], nums[i]] = [nums[i], nums[index]];
      dfs(index + 1);
      [nums[index], nums[i]] = [nums[i], nums[index]];
    }
  };

  dfs(0);

  return res;
};
```
