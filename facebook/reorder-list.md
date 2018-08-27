---
description: 这种链表相关的模拟题仍然需要大量练习
---

# Reorder List

## Description

Given a singly linked list _L_: _L_0→_L_1→…→_Ln_-1→_L_n,  
reorder it to: _L_0→_Ln_→_L_1→_Ln_-1→_L_2→_Ln_-2→…

You may **not** modify the values in the list's nodes, only nodes itself may be changed.

## Example

**Example 1:**

```text
Given 1->2->3->4, reorder it to 1->4->2->3.
```

**Example 2:**

```text
Given 1->2->3->4->5, reorder it to 1->5->2->4->3.
```

## Solution

拿到手之后有点被吓住了，其实算是一条标准的模拟题。

注意corner case的处理！！！

```text
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public void reorderList(ListNode head) {
        int count = 0;
        Stack<ListNode> stack = new Stack<>();
        if (head == null || head.next == null) return;
        ListNode dummy = head;
        while (dummy != null) {
            stack.push(dummy);
            dummy = dummy.next;
            count++;
        }
        
        dummy = head;
        //这里操作的时候，你就没有想到corner case嘛
        ListNode tail = stack.pop();
        int cnt = 0;
        while (cnt < count - 1) {
            
            ListNode next = dummy.next;
            if (cnt >= count - 2) next = null;
            tail.next = next;
            dummy.next = tail;  
            dummy = next;
            //corner case， 请注意
            if (cnt != count - 2) {
                tail = stack.pop();
            }
            cnt += 2;
        }
        tail.next = null;
    }
}
```

还有一些别的思路，可以参考一下：

[https://leetcode.com/problems/reorder-list/discuss/44992/Java-solution-with-3-steps](https://leetcode.com/problems/reorder-list/discuss/44992/Java-solution-with-3-steps)

