---
author: tkhwang
title: "0-1 Knapsack Problem | dynamic-programming"
slug: 2022-10-14-geeksforgeeks-01-knapsack
datetime: 2022-10-14T00:00:00Z
description: "geeksforgeeks 0-1 Knapsack Problem | javascript | dynamic-programming"
tags:
  - dynamic-programming
  - geeksforgeeks
---

아주 유명한 dynamic programming 중의 하나. <br />
leetcode 에 딱 이 기본 문제에 해당하는 문제가 없는 듯 해서 geeksforgeeks 사이트를 이용함.

## 🗒️ Problems

[0 - 1 Knapsack Problem | Practice | GeeksforGeeks](https://practice.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1)

You are given weights and values of N items, put these items in a knapsack of capacity W to get the maximum total value in the knapsack. Note that we have only one quantity of each item.
In other words, given two integer arrays val[0..N-1] and wt[0..N-1] which represent values and weights associated with N items respectively. Also given an integer W which represents knapsack capacity, find out the maximum value subset of val[] such that sum of the weights of this subset is smaller than or equal to W. You cannot break an item, either pick the complete item or dont pick it (0-1 property).

각각 무게와 값어치가 주어진 N개의 물건이 있을 때 무게 W까지 넣을 수 있는 배낭에 이 물건들을 골라서 넣을 때 값어치의 최대값은 ?

```
Input:
N = 3
W = 4
values[] = {1,2,3}
weight[] = {4,5,1}
Output: 3
```

## ✨ Idea

- 특별한 algorithm이 아니라 DP 이용해서 전체 탐색을 해서 최대값 구함.
  - DP는 전체 탐색 시 시간 효율성을 위하여 좀더 빠른 실행을 위하여 `memo[]` 혹은 `dp[]` 공간을 사용하여 기존 결과값을 저장하여 필요 시 다시 계산하지 않고, 값을 리턴함.
- `선택`
  - i번째 물건을 선택하는 경우
  - i번째 물건을 선택하지 않는 경우
- `상태`
  - 선택 가능한 물건
  - 배낭 무게
- `dp[]`
  - `dp[선택가능한_물건][배낭무게]`
  - 최종 답은 `dp[N][W]`

## ⬇️ top-down solution `dp() 함수`

### ⬇️ 🍀 Intuition

#### Base case

선택할 수 있는 물건이 하나도 없거나 배낭 무게 재한이 0 보다 작은 경우에는 선택을 할 수 없으므로 값어치도 0가 됨.

```javascript
    const dp = (n, w) => {
      if (n <= 0 || w <= 0) return 0
      ...
```

#### 기본 state transfer function

n번째 물건까지 선택 가능, 무게 w 인 경우에

- n번째 물건이 포함되지 않은 경우 : 무게와 값어치 모두 기여하지 못하므로 이전과 동일함.

```javascript
dp(n, w) = dp(n - 1, w)
```

- n번째 물건이 포함된 경우: 무게 `wt[n-1]` 와 값어치 `vt[n-1]` 기여했으므로 이만큼 삭제함.

```javascript
dp(n, w) = dp(n - 1, w - wt[n - 1]) + val[n - 1]
```

#### 고려하면 안 되는 경우

n번째 물건을 넣을 지 ? 넣지 않을지 ? 선택을 해야하는 상황이므로 n번째 물건의 무게 (`wt[n-1]`) 자체가 전체 무게 `w` 보다 큰 경우라면 고려할 수 없 수 없으므로 이전 상태로 return 한다.

```javascript
    const dp = (n, w) => {
      if (n <= 0 || w <= 0) return 0

      if (wt[n - 1] > w) {
        return dp(n - 1, w)
      } else {
        ...
      }
    }
}
```

### ⬇️ top-down w/o memoization

memoization 없는 top-down solution 이라서 TLE 발생합니다.

```javascript
class Solution {
  knapSack(W, wt, val, N) {
    const dp = (n, w) => {
      if (n <= 0 || w <= 0) return 0;

      if (wt[n - 1] > w) {
        return dp(n - 1, w);
      } else {
        return Math.max(
          // n번째 물건 포함
          dp(n - 1, w - wt[n - 1]) + val[n - 1],
          // n번째 물건 포함하지 않음
          dp(n - 1, w)
        );
      }
    };

    return dp(N, W);
  }
}
```

### ⬇️🔥 Solution : top-down with memoization

cache 추가

```javascript
class Solution {
  knapSack(W, wt, val, N) {
    const cache = {};
    const genKey = (n, w) => `${n}:${w}`;

    const dp = (n, w) => {
      const key = genKey(n, w);

      if (n <= 0 || w <= 0) return 0;
      if (cache[key] !== undefined) return cache[key];

      if (wt[n - 1] > w) {
        return dp(n - 1, w);
      } else {
        cache[key] = Math.max(
          // n번째 물건 포함
          dp(n - 1, w - wt[n - 1]) + val[n - 1],
          // n번째 물건 포함하지 않음
          dp(n - 1, w)
        );
        return cache[key];
      }
    };

    return dp(N, W);
  }
}
```

## ⬆️ bottom-up solution `dp[] array`

### ⬆️ 🍀 Intuition

#### bottom up DP frame

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

#### `dp[]` 정의

- `dp[선택할_수_있는_물건][배낭무게]`
- 각각 `0`인 경우는 base case 로 0 값을 갖고, array index `[0, 1, ... N]` 까지 사용하므로 1-indexed array 사용.

```javascript
const dp = Array(N + 1)
  .fill(null)
  .map(_ => Array(W + 1).fill(0));
```

### ⬆️ 🔥 Solution : bottom-up

```javascript
class Solution {
  knapSack(W, wt, val, N) {
    const dp = Array(N + 1)
      .fill(null)
      .map(_ => Array(W + 1).fill(0));

    for (let n = 1; n <= N; n += 1) {
      for (let w = 1; w <= W; w += 1) {
        if (wt[n - 1] > w) {
          dp[n][w] = dp[n - 1][w];
        } else {
          dp[n][w] = Math.max(
            // n 번째 포함하지 않는 경우
            dp[n - 1][w],
            // n 번째 포함한 경우
            dp[n - 1][w - wt[n - 1]] + val[n - 1]
          );
        }
      }
    }

    return dp[N][W];
  }
}
```
