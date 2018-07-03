# Search in Rotated Sorted Array

## Description

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

\(i.e., `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`\).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

## Example

For `[4, 5, 1, 2, 3]` and `target=1`, return `2`.

For `[4, 5, 1, 2, 3]` and `target=0`, return `-1`.

## Challenge

O\(logN\) time

## Solution

首先，brute-force的方法就是扫一遍，略。。

然后，binary search的方法，关键点在于如何确定分界点， 有三个因素：1. target处于rotated的左半部分还是右半部分，2. mid处于左半还是右半，3. 两者的大小关系。

总的来说，慢慢来，理清各种情况的话，还是可以顺利解决的。不过，说实话，我本能地会去避免这种三种条件结合的判断，一搞不好，程序就变成各种if嵌套，极其ugly。

```java
public class Solution {
    /**
     * @param A: an integer rotated sorted array
     * @param target: an integer to be searched
     * @return: an integer
     */
    public int search(int[] A, int target) {
        // write your code here
        if (A == null || A.length == 0) return -1;
        
        int le = 0;
        int ri = A.length - 1;
        int tag = A[ri];
        
        while (le <= ri) {
            int mid = le + (ri - le) / 2;
            if (A[mid] == target) return mid;
            
            if (target <= tag) {
                
                if (A[mid] < target) {
                    le = mid + 1;
                } else if (A[mid] <= tag){
                    ri = mid - 1;
                } else {
                    le = mid + 1;
                }
                //从结果来看，这里的逻辑判断可以再稍微合并一下, 如下：
                //if (A[mid] < tag) {
                //    if (A[mid] > target && A[mid] > tag) {
                //        le = mid + 1;
                //    } else {
                //        ri = mid - 1;
                //    }
                //}
            } else {
                if (A[mid] > target) {
                    ri = mid - 1;
                } else if (A[mid] <= tag) {
                    ri = mid - 1;
                } else {
                    le = mid + 1;
                }
            }
        } 
        return -1;
    }
}
```



