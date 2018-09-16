# Kth Smallest Element in a Sorted Matrix

## Description

Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

**Example:**

```text
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.
```

**Note:**   
You may assume k is always valid, 1 ≤ k ≤ n^2.

## Solution

卧槽，这个数组很有特色啊。。感觉好像做过相关的题目。

第一个方法，看过了，怎么说呢，复杂度没有我想得那么好，但是呢，鉴于这个matrix的特点，不失为一个好方法，就是用MinHeap，每次从堆顶poll出来的就是当前的最小值，然后鉴于当前一行都已经在堆中了，下一个候选元素就是同一列的，下一个元素。

我没有定义新的类，同时，我只存了坐标（这样会导致速度慢很多）

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        PriorityQueue<int[]> heap = new PriorityQueue<>((a, b) -> matrix[a[0]][a[1]] - matrix[b[0]][b[1]]);
        for (int i = 0; i < matrix[0].length; i++) heap.offer(new int[]{0, i});
        for (int i = 0; i < k - 1; i++) {
            int[] top = heap.poll();
            if (top[0] == matrix.length - 1) continue;
            heap.offer(new int[]{top[0] + 1, top[1]});
        }
        int[] top = heap.poll();
        return matrix[top[0]][top[1]];
    }
}
```

还有一个Binary Search的方法，非常🐂🍺：

[https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/discuss/85173/Share-my-thoughts-and-Clean-Java-Code](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/discuss/85173/Share-my-thoughts-and-Clean-Java-Code)

