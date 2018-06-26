# Find K Closest Elements

## Description

Given a target number, a non-negative integer `k` and an integer array A sorted in ascending order, find the `k`closest numbers to target in A, sorted in ascending order by the difference between the number and target. Otherwise, sorted in ascending order by number if the difference is same.

## Example

Given A = `[1, 2, 3]`, target = `2` and k = `3`, return `[2, 1, 3].`

Given A = `[1, 4, 6, 8]`, target = `3` and k = `3`, return `[4, 1, 6].`

## Challenge

O\(logn + k\) time complexity.

## Solution

我个人觉得比较直接的解法就是用二分法，先找到最近那个值，然后向两个方向扩展。

记住要先审题，审题！ 距离相等的，按照本身的值由小到大排

私以为我的解法在思维上，更为直接一些



```java
public class Solution {
    /**
     * @param A: an integer array
     * @param target: An integer
     * @param k: An integer
     * @return: an integer array
     */
    public int[] kClosestNumbers(int[] A, int target, int k) {
        // write your code here
        int[] result = new int[k];
        if (A == null || A.length == 0 || A.length < k) {
            return result;
        }
        int le = 0;
        int ri = A.length - 1;
        //这里不是找一个确切的值，而是找最近的值，所以用le + 1 < ri 比较合适
        while (le + 1 < ri) {
            int mid = le + (ri - le) / 2;
            if (A[mid] == target) {
                break;
            } else if (A[mid] < target) {
                le = mid;
            } else {
                ri = mid;
            }
        } 
        int  mid = (le + ri) / 2;
        //java int取整的时候，本身是左偏的，所以你可以想象，最接近的位置必定出自mid或ri
        //需要边写边思考
        int start = Math.abs(A[mid] - target) <= Math.abs(A[ri] - target) ? mid : ri;
        le = start;
        ri = start + 1;
        //int[] result = new int[k];
        int index = 0;
        while (index < k) {
            if (le >= 0 && ri < A.length) {
                result[index] = Math.abs(A[le] - target) <= Math.abs(A[ri] - target) ? A[le--] : A[ri++];
            } else if (le >= 0) {
                result[index] = A[le--];
            } else {
                result[index] = A[ri++];
            }
            index++;
        }
        return result;
    }
}
```

