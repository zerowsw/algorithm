# Search in Rotated Sorted Array

## Description

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

\(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`\).

You are given a target value to search. If found in the array return its index, otherwise return `-1`.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of _O_\(log _n_\).

## Example

**Example 1:**

```text
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```text
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

## Solution

又忘了，需要三个判断条件

```java
class Solution {
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) return -1;
        int tag = nums[nums.length - 1];
        int le = 0;
        int ri = nums.length - 1;
        while (le <= ri) {
            int mid = le + (ri - le) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (target <= tag) {
                if (nums[mid] <= tag && nums[mid] < target || nums[mid] > tag) {
                    le = mid + 1;
                } else {
                    ri = mid - 1;
                } 
            } else if (target > tag) {
                if (nums[mid] <= tag || nums[mid] > tag && nums[mid] > target) {
                    ri = mid - 1;
                } else {
                    le = mid + 1;
                }
            }
        }
        return -1;
    }
}
```

