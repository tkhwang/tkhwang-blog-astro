---
author: tkhwang
title: "leetcode 2063. Vowels of All Substrings | medium | contribution"
slug: 2022-12-05-leetcode-2063-vowels-of-all-substrings
datetime: 2022-12-05T00:00:00Z
description: "leetcode 2063. Vowels of All Substrings | javascript | medium | contributions"
tags:
  - medium
  - contribution
---

## 🗒️ Problems

[Vowels of All Substrings - LeetCode](https://leetcode.com/problems/vowels-of-all-substrings/)

```
Given a string word, return the sum of the number of vowels ('a', 'e', 'i', 'o', and 'u') in every substring of word.

A substring is a contiguous (non-empty) sequence of characters within a string.

Note: Due to the large constraints, the answer may not fit in a signed 32-bit integer. Please be careful during the calculations.
```

```
Input: word = "aba"
Output: 6
Explanation:
All possible substrings are: "a", "ab", "aba", "b", "ba", and "a".
- "b" has 0 vowels in it
- "a", "ab", "ba", and "a" have 1 vowel each
- "aba" has 2 vowels in it
Hence, the total sum of vowels = 0 + 1 + 1 + 1 + 1 + 2 = 6.
```

## ✨💡 Idea

- Sometime it can reduce the time complexity by considering its **contribution** by traversing `O(n)` instead of the nested loop.

## 🍀 Intuition

For word = "abc"

```
    aba

   [a]
    a
    ab
    aba

     [a]
      a
     ba
    aba
```

```
         vowel index
             |
    0 ....   i   ...   N -1
```

### Left `[0...i]` substring

```
i - 0 + 1
```

### Right `[i...N-1]` substring

```
N-1 -i + 1
```

### Total substring

```
(i + 1) * (N - i)
```

## 🔥 My Solution

```javascript
/**
 * @param {string} word
 * @return {number}
 */
var countVowels = function (word) {
  const N = word.length;

  const set = new Set(["a", "e", "i", "o", "u"]);

  let sum = 0;
  for (let i = 0; i < N; i += 1) {
    const cur = word[i];
    if (set.has(cur)) {
      sum += (i - 0 + 1) * (N - 1 - i + 1);
    }
  }
  return sum;
};
```
