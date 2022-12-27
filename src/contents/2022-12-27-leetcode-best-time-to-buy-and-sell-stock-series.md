---
title: "leetcode Best Time to Buy and Sell Stock series"
datetime: 2022-12-27T00:00:00Z
description: "leetcode Best Time to Buy and Sell Stock series | javascript  | hard | XXX"
slug: "leetcode-best-time-to-buy-and-sell-stock-series"
featured: false
draft: false
tags:
  - medium
  - hard
ogImage: ""
---

## 🤔 First attempt

Sometimes there are some problems that I don't like to solve without the special reasons. This "Best Time to Buy and Sell Stock" problem is one of them to me.

- [121. Best Time to Buy and Sell Stock - LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
- [122. Best Time to Buy and Sell Stock II - LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
- [309. Best Time to Buy and Sell Stock with Cooldown - LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)
- [123. Best Time to Buy and Sell Stock III - LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

## 🗒️ [easy] Problems : 121

[121. Best Time to Buy and Sell Stock - LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

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

### 🍀 Constraint

- Buy one day
- Sell another day

### ✨ Idea

- Buy at min price
- Sell when profit is max.

### 🔥 [121: I] My Solution

```javascript
var maxProfit = function (prices) {
  let minPrice = Infinity;
  let maxProfit = -Infinity;

  for (const price of prices) {
    // buy : minPrice
    minPrice = Math.min(minPrice, price);
    // sell
    maxProfit = Math.max(maxProfit, price - minPrice);
  }

  return maxProfit;
};
```

## 🗒️ [medium] Problems : 309

[309. Best Time to Buy and Sell Stock with Cooldown - LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

```
You are given an array prices where prices[i] is the price of a given stock on the ith day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).
Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
```

```
Input: prices = [1,2,3,0,2]
Output: 3
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```

### 🍀 Constraint

- After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

### ✨ Idea

- XXX

### 🔥 [309] My Solution

```javascript

```

## 🗒️ [medium] Problems : 122

[122. Best Time to Buy and Sell Stock II - LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

```
You are given an integer array prices where prices[i] is the price of a given stock on the ith day.

On each day, you may decide to buy and/or sell the stock. You can only hold at most one share of the stock at any time. However, you can buy it then immediately sell it on the same day.

Find and return the maximum profit you can achieve.
```

```
Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.
```

### 🍀 Constraint

- can buy and then sell on the same day.
- but hold at most one share of the stock

### ✨ Idea

- XXX

### 🔥 [122] My Solution

```javascript

```

## 🗒️ [714] Problems : XXX

[Best Time to Buy and Sell Stock with Transaction Fee - LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

```

```

```

```

### 🍀 Constraint

- XXX

### ✨ Idea

- XXX

### 🔥 [122: I] My Solution

```javascript

```

## 🗒️ [hard] Problems : III

[123 Best Time to Buy and Sell Stock III - LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

```
You are given an array prices where prices[i] is the price of a given stock on the ith day.

Find the maximum profit you can achieve. You may complete at most two transactions.

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
```

```
Input: prices = [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
```

### 🍀 Constraint

- You may complete at most two transactions.
- You may not engage in multiple transactions simultaneously
  - You must sell the stock before you buy again.

### ✨ Idea

- XXX

### 🔥 [122: I] My Solution

```javascript

```

## 🗒️ [hard] Problems : IV

[188. Best Time to Buy and Sell Stock IV - LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)

```
You are given an integer array prices where prices[i] is the price of a given stock on the ith day, and an integer k.

Find the maximum profit you can achieve. You may complete at most k transactions.

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
```

```
Input: k = 2, prices = [2,4,1]
Output: 2
Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
```

### 🍀 Constraint

- You may complete at most k transactions.
- You may not engage in multiple transactions simultaneously
  - you must sell the stock before you buy again.

### ✨ Idea

- XXX

### 🔥 [122: I] My Solution

```javascript

```

## 🗒️ [] Problems : XXX

```

```

```

```

### 🍀 Constraint

- XXX

### ✨ Idea

- XXX

### 🔥 [122: I] My Solution

```javascript

```

## 🍀 Intuition

### ⬇️ top-down

### ⬆️ bottom-up

### 🔀 backtrack

### 🕸️ dynamic programming

### 🔗 Linked list

### 📏 Binary search

## 🪜 Steps

## 🌲 tree

## 🥞 stack

## ♻️ recursive

## ➡️ iterative

### 💡

## 🔥 My Solution

```javascript
var maxProfit = function (prices) {
  let minPrice = Infinity;
  let maxProfit = -Infinity;

  for (const price of prices) {
    // buy : minPrice
    minPrice = Math.min(minPrice, price);
    // sell
    maxProfit = Math.max(maxProfit, price - minPrice);
  }

  return maxProfit;
};
```
