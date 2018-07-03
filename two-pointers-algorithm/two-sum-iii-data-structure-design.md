# Two Sum III - Data structure design

## Description

Design and implement a TwoSum class. It should support the following operations: `add` and `find`.

`add` - Add the number to an internal data structure.  
`find` - Find if there exists any pair of numbers which sum is equal to the value.

## Example

```text
add(1); add(3); add(5);
find(4) // return true
find(7) // return false
```

## Solution

一般来讲，这种数据结构设计题，都有插入优化和查找优化的trade-off

优先查找：选用一个HashSet存储所有可能的Two-Sum, 当然，随之带来的问题就是每次插入的时候，时间复杂度为N。 （超时间，说明测试集是插入frequent的）

优先插入： 每次O\(1\)时间插入，查找的使用当成Two-Sum的问题来做。

Two-Sum的解法，首先切莫忘记使用HashMap或者说HashSet的方法

查找优先：

```java
public class TwoSum {
    
    List<Integer> container = new ArrayList<>();
    Set<Integer> set = new HashSet<>();
    /*
     * @param number: An integer
     * @return: nothing
     */
    public void add(int number) {
        // write your code here
        for (Integer inte : container) {
            set.add(inte + number);
        }
        container.add(number);
    }

    /*
     * @param value: An integer
     * @return: Find if there exists any pair of numbers which sum is equal to the value.
     */
    public boolean find(int value) {
        // write your code here
        return set.contains(value);
    }
}
```

插入优先

```java
public class TwoSum {
    
    List<Integer> list = new ArrayList<>();
    /*
     * @param number: An integer
     * @return: nothing
     */
    public void add(int number) {
        // write your code here
        list.add(number);
    }

    /*
     * @param value: An integer
     * @return: Find if there exists any pair of numbers which sum is equal to the value.
     */
    public boolean find(int value) {
        // write your code here
        Set<Integer> set = new HashSet<>();
        for (Integer inte : list) {
            if (set.contains(value - inte)) return true;
            set.add(inte);
        }
        return false;
    }
}
```

















