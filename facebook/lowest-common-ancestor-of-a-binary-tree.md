---
description: 这道题需要掌握一下最简洁的方法
---

# Lowest Common Ancestor of a Binary Tree

## Solution

我也不用我自己原来那种，返回boolean然后进行判断的版本了。

直接上赵文瀚用过的discussion里面的那种版本。 （注意，这种解法有个前提，那就是p, q一定存在于二叉树当中）

稍微想象就知道了，就是这么简单。

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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return null;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null) return root;
        if (root == p || root == q) return root;
        return left == null ? right : left;
    }
}
```

