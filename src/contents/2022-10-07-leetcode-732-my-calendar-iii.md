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

## ๐๏ธ Problems

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

## ๐ Intuition

### interval์ ๋ํ ์ฒ๋ฆฌ

interval ๋ฌธ์ ์ ๊ฒฝ์ฐ์ `[begin, end]` ๋ก ์ฃผ์ด์ง๋๋ฐ, ๊ฐ์ ์๊ฐ์ด์ง๋ง `begin`์ธ์ง `end` ์ ๋ฐ๋ผ์ ์ฒ๋ฆฌ๋ฅผ ๋ฌ๋ฆฌ ํ๊ธฐ ์ํ์ฌ type (`START = 1`, `END = -1`) ๋ก ๊ตฌ๋ถํ์ฌ ๊ตฌ๊ฐ์ ์๊ฐ์ธ์ง ๋์ธ์ง ์ฒ๋ฆฌํ๋ฉด ํธ๋ฆฌํ ๋๊ฐ ๋ง์ด ์๋ค.

์ด๋ฒ ๋ฌธ์ ์์๋ ๊ฐ ์๊ฐ์์ ์ ํจํ ์ผ์ ์ ๊ฐฏ์๋ฅผ ์์๋ณด๊ธฐ ์ํด์ start ์๊ฐ์์ count๋ฅผ ํ๋ ์ฆ๊ฐ์์ผ์ฃผ๊ณ , end ์๊ฐ์์ count๋ฅผ ํ๋ ๊ฐ์์์ผ์ค์ผ๋ก์จ ํด๋น ์๊ฐ์ ์ ํจํ ์ผ์ ์ ๊ฐฏ์๋ฅผ ๋ํ๋ด๋๋ก ํ๋ค.

### ๐ก javascript์์ python์ `SortedDict` ๊ฐ์ ๊ฒ์ ?

javascript๋ก algorithm ๋ฌธ์ ๋ฅผ ํ๋ค๋ณด๋ฉด collection์ด๋ library ์ง์์ด ๋ง์ง ์์์ ๋ถํธํ ๊ฒฝ์ฐ๊ฐ ๋ง์ด ์๋ค.

์์์ object์ key์ ๊ฐ ์๊ฐ์ ๋ฃ์ด์ event ๊ฐฏ์๋ฅผ ๋ฃ์ด์ ๊ด๋ฆฌํ๋ค๋ฉด ์ด object ๊ฐ ๊ฐ ์๊ฐ์ ๋ฐ๋ผ์ฌ ์ ๋ ฌ์ด ๋์ด ์์ผ๋ฉด ์์ ๊ฒ๋ถํฐ ์์ฐจ์ ์ผ๋ก ์ฌ๋ผ๊ฐ๋ฉด์ ์ต๋ events ๊ฐฏ์๋ฅผ ๊ตฌํ  ์ ์๋ค.

python์์๋ `SortedDict` ๊ฐ ์ง์๋๋ ๊ฒ ๊ฐ์๋ฐ, ๋ง์ javascript์์๋ object literal `{ }` ๋ก ํธํ๊ฒ ์ด์ฉํ  ์๋ ์๋๋ฐ, ๋ฐ๋ก key ๋ก sorted ๋๋ object ๋ ~~๋ค์ด๋ณด์ง ๋ชปํ๋ค.~~

[Does JavaScript guarantee object property order? - Stack Overflow](https://stackoverflow.com/a/23202095)

```
Most Browsers iterate object properties as:

Integer keys in ascending order (and strings like "1" that parse as ints)
String keys, in insertion order (ES2015 guarantees this and all browsers comply)
Symbol names, in insertion order (ES2015 guarantees this and all browsers comply)
```

์ต์ ์๋ object ์ key ๊ฐ integer key ์ธ ๊ฒฝ์ฐ์๋ ascending order๊ฐ ๋ณด์ฅ์ด ๋๋๊ตฌ๋ !!!

์์ ํน์ฑ์ ์ํด์ time ์ด ascending order ๋ก ์ ๋ ฌ์ด ๋์ด ์์ผ๋ฏ๋ก ์ด์ ์ ์์ฝ๋ ์๊ฐ์ ๋ฐ๋ผ์ activeํ event ๊ฐฏ์๋ฅผ ๋์ ํ๋ฉด์ ์ต๋๊ฐ์ ๊ตฌํ๋ฉด ๋๋ค.

### implementation detail์ ์์กด ?

์ ํํ๊ฒ ๊ธฐ์ต์ ๋์ง ์๋๋ฐ, ์์ ์ object์ key ์์์ ๊ฐ์ด implementation detail ํ ๊ฒ์ ์์กดํด์ ํ๋ก๊ทธ๋๋ฐ์ ํ๋ ๊ฒ์ ์ข์ง ์๋ค๊ณ  ๋ณธ ๊ฒ ๊ฐ์๋ฐ... ์์ object ์ key ์์ ๋ณด์ฅ์ ์ด์  spec ์์๋ถํฐ ๋ณด์ฅ๋๋ ๋ฏ ํ๋๊น ์ฌ์ฉํด๋ ๋๋ ๊ฒ์ธ๊ฐ ? ์๋๊ฐ ? ...

## ๐ฅ My Solution

๋ฌธ์ ์์ ๊ตฌํด์ผํ๋ ๊ฒ์ ์ถ๊ฐ๋๋ event๋ฅผ ํฌํจํด์ ์ด์ ์ ์์ฝ๋ ์ผ์  ์ค์์ **์ต๋ ๊ฒน์น๋ ์ผ์  ๊ฐฏ์**๋ฅผ ๊ตฌํ๋ฉด ๋๋ ๊ฒ์ด๋ค.

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
