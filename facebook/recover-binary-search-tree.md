# Recover Binary Search Tree

## Description

Two elements of a binary search tree \(BST\) are swapped by mistake.

Recover the tree without changing its structure.

## Example

**Example 1:**

```text
Input: [1,3,null,null,2]

   1
  /
 3
  \
   2

Output: [3,1,null,null,2]

   3
  /
 1
  \
   2
```

**Example 2:**

```text
Input: [3,1,4,null,null,2]

  3
 / \
1   4
   /
  2

Output: [2,1,4,null,null,3]

  2
 / \
1   4
   /
  3
```

## Follow up

* A solution using O\(_n_\) space is pretty straight forward.
* Could you devise a constant space solution?

## Solution

我的想法是先做中序遍历找到两个被交换的node，然后in-place交换两个节点。用Stack进行非递归遍历的时候，需要注意保存父节点，这样才能进行交换。

卧槽，卧槽，这。。找到两个点之后，直接交换一下他们的值就好了。。。这，，，，我居然还想着把两个节点整个进行交换。

另外，O\(1\)空间的Morris  Traversal了解一下:  [https://leetcode.com/problems/recover-binary-search-tree/discuss/32559/Detail-Explain-about-How-Morris-Traversal-Finds-two-Incorrect-Pointer](https://leetcode.com/problems/recover-binary-search-tree/discuss/32559/Detail-Explain-about-How-Morris-Traversal-Finds-two-Incorrect-Pointer)

然后，这是我写的非递归版本：

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
    public void recoverTree(TreeNode root) {
        TreeNode pre = new TreeNode(Integer.MIN_VALUE);
        TreeNode first = null;
        TreeNode second = null;
        
        Stack<TreeNode> stack = new Stack<>();
        TreeNode head = root;
        while (head != null) {
            stack.push(head);
            head = head.left;
        }
        while (!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            if (pre.val > cur.val && first == null) {
                first = pre;
            } 
            if (pre.val > cur.val && first != null) {
                second = cur;
            }
            pre = cur;
            TreeNode right = cur.right;
            while (right != null) {
                stack.push(right);
                right = right.left;
            }
        }   
        int temp = first.val;
        first.val = second.val;
        second.val = temp;
    }
}
```



