# Subarray Product Less Than K

## Description

Your are given an array of positive integers `nums`.

Count and print the number of \(contiguous\) subarrays where the product of all the elements in the subarray is less than `k`.

**Example 1:**

```text
Input: nums = [10, 5, 2, 6], k = 100
Output: 8
Explanation: The 8 subarrays that have product less than 100 are: [10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6].
Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.
```

**Note:**

`0 < nums.length <= 50000`.

`0 < nums[i] < 1000`.

`0 <= k < 10^6`.

## Solution

妈勒个鸡，拿到手第一瞬间感觉可以sliding window，然后就可卡住了。。

我日啦，怎么样不重复计算呢，就是每次有新的left的时候，我们就计算一下包含这个left的符合要求的subarray。（答案的思路，每有一个新的right，计算一次当前符合要求的）。

1. The idea is always keep an `max-product-window` less than `K`;
2. Every time shift window by adding a new number on the right\(`j`\), if the product is greater than k, then try to reduce numbers on the left\(`i`\), until the subarray product fit less than `k` again, \(subarray could be empty\);
3. Each step introduces `x` new subarrays, where x is the size of the current window `(j + 1 - i)`; example: for window \(5, 2\), when 6 is introduced, it add 3 new subarray: \(5, \(2, \(6\)\)\)

```text
        (6)
     (2, 6)
  (5, 2, 6)
```

```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int product = 1;
        int left = 0;
        int count = 0;
        for (int right = 0; right < nums.length; right++) {
            product *= nums[right];
            while (left <= right && product >= k) product /= nums[left++];
            if (product < k) count += (right - left + 1);
        }
        return count;
    }
}
```

