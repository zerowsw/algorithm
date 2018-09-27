# Populating Next Right Pointers in Each Node

## Description

Given a binary tree

```text
struct TreeLinkNode {
  TreeLinkNode *left;
  TreeLinkNode *right;
  TreeLinkNode *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

**Note:**

* You may only use constant extra space.
* Recursive approach is fine, implicit stack space does not count as extra space for this problem.
* You may assume that it is a perfect binary tree \(ie, all leaves are at the same level, and every parent has two children\).

**Example:**

Given the following perfect binary tree,

```text
     1
   /  \
  2    3
 / \  / \
4  5  6  7
```

After calling your function, the tree should look like:

```text
     1 -> NULL
   /  \
  2 -> 3 -> NULL
 / \  / \
4->5->6->7 -> NULL
```

## Solution

卧槽，看了答案，很简洁，并不难。然后不论是Recursive的，还是iterative的方法我都没有想的到。。

我日了狗了。。

这道题我做过。。

你不是在想，怎么样获得在不同父节点下面的临近右子节点嘛，通过你的父节点的next指针过去呗。。

也就是说，我没有想到利用next指针。。

```java
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void connect(TreeLinkNode root) {
        //因为是perfect binary tree, 所以我才这样干的。。
        if (root == null || root.left == null) return;
        root.left.next = root.right;
        if (root.next != null) {
            root.right.next = root.next.left;
        }
        connect(root.left);
        connect(root.right);
    }
}
```

还在想怎么用iterative的方法做嘛？其实就是类似BFS，只不过借用了next指针。。

```java
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void connect(TreeLinkNode root) {
        if (root == null) return;
        while (root.left != null) {
            root.left.next = root.right;
            TreeLinkNode prev = root.right;
            TreeLinkNode next = root.next;
            while (next != null) {
                prev.next = next.left;
                next.left.next = next.right;
                prev = next.right;
                next = next.next;
            }
            root = root.left;
        }
    }
}
```

