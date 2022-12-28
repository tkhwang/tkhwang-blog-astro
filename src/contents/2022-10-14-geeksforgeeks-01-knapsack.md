---
author: tkhwang
title: "0-1 Knapsack Problem | dynamic-programming"
slug: 2022-10-14-geeksforgeeks-01-knapsack
datetime: 2022-10-14T00:00:00Z
description: "geeksforgeeks 0-1 Knapsack Problem | javascript | dynamic-programming"
featured: true
draft: false
tags:
  - dynamic-programming
  - knapsack
  - geeksforgeeks
---

One of the most famous dynamic programming problems. <br />
Use one in geeksforgeeks site, because I couldn't find the exact same problem in leetcode.

## 🗒️ Problems

[0 - 1 Knapsack Problem | Practice | GeeksforGeeks](https://practice.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1)

```
You are given weights and values of N items, put these items in a knapsack of capacity W to get the maximum total value in the knapsack. Note that we have only one quantity of each item.
In other words, given two integer arrays val[0..N-1] and wt[0..N-1] which represent values and weights associated with N items respectively. Also given an integer W which represents knapsack capacity, find out the maximum value subset of val[] such that sum of the weights of this subset is smaller than or equal to W. You cannot break an item, either pick the complete item or dont pick it (0-1 property).
```

If the weight `weight[i]` and `values[i]` of total N items are given, what is the maximum sum of those values which can be added in the total weight limit `W` knapsack bag ?

```
Input:
N = 3
W = 4
values[] = {1,2,3}
weight[] = {4,5,1}
Output: 3
```

## ✨ Idea

Not the special algorithm but find the maximum value by the full search using `DP` array.

#### choice

- choose ith item
- not choose ith item

#### state

- choosable item
- weight

#### `dp[]`

- `dp[i-th item][weight]`: for i-th items and `weight` limit case, the maximum `value` which can be added.
- final answer will be `dp[N][W]`

## ⬇️ top-down solution `dp() function`

### ⬇️ 🍀 Intuition

#### Base case

If there is no item or knapsack weight limit is less than 0, the final maximum should be 0.

```javascript
    const dp = (n, w) => {
      if (n <= 0 || w <= 0) return 0
      ...
    }

    return dp(N, W);
```

#### state transfer function

For the case n-th item, weight `w`:

- (choice1) n-th item is not included:
  - it doesn't change both weight and value, so that it is the same as the previous.

```javascript
dp(n, w) = dp(n - 1, w)
```

- (choice2) n-th item is included:
  - n-th item / weight `wt[n-1]` is chosen, the final result can be calculated like the following using the previous one and n-th item's value `val[n-1]`.

```javascript
dp(n, w) = dp(n - 1, w - wt[n - 1]) + val[n - 1]
```

#### the case which should not be considered

- If the weight of n-th item itself is larger than the weight limit `W`, we couldn't select n-th item or not.
- If so, just return the previous value.

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

The top-down solution without the memoization causes the TLE error.

```javascript
class Solution {
  knapSack(W, wt, val, N) {
    const dp = (n, w) => {
      if (n <= 0 || w <= 0) return 0;

      if (wt[n - 1] > w) {
        return dp(n - 1, w);
      } else {
        return Math.max(
          // add n-th item
          dp(n - 1, w - wt[n - 1]) + val[n - 1],
          // not add n-th item
          dp(n - 1, w)
        );
      }
    };

    return dp(N, W);
  }
}
```

### ⬇️🔥 Solution : top-down with memoization

Add `cache[]` for memoization.

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
          // not add
          dp(n - 1, w),
          // add
          dp(n - 1, w - wt[n - 1]) + val[n - 1]
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
for (const state1 of ALL_STATE1) {
    for (const stat2 of ALL_STATE2) {
        for (...) {
            dp[state1][state2][....] = calc_max(choice1, choice2, ...);
        }
    }
}
```

#### `dp[]` definition

- `dp[ith-item][weight]`
- basecase value is 0.
- `[1...N]`. Use 1-indexed array.

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
            // not add i-th item
            dp[n - 1][w],
            // add i-th item
            dp[n - 1][w - wt[n - 1]] + val[n - 1]
          );
        }
      }
    }

    return dp[N][W];
  }
}
```
