---
author: tkhwang
title: "leetcode 611. Valid Triangle Number | medium | binary-search"
slug: 2022-12-12-leetcode-611-valid-triangle-number
datetime: 2022-12-12T00:00:00Z
description: "leetcode 611. Valid Triangle Number | javascript | medium | binary-search"
tags:
  - medium
  - binary-search
---

## 🗒️ Problems

[Valid Triangle Number - LeetCode](https://leetcode.com/problems/valid-triangle-number/)

```
Given an integer array nums, return the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle.
```

```
Input: nums = [2,2,3,4]
Output: 3
Explanation: Valid combinations are:
2,3,4 (using the first 2)
2,3,4 (using the second 2)
2,2,3
```

#### Constraints

```
1 <= nums.length <= 1000
0 <= nums[i] <= 1000
```

## ✨ Idea

One of the properties of a triangle is ...

```
smallest + mid > largest
```

## 🤔 First attempt

- Usually the first solution I come up with was the backtracking.

```javascript
var triangleNumber = function (nums) {
  const N = nums.length;
  nums.sort((a, b) => a - b);

  const isTriangle = (a, b, c) => a + b > c;
  const res = [];

  const dfs = (cur, index) => {
    if (cur.length === 3) {
      const [a, b, c] = cur;
      if (isTriangle(nums[a], nums[b], nums[c])) {
        res.push([a, b, c]);
      }
    }

    for (let i = index; i < N; i += 1) {
      cur.push(i);
      dfs(cur, i + 1);
      cur.pop();
    }
  };

  dfs([], 0);

  return res.length;
};
```

- I got TLE.
  - Backtracking is usually `O(2^n)` solution.
  - The input size should be smaller than 20.
- The input size length is `1000`.
  - This means that the nested for loop can be used.

## 🍀💡 Intuition

- Two nums are selected by using the nested for loop.
- The other one should be found by using the binary search algorithm.
  - The number can be duplicate, so that let's use `bisectLeft()`.
  - The found index means that there are (index EA) small numbers, sum of which will the the answer.

## 🔥 My Solution

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var triangleNumber = function (nums) {
  const N = nums.length;
  nums.sort((a, b) => a - b);

  const bisectLeft = (array, target) => {
    const N = array.length;

    // [0, N)
    let left = 0;
    let right = N;

    while (left < right) {
      const mid = Math.floor((left + right) / 2);

      // => [left, mid)
      if (array[mid] === target) {
        right = mid;
        // select left space => [left, mid)
      } else if (array[mid] > target) {
        right = mid;
        // select right space => [mid + 1, right)
      } else if (array[mid] < target) {
        left = mid + 1;
      }
    }
    return left;
  };

  // [ a, b, c] => a + b > c
  const isTriangle = (a, b, c) => a + b > c;

  let sum = 0;

  for (let i = 0; i < N - 2; i += 1) {
    for (let j = i + 1; j < N - 1; j += 1) {
      const index = bisectLeft(nums.slice(j + 1), nums[i] + nums[j]);
      sum += index;
    }
  }
  return sum;
};
```
