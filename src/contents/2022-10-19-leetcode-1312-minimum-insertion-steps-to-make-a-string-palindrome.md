---
author: tkhwang
title: "leetcode 1312. Minimum Insertion Steps to Make a String Palindrome | hard | dynamic-programming"
slug: 2022-10-19-leetcode-1312-minimum-insertion-steps-to-make-a-string-palindrome
datetime: 2022-10-19T00:00:00Z
description: "leetcode 1312. Minimum Insertion Steps to Make a String Palindrome | javascript | hard | dynamic-programming"
tags:
  - hard
  - dynamic-programming
---

## 🗒️ Problems

[Minimum Insertion Steps to Make a String Palindrome - LeetCode](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)

```
Given a string s. In one step you can insert any character at any index of the string.

Return the minimum number of steps to make s palindrome.

A Palindrome String is one that reads the same backward as well as forward.
```

```
Input: s = "zzazz"
Output: 0
Explanation: The string "zzazz" is already palindrome we do not need any insertions.
```

## ✨ Idea

- `dp[i][j]` : `s[i,...,j]` substring 을 palindrome 만들기 위해서 필요한 최소 insertion 횟수.
  - 최종 답 : `dp[0][N-1]`
- basecase : i, j가 동일한 경우인 `dp[i][j]` 의 경우에는 단일 캐릭터이므로 항상 동일하므로 insertion 횟수는 0.
- 상태 전이 방정식: `s[i]` 와 `s[j]` 비교하여
  - 동일할 때 : 맨 외부가 동일하므로 내부 `dp[i+1][j-1]` 변경횟수와 동일함.
  - 다를 때 : 한쪽만 포함한 경우에서 한 번 insertion 한 것 비교해서 minimum

## 🍀 Intuition

### DP : 결정, 상태, 상태 전이 방정식

- 결정 : i,j string 값의 일치 유무
- 상태 : `dp[i][j]` 는 `s[i,...,j]` 의 insertion을 통한 palindrome 만들 수 있는 최소값.
- 상태 전이 방정식

```javascript
if (s[i] === s[j]) {
  dp[i][j] = dp[i + 1][j - 1];
} else {
  dp[i][j] = Math.min(1 + dp[i + 1][j], 1 + dp[i][j - 1]);
}
```

### 🕸️ 💡 dynamic programming 에서 순회 방향의 결정은 어떻게 ?

Dynamic programming 문제를 보면 어떨때는 `0 -> N` 으로 순회를 하고, 어떨때는 `N ->0` 으로 순회하는 등 다양하였는데 아래 내용을 알게 되어 정리해봅니다.

상태 전이 방정식이 계산이 되기 위해서는 다음을 만족해야 한다.

- 상위 문제 `dp[i][j]` 계산을 위하여 이미 계산된 하위 문제의 결과를 참조하게 됩니다.
  - 이전 값 `dp[i-1][j-1]` 을 참조하기도 하고
  - palindrome의 경우에는 내부 string 을 결과를 참조해야하므로 `dp[i+1][j-1]` 값을 참조해야함.
- 상태 전이 방정식 계산 맨 마지막에 최종 결과가 도출되어 함.
  - 상태의 정의에 따라서 최종 값이 달라짐.
  - 어떤 경우에는 `dp[N-1][N-1]`
  - 이번 경우에는 `string[0, N-1]` 처음부터 끝까지 구성된 경우의 최소 insertion 횟수가 필요하므로 최종 값은 `dp[0][N-1]`

이렇게 상태 전이 방정식 계산 과정에서 이전 값이 계산이 되어야 하고, 그 과정의 마지막에 최종 답이 도출되도록 순회 방향을 결정 합니다.

이번 문제의 경우

- basecase i,j 가 같은 경우에는 0으로 초기값.
- `dp[i][j]` 계산 시 다음 세개의 값이 필요함.
  - `dp[i+1][j-1]`
  - `dp[i+1][j]`
  - `dp[i]][j-1]`

이 계산을 위해서는 다음과 같이 역방향으로 움직이면 가능하다.

- row 를 `[N-2, ..., 0]`
- col 을 `[row + 1, ..., N-1]`

```javascript
    for (let i = N -2; i >= 0; i -= 1) {
        for (let j = i + 1; j < N; j += 1) {
            ...
        }
    }
```

## 🔥 My Solution

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var minInsertions = function (s) {
  const N = s.length;

  const dp = Array(N)
    .fill(null)
    .map(_ => Array(N).fill(0));

  for (let i = N - 2; i >= 0; i -= 1) {
    for (let j = i + 1; j < N; j += 1) {
      if (s[i] === s[j]) {
        dp[i][j] = dp[i + 1][j - 1];
      } else {
        dp[i][j] = Math.min(1 + dp[i + 1][j], 1 + dp[i][j - 1]);
      }
    }
  }

  return dp[0][N - 1];
};
```
