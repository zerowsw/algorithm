# Count Complete Tree Nodes

## Description

Given a **complete** binary tree, count the number of nodes.

**Note:**

**Definition of a complete binary tree from** [**Wikipedia**](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**:**  
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

**Example:**

```text
Input: 
    1
   / \
  2   3
 / \  /
4  5 6

Output: 6
```

## Solution

我去，这题我居然卡了一会儿。。

方法很简单，recursion，check当前是不是complete binary tree，是的话，直接用公式算，不是的话，左右子树递归接着算。

时间复杂度O\(\(logn\) ^ 2\) , 用 logn时间把问题转化成n/2的问题。

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
    public int countNodes(TreeNode root) {
        if (root == null) return 0;
        int leftcount = 0;
        TreeNode left = root;
        while (left != null) {
            leftcount++;
            left = left.left;
        }
        int rightcount = 0;
        TreeNode right = root;
        while (right != null) {
            rightcount++;
            right = right.right;
        }
        if (leftcount == rightcount) return (int)Math.pow(2, leftcount) - 1;
        return 1 + countNodes(root.left) + countNodes(root.right);
    }
}
```

