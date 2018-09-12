# Find Peak Element

## Description

A peak element is an element that is greater than its neighbors.

Given an input array `nums`, where `nums[i] ≠ nums[i+1]`, find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that `nums[-1] = nums[n] = -∞`.

**Note:**

Your solution should be in logarithmic complexity.

## Example

**Example 1:**

```text
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

**Example 2:**

```text
Input: nums = [1,2,1,3,5,6,4]
Output: 1 or 5 
Explanation: Your function can return either index number 1 where the peak element is 2, 
             or index number 5 where the peak element is 6.
```

## Solution

linear的方法就压根儿不需要考虑了。。

给了提示，同时我也想起来了，就是binary search。

二分的方式在于，一个上升子序列和一个下降子序列之间是一定有一个顶点存在的。（可以看看题目的假设）

卧槽，我还以为会debug很久，结果，秒解：

```java
class Solution {
    public int findPeakElement(int[] nums) {  
        int le = 0;
        int ri = nums.length - 1;
        while (le + 1 < ri) {
            int mid = le + (ri - le) / 2;
            if (nums[mid] < nums[mid + 1]) {
                le = mid + 1;
            } else {
                ri = mid;
            }
        }
        return nums[le] < nums[ri] ? ri : le;
    }
}
```



