---
layout: post
title: "Daily Leetcode #1"
date: 2022/7/1 00:00:00 -0500
description: Solving one Leetcode problem per day.
categories: leetcode
author: You
---

I decided to solve one Leetcode problem a day to get better at Algos and DS. Here's the first exercise I did:

1480. Running Sum if 1d Array
Given an array `nums` We defined a running sum of an array as `runningSum[i] = sum(nums[0]…nums[i])`.

Return the running sum of `nums`.

My solution:

```js
/** 
 * Overwritten Input Approach
 *
 * @param {number[]} nums
 * @return {number[]}
 */
const runningSum = function(nums) {
    for(let i = 1; i < nums.length; i++){
        nums[i] += nums[i-1] 
    }
    return nums;
};

```

- Time Complexity: O(n)
- Space Complexity: O(1)
