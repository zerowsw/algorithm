---
description: DSU的写法！！！多练，多练！
---

# Number of Islands II

## Description

A 2d grid map of `m` rows and `n` columns is initially filled with water. We may perform an addLand operation which turns the water at position \(row, col\) into a land. Given a list of positions to operate, **count the number of islands after each addLand operation**. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

## Example

```text
Input: m = 3, n = 3, positions = [[0,0], [0,1], [1,2], [2,1]]
Output: [1,1,2,3]
```

## Follow up

Can you do it in time complexity O\(k log mn\), where k is the length of the `positions`?

## Solution

拿到手的一瞬间，有种无比强烈的使用Union Find来做的感觉。。让我来check一下topic，果然，是Union Find。

其实，在脑海中已经想好怎么用Union Find来做了，之前也学过，Union Find有很多种。我之前使用link形式的那种（**aka Disjoint Set**），感觉还挺方便的，觉着这里也可以用一下。

先看一下答案的解题方式吧，来和我自己的方法印证一下。

解答方案中国还有一个Ad hoc的方法，就是记录每一个operation涉及的位置所属的岛的编号，然后每次根据这个来修改相应的编号。

解答的方法好像用Rank来减少树的高度\(Union by Rank\)

我还是看看Discussion里面的高票答案吧。。稍微修改一下，定义我自己的Union Find解题思路。

我的解法：

```java
class Solution {
    //Disjoint Set Union
    private class DSU {
        int[] parents;
        public DSU(int m, int n) {
            parents = new int[m * n];
            Arrays.fill(parents, -1);
        }
        
        public int find(int m) {
            //好好看看这个写法
            if (parents[m] != m) parents[m] = find(parents[m]);
            return parents[m];
        }
        public void union(int m, int n) {
            //parents[m] = find(parents[n]);
            parents[find(m)] = find(n);
        }
    }
    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        
        DSU dsu = new DSU(m, n);
        int count = 0;
        List<Integer> res = new ArrayList<>();
        for (int[] position : positions) {
            int x = position[0];
            int y = position[1];
            int tag = x * n + y;
            dsu.parents[tag] = tag;
            count++;
            
            for (int[] dir : dirs) {
                int newx = x + dir[0];
                int newy = y + dir[1];
                if (newx < 0 || newy < 0 || newx >= m || newy >= n || dsu.parents[newx * n + newy] == -1) continue;
                //注意这里的写法
                if (dsu.find(tag) != dsu.find(newx * n + newy)) {
                    dsu.union(tag, newx * n + newy);
                    count--;
                }
            } 
            res.add(count);
        }
        return res;
    }
}
```



  


