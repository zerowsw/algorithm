# Convert Sorted List to Binary Search Tree

## Description

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of _every_ node never differ by more than 1.

**Example:**

```text
Given the sorted linked list: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```

## Solution

其实大致想得出来怎么做，每次取中点作为当前BST的根结点。时间复杂度看一下，n的时间，将问题转化成两个n/2的问题，所以，**最后的复杂度应该是nlogn**

这是我的解法：

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
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
    public TreeNode sortedListToBST(ListNode head) {
        if (head == null) return null;
        
        ListNode a = head;
        ListNode b = head;
        ListNode prev = new ListNode(-1);
        prev.next = a;
        while (b != null && b.next != null) {
            b = b.next.next;
            a = a.next;
            prev = prev.next;
        }
        prev.next = null;
        TreeNode leftSubTree = null;
        if (head != a) {
            leftSubTree = sortedListToBST(head);
        }
        TreeNode rightSubTree = sortedListToBST(a.next);
        TreeNode root = new TreeNode(a.val);
        root.left = leftSubTree;
        root.right = rightSubTree;
        return root;
    }
}
```

