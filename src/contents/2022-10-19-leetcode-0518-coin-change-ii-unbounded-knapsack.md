---
title: "leetcode 518. Coin Change II | medium | dynamic-programming"
datetime: 2022-10-19T00:00:00Z
description: "leetcode 518. Coin Change II | javascript | medium | dynamic-programming"
tags:
  - dynamic-programming
  - medium
---

이 문제가 01 knapsack 과 달리 물건의 갯수가 무한한 경우일때 특별히 `unbounded knapsack` 문제라고 해서 한다고 합니다. 이에 knapsack DP 형식으로 정리해봅니다.

## 🗒️ Problems

[Coin Change II - LeetCode](https://leetcode.com/problems/coin-change-ii/)

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0.

You may assume that you have an infinite number of each kind of coin.

The answer is guaranteed to fit into a signed 32-bit integer.

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

| 01 Knapsack                             | Coin change II                         |
| --------------------------------------- | -------------------------------------- |
| 배낭 총 무게 W                          | 총 금액 `amount`                       |
| 물건 종류 N                             | 코인 종류 N                            |
| 물건 무게 `wt[0,...,N-1]`               | 코인 금액 `coins[0,...,N-1]`           |
| 물건 가치 `vat[0,...,N-1]`              | 코인 갯수 무제한                       |
| 무게W 배낭에 넣을 수 있는 최대 가치는 ? | 금액 W 만들 수 있는 코인의 종류 수는 ? |

## 🍀 Intuition

### 🕸️ dynamic programming 기본 정리

- 선택 : i번째 코인을 선택하기 or 하지 않기
- 상태 :
  - 코인 종류 `N`
  - 금액 `w`
- `dp[]` : `dp[c][w]` 코인 `[1,...,c]` 사용해서 금액 `w` 만들 수 있는 코인 종류 가짓수.
- 최종 값 : `dp[C+1][W+1]`
- basecase
  - `dp[0][*] = 0` : 코인을 하나도 사용하지 않으면 코인 종류 가짓수는 언제나 0.
  - `dp[*][0] = 1` : 총 금액 0을 만들 수 있는 코인 종류 가짓수는 해당 코인 전부 사용하지 않는 방법 1.

### 🕸️ 💡 상태 전이 방정식

- c번째 현재 코인 `coins[c-1]` 의 무게가 총 금액보다 큰 경우에는 c번째 코인 선택을 할 수 없으므로 이전과 동일.
- c번째 코인 선택 가능한 경우 만들 수 있는 코인 종류의 갯수는 다음 두 가지의 합
  - `(c-1)` 번째 코인 선택해서 만들 수 있는 방법 : `dp[c-1][w]`
  - `c` 번째 코인 선택 : `dp[c][w - coins[c-1]]`
  - 금액2 `coins[c-1]` 인 동전으로 5 `w` 를 만들려면 3 `w - coins[c-1]`을 만드는 방법을 알고, 이에 2를 더해 5를 만들 수 있음.

```javascript
if (w < coins[c]) {
  dp[c][w] = dp[c - 1][w];
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
