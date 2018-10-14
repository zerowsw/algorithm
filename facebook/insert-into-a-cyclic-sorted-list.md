---
description: 我他么，这道题堪称是搞corner case的典范啊，真心不好写。。
---

# Insert into a Cyclic Sorted List

## Description

Given a node from a cyclic linked list which is sorted in ascending order, write a function to insert a value into the list such that it remains a cyclic sorted list. The given node can be a reference to _any_ single node in the list, and may not be necessarily the smallest value in the cyclic list.

If there are multiple suitable places for insertion, you may choose any place to insert the new value. After the insertion, the cyclic list should remain sorted.

If the list is empty \(i.e., given node is `null`\), you should create a new single cyclic list and return the reference to that single node. Otherwise, you should return the original given node.

The following example may help you understand the problem better:

## Solution

我就是当成普通的模拟题来做的，可是这里的edge case着实烦人，虽然我的解法要比discussion里面的都要简单：

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node next;

    public Node() {}

    public Node(int _val,Node _next) {
        val = _val;
        next = _next;
    }
};
*/
class Solution {
    public Node insert(Node head, int insertVal) {
        
        Node start = head;
        Node newnode = new Node(insertVal);
        //注意corner case啊
        if (start == null) {
            newnode.next = newnode;
            return newnode;
        }
        
        while (start.val > insertVal && start.val < start.next.val) {
            start = start.next;
        }
        //这他么corner case也太麻烦了吧。。
        while (start.next.val < insertVal && (insertVal < start.val || start.val < start.next.val)) {
            start = start.next;
        }
        
        newnode.next = start.next;
        start.next = newnode;
        return head;
    }
}
```

