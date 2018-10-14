# Maximum Average Subarray II

## Description

Given an array consisting of `n` integers, find the contiguous subarray whose **length is greater than or equal to** `k` that has the maximum average value. And you need to output the maximum average value.

**Example 1:**

```text
Input: [1,12,-5,-6,50,3], k = 4
Output: 12.75
Explanation:
when length is 5, maximum average value is 10.8,
when length is 6, maximum average value is 9.16667.
Thus return 12.75.
```

**Note:**

1. 1 &lt;= `k` &lt;= `n` &lt;= 10,000.
2. Elements of the given array will be in range \[-10,000, 10,000\].
3. The answer with the calculation error less than 10-5 will be accepted.

## Solution

binary search的方法，扫描数组我们可以知道可能的average的最大值和最小值，然后做binary search找到最大的mid \(二分的基础在于，如果当前mid符合要求的话，比它小的mid肯定也是符合要求的\)

这个问题的核心就是如何在O\(n\)的时间判断数组当中是否存在长度至少为K的，average至少为mid的subarray， 这个求解思路借鉴maximum sum of subarray, 只不过很巧妙的保证了K步之外这个条件，具体可以看一下代码：

```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        double lo = Double.MAX_VALUE;
        double hi = Double.MIN_VALUE;
        for (int i = 0; i < nums.length; i++) {
            lo = Math.min(lo, nums[i]);
            hi = Math.max(hi, nums[i]);
        }
        
        while (lo + 0.000001 < hi) {
            double mid = lo + (hi - lo) / 2;
            if (check(nums, mid, k)) {
                lo = mid;
            } else {
                hi = mid;
            }
        }
        return lo;
    }
    
    //O(n)时间check是否存在average至少为mid的，长度至少为K的subarray
    public boolean check(int[] nums, double mid, int k) {
        double sum = 0;
        double prevsum = 0;
        double minprev = 0;
        for (int i = 0; i < k; i++) {
            sum += (nums[i] - mid);
        }
        if (sum >= 0) return true;
        for (int i = k; i < nums.length; i++) {
            sum += (nums[i] - mid);
            prevsum += (nums[i - k] - mid);
            minprev = Math.min(prevsum, minprev);
            if (sum >= minprev) return true;
        }
        return false;
    }
}
```

