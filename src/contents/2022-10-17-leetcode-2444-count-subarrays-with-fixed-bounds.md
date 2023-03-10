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

## ๐๏ธ Problems

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

๋ฌธ์  ์์ฒด๋ ์ดํดํ๊ธฐ ์ด๋ ต์ง ์์ต๋๋ค. <br/>
์ฃผ์ด์ง min, max ๊ฐ์ ๊ฐ๋ sub-array ์ ์ด ๊ฐฏ์ ๊ตฌํ๊ธฐ.

## ๐ค First thought

์กฐ๊ฑด์ ๋ง์กฑํ๋ sub-array๋ฅผ **sliding window** ๋ก ๊ด๋ฆฌํ๋ฉด์ ๋์ง ์์๊น ?

- ์ ํ์ ์ธ sliding window ๋ฌธ์ ๋ก์ sliding window ์ด์ฉํ์.
- window๊ฐ min/max ๊ฐ์ ์ ์งํด์ผํ๋๋ฐ, ์ด ๊ฒฝ์ฐ shrink window ๋ ์ธ์  ํ์ง ?
- window๋ ์ฐพ์๋ค๊ณ  ํ๋ฉด sub-array ๊ฐฏ์๋ ์ด๋ป๊ฒ ์ฐพ์ง ?

## โจ Idea

- sliding window ์ด์ฉํด์ `right` ์ค๋ฅธ์ชฝ ์ํํ๋ฉด์ ์กฐ๊ฑด์ ๋ง๋ sub array ๊ฐฏ์๋ฅผ ์ถ๊ฐํ๋ค.
- sub-array์ min๊ฐ์ `minK`, max๊ฐ์ `maxK` ๊ฐ ๋์ด์ผ ํ๋ฏ๋ก `right` ํฌ์ธํฐ ์ํํ๋ฉด์ ์ด ๊ฐ๋ค์ ์ฐพ์๋์ง flag ๋ก ์ค์ ํ๋ค.

```
// left  ...   minCurIndex ... maxCurIndex... right
// left  ...   maxCurIndex ... minCurIndex... right
```

- ์์ ๊ฐ์ ๊ฒฝ์ฐ์ sub-array ๊ฐฏ์๋ฅผ ๊ตฌํด์ ์ถ๊ฐํ๋ค.

## ๐ Intuition

### Sliding window

๊ธฐ๋ณธ template

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

### ๐ก `right` ์ํํ๋ฉด์ min/max ๊ฐ์ ๊ฐ๋ index ์ฐพ์

- min/max ํน์ฑ ๋๋ฌธ์ window ์์ ๊ฐ์ด min/max ๋ฅผ ๋์ด์์ง ์์ผ๋ฉด window ๋ ๊ณ์ ํ์ธ๋จ.
- min/max ์ฐพ์๋ค๊ณ  ํด์ ๋ฐ๋ก window ๊ฐ ๋ซ๊ฑฐ๋ ํ  ์ ์๊ณ , ์ดํ์ ๊ณ์ ๋์ด๋  ์๋ ์๊ณ , window๋ ์ปค์ง ์ ์์ง๋ง ์กฐ๊ฑด์ ๋ง๋ min/max ์์น๋ ๋ชป ์ฐพ์ ์๋ ์๋ค.
  - ์งํํ๋ฉด์ ์กฐ๊ฑด์ ๋ง๋ ๊ฒฝ์ฐ๋ฅผ ์ ์ฅํ์.(`minCurIndex`, `maxCurIndex`)
  - ๋ ๊ฐ์ง ์กฐ๊ฑด์ด ๋ค ๋ง์กฑํ ๊ฒฝ์ฐ์๋ง sub-array ๋ด๋ถ ๊ฐฏ์ ์ธ๊ธฐ ์ํ์ฌ flag ์ค์ ์ ํ์ (`minFound`, `maxFound`).

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

### ๐ก shrink window

- `right` ํฌ์ธํฐ ๊ฐ์ด ์ฃผ์ด์ง ์ต์/์ต๋๋ฅผ ๋์ด์๋ฉด ๋ ์ด์ `left` ๋ก๋ถํฐ ์์ํ๋ sub-array ๊ฐ valid ํ์ง ์์.
- ํ์ฌ ์์น ๋ค์๋ถํฐ ์๋ก window ์์ํ  ์ ์๋๋ก ์ค์ .

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

### ๐ก ์์์ ์ฐพ์ ํฌ์ธํฐ๋ก๋ถํฐ sub-array ๊ณ์ฐ

```javascript
// left  ...   minCurIndex ... maxCurIndex... right
// left  ...   maxCurIndex ... minCurIndex... right
```

- ํ์ฌ ํฌ์ธํฐ์ ์์น๋ ์์ ๊ฐ๋ค.

```javascript
// left  ...   minCurIndex ... maxCurIndex... right
// left  ...   maxCurIndex ... minCurIndex... right
                   |<----------->|
```

- ์์ ๊ตฌ๊ฐ์ min/max ๊ฐ์ ๊ตฌ์ฑํ๋ ํ์ sub-array ๊ตฌ๊ฐ์ด ๋จ.
  - ์ด ๋ถ๋ถ์ ํ๋๋ก ๋ชจ๋ ํฌํจ์ด ๋์ด์ผ ํจ.
  - ์ด ๊ตฌ๊ฐ ์ฌ์ด์์๋ sub-array๋ก ๋๋  ์ ์์.

### ๐ ๐ ์กฐ๊ฑด ๋ง์กฑํ๋ sub-array ๊ฐฏ์๋ ?

```javascript
// left  ...   minCurIndex ... maxCurIndex... right
// left  ...   maxCurIndex ... minCurIndex... right
    o  o  o  o  o  |<----------->|
                   [ ํ  ์  ๊ตฌ  ๊ฐ ]
    [ ์   ํ ๊ตฌ ๊ฐ]
                |<-------------->|
             |<----------------->|
          |<-------------------->|
       |<----------------------->|
    |<-------------------------->|
```

- `left` ์ `Math.min(minCurIndex, maxCurIndex)` ์ฌ์ด์ ํฌ์ธํธ๋ก๋ถํฐ ์๋ก์ด sub-array๊ฐ ์์ํ  ์ ์์ผ๋ฏ๋ก ์ด ์ฌ์ด์ ํฌ์ธํฐ์ ๊ฐฏ์๊ฐ sub-array์ ๊ฐฏ์๊ฐ ๋๋ค.

```javascript
if (minFound && maxFound) {
  res += Math.min(minCurIndex, maxCurIndex) - left + 1;
}
```

## ๐ฅ My Solution

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
