---
description: 经典高频题，又来一遍
---

# LRU Cache

## Description

Design and implement a data structure for [Least Recently Used \(LRU\) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU). It should support the following operations: `get` and `put`.

`get(key)` - Get the value \(will always be positive\) of the key if the key exists in the cache, otherwise return -1.  
`put(key, value)` - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

**Follow up:**  
Could you do both operations in **O\(1\)** time complexity?

## Example

```text
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

## Solution

好了，我们来思考一下这道题， 要保证两个操作的O\(1\)的话。 get肯定是没有问题的，关键是put的时候，如果快速的删除Leat Recently used node。 用double linked list是一个比较好的选择。

九章还提供了single list的版本，这道题还可能有不同的follow up, 研究一下。

下面是我这次写的版本：

```java
class LRUCache {
    
    private class Node {
        Node next;
        Node prev;
        int val;
        int key;
        public Node(int key, int val) {
            this.val = val;
            this.key = key;
        }
    }
    
    Map<Integer, Node> map;
    Node head;
    Node tail;
    int capacity;
    
    public LRUCache(int capacity) {
        map = new HashMap<>();
        head = new Node(0,0);
        tail = new Node(0,0);
        head.next = tail;
        tail.prev = head;
        this.capacity = capacity;
    }
    
    public int get(int key) {
        //忘了处理，找不到的情况
        if (!map.containsKey(key)) return -1;
        int res = map.get(key).val;
    
        //remove the node from original position
        Node target = map.get(key);
        moveToHead(target);
        return res;
    }
    
    public void put(int key, int value) {
        if (map.containsKey(key)) {
            map.get(key).val = value;
            moveToHead(map.get(key));
            return;
        }
        
        Node newnode = new Node(key, value);
        map.put(key, newnode);
        //new Node 怎么能movetoHead呢，是不是傻
        //moveToHead(newnode);
        newnode.next = head.next;
        head.next.prev = newnode;
        head.next = newnode;
        newnode.prev = head;
        
        if (map.size() > capacity) {
            Node prev = tail.prev;
            map.remove(prev.key);
            prev.prev.next = tail;
            tail.prev = prev.prev;
            prev.next = null;
            prev.prev = null;
        }
    }
    
    private void moveToHead(Node target) {
        target.prev.next = target.next;
        target.next.prev = target.prev;
        
        Node first = head.next;
        head.next = target;
        target.prev = head;
        target.next = first;
        first.prev = target;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

