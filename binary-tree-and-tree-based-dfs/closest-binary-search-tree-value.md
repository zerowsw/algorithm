# Closest Binary Search Tree Value

## Description

Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

## Solution

easy题，没啥好说的。

非递归版

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
     * @param target: the given target
     * @return: the value in the BST that is closest to the target
     */
    
    
    public int closestValue(TreeNode root, double target) {
        // write your code here
        int closesValue = root.val;
        //double abs = Math.abs(root.val - target);
        while (root != null) {
            if (Math.abs(closesValue - target) > Math.abs(root.val - target)) {
                closesValue = root.val;
            }
            if (target > root.val) {
                root = root.right;
            } else if (target < root.val) {
                root = root.left;
            } else {
                return root.val;
            }
        }
        return closesValue;
    }
}
```

