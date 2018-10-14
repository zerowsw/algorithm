# Random Pick with Blacklist

## inDescription

Given a blacklist `B` containing unique integers from `[0, N)`, write a function to return a uniform random integer from `[0, N)` which is **NOT** in `B`.

Optimize it such that it minimizes the call to system’s `Math.random()`.

**Note:**

1. `1 <= N <= 1000000000`
2. `0 <= B.length < min(100000, N)`
3. `[0, N)` does NOT include N. See [interval notation](https://en.wikipedia.org/wiki/Interval_%28mathematics%29).

**Example 1:**

```text
Input: 
["Solution","pick","pick","pick"]
[[1,[]],[],[],[]]
Output: [null,0,0,0]
```

**Example 2:**

```text
Input: 
["Solution","pick","pick","pick"]
[[2,[]],[],[],[]]
Output: [null,1,1,1]
```

**Example 3:**

```text
Input: 
["Solution","pick","pick","pick"]
[[3,[1]],[],[],[]]
Output: [null,0,0,2]
```

**Example 4:**

```text
Input: 
["Solution","pick","pick","pick"]
[[4,[2]],[],[],[]]
Output: [null,1,3,1]
```

**Explanation of Input Syntax:**

The input is two lists: the subroutines called and their arguments. `Solution`'s constructor has two arguments, `N` and the blacklist `B`. `pick` has no arguments. Arguments are always wrapped with a list, even if there aren't any.

## Solution

首先，比较简单的方法就是创建一个whitelist, 然后再从里面随机取，创建时的时间复杂度为O\(n\), 会超时，同时空间复杂度为O\(n\), 也超出空间使用限制。

卧槽，binary search的方法很Tricky啊，首先我们可以很简单的之后whitelist的大小，然后获取一个whitelist size范围的随机数， 然后。。。。。。。反正就那么个意思，你自己看代码吧：

你他么，，什么叫就那么个意思，而且这个方法的时空复杂度也不是最优的吧。。

```java
class Solution {

    int n;
    int[] b;
    Random r;

    public Solution(int N, int[] blacklist) {
        n = N;
        Arrays.sort(blacklist);
        b = blacklist;
        r = new Random();
    }

    public int pick() {
        int k = r.nextInt(n - b.length);
        int lo = 0;
		int hi = b.length - 1;

		while (lo < hi) {
			int i = (lo + hi + 1) / 2;
			if (b[i] - i > k) hi = i - 1;
			else lo = i;
		}
		return lo == hi && b[lo] - lo <= k ? k + lo + 1 : k;
    }
}
```

这个Binary Search的方法不太好想哦，有个更好的方法，假设一共N个数，blacklist的size为B， 那么whitelist的长度就是N - B,  然后，将出现在白名单范围内的黑名单数字map到后半部分的whitelist数字上面即可。

```java
class Solution {
    int N;
    Random rand;
    Map<Integer, Integer> map;
    int wlen;
    
    public Solution(int N, int[] blacklist) {
        this.N = N;
        this.rand = new Random();
        map = new HashMap<>();
        Set<Integer> w = new HashSet<>();
        this.wlen = N - blacklist.length;
        for (int i = N - blacklist.length; i < N; i++) {
            w.add(i);
        }
        for (int num : blacklist) {
            w.remove(num);
        }
        Iterator<Integer> it = w.iterator();
        for (int num : blacklist) {
            if (num < N - blacklist.length) {
                map.put(num, it.next());
            }
        }   
    }
    
    public int pick() {
        int r = rand.nextInt(wlen);
        return map.getOrDefault(r, r);
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(N, blacklist);
 * int param_1 = obj.pick();
 */
```

