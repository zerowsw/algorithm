# Maximum Subarray

## Description

Given an array of integers, find a contiguous subarray which has the largest sum.

## Example

Given the array `[−2,2,−3,4,−1,2,1,−5,3]`, the contiguous subarray `[4,−1,2,1]` has the largest sum = `6`.

## Challenge

Can you do it in time complexity O\(n\)?

## Solution

首先，如果用prefix sum来遍历，毫无难度，时间复杂度为O\(n^2\)。

我呵呵了，正确的prefix sum的解法了解一下。

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: A integer indicate the sum of max subarray
     */
    public int maxSubArray(int[] nums) {
        // write your code here
        int sum = 0;
        int max = Integer.MIN_VALUE;
        int min_sum = 0;
        
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            max = Math.max(max, sum - min_sum);
            min_sum = Math.min(min_sum, sum);
        }
        return max;
    }
}
```

