---
author: tkhwang
title: "leetcode 622. Design Circular Queue | medium | queue"
slug: 2022-12-14-leetcode-622-design-circular-queue
datetime: 2022-12-14T00:00:00Z
update: 2022-12-14
description: "leetcode 622. Design Circular Queue | javascript | medium | queue"
tags:
  - medium
  - queue
  - design
---

## 🗒️ Problems

[Design Circular Queue - LeetCode](https://leetcode.com/problems/design-circular-queue/)

```
Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO (First In First Out) principle, and the last position is connected back to the first position to make a circle. It is also called "Ring Buffer".

One of the benefits of the circular queue is that we can make use of the spaces in front of the queue. In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue. But using the circular queue, we can use the space to store new values.

Implement the MyCircularQueue class:

- `MyCircularQueue(k)` Initializes the object with the size of the queue to be k.
- `int Front()` Gets the front item from the queue. If the queue is empty, return -1.
- `int Rear()` Gets the last item from the queue. If the queue is empty, return -1.
- `boolean enQueue(int value)` Inserts an element into the circular queue. Return true if the operation is successful.
- `boolean deQueue()` Deletes an element from the circular queue. Return true if the operation is successful.
- `boolean isEmpty()` Checks whether the circular queue is empty or not.
- `boolean isFull()` Checks whether the circular queue is full or not.

You must solve the problem without using the built-in queue data structure in your programming language.
```

## ✨ Idea

- Circular queue uses the double pointer `head` and `tail`.
- Besides those pointers, let's use another variable called `len` to indicate the actual data length.

### 💡 basic variables

- `k` : maximum queue size
- `len` : actual queue data size
- `head` : head pointer which there is the first data in. `-1` if the queue is empty.
- `tail` : tail pointer which there is the last data in. `-1` if the queue is empty.

```javascript
var MyCircularQueue = function (k) {
  this.k = k;
  this.array = Array(this.k).fill(0);
  this.len = 0;
  this.head = -1;
  this.tail = -1;
};
```

## 💡 Enqueue

- If the queue is full now, it fails.
- Update `tail` pointer
  - If the queue is empty now, set `head` and `tail` be `0`.
  - If not, increment the tail pointer.
- And set value at the updated `tail` pointer.
- Increment the actual data length `len`.
- Return result `true`.

```javascript
MyCircularQueue.prototype.enQueue = function (value) {
  if (this.isFull()) return false;

  if (this.head === -1 && this.tail == -1) {
    this.head = 0;
    this.tail = 0;
  } else {
    this.tail = (this.tail + 1) % this.k;
  }
  this.array[this.tail] = value;
  this.len += 1;

  return true;
};
```

## 💡 Dequeue

- If the queue is empty now, it fails.
- Decrement the actual data length `len`.
- Check the current data size.
  - If the queue is empty now, set `head` and `tail` be `-1`.
  - If not, increment the head pointer `head`.
- Return result `true`.

```javascript
MyCircularQueue.prototype.deQueue = function () {
  if (this.isEmpty()) return false;

  this.len -= 1;

  if (this.isEmpty()) {
    this.head = -1;
    this.tail = -1;
  } else {
    this.head = (this.head + 1) % this.k;
  }

  return true;
};
```

## 🔥 My Solution

```javascript
/**
 * @param {number} k
 */
var MyCircularQueue = function (k) {
  this.k = k;
  this.array = Array(this.k).fill(0);
  this.len = 0;
  this.head = -1;
  this.tail = -1;
};

/**
 * @param {number} value
 * @return {boolean}
 */
MyCircularQueue.prototype.enQueue = function (value) {
  if (this.isFull()) return false;

  if (this.head === -1 && this.tail == -1) {
    this.head = 0;
    this.tail = 0;
  } else {
    this.tail = (this.tail + 1) % this.k;
  }
  this.array[this.tail] = value;
  this.len += 1;

  return true;
};

/**
 * @return {boolean}
 */
MyCircularQueue.prototype.deQueue = function () {
  if (this.isEmpty()) return false;

  this.len -= 1;

  if (this.isEmpty()) {
    this.head = -1;
    this.tail = -1;
  } else {
    this.head = (this.head + 1) % this.k;
  }

  return true;
};

/**
 * @return {number}
 */
MyCircularQueue.prototype.Front = function () {
  if (this.head === -1) return -1;

  return this.array[this.head];
};

/**
 * @return {number}
 */
MyCircularQueue.prototype.Rear = function () {
  if (this.tail === -1) return -1;

  return this.array[this.tail];
};

/**
 * @return {boolean}
 */
MyCircularQueue.prototype.isEmpty = function () {
  return this.len === 0;
};

/**
 * @return {boolean}
 */
MyCircularQueue.prototype.isFull = function () {
  return this.len === this.k;
};

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * var obj = new MyCircularQueue(k)
 * var param_1 = obj.enQueue(value)
 * var param_2 = obj.deQueue()
 * var param_3 = obj.Front()
 * var param_4 = obj.Rear()
 * var param_5 = obj.isEmpty()
 * var param_6 = obj.isFull()
 */
```
