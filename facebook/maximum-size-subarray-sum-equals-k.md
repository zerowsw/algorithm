# Maximum Size Subarray Sum Equals k

## Description

Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn't one, return 0 instead.

**Note:**  
The sum of the entire nums array is guaranteed to fit within the 32-bit signed integer range.

## Example

```text
Input: nums = [1, -1, 5, -2, 3], k = 3
Output: 4 
Explanation: The subarray [1, -1, 5, -2] sums to 3 and is the longest.
```

```text
Input: nums = [-2, -1, 2, 1], k = 1
Output: 2 
Explanation: The subarray [-1, 2] sums to 1 and is the longest.
```

## Solution

用brute force的方法，来监测所有的subarray sum的话，时间复杂度为O\(n^2\)

妈蛋，参考Subarray Sum Equals K使用HashMap的话，就可以将时间复杂度降到O\(n\)

```java
class Solution {
    public int maxSubArrayLen(int[] nums, int k) {
        int max = Integer.MIN_VALUE;
        Map<Integer, Integer> map = new HashMap<>();
        int sum = 0;
        map.put(0, -1);
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (map.containsKey(sum - k)) {
                max = Math.max(max, i - map.get(sum - k));
            }
            if (!map.containsKey(sum)) {
                map.put(sum, i);
            }
        }
        //别忘了Corner Case
        return max == Integer.MIN_VALUE ? 0 : max;
    }
}
```



