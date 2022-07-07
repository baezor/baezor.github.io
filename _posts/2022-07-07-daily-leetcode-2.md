---
layout: post
title: "Daily Leetcode #2"
date: 2022/7/7 00:00:00 -0500
description: Solving one Leetcode problem per day.
categories: leetcode
author: You
---

I decided to solve one Leetcode problem a day to get better at Algos and DS.

### FizzBuzz

My solution:

```js
/**
 * @param {number} n
 * @return {string[]}
 */
const fizzBuzz = function (n) {
  let result = [];
  for (let i = 1; i <= n; i++) {
    let answer = "";
    const fizz = i % 3 === 0;
    const buzz = i % 5 === 0;

    if (fizz) {
      answer += "Fizz";
    }

    if (buzz) {
      answer += "Buzz";
    }

    if (!answer.length) {
      answer = i.toString();
    }

    result.push(answer);
  }

  console.log(result);
  return result;
};


```

- Time Complexity: O(n)
- Space Complexity: O(1)

