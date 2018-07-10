# Kth Smallest Element in a BST

## Description

Given a binary search tree, write a function `kthSmallest` to find the kth smallest element in it.

## Challenge

What if the BST is modified \(insert/delete operations\) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

## Solution

这道题拿到手，基本上有两个最基本的思路: \(呵呵哒\)

好吧，inorder遍历的第K个元素即可。。

然而实际操作过程中，思路还是混乱了， 最直接的方法就是自顶向下传入一个index用来统计当前遍历的顺序。结果嘛，我做成了自底向上的实现。 

```java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */

public class Solution {
    /**
     * @param root: the given BST
     * @param k: the given k
     * @return: the kth smallest element in BST
     */
    int kth = -1;
    public int kthSmallest(TreeNode root, int k) {
        // write your code here
        helper(root, k);
        return kth;
    }
    
    public int helper(TreeNode root, int k) {
        if (root == null) return 0;
        int le = helper(root.left, k);
        if (le + 1 == k) {
            kth = root.val;
        }
        int ri = helper(root.right, k - le - 1);
        return le + ri + 1;
    }
}
```

然而实际上，这样写更为直观一些：

```java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */

public class Solution {
    /**
     * @param root: the given BST
     * @param k: the given k
     * @return: the kth smallest element in BST
     */
    
    private int kth, count;
    public int kthSmallest(TreeNode root, int k) {
        // write your code here
        count = 0;
        inorder(root, k);
        return kth;
    }
    
    private void inorder(TreeNode root, int k) {
        if (root == null) return;
        inorder(root.left, k);
        count++;
        if (count == k) {
            kth = root.val;
        }
        if (k > count) {
            inorder(root.right, k);
        }
    }
}
```

