---
description: 这题还要好好看看！！！
---

# Shortest Subarray with Sum at Least K

## Description

Return the **length** of the shortest, non-empty, contiguous subarray of `A` with sum at least `K`.

If there is no non-empty subarray with sum at least `K`, return `-1`.

## Example

**Example 1:**

```text
Input: A = [1], K = 1
Output: 1
```

**Example 2:**

```text
Input: A = [1,2], K = 4
Output: -1
```

**Example 3:**

```text
Input: A = [2,-1,2], K = 3
Output: 3
```

## Solution

感觉就是sliding window。。为啥topic里面还有binary search 和 Queue

卧槽，我的做法压根儿不对。。因为有负数，left指针右移的时候，不好确定停止的位置。

稍等，怎么感觉好像可以当成prefix sum来做，，我日，你是脑子秀逗了嘛？怎么没想到呢。。

解法非常的6，有诸多的思考在其中，如下： 首先计算全部的prefix sum并存在数组当中，然后用Deque来存储prefix sum数组的下标（方便在队首以及队尾进行操作），同时，处理的时候，已经作为有效subarray起点的下标可以直接删除，因为其必然不可能为我们带来更小的结果。

采用这个做法之后，我的代码：

```java
class Solution {
    public int shortestSubarray(int[] A, int K) {
        int[] sum = new int[A.length + 1];
        int total = 0;
        for (int i = 0; i < A.length + 1; i++) {
            if (i == 0) {
                sum[0] = 0;
                continue;
            }
            total += A[i - 1];
            sum[i] = total;
        }
        
        Deque<Integer> queue = new LinkedList<>();
        queue.offer(0);
        int min = Integer.MAX_VALUE;
        for (int i = 1; i < sum.length; i++) {
            while (!queue.isEmpty() && sum[i] < sum[queue.peekLast()]) queue.pollLast();
            while (!queue.isEmpty() && sum[i] - sum[queue.peekFirst()] >= K) {
                int start = queue.pollFirst();
                min = Math.min(min, i - start);
            }
            queue.offer(i);
        }
        return min == Integer.MAX_VALUE ? -1 : min;
    }
}
```





