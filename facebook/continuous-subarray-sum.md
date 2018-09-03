# Continuous Subarray Sum

## Description

Given a list of **non-negative** numbers and a target **integer** k, write a function to check if the array has a continuous subarray of size at least 2 that sums up to the multiple of **k**, that is, sums up to n\*k where n is also an **integer**.

## Example

```text
Input: [23, 2, 4, 6, 7],  k=6
Output: True
Explanation: Because [2, 4] is a continuous subarray of size 2 and sums up to 6.
```

```text
Input: [23, 2, 6, 4, 7],  k=6
Output: True
Explanation: Because [23, 2, 6, 4, 7] is an continuous subarray of size 5 and sums up to 42.
```

## Solution

现阶段看到这种subarray的问题，我脑海中冒出来的就是prefix sum以及hashmap

然后，我觉得hashmap没法帮我搞定，因为没法通过存的Sum直接查询。（naive）

O\(n^2\) 版本，需要注意k = 0的corner case:

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        //corner case, if k == 0, return false;
        //if (k == 0) return false;
        for(int i = 0; i < nums.length - 1; i++) {
            int sum = 0;
            for (int j = i; j < nums.length; j++) {
                sum += nums[j];
                if (j - i > 0 && k != 0 &&sum % k == 0) return true;
                if (k == 0 && sum == 0 && j - i > 0) return true;
            }
        }
        return false;
    }
}
```

HashMap, O\(n\):

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        int sum = 0;
        map.put(0, -1);
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (k != 0) sum %= k;
            if (map.containsKey(sum) && i - map.get(sum) >= 2) return true;
            if (!map.containsKey(sum)) map.put(sum, i);
        }
        return false;
    }
}
```

