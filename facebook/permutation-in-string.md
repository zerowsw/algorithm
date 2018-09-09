# Permutation in String

## Description

Given two strings **s1** and **s2**, write a function to return true if **s2** contains the permutation of **s1**. In other words, one of the first string's permutations is the **substring** of the second string.

## Example

**Example 1:**

```text
Input:s1 = "ab" s2 = "eidbaooo"
Output:True
Explanation: s2 contains one permutation of s1 ("ba").
```

**Example 2:**

```text
Input:s1= "ab" s2 = "eidboaoo"
Output: False
```

## Solution

妈蛋，有点懵逼，，感觉应该用sliding window，并且topic也是two pointer。。

关键是怎么用呢。。

罢了，先去看会儿亚麻OA吧

傻了嘛，这个长度是固定的呀，只要保证长度为K，然后两跟指针同时移动就可以了。

写了试试吧。

debug的时候被卡住了，搞错了什么时候应该加count。。

时间复杂度：O\(l2 - l1 + l2\),  空间复杂度可是说是O\(1\), 用了一个26大小的数组。

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int[] hash = new int[26];
        for (char chr : s1.toCharArray()) {
            hash[chr - 'a']++;
        }
        int k = s1.length();
        int count = k;
        int le = 0;
        int ri = 0;
        char[] array2 = s2.toCharArray();
        while (ri < array2.length) {
            if (ri - le < k && hash[array2[ri++] - 'a']-- > 0) {
                count--;
            } 
            if(ri - le == k && count == 0) return true;
            //注意不是等于0的时候才加，我日了。。。
            if (ri - le == k && hash[array2[le++] - 'a']++ >= 0) {
                count++;
            }
        }
        return false;
    }
}
```



