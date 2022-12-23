---
title: "leetcode 204. Count Primes | medium | prime-number"
datetime: 2023-01-01T00:00:00Z
description: "leetcode 204. Count Primes | javascript  | medium | prime-number"
slug: "2023-01-01-leetcode-204-count-primes"
featured: false
draft: false
tags:
  - medium
  - prime-number
ogImage: ""
---

## 🗒️ Problems

[Count Primes - LeetCode](https://leetcode.com/problems/count-primes/)

```
Given an integer n, return the number of prime numbers that are strictly less than n.
```

```
Input: n = 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```

What is the most efficient way to calculate the prime number ?

## 🤔 First attempt

naive solution : There are so many repeated calculation.

- Is the given number divided by one of number from 2 to `n-1 ?
  - If so, it's a prime number.
  - If not, it's a prime number.

```javascript
var countPrimes = function (N) {
  let count = 0;

  const isPrime = num => {
    for (let i = 2; i < num; i += 1) {
      if (num % i === 0) return false;
    }
    return true;
  };

  for (let i = 2; i < N; i += 1) {
    if (isPrime(i)) count += 1;
  }

  return count;
};
```

## ✨ Idea

- The number from 2 to `N-1`
- If the number can divide the given target number, it's a prime number.
- But the multiples of this prime number is not a prime number.

## 🍀 Sieve of Eratosthenes

1. Create a list of consecutive integers from 2 through n: (2, 3, 4, ..., n).
2. Let p be the variable we use in the outer loop that iterates from 2 to `\sqrt{n}`. Initially, let p equal 2, the smallest prime number.
3. Enumerate the multiples of p by counting in increments of p from `p*p` to `n`, and mark them in the list (these will be `p*p`, `p*p + p`, `p*p + 2\*p`, ...; `p` itself should be prime).
4. Find the smallest number in the list greater than p that is not marked. If there was no such number, stop. Otherwise, let p now equal this new number (which is the next prime), and repeat from step 3.
5. When the algorithm terminates, all of the remaining numbers that are not marked are prime.

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
/**
 * @param {number} n
 * @return {number}
 */
var countPrimes = function (N) {
  const seen = Array(N + 1).fill(false);
  let count = 0;

  for (let i = 2; i < N; i += 1) {
    if (seen[i]) continue;

    count += 1;

    for (let j = i * i; j < N; j += i) {
      seen[j] = true;
    }
  }

  return count;
};
```
