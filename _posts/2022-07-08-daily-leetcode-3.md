---
layout: post
title: "Daily Leetcode #3"
date: 2022/7/8 00:00:00 -0500
description: Solving one Leetcode problem per day.
categories: leetcode
author: You
---

I decided to solve one Leetcode problem a day to get better at Algos and DS.

### Number of Steps to Reduce a Number to Zero

Given an integer `num`, return the number of steps to reduce it to zero.

In one step, if the current number is even, you have to divide it by 2, otherwise, you have to subtract 1 from it.

My solution:

```js
/**
 * @param {number} n
 * @return {string[]}
 */
const numberOfSteps = function (num) {
  let steps = 0;
  while (num > 0) {
    if (num % 2 == 0) {
      num /= 2;
    } else {
      num--;
    }
    steps++;
  }
  return steps;
};
```

- Time Complexity: O(logn)
- Space Complexity: O(1)
