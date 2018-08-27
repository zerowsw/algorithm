---
description: 第二次做这题了。。。。。说明很多时候，就是要多写多练
---

# Binary Search Tree Iterator

## Description

Implement an iterator over a binary search tree \(BST\). Your iterator will be initialized with the root node of a BST.

Calling `next()` will return the next smallest number in the BST.

**Note:** `next()` and `hasNext()` should run in average O\(1\) time and uses O\(h\) memory, where h is the height of the tree.

## Solution

从note当中的时间以及空间复杂度的要求上来看，这道题明显是需要使用Stack来做，模拟中序遍历。

其实你都明白了，很简单：

```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

public class BSTIterator {
    Stack<TreeNode> stack;
    public BSTIterator(TreeNode root) {
        stack = new Stack<>();
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
    }
    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !stack.isEmpty();
    }
    /** @return the next smallest number */
    public int next() {
        TreeNode top = stack.pop();
        int res = top.val;
        top = top.right;
        while (top != null) {
            stack.push(top);
            top = top.left;
        }
        return res;
    }
}
/**
 * Your BSTIterator will be called like this:
 * BSTIterator i = new BSTIterator(root);
 * while (i.hasNext()) v[f()] = i.next();
 */
```

