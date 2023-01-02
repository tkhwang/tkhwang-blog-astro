---
author: tkhwang
title: "leetcode 394. Decode String | medium | parsing"
slug: 2022-12-15-leetcode-394-decode-string
datetime: 2022-12-15T00:00:00Z
update: 2022-12-15
description: "leetcode 394. Decode String | javascript | medium | parsing"
tags:
  - medium
  - parsing
---

## 🗒️ Problems

[Decode String - LeetCode](https://leetcode.com/problems/decode-string/)

```
Given an encoded string, return its decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; there are no extra white spaces, square brackets are well-formed, etc. Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there will not be input like 3a or 2[4].

The test cases are generated so that the length of the output will never exceed 105.
```

## 🤔 First attempt

- Non nested case like `3[a]2[bc]` was not difficult.
- But the nested one like `3[a2[c]]` was a little bit tricky.
  - How to parse ? using recursive ? ...

## ✨ Idea

- Parsing sequence
  - 💡 First find `]`.
  - Find the matching previous `[`
  - And find the repeating number.
  - Decode string using the above found and save back to the stack.

## 🍀 Steps

### 💡 First find `]`.

```javascript
    if (ch === "]") {
```

### Find the matching previous `[`

- Find the previous matching `[` in the stack.

```javascript
let sub = "";
while (stack.at(-1) !== "[") {
  sub = stack.pop() + sub;
}
// pop "["
stack.pop();
```

### Find the repeating number.

- Repeating number can be more than one character.

```javascript
    if (ch === "]") {
      let num = ""
      while (!isNaN(stack.at(-1))) {
        num = stack.pop() + num
      }
```

### Decode string using the above found and save back to the stack.

```javascript
stack.push(sub.repeat(num));
```

## 🔥 My Solution

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var decodeString = function (s) {
  const N = s.length;

  const stack = [];

  for (let i = 0; i < N; i += 1) {
    const ch = s[i];

    if (ch === "]") {
      let sub = "";
      while (stack.at(-1) !== "[") {
        sub = stack.pop() + sub;
      }
      stack.pop();
      let num = "";
      while (!isNaN(stack.at(-1))) {
        num = stack.pop() + num;
      }
      stack.push(sub.repeat(num));
    } else {
      stack.push(ch);
    }
  }

  return stack.join("");
};
```
