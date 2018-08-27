# Subarray Sum Equals K

## Description

Given an array of integers and an integer **k**, you need to find the total number of continuous subarrays whose sum equals to **k**.

## Example

```text
Input:nums = [1,1,1], k = 2
Output: 2
```

## Solution

拿到手一瞬间，脑海中出现prefix sum, 时间复杂度为O\(n^2\)

我的做法，修改了原数组，本可以额外用一个数组来做。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            int prefixsum = i > 0 ? nums[i - 1] : 0;
            nums[i] = prefixsum + nums[i];
            if (nums[i] == k) count++;
            for (int j = i - 1; j >= 0; j--) {
                if (nums[i] - nums[j] == k) count++;
            }
        }
        return count;
    }
}
```

另外，O\(n ^ 2\)的方法，还有一个更好的O\(1\)空间的方法, 简洁明了，不要被prefix sum限制住了：

```java
class Solution {
    public int subarraySum(int[] nums, int k) { 
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;
            for (int j = i; j < nums.length; j++) {
                sum += nums[j];
                if (sum == k) count++;
            }
        }
        return count;
    }
}
```

此外，我居然忘了使用HashMap。。。。。。。妈勒个鸡。。

记得把（0， 1）放进去！！！

```java
class Solution {
    public int subarraySum(int[] nums, int k) { 
        int count = 0;
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            count += map.getOrDefault(sum - k, 0);
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
}
```

