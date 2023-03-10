---
author: tkhwang
title: "leetcode 1770. Maximum Score from Performing Multiplication Operations | hard | dynamic-programming"
slug: 2022-10-21-leetcode-hard-1770-maximum-score-from-performing-multiplication-operations
datetime: 2022-10-21T00:00:00Z
description: "leetcode 1770. Maximum Score from Performing Multiplication Operations | javascript | hard | dynamic-programming"
tags:
  - hard
  - dynamic-programming
---

## ποΈ Problems

[Maximum Score from Performing Multiplication Operations - LeetCode](https://leetcode.com/problems/maximum-score-from-performing-multiplication-operations/)

```
You are given two 0-indexed integer arrays nums and multipliers of size n and m respectively, where n >= m.

You begin with a score of 0. You want to perform exactly m operations. On the ith operation (0-indexed) you will:

- Choose one integer x from either the start or the end of the array nums.
- Add `multipliers[i] * x` to your score.
  - Note that multipliers[0] corresponds to the first operation, multipliers[1] to the second operation, and so on.
  - Remove x from nums.

Return the maximum score after performing m operations.
```

```
Input: nums = [1,2,3], multipliers = [3,2,1]
Output: 14
Explanation: An optimal solution is as follows:
- Choose from the end, [1,2,3], adding 3 * 3 = 9 to the score.
- Choose from the end, [1,2], adding 2 * 2 = 4 to the score.
- Choose from the end, [1], adding 1 * 1 = 1 to the score.
The total score is 9 + 4 + 1 = 14.
```

## π€ First attempt

- iλ²μ§Έμμ μΌμͺ½ νΉμ μ€λ₯Έμͺ½μ μ ννκ³ , μ΄ μ νμ λ°λΌμ λ―Έλμ μν₯μ λ―ΈμΉλ―λ‘ dynamic programming μ΄λΌ μκ°ν¨.
- greedy λΌλ©΄ νμ¬ ν° μͺ½λ§μ μ ννλ©΄ λκ² μ§λ§ νμ¬ μμ κ±° μ νν μ΄νμ μμ£Ό ν° κ°μ΄ μ¨μ΄ μμ μ μμΌλ―λ‘ greedy ν μ νμ΄ λ΅μ΄ λ  μλ μμ.

## β¨ Idea

- 0λ²μ§ΈλΆν° μΌμͺ½, μ€λ₯Έμͺ½ μ νμ recursive λ‘ κ³μ°
  - μΌμͺ½ μ ν μ `nums[left] * multipliers[times] + dfs(left + 1, times + 1)`
  - μ€λ₯Έμͺ½ μ ν μ `nums[right] * multipliers[times] + dfs(left, times + 1)`
  - νμ¬ λ¨κ³μμλ recursive λ‘ κ³μ° μ΄ λ κ° μ€μμ ν° κ°μ return
- μ΄λ₯Ό `multipliers[]` λκΉμ§ κ³μ°

## π Intuition

μ΄ λ¬Έμ μμ κ°μ₯ μ΄λ €μ λ λΆλΆμ ν λ² μ νμ ν λ€μμ μ΄νμλ μ νλ κ°μ λ°°μ ν array μμ μ νμ ν΄μΌνλλ° μ΄ λΆλΆ μ²λ¦¬κ° μ΄λ €μ μ. μ νν κ°μ μ§μ΄ arrayλ₯Ό κ³μ κ°μ§κ³  κ°μΌ νλλ°... μ’μ λ°©λ²μ΄ μμμ΅λλ€.

λ€μ κ²½μ°μ `right` index λ μλ κ³μ°μ΄ κ°λ₯ν κΉμ ?

- μ£Όμ΄μ§ `nums` array μ length λ `N`.
- νμ¬ μΌμͺ½ index κ° `left`
- νμ¬ μ΄ μ ν νμ `times`

### π‘ `right` λ μλ κ³μ° κ°λ₯νκ° ?

λͺ¨λ  κ²μ μ€λ₯Έμͺ½μμλ§ μ ννλ€λ©΄

```javascript
right = N - 1 - times;
```

`times` μ€μμ λͺ λ²μ `left` μμ μ νμ νλ€λ©΄

- λ§€λ² μ€λ₯Έμͺ½μμλ§ μ ννλ€λ©΄ `times` λ§νΌ μ€μ΄λ€νλ°
- κ·Έμ€μμ `left` λ§νΌμ μΌμͺ½μμ μ νμ νμΌλ―λ‘ μ€λ₯Έμͺ½μ κ·Έλ§νΌ μ€μ΄λ€μ§ μμλ λ¨.

```javascript
right = N - 1 - (times - left);
```

## πΈοΈβ¬οΈπ₯ top-down Solution

```javascript
const N = nums.length;
const NM = multipliers.length;

const cache = {};
const genKey = (left, times) => `${left}:${times}`;

const dfs = (left, times) => {
  const key = genKey(left, times);

  if (times >= NM) return 0;
  if (cache[key] !== undefined) return cache[key];

  const right = N - 1 - (times - left);

  const leftValue = nums[left] * multipliers[times] + dfs(left + 1, times + 1);
  const rightValue = nums[right] * multipliers[times] + dfs(left, times + 1);

  return (cache[key] = Math.max(leftValue, rightValue));
};

return dfs(0, 0);
```
