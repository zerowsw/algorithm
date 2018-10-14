---
description: 状态压缩BFS（Dijkstra）
---

# Race Car

## Description

Your car starts at position 0 and speed +1 on an infinite number line.  \(Your car can go into negative positions.\)

Your car drives automatically according to a sequence of instructions A \(accelerate\) and R \(reverse\).

When you get an instruction "A", your car does the following: `position += speed, speed *= 2`.

When you get an instruction "R", your car does the following: if your speed is positive then `speed = -1` , otherwise `speed = 1`.  \(Your position stays the same.\)

For example, after commands "AAR", your car goes to positions 0-&gt;1-&gt;3-&gt;3, and your speed goes to 1-&gt;2-&gt;4-&gt;-1.

Now for some target position, say the **length** of the shortest sequence of instructions to get there.

## Example

```text
Example 1:
Input: 
target = 3
Output: 2
Explanation: 
The shortest instruction sequence is "AA".
Your position goes from 0->1->3.
```

```text
Example 2:
Input: 
target = 6
Output: 5
Explanation: 
The shortest instruction sequence is "AAARA".
Your position goes from 0->1->3->7->7->6.
```

## Solution

Ok, 其实吧，这种求最短的问题，一般来讲就有两种方法，BFS或者DP

来试试吧:

OK, 超时了，49／53个test case的时候超时了。。\(起码要做一些剪枝才可以\)

```text
class Solution {
    private class State{
        int position;
        int speed;
        public State(int position, int speed) {
            this.position = position;
            this.speed = speed;
        }
        
        public int hashCode() {
            //这里注意一下怎么写hashCode function
            int result = 17;
            result = result * 31 + position;
            result = result * 31 + speed;
            return result;
        }
        
        public boolean equals(Object o) {
            if (o == this) return true;
            if (!(o instanceof State)) {
                return false;
            }
            State obj = (State) o;
            return this.position == obj.position && this.speed == obj.speed;
        }
    }
    
    
    public int racecar(int target) {
        if (target == 0) return 0;
        Queue<State> queue = new LinkedList<>();
        Set<State> visited = new HashSet<>();
        State initial = new State(0, 1);
        queue.offer(initial);
        visited.add(initial);
        int instruction = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            while (size > 0) {
                State cur = queue.poll();
                if (cur.position == target) return instruction;
                State A = new State(cur.position + cur.speed, cur.speed * 2);
                State R = new State(cur.position, cur.speed > 0 ? -1 : 1);
                if (!visited.contains(A)) {
                    queue.offer(A);
                    visited.add(A);
                }
                if (!visited.contains(R)) {
                    queue.offer(R);
                    visited.add(R);
                }
                size--;
            }
            instruction++;
        }
        return -1;
    }
}
```

其实这道题，他真正优化的点在于分析问题之后，对搜索状态进行了压缩。

这道题我打算看看花花的视频



