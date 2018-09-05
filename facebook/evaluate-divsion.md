# Evaluate Divsion

## Description

Equations are given in the format `A / B = k`, where `A` and `B` are variables represented as strings, and `k` is a real number \(floating point number\). Given some queries, return the answers. If the answer does not exist, return `-1.0`.

## Example

Given `a / b = 2.0, b / c = 3.0.`   
queries are: `a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? .`   
return `[6.0, 0.5, -1.0, 1.0, -1.0 ].`

The input is: `vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries`, where `equations.size() == values.size()`, and the values are positive. This represents the equations. Return `vector<double>`.

According to the example above:

```text
equations = [ ["a", "b"], ["b", "c"] ],
values = [2.0, 3.0],
queries = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ]. 
```

The input is always valid. You may assume that evaluating the queries will result in no division by zero and there is no contradiction.

## Solution

比较直接的解法就是构建双向Graph， 然后用DFS， edge包含有单向的值。

卡在了一个小点上: 用DFS的时候如果得到最终的计算结果。。（用全局的变量？）

好了，这是我的解答，你们这些凡人，好好看看：

```java
class Solution {
    
    //definition of graph node
    private class Node{
        String node;
        Map<Node, Double> edge;
        public Node(String val) {
            node = val;
            edge = new HashMap<>();
        }    
    }
    
    private double res = -1.0;
    
    public double[] calcEquation(String[][] equations, double[] values, String[][] queries) {
        Map<String, Node> graph = new HashMap<>();     
        for (int i = 0; i < equations.length; i++) {
            String a = equations[i][0];
            String b = equations[i][1];
            Node start = graph.computeIfAbsent(a, x -> new Node(a));
            Node end = graph.computeIfAbsent(b, x -> new Node(b));
            start.edge.put(end, values[i]);
            end.edge.put(start, 1/values[i]);
        }
        double[] result = new double[queries.length];
        for (int i = 0; i < queries.length; i++) {
            result[i] = query(graph, queries[i]);
            res = -1.0;
        }
        return result;
    }
    
    public double query(Map<String, Node> graph, String[] query) {
        String a = query[0];
        String b = query[1];
        if (!graph.containsKey(a) || !graph.containsKey(b)) return -1.0;
        Set<Node> set = new HashSet<>();
        set.add(graph.get(a));
        helper(graph.get(a), set, graph.get(b), 1.0);
        return res;
    }
    
    private void helper(Node root, Set<Node> set, Node target, double val) {
        if (root == target) {
            res = val;
            return;
        }
        for (Node next : root.edge.keySet()) {
            if (set.contains(next)) continue;
            set.add(next);
            helper(next, set, target, val * root.edge.get(next));
            set.remove(next);
        }
    }
}
```

