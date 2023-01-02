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

## 🗒️ Problems

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

## ✨ Idea

### 💡 greedy solution

이런 종류의 greedy 하게 로직을 생각하는 문제 스타일이 어려운 것 같습니다. <br />
직접 알아낸 것은 아니고, 답 보고 확인한 내용이지만 정리해봅니다.

- (n) 개 중에서 가장 큰 것을 찾아서 이것을 가장 아래쪽으로 뒤집는다.
  - n 개 중에서 가장 큰 값을 갖는 index `maxIndex` 를 찾는다.
  - index `[0...maxIndex]` 를 뒤집어서 array `[maxValue, ... ]` 오도록 한 다음에
  - 이를 다시 index `[0, ... , n-1]` 뒤집에서 array `[..., maxValue]` 가장 큰 값이 맨 아래 오도록 한다.
- 맨 아래 1개는 큰 값을 찾았으니 그 앞에 (n-1)개에 step1 을 반복한다.

## 🔥 My Solution

이 greedy solution이 최적의 solution은 아닐 수도 있는 듯 하지만 medium 이라서 그런지 답이 맞으면 accept 는 해주는 듯 하네요.

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

### Bill Gates 가 솔루션을 ??

discussion 댓글에 보니깐 Bill Gates 가 이 문제의 upper bound 인 `O((5n + 5)/3)` 해법을 알아냈다는 이야기가 있네요. Bill Gates는 algorithm도 잘 했었나 보군요.. :)

```
Bill Gates is known for giving an upper bound of O((5n + 5)/3) for this problem
where the flipped window may not necessarily start from beginning index as in this problem.
```

Read more about it here: https://en.wikipedia.org/wiki/Pancake_sorting
