---
description: 先跳过，之后再看
---

# Reverse Pairs

## Description

Given an array `nums`, we call `(i, j)` an **important reverse pair** if `i < j` and `nums[i] > 2*nums[j]`.

You need to return the number of important reverse pairs in the given array.

**Note:**

1. The length of the given array will not exceed `50,000`.
2. All the numbers in the input array are in the range of 32-bit integer.

## Example

**Example1:**

```text
Input: [1,3,2,3,1]
Output: 2
```

**Example2:**

```text
Input: [2,4,3,5,1]
Output: 3
```

## Solution

感觉这道题目挺有意思的

首先，brute force的方法，时间复杂度为O\(n^2\).

优化一点的方法，就是存下之前的数字并保证有序，然后每次做一个二分搜索，时间复杂度可以优化到O\(nlogn\)。

不知道还没有更优的方法了

segment tree!!! 

