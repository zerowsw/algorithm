# Inorder Successor in BST

## Description

Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

**Note**: If the given node has no in-order successor in the tree, return `null`.

## Example

**Example 1:**

```text
Input: root = [2,1,3], p = 1

  2
 / \
1   3

Output: 2
```

**Example 2:**

```text
Input: root = [5,3,6,2,4,null,null,1], p = 6

      5
     / \
    3   6
   / \
  2   4
 /   
1

Output: null
```

## Solution

别傻哦，这是BST，既然没有parent指针的话，在没有右子树的情况下，就需自顶向下处理。

先写了非递归的方法，有一些corner case没注意，时间复杂度为O\(logn\)

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
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        TreeNode right = p.right;
        while (right != null && right.left != null) {
            right = right.left;
        }
        if (right != null) return right;
        
        Stack<TreeNode> stack = new Stack<>();
        TreeNode start = root;
        while (start != p) {
            stack.push(start);
            if (p.val > start) {
                start = start.right;
            } else {
                start = start.left;
            }
        }
        TreeNode top = stack.pop();
        TreeNode prev = p;
        whlie (top.right == prev) {
            prev = top;
            top = stack.pop();
        }
        return top;
    }
}
```

如果是非递归的求predecessor的话，也是类似的解法了。

我凑，居然有那么简单的非递归的，并且空间复杂度为O\(1\)的方法，稍等，如果在往下找的过程中就记录下前一个左拐点，那么就不用返回去找的这个过程了, 类似的predecessor也可以找到。

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
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        TreeNode prev = null;
        while (root != null) {
            if (root.val > p.val) {
                prev = root;
                root = root.left;
            } else {
                root = root.right;
            }
        }
        return prev;
    }
}
```

然后递归的解法：

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
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        if (root == null) return null;
        if (root.val <= p.val) { 
            TreeNode successor = inorderSuccessor(root.right, p);
            return successor;
        } else {
            TreeNode successor = inorderSuccessor(root.left, p);
            return successor == null ?  root : successor;
        }
    }
}
```

