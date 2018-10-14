# Minimize Max Distance to Gas Station

## Description

On a horizontal number line, we have gas stations at positions `stations[0], stations[1], ..., stations[N-1]`, where `N = stations.length`.

Now, we add `K` more gas stations so that **D**, the maximum distance between adjacent gas stations, is minimized.

Return the smallest possible value of **D**.

**Example:**

```text
Input: stations = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], K = 9
Output: 0.500000
```

**Note:**

1. `stations.length` will be an integer in range `[10, 2000]`.
2. `stations[i]` will be an integer in range `[0, 10^8]`.
3. `K` will be an integer in range `[1, 10^6]`.
4. Answers within `10^-6` of the true value will be accepted as correct.

## Solution

首先吧，先做一个比较straightforward的方法，那就是每次处理一下最长的interval，这里的处理， 是均分成几段。

超时：

```java
class Solution {
    public double minmaxGasDist(int[] stations, int K) {
        int N = stations.length;
        PriorityQueue<int[]> queue = new PriorityQueue<int[]>((x, y) -> (double) y[0] / y[1] < (double) x[0] / x[1] ? -1 : 1);
        for (int i = 1; i < stations.length; i++) {
            queue.offer(new int[]{stations[i] - stations[i - 1], 1});
        }
        
        for (int i = 0; i < K; i++) {
            int[] cur = queue.poll();
            cur[1]++;
            queue.offer(cur);
        }
        int[] top = queue.peek();
        return (double)top[0] / top[1];
    }
}
```

二分法，尝试找到最后的最优分割结果：

```java
class Solution {
    public double minmaxGasDist(int[] stations, int K) {
        double lo = 0;
        double hi = 1e8;
        while (lo + 1e-6 < hi) {
            double mid = lo + (hi - lo) / 2;
            if (check(stations, mid, K)) {
                hi = mid;
            } else {
                lo = mid;
            }
        }
        return lo;
    }
    
    private boolean check(int[] stations, double mid, int K) {
        int sum = 0;
        for (int i = 1; i < stations.length; i++) {
            sum += (stations[i] - stations[i - 1]) / mid;
        }
        return sum <= K;
     }
}
```

