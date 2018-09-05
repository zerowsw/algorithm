# Median of Two Sorted Arrays

## Description

There are two sorted arrays **nums1** and **nums2** of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O\(log \(m+n\)\).

You may assume **nums1** and **nums2** cannot be both empty.

## Example

**Example 1:**

```text
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**

```text
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

## Solution

用两个指针的O\(m + n\)的方法就直接跳过了

这道题的话，我打算采用九章的分治法，用findKth。。好好参悟吧

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int n = nums1.length + nums2.length;
        if (n % 2 == 0) {
            return (findKth(nums1, nums2, 0, 0, n / 2) + findKth(nums1, nums2, 0, 0, n/ 2 + 1)) / 2.0;
        }
        return (double)findKth(nums1, nums2, 0, 0, n / 2 + 1);
    }
    
    private int findKth(int[] A, int[] B, int startA, int startB, int k) {
        if (startA == A.length) return B[startB + k -1];
        if (startB == B.length) return A[startA + k - 1];
        
        if (k == 1) return Math.min(A[startA], B[startB]);
        
        int midA = startA + k/2 <= A.length ? A[startA + k/2 - 1] : Integer.MAX_VALUE;
        int midB = startB + k/2 <= B.length ? B[startB + k/2 - 1] : Integer.MAX_VALUE;
        if (midA < midB) {
            return findKth(A, B, startA + k/2, startB, k - k/2);
        } else {
            return findKth(A, B, startA, startB + k/2, k - k/2);
        }
    }
}
```

