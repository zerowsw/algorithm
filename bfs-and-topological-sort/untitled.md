# Course Schedule

## Description

There are a total of n courses you have to take, labeled from `0` to `n - 1`.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: \[0,1\]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

## Example

Given n = `2`, prerequisites = `[[1,0]]`  
Return `true`

Given n = `2`, prerequisites = `[[1,0],[0,1]]`  
Return `false`

## Solution

相当经典的题目，读完之后印入脑中的就是一幅层层相邻的拓扑图，非常简单直接的层序遍历的题目。分成以下几个步骤：

首先将所有prerequisites的pair读入，构成一张graph，同时统计所有入度为0的点所谓层序遍历的第一层。（这里突然有些卡住，忘了怎么统计比较简便）

然后就是层序遍历，结束后，看看是否有上不了的课。\(着了道，忘了需要修完所有先修课程之后才能开始修，也就是入度为0的时候才能进去队列（其实也就是加个判断而已）\)

其实可以用List\[\]来代替hashmap，并且我们也不需要所谓的set的来处理访问过的点。

```java
public class Solution {
    /*
     * @param numCourses: a total of n courses
     * @param prerequisites: a list of prerequisite pairs
     * @return: true if can finish all courses or false
     */
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // write your code here
        Map<Integer, List<Integer>> graph = new HashMap<>();
        int[] indegree = new int[numCourses];
        for (int[] pre : prerequisites) {
            indegree[pre[1]]++;
            if (graph.containsKey(pre[0])) {
                graph.get(pre[0]).add(pre[1]);
            } else {
                List<Integer> outedges = new ArrayList<>();
                outedges.add(pre[1]);
                graph.put(pre[0], outedges);
            }
        }
        
        //add 0 degree courses into the graph and set
        Queue<Integer> queue = new LinkedList<>();
        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                queue.add(i);
                set.add(i);
            }
        }
        
        //BFS
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int course = queue.poll();
                if (!graph.containsKey(course)) continue;
                List<Integer> toCourses = graph.get(course);
                for (Integer inte : toCourses) {
                    if (set.contains(inte)) continue;
                    indegree[inte]--;
                    if (indegree[inte] != 0) continue; //所有前修课程完成才能开始修习
                    set.add(inte);
                    queue.offer(inte);
                }
            }
        }
        return set.size() == numCourses;
    }
}
```



