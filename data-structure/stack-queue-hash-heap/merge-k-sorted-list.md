---
description: 经典题，三种做法都试一试吧
---

# Merge K Sorted List

## Description

Merge _k_ sorted linked lists and return it as one sorted list.

Analyze and describe its complexity.

## Example

Given lists:

```text
[
  2->4->null,
  null,
  -1->null
],
```

return `-1->2->4->null`.

## Solution

解法一：两两合并

解法二： 用最小堆，每次pick最小值 （之前做的分布式grep tool 有用到这个算法）

解法三：忘了。。

使用堆：\(写的时候使用dummy node， 是一个不错的小技巧\)

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */
    public ListNode mergeKLists(List<ListNode> lists) {  
        // write your code here
        Queue<ListNode> heap = new PriorityQueue<>(new Comparator<ListNode>(){
           public int compare(ListNode a, ListNode b) {
               return a.val - b.val;
           } 
        });
        
        int k = lists.size();
        for (ListNode node : lists) {
            if (node == null) continue;
            heap.offer(node);
        }
        
        ListNode dummy = new ListNode(0);
        //这里写起来有个小技巧，能够减少一些无用的逻辑
        ListNode tail = dummy;
        while (!heap.isEmpty()) {
            ListNode cur = heap.poll();
            tail.next = cur;
            tail = tail.next;
            if (cur.next != null) {
                heap.offer(cur.next);
            }
        }
        return dummy.next;
    }
}
```





