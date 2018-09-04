# Find First and Last Position of Element in Sorted Array

## Description

Given an array of integers `nums` sorted in ascending order, find the starting and ending position of a given `target` value.

Your algorithm's runtime complexity must be in the order of _O_\(log _n_\).

If the target is not found in the array, return `[-1, -1]`.

## Example

**Example 1:**

```text
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```text
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

## Solution

没啥好说的

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] res = {-1, -1};
        if (nums == null || nums.length == 0) return res;
        int le = 0;
        int ri = nums.length - 1;
        while (le < ri) {
            int mid = le + (ri - le) / 2;
            if (nums[mid] >= target) {
                ri = mid;
            } else {
                le = mid + 1;
            }
        }
        if (nums[ri] != target) return res;
        int end = ri;
        while (end + 1 < nums.length && nums[end + 1] == target) end++;
        res[0] = ri;
        res[1] = end;
        return res;
    }
}
```

