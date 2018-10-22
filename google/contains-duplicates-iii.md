# Contains Duplicates III

## Description

Given an array of integers, find out whether there are two distinct indices iand j in the array such that the **absolute** difference between **nums\[i\]** and **nums\[j\]** is at most t and the **absolute** difference between i and j is at most k.

**Example 1:**

```text
Input: nums = [1,2,3,1], k = 3, t = 0
Output: true
```

**Example 2:**

```text
Input: nums = [1,0,1,1], k = 1, t = 2
Output: true
```

**Example 3:**

```text
Input: nums = [1,5,9,1,5,9], k = 2, t = 3
Output: false
```

## Solution

解法的话，用sliding window + bucket， 很是巧妙，这里discussion里面的解法，没有remap，我们实际写的时候，可以减去Integer.MIN\_VALUE用差值来remap. 









