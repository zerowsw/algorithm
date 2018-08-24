---
description: 做过，再来回忆一下
---

# Meeting Room II

## Description

Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...]` \(si &lt; ei\), find the minimum number of conference rooms required.

## Example

**Example 1:**

```text
Input: [[0, 30],[5, 10],[15, 20]]
Output: 2
```

**Example 2:**

```text
Input: [[7,10],[2,4]]
Output: 1
```

## Solution

扫描线问题，sweep line algorithm

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
    public int minMeetingRooms(Interval[] intervals) {
        
        int[] starts = new int[intervals.length];
        int[] ends = new int[intervals.length];
        for (int i = 0; i < intervals.length; i++) {
            starts[i] = intervals[i].start;
            ends[i] = intervals[i].end;
        }
        Arrays.sort(starts);
        Arrays.sort(ends);
    
        int sp = 0;
        int ep = 0;
        int max = 0;
        int count = 0;
        while (ep < intervals.length) {
            int curEndTime = ends[ep];
            //int count = 0;
            while (sp < intervals.length && starts[sp] < curEndTime) {
                sp++;
                count++;
            }
            max = Math.max(max, count);
            ep++;
            count--;
        }
        return max;
    }
}
```

