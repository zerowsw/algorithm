# Binary Search Tree Iterator

## Description

Design an iterator over a binary search tree with the following rules:

* Elements are visited in ascending order \(i.e. an in-order traversal\)
* `next()` and `hasNext()` queries run in O\(_1_\) time in **average**.

## Example

For the following binary search tree, in-order traversal by using iterator is `[1, 6, 10, 11, 12]`

```text
   10
 /    \
1      11
 \       \
  6       12
```

## Challenge

Extra memory usage O\(h\), h is the height of the tree.

**Super Star**: Extra memory usage O\(1\)

## Solution

如果没用空间上的限制，直接保存inorder的序列即可。

如果Extra memory usage是O\(h\)的话，借鉴Closest Binary Search Tree Value II的方法，用一个Stack先保存最小节点的traverse序列，然后每次moverUpper即可。（顿时感觉之前做的题目有了用武之地）。

如果Extra memory要求是O\(1\)的话，额， 难道要借鉴Morris Traversal? \(我觉得没有必要掌握这个算法\)。

综上，我就写一个用Stack实现的\(O\(h\)\) iterator, 也算是最常用的实现方法。

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
 * Example of iterate a tree:
 * BSTIterator iterator = new BSTIterator(root);
 * while (iterator.hasNext()) {
 *    TreeNode node = iterator.next();
 *    do something for node
 * } 
 */

public class BSTIterator {
    Stack<TreeNode> stack = new Stack<>();
    /*
    * @param root: The root of binary tree.
    */public BSTIterator(TreeNode root) {
        // do intialization if necessary
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
    }

    /*
     * @return: True if there has next node, or false
     */
    public boolean hasNext() {
        // write your code here
        return !stack.isEmpty();
    }

    /*
     * @return: return next node
     */
    public TreeNode next() {
        // no need to judge empty or not here, consumer will do that
        TreeNode res = stack.peek();
        if (res.right == null) {
            TreeNode peek = stack.pop();
            while (!stack.isEmpty() && stack.peek().right == peek) {
                peek = stack.pop();
            }
            return res;
        }
        TreeNode t = res.right;
        while (t != null) {
            stack.push(t);
            t = t.left;
        }
        return res;
    } 
}
```



