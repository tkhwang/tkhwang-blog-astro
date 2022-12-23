---
title: "how to use binary search to find min-max problems"
datetime: 2023-01-07T00:00:00Z
description: "leetcode XXX | javascript  | hard | XXX"
slug: "2023-01-07-how-to-use-binary-search-to-find-min-max-problem"
featured: true
draft: false
tags:
  - binary-search
ogImage: ""
---

## 🗒️ Reference

[(Python) Powerful Ultimate Binary Search Template. Solved many problems](https://leetcode.com/discuss/general-discussion/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems?utm_source=pocket_mylist)

## ⬇️ find `MIN` problem

What is the `MIN` value which meets some requirement ?

```
                --------------
                |
                |
      -----------
           +    +    +
         min-1 min min+1
```

### 📏 How to use binary search algorithm for finding `MIN` problem?

## ⬆️ find `MAX` problem

## 🤔 First attempt

### 🙅‍♂️ Failed time complexity : `O(n)`

## ✨ Idea

## 🍀 Intuition

### ⬆️ bottom-up

### 🔀 backtrack

### 🕸️ dynamic programming

### 🔗 Linked list

### 📏 Binary search

## 🪜 Steps

## 🌲 tree

## 🥞 stack

## ♻️ recursive

## ➡️ iterative

### 💡

## 🔥 My Solution

```javascript
const bisectGeneralMin = array => {
  let left = MIN_SEARCH_SPACE;
  let right = MAX_SEARCH_SPACE;

  // Minimize such that isOK(k) is True
  //                --------------
  //                |
  //                |
  //      -----------
  // ...    left - 1, [left, ..., right ]
  //
  const isOK = n => {
    // decision logic
    return true;
  };

  while (left < right) {
    const mid = Math.floor((left + right) / 2);

    if (isOK(mid)) {
      // Because mid is checked to be OK, now mid should be the upper bound. [..., mid]
      right = mid;
    } else {
      // Now mid is not OK, so that we need to check from [mid + 1, ...].
      left = mid + 1;
    }
  }

  // After exiting the while loop,
  // left is the minimal k​ satisfying the condition function;
  return left;
};
```

### 🙆‍♂️ Time complexity: `O(n)`
