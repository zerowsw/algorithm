---
description: 又是一道乍一看很懵逼，但实际上
---

# Flatten Binary Tree to Linked List

## Description

Given a binary tree, flatten it to a linked list in-place.

For example, given the following tree:

```text
    1
   / \
  2   5
 / \   \
3   4   6
```

The flattened tree should look like:

```text
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```

## Solution

乍一看真的很懵逼啊，尤其是我还打算用非递归做的时候，就更是懵逼了。。

然而其实，很简单。。

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
    public void flatten(TreeNode root) {
        //TreeNode dummy = new TreeNode(0);
        helper(root);
    }
    
    private TreeNode helper(TreeNode root) {
        if (root == null) return null;
        TreeNode leftend = helper(root.left);
        TreeNode rightend = helper(root.right);
        TreeNode right = root.right;
        if (root.left != null) {
            root.right = root.left;
            leftend.right = right;
        } 
        root.left = null;
        return rightend != null ? rightend : leftend != null ? leftend : root;
    }
}
```

再看看discussion里面的递归解和非递归解，很强：

[https://leetcode.com/problems/flatten-binary-tree-to-linked-list/discuss/](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/discuss/)

