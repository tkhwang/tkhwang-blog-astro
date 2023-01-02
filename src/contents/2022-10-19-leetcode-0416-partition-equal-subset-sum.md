---
author: tkhwang
title: "leetcode 416. Partition Equal Subset Sum | medium | backtrack | dynamic-programming"
slug: 2022-10-19-leetcode-0416-partition-equal-subset-sum
datetime: 2022-10-19T00:00:00Z
description: "leetcode 416. Partition Equal Subset Sum | javascript | medium | backtrack | dynamic-programming"
tags:
  - medium
  - backtrack
  - dynamic-programming
---

가장 먼저 생각난 솔루션은 backtrack 이지만 효율이 좋지 못하네요. <br />
knapsack을 이용한 dynamic programming 방법으로 푸는 방법이 있어서 정리해보았습니다.

## 🗒️ Problems

[Partition Equal Subset Sum - LeetCode](https://leetcode.com/problems/partition-equal-subset-sum/)

```
Given a non-empty array nums containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.
```

```
Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

## 🔀 🤔 First attempt

이런 문제 스타일은 backtrack 이 편합니다.

```javascript
var canPartition = function (nums) {
  const total = nums.reduce((a, b) => a + b, 0);
  if (total % 2 === 1) return false;

  const target = total / 2;
  const N = nums.length;

  const dp = (remain, cur, index) => {
    if (remain === 0) {
      if (cur.length < N) return true;
      return false;
    }
    if (remain < 0) return false;

    let local = false;
    for (let i = index; i < N; i += 1) {
      cur.push(nums[i]);
      local = local || dp(remain - nums[i], cur, i + 1);
      cur.pop();
    }
    return local;
  };

  return dp(target, [], 0);
};
```

바로 TLE 발생합니다.
여기에 `memo` 붙일 수도 있겠지만 knapsack을 이용한 dynamic programming 방법 풀이도 가능하다고 합니다.

## ✨ Idea

- `nums[]` 합을 반이 되도록 하는 subset 이 있는가 ?
  - `nums[]` 중에서 선택해서 타켓 무게 (주어진 총합 무게 /2)를 만들 수 있는가 ?
- 01 Knapsack
  - 타켓 무게 : 주어진 무게 / 2
  - 각 물건 무게 : `nums[]`
  - `dp[n][w]` : 물건 `[1,...,n]` 이용해서 무게 w 를 꽉 채울 수 있는가 ?

## 🍀 Intuition

### basecase

무게가 0이라면 아무런 선택을 하지 않아도 목표 달성함.

```javascript
for (let n = 0; n <= N; n += 1) {
  dp[n][0] = true;
}
```

### 🕸️💡 상태 전이 방정식

현재 선택을 결정해야하는 `nums[n-1]` 하나만으로 limit 무게 `w` 보다 크다면 선택 결정을 할 수 없으므로 바로 이전 결과와 동일함.

```javascript
if (nums[n - 1] > w) {
  dp[n][w] = dp[n - 1][w];
}
```

`nums[c-1]` 선택을 하는 경우에는

- 선택하지 않고 이전에 이미 성공한 경우 `dp[n-1][w]`
- `nums[n-1]` 을 넣은 경우, `nums[n-1]` 을 넣었으므로 이 무게를 뺸 이전 결과에 따라서 결과가 결정됨. `dp[n-1][w - nums[n-1]]`

```javascript
            } else {
                dp[n][w] = dp[n-1][w] || dp[n-1][w - nums[n-1]];
            }
```

## 🔥⬆️🕸️ My Solution

```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canPartition = function (nums) {
  const total = nums.reduce((a, b) => a + b, 0);
  if (total % 2 === 1) return false;

  const W = total / 2;

  const N = nums.length;
  const dp = Array(N + 1)
    .fill(null)
    .map(_ => Array(W + 1).fill(false));

  for (let n = 0; n <= N; n += 1) {
    dp[n][0] = true;
  }

  for (let n = 1; n <= N; n += 1) {
    for (let w = 1; w <= W; w += 1) {
      if (nums[n - 1] > w) {
        dp[n][w] = dp[n - 1][w];
      } else {
        dp[n][w] = dp[n - 1][w] || dp[n - 1][w - nums[n - 1]];
      }
    }
  }

  return dp[N][W];
};
```
