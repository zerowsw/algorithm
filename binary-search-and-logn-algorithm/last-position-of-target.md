---
description: >-
  Description: Find the last position of a target number in a sorted array.
  Return -1 if target does not exist.
---

# Last Position of Target

## Example: 

Given `[1, 2, 2, 4, 5, 5]`.

For target = `2`, return 2.

For target = `5`, return 5.

For target = `6`, return -1.

## Interface: 

```text

public class Solution { /**
@param nums: An integer array sorted in ascending order@param target: An integer@return: An integer*/public int lastPosition(int[] nums, int target) { // write your code here}}
```

## Solution

较为经典的二分搜索，关键点在于如何优雅地在查找过程中去重，从而找到最后一个位置。首先确定这里的结束条件是 &lt;= \(多想想\)， 然后为了找到最后一个位置，在找到第一个值的时候，先记录下来，然后右移。 同时，将记录变量初始化为-1， 可以减少最后的判断。

```java
public class Solution {
    public int lastPosition(int[] nums, int target) {
        // write your code here
        if (nums == null || nums.length == 0) return -1;
        int le = 0;
        int ri = nums.length - 1;
        int result = -1;
        while (le <= ri) {
            int mid = le + (ri - le) / 2;
            if (nums[mid] == target) {
                result = mid;
                le = mid + 1;
            } else if (nums[mid] < target) {
                le = mid + 1;
            } else {
                ri = mid - 1;
            }
        }
        return result;
    }
}
```



