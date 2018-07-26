# Maximum Subarray

## Description

Given an array of integers, find a contiguous subarray which has the largest sum.

## Example

Given the array `[−2,2,−3,4,−1,2,1,−5,3]`, the contiguous subarray `[4,−1,2,1]` has the largest sum = `6`.

## Challenge

Can you do it in time complexity O\(n\)?

## Solution

首先，如果用prefix sum来遍历，毫无难度，时间复杂度为O\(n^2\)。

O\(n\) 的实现方法， 想来是DP无疑了。

转移方程，路上想想

