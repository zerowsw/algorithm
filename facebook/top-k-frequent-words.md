# Top K Frequent Words

## Description

Given a non-empty list of words, return the k most frequent elements.

Your answer should be sorted by frequency from highest to lowest. If two words have the same frequency, then the word with the lower alphabetical order comes first.

## Example

**Example 1:**  


```text
Input: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
Output: ["i", "love"]
Explanation: "i" and "love" are the two most frequent words.
    Note that "i" comes before "love" due to a lower alphabetical order.
```

**Example 2:**  


```text
Input: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
Output: ["the", "is", "sunny", "day"]
Explanation: "the", "is", "sunny" and "day" are the four most frequent words,
    with the number of occurrence being 4, 3, 2 and 1 respectively.
```

## Note & Follow up

**Note:**

1. You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
2. Input words contain only lowercase letters.

**Follow up:**

1. Try to solve it in O\(n log k\) time and O\(n\) extra space.

## Solution

感觉follow up给了我一些提示，先用HashMap统计词频。然后就是一个普通的top K的问题。

和标准答案的解答完全一致，注意heap的大小关系\(词频最小堆，字母顺序最大堆\)，以及倒出到list时候的大小关系。

注意compareTo\(\)的用法。

```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> map = new HashMap<>();
        for (String word : words) {
            map.put(word, map.getOrDefault(word, 0) + 1);
        }
        
        PriorityQueue<Map.Entry<String, Integer>> queue = new PriorityQueue<>(new Comparator<Map.Entry<String, Integer>>(){
           public int compare(Map.Entry<String, Integer> a, Map.Entry<String, Integer> b) {
               return a.getValue() == b.getValue() ? b.getKey().compareTo(a.getKey()) : a.getValue() - b.getValue();
           } 
        });
        
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            queue.offer(entry);
            if (queue.size() > k) queue.poll();
        }
        List<String> res = new ArrayList<>();
        while (!queue.isEmpty()) {
            res.add(queue.poll().getKey());
        }
        Collections.reverse(res);
        return res;
    }
}
```

看一看答案的写法，可以不存Map.Entry:

```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> count = new HashMap();
        for (String word: words) {
            count.put(word, count.getOrDefault(word, 0) + 1);
        }
        PriorityQueue<String> heap = new PriorityQueue<String>(
                (w1, w2) -> count.get(w1).equals(count.get(w2)) ?
                w2.compareTo(w1) : count.get(w1) - count.get(w2) );

        for (String word: count.keySet()) {
            heap.offer(word);
            if (heap.size() > k) heap.poll();
        }

        List<String> ans = new ArrayList();
        while (!heap.isEmpty()) ans.add(heap.poll());
        Collections.reverse(ans);
        return ans;
    }
}
```



