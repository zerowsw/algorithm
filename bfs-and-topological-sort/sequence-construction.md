---
description: 这题真是跑了很多遍才过。。
---

# Sequence Construction

## Description

Check whether the original sequence `org` can be uniquely reconstructed from the sequences in `seqs`. The org sequence is a permutation of the integers from 1 to n, with 1 ≤ n ≤ 10^4. Reconstruction means building a shortest common supersequence of the sequences in `seqs` \(i.e., a shortest sequence so that all sequences in `seqs` are subsequences of it\). Determine whether there is only one sequence that can be reconstructed from `seqs` and it is the `org` sequence.

## Example

```text
Given org = [1,2,3], seqs = [[1,2],[1,3]]
Return false
Explanation:
[1,2,3] is not the only one sequence that can be reconstructed, because [1,3,2] is also a valid sequence that can be reconstructed.

Given org = [1,2,3], seqs = [[1,2]]
Return false
Explanation:
The reconstructed sequence can only be [1,2].

Given org = [1,2,3], seqs = [[1,2],[1,3],[2,3]]
Return true
Explanation:
The sequences [1,2], [1,3], and [2,3] can uniquely reconstruct the original sequence [1,2,3].

Given org = [4,1,5,2,6,3], seqs = [[5,2,6,3],[4,1,5,2]]
Return true
```

## Solution

在试图从seqs构建supersequence的过程中，就会发现这又是一个拓扑排序，或者说是course schedule II， 相当于再问有没有唯一的课程安排计划。 不过，处理seqs的时候又有些不一样，这里的关系不再是一对一的pair，需要稍微进行一下转化。

毛线了，这道题直接跪掉了。。 写得乱七八糟，然后还超时了。。呵呵

以下是超时的写法：

```java
public class Solution {
    /**
     * @param org: a permutation of the integers from 1 to n
     * @param seqs: a list of sequences
     * @return: true if it can be reconstructed only one or false
     */
    public boolean sequenceReconstruction(int[] org, int[][] seqs) {
        // write your code here    
        Map<Integer, Integer> indegree = new HashMap<>();
        //关键是要想到用这个数据结构，这样才能有效获取存储原本的数据
        Map<Integer, Set<Integer>> graph = new HashMap<>();
        Set<Integer> initial = new HashSet<>();
        Set<Integer> newget = new HashSet<>();
        
        for (Integer inte : org) {
            initial.add(inte);
        }
        
        for (int[] t : seqs) {
            int s = t.length;
            for (int i= 0; i < s; i++) {
                if (!initial.contains(t[i])) return false;
                newget.add(t[i]);
                if (!graph.containsKey(t[i])) {
                    Set<Integer> set = new HashSet<>();
                    graph.put(t[i], set);
                }
                Set<Integer> temp = graph.get(t[i]);
                for (int j = i + 1; j < s; j++) {
                    if (temp.contains(t[j])) continue;
                    temp.add(t[j]);
                    indegree.put(t[j], indegree.getOrDefault(t[j], 0) + 1);
                }
            }
        }
        
        for (Integer inte : org) {
            if (!newget.contains(inte)) return false;
        }
        
        Queue<Integer> queue = new LinkedList<>();
        for (Integer inte : initial) {
            if (!indegree.containsKey(inte)) {
                queue.add(inte);
            }
        }
        
        int count = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            if (size != 1) return false;
            int cur = queue.poll();
            if (org[count++] != cur) return false;
            Set<Integer> outer = graph.get(cur);
            for (Integer inte : outer) {
                indegree.put(inte, indegree.get(inte) - 1);
                if (indegree.get(inte) == 0) {
                    queue.offer(inte);
                }
            }
        } 
        return count == initial.size();
    }
}
```

重读了一遍题之后，发现之前有很重要的没有注意到的点。Org里面的元素从1到n，是所有可能出现的值。

同时也有一个非常重要的优化点，在统计出边的时候，不需要把sequence中所有位于当前位置之后的点都加入出边，只需要把邻近的后一个位置的点加入出边集合。（仔细想一想，首先完全重复的边已经通过set排除了，其次，当计算topo order走到当前位置的时候，非相邻点所带来的入度其实已经已经减去了。所以在统计过程中，这样的计算是重复性的。）（一开始理解题目的时候，还是走了歪路，其实想一想拓扑排序的图，就不会有那么奇怪的想法了）

最后的解答如下：

```java
public class Solution {
    /**
     * @param org: a permutation of the integers from 1 to n
     * @param seqs: a list of sequences
     * @return: true if it can be reconstructed only one or false
     */
    public boolean sequenceReconstruction(int[] org, int[][] seqs) {
        // write your code here
        
        Map<Integer, Integer> indegree = new HashMap<>();
        //关键是要想到用这个数据结构，这样才能有效获取存储原本的数据
        Map<Integer, Set<Integer>> graph = new HashMap<>();
        int n = org.length;
        
        for (int i=1; i <= n; i++) {
            indegree.put(i, 0);
            graph.put(i, new HashSet<>());
        }
        
        int cnt = 0;
        for (int[] t : seqs) {
            int s = t.length;
            cnt += s;
            if (s > n) return false;
            if (s >= 1 && (t[0] <= 0 || t[0] > n)) return false;
            for (int i= 1; i < s; i++) {
                if (t[i] <= 0 || t[i] > n) return false;
                if (!graph.containsKey(t[i])) {
                    Set<Integer> set = new HashSet<>();
                    graph.put(t[i], set);
                }
                if (graph.get(t[i - 1]).add(t[i])) {
                    indegree.put(t[i], indegree.get(t[i]) + 1);
                }
            }
        }
        
        //这个判断主要是处理空集的情况，唉，corner case是真的麻烦呀。。
        if (cnt < n) return false;
        
        Queue<Integer> queue = new LinkedList<>();
        for (Map.Entry<Integer, Integer> entry : indegree.entrySet()) {
            if (entry.getValue() == 0) {
                queue.add(entry.getKey());
            }
        }
        
        int count = 0;
        while (queue.size() == 1) {
            int cur = queue.poll();
            if (org[count++] != cur) return false;
            Set<Integer> outer = graph.get(cur);
            for (Integer inte : outer) {
                indegree.put(inte, indegree.get(inte) - 1);
                if (indegree.get(inte) == 0) {
                    queue.offer(inte);
                }
            }
        }
        return count == org.length;
    }
}
```



