# Word Break II

## Description

Given a **non-empty** string _s_ and a dictionary _wordDict_ containing a list of **non-empty** words, add spaces in _s_ to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.

**Note:**

* The same word in the dictionary may be reused multiple times in the segmentation.
* You may assume the dictionary does not contain duplicate words.

## Example

**Example 1:**

```text
Input:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output:
[
  "cats and dog",
  "cat sand dog"
]
```

**Example 2:**

```text
Input:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
Output:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
Explanation: Note that you are allowed to reuse a dictionary word.
```

**Example 3:**

```text
Input:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output:
[]
```

## Solution

首先嘛，DFS肯定是可以做的。。

其次，如果拿DP的话，感觉每个位置都得存一个List&lt;List&lt;String&gt;&gt;？还有啥更好的表示方法嘛？

当然啦，应该是可以用DFS with memory来做的。。

看了一下答案，和我想的一摸一样，DP就不写了，没啥特别的，写一个**Recursion with memoization**

另外，这道题，DP的方法内存溢出了。

此外，这道题的时空复杂度需要我好好地想想， O\(n ^ 3\) ,  O\(n ^ 3\)

```java
class Solution {
    public List<String> wordBreak(String s, List<String> wordDict) {
        Set<String> set = new HashSet<>(wordDict);
        //if (s == null) return new ArrayList<>();
        Map<Integer, List<String>> cache = new HashMap<>();
        List<String> result = helper(s, set, 0, cache);
        return result;
    }
    
    private List<String> helper(String s, Set<String> wordDict, int start, Map<Integer, List<String>> cache) {
        if (cache.containsKey(start)) {
            return cache.get(start);
        }
        List<String> result = new ArrayList<>();
        if (start >= s.length()) return null;
        for (int i = start + 1; i <= s.length(); i++) {
            if (wordDict.contains(s.substring(start, i))) {
                String addition = s.substring(start, i);
                List<String> res = helper(s, wordDict, i, cache);
                if (res != null) { 
                    for (String str : res) {
                        result.add(addition + " " + str);
                    }
                } else {
                    result.add(addition);
                }
            }
        }
        cache.put(start, result);
        return result;
    }
}
```





