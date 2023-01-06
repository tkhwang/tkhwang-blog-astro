---
title: "leetcode 1213. Intersection of Three Sorted Arrays | easy | binary-search"
datetime: 2023-01-06T00:00:00Z
description: "leetcode 1213. Intersection of Three Sorted Arrays | javascript  | easy | binary-search"
slug: "2023-01-06-leetcode-1213-intersection-of-three-sorted-arrays"
featured: false
draft: false
tags:
  - easy
  - binary-search
ogImage: ""
---

## 🗒️ Problems

[1213. Intersection of Three Sorted Arrays - Leetcode](https://leetcode.com/problems/intersection-of-three-sorted-arrays/)

```
Given three integer arrays arr1, arr2 and arr3 sorted in strictly increasing order, return a sorted array of only the integers that appeared in all three arrays.
```

```
Input: arr1 = [1,2,3,4,5], arr2 = [1,2,5,7,9], arr3 = [1,3,4,5,8]
Output: [1,5]
Explanation: Only 1 and 5 appeared in the three arrays.
```

## 🤔 First attempt

- What about this solution to use the frequency counter of each number ?

```javascript
var arraysIntersection = function (arr1, arr2, arr3) {
  const freq = {};

  for (const arr of [arr1, arr2, arr3]) {
    for (const num of arr) {
      freq[num] = (freq[num] || 0) + 1;
    }
  }

  const res = [];
  for (const key in freq) {
    if (freq[key] >= 3) res.push(key);
  }

  return res;
};
```

## ✨ Idea

- Is it better to use the binary search because the sorted arrays are given ?

## 🔥 My Solution

```javascript
/**
 * @param {number[]} arr1
 * @param {number[]} arr2
 * @param {number[]} arr3
 * @return {number[]}
 */
var arraysIntersection = function (arr1, arr2, arr3) {
  const bisect = (array, target) => {
    const N = array.length;

    // [left, right]
    let left = 0;
    let right = N - 1;

    while (left <= right) {
      const mid = Math.floor((left + right) / 2);

      if (array[mid] === target) return mid;
      else if (array[mid] > target) {
        right = mid - 1;
      } else if (array[mid] < target) {
        left = mid + 1;
      }
    }
    return -1;
  };

  const res = [];

  for (const num of arr1) {
    const second = bisect(arr2, num);
    const third = bisect(arr3, num);

    if (second !== -1 && third !== -1) res.push(num);
  }

  return res;
};
```
