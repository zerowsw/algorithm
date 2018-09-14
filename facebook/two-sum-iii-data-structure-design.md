# Two Sum III - Data structure design

## Description

Design and implement a TwoSum class. It should support the following operations: `add` and `find`.

`add` - Add the number to an internal data structure.  
`find` - Find if there exists any pair of numbers which sum is equal to the value.

## Example

**Example 1:**

```text
add(1); add(3); add(5);
find(4) -> true
find(7) -> false
```

**Example 2:**

```text
add(3); add(1); add(2);
find(3) -> true
find(6) -> false
```

## Solution

另一种超时了

```java
class TwoSum {

    List<Integer> data;
    //Set<Integer> set;
    /** Initialize your data structure here. */
    public TwoSum() {
        data = new LinkedList<>();
        //set = new HashSet<>();
    }
    
    /** Add the number to an internal data structure.. */
    public void add(int number) {
        data.add(number);
    }
    
    /** Find if there exists any pair of numbers which sum is equal to the value. */
    public boolean find(int value) {
        Set<Integer> set = new HashSet<>();
        for (Integer inte : data) {
            if (set.contains(value - inte)) return true;
            set.add(inte);
        }
        return false;
    }
}

/**
 * Your TwoSum object will be instantiated and called as such:
 * TwoSum obj = new TwoSum();
 * obj.add(number);
 * boolean param_2 = obj.find(value);
 */
```

