---
description: 非常明显的Dijkstra算法的应用
---

# Network Delay Time

## Description

There are `N` network nodes, labelled `1` to `N`.

Given `times`, a list of travel times as **directed** edges `times[i] = (u, v, w)`, where `u` is the source node, `v` is the target node, and `w` is the time it takes for a signal to travel from source to target.

Now, we send a signal from a certain node `K`. How long will it take for all nodes to receive the signal? If it is impossible, return `-1`.

**Note:**

1. `N` will be in the range `[1, 100]`.
2. `K` will be in the range `[1, N]`.
3. The length of `times` will be in the range `[1, 6000]`.
4. All edges `times[i] = (u, v, w)` will have `1 <= u, v <= N` and `1 <= w <= 100`.

## Solution

没啥好说的，Dijkstra， 关键是怎么样用较为简洁的算法实现，并且正确地分析时间复杂度。

仔细思考一下这种实现方式，确实是符合dijkstra算法的，如果有迂回更新这种情况出现，那么它在后续的Heap poll\(\)的过程中一定是先出现的，把它加入到已构建的最近Set之后，那么后续重复的距离较长的就不需要考虑了。

这种Dijkstra的实现方式不同于Maze II的实现方法，可以对比一下。

看了下维基百科上面的伪代码， 果然最正统的方式还是用外带一下distance数组（同时可以加一个path数组来记录路径，这也是我当时学Dijkstra的时候，教的方法， 可以用Maze II做一下实验）。

```java
class Solution {
    public int networkDelayTime(int[][] times, int N, int K) {
        Map<Integer, List<int[]>> graph = new HashMap<>();
        for (int[] time : times) {
            List<int[]> list = graph.computeIfAbsent(time[0], k -> new ArrayList<int[]>());
            int[] target = {time[1], time[2]};
            list.add(target);
        }
        
        PriorityQueue<int[]> queue = new PriorityQueue<int[]>(
            (info1, info2) -> info1[1] - info2[1]);
        
        queue.offer(new int[]{K, 0});
        int distance = Integer.MIN_VALUE;
        Set<Integer> dist = new HashSet<>();
        int count = 0;
        int res = 0;
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            if (dist.contains(cur[0])) continue;
            dist.add(cur[0]);
            res = Math.max(cur[1], res);
            if (graph.containsKey(cur[0])) {
                for (int[] next : graph.get(cur[0])) {
                    if (!dist.contains(next[0])) {
                        queue.offer(new int[]{next[0], next[1] + cur[1]});
                    }
                }
            }
        }
        return dist.size() == N ? res : -1;
    }
}
```

时间空间复杂度的分析：

