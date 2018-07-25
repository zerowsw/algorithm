# Ugly Number II

## Description

Ugly number is a number that only have factors `2`, `3` and `5`.

Design an algorithm to find the _n_th ugly number. The first 10 ugly numbers are `1, 2, 3, 4, 5, 6, 8, 9, 10, 12...`

## Example

If `n=9`, return `10`.

## Challenge

O\(_n_ log _n_\) or O\(_n_\) time.

## Solution

我居然还有印象，从前往后，逐步往后算。

怎么能从小到大往后推呢？ 该死，，明明做过。。

解法一，我去，，，这是最正常的想法，我想出来了，结果脑抽了。。

```java
public class Solution {
    /**
     * @param n: An integer
     * @return: the nth prime number as description.
     */
    public int nthUglyNumber(int n) {
        // write your code here
        Queue<Long> queue = new PriorityQueue<>();
        Set<Long> set = new HashSet<>();
        
        long[] prime = new long[3];
        prime[0] = 2;
        prime[1] = 3;
        prime[2] = 5;
        
        for (int i = 0; i < 3; i++) {
            queue.offer(prime[i]);
            set.add(prime[i]);
        }
    
        long number = 1;
        for (int i = 1; i < n; i++) {
            number = queue.poll();
            for (int j = 0; j < 3; j++) {
                if (set.contains(number * prime[j])) continue;
                queue.offer(number * prime[j]);
                set.add(number * prime[j]);
            }
        }
        return (int)number;
    }
}
```

解法二，保存三个指针分别对应2，3，5的乘数，然后，每次确保往前推一个最小值，同时确定指针如何移动。

```java
public class Solution {
    /**
     * @param n: An integer
     * @return: the nth prime number as description.
     */
    public int nthUglyNumber(int n) {
        // write your code here
        List<Integer> ugly = new ArrayList<>();
        ugly.add(1);
        int p2 = 0;
        int p3 = 0;
        int p5 = 0;
        
        for (int i = 1; i < n; i++) {
            int lastone = ugly.get(i - 1);
            if (ugly.get(p2) * 2 <= lastone) p2++;
            if (ugly.get(p3) * 3 <= lastone) p3++;
            if (ugly.get(p5) * 5 <= lastone) p5++;
            ugly.add(Math.min(Math.min(ugly.get(p2) * 2, ugly.get(p3) * 3) , ugly.get(p5) * 5));
         }
        return ugly.get(ugly.size() - 1);
    }
}
```

