# Binary Tree Maximum Path Sum

## Description

Given a **non-empty** binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain **at least one node** and does not need to go through the root.

## Example

**Example 1:**

```text
Input: [1,2,3]

       1
      / \
     2   3

Output: 6
```

**Example 2:**

```text
Input: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

Output: 42
```

## Solution

之前做过，怎么感觉这道题这么简单呢。。

忘了左右子树都是负数的corner case。。。。。。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    int max = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        helper(root);
        return max;
    }
    
    private int helper(TreeNode root) {
        //end condition
        if (root == null) return 0;
        
        int left = helper(root.left);
        int right = helper(root.right);
        max = Math.max(left + right + root.val, max);
        max = Math.max(left + root.val, max);
        max = Math.max(right + root.val, max);
        //这种corner case别忘了
        max = Math.max(max, root.val);
            
        return Math.max(Math.max(left + root.val, right + root.val), root.val);
    }
}
```

