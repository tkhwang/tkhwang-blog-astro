---
title: "binary search bisect, bisectLeft and bisectRight in javascript"
datetime: 2023-01-07T00:00:00Z
description: "leetcode XXX | javascript  | hard | XXX"
slug: "2023-01-07-binerysearch-bisect-bisectleft-bisectright-in-javascript"
featured: true
draft: false
tags:
  - binary-search
ogImage: ""
---

## 🔥 `bisect()`

`bisect()` is a typical binary search algorithm.

- search space : `[left, right]` inclusive

```javascript
const bisect = (array, target) => {
  const N = array.length;

  // [left, right]
  let left = 0;
  let right = N - 1;

  while (left <= right) {
    const mid = Math.floor((left + right) / 2);

    if (array[mid] === target) return mid;
    else if (array[mid] > target) {
      right = mid - 1;
    } else if (array[mid] < target) {
      left = mid + 1;
    }
  }
  // insertion point
  return left;
};
```

### Why `while (left <= right)` instead of `while (left < right)` ?

## 🔥 `bisectLeft()`

- search space : `[left, right)` half inclusive

```javascript
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
```

## 🔥 `bisectRight()`

- search space : `[left, right)` half inclusive

```javascript
const bisecRight = (array, target) => {
  const N = array.length;

  // [0, N)
  let left = 0;
  let right = N;

  while (left < right) {
    const mid = Math.floor((left + right) / 2);

    // [mid+1, right)
    if (array[mid] === target) {
      left = mid + 1;
      // select left space =>
    } else if (array[mid] > target) {
      right = mid;
      // select right space =>
    } else if (array[mid] < target) {
      left = mid + 1;
    }
  }
  return left - 1;
};
```

## 🗒️ Problems

## 🤔 First attempt

### 🙅‍♂️ Failed time complexity : `O(n)`

## ✨ Idea

## 🍀 Intuition

### ⬇️ top-down

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
const bisect = (array, target) => {
  const N = array.length;

  // [left, right]
  let left = 0;
  let right = N - 1;

  while (left <= right) {
    const mid = Math.floor((left + right) / 2);

    if (array[mid] === target) return mid;
    else if (array[mid] > target) {
      right = mid - 1;
    } else if (array[mid] < target) {
      left = mid + 1;
    }
  }
  // insertion point
  return left;
};
```

### 🙆‍♂️ Time complexity: `O(n)`
