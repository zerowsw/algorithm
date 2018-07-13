# Next Closest Time

## Description

Given a time represented in the format "HH:MM", form the next closest time by reusing the current digits. There is no limit on how many times a digit can be reused.

You may assume the given input string is always valid. For example, "01:34", "12:09" are all valid. "1:34", "12:9" are all invalid.

## Example

Given time = `"19:34"`, return `"19:39"`.

```text
Explanation: 
The next closest time choosing from digits 1, 9, 3, 4, is 19:39, which occurs 5 minutes later.  It is not 19:33, because this occurs 23 hours and 59 minutes later.
```

Given time = `"23:59"`, return `"22:22"`.

```text
Explanation: 
The next closest time choosing from digits 2, 3, 5, 9, is 22:22. It may be assumed that the returned time is next day's time since it is smaller than the input time numerically.
```

## Solution

首先，比较时间先后关系，可以借鉴timestamp概念，计算秒数。

能想到的比较直接的方法就是，找到所有可能的时间组合，然后从中筛选出最邻近的那个。

基本思路没啥，不一样的地方，在于向下搜索的时候，可以加一些判断。

```java
public class Solution {
    /**
     * @param time: the given time
     * @return: the next closest time
     */
    
    String next_time = null;
    int next_value = Integer.MAX_VALUE;
    public String nextClosestTime(String time) {
        // write your code here
        Set<Character> set = new HashSet<>();
        for (Character chr : time.toCharArray()) {
            if (chr == ':') continue;
            set.add(chr);
        }
        String[] times = time.split(":");
        int hour = Integer.parseInt(times[0]);
        int minute = Integer.parseInt(times[1]);
        int minutes = 60 * hour + minute;
        helper(minutes, set, 0, "");
        return next_time;
    }
    
    public void helper(int minutes, Set<Character> set, int index, String cur) {
        if (index == 4) {
            int hour = Integer.parseInt(cur.substring(0, 2));
            int minute = Integer.parseInt(cur.substring(2, 4));
            if (hour > 23 || minute > 59) return;
            int cur_minutes = hour * 60 + minute;
            if (cur_minutes <= minutes) cur_minutes += 24 * 60;
            if (next_value > cur_minutes) {
                next_value = cur_minutes;
                //avoid transfor "05" to "5"
                next_time = cur.substring(0, 2) + ":" + cur.substring(2, 4);
            }
            return;
        }
        for (Character chr : set) {
            helper(minutes, set, index + 1, cur + chr);
        }
    }
 }
```

