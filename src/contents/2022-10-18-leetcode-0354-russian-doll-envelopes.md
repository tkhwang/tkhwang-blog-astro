---
author: tkhwang
title: "leetcode 354. Russian Doll Envelopes | hard | binary-search"
slug: 2022-10-18-leetcode-0354-russian-doll-envelopes
datetime: 2022-10-18T00:00:00Z
description: "leetcode 354. Russian Doll Envelopes | javascript | hard | binary-search"
tags:
  - hard
  - binary-search
---

## 🗒️ Problems

[Russian Doll Envelopes - LeetCode](https://leetcode.com/problems/russian-doll-envelopes/)

```
You are given a 2D array of integers envelopes where envelopes[i] = [wi, hi] represents the width and the height of an envelope.

One envelope can fit into another if and only if both the width and height of one envelope are greater than the other envelope's width and height.

Return the maximum number of envelopes you can Russian doll (i.e., put one inside the other).

Note: You cannot rotate an envelope.
```

```
Input: envelopes = [[5,4],[6,4],[6,7],[2,3]]
Output: 3
Explanation: The maximum number of envelopes you can Russian doll is 3 ([2,3] => [5,4] => [6,7]).
```

## ✨ Idea

때로는 한 번 경험을 해봐야 눈이 떠지는 문제가 있다.

- `[ [ width, height ], [], ...]` 를 잘 sorting 한다.
  - `width` 오름차순으로 정렬하고
  - 같으면 `height` 반대방향인 내림차순으로 정렬
- 이렇게 정렬하고 나면 width는 커지는 방향이므로 이 상태에서 `height` 로 LIS (Longest Increasing Subsequence)를 찾아내면 그것이 답이 된다.

## 🍀 Intuition

### 다중 조건에 의한 정렬

- `width` 오름차순
- 같으면 `height` 내림차순

```javascript
envelopes.sort((a, b) => a[0] - b[0] || b[1] - a[1]);
```

### 🍀 💡 LIS

- LIS 를 binary search 로 `nlogn` 으로 찾는 것은 library 처럼 준비해두면 좋을 듯 하다.

```javascript
const piles = [];

for (const num of array) {
  const index = bisectLeft(array, num);

  if (index < piles.length) piles[index] = num;
  else piles.push(num);
}

return piles.length;
```

## 🔥 My Solution

```javascript
/**
 * @param {number[][]} envelopes
 * @return {number}
 */
var maxEnvelopes = function (envelopes) {
  const bisectLeft = (array, target) => {
    const N = array.length;

    // [0, N)
    let left = 0;
    let right = N;

    while (left < right) {
      const mid = Math.floor((left + right) / 2);

      // => [left, mid)
      if (array[mid] === target) {
        right = mid;
        // select left space => [left, mid)
      } else if (array[mid] > target) {
        right = mid;
        // select right space => [mid + 1, right)
      } else if (array[mid] < target) {
        left = mid + 1;
      }
    }
    return left;
  };

  envelopes.sort((a, b) => a[0] - b[0] || b[1] - a[1]);

  const piles = [];

  for (const [first, last] of envelopes) {
    const index = bisectLeft(piles, last);

    if (index < piles.length) {
      piles[index] = last;
    } else {
      piles.push(last);
    }
  }

  return piles.length;
};
```
