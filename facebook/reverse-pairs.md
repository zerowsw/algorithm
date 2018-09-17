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

这个总结，好好研读！！！：[https://leetcode.com/problems/reverse-pairs/discuss/97268/General-principles-behind-problems-similar-to-%22Reverse-Pairs%22](https://leetcode.com/problems/reverse-pairs/discuss/97268/General-principles-behind-problems-similar-to-%22Reverse-Pairs%22)

这是我自己写的，merge sort的方法，时间复杂度为O\(nlogn\), 空间复杂度为O\(n\):

```java
class Solution {
    public int reversePairs(int[] nums) {
        return helper(nums, 0, nums.length - 1);
    }
    
    private int helper(int[] nums, int start, int end) {
        if (start >= end) return 0;
        int m = start + (end - start) / 2;
        int res = helper(nums, start, m) + helper(nums, m + 1, end);
        int le = m;
        int ri = end;
        while (le >= start) {
            while (ri > m && nums[le] <= 2 * (long)nums[ri]) ri--;
            res += (ri - m);
            le--;
        }
        
        int[] merge = merageTwoSortedArray(nums, start, m, end);
        for (int i = start; i <= end; i++) {
            nums[i] = merge[i - start];
        }
        return res;
    }
    
    private int[] merageTwoSortedArray(int[] nums, int left, int m, int right) {
        int[] merge = new int[right - left + 1];
        int i = left;
        int j = m + 1;
        int index = 0;
        while (i <= m || j <= right) {
            if (i <= m && j <= right) {
                merge[index++] = nums[i] < nums[j] ? nums[i++] : nums[j++];
            } else if (i > m) {
                merge[index++] = nums[j++];
            } else {
                merge[index++] = nums[i++];
            }
        }
        return merge;
    }
}
```

