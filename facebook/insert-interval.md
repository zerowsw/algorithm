# Insert Interval

## Description

Given a set of _non-overlapping_ intervals, insert a new interval into the intervals \(merge if necessary\).

You may assume that the intervals were initially sorted according to their start times.

## Example

**Example 1:**

```text
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

**Example 2:**

```text
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

## Solution

乍一看，怎么感觉不难呢。。

我写了一个解答，同时也看了别人的解答，确实就是模拟这个合并的过程：

```java
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
class Solution {
    public List<Interval> insert(List<Interval> intervals, Interval newInterval) {

        List<Interval> result = new ArrayList<>();
        boolean find = false;
        boolean merge = false;
        for (int i = 0; i < intervals.size(); i++) {
            Interval cur = intervals.get(i);
            if (merge) {
                Interval last = result.get(result.size() - 1);
                if (last.end < cur.start) {
                    result.add(cur);
                    merge = false;
                } else {
                    last.end = Math.max(last.end, cur.end);
                }
            } else if (!find && newInterval.start <= cur.end) {
                find = true;
                //result.add(new Interval(Math.min(newInterval.start, cur.start), Math.min(newInterval.end, cur.end)));
                if (newInterval.end < cur.start) {
                    result.add(newInterval);
                    result.add(cur);
                } else {
                    result.add(new Interval(Math.min(cur.start, newInterval.start), Math.max(cur.end, newInterval.end)));
                    merge = true;
                }
            } else {
                result.add(cur);
            }
        }
        //妈蛋，又没有考虑到这个corner case
        if (!find) result.add(newInterval);
        return result;
    }
}
```

