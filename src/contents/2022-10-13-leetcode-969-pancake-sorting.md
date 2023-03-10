---
author: tkhwang
title: "leetcode 969. Pancake Sorting | medium | greedy"
slug: 2022-10-13-leetcode-969-pancake-sorting
datetime: 2022-10-13T00:00:00Z
description: "leetcode 969. Pancake Sorting | javascript | medium | greedy"
tags:
  - medium
  - greedy
---

## ποΈ Problems

[Pancake Sorting - LeetCode](https://leetcode.com/problems/pancake-sorting/)

```
Given an array of integers arr, sort the array by performing a series of pancake flips.
In one pancake flip we do the following steps:

- Choose an integer k where 1 <= k <= arr.length.
- Reverse the sub-array arr[0...k-1] (0-indexed).

For example, if arr = [3,2,1,4] and we performed a pancake flip choosing k = 3, we reverse the sub-array [3,2,1], so arr = [1,2,3,4] after the pancake flip at k = 3.

Return an array of the k-values corresponding to a sequence of pancake flips that sort arr. Any valid answer that sorts the array within 10 \* arr.length flips will be judged as correct.
```

```
Input: arr = [3,2,4,1]
Output: [4,2,4,3]
Explanation:
We perform 4 pancake flips, with k values 4, 2, 4, and 3.
Starting state: arr = [3, 2, 4, 1]
After 1st flip (k = 4): arr = [1, 4, 2, 3]
After 2nd flip (k = 2): arr = [4, 1, 2, 3]
After 3rd flip (k = 4): arr = [3, 2, 1, 4]
After 4th flip (k = 3): arr = [1, 2, 3, 4], which is sorted.
```

## β¨ Idea

### π‘ greedy solution

μ΄λ° μ’λ₯μ greedy νκ² λ‘μ§μ μκ°νλ λ¬Έμ  μ€νμΌμ΄ μ΄λ €μ΄ κ² κ°μ΅λλ€. <br />
μ§μ  μμλΈ κ²μ μλκ³ , λ΅ λ³΄κ³  νμΈν λ΄μ©μ΄μ§λ§ μ λ¦¬ν΄λ΄λλ€.

- (n) κ° μ€μμ κ°μ₯ ν° κ²μ μ°Ύμμ μ΄κ²μ κ°μ₯ μλμͺ½μΌλ‘ λ€μ§λλ€.
  - n κ° μ€μμ κ°μ₯ ν° κ°μ κ°λ index `maxIndex` λ₯Ό μ°Ύλλ€.
  - index `[0...maxIndex]` λ₯Ό λ€μ§μ΄μ array `[maxValue, ... ]` μ€λλ‘ ν λ€μμ
  - μ΄λ₯Ό λ€μ index `[0, ... , n-1]` λ€μ§μμ array `[..., maxValue]` κ°μ₯ ν° κ°μ΄ λ§¨ μλ μ€λλ‘ νλ€.
- λ§¨ μλ 1κ°λ ν° κ°μ μ°ΎμμΌλ κ·Έ μμ (n-1)κ°μ step1 μ λ°λ³΅νλ€.

## π₯ My Solution

μ΄ greedy solutionμ΄ μ΅μ μ solutionμ μλ μλ μλ λ― νμ§λ§ medium μ΄λΌμ κ·Έλ°μ§ λ΅μ΄ λ§μΌλ©΄ accept λ ν΄μ£Όλ λ― νλ€μ.

```javascript
/**
 * @param {number[]} arr
 * @return {number[]}
 */
var pancakeSort = function (arr) {
  const N = arr.length;
  const res = [];

  const revert = (array, i, j) => {
    while (i < j) {
      [array[i], array[j]] = [array[j], array[i]];
      i += 1;
      j -= 1;
    }
  };

  const sort = (array, n) => {
    if (n === 1) return;

    // find max i
    let max = -Infinity;
    let maxIndex;
    for (let i = 0; i < n; i += 1) {
      if (max < array[i]) {
        max = array[i];
        maxIndex = i;
      }
    }

    // revert [0...i]
    revert(array, 0, maxIndex);
    res.push(maxIndex + 1);

    // revert [0...N-1]
    revert(array, 0, n - 1);
    res.push(n);

    sort(array, n - 1);
  };

  sort(arr, N);

  return res;
};
```

### Bill Gates κ° μλ£¨μμ ??

discussion λκΈμ λ³΄λκΉ Bill Gates κ° μ΄ λ¬Έμ μ upper bound μΈ `O((5n + 5)/3)` ν΄λ²μ μμλλ€λ μ΄μΌκΈ°κ° μλ€μ. Bill Gatesλ algorithmλ μ νμλ λ³΄κ΅°μ.. :)

```
Bill Gates is known for giving an upper bound of O((5n + 5)/3) for this problem
where the flipped window may not necessarily start from beginning index as in this problem.
```

Read more about it here: https://en.wikipedia.org/wiki/Pancake_sorting
