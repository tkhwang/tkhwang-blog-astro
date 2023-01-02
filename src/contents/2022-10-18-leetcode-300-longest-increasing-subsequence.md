---
author: tkhwang
title: "leetcode 300. Longest Increasing Subsequence | medium | dynamic-programming | binary-search"
slug: 2022-10-18-leetcode-300-longest-increasing-subsequence
datetime: 2022-10-18T00:00:00Z
description: "leetcode 300. Longest Increasing Subsequence | javascript | medium | dynamic-programming | binary-search"
tags:
  - medium
  - dynamic-programming
  - binary-search
---

## 🗒️ Problem

[Longest Increasing Subsequence - LeetCode](https://leetcode.com/problems/longest-increasing-subsequence/)

```
Given an integer array nums, return the length of the longest strictly increasing subsequence.

A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, [3,6,2,7] is a subsequence of the array [0,3,1,6,2,2,7].
```

```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

## ⬆️ 전형적인 DP 풀이 방법: `O(n^2)`

전형적으로는 dynamic programming을 이용해서 풀 수 있다.
이 경우 시간 복잡도는 `n^2` 이 된다.

- `dp[i]` 의미 : `nums[i]`에서 끝나는 LIS 길이

### ⬆️ 🍀 Intuition

- `nums[j] < nums[i]` 인 경우에 이전 (`dp[j]`)보다 하나 더 큰 increasing sequence 찾음.

```javascript
for (let i = 0; i < n; i += 1) {
  for (let j = 0; j < i; j += 1) {
    if (nums[j] < nums[i]) dp[i] = Math.max(dp[i], dp[j] + 1);
  }
}
```

### ⬆️ 🔥 bottom solution

```javascript
var lengthOfLIS = function (nums) {
  const n = nums.length;

  const dp = Array(n).fill(1);

  for (let i = 0; i < n; i += 1) {
    for (let j = 0; j < i; j += 1) {
      if (nums[j] < nums[i]) dp[i] = Math.max(dp[i], dp[j] + 1);
    }
  }
  return Math.max(...dp);
};
```

## 🔎 Binary search 이용하여 `n log(n)` 가능 ?

[알라딘: 코딩 인터뷰를 위한 알고리즘 치트시트](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=301923855) 책을 보면서 보게된 내용인데, 신기해서 정리해봅니다. 자세한 내용은 아래 링크에서 확인할 수 있습니다.

- [원문 링크](https://labuladong.github.io/algo/3/26/76/)
- [한글번역 링크](https://papago.naver.net/website?locale=ko&source=zh-CN&target=ko&url=https%3A%2F%2Flabuladong.github.io%2Falgo%2F3%2F26%2F76%2F)

### 🔎 🍀 Intuition : binary search

![img](https://raw.githubusercontent.com/tkhwang/tkhwang-etc/master/img/2022/10/poker1.jpeg)

![img](https://raw.githubusercontent.com/tkhwang/tkhwang-etc/master/img/2022/10/poker3.jpeg)

![img](https://raw.githubusercontent.com/tkhwang/tkhwang-etc/master/img/2022/10/poker4.jpeg)

- 카드 게임 룰과 동일.
  - 카드 윗줄에서 왼쪽부터 아래에 내려놓는데, 놓여진 카드 위에는 아래 카드보다 값이 작은 경우에만 위로 올려 놓을 수 있음.
  - 윗줄에서 선택된 카드가 아랫줄에 놓여져 있는 카드보다 크다면 (-> 여기서 **increasing sequence 가 생성**됨) 아래줄 오른쪽에 새로 생성.
  - 윗줄에서 선택한 카드 값이 너무 작아서 아랫줄에 여러 줄에 놓을 수 있다면 가장 왼쪽에 놓음.
- 최종 아랫줄에 생긴 컬럼 수가 LIS 길이가 됨.

### 🔎 ✨ Algorithm

- 아래 줄을 나타내는 array 를 만들고
- 윗줄 맨 왼쪽부터 카드를 하나 선택해서 아래 줄 어디에 놓아야 하는지를 판단한다.
  - 이때 binary search left를 이용해서 선택된 카드의 insertion point를 찾는다.
  - 더 큰 카드가 있는 경우 (insertion point 가 < length) 해당 위치의 카드 위에 놓고 (해당 index 의 값을 overwrite)
  - 더 큰 카드가 없어서 insertion point 가 `length` 인 경우에는 array size를 늘려서 맨 뒤에 넣는다.
  - ...
- 최종 array의 length 가 LIS 가 된다.

### 🔎 🔥 Solution: binary search

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var lengthOfLIS = function (nums) {
  const bisectLeft = (array, target) => {
    const N = array.length;

    let left = 0;
    let right = N;

    while (left < right) {
      const mid = Math.floor((left + right) / 2);

      if (array[mid] === target) {
        right = mid;
      } else if (array[mid] > target) {
        right = mid;
      } else if (array[mid] < target) {
        left = mid + 1;
      }
    }
    return left;
  };

  const N = nums.length;
  const piles = [];

  for (const num of nums) {
    const index = bisectLeft(piles, num);

    if (index === piles.length) piles.push(num);
    else piles[index] = num;
  }

  return piles.length;
};
```
