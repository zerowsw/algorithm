# Lowest Common Ancestor III

## Description

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor\(LCA\) of the two nodes.  
The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.  
Return `null` if LCA does not exist.

## Example

For the following binary tree:

```text
  4
 / \
3   7
   / \
  5   6
```

LCA\(3, 5\) = `4`

LCA\(5, 6\) = `7`

LCA\(6, 7\) = `7`

## Solution

目前来讲，我能想到的最直接的解法，就是自底向上，然后在每个节点处进行判断。

最近公共祖先一定是 1. Node A 和 Node B分布在其左右子树上的。2. 当前节点是Node A或者Node B然后子树上包含另外一个节点。

貌似不需要定义return type，只用一个boolean返回类型也可以？

然而，然而！我忘了考虑TreeNode A == B的情况。。所以还需要增加corner case的处理。

（总体来说，这样的解法，比原本的，定义Return Type的方法要简洁清晰得多）

原本九章提供的算法如下：[https://www.jiuzhang.com/solution/lowest-common-ancestor-iii/](https://www.jiuzhang.com/solution/lowest-common-ancestor-iii/)

如下：

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
    /*
     * @param root: The root of the binary tree.
     * @param A: A TreeNode
     * @param B: A TreeNode
     * @return: Return the LCA of the two nodes.
     */
     
    TreeNode ancestor = null;
    public TreeNode lowestCommonAncestor3(TreeNode root, TreeNode A, TreeNode B) {
        // write your code here
        helper(root, A, B);
        return ancestor;
    }
    
    public boolean helper(TreeNode root, TreeNode A, TreeNode B) {
        if (root == null) return false;
        
        boolean le = helper(root.left, A, B);
        boolean ri = helper(root.right, A, B);
        
        if (A == B && root == A) {
            ancestor = root;
            return false;
        }
        
        if (le && ri) {
            ancestor = root;
            return true;
        } else if (le && (root == A || root == B)) {
            ancestor = root;
            return true;
        } else if (ri && (root == A || root == B)) {
            ancestor = root;
            return true;
        } else {
            return le || ri || root == A || root == B;
        }
    }
    
}
```

