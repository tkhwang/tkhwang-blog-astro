---
author: tkhwang
title: "leetcode 322. Coin Change | medium | dynamic-programming"
slug: 2022-10-11-leetcode-0322-coin-change
datetime: 2022-10-11T00:00:00Z
description: "leetcode 322. Coin Change | javascript  | medium | dynamic-programming"
tags:
  - medium
  - dynamic-programming
featured: true
draft: false
---

One of the famous dynamic programming problems.

## 🗒️ Problems

[Coin Change - LeetCode](https://leetcode.com/problems/coin-change/)

```
You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.
```

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

## 🍀 Intuition

```
// base case
dp[0][0][...] = base case

// state transfer
for (const state1 of ALL_STATE1) {
    for (const state2 of ALL_STATE2) {
        for (...) {
            dp[state1][state2][....] = calc_max(choice1, choice2, ...);
        }
    }
}
```

- state : money amount
- choice : select one coin
- `dp[amount]` : the minimum number of coins with that amount of money

## ⬆️ bottom-up

`dp` array

```javascript
var coinChange = function (coins, amount) {
  const dp = Array(amount + 1).fill(Infinity);
  dp[0] = 0;

  // for all money
  for (let i = 1; i <= amount; i += 1) {
    // for the specific amount of money, calculate all possible choices
    for (const coin of coins) {
      if (i - coin < 0) continue;

      dp[i] = Math.min(dp[i], 1 + dp[i - coin]);
    }
  }

  return dp[amount] === Infinity ? -1 : dp[amount];
};
```

## ⬇️ top-down

`dp` function

```javascript
var coinChange = function (coins, amount) {
  const dp = n => {
    if (n === 0) return 0;
    if (n < 0) return -1;

    let counts = Infinity;
    for (const coin of coins) {
      const subProblem = dp(n - coin);
      if (subProblem === -1) continue;

      counts = Math.min(counts, 1 + subProblem);
    }
    return counts === Infinity ? -1 : counts;
  };

  return dp(amount);
};
```

The top-down solution without memoization causes the TLE error.

## ⬇️ 🔥 top-down with memoization

- top-down solution with `cache` object for memoization.

```javascript
var coinChange = function (coins, amount) {
  const cache = {};

  const dp = n => {
    if (n === 0) return 0;
    if (n < 0) return -1;
    if (cache[n] !== undefined) return cache[n];

    let counts = Infinity;
    for (const coin of coins) {
      const subProblem = dp(n - coin);
      if (subProblem === -1) continue;

      counts = Math.min(counts, 1 + subProblem);
    }
    counts = counts === Infinity ? -1 : counts;
    return (cache[n] = counts);
  };

  return dp(amount);
};
```
