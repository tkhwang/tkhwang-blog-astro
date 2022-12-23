---
title: "leetcode 275. H-Index II | medium | binary-search"
datetime: 2023-01-04T00:00:00Z
description: "leetcode 275. H-Index II | javascript  | medium | binary-search"
slug: "2023-01-04-leetcode-275-h-index-ii"
featured: false
draft: false
tags:
  - medium
  - binary-search
ogImage: ""
---

## 🗒️ Problems

[H-Index II - LeetCode](https://leetcode.com/problems/h-index-ii/)

```
Given an array of integers citations where citations[i] is the number of citations a researcher received for their ith paper and citations is sorted in an ascending order, return compute the researcher's h-index.

According to the definition of h-index on Wikipedia: A scientist has an index h if h of their n papers have at least h citations each, and the other n − h papers have no more than h citations each.

If there are several possible values for h, the maximum one is taken as the h-index.

You must write an algorithm that runs in logarithmic time.
```

```
Input: citations = [0,1,3,5,6]
Output: 3
Explanation: [0,1,3,5,6] means the researcher has 5 papers in total and each of them had received 0, 1, 3, 5, 6 citations respectively.
Since the researcher has 3 papers with at least 3 citations each and the remaining two with no more than 3 citations each, their h-index is 3.
```

## 🤔 First attempt

At first it's very difficult to understand the meaning of the problem.

- index `h` in `n` citations
  - `h` papers have at least `h` citations each
  - `n - h` papers have no more than `h` citations each

```
              [0 1 3 5 6 ]
                       6 6 --
                     5 | 5  |
                     | |    | 3 paper : at least `h = 3` citations each.
                   3 | | 3 --
                   | | |
           --- 1 1 | | |
2 papers = --- 0 | | | |
              [0 1 2 3 4 ]
no more than 3 citations each.
```

## ✨ Idea

- Sort `citations` in ascending order.
- Find the mid-index which meets the H-index requirement.

### 💡 Change

- For `mid` value, `h-index` should be `N - mid` because

## 🔥📏 My Solution

```javascript
/**
 * @param {number[]} citations
 * @return {number}
 */
var hIndex = function (citations) {
  const N = citations.length;

  let left = 0;
  let right = N - 1;

  while (left <= right) {
    const mid = Math.floor((left + right) / 2);

    if (citations[mid] === N - mid) {
      return N - mid;
    } else if (citations[mid] > N - mid) {
      right = mid - 1;
    } else if (citations[mid] < N - mid) {
      left = mid + 1;
    }
  }
  return N - left;
};
```

### 🙆‍♂️ Time complexity: `O(n)`
