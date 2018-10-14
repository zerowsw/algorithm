---
description: >-
  这道题Google有个变种，mountain定义为一段区间，区间中的所有点高度都比两端山脚高度要高 2
  （左右各扫一遍，每次记录当前端点，碰到大的跳过，碰到小的，计算一下区间长度并更新最大值）
---

# Longest Mountain in Array

## Description

Let's call any \(contiguous\) subarray B \(of A\) a _mountain_ if the following properties hold:

* `B.length >= 3`
* There exists some `0 < i < B.length - 1` such that `B[0] < B[1] < ... B[i-1] < B[i] > B[i+1] > ... > B[B.length - 1]`

\(Note that B could be any subarray of A, including the entire array A.\)

Given an array `A` of integers, return the length of the longest _mountain_. 

Return `0` if there is no mountain.

**Example 1:**

```text
Input: [2,1,4,7,3,2,5]
Output: 5
Explanation: The largest mountain is [1,4,7,3,2] which has length 5.
```

**Example 2:**

```text
Input: [2,2,2]
Output: 0
Explanation: There is no mountain.
```

**Note:**

1. `0 <= A.length <= 10000`
2. `0 <= A[i] <= 10000`

**Follow up:**

* Can you solve it using only one pass?
* Can you solve it in `O(1)` space?

## Solution

one pass, o\(n\)的时间复杂度

```java
class Solution {
    public int longestMountain(int[] A) {
        int[] left = new int[A.length];
        int [] right = new int[A.length];
            
        for (int i = A.length - 1; i >= 0; i--) {
            if (i < A.length - 1 && A[i] > A[i + 1]) {
                right[i] = right[i + 1] + 1;
            } else {
                right[i] = 1;
            }
        }
        int max = 1;
        int pre = 1;
        for (int i = 0; i < A.length; i++) {
            if (i > 0 && A[i] > A[i - 1]) {
                pre++;
                
            } else {
                pre = 1;
            }
            
            if (right[i] > 1 && pre > 1) {
                max = Math.max(max, pre + right[i] - 1);
            }
        
        }
        return max == 1 ? 0 : max;
    }
}
```

one pass, O\(1\)空间：

```java
class Solution {
    public int longestMountain(int[] A) {
        int maxlen = 0;
        int base = 0;
        int N = A.length;
        while (base < N) {
            int end = base;
            if (end < N - 1 && A[end] < A[end + 1]) {
                while (end < N - 1 && A[end] < A[end + 1]) {
                    end++;
                }
                if (end < N -1 && A[end] > A[end + 1]) {
                    while (end < N - 1 && A[end] > A[end + 1]) {
                        end++;
                    }   
                    maxlen = Math.max(maxlen, end - base + 1);
                }
            }
            base = Math.max(end, base + 1);
        }
        return maxlen;
    }
}
```

