# All O\`one Data Structure

## Description

Implement a data structure supporting the following operations:

1. Inc\(Key\) - Inserts a new key with value 1. Or increments an existing key by 1. Key is guaranteed to be a **non-empty** string.
2. Dec\(Key\) - If Key's value is 1, remove it from the data structure. Otherwise decrements an existing key by 1. If the key does not exist, this function does nothing. Key is guaranteed to be a **non-empty** string.
3. GetMaxKey\(\) - Returns one of the keys with maximal value. If no element exists, return an empty string `""`.
4. GetMinKey\(\) - Returns one of the keys with minimal value. If no element exists, return an empty string `""`.

Challenge: Perform all these in O\(1\) time complexity.

## Solution

卧槽了，感觉咋样都可以。。不想写

我一开始的想法，就是用一个HashMap保存key - node映射，然后用一个Map保存 key - doublelinkedList 映射，然后我有个问题没有考虑到，如果当前的minValue的Node删除之后，没法用O\(1\)的时间来获得下一个minValue。

爆表了爆表了。。

写了一个，debug了好久，有空再优化一下：

```java
class AllOne {
 
    private class Bucket{
        Bucket prev;
        Bucket next;
        Set<String> keySet;
        int value;
        public Bucket(int count) {
            this.value = count;
            keySet = new HashSet<>();
        }
    }
    
    Bucket head;
    Bucket tail;
    
    Map<Integer, Bucket> valueToBucket;
    Map<String, Integer> keyValue;
    
    /** Initialize your data structure here. */
    public AllOne() {
        head = new Bucket(0);
        tail = new Bucket(0);
        head.next = tail;
        tail.prev = head;
        valueToBucket = new HashMap<>();
        keyValue = new HashMap<>();  
    }
    
    /** Inserts a new key <Key> with value 1. Or increments an existing key by 1. */
    public void inc(String key) {
        if (!keyValue.containsKey(key)) {
            keyValue.put(key, 1);
            if (valueToBucket.containsKey(1)) {
                Bucket target = valueToBucket.get(1);
                target.keySet.add(key);
            } else {
                Bucket newbucket = new Bucket(1);
                newbucket.keySet.add(key);
                valueToBucket.put(1, newbucket);
                insertIntoHead(head, newbucket);
            }
        } else {
            int originalvalue = keyValue.get(key);
            keyValue.put(key, originalvalue + 1);
            
            Bucket oldBucket = valueToBucket.get(originalvalue);
            oldBucket.keySet.remove(key);
           
            int newvalue = originalvalue + 1;
            if (valueToBucket.containsKey(newvalue)) {
                valueToBucket.get(newvalue).keySet.add(key);
            } else {
                Bucket newbucket = new Bucket(newvalue);
                newbucket.keySet.add(key);
                valueToBucket.put(newvalue, newbucket);
                insertIntoHead(oldBucket, newbucket);
            } 
            if (oldBucket.keySet.size() == 0) {
                deleteBucket(oldBucket);
            }
        }
    }
    
     /** Decrements an existing key by 1. If Key's value is 1, remove it from the data structure. */
    public void dec(String key) {
        if (keyValue.containsKey(key)) {
            int oldvalue = keyValue.get(key);
            if (oldvalue == 1) {
                keyValue.remove(key);
            } else {
                keyValue.put(key, oldvalue - 1);
            }
            Bucket oldBucket = valueToBucket.get(oldvalue);
            oldBucket.keySet.remove(key);
             
            if (valueToBucket.containsKey(oldvalue - 1)) {
                valueToBucket.get(oldvalue - 1).keySet.add(key);
            } else if (oldvalue != 1) {
                Bucket newbucket = new Bucket(oldvalue - 1);
                newbucket.keySet.add(key);
                valueToBucket.put(oldvalue - 1, newbucket);
                
                newbucket.next = oldBucket;
                newbucket.prev = oldBucket.prev;
                oldBucket.prev.next = newbucket;
                oldBucket.prev = newbucket;
            }
            
            if (oldBucket.keySet.size() == 0) {
                deleteBucket(oldBucket);
            }
        }
    }
    
    private void deleteBucket(Bucket oldbucket) {
        valueToBucket.remove(oldbucket.value);
        oldbucket.prev.next = oldbucket.next;
        oldbucket.next.prev = oldbucket.prev;
        oldbucket.prev = null;
        oldbucket.next = null;
    }
    
    private void insertIntoHead(Bucket he, Bucket newbucket) {
        newbucket.next = he.next;
        he.next.prev = newbucket;
        he.next = newbucket;
        newbucket.prev = he;
    }
    
   
    
    /** Returns one of the keys with maximal value. */
    public String getMaxKey() {
        return tail.prev.value == 0 ? "" : tail.prev.keySet.iterator().next();
    }
    
    /** Returns one of the keys with Minimal value. */
    public String getMinKey() {
        return head.next.value == 0 ? "" : head.next.keySet.iterator().next();
    }
}

/**
 * Your AllOne object will be instantiated and called as such:
 * AllOne obj = new AllOne();
 * obj.inc(key);
 * obj.dec(key);
 * String param_3 = obj.getMaxKey();
 * String param_4 = obj.getMinKey();
 */
```



