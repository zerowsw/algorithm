# Find Eventual Safe State

## Description

In a directed graph, we start at some node and every turn, walk along a directed edge of the graph.  If we reach a node that is terminal \(that is, it has no outgoing directed edges\), we stop.

Now, say our starting node is _eventually safe_ if and only if we must eventually walk to a terminal node.  More specifically, there exists a natural number `K` so that for any choice of where to walk, we must have stopped at a terminal node in less than `K` steps.

Which nodes are eventually safe?  Return them as an array in sorted order.

The directed graph has `N` nodes with labels `0, 1, ..., N-1`, where `N`is the length of `graph`.  The graph is given in the following form: `graph[i]` is a list of labels `j` such that `(i, j)` is a directed edge of the graph.

```text
Example:
Input: graph = [[1,2],[2,3],[5],[0],[5],[],[]]
Output: [2,4,5,6]
Here is a diagram of the above graph.

```

![Illustration of graph](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/17/picture1.png)

**Note:**

* `graph` will have length at most `10000`.
* The number of edges in the graph will not exceed `32000`.
* Each `graph[i]` will be a sorted list of different integers, chosen within the range `[0, graph.length - 1]`.

## Solution

很巧妙的做法，从final state出发的，可以算作topologic sort的方法吧：

```java
class Solution {
    public List<Integer> eventualSafeNodes(int[][] tgraph) {
        Map<Integer, Set<Integer>> graph = new HashMap<>();
        Map<Integer, Set<Integer>> regraph = new HashMap<>();
        //Map<Integer, Integer> indegree = new HashMap<>();
        
        for (int i = 0; i < tgraph.length; i++) {
            graph.put(i, new HashSet<>());
            regraph.put(i, new HashSet<>());
        }
        
        Queue<Integer> queue = new LinkedList<>();
        
        for (int i = 0; i < tgraph.length; i++) {
            if (tgraph[i].length == 0) {
                queue.offer(i);
            }
            for (int next : tgraph[i]) {
                graph.get(i).add(next);
                regraph.get(next).add(i);
            }
        }
        
        List<Integer> result = new LinkedList<>();
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            result.add(cur);
            for (int reverse : regraph.get(cur)) {
                Set<Integer> nei = graph.get(reverse);
                nei.remove(cur);
                if (nei.size() == 0) {
                    queue.offer(reverse);
                }
            }
        }
        Collections.sort(result);
        return result;
    }
}
```

另外一种解法，detect cycle in a directed graph using color:



