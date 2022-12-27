---
author: tkhwang
title: "leetcode 206. Reverse Linked List | easy | linked-list | iterative | recursive"
datetime: 2022-12-26T00:00:00Z
slug: 2022-12-26-leetcode-206-reverse-linked-list
description: "leetcode 206. Reverse Linked List | javascript  | easy | linked-list | iterative | recursive"
featured: true
draft: false
tags:
  - easy
  - linked-list
  - iterative
  - recursive
ogImage: ""
---

## 🗒️ Problems

[Reverse Linked List - LeetCode](https://leetcode.com/problems/reverse-linked-list/)

```
Given the head of a singly linked list, reverse the list, and return the reversed list.
```

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

## 🤔 First attempt

- I don't know why... but I don't like the linked list.
- Among them, especially I don't like reverting the linked list.
- ...
- Let's be more familiar with the linked list further.

## ✨ Idea

- Use the iterative method.
- Use the recursive function.

## ➡️ iterative

### 💡 How to revert

```
head -> 1 -> 2 -> ...
  |     |
 prv   cur

null <- 1 <- 2 <- ...
        |    |
       prv  cur
```

### 1. revert: set next of `cur` (`cur.next`) to point `prv`

```javascript
cur.next = prv;
```

### 2. backup `cur.next` for later usage

```javascript
// backup
const next = cur.next;
// revert
cur.next = prv;
```

### 3. proceed to the next

```javascript
while (cur) {
  ...
  prv = cur;
  cur = next;
}
```

### 🔥🔗➡️ My recursive solution

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function (head) {
  let prv = null;
  let cur = head;

  while (cur) {
    const next = cur.next;
    cur.next = prv;

    prv = cur;
    cur = next;
  }

  return prv;
};
```

## ♻️ recursive

- Let's define the recursive function which reverts the linked list in action.
  - revert all beneath the given node.
  - return the head of the reverted linked list.

```javascript
  const reverse = node => {
  ...
    return XXX
  };
```

### The result

- is just calling the recursive function `reverse(head)` with the head node.

```javascript
return reverse(head);
```

### ♻️ inside the recursive function

#### 0. problem

```javascript
head -> 1 -> 2 -> 3 -> 4 -> 5 -> null

null <- 1 <- 2 <- 3 <- 4 <- 5 <- head
```

#### 1. basecase

- If the node is null or the only node (next is null), no need to revert.
- Just return the node.

```javascript
if (!node || node.next === null) return node;
```

#### 2. 💡 revert except for the head node

```javascript
const last = reverse(node.next);
```

- `last` is the new head of the reverted linked list.
  - should be returned as the head of the reverted linked list.

```
// proceed to the end
reverse(1,2,3,4,5)


|
1 => reverse(2,3,4,5)
     |
1 => 2 => reverse(3,4,5)
          |
1 => 2 => 3 => reverse(4,5)
               |
1 => 2 => 3 => 4 => reverse(5)

1 => 2 => 3 => 4 => 5
                    |
                   last
```

#### 3. 💡 after returning from the end

- let `node` point back to `node`
- set `node` null
  - only the head node remains `null`.
  - the beneath node was set to be `null`, but later changed to point the previous node after returning `revert()` function.

```javascript
head
|
1 => 2 => 3 => 4 => 5

// revert
node -> // node.next
     <- // node.next.next

node.next.next = node;
node = null;

1 <= 2 <= 3 <= 4 <= 5
|                   |
null               last
```

#### ⬇️♻️ 4. put together

```javascript
const reverse = node => {
  if (!node || node.next === null) return node;

  const last = reverse(node.next);

  node.next.next = node;
  node.next = null;

  return last;
};
```

#### 🔗 5. final result

```
null <- 1 <- 2 <- 3 <- 4 <- 5
```

### 🔥🔗⬇️♻️ My recursive solution

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function (head) {
  const reverse = node => {
    if (!node || node.next === null) return node;

    const last = reverse(node.next);

    node.next.next = node;
    node.next = null;

    return last;
  };

  return reverse(head);
};
```
