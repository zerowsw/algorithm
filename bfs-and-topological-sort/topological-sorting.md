---
description: 这题嘛，练练手而已
---

# Topological Sorting

## Description

Given an directed graph, a topological order of the graph nodes is defined as follow:

* For each directed edge `A -> B` in graph, A must before B in the order list.
* The first node in the order can be any node in the graph with no nodes direct to it.

Find any topological order for the given graph.

## Example



For graph as follow:

![picture](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcThE9AgZZszyhwe0o9qpp3VyizdIj9kWwMY50HiQEysXvkSLsoZ)

The topological order can be:

```text
[0, 1, 2, 3, 4, 5]
[0, 2, 3, 1, 5, 4]
...
```

## Challenge

Can you do it in both BFS and DFS?

## Solution

这道题已经没啥意思了，就是拿个提交记录而已

```java
/**
 * Definition for Directed graph.
 * class DirectedGraphNode {
 *     int label;
 *     ArrayList<DirectedGraphNode> neighbors;
 *     DirectedGraphNode(int x) { label = x; neighbors = new ArrayList<DirectedGraphNode>(); }
 * };
 */

public class Solution {
    /*
     * @param graph: A list of Directed graph node
     * @return: Any topological order for the given graph.
     */
    public ArrayList<DirectedGraphNode> topSort(ArrayList<DirectedGraphNode> graph) {
        // write your code here
        Map<Integer, Integer> indegree = new HashMap<>();
        ArrayList<DirectedGraphNode> res = new ArrayList<>();
        
        for (DirectedGraphNode node : graph) {
            ArrayList<DirectedGraphNode> neighbors = node.neighbors;
            for (DirectedGraphNode nei : neighbors) {
                indegree.put(nei.label, indegree.getOrDefault(nei.label, 0) + 1);
            }
        }
        
        Queue<DirectedGraphNode> queue =new LinkedList<>();
        for (DirectedGraphNode node : graph) {
            if (!indegree.containsKey(node.label)) {
                queue.offer(node);
            }
        }
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                DirectedGraphNode cur = queue.poll();
                res.add(cur);
                ArrayList<DirectedGraphNode> neighbors = cur.neighbors;
                for (DirectedGraphNode nei : neighbors) {
                    indegree.put(nei.label, indegree.get(nei.label) - 1);
                    if (indegree.get(nei.label) == 0) {
                        queue.offer(nei);
                    }
                }
            }
        }
        return res;
    }
}
```

