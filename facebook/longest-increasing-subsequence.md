# Longest Increasing Subsequence

## Example

Given an unsorted array of integers, find the length of longest increasing subsequence.

## Example

```text
Input: [10,9,2,5,3,7,101,18]
Output: 4 
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 
```

## Follow up

Could you improve it to O\(n log n\) time complexity?

## Solution

DP解法，n^2 时间复杂度就直接跳过了，没啥好说的。

看看 nlogn方法的binary search是怎么用进去的。我他么，原来Java还自带binarysearch方法的？？？

这个DP + Binary Search的方法很爆表，一般想不到，可以看这个解释：[https://leetcode.com/problems/longest-increasing-subsequence/discuss/74824/JavaPython-Binary-search-O\(nlogn\)-time-with-explanation](https://leetcode.com/problems/longest-increasing-subsequence/discuss/74824/JavaPython-Binary-search-O%28nlogn%29-time-with-explanation)

同时，这是用Java 自带的binarysearch的方法的解答：

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        int len = 0;
        for (int num : nums) {
            int i = Arrays.binarySearch(dp, 0, len, num);
            if (i < 0) {
                i = -(i + 1);
            }
            dp[i] = num;
            if (i == len) {
                len++;
            }
        }
        return len;
    }
}
```



