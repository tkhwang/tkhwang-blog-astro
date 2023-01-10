---
title: "leetcode 121. Best Time to Buy and Sell Stock  | easy | greedy"
datetime: 2023-01-10T00:00:00Z
description: "leetcode 121. Best Time to Buy and Sell Stock | javascript  | easy | greedy"
slug: "2023-01-10-leetcode-121-best-time-to-buy-and-sell-stock"
featured: false
draft: false
tags:
  - easy
  - greedy
  - best-time-to-buy-and-sell-stock
ogImage: ""
---

## 🗒️ Problems

[121. Best Time to Buy and Sell Stock - Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

```
You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.
```

```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
```

#### Constraints:

```
1 <= prices.length <= 10^5
0 <= prices[i] <= 10^4
```

## 🤔 First attempt

- Buy in `i` day, and then sell in `j` day.

```javascript
var maxProfit = function (prices) {
  const N = prices.length;
  let max = -Infinity;

  for (let i = 0; i < N; i += 1) {
    for (let j = i + 1; j < N; j += 1) {
      const profit = prices[j] - prices[i];
      if (max < profit) max = profit;
    }
  }

  return max > 0 ? max : 0;
};
```

### 🙅‍♂️ Failed time complexity : `O(n^2)`

- Input size is `10^5`, so that the nested loop will cause TLE.

## ✨ Idea

- When does the maximum profit occur ?
  - Buy the lowest price.
  - Sell the highest price.
- Can you do this in `O(n)` ?

## 🍀 Update min price

```javascript
    let min = Infinity;

    for (const [ i, price ] of prices.entries()) {
        if (min > price) {
            min = price;
        } else {
            ...
        }
    }
```

## 🍀 Update max profit

```javascript
var maxProfit = function (prices) {
  let max = -Infinity;

  for (const [i, price] of prices.entries()) {
    if (min > price) {
        ...
    } else {
      const profit = price - min;
      max = Math.max(max, profit);
    }
  }
};
```

## 🔥 My Solution

```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function (prices) {
  const N = prices.length;

  let min = Infinity;
  let max = -Infinity;
  for (const [i, price] of prices.entries()) {
    if (min > price) {
      min = price;
    } else {
      const profit = price - min;
      max = Math.max(max, profit);
    }
  }

  return max === -Infinity ? 0 : max;
};
```

### 🙆‍♂️ Time complexity: `O(n)`
