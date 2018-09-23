# Smallest Range II

## Description

Given an array `A` of integers, for each integer `A[i]` we need to choose **either `x = -K` or `x = K`**, and add `x` to `A[i]` **`(only once)`**.

After this process, we have some array `B`.

Return the smallest possible difference between the maximum value of `B` and the minimum value of `B`.

1. 
**Example 1:**

```text
Input: A = [1], K = 0
Output: 0
Explanation: B = [1]
```

**Example 2:**

```text
Input: A = [0,10], K = 2
Output: 6
Explanation: B = [2,8]
```

**Example 3:**

```text
Input: A = [1,3,6], K = 3
Output: 3
Explanation: B = [4,6,3]
```

**Note:**

1. `1 <= A.length <= 10000`
2. `0 <= A[i] <= 10000`
3. `0 <= K <= 10000`

## Solution

这道题有个很巧妙的转换的思想， 如果所有的数都往上平移K或者向下平移K，其差值是不会变的。所有，问题等效转换成，每个数有两个选择，可以保持不变，或者加2K。

然后在排序的基础上，从前往后试着调整数字，并检查是否能够使得这个difference减小。你所设想的前面不调整，后面调整的情况是不存在的，因为这只能使当前的difference变大而已。

这是一种greedy的做法

```java
class Solution {
    public int smallestRangeII(int[] A, int K) {
        if (A == null || A.length <= 1) return 0;
        Arrays.sort(A);
        int max = A[A.length - 1];
        int min = A[0];
        int res = max - min;
        for (int i = 0; i < A.length - 1; i++) {
            max = Math.max(max, A[i] + 2 * K);
            min = Math.min(A[i + 1], A[0] + 2 * K);
            res = Math.min(max - min, res);
        }
        return res;
    }
}
```

