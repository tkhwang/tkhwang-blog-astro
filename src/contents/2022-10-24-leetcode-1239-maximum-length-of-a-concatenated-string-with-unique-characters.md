---
author: tkhwang
title: "leetcode 1239. Maximum Length of a Concatenated String with Unique Characters | medium | bitset"
slug: 2022-10-24-leetcode-1239-maximum-length-of-a-concatenated-string-with-unique-characters
datetime: 2022-10-24T00:00:00Z
description: "leetcode 1239. Maximum Length of a Concatenated String with Unique Characters | javascript | medium | bitset"
tags:
  - medium
  - bitset
---

## ποΈ Problems

[Maximum Length of a Concatenated String with Unique Characters - LeetCode](https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/)

```
You are given an array of strings arr. A string s is formed by the concatenation of a subsequence of arr that has unique characters.

Return the maximum possible length of s.

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.
```

```
Input: arr = ["un","iq","ue"]
Output: 4
Explanation: All the valid concatenations are:
- ""
- "un"
- "iq"
- "ue"
- "uniq" ("un" + "iq")
- "ique" ("iq" + "ue")
Maximum length is 4.
```

## π€ First attempt

- iλ²μ§Έ word λ₯Ό μ νν κ²½μ°, μ ννμ§ μμ κ²½μ°μ λν΄μ μ²λ¦¬.
- νμ¬κΉμ§ μΆκ°λ word λ₯Ό character frequency map μ κ°μ§κ³  λ€λλ©΄μ conflict κ° μλμ§ λΉκ΅νκΈ°.

```javascript
var maxLength = function (arr) {
  const N = arr.length;
  let max = -Infinity;

  const dfs = (index, cur, set) => {
    if (index >= N) {
      if (max < cur.length) max = cur.length;
      return;
    }

    // not add
    dfs(index + 1, cur, set);

    // add
    const localSet = new Set();
    for (const ch of arr[index]) {
      if (localSet.has(ch)) return;
      localSet.add(ch);
    }
    for (const ch of arr[index]) {
      if (set.has(ch)) return;
    }

    for (const ch of arr[index]) {
      set.add(ch);
    }
    dfs(index + 1, cur + arr[index], set);

    for (const ch of arr[index]) {
      set.delete(ch);
    }
  };

  dfs(0, "", new Set());

  return max;
};
```

character μ μ€λ³΅ μ¬λΆ μ²΄ν¬λ₯Ό character frequencyλ₯Ό object λ‘ μ²λ¦¬νλλ°... <br />
μ΄ λΆλΆμ΄ κΉλνμ§ μμμ **bitset** μ΄μ©ν λ°©λ²μ΄ λ μ’μ κ² κ°μμ μλ μ λ¦¬ν΄λ΄λλ€.

## β¨ Idea

- μ£Όμ΄μ§ array index `[0, ..., N-1]` recursive λ‘ μννλ©΄μ
- ν΄λΉ word λ₯Ό μΆκ°νκ±°λ μΆκ°νμ§ μκ±°λ μ ν.
- μΆκ°νλ κ²½μ°μλ κΈ°μ‘΄ μΆκ°λ characterλ€κ³Ό conflict κ° μλμ§λ₯Ό νμΈν΄μ μ²λ¦¬.
- μ΄ character μ conflict μ²λ¦¬λ₯Ό μνμ¬ bitset μ΄μ©ν¨.

## π Intuition

### bitset

alphabet characterμ μ€λΆ μ¬λΆ μ²΄ν¬λ₯Ό μνμ¬ alphabet μ μμλλ‘ ν΄λΉ bit μ μ μ₯νκΈ°.

```
// "ab" μ κ²½μ°

a  b  c  ...  z
0  1  2  ...  26

1  1  0  ...   0
```

### π‘ bitset κ° λμ  (bitwise OR)

- ν΄λΉ `word` μμ `ch` character λ¨μλ‘ μ²λ¦¬νλ©΄μ
- ν΄λΉ ch μμΉλ₯Ό κ³μ° (`ch.charCodeAt(0) - 'a'.charCodeAt(0)`) ν΄μ
- μμ chμ conflict κ° μμΌλ©΄ bitwise OR ν΄μ κ°μ λμ νκ³  μ΅μ’ return ν¨.

```javascript
const getSet = word => {
  let b = 0;
  for (const ch of word) {
    const d = ch.charCodeAt(0) - "a".charCodeAt(0);
    if ((b & (1 << d)) > 0) return -1;

    b |= 1 << d;
  }
  return b;
};
```

### π‘ bitset νμΈ (bitwise AND)

- **character μ conflict λΉκ΅**λ character λ³λ‘ νλμ© λΉκ΅νλ κ²μ΄ μλλΌ bitsetμΌλ‘ λ³κ²½λ κ°μ bitwise and ν΄μ νμΈν  μ μμ.
  - `=== 0` : conflict μλ κ²½μ°
  - `> 0` : ν΄λΉ bit λ 1 μ΄ λλ―λ‘ 0 μ΄μμ κ°μ κ°κ² λ¨.
- conflict κ° μλ μ‘°κ±΄ νμΈ
  - ν΄λΉ word (`arr[index]`)μ `lookup[index]` κ° μ‘΄μ¬ν΄μΌνλ©° (word λ΄μ conflict κ° μλ€λ©΄ lookup μμ²΄κ° μ‘΄μ¬νμ§ μμ.)
  - κΈ°μ‘΄ bitset κ³Ό bitwise AND ν κ²°κ³Όκ° 0 μΌλ‘ conflict κ° μμ΄μΌ νλ€.

```javascript
    const dfs = (index, currentS, currentL) => {
        ...
        // use
        if (lookup[index] !== -1 && (currentS & lookup[index]) === 0) {
            dfs(index + 1, currentS | lookup[index], currentL + arr[index].length)
        }
    }
```

## π₯ My Solution

```javascript
/**
 * @param {string[]} arr
 * @return {number}
 */
var maxLength = function (arr) {
  const N = arr.length;

  const lookup = [];

  const getSet = word => {
    let b = 0;
    for (const ch of word) {
      const d = ch.charCodeAt(0) - "a".charCodeAt(0);
      if ((b & (1 << d)) > 0) return -1;

      b |= 1 << d;
    }
    return b;
  };

  let max = -Infinity;

  const dfs = (index, currentS, currentL) => {
    if (index >= N) {
      if (max < currentL) max = currentL;
      return;
    }

    // not use
    dfs(index + 1, currentS, currentL);

    // use
    if (lookup[index] !== -1 && (currentS & lookup[index]) === 0) {
      dfs(index + 1, currentS | lookup[index], currentL + arr[index].length);
    }
  };

  for (const word of arr) {
    lookup.push(getSet(word));
  }

  dfs(0, 0, 0);

  return max;
};
```
