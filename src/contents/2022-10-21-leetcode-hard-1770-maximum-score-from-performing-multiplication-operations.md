---
author: tkhwang
title: "leetcode 1770. Maximum Score from Performing Multiplication Operations | hard | dynamic-programming"
slug: 2022-10-21-leetcode-hard-1770-maximum-score-from-performing-multiplication-operations
datetime: 2022-10-21T00:00:00Z
description: "leetcode 1770. Maximum Score from Performing Multiplication Operations | javascript | hard | dynamic-programming"
tags:
  - hard
  - dynamic-programming
---

## 🗒️ Problems

[Maximum Score from Performing Multiplication Operations - LeetCode](https://leetcode.com/problems/maximum-score-from-performing-multiplication-operations/)

```
You are given two 0-indexed integer arrays nums and multipliers of size n and m respectively, where n >= m.

You begin with a score of 0. You want to perform exactly m operations. On the ith operation (0-indexed) you will:

- Choose one integer x from either the start or the end of the array nums.
- Add `multipliers[i] * x` to your score.
  - Note that multipliers[0] corresponds to the first operation, multipliers[1] to the second operation, and so on.
  - Remove x from nums.

Return the maximum score after performing m operations.
```

```
Input: nums = [1,2,3], multipliers = [3,2,1]
Output: 14
Explanation: An optimal solution is as follows:
- Choose from the end, [1,2,3], adding 3 * 3 = 9 to the score.
- Choose from the end, [1,2], adding 2 * 2 = 4 to the score.
- Choose from the end, [1], adding 1 * 1 = 1 to the score.
The total score is 9 + 4 + 1 = 14.
```

## 🤔 First attempt

- i번째에서 왼쪽 혹은 오른쪽을 선택하고, 이 선택에 따라서 미래에 영향을 미치므로 dynamic programming 이라 생각함.
- greedy 라면 현재 큰 쪽만을 선택하면 되겠지만 현재 작은 거 선택한 이후에 아주 큰 값이 숨어 있을 수 있으므로 greedy 한 선택이 답이 될 수는 없음.

## ✨ Idea

- 0번째부터 왼쪽, 오른쪽 선택을 recursive 로 계산
  - 왼쪽 선택 시 `nums[left] * multipliers[times] + dfs(left + 1, times + 1)`
  - 오른쪽 선택 시 `nums[right] * multipliers[times] + dfs(left, times + 1)`
  - 현재 단계에서는 recursive 로 계산 이 두 값 중에서 큰 값을 return
- 이를 `multipliers[]` 끝까지 계산

## 🍀 Intuition

이 문제에서 가장 어려웠던 부분은 한 번 선택을 한 다음에 이후에는 선택된 값을 배제한 array 에서 선택을 해야하는데 이 부분 처리가 어려웠음. 선택한 값을 지운 array를 계속 가지고 가야 했는데... 좋은 방법이 있었습니다.

다음 경우에 `right` index 는 자동 계산이 가능할까요 ?

- 주어진 `nums` array 의 length 는 `N`.
- 현재 왼쪽 index 가 `left`
- 현재 총 선택 횟수 `times`

### 💡 `right` 는 자동 계산 가능한가 ?

모든 것을 오른쪽에서만 선택한다면

```javascript
right = N - 1 - times;
```

`times` 중에서 몇 번은 `left` 에서 선택을 했다면

- 매번 오른쪽에서만 선택했다면 `times` 만큼 줄어들텐데
- 그중에서 `left` 만큼은 왼쪽에서 선택을 했으므로 오른쪽은 그만큼 줄어들지 않아도 됨.

```javascript
right = N - 1 - (times - left);
```

## 🕸️⬇️🔥 top-down Solution

```javascript
const N = nums.length;
const NM = multipliers.length;

const cache = {};
const genKey = (left, times) => `${left}:${times}`;

const dfs = (left, times) => {
  const key = genKey(left, times);

  if (times >= NM) return 0;
  if (cache[key] !== undefined) return cache[key];

  const right = N - 1 - (times - left);

  const leftValue = nums[left] * multipliers[times] + dfs(left + 1, times + 1);
  const rightValue = nums[right] * multipliers[times] + dfs(left, times + 1);

  return (cache[key] = Math.max(leftValue, rightValue));
};

return dfs(0, 0);
```
