---
layout: post
title: "Daily Leetcode #4"
date: 2022/7/8 00:00:00 -0500
description: Solving one Leetcode problem per day.
categories: leetcode
author: You
---

I decided to solve one Leetcode problem a day to get better at Algos and DS.

## Middle of The Linked List

```js
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
var middleNode = function(head) {
    let A = [head];
    while (A[A.length - 1].next != null)
        A.push(A[A.length - 1].next);
    return A[Math.trunc(A.length / 2)]
};
```
