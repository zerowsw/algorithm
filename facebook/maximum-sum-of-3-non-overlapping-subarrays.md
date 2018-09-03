# Maximum Sum of 3 Non-Overlapping Subarrays

## Description

In a given array `nums` of positive integers, find three non-overlapping subarrays with maximum sum.

Each subarray will be of size `k`, and we want to maximize the sum of all `3*k` entries.

Return the result as a list of indices representing the starting position of each interval \(0-indexed\). If there are multiple answers, return the lexicographically smallest one.

## Example

```text
Input: [1,2,1,2,6,7,5,1], 2
Output: [0, 3, 5]
Explanation: Subarrays [1, 2], [2, 6], [7, 5] correspond to the starting indices [0, 3, 5].
We could have also taken [2, 1], but an answer of [1, 3, 5] would be lexicographically larger.
```

## Solution

好吧，居然是Dynamic Programming的Hard问题。。

按照，题解的意思，比较自然的一个想法，就是先找到所有的单个的interval，时间复杂度为O\(n\)（使用sliding window的方法），如此以来，可以将问题先简化。

然后，我本能地没有往这上面去想，可以发现我时常会去排斥一些预处理，实际上并不会增加时间复杂度。

动态规划的核心就是记住已经解决的子问题的解。

```java
class Solution {
    public int[] maxSumOfThreeSubarrays(int[] nums, int k) {
        
        int[] interval = new int[nums.length - k + 1];
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (i >= k) sum -= nums[i - k];
            if (i >= k - 1) interval[i - k + 1] = sum;
        }
        
        int[] left = new int[nums.length - k + 1];
        int max = 0;
        for (int i = 0; i < interval.length; i++) {
            if (interval[i] > interval[max]) {
                max = i;
            }
            left[i] = max;
        }
        
        int[] right = new int[nums.length - k + 1];
        max = nums.length - k;
        for (int i = nums.length - k; i >= 0; i--) {
            //注意，这里是大于或等于
            if (interval[i] >= interval[max]) {
                max = i;
            }
            right[i] = max;
        }
        
        int[] res = {-1, -1, -1};
        max = Integer.MIN_VALUE;
        for (int i = k; i < interval.length - k; i++) {
            if (interval[left[i - k]] + interval[i] + interval[right[i + k]] > max) {
                max = interval[left[i - k]] + interval[i] + interval[right[i + k]];
                res[0] = left[i - k];
                res[1] = i;
                res[2] = right[i + k];
            }
        }
        return res;
    }
}
```



