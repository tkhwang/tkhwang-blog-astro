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

## 🗒️ Problems

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

## 🤔 First attempt

- i번째 word 를 선택한 경우, 선택하지 않은 경우에 대해서 처리.
- 현재까지 추가된 word 를 character frequency map 을 가지고 다니면서 conflict 가 있는지 비교하기.

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

character 의 중복 여부 체크를 character frequency를 object 로 처리했는데... <br />
이 부분이 깔끔하지 않아서 **bitset** 이용한 방법이 더 좋은 것 같아서 아래 정리해봅니다.

## ✨ Idea

- 주어진 array index `[0, ..., N-1]` recursive 로 순회하면서
- 해당 word 를 추가하거나 추가하지 않거나 선택.
- 추가하는 경우에는 기존 추가된 character들과 conflict 가 없는지를 확인해서 처리.
- 이 character 의 conflict 처리를 위하여 bitset 이용함.

## 🍀 Intuition

### bitset

alphabet character의 중부 여부 체크를 위하여 alphabet 의 순서대로 해당 bit 에 저장하기.

```
// "ab" 의 경우

a  b  c  ...  z
0  1  2  ...  26

1  1  0  ...   0
```

### 💡 bitset 값 누적 (bitwise OR)

- 해당 `word` 에서 `ch` character 단위로 처리하면서
- 해당 ch 위치를 계산 (`ch.charCodeAt(0) - 'a'.charCodeAt(0)`) 해서
- 앞의 ch와 conflict 가 없으면 bitwise OR 해서 값을 누적하고 최종 return 함.

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

### 💡 bitset 확인 (bitwise AND)

- **character 의 conflict 비교**는 character 별로 하나씩 비교하는 것이 아니라 bitset으로 변경된 값을 bitwise and 해서 확인할 수 있음.
  - `=== 0` : conflict 없는 경우
  - `> 0` : 해당 bit 는 1 이 되므로 0 이상의 값을 갖게 됨.
- conflict 가 없는 조건 확인
  - 해당 word (`arr[index]`)의 `lookup[index]` 가 존재해야하며 (word 내에 conflict 가 있다면 lookup 자체가 존재하지 않음.)
  - 기존 bitset 과 bitwise AND 한 결과가 0 으로 conflict 가 없어야 한다.

```javascript
    const dfs = (index, currentS, currentL) => {
        ...
        // use
        if (lookup[index] !== -1 && (currentS & lookup[index]) === 0) {
            dfs(index + 1, currentS | lookup[index], currentL + arr[index].length)
        }
    }
```

## 🔥 My Solution

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
