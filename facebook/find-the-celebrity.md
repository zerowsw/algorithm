---
description: 逻辑上很巧妙，这种题目挺有意思的。
---

# Find the Celebrity

## Description

Suppose you are at a party with `n` people \(labeled from `0` to `n - 1`\) and among them, there may exist one celebrity. The definition of a celebrity is that all the other `n - 1` people know him/her but he/she does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of whether A knows B. You need to find out the celebrity \(or verify there is not one\) by asking as few questions as possible \(in the asymptotic sense\).

You are given a helper function `bool knows(a, b)` which tells you whether A knows B. Implement a function `int findCelebrity(n)`, your function should minimize the number of calls to `knows`.

**Note**: There will be exactly one celebrity if he/she is in the party. Return the celebrity's label if there is a celebrity in the party. If there is no celebrity, return `-1`.

## Solution

这道题果然做过。。

整理一下思路啊，首先，Brute Force的方法，肯定就不用考虑了。

然后，可以感觉的出来他们构成了一张graph，celebrity就是其他所有点可达的终点。

可以从左到右找到候选终点之后，再加上少量的判断即可。。。。。。。（呵呵，脑残了。。）

你是啥的嘛。。怎么可能有两个终点，不存在一个以上的出度为0的点，他们相互之间也不认识，这种情况下，压根儿就不存在celebrity了。

Two-pass的解法有逻辑上的充分必要的推理，如果有celebrity，只会有一个。。。啥啥啥的，你自己看吧。。。。。。

```java
/* The knows API is defined in the parent class Relation.
      boolean knows(int a, int b); */

public class Solution extends Relation {
    public int findCelebrity(int n) {
        
        int celebrity = 0;
        //这个celebrity的点如果存在的话，一定会被这样替换到
        for (int i = 1; i < n; i++) {
            if (knows(celebrity, i)) celebrity = i;
        }
        
        for (int i = 0; i < n; i++) {
            if (i != celebrity && (!knows(i, celebrity) || knows(celebrity, i))) return -1;
        }
        return celebrity;
    }
}
```



