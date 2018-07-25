# Merge Two Sorted Interval Lists

## Description

Merge two sorted \(ascending\) lists of interval and return it as a new sorted list. The new sorted list should be made by splicing together the intervals of the two lists and sorted in ascending order.

## Example

Given list1 = `[(1,2),(3,4)]` and list2 = `[(2,3),(5,6)]`, return `[(1,4),(5,6)]`.

## Solution

这道题什么时候成了easy了。。

做过很多遍，算法倒是很熟了。每次比较当前两个指针所指向的interval同时保存上一个interval的end即可。

```java
/**
 * Definition of Interval:
 * public classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 * }
 */

public class Solution {
    /**
     * @param list1: one of the given list
     * @param list2: another list
     * @return: the new sorted list of interval
     */
     
    public List<Interval> mergeTwoInterval(List<Interval> list1, List<Interval> list2) {
        // write your code here
        List<Interval> result = new ArrayList<>();
        int l1 = 0;
        int l2 = 0;
        
        if (list1 == null || list1.size() == 0) return list2;
        if (list2 == null || list2.size() == 0) return list1;
        
        if (list1.get(0).start < list2.get(0).start) {
            result.add(list1.get(0));
        } else {
            result.add(list2.get(0));
        }
        
        while (l1 < list1.size() && l2 < list2.size()) {
            Interval last = result.get(result.size() - 1);
            Interval interval1 = list1.get(l1);
            Interval interval2 = list2.get(l2);
            
            if (last.end < Math.min(interval1.start, interval2.start)) {
                if (interval1.start < interval2.start) {
                    result.add(interval1);
                    l1++;
                } else {
                    result.add(interval2);
                    l2++;
                }
            } else {
                if (interval1.start < interval2.start) {
                    last.end = Math.max(last.end, interval1.end);
                    l1++;
                } else {
                    last.end = Math.max(last.end, interval2.end);
                    l2++;
                }
            }
        }
        
        while (l2 < list2.size()) {
            Interval last = result.get(result.size() - 1);
            Interval cur = list2.get(l2);
            if (last.end < cur.start) {
                result.add(cur);
            } else {
                last.end = Math.max(last.end, cur.end);
            }
            l2++;
        }
        
        while (l1 < list1.size()) {
            Interval last = result.get(result.size() - 1);
            Interval cur = list1.get(l1);
            if (last.end < cur.start) {
                result.add(cur);
            } else {
                last.end = Math.max(last.end, cur.end);
            }
            l1++;
        }
        return result;
    }
}
```

