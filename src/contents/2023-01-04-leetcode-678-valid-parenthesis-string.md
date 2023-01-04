---
title: "leetcode 678. Valid Parenthesis String | medium | stack"
datetime: 2023-01-04T00:00:00Z
description: "leetcode 678. Valid Parenthesis String | javascript  | medium | stack"
slug: "2023-01-04-leetcode-678-valid-parenthesis-string"
featured: false
draft: false
tags:
  - medium
  - stack
ogImage: ""
---

## 🗒️ Problems

[Valid Parenthesis String - LeetCode](https://leetcode.com/problems/valid-parenthesis-string/)

```
Given a string s containing only three types of characters: '(', ')' and '*', return true if s is valid.

The following rules define a valid string:

Any left parenthesis '(' must have a corresponding right parenthesis ')'.
Any right parenthesis ')' must have a corresponding left parenthesis '('.
Left parenthesis '(' must go before the corresponding right parenthesis ')'.
'*' could be treated as a single right parenthesis ')' or a single left parenthesis '(' or an empty string "".
```

```
Input: s = "(*))"
Output: true
```

#### Constraints:

```
1 <= s.length <= 100
s[i] is '(', ')' or '*'.
```

## 🤔 First attempt

- `*` can be treated as `(`, `)`, or `""`.
- Just change `*` to the possible character by recursion.
- Check the validity at the last.

```javascript
var checkValidString = function (s) {
  const N = s.length;

  const dfs = (index, left, right) => {
    if (index >= N) {
      return left === right;
    }

    if (left < right) return false;

    let res = false;
    if (s[index] === "*") {
      // left
      res = res || dfs(index + 1, left + 1, right);
      // right
      res = res || dfs(index + 1, left, right + 1);
      // empty
      res = res || dfs(index + 1, left, right);
    } else {
      if (s[index] === "(") res = res || dfs(index + 1, left + 1, right);
      else res = res || dfs(index + 1, left, right + 1);
    }
    return res;
  };

  return dfs(0, 0, 0);
};
```

### 🙅‍♂️ Failed time complexity : `O(3^n)`

If the string consists of only `*`, it splits into 3 branches (`(`, `)`, `"`).
So that it will be `O(3^n)`. It fails due TLE because `s.length` can be 100.

## 🍀 Intuition

- Can we use the stack solution like the typical parenthesis matching problem?
- Use another stack for `*`.
- Use `*` stack when open `(` is not available instead.

## 💡🥞 Check close parenthesis `)`

Check the character from the beginning.

- Push `(` in `opens` stack.
- Push `*` in `stars` stack.
- For `)`
  - Firstly use `(` in `opens` stack if possible.
  - If not, use `(` in `stars` stack if possible.
  - If none of the above, it fails to match.

```javascript
for (const [i, ch] of s.split("").entries()) {
  if (ch === "*") {
    stars.push(i);
  } else if (ch === "(") {
    opens.push(i);
  } else if (ch === ")") {
    if (opens.length > 0) opens.pop();
    else if (stars.length > 0) stars.pop();
    else return false;
  }
}
```

If it succeeds, it means that the close parenthesis is matched.

## 💡🥞 Check open parenthesis `(`

Now we need to check the open parenthesis matching.

- The remained `(`s should be matched the remained `*`s.
- When there is `(`, but `*` is not there, it fails.
- The matching `*` should be in the larger position than `(` for matching.

```javascript
while (opens.length > 0) {
  if (stars.length === 0) return false;
  else {
    if (opens.at(-1) < stars.at(-1)) {
      opens.pop();
      stars.pop();
    } else {
      return false;
    }
  }
}

return true;
```

## 🔥🥞 My Solution

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var checkValidString = function (s) {
  const N = s.length;

  const opens = [];
  const stars = [];

  for (const [i, ch] of s.split("").entries()) {
    if (ch === "*") {
      stars.push(i);
    } else if (ch === "(") {
      opens.push(i);
    } else if (ch === ")") {
      if (opens.length > 0) opens.pop();
      else if (stars.length > 0) stars.pop();
      else return false;
    }
  }

  while (opens.length > 0) {
    if (stars.length === 0) return false;
    else {
      if (opens.at(-1) < stars.at(-1)) {
        opens.pop();
        stars.pop();
      } else {
        return false;
      }
    }
  }

  return true;
};
```

### 🙆‍♂️ Time complexity: `O(n)`
