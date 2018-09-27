# Populating Next Right Pointers in Each Node II

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

**Example:**

Given the following binary tree,

```text
     1
   /  \
  2    3
 / \    \
4   5    7
```

After calling your function, the tree should look like:

```text
     1 -> NULL
   /  \
  2 -> 3 -> NULL
 / \    \
4-> 5 -> 7 -> NULL
```

## Solution

如果允许使用BFS，那么问题就简单了。可是它现在不允许使用extra space。。然后提醒递归实现时的函数栈不算额外空间。。

有意思有意思，这道题还是挺有趣的。。

思路和一是类似的，依旧是BFS， 只不过，连接的时候需要注意跳过一些没有节点。

这是我的iterative的写法：

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
        TreeLinkNode prev = null;
        TreeLinkNode nextlevel = root;
        TreeLinkNode cur = null;
        
        while (root != null || nextlevel != null) {
            
            while (root != null && root.left == null && root.right == null) {
                root = root.next;
            }
            
            if (root == null) {
                if (nextlevel == null) return;
                root = nextlevel;
                prev = null;
                nextlevel = null;
                continue;
            }
            
            if (prev != null) {
                if (root.left != null && root.right != null) {
                    prev.next = root.left;
                    root.left.next = root.right;
                    prev = root.right;
                } else if (root.left != null) {
                    prev.next = root.left;
                    prev = root.left;
                } else if (root.right != null) {
                    prev.next = root.right;
                    prev = root.right;
                }
                root = root.next;
            } else {
                if (root.left != null && root.right != null) {
                    nextlevel = root.left;
                    root.left.next = root.right;
                    prev = root.right;
                } else {
                    prev = root.left != null ? root.left : root.right;
                    nextlevel = root.left != null ? root.left : root.right;
                }
                root = root.next;
            }
        }
    }
}
```

参照Discussion之后，简化之后的写法：

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
        
        TreeLinkNode head = root;
        TreeLinkNode cur = root;
        TreeLinkNode prev = null;
        while (cur != null) { 
            while (cur != null) {
                if (cur.left != null) {
                    if (prev == null) {
                        head = cur.left;
                        prev = cur.left;
                    } else {
                        prev.next = cur.left;
                        prev = cur.left;
                    }
                }
                
                if (cur.right != null) {
                    if (prev == null) {
                        head = cur.right;
                        prev = cur.right;
                    } else {
                        prev.next = cur.right;
                        prev = cur.right;
                    }
                }
                cur = cur.next;
            }
            cur = head;
            head = null;
            prev = null;
        }
    }
}
```

