# K Closest Points

## Description

Given some `points` and a point `origin` in two dimensional space, find `k` points out of the some points which are nearest to `origin`.  
Return these points sorted by distance, if they are same with distance, sorted by x-axis, otherwise sorted by y-axis.

## Example

Given points = `[[4,6],[4,7],[4,4],[2,5],[1,1]]`, origin = `[0, 0]`, k = `3`  
return `[[1,1],[2,5],[4,4]]`

## Solution

K closest, 某种程度上来讲类似Kth largest。 鉴于这题是数据结构设计题，比较简单的做法就是直接用heap，设计好comparator即可。

虽然很简单，有些生疏了。。。。。。

```java
/**
 * Definition for a point.
 * class Point {
 *     int x;
 *     int y;
 *     Point() { x = 0; y = 0; }
 *     Point(int a, int b) { x = a; y = b; }
 * } * Definition for a point.
 * class Point {
 *     int x;
 *     int y;
 *     Point() { x = 0; y = 0; }
 *     Point(int a, int b) { x = a; y = b; }
 * }
 */

public class Solution {
    /**
     * @param points: a list of points
     * @param origin: a point
     * @param k: An integer
     * @return: the k closest points
     */
    public Point[] kClosest(Point[] points, Point origin, int k) {
        // write your code here
         // write your code here        
         Queue<Point> heap = new PriorityQueue<>(new Comparator<Point>(){           
             public int compare(Point a, Point b) {                
                 int ox = origin.x;                
                 int oy = origin.y; 
                 int re = (b.x - ox) * (b.x- ox) + (b.y - oy) * (b.y - oy)          
                 - (a.x - ox) * (a.x - ox) - (a.y - oy) * (a.y - oy);
                 if (re != 0) {
                     return re;
                 } else if (a.x != b.x){
                     return b.x - a.x;
                 } else {
                     return b.y - a.y;
                 }
                 
             }        
         });      
         
         for (Point p : points) {            
             heap.offer(p);            
             if (heap.size() > k) heap.poll();        
         }        
         Point[] res = new Point[k];        
         for (int i = k - 1; i >= 0; i--) {            
             res[i] = heap.poll();        
         }        
         return res;    
    }
}
```

