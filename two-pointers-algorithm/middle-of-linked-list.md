---
description: 练手基础题
---

# Middle of Linked List

## Description

Find the middle node of a linked list.

## Example

Given `1->2->3`, return the node with value 2.

Given `1->2`, return the node with value 1.

## Challenge

If the linked list is in a data stream, can you find the middle without iterating the linked list again?

## Solution

首先Two pass的方法就不提了，而且也不适用challenge的情况。

简单的Two Pointers解决。

```java
/**
 * Definition for ListNode
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */


public class Solution {
    /*
     * @param head: the head of linked list.
     * @return: a middle node of the linked list
     */
    public ListNode middleNode(ListNode head) {
        // write your code here
        if (head == null) return null;
        ListNode first = head;
        ListNode second = head;
        while (first.next != null && first.next.next != null) {
            first = first.next.next;
            second = second.next;
        }
        return second;
    }
}
```



