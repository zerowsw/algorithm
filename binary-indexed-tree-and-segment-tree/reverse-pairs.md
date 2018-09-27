# Reverse Pairs

## Description

Given an array `nums`, we call `(i, j)` an **important reverse pair** if `i < j` and `nums[i] > 2*nums[j]`.

You need to return the number of important reverse pairs in the given array.

**Example1:**

```text
Input: [1,3,2,3,1]
Output: 2
```

**Example2:**

```text
Input: [2,4,3,5,1]
Output: 3
```

**Note:**  


1. The length of the given array will not exceed `50,000`.
2. All the numbers in the input array are in the range of 32-bit integer.

## Solution

首先是BST的方法，平均复杂度是O\(nlogn\), 最差的情况是N^2, 没有过所有的Test Case

```java
class Solution {
    
    private class BSTNode{
        int val;
        int count;
        BSTNode left;
        BSTNode right;
        public BSTNode(int val) {
            this.val = val;
            count = 1;
        }
    }
    
    private int search(BSTNode root, long val) {
        if (root == null) return 0;
        if (val == root.val) {
            return root.count;
        } else if (val < root.val) {
            return root.count + search(root.left, val);
        } else {
            return search(root.right, val);
        }
    }
    
    private BSTNode insert(BSTNode root, int val) {
        if (root == null) {
            return new BSTNode(val);
        }
        if (root.val == val) {
            root.count++;
        } else if(root.val < val) {
            root.count++;
            root.right = insert(root.right, val);
        } else {
            root.left = insert(root.left, val);
        }
        return root;
    }
    
    public int reversePairs(int[] nums) {
        int count = 0;
        if (nums == null || nums.length == 0) return count;
        BSTNode root = null;
        for (Integer num : nums) {
            count += search(root, (long)num * 2 + 1);
            root = insert(root, num);
        }
        return count;
    }
}
```

然后是BIT \(Binary Indexed Tree\)的方法, 

