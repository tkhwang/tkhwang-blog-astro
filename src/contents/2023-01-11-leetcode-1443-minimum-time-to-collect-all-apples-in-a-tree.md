---
title: "leetcode 1443. Minimum Time to Collect All Apples in a Tree | medium | dfs"
datetime: 2023-01-11T00:00:00Z
description: "leetcode 1443. Minimum Time to Collect All Apples in a Tree | javascript  | medium | dfs"
slug: "2023-01-11-leetcode-1443-minimum-time-to-collect-all-apples-in-a-tree"
featured: false
draft: false
tags:
  - medium
  - dfs
ogImage: ""
---

## 🗒️ Problems

[1443. Minimum Time to Collect All Apples in a Tree - Leetcode](https://leetcode.com/problems/minimum-time-to-collect-all-apples-in-a-tree/)

```
Given an undirected tree consisting of n vertices numbered from 0 to n-1, which has some apples in their vertices. You spend 1 second to walk over one edge of the tree. Return the minimum time in seconds you have to spend to collect all apples in the tree, starting at vertex 0 and coming back to this vertex.

The edges of the undirected tree are given in the array edges, where edges[i] = [ai, bi] means that exists an edge connecting the vertices ai and bi. Additionally, there is a boolean array hasApple, where hasApple[i] = true means that vertex i has an apple; otherwise, it does not have any apple.
```

```
Input: n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,true,true,false]
Output: 8
Explanation: The figure above represents the given tree where red vertices have an apple. One optimal path to collect all apples is shown by the green arrows.
```

## ✨ Idea

- Use dfs post-order traversal.
- The recursive dfs function returns the time which is required to gether apples in subtree.

### The leaf node

- return 0 because there is no further node to traverse.

```
    .
   / \
  /   \
{4}   {5} // 4 => 0, 5 => 0
[0]    [0]
```

### Non-leaf node

- If the child has an apple, the paths (2) between the node and the child should be added on the return of the child node.

```
    1     // 1 => 2 (1->4,4<-1) + 2 (1->5,5<-1)
   / \
  /   \
{4}   {5}
```

```
0      // 0 => 2 (0->2,2->0)
 \
  \
  {2} => 0
  / \
 /   \
3     6
```

## 💡 Adjacent lists

### Adjacent lists using object

I usually used the object for the adjacent list.

```javascript
const graph = {};
for (const [s, e] of edges) {
  if (graph[s] === undefined) graph[s] = [];
  if (graph[e] === undefined) graph[e] = [];
  graph[s].push(e);
  graph[e].push(s);
}
```

Sometimes I confused to check the availability of the node's children.

- `graph[node] === undefined`
- `graph[node].length === 0`
- `graph[node] === undefined || graph[node].length === 0`

### Adjacent lists using array

- It uses the default empty array for all node, it's more easier to check the availability of the node's children.
- `graph[node].length === 0`

```javascript
const graph = Array(N)
  .fill(null)
  .map(_ => []);
for (const [s, e] of edges) {
  graph[s].push(e);
  graph[e].push(s);
}
```

## 🔥♻️ My Solution

```javascript
/**
 * @param {number} n
 * @param {number[][]} edges
 * @param {boolean[]} hasApple
 * @return {number}
 */
var minTime = function (N, edges, hasApple) {
  const graph = Array(N)
    .fill(null)
    .map(_ => []);
  for (const [s, e] of edges) {
    graph[s].push(e);
    graph[e].push(s);
  }

  const dfs = (cur, parent) => {
    if (graph[cur].length === 0) return 0;

    let time = 0;
    for (const child of graph[cur]) {
      if (child === parent) continue;

      const childTime = dfs(child, cur);
      if (childTime > 0 || hasApple[child]) time += 2 + childTime;
    }
    return time;
  };

  return dfs(0, -1);
};
```

### 🙆‍♂️ Time complexity: `O(n)`
