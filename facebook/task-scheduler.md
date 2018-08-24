---
description: 这道题也做过
---

# Task Scheduler

## Description

Given a char array representing tasks CPU need to do. It contains capital letters A to Z where different letters represent different tasks.Tasks could be done without original order. Each task could be done in one interval. For each interval, CPU could finish one task or just be idle.

However, there is a non-negative cooling interval **n** that means between two **same tasks**, there must be at least n intervals that CPU are doing different tasks or just be idle.

You need to return the **least** number of intervals the CPU will take to finish all the given tasks.

## Example

```text
Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
Explanation: A -> B -> idle -> A -> B -> idle -> A -> B.
```

## Solution

首先，我有一个用priorityqueue的解法：

```java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] cnt = new int[26];
        for (Character chr : tasks) {
            cnt[chr - 'A']++;
        }
        Queue<Integer> queue = new PriorityQueue<>(new Comparator<Integer>(){
            public int compare(Integer a, Integer b){
                return b - a;
            }
        });
        for (int i = 0; i < 26; i++) {
            if (cnt[i] != 0) queue.offer(cnt[i]);
        }
        
        int count = 0;
        while (!queue.isEmpty()) {
            if (n < queue.size()) {
                List<Integer> list = new ArrayList<>();
                for (int i = 0; i <= n; i++) {
                    int top = queue.poll();
                    if (top > 1) list.add(top - 1);
                }
                count += (n + 1);
                queue.addAll(list);   
            } else {
                int top = queue.poll();
                count += (n + 1)  * (top - 1);
                int addition = 1;
                while (!queue.isEmpty() && queue.poll() == top) addition++;
                count += addition;
                return count;
            }
        }
        return count;
    }
}
```

看一看这个问题的解法三，问题的本质就是计算idle slots的数量，你所设想的那种情况，压根儿就没有idle slots

[https://leetcode.com/problems/task-scheduler/solution/](https://leetcode.com/problems/task-scheduler/solution/)

