# Merge K Sorted Interval Lists

Description

Merge _K_ sorted interval lists into one sorted interval list. You need to merge overlapping intervals too.

## Example

Given

```text
[
  [(1,3),(4,7),(6,8)],
  [(1,2),(9,10)]
]
```

Return

```text
[(1,3),(4,8),(9,10)]
```

## Solution

基本思路没有问题， 用一个PriorityQueue存储k个list中的各一个节点。不过，为了访问下一个interval，同时需要存储它的位置。\(这里一个比较机智的做法，就是直接存储它的位置，而不是在interval的基础上附加一个位置元素\)

我之前在想啊，为什么不把所有的元素放进PriorityQueue然后排序呢。你是不是啥啊，所有都放进去的话，时间复杂度就是NlogN, 每个list放一个的话，时间复杂度就是NlogK

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
    
    private class Point{
        int row,col;
        Point(int row, int col) {
            this.row = row;
            this.col = col;
        }
    }
    /**
     * @param intervals: the given k sorted interval lists
     * @return:  the new sorted interval list
     */
    public List<Interval> mergeKSortedIntervalLists(List<List<Interval>> intervals) {
        // write your code here
        Queue<Point> queue = new PriorityQueue<>(new Comparator<Point>(){
           public int compare(Point a, Point b) {
               return intervals.get(a.row).get(a.col).start - intervals.get(b.row).get(b.col).start;
           } 
        });
        
        for (int i = 0 ;i < intervals.size(); i++) {
            //这里需要考虑空list的情况
            if (intervals.get(i) != null && intervals.get(i).size() > 0) {
                queue.offer(new Point(i, 0));
            }
        }
        
        List<Interval> result = new ArrayList<>();
        while (!queue.isEmpty()) {
            Point cur = queue.poll();
            result.add(intervals.get(cur.row).get(cur.col));
            if (cur.col < intervals.get(cur.row).size() - 1) {
                queue.offer(new Point(cur.row, cur.col + 1));
            }
        }
        List<Interval> merge = mergeInterval(result);
        return merge;
    }
    
    private List<Interval> mergeInterval(List<Interval> result) {
        if (result.size() == 0) return result;
        
        List<Interval> merge = new ArrayList<>();
        merge.add(result.get(0));
        for (int i = 1; i < result.size(); i++) {
            Interval last = merge.get(merge.size() - 1);
            Interval cur = result.get(i);
            if (last.end < cur.start) {
                merge.add(cur);
            } else {
                last.end = Math.max(last.end, cur.end);
            }
        }
        return merge;
    }
}
```

