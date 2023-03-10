---
author: tkhwang
title: "leetcode 112. Integer to Roman | medium"
slug: 2022-10-20-leetcode-0012-integer-to-roman
datetime: 2022-10-20T00:00:00Z
description: "leetcode 12. Integer to Roman | javascript | medium"
tags:
  - medium
---

## ποΈ Problems

[(1) Integer to Roman - LeetCode](https://leetcode.com/problems/integer-to-roman/)

```
Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000

For example, 2 is written as II in Roman numeral, just two one's added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

- I can be placed before V (5) and X (10) to make 4 and 9.
- X can be placed before L (50) and C (100) to make 40 and 90.
- C can be placed before D (500) and M (1000) to make 400 and 900.

Given an integer, convert it to a roman numeral.
```

```
Input: num = 58
Output: "LVIII"
Explanation: L = 50, V = 5, III = 3.
```

## π€ First attempt

μ²μμλ recursive λ‘ λλ©΄μ 10 μλ¦Ώμλ‘ λλμ΄ κ°λ©΄μ μ²λ¦¬νλ €κ³  νμλλ°... <br />
λ­κ° κΉλνκ² μ λμ§ μμλ€...

## β¨ Idea

μμ£Ό general solutionμ μλλλΌλ μ’λ κ°νΈνκ² ν  μ μλ λ°©λ²μ μμκΉ ?

- κ° μλ¦¬μλ§λ€ `number2roman = { }` κ°μ mapping table μ μλ€λ©΄ μλ¦¬μλ§λ€ λ³νμ ν΄μ£Όμ.

## π Intuition

`num` μ΄ μ£Όμ΄μ‘λ€κ³  ν  λ, κ° μλ¦¬μ κ°μ ?

### 1μ μλ¦¬μ

```javascript
num % 10;
```

### 10μ μλ¦¬μ

```javascript
Math.floor((num % 100) / 10);
```

### 100μ μλ¦¬μ

```javascript
Math.floor((num % 1000) / 100);
```

## π₯ My Solution

```javascript
/**
 * @param {number} num
 * @return {string}
 */
var intToRoman = function (num) {
  const M2Roman = ["", "M", "MM", "MMM"];
  const C2Roman = ["", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"];
  const X2Roman = ["", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"];
  const I2Roman = ["", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"];

  return (
    M2Roman[Math.floor(num / 1000)] +
    C2Roman[Math.floor((num % 1000) / 100)] +
    X2Roman[Math.floor((num % 100) / 10)] +
    I2Roman[num % 10]
  );
};
```
