# Frog Jump

## Description

A frog is crossing a river. The river is divided into x units and at each unit there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.

Given a list of stones' positions \(in units\) in sorted ascending order, determine if the frog is able to cross the river by landing on the last stone. Initially, the frog is on the first stone and assume the first jump must be 1 unit.

If the frog's last jump was k units, then its next jump must be either k - 1, k, or k + 1 units. Note that the frog can only jump in the forward direction.

**Note:**

* The number of stones is ≥ 2 and is &lt; 1,100.
* Each stone's position will be a non-negative integer &lt; 231.
* The first stone's position is always 0.

## Example

**Example 1:**

```text
[0,1,3,5,6,8,12,17]

There are a total of 8 stones.
The first stone at the 0th unit, second stone at the 1st unit,
third stone at the 3rd unit, and so on...
The last stone at the 17th unit.

Return true. The frog can jump to the last stone by jumping 
1 unit to the 2nd stone, then 2 units to the 3rd stone, then 
2 units to the 4th stone, then 3 units to the 6th stone, 
4 units to the 7th stone, and 5 units to the 8th stone.
```

**Example 2:**

```text
[0,1,2,3,4,8,9,11]

Return false. There is no way to jump to the last stone as 
the gap between the 5th and 6th stone is too large.
```

## Solution

因为需要根据之前跳跃的长度来决定当前跳跃的距离，我决定用Set array 或者 List of set来存储跳到之前位置上可能有的步数，如果全部check一遍的话，时间复杂度为O\(n ^ 2\)。

最优解时间复杂度就是O\(n ^ 2\)

这是我的解：

```java
class Solution {
    public boolean canCross(int[] stones) {
        if (stones == null || stones.length <= 1) return true;
        if (stones[1] - stones[0] != 1) return false;
        List<Set<Integer>> dp = new ArrayList<>();
        Set<Integer> set0 = new HashSet<>();
        set0.add(1);
        dp.add(set0);
        for (int i = 2; i < stones.length; i++) {
            Set<Integer> set = new HashSet<>();
            for (int j = 1; j < i; j++) {
                Set<Integer> temp = dp.get(j - 1);
                int dis = stones[i] - stones[j];
                if (temp.contains(dis) || temp.contains(dis - 1) || temp.contains(dis + 1)) {
                    set.add(dis);
                }
            }
            dp.add(set);
        }
        return dp.get(stones.length - 2).size() != 0;
    }
}
```

不对哦，如果从前往后推的话，能够节省更多的时间！！！

```text

```

