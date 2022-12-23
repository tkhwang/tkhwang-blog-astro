---
title: "leetcode 1851. Minimum Interval to Include Each Query | hard | heap"
datetime: 2023-01-05T00:00:00Z
description: "leetcode 1851. Minimum Interval to Include Each Query | javascript  | hard | heap"
slug: "2023-01-05-leetcode-1851-minimum-interval-to-include-each-query"
featured: false
draft: false
tags:
  - hard
  - heap
ogImage: ""
---

## 🗒️ Problems

[Minimum Interval to Include Each Query - LeetCode](https://leetcode.com/problems/minimum-interval-to-include-each-query/)

```
You are given a 2D integer array intervals, where intervals[i] = [lefti, righti] describes the ith interval starting at lefti and ending at righti (inclusive). The size of an interval is defined as the number of integers it contains, or more formally righti - lefti + 1.

You are also given an integer array queries. The answer to the jth query is the size of the smallest interval i such that lefti <= queries[j] <= righti. If no such interval exists, the answer is -1.

Return an array containing the answers to the queries.
```

```
Input: intervals = [[1,4],[2,4],[3,6],[4,4]], queries = [2,3,4,5]
Output: [3,3,1,4]
Explanation: The queries are processed as follows:
- Query = 2: The interval [2,4] is the smallest interval containing 2. The answer is 4 - 2 + 1 = 3.
- Query = 3: The interval [2,4] is the smallest interval containing 3. The answer is 4 - 2 + 1 = 3.
- Query = 4: The interval [4,4] is the smallest interval containing 4. The answer is 4 - 4 + 1 = 1.
- Query = 5: The interval [3,6] is the smallest interval containing 5. The answer is 6 - 3 + 1 = 4.
```

## 🤔 First attempt

- Calculate the length of the interval and update every point in the interval to the minimum interval length.
- Finally choose the smallest interval length of query.

```javascript
var minInterval = function (intervals, queries) {
  const obj = {};

  for (const [start, end] of intervals) {
    for (let i = start; i <= end; i += 1) {
      if (obj[i] === undefined) obj[i] = Infinity;
      obj[i] = Math.min(obj[i], end - start + 1);
    }
  }

  const res = [];
  for (const query of queries) {
    res.push(obj[query] || -1);
  }

  return res;
};
```

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
