---
title: "leetcode 57. Insert Interval | medium | interval"
datetime: 2023-01-16T00:00:00Z
description: "leetcode 57. Insert Interval | javascript  | medium | interval"
slug: "2023-01-16-leetcode-57-insert-interval"
featured: false
draft: false
tags:
  - medium
ogImage: ""
---

## 🗒️ Problems

[57. Insert Interval - Leetcode](https://leetcode.com/problems/insert-interval/)

```
You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the ith interval and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.

Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return intervals after the insertion.
```

```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

## ✨ Idea

### 💡 Non-overlapping intervals

- `newInterval` is there before the interval.
- let's collect this interval in `left[]`.

```
  ------------------
  |                |
 first            last
                         --------------
                         |             |
                       start          end
```

```javascript
    let [ start, end ] = newInterval;
    const left = [];
    const right = [];

    for (const interval of intervals) {
        const [ first, last ] = interval;

        if (last < start) left.push(interval);
        ...
    }
```

- `newInterval` is there after the interval.
- let's collect this interval in `right[]`.

```
                                           ------------------
                                           |                |
                                          first            last
                      --------------
                      |             |
                    start          end
```

```javascript
    let [ start, end ] = newInterval;
    const left = [];
    const right = [];

    for (const interval of intervals) {
        const [ first, last ] = interval;

        ...
        else if (first > end) right.push(interval);
        ...
    }
```

### 💡 Overlapping interval matters.

```
          ------------------
          |                |
         first            last
   --------------     ---------------
   |             |    |             |
 start          end  start         end

 =>
   ----------------------------------
   |                                |
Math.min(start, first)   Math.max(last, end)
```

```javascript
    let [ start, end ] = newInterval;
    const left = [];
    const right = [];

    for (const interval of intervals) {
        const [ first, last ] = interval;

        ...
        else {
            start = Math.min(start, first);
            end = Math.max(end, last);
        }
    }
```

### 💡 Putting it all together

```javascript
return [...left, [start, end], ...right];
```

## 🔥 My Solution

```javascript
/**
 * @param {number[][]} intervals
 * @param {number[]} newInterval
 * @return {number[][]}
 */
var insert = function (intervals, newInterval) {
  const N = intervals.length;

  let [start, end] = newInterval;
  const left = [];
  const right = [];

  for (const interval of intervals) {
    const [first, last] = interval;

    if (last < start) left.push(interval);
    else if (first > end) right.push(interval);
    else {
      start = Math.min(start, first);
      end = Math.max(end, last);
    }
  }

  return [...left, [start, end], ...right];
};
```

### 🙆‍♂️ Time complexity: `O(n)`
