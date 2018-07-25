---
description: 经典中的经典，一定要掌握。。
---

# LRU Cache

## Description

Design and implement a data structure for Least Recently Used \(LRU\) cache. It should support the following operations: `get` and `set`.

`get(key)` - Get the value \(will always be positive\) of the key if the key exists in the cache, otherwise return -1.  
`set(key, value)` - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

## Solution

这道题的关键是怎样用一个数据结构实现LRU，也就是说，一个以访问顺序为优先级的队列。

鉴于每次访问之后，需要调整被访问点在队列的位置，将其放到队尾或队首。链表是一个比较好的选择，调整位置只需要移动几个指针即可。当然，为了能够定位到某个具体的节点及修改指针，自定义节点实现链表是一个比较好的选择。同时为了方便移动元素到队尾和队首以及删除元素，需要队尾及队首指针。

为了方便断链得是双向链表

如下：（moveToTail可以考虑重构一下，移出前两行的逻辑）

```java
public class LRUCache {
     /*    * @param capacity: An integer    */        
     private class Node{        
         int value;        
         int key;        
         Node next;        
         Node prev;        
         Node(int value) {            
             this.value = value;        
         }    
     }    
     private int capacity;     
     private Node head;    
     private Node tail;    
     private Map<Integer, Node> map;            
     public LRUCache(int capacity) {        
         // do intialization if necessary        
         this.capacity = capacity;        
         this.head = new Node(-1);        
         this.tail = new Node(-1);        
         head.next = tail;        
         tail.prev = head;        
         this.map = new HashMap<>();          
     }
    /*     
     * @param key: An integer     
     * @return: An integer     
     */    
     public int get(int key) {        
         // write your code here        
         if (!map.containsKey(key)) return -1;        
         Node node = map.get(key);        
         moveToTail(node);        
         return node.value;    
     }
    /*     
     * @param key: An integer     
     * @param value: An integer     
     * @return: nothing     
     */    
     public void set(int key, int value) {        
         // write your code here        
         if (map.containsKey(key)) {            
             moveToTail(map.get(key));            
             map.get(key).value = value;        
             
         } else {            
             Node newNode = new Node(value);            
             newNode.key = key;            
             if (map.size() == capacity) {                
                 Node next = head.next;                
                 head.next = head.next.next;                
                 head.next.prev = head;                
                 next.next = null;                
                 next.prev = null;                
                 map.remove(next.key);            
             }            
             map.put(key, newNode);            
             tail.prev.next = newNode;            
             newNode.prev = tail.prev;            
             newNode.next = tail;            
             tail.prev = newNode;             
         }    
     } 
     
     private void moveToTail(Node node) {        
         node.prev.next = node.next;        
         node.next.prev = node.prev;                
         tail.prev.next = node;        
         node.prev = tail.prev;        
         node.next = tail;        
         tail.prev = node;    
     }
}
```

single list 的版本，抽空可以看一下：

[https://www.jiuzhang.com/solution/lru-cache/](https://www.jiuzhang.com/solution/lru-cache/)

