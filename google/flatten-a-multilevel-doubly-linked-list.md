---
description: 其实不是狗家的高频题
---

# Flatten a Multilevel Doubly Linked List

## Description

You are given a doubly linked list which in addition to the next and previous pointers, it could have a child pointer, which may or may not point to a separate doubly linked list. These child lists may have one or more children of their own, and so on, to produce a multilevel data structure, as shown in the example below.

Flatten the list so that all the nodes appear in a single-level, doubly linked list. You are given the head of the first level of the list.

**Example:**

```text
Input:
 1---2---3---4---5---6--NULL
         |
         7---8---9---10--NULL
             |
             11--12--NULL

Output:
1-2-3-7-8-11-12-9-10-4-5-6-NULL
```

**Explanation for the above example:**

Given the following multilevel doubly linked list:

![](https://leetcode.com/static/images/problemset/MultilevelLinkedList.png)

We should return the following flattened doubly linked list:

![](https://leetcode.com/static/images/problemset/MultilevelLinkedListFlattened.png)

## Solution

这个Recursion的解法相当elegant，尤其是对next为null的corner case的处理。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node prev;
    public Node next;
    public Node child;

    public Node() {}

    public Node(int _val,Node _prev,Node _next,Node _child) {
        val = _val;
        prev = _prev;
        next = _next;
        child = _child;
    }
};
*/
class Solution {
    public Node flatten(Node head) {
        helper(head);
        return head;
    }
    //return last node of current level
    public Node helper(Node head) {
        if (head == null) return null;
        if (head.child == null) {
            if (head.next == null) return head;
            return helper(head.next);
        } else {
            Node child = head.child;
            Node next = head.next;
            head.child = null;
            Node childtail = helper(child);
            
            head.next = child;
            child.prev = head;
            
            if (next != null) {
                childtail.next = next;
                next.prev = childtail;
                return helper(next);
            }
            return childtail;
        }
    }
}
```

