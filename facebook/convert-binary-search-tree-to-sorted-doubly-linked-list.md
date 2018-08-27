# Convert Binary Search Tree to Sorted Doubly Linked List

## Description

Convert a BST to a sorted circular doubly-linked list in-place. Think of the left and right pointers as synonymous to the previous and next pointers in a doubly-linked list.

Let's take the following BST as an example, it may help you understand the problem better: 

![](https://leetcode.com/static/images/problemset/BSTDLLOriginalBST.png)

We want to transform this BST into a circular doubly linked list. Each node in a doubly linked list has a predecessor and successor. For a circular doubly linked list, the predecessor of the first element is the last element, and the successor of the last element is the first element.

The figure below shows the circular doubly linked list for the BST above. The "head" symbol means the node it points to is the smallest element of the linked list. 

![](https://leetcode.com/static/images/problemset/BSTDLLReturnDLL.png)

Specifically, we want to do the transformation in place. After the transformation, the left pointer of the tree node should point to its predecessor, and the right pointer should point to its successor. We should return the pointer to the first element of the linked list.

The figure below shows the transformed BST. The solid line indicates the successor relationship, while the dashed line means the predecessor relationship. 

![](https://leetcode.com/static/images/problemset/BSTDLLReturnBST.png)

## Solution

先不管改不改指针的问题，想要排好序的结果，中序遍历是必须的。

两种方式，递归或者用Stack

第一反应，用Stack，感觉在递归的情况下不太好修改指针。（用Stack感觉逻辑上比较直观，这里就跳过了）

卧槽，我他么居然拿递归实现了，感觉这里面的逻辑还是挺麻烦的。。

注意输入为null的corner case!!!

discussion 里面也有用递归实现的，可以参考：[https://leetcode.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list/discuss/149151/Concise-Java-solution-Beats-100](https://leetcode.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list/discuss/149151/Concise-Java-solution-Beats-100)

下面是我的解答：

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;

    public Node() {}

    public Node(int _val,Node _left,Node _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
    public Node treeToDoublyList(Node root) {
        if (root == null) return root;
        
        Node dummy = new Node(0, null, null);
        Node last = helper(root, dummy);
        last.right = dummy.right;
        dummy.right.left = last;
        return dummy.right;
    }
    
    private Node helper(Node root, Node previous) {
        if (root == null) return null;
        if (root.left == null) {
            previous.right = root;
            root.left = previous;
        } else {
             Node pre = helper(root.left, previous);
             pre.right = root;
             root.left = pre;
        }
        
        if (root.right == null) return root;
        Node rightpre = helper(root.right, root);
        return rightpre;
    }
}
```

