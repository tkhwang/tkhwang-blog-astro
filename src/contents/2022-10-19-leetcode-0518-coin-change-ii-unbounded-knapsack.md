---
author: tkhwang
title: "leetcode 518. Coin Change II | medium | dynamic-programming"
slug: 2022-10-19-leetcode-0518-coin-change-ii-unbounded-knapsack
datetime: 2022-10-19T00:00:00Z
description: "leetcode 518. Coin Change II | javascript | medium | dynamic-programming"
tags:
  - medium
  - dynamic-programming
  - knapsack
---

Unlikely 01 kanpsack problem, this kind problem is called as `unbounded kanpsack` because the number of items is unlimited.

## 🗒️ Problems

[Coin Change II - LeetCode](https://leetcode.com/problems/coin-change-ii/)

```
You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0.

You may assume that you have an infinite number of each kind of coin.

The answer is guaranteed to fit into a signed 32-bit integer.
```

```
Input: amount = 5, coins = [1,2,5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

## ✨ Idea

| 01 Knapsack                                | Unbounded knapsack (Coin change II) |
| ------------------------------------------ | ----------------------------------- |
| knapsack total weight `W`                  | total money `amount`                |
| `N` items                                  | `N` coins                           |
| item weight `weight[0...N-1]`              | coin money `coins[0...N-1]`         |
| item value `value[0...N-1]`                | number of coins: unlimited          |
| maximum value in weight limit `W` knapsack | number of coins which amount is `W` |

## 🍀🕸️ dynamic programming concept

#### Choice

- select i-th coin
- or not

#### State

- coin type `N`
- money `w`

##### DP

- `dp[c][w]` : the number of used coin of coin `coins[0...c-1]` for amount `w`
- final answer will be `dp[C][W]`

#### basecase

- `dp[0][*] = 0` : if none of coins is used, the number of used coins is zero.
- `dp[*][0] = 1` : the number of used coins for making total money 0 is only one - not to use none of coins.

### 🕸️💡 State transfer equation

- If the money amount of c-th coin `coins[c-1]` itself is larger than the money amount limit `w`, we couldn't consider this case, so that the number of used coins is same as that of previous one.

```javascript
if (w < coins[c]) {
  dp[c][w] = dp[c - 1][w];
}
```

- If c-th coin can be selectable, the possible cases are following:
  1. `dp[c-1][w]` : the number of used coin of using `c-1` coins for amount `w`.
  2. `dp[c][w - coins[c-1]]`
  - For example, you want to make `money 2` coin for `amount 5`. If you know possible coin numbers for `amount 3`, you can just add possible number of making `amount 2` with coin `money 2` on top of the previous result.

```javascript
} else {
  dp[c][w] = dp[c - 1][w] + dp[c][w - coins[c - 1]];
}
```

## 🕸️ ⬆️ 🔥 My Solution

```javascript
/**
 * @param {number} amount
 * @param {number[]} coins
 * @return {number}
 */
var change = function (amount, coins) {
  const N = coins.length;
  const W = amount;

  const dp = Array(N + 1)
    .fill(null)
    .map(_ => Array(W + 1).fill(0));

  for (let c = 1; c <= N; c += 1) {
    dp[c][0] = 1;
  }

  for (let c = 1; c <= N; c += 1) {
    for (let w = 1; w <= W; w += 1) {
      if (w < coins[c - 1]) {
        dp[c][w] = dp[c - 1][w];
      } else {
        dp[c][w] = dp[c - 1][w] + dp[c][w - coins[c - 1]];
      }
    }
  }

  return dp[N][W];
};
```
