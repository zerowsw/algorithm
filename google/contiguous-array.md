---
description: 或者是数组中正负数的数量相等
---

# Contiguous Array

## Description

Given a binary array, find the maximum length of a contiguous subarray with equal number of 0 and 1.

**Example 1:**

```text
Input: [0,1]
Output: 2
Explanation: [0, 1] is the longest contiguous subarray with equal number of 0 and 1.
```

**Example 2:**

```text
Input: [0,1,0]
Output: 2
Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.
```

**Note:** The length of the given binary array will not exceed 50,000.

## Solution

又是一个subarray的题目，用HashMap来做。有一个巧妙的转化，那就是把0或者说负数当成-1， 1或者说正数当成1，然后就可以

