---
author: tkhwang
title: "leetcode 112. Integer to Roman | medium"
slug: 2022-10-20-leetcode-0012-integer-to-roman
datetime: 2022-10-20T00:00:00Z
description: "leetcode 12. Integer to Roman | javascript | medium"
tags:
  - medium
---

## 🗒️ Problems

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

## 🤔 First attempt

처음에는 recursive 로 돌면서 10 자릿수로 나누어 가면서 처리하려고 했었는데... <br />
뭔가 깔끔하게 잘 되지 않았다...

## ✨ Idea

아주 general solution은 아니더라도 좀더 간편하게 할 수 있는 방법은 없을까 ?

- 각 자리수마다 `number2roman = { }` 같은 mapping table 을 있다면 자리수마다 변환을 해주자.

## 🍀 Intuition

`num` 이 주어졌다고 할 때, 각 자리수 값은 ?

### 1의 자리수

```javascript
num % 10;
```

### 10의 자리수

```javascript
Math.floor((num % 100) / 10);
```

### 100의 자리수

```javascript
Math.floor((num % 1000) / 100);
```

## 🔥 My Solution

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
