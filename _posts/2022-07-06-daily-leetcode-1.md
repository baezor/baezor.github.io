---
layout: post
title: "Daily Leetcode #1"
date: 2022/7/6 00:00:00 -0500
description: Solving one Leetcode problem per day.
categories: leetcode
author: You
---

I decided to solve one Leetcode problem a day to get better at Algos and DS. Here's the first exercise I did:

### Running Sum of 1d Array
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


### Richest Customer Wealth
You are given an `m x n` integer grid accounts where `accounts[i][j]` is the amount of money the i​​​​​​​​​​​th​​​​ customer has in the `j​​​​​​​​​​​th`​​​​ bank. Return the wealth that the richest customer has.

A customer's wealth is the amount of money they have in all their bank accounts. The richest customer is the customer that has the maximum wealth.

My solution:

```js 
/**
 * Find wealthiest account.
 *
 * @param {number[][]} accounts
 * @return {number}
 */
const maximumWealth = function(accounts) {
  let max = 0;
  accounts.forEach((account) => {
    const sum = account.reduce((prev, curr) => prev + curr, 0);
    if (sum > max) {
      max = sum;
    }
  });
  return max;
};

```