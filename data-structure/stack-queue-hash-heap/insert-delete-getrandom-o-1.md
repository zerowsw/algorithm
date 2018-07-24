# Insert Delete GetRandom O\(1\)

## Description

Design a data structure that supports all following operations in average `O(1)` time.

* `insert(val)`: Inserts an item val to the set if not already present.
* `remove(val)`: Removes an item val from the set if present.
* `getRandom`: Returns a random element from current set of elements. Each element must have the same probability of being returned.

## Example

```text
// Init an empty set.
RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomSet.insert(1);

// Returns false as 2 does not exist in the set.
randomSet.remove(2);

// Inserts 2 to the set, returns true. Set now contains [1,2].
randomSet.insert(2);

// getRandom should return either 1 or 2 randomly.
randomSet.getRandom();

// Removes 1 from the set, returns true. Set now contains [2].
randomSet.remove(1);

// 2 was already in the set, so return false.
randomSet.insert(2);

// Since 2 is the only number in the set, getRandom always return 2.
randomSet.getRandom();// Init
```

## Solution

要实现O\(1\)时间的random函数，感觉需要水库抽样。

额，不对，有删减过程，所以不能用水库抽样。

要想使所有的操作都是O\(1\)的时间，有一些矛盾点存在。

所以，解法并不简单。。

单纯用Set无法实现O\(1\)时间的random，只是加上一个list的话，无法实现O\(1\)时间的remove。

想法很厉害，用HashMap记录动态变化的位置，每次从list末尾移除，保证了底层也是O\(1\)的时间。

```java
public class RandomizedSet {
    Map<Integer, Integer> map;
    List<Integer> list;
    Random rand;
    public RandomizedSet() {
        // do intialization if necessary
        map = new HashMap<>();
        list = new ArrayList<>();
        rand = new Random();
    }
    
    /*
     * @param val: a value to the set
     * @return: true if the set did not already contain the specified element or false
     */
    public boolean insert(int val) {
        // write your code here
        if (map.containsKey(val)) return false;
        list.add(val);
        map.put(val, list.size() - 1);
        return true;
    }

    /*
     * @param val: a value from the set
     * @return: true if the set contained the specified element or false
     */
    public boolean remove(int val) {
        // write your code here
        if (!map.containsKey(val)) return false;
        int loc = map.get(val);
        if (loc < list.size() - 1) {
            int lastValue = list.get(list.size() - 1);
            list.set(loc, lastValue);
            map.put(lastValue, loc);
        }
        list.remove(list.size() - 1);
        map.remove(val);
        return true;
    }

    /*
     * @return: Get a random element from the set
     */
    public int getRandom() {
        // write your code here
        return list.get(rand.nextInt(list.size()));
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * boolean param = obj.insert(val);
 * boolean param = obj.remove(val);
 * int param = obj.getRandom();
 */
```



