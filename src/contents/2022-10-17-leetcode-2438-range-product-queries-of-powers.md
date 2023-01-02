---
author: tkhwang
title: "leetcode 2438. Range Product Queries of Powers | medium | bit-masking"
slug: 2022-10-17-leetcode-2438-range-product-queries-of-powers
datetime: 2022-10-17T00:00:00Z
description: "leetcode 2438. Range Product Queries of Powers | javascript | medium | bit-masking"
tags:
  - medium
  - bit-masking
---

## 🗒️ Problems

[(2) Range Product Queries of Powers - LeetCode](https://leetcode.com/problems/range-product-queries-of-powers/)

```
Given a positive integer n, there exists a 0-indexed array called powers, composed of the minimum number of powers of 2 that sum to n. The array is sorted in non-decreasing order, and there is only one way to form the array.

You are also given a 0-indexed 2D integer array queries, where queries[i] = [lefti, righti]. Each queries[i] represents a query where you have to find the product of all powers[j] with lefti <= j <= righti.

Return an array answers, equal in length to queries, where answers[i] is the answer to the ith query. Since the answer to the ith query may be too large, each answers[i] should be returned modulo 109 + 7.
```

```
Input: n = 15, queries = [[0,1],[2,2],[0,3]]
Output: [2,4,64]
Explanation:
For n = 15, powers = [1,2,4,8]. It can be shown that powers cannot be a smaller size.
Answer to 1st query: powers[0] * powers[1] = 1 * 2 = 2.
Answer to 2nd query: powers[2] = 4.
Answer to 3rd query: powers[0] * powers[1] * powers[2] * powers[3] = 1 * 2 * 4 * 8 = 64.
Each answer modulo 109 + 7 yields the same answer, so [2,4,64] is returned.
```

## ✨ Idea

- 주어진 숫자를 2의 지수승으로 구성해서 `powers[]` 를 만들고,
- `powers[]`를 range product query 수행한다.
  - 필요하다면 TLE 방지를 위하여 prefix product 이용 ?

## 🍀 Intuition

### 2의 지수승 숫자로 분해하기

- binary 32bit에 대햇 bit masking 해서 해당 bit가 1이면 bit masking 비교한 값 (`1 << i`)을 `powers[]`에 추가한다.

```javascript
const powers = [];

for (let i = 0; i < 32; i += 1) {
  if (n & (1 << i)) {
    powers.push(1 << i);
  }
}
```

## 🔥 My Solution

```javascript
/**
 * @param {number} n
 * @param {number[][]} queries
 * @return {number[]}
 */
var productQueries = function (n, queries) {
  const MOD = 7 + 10 ** 9;

  const powers = [];
  for (let i = 0; i < 32; i += 1) {
    if (n & (1 << i)) {
      powers.push(1 << i);
    }
  }
  const N = powers.length;

  const res = [];

  for (const [start, end] of queries) {
    let product = 1;
    for (let i = start; i <= end; i += 1) {
      product = (product * powers[i]) % MOD;
    }
    res.push(product);
  }

  return res;
};
```
