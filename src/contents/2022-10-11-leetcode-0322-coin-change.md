---
author: tkhwang
title: "leetcode 322. Coin Change | medium | dynamic-programming"
slug: 2022-10-11-leetcode-0322-coin-change
datetime: 2022-10-11T00:00:00Z
description: "leetcode 322. Coin Change | javascript  | medium | dynamic-programming"
tags:
  - dynamic-programming
  - medium
---

아주 유명한 전형적인 dynamic programming 문제입니다.<br />
그동안 bottom-up 에만 익숙해서 top-down 도 익숙해질 겸해서 정리해봅니다.

## 🗒️ Problems

[Coin Change - LeetCode](https://leetcode.com/problems/coin-change/)

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

## 🍀 Intuition

```javascript
// base case
dp[0][0][...] = base case

// state transfer
for (const 상태1 of 상태1_모든_데이터) {
    for (const 상태2 of 상태2_모든_데이터) {
        for (...) {
            dp[상태1][상태2][....] = 최댓값_구하기(선택1, 선택2, ...);
        }
    }
}
```

- 상태 : 특정 금액
- 선택 : 가지고 있는 동전 중에서 하나를 선택
- `dp[amount]` : 해당 금액을 가지고 있는 동전으로 교환 시에 가장 작은 동전 사용 갯수.

## ⬆️ bottom-up

`dp` array

```javascript
var coinChange = function (coins, amount) {
  const dp = Array(amount + 1).fill(Infinity);
  dp[0] = 0;

  // 특정 금액인 상태의 모든 값을 순환
  for (let i = 1; i <= amount; i += 1) {
    // 주어진 특정한 상태 (amount)에서 가능한 모든 선택에 대해서 값 계산
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

그냥 top-down 으로 풀이 시 반복되는 recursive 특성 때문에 TLE 발생합니다.

## ⬇️ 🔥 top-down with memoization

- top-down solution 에 `cache` object 를 이용한 memoization 적용.

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

## Reference

- [알라딘: 코딩 인터뷰를 위한 알고리즘 치트시트](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=301923855&start=slayer)
