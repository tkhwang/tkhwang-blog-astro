---
author: tkhwang
title: "leetcode 2444. Count Subarrays With Fixed Bounds | hard | sliding-window"
slug: 2022-10-17-leetcode-2444-count-subarrays-with-fixed-bounds
datetime: 2022-10-17T00:00:00Z
description: "leetcode 2444. Count Subarrays With Fixed Bounds | javascript | hard | sliding-window"
tags:
  - hard
  - sliding-window
---

## 🗒️ Problems

[Count Subarrays With Fixed Bounds - LeetCode](https://leetcode.com/problems/count-subarrays-with-fixed-bounds/)

```
You are given an integer array nums and two integers minK and maxK.
A fixed-bound subarray of nums is a subarray that satisfies the following conditions:

- The minimum value in the subarray is equal to minK.
- The maximum value in the subarray is equal to maxK.

Return the number of fixed-bound subarrays.
A subarray is a contiguous part of an array.
```

```
Input: nums = [1,3,5,2,7,5], minK = 1, maxK = 5
Output: 2
Explanation: The fixed-bound subarrays are [1,3,5] and [1,3,5,2].
```

문제 자체는 이해하기 어렵지 않습니다. <br/>
주어진 min, max 값을 갖는 sub-array 의 총 갯수 구하기.

## 🤔 First thought

조건을 만족하는 sub-array를 **sliding window** 로 관리하면서 되지 않을까 ?

- 전형적인 sliding window 문제로서 sliding window 이용하자.
- window가 min/max 값을 유지해야하는데, 이 경우 shrink window 는 언제 하지 ?
- window는 찾았다고 하면 sub-array 갯수는 어떻게 찾지 ?

## ✨ Idea

- sliding window 이용해서 `right` 오른쪽 순회하면서 조건에 맞는 sub array 갯수를 추가한다.
- sub-array의 min값은 `minK`, max값은 `maxK` 가 되어야 하므로 `right` 포인터 순회하면서 이 값들을 찾았는지 flag 로 설정한다.

```
// left  ...   minCurIndex ... maxCurIndex... right
// left  ...   maxCurIndex ... minCurIndex... right
```

- 위와 같은 경우에 sub-array 갯수를 구해서 추가한다.

## 🍀 Intuition

### Sliding window

기본 template

```javascript
let res = 0;

let left = 0;
for (let right = 0; right < n; right += 1) {
  // do logic here to add arr[right] to curr

  while (WINDOW_CONDITION_BROKEN) {
    // remove top left from window
    left += 1;
  }

  // update ans
}

return res;
```

### 💡 `right` 순회하면서 min/max 값을 갖는 index 찾자

- min/max 특성 때문에 window 안의 값이 min/max 를 넘어서지 않으면 window 는 계속 확인됨.
- min/max 찾았다고 해서 바로 window 가 닫거나 할 수 없고, 이후에 계속 늘어날 수도 있고, window는 커질 수 있지만 조건에 맞는 min/max 위치는 못 찾을 수도 있다.
  - 진행하면서 조건에 맞는 경우를 저장하자.(`minCurIndex`, `maxCurIndex`)
  - 두 가지 조건이 다 만족한 경우에만 sub-array 내부 갯수 세기 위하여 flag 설정을 하자 (`minFound`, `maxFound`).

```javascript
  let minFound = false
  let maxFound = false
  let minCurIndex = -1
  let maxCurIndex = -1

  let left = 0
  for (let right = 0; right < N; right += 1) {
    const cur = nums[right]

    if (cur === minK) {
      minFound = true
      minCurIndex = right
    }
    if (cur === maxK) {
      maxFound = true
      maxCurIndex = right
    }
```

### 💡 shrink window

- `right` 포인터 값이 주어진 최소/최대를 넘어서면 더 이상 `left` 로부터 시작하는 sub-array 가 valid 하지 않음.
- 현재 위치 다음부터 새로 window 시작할 수 있도록 설정.

```javascript
  let left = 0
  for (let right = 0; right < N; right += 1) {
    const cur = nums[right]

    if (cur < minK || cur > maxK) {
      left = right + 1

      minFound = false
      maxFound = false
      minCurIndex = -1
      maxCurIndex = -1
    }
```

### 💡 앞에서 찾은 포인터로부터 sub-array 계산

```javascript
// left  ...   minCurIndex ... maxCurIndex... right
// left  ...   maxCurIndex ... minCurIndex... right
```

- 현재 포인터의 위치는 위와 같다.

```javascript
// left  ...   minCurIndex ... maxCurIndex... right
// left  ...   maxCurIndex ... minCurIndex... right
                   |<----------->|
```

- 위의 구간은 min/max 값을 구성하는 필수 sub-array 구간이 됨.
  - 이 부분은 하나로 모두 포함이 되어야 함.
  - 이 구간 사이에서는 sub-array로 나뉠 수 없음.

### 🍀 🚀 조건 만족하는 sub-array 갯수는 ?

```javascript
// left  ...   minCurIndex ... maxCurIndex... right
// left  ...   maxCurIndex ... minCurIndex... right
    o  o  o  o  o  |<----------->|
                   [ 필  수  구  간 ]
    [ 선  택 구 간]
                |<-------------->|
             |<----------------->|
          |<-------------------->|
       |<----------------------->|
    |<-------------------------->|
```

- `left` 와 `Math.min(minCurIndex, maxCurIndex)` 사이의 포인트로부터 새로운 sub-array가 시작할 수 있으므로 이 사이의 포인터의 갯수가 sub-array의 갯수가 된다.

```javascript
if (minFound && maxFound) {
  res += Math.min(minCurIndex, maxCurIndex) - left + 1;
}
```

## 🔥 My Solution

```javascript
/**
 * @param {number[]} nums
 * @param {number} minK
 * @param {number} maxK
 * @return {number}
 */
var countSubarrays = function (nums, minK, maxK) {
  const N = nums.length;

  let minFound = false;
  let maxFound = false;
  let minCurIndex = -1;
  let maxCurIndex = -1;

  let res = 0;

  let left = 0;
  for (let right = 0; right < N; right += 1) {
    const cur = nums[right];

    if (cur === minK) {
      minFound = true;
      minCurIndex = right;
    }
    if (cur === maxK) {
      maxFound = true;
      maxCurIndex = right;
    }

    if (cur < minK || cur > maxK) {
      left = right + 1;
      minFound = false;
      maxFound = false;
      minCurIndex = -1;
      maxCurIndex = -1;
    }

    if (minFound && maxFound) {
      res += Math.min(minCurIndex, maxCurIndex) - left + 1;
    }
  }

  return res;
};
```
