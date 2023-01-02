---
author: tkhwang
title: "leetcode 1523. Count Odd Numbers in an Interval Range | easy | math"
slug: 2022-12-13-leetcode-1523-count-odd-numbers-in-an-interval-range
datetime: 2022-12-13T00:00:00Z
update: 2022-12-13
description: "leetcode 1523. Count Odd Numbers in an Interval Range | javascript | easy | math"
tags:
  - easy
  - math
---

## 🗒️ Problems

[Count Odd Numbers in an Interval Range - LeetCode](https://leetcode.com/problems/count-odd-numbers-in-an-interval-range/)

```
Given two non-negative integers low and high. Return the count of odd numbers between low and high (inclusive).
```

```
Input: low = 3, high = 7
Output: 3
Explanation: The odd numbers between 3 and 7 are [3,5,7].
```

## 🤔 First attempt

I thought that the answer depends on the given interval length.

- `odd` :
- `even` :
  - start num is `odd`
  - start num is `even`

```javascript
var countOdds = function (low, high) {
  const len = high - low + 1;

  if (len % 2 === 0) return Math.floor(len / 2);

  return low % 2 === 0 ? Math.floor(len / 2) : Math.floor(len / 2) + 1;
};
```

Hmm... not good enough.

## ✨ Idea

- Is it possible to make it simple, which doesn't depend of various combination of `even` and `odd` numbers ?

## 🍀 Intuition

- `[1...num]` : Math.floor((num + 1)/2)

```
1     2     3      4      5      6      7      8      9
                  num               => 1,3
                         num        => 1,3,5
                                num => 1,3,5
```

## 💡 calculate like prefixSum

```
1     2     3      4      5      6      7      8      9
                 [low           high]
[1       low-1]
[1                              high]
```

```
[low ... high] =
  [1...high]      Math.floor((high+1)/2)
  -
  [1...low-1]     Math.floor(low/2)
```

## 🔥 My Solution

```javascript
/**
 * @param {number} low
 * @param {number} high
 * @return {number}
 */
var countOdds = function (low, high) {
  return Math.floor((high + 1) / 2) - Math.floor(low / 2);
};
```
