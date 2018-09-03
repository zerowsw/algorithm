# Is Graph Bipartite?

## Description

Given an undirected `graph`, return `true` if and only if it is bipartite.

Recall that a graph is _bipartite_ if we can split it's set of nodes into two independent subsets A and B such that every edge in the graph has one node in A and another node in B.

The graph is given in the following form: `graph[i]` is a list of indexes `j` for which the edge between nodes `i` and `j` exists.  Each node is an integer between `0` and `graph.length - 1`.  There are no self edges or parallel edges: `graph[i]` does not contain `i`, and it doesn't contain any element twice.

## Example

```text
Example 1:
Input: [[1,3], [0,2], [1,3], [0,2]]
Output: true
Explanation: 
The graph looks like this:
0----1
|    |
|    |
3----2
We can divide the vertices into two groups: {0, 2} and {1, 3}.
```

```text
Example 2:
Input: [[1,2,3], [0,2], [0,1,3], [0,2]]
Output: false
Explanation: 
The graph looks like this:
0----1
| \  |
|  \ |
3----2
We cannot find a way to divide the set of nodes into two independent subsets.
```

## Solution

简单明了的二分图判断问题，离散数学学过，用染色法即可解决。

方法一目了然，然后，这道题的关键，是如何用简洁有效的方法来表述问题并解决。

注意输入格式是已经表示好的Graph，并不需要你额外构建graph

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int[] color = new int[graph.length];
        Arrays.fill(color, -1);
        
        for (int i = 0; i < graph.length; i++) {
            if (color[i] != -1) continue;
            Queue<Integer> queue = new LinkedList<>();
            color[i] = 0;
            queue.offer(i);
            while (!queue.isEmpty()) {
                int cur = queue.poll();
                int cl = color[cur];
                for (int nei : graph[cur]) {
                    if (color[nei] == -1) {
                        color[nei] = 1 - cl;
                        queue.offer(nei);
                    } else if (color[nei] == cl) {
                        return false;
                    }
                }
            }
        }
        return true;
    }
}
```

