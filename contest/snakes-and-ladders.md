# Snakes and Ladders

## Description

On an N x N `board`, the numbers from `1` to `N*N` are written _boustrophedonically_ **starting from the bottom left of the board**, and alternating direction each row.  For example, for a 6 x 6 board, the numbers are written as follows:

```text

```

You start on square `1` of the board \(which is always in the last row and first column\).  Each move, starting from square `x`, consists of the following:

* You choose a destination square `S` with number `x+1`, `x+2`, `x+3`, `x+4`, `x+5`, or `x+6`, provided this number is `<= N*N`.
  * \(This choice simulates the result of a standard 6-sided die roll: ie., there are always at most 6 destinations.\)
* If `S` has a snake or ladder, you move to the destination of that snake or ladder.  Otherwise, you move to `S`.

A board square on row `r` and column `c` has a "snake or ladder" if `board[r][c] != -1`.  The destination of that snake or ladder is `board[r][c]`.

Note that you only take a snake or ladder at most once per move: if the destination to a snake or ladder is the start of another snake or ladder, you do **not** continue moving.  \(For example, if the board is \`\[\[4,-1\],\[-1,3\]\]\`, and on the first move your destination square is \`2\`, then you finish your first move at \`3\`, because you do **not** continue moving to \`4\`.\)

Return the least number of moves required to reach square N\*N.  If it is not possible, return `-1`.

**Example 1:**

```text
Input: [
[-1,-1,-1,-1,-1,-1],
[-1,-1,-1,-1,-1,-1],
[-1,-1,-1,-1,-1,-1],
[-1,35,-1,-1,13,-1],
[-1,-1,-1,-1,-1,-1],
[-1,15,-1,-1,-1,-1]]
Output: 4
Explanation: 
At the beginning, you start at square 1 [at row 5, column 0].
You decide to move to square 2, and must take the ladder to square 15.
You then decide to move to square 17 (row 3, column 5), and must take the snake to square 13.
You then decide to move to square 14, and must take the ladder to square 35.
You then decide to move to square 36, ending the game.
It can be shown that you need at least 4 moves to reach the N*N-th square, so the answer is 4.
```

**Note:**

1. `2 <= board.length = board[0].length <= 20`
2. `board[i][j]` is between `1` and `N*N` or is equal to `-1`.
3. The board square with number `1` has no snake or ladder.
4. The board square with number `N*N` has no snake or ladder.

## Solution

我的想法就是DFS + Cache，然后并没有成功解出来, 过了170个test case之后就卡住了。

突然发现，我他么看错题目了。。我日。。

如果是连跳的情况，不是就不能走那条路了，而是说跳了一次之后，就停在那儿了。。

卧槽，还是debug不出来。。

```java
class Solution {
    
    public int snakesAndLadders(int[][] board) {
        if (board == null || board.length == 0 || board[0].length == 0) return -1;
         int N = board.length;
        int[] cache = new int[N * N + 1];
        //boolean[] visited = new boolean[N * N + 1];
        return helper(board, cache, 1, N, new boolean[N * N + 1]);
    }
    
    public int helper(int[][] board, int[] cache, int current, int N, boolean[] visited) {
        if (cache[current] != 0) {
            return cache[current];
        }
        if (current == N * N) {
            return 0;
        }
        if (visited[current]) return -1;
        
        int min = Integer.MAX_VALUE;  
        int[] currentPosition = getPosition(N, current);
        int x = currentPosition[0];
        int y = currentPosition[1];
        if (board[x][y] != -1) {
            current = board[x][y];
        }
        if (current == N * N) {
            return 0;
        }
        visited[current] = true;
          
        for (int i = 6; i >= 1; i--) {
            int nextnum = current + i;
            if (nextnum > N * N) continue;
            if (nextnum == N * N) {
                return 0;
            }
            int step = helper(board, cache, nextnum, N, visited);
            if (step != -1) {
                min = Math.min(min, step + 1);
            }   
        }
        cache[current] = (min == Integer.MAX_VALUE) ? -1 : min;
        return cache[current];
    }
    
    public int[] getPosition(int N, int number) {
        int[] p = new int[2];
        int x = N - 1 - (number - 1) / N;
        int y = ((number - 1) / N) % 2 == 0 ? (number - 1) % N : N - 1 - (number - 1) % N;
        p[0] = x;
        p[1] = y;
        return p;
    }
}
```

然后我发现他们基本都是拿BFS做的。。，讲道理，不是不可以，关键是我想看看他们是怎么处理跳两步这个情况的，我的处理方法挺愚蠢的。。

卧槽，你蠢的嘛，你求最短跳跃路径，当然从起点开始做BFS更为直观啊。。

不懂你脑子里在想些啥。。仔细再一想，真的直观很多诶。。

```java
class Solution {
    public int snakesAndLadders(int[][] board) {
        Map<Integer, Integer> map = new HashMap<>();
        map.put(1, 0);
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(1);
        int N = board.length;
        
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            if (cur == N * N) return map.get(cur);
            
            for (int i = 1; i <= 6; i++) {
                if (cur + i > N * N) break;
                int[] p = getPosition(N, cur + i);
                int finalcur = board[p[0]][p[1]] == -1 ? cur + i : board[p[0]][p[1]];
                if (!map.containsKey(finalcur)) {
                    map.put(finalcur, map.get(cur) + 1);
                    queue.offer(finalcur);
                }
            }
        }
        return -1;
    }
    
    public int[] getPosition(int N, int number) {
        int[] p = new int[2];
        int x = N - 1 - (number - 1) / N;
        int y = ((number - 1) / N) % 2 == 0 ? (number - 1) % N : N - 1 - (number - 1) % N;
        p[0] = x;
        p[1] = y;
        return p;
    }
}
```





