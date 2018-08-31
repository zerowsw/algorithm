---
description: 做过很多次的题目了，打个卡
---

# Clone Graph

## Description

Clone an undirected graph. Each node in the graph contains a `label` and a list of its `neighbors`.  
**OJ's undirected graph serialization:**

Nodes are labeled uniquely.We use `#` as a separator for each node, and `,` as a separator for node label and each neighbor of the node.

As an example, consider the serialized graph `{0,1,2#1,2#2,2}`.

The graph has a total of three nodes, and therefore contains three parts as separated by `#`.

1. First node is labeled as `0`. Connect node `0` to both nodes `1` and `2`.
2. Second node is labeled as `1`. Connect node `1` to node `2`.
3. Third node is labeled as `2`. Connect node `2` to node `2` \(itself\), thus forming a self-cycle.

Visually, the graph looks like the following:

```text
       1
      / \
     /   \
    0 --- 2
         / \
         \_/
```

## Solution

相当经典的题目，我也直接明了地知道应该分成两步，第一步复制所有的节点，第二步复制所有的link。

不过，我卡住了一会儿， 因为复制节点时需要遍历图，而我的印象中，没有这么麻烦。

ConcurrentMotificationException,  不知道为啥。。

麻辣个鸡，我忘了copy点了。。, 忘了!queue.isEmpty\(\)

如下：

```java
/**
 * Definition for undirected graph.
 * class UndirectedGraphNode {
 *     int label;
 *     List<UndirectedGraphNode> neighbors;
 *     UndirectedGraphNode(int x) { label = x; neighbors = new ArrayList<UndirectedGraphNode>(); }
 * };
 */
public class Solution {
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        if (node == null) return node;
        List<UndirectedGraphNode> nodes = getNodes(node);
        Map<Integer, UndirectedGraphNode> map = new HashMap<>();
        for (UndirectedGraphNode no : nodes) {
            map.put(no.label, new UndirectedGraphNode(no.label));
        }
        
        for (UndirectedGraphNode no : nodes) {
            List<UndirectedGraphNode> neighbors = no.neighbors;
            UndirectedGraphNode copynode = map.get(no.label);
            for (UndirectedGraphNode nei : neighbors) {
                copynode.neighbors.add(map.get(nei.label));
            }
        }
        return map.get(node.label);
    }
    
    public List<UndirectedGraphNode> getNodes(UndirectedGraphNode node) {
        Queue<UndirectedGraphNode> queue = new LinkedList<>();
        Set<UndirectedGraphNode> set = new HashSet<>();
        queue.offer(node);
        set.add(node);
        while (!queue.isEmpty()) {
            UndirectedGraphNode cur = queue.poll();
            for (UndirectedGraphNode no : cur.neighbors) {
                if (!set.contains(no)) {
                    queue.offer(no);
                    set.add(no);
                }
            }
        }
        return new ArrayList<UndirectedGraphNode>(set);
    }
}
```

其实，可以边复制点，边构建边，既节约时间，代码又简洁：

```java
/**
 * Definition for undirected graph.
 * class UndirectedGraphNode {
 *     int label;
 *     List<UndirectedGraphNode> neighbors;
 *     UndirectedGraphNode(int x) { label = x; neighbors = new ArrayList<UndirectedGraphNode>(); }
 * };
 */
public class Solution {
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        if (node == null) return node;
        Map<UndirectedGraphNode, UndirectedGraphNode> graph = new HashMap<>();
        Set<UndirectedGraphNode> set = new HashSet<>();
        graph.put(node, new UndirectedGraphNode(node.label));
        Queue<UndirectedGraphNode> queue = new LinkedList<>();
        queue.offer(node);
        set.add(node);
        
        while (!queue.isEmpty()) {
            UndirectedGraphNode cur = queue.poll();
            UndirectedGraphNode copycur = graph.get(cur);
            for (UndirectedGraphNode next : cur.neighbors) {
                if (graph.containsKey(next)) {
                    copycur.neighbors.add(graph.get(next));
                } else {
                    UndirectedGraphNode copynext = new UndirectedGraphNode(next.label);
                    graph.put(next, copynext);
                    copycur.neighbors.add(copynext);
                }
                if (set.contains(next)) continue;
                set.add(next);
                queue.offer(next);
            }
        }
        return graph.get(node);
    }
}
```

