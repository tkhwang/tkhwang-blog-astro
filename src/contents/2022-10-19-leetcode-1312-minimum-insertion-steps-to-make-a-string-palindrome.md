---
author: tkhwang
title: "leetcode 1312. Minimum Insertion Steps to Make a String Palindrome | hard | dynamic-programming"
slug: 2022-10-19-leetcode-1312-minimum-insertion-steps-to-make-a-string-palindrome
datetime: 2022-10-19T00:00:00Z
description: "leetcode 1312. Minimum Insertion Steps to Make a String Palindrome | javascript | hard | dynamic-programming"
tags:
  - hard
  - dynamic-programming
---

## ποΈ Problems

[Minimum Insertion Steps to Make a String Palindrome - LeetCode](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)

```
Given a string s. In one step you can insert any character at any index of the string.

Return the minimum number of steps to make s palindrome.

A Palindrome String is one that reads the same backward as well as forward.
```

```
Input: s = "zzazz"
Output: 0
Explanation: The string "zzazz" is already palindrome we do not need any insertions.
```

## β¨ Idea

- `dp[i][j]` : `s[i,...,j]` substring μ palindrome λ§λ€κΈ° μν΄μ νμν μ΅μ insertion νμ.
  - μ΅μ’ λ΅ : `dp[0][N-1]`
- basecase : i, jκ° λμΌν κ²½μ°μΈ `dp[i][j]` μ κ²½μ°μλ λ¨μΌ μΊλ¦­ν°μ΄λ―λ‘ ν­μ λμΌνλ―λ‘ insertion νμλ 0.
- μν μ μ΄ λ°©μ μ: `s[i]` μ `s[j]` λΉκ΅νμ¬
  - λμΌν  λ : λ§¨ μΈλΆκ° λμΌνλ―λ‘ λ΄λΆ `dp[i+1][j-1]` λ³κ²½νμμ λμΌν¨.
  - λ€λ₯Ό λ : νμͺ½λ§ ν¬ν¨ν κ²½μ°μμ ν λ² insertion ν κ² λΉκ΅ν΄μ minimum

## π Intuition

### DP : κ²°μ , μν, μν μ μ΄ λ°©μ μ

- κ²°μ  : i,j string κ°μ μΌμΉ μ λ¬΄
- μν : `dp[i][j]` λ `s[i,...,j]` μ insertionμ ν΅ν palindrome λ§λ€ μ μλ μ΅μκ°.
- μν μ μ΄ λ°©μ μ

```javascript
if (s[i] === s[j]) {
  dp[i][j] = dp[i + 1][j - 1];
} else {
  dp[i][j] = Math.min(1 + dp[i + 1][j], 1 + dp[i][j - 1]);
}
```

### πΈοΈ π‘ dynamic programming μμ μν λ°©ν₯μ κ²°μ μ μ΄λ»κ² ?

Dynamic programming λ¬Έμ λ₯Ό λ³΄λ©΄ μ΄λ¨λλ `0 -> N` μΌλ‘ μνλ₯Ό νκ³ , μ΄λ¨λλ `N ->0` μΌλ‘ μννλ λ± λ€μνμλλ° μλ λ΄μ©μ μκ² λμ΄ μ λ¦¬ν΄λ΄λλ€.

μν μ μ΄ λ°©μ μμ΄ κ³μ°μ΄ λκΈ° μν΄μλ λ€μμ λ§μ‘±ν΄μΌ νλ€.

- μμ λ¬Έμ  `dp[i][j]` κ³μ°μ μνμ¬ μ΄λ―Έ κ³μ°λ νμ λ¬Έμ μ κ²°κ³Όλ₯Ό μ°Έμ‘°νκ² λ©λλ€.
  - μ΄μ  κ° `dp[i-1][j-1]` μ μ°Έμ‘°νκΈ°λ νκ³ 
  - palindromeμ κ²½μ°μλ λ΄λΆ string μ κ²°κ³Όλ₯Ό μ°Έμ‘°ν΄μΌνλ―λ‘ `dp[i+1][j-1]` κ°μ μ°Έμ‘°ν΄μΌν¨.
- μν μ μ΄ λ°©μ μ κ³μ° λ§¨ λ§μ§λ§μ μ΅μ’ κ²°κ³Όκ° λμΆλμ΄ ν¨.
  - μνμ μ μμ λ°λΌμ μ΅μ’ κ°μ΄ λ¬λΌμ§.
  - μ΄λ€ κ²½μ°μλ `dp[N-1][N-1]`
  - μ΄λ² κ²½μ°μλ `string[0, N-1]` μ²μλΆν° λκΉμ§ κ΅¬μ±λ κ²½μ°μ μ΅μ insertion νμκ° νμνλ―λ‘ μ΅μ’ κ°μ `dp[0][N-1]`

μ΄λ κ² μν μ μ΄ λ°©μ μ κ³μ° κ³Όμ μμ μ΄μ  κ°μ΄ κ³μ°μ΄ λμ΄μΌ νκ³ , κ·Έ κ³Όμ μ λ§μ§λ§μ μ΅μ’ λ΅μ΄ λμΆλλλ‘ μν λ°©ν₯μ κ²°μ  ν©λλ€.

μ΄λ² λ¬Έμ μ κ²½μ°

- basecase i,j κ° κ°μ κ²½μ°μλ 0μΌλ‘ μ΄κΈ°κ°.
- `dp[i][j]` κ³μ° μ λ€μ μΈκ°μ κ°μ΄ νμν¨.
  - `dp[i+1][j-1]`
  - `dp[i+1][j]`
  - `dp[i]][j-1]`

μ΄ κ³μ°μ μν΄μλ λ€μκ³Ό κ°μ΄ μ­λ°©ν₯μΌλ‘ μμ§μ΄λ©΄ κ°λ₯νλ€.

- row λ₯Ό `[N-2, ..., 0]`
- col μ `[row + 1, ..., N-1]`

```javascript
    for (let i = N -2; i >= 0; i -= 1) {
        for (let j = i + 1; j < N; j += 1) {
            ...
        }
    }
```

## π₯ My Solution

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var minInsertions = function (s) {
  const N = s.length;

  const dp = Array(N)
    .fill(null)
    .map(_ => Array(N).fill(0));

  for (let i = N - 2; i >= 0; i -= 1) {
    for (let j = i + 1; j < N; j += 1) {
      if (s[i] === s[j]) {
        dp[i][j] = dp[i + 1][j - 1];
      } else {
        dp[i][j] = Math.min(1 + dp[i + 1][j], 1 + dp[i][j - 1]);
      }
    }
  }

  return dp[0][N - 1];
};
```
