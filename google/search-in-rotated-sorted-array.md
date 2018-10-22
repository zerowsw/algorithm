# Search in Rotated Sorted Array

## Description

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

\(i.e., `[0,0,1,2,2,5,6]` might become `[2,5,6,0,0,1,2]`\).

You are given a target value to search. If found in the array return `true`, otherwise return `false`.

**Example 1:**

```text
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```

**Example 2:**

```text
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```

**Follow up:**

* This is a follow up problem to [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/description/), where `nums` may contain duplicates.
* Would this affect the run-time complexity? How and why?

## Solution

卧槽，居然就一次过了？？

```java
class Solution {
    public boolean search(int[] nums, int target) {
        int le = 0;
        int ri = nums.length - 1;
        
        while (le <= ri) {
            int mid = le + (ri - le) / 2;
            if (nums[mid] == target) return true;
            if (nums[mid] < nums[ri]) {
                if (target < nums[mid] || target > nums[ri]) {
                    ri = mid - 1;
                } else if (target < nums[ri]) {
                    le = mid + 1;
                } else if (target == nums[ri]) {
                    return true;
                }
            } else if (nums[mid] > nums[ri]) {
                if (target > nums[mid] || target < nums[ri]) {
                    le = mid + 1;
                } else if (target > nums[ri]) {
                    ri = mid - 1;
                } else {
                    return true;
                }
            } else {
                ri--;
            }
        }
        return false;
    }
}
```

