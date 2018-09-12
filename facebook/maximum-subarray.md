---
description: easy题，但是老子edge case没注意，初始化出了点问题
---

# Maximum Subarray

## Description

Given an integer array `nums`, find the contiguous subarray \(containing at least one number\) which has the largest sum and return its sum.

**Example:**

```text
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

**Follow up:**

If you have figured out the O\(_n_\) solution, try coding another solution using the divide and conquer approach, which is more subtle.

## Solution

我的直观的解法：

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int prefixMin = 0;
        int sum = 0;
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            max = Math.max(max, sum - prefixMin);
            prefixMin = Math.min(prefixMin, sum);
        }
        return max;
    }
}
```

诶呦，看看它的follow up, divide and conquer



