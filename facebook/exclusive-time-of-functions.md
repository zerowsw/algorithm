# Exclusive Time of Functions

## Description

Given the running logs of **n** functions that are executed in a nonpreemptive single threaded CPU, find the exclusive time of these functions.

Each function has a unique id, start from **0** to **n-1**. A function may be called recursively or by another function.

A log is a string has this format : `function_id:start_or_end:timestamp`. For example, `"0:start:0"` means function 0 starts from the very beginning of time 0. `"0:end:0"` means function 0 ends to the very end of time 0.

Exclusive time of a function is defined as the time spent within this function, the time spent by calling other functions should not be considered as this function's exclusive time. You should return the exclusive time of each function sorted by their function id.

## Example

```text
Input:
n = 2
logs = 
["0:start:0",
 "1:start:2",
 "1:end:5",
 "0:end:6"]
Output:[3, 4]
Explanation:
Function 0 starts at time 0, then it executes 2 units of time and reaches the end of time 1. 
Now function 0 calls function 1, function 1 starts at time 2, executes 4 units of time and end at time 5.
Function 0 is running again at time 6, and also end at the time 6, thus executes 1 unit of time. 
So function 0 totally execute 2 + 1 = 3 units of time, and function 1 totally execute 4 units of time.
```

## Solution

首先，注意条件，**nonpreemptive single threaded，** 也就是说那种交叉的情况是不会出现的。 

这种况下，我会想要用Stack来处理，单仔细一看，好像单线地扫描也能处理？？（不能）

我的想法与标准答案不谋而合，需要一个额外的变量来记录当前的时间，或者说是之前的timestamp的时间。

其实，这样看的话，我不需要新建一个Log变量来记录timstamp, 也就是说tt，完全没有用到。

下面是没有简化之间的版本。

```text
class Solution {
    private class Log{
        int id;
        int timestamp;
        public Log(int id, int timestamp) {
            this.id = id;
            this.timestamp = timestamp;
        }
    }
    public int[] exclusiveTime(int n, List<String> logs) {
        int[] res = new int[n];
        int prev = 0;
        Stack<Log> stack = new Stack<>();
        for (int i = 0; i < logs.size(); i++) {
            String[] info = logs.get(i).split(":");
            int id = Integer.parseInt(info[0]);
            int timestamp = Integer.parseInt(info[2]);
            if (i == 0) prev = timestamp;
            if (info[1].equals("end")) {
                Log top = stack.pop();
                int tid = top.id;
                res[id] += (timestamp - prev + 1);
                prev = timestamp + 1;
            } else {
                if (!stack.isEmpty()) {
                    Log top = stack.peek();
                    int tid = top.id;
                    int tt = top.timestamp;
                    res[tid] += (timestamp - prev);
                }
                prev = timestamp;
                stack.push(new Log(id, timestamp));
            }
        }
        return res;
    }
}
```

进一步简化之后的版本：

```java
class Solution {
    public int[] exclusiveTime(int n, List<String> logs) {
        int[] res = new int[n];
        int prev = 0;
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < logs.size(); i++) {
            String[] info = logs.get(i).split(":");
            int id = Integer.parseInt(info[0]);
            int timestamp = Integer.parseInt(info[2]);
            if (i == 0) prev = timestamp;
            if (info[1].equals("end")) {
                stack.pop();
                res[id] += (timestamp - prev + 1);
                prev = timestamp + 1;
            } else {
                if (!stack.isEmpty()) {
                    int tid = stack.peek();
                    res[tid] += (timestamp - prev);
                }
                prev = timestamp;
                stack.push(id);
            }
        }
        return res;
    }
}
```

