---
author: tkhwang
title: "leetcode 300. Longest Increasing Subsequence | medium | dynamic-programming | binary-search"
slug: 2022-10-18-leetcode-300-longest-increasing-subsequence
datetime: 2022-10-18T00:00:00Z
description: "leetcode 300. Longest Increasing Subsequence | javascript | medium | dynamic-programming | binary-search"
tags:
  - medium
  - dynamic-programming
  - binary-search
---

## ๐๏ธ Problem

[Longest Increasing Subsequence - LeetCode](https://leetcode.com/problems/longest-increasing-subsequence/)

```
Given an integer array nums, return the length of the longest strictly increasing subsequence.

A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, [3,6,2,7] is a subsequence of the array [0,3,1,6,2,2,7].
```

```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

## โฌ๏ธ ์ ํ์ ์ธ DP ํ์ด ๋ฐฉ๋ฒ: `O(n^2)`

์ ํ์ ์ผ๋ก๋ dynamic programming์ ์ด์ฉํด์ ํ ์ ์๋ค.
์ด ๊ฒฝ์ฐ ์๊ฐ ๋ณต์ก๋๋ `n^2` ์ด ๋๋ค.

- `dp[i]` ์๋ฏธ : `nums[i]`์์ ๋๋๋ LIS ๊ธธ์ด

### โฌ๏ธ ๐ Intuition

- `nums[j] < nums[i]` ์ธ ๊ฒฝ์ฐ์ ์ด์  (`dp[j]`)๋ณด๋ค ํ๋ ๋ ํฐ increasing sequence ์ฐพ์.

```javascript
for (let i = 0; i < n; i += 1) {
  for (let j = 0; j < i; j += 1) {
    if (nums[j] < nums[i]) dp[i] = Math.max(dp[i], dp[j] + 1);
  }
}
```

### โฌ๏ธ ๐ฅ bottom solution

```javascript
var lengthOfLIS = function (nums) {
  const n = nums.length;

  const dp = Array(n).fill(1);

  for (let i = 0; i < n; i += 1) {
    for (let j = 0; j < i; j += 1) {
      if (nums[j] < nums[i]) dp[i] = Math.max(dp[i], dp[j] + 1);
    }
  }
  return Math.max(...dp);
};
```

## ๐ Binary search ์ด์ฉํ์ฌ `n log(n)` ๊ฐ๋ฅ ?

[์๋ผ๋: ์ฝ๋ฉ ์ธํฐ๋ทฐ๋ฅผ ์ํ ์๊ณ ๋ฆฌ์ฆ ์นํธ์ํธ](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=301923855) ์ฑ์ ๋ณด๋ฉด์ ๋ณด๊ฒ๋ ๋ด์ฉ์ธ๋ฐ, ์ ๊ธฐํด์ ์ ๋ฆฌํด๋ด๋๋ค. ์์ธํ ๋ด์ฉ์ ์๋ ๋งํฌ์์ ํ์ธํ  ์ ์์ต๋๋ค.

- [์๋ฌธ ๋งํฌ](https://labuladong.github.io/algo/3/26/76/)
- [ํ๊ธ๋ฒ์ญ ๋งํฌ](https://papago.naver.net/website?locale=ko&source=zh-CN&target=ko&url=https%3A%2F%2Flabuladong.github.io%2Falgo%2F3%2F26%2F76%2F)

### ๐ ๐ Intuition : binary search

![img](https://raw.githubusercontent.com/tkhwang/tkhwang-etc/master/img/2022/10/poker1.jpeg)

![img](https://raw.githubusercontent.com/tkhwang/tkhwang-etc/master/img/2022/10/poker3.jpeg)

![img](https://raw.githubusercontent.com/tkhwang/tkhwang-etc/master/img/2022/10/poker4.jpeg)

- ์นด๋ ๊ฒ์ ๋ฃฐ๊ณผ ๋์ผ.
  - ์นด๋ ์์ค์์ ์ผ์ชฝ๋ถํฐ ์๋์ ๋ด๋ ค๋๋๋ฐ, ๋์ฌ์ง ์นด๋ ์์๋ ์๋ ์นด๋๋ณด๋ค ๊ฐ์ด ์์ ๊ฒฝ์ฐ์๋ง ์๋ก ์ฌ๋ ค ๋์ ์ ์์.
  - ์์ค์์ ์ ํ๋ ์นด๋๊ฐ ์๋ซ์ค์ ๋์ฌ์ ธ ์๋ ์นด๋๋ณด๋ค ํฌ๋ค๋ฉด (-> ์ฌ๊ธฐ์ **increasing sequence ๊ฐ ์์ฑ**๋จ) ์๋์ค ์ค๋ฅธ์ชฝ์ ์๋ก ์์ฑ.
  - ์์ค์์ ์ ํํ ์นด๋ ๊ฐ์ด ๋๋ฌด ์์์ ์๋ซ์ค์ ์ฌ๋ฌ ์ค์ ๋์ ์ ์๋ค๋ฉด ๊ฐ์ฅ ์ผ์ชฝ์ ๋์.
- ์ต์ข ์๋ซ์ค์ ์๊ธด ์ปฌ๋ผ ์๊ฐ LIS ๊ธธ์ด๊ฐ ๋จ.

### ๐ โจ Algorithm

- ์๋ ์ค์ ๋ํ๋ด๋ array ๋ฅผ ๋ง๋ค๊ณ 
- ์์ค ๋งจ ์ผ์ชฝ๋ถํฐ ์นด๋๋ฅผ ํ๋ ์ ํํด์ ์๋ ์ค ์ด๋์ ๋์์ผ ํ๋์ง๋ฅผ ํ๋จํ๋ค.
  - ์ด๋ binary search left๋ฅผ ์ด์ฉํด์ ์ ํ๋ ์นด๋์ insertion point๋ฅผ ์ฐพ๋๋ค.
  - ๋ ํฐ ์นด๋๊ฐ ์๋ ๊ฒฝ์ฐ (insertion point ๊ฐ < length) ํด๋น ์์น์ ์นด๋ ์์ ๋๊ณ  (ํด๋น index ์ ๊ฐ์ overwrite)
  - ๋ ํฐ ์นด๋๊ฐ ์์ด์ insertion point ๊ฐ `length` ์ธ ๊ฒฝ์ฐ์๋ array size๋ฅผ ๋๋ ค์ ๋งจ ๋ค์ ๋ฃ๋๋ค.
  - ...
- ์ต์ข array์ length ๊ฐ LIS ๊ฐ ๋๋ค.

### ๐ ๐ฅ Solution: binary search

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var lengthOfLIS = function (nums) {
  const bisectLeft = (array, target) => {
    const N = array.length;

    let left = 0;
    let right = N;

    while (left < right) {
      const mid = Math.floor((left + right) / 2);

      if (array[mid] === target) {
        right = mid;
      } else if (array[mid] > target) {
        right = mid;
      } else if (array[mid] < target) {
        left = mid + 1;
      }
    }
    return left;
  };

  const N = nums.length;
  const piles = [];

  for (const num of nums) {
    const index = bisectLeft(piles, num);

    if (index === piles.length) piles.push(num);
    else piles[index] = num;
  }

  return piles.length;
};
```
