---
description: '经典题，练手, 好吧，其实2分钟就解决了。。'
---

# Find Minimum in Rotated Sorted Array

## Description

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

\(i.e., `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`\).

Find the minimum element.

## Example

Given `[4, 5, 6, 7, 0, 1, 2]` return `0`

## Solution

可以想象，问题的关键点在于搞清楚边界条件，尤其是左右指针的移动。但是呢，如果用le + 1 &lt; ri 就可以避免这个问题。

```java
public class Solution {
    /**
     * @param nums: a rotated sorted array
     * @return: the minimum number in the array
     */
    public int findMin(int[] nums) {
        // write your code here
        int flag = nums[nums.length - 1];
        int le = 0;
        int ri = nums.length - 1;
        while (le + 1 < ri) {
            int mid = le + (ri - le) / 2;
            if (nums[mid] <= flag) {
                ri = mid;
            } else {
                le = mid + 1;
            }
        }
        return nums[le] < nums[ri] ? nums[le] : nums[ri];
    }
}
```

