---
author: tkhwang
title: "leetcode 732-my-calendar-iii | hard | intervals"
slug: 2022-10-07-leetcode-732-my-calendar-iii
datetime: 2022-10-07T00:00:00Z
description: "leetcode 732-my-calendar-iii | javascript  | hard | intervals"
tags:
  - hard
  - intervals
---

## 🗒️ Problems

[(1) My Calendar III - LeetCode](https://leetcode.com/problems/my-calendar-iii/)

```
A k-booking happens when k events have some non-empty intersection (i.e., there is some time that is common to all k events.)

...

Implement the MyCalendarThree class:

- MyCalendarThree() Initializes the object.
- int book(int start, int end) Returns an integer k representing the largest integer such that there exists a k-booking in the calendar.
```

```
Input
["MyCalendarThree", "book", "book", "book", "book", "book", "book"]
[[], [10, 20], [50, 60], [10, 40], [5, 15], [5, 10], [25, 55]]
Output
[null, 1, 1, 2, 3, 3, 3]
```

## 🍀 Intuition

### interval에 대한 처리

interval 문제의 경우에 `[begin, end]` 로 주어지는데, 같은 시각이지만 `begin`인지 `end` 에 따라서 처리를 달리 하기 위하여 type (`START = 1`, `END = -1`) 로 구분하여 구간의 시간인지 끝인지 처리하면 편리할때가 많이 있다.

이번 문제에서는 각 시각에서 유효한 일정을 갯수를 알아보기 위해서 start 시각에서 count를 하나 증가시켜주고, end 시각에서 count를 하나 감소시켜줌으로써 해당 시각에 유효한 일정의 갯수를 나타내도록 했다.

### 💡 javascript에서 python의 `SortedDict` 같은 것은 ?

javascript로 algorithm 문제를 풀다보면 collection이나 library 지원이 많지 않아서 불편한 경우가 많이 있다.

앞에서 object의 key에 각 시각을 넣어서 event 갯수를 넣어서 관리한다면 이 object 가 각 시각에 따라사 정렬이 되어 있으면 작은 것부터 순차적으로 올라가면서 최대 events 갯수를 구할 수 있다.

python에서는 `SortedDict` 가 지원되는 것 같은데, 막상 javascript에서는 object literal `{ }` 로 편하게 이용할 수는 있는데, 따로 key 로 sorted 되는 object 는 ~~들어보지 못했다.~~

[Does JavaScript guarantee object property order? - Stack Overflow](https://stackoverflow.com/a/23202095)

```
Most Browsers iterate object properties as:

Integer keys in ascending order (and strings like "1" that parse as ints)
String keys, in insertion order (ES2015 guarantees this and all browsers comply)
Symbol names, in insertion order (ES2015 guarantees this and all browsers comply)
```

최신에는 object 의 key 가 integer key 인 경우에는 ascending order가 보장이 되는구나 !!!

위의 특성에 의해서 time 이 ascending order 로 정렬이 되어 있으므로 이전에 예약된 시간에 따라서 active한 event 갯수를 누적하면서 최대값을 구하면 된다.

### implementation detail에 의존 ?

정확하게 기억은 나지 않는데, 예전에 object의 key 순서와 같이 implementation detail 한 것에 의존해서 프로그래밍을 하는 것은 좋지 않다고 본 것 같은데... 위의 object 의 key 순서 보장은 이제 spec 에서부터 보장되는 듯 하니깐 사용해도 되는 것인가 ? 아닌가 ? ...

## 🔥 My Solution

문제에서 구해야하는 것은 추가되는 event를 포함해서 이전에 예약된 일정 중에서 **최대 겹치는 일정 갯수**를 구하면 되는 것이다.

```javascript
const START = 1;
const END = -1;

var MyCalendarThree = function () {
  this.data = {};
};

MyCalendarThree.prototype.book = function (start, end) {
  this.data[start] = (this.data[start] || 0) + START;
  this.data[end] = (this.data[end] || 0) + END;

  let events = 0;
  let max = 0;

  for (const time of Object.keys(this.data)) {
    events += this.data[time];
    max = Math.max(max, events);
  }

  return max;
};
```
