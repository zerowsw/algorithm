# Word Search II

## Description

Given a matrix of lower alphabets and a dictionary. Find all words in the dictionary that can be found in the matrix. A word can start from any position in the matrix and go left/right/up/down to the adjacent position. One character only be used once in one word.

## Example

Given matrix:

```text
doaf
agai
dcan
```

and dictionary:

```text
{"dog", "dad", "dgdg", "can", "again"}
```

  
return {"dog", "dad", "can", "again"}  
  
dog:

```text
doaf
agai
dcan
```

dad:  


```text
doaf
agai
dcan
```

can:

```text
doaf
agai
dcan
```

again:

```text
doaf
agai
dcan
```

## Challenge

Using trie to implement your algorithm.

## Solution

就像提示所说，这道题用\(Prefix Tree）来做是比较合理的。

首先，我们排除遍历matrix找出所有可能的words的方法，时间复杂度太高，也可以是最brute force的方法了。

然后，直接遍历字典，到matrix中搜索的方法：这里自然而言想到先组织一下matrix中的数据，然而，然而，我居然忘了trie。。。。。。

首先，是我的空间复杂度过高的算法：

```java
public class Solution {
    /**
     * @param board: A list of lists of character
     * @param words: A list of string
     * @return: A list of string
     */
    
    int[][] direction = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}}; 
    private class TrieNode{ 
        char val;
        TrieNode[] next;
        TrieNode(char val) {
            this.val = val;
            next = new TrieNode[26];
        }
        
    }
     
    public List<String> wordSearchII(char[][] board, List<String> words) {
        // write your code here
        TrieNode[] trie = constructTrie(board);
        List<String> result = new ArrayList<>();
        for (String str : words) {
            if (!check(trie, str, 0)) continue;
            result.add(str);
        }
        return result;
    }
    
    private boolean check(TrieNode[] trie, String str, int index) {
        if (index == str.length()) return true;
        if (trie[str.charAt(index) - 'a'] == null) return false;
        return check(trie[str.charAt(index) - 'a'].next, str, index + 1);
    }
    
    private TrieNode[] constructTrie(char[][] board) {
        TrieNode[] trie = new TrieNode[26];
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                char cur = board[i][j];
                Set<String> visited = new HashSet<>();
                visited.add(i + "" + j);
                //we still have a lot to do
                if (trie[cur - 'a'] != null) {
                    traversal(board, visited, trie[cur - 'a'], i, j);
                } else {
                    TrieNode newNode = new TrieNode(cur);
                    trie[cur - 'a'] = newNode;
                    traversal(board, visited, newNode, i, j);
                }
            }
        }
        return trie;
    }
    
    private void traversal(char[][] board, Set<String> visited, TrieNode cur,int x, int y) { 
        for (int i = 0; i < 4; i++) {
            int newx = x + direction[i][0];
            int newy = y + direction[i][1];
            if (newx < 0 || newx >= board.length || newy < 0 || newy >= board[0].length) continue;
            if (visited.contains(newx+""+newy)) continue;
            //we still have 
            if (cur.next[board[newx][newy] - 'a'] != null) {
                visited.add(newx+""+newy);
                traversal(board, visited, cur.next[board[newx][newy] - 'a'],newx, newy);
                visited.remove(newx+""+newy);
            } else {
                cur.next[board[newx][newy] - 'a'] = new TrieNode(board[newx][newy]);
                visited.add(newx+""+newy);
                traversal(board, visited, cur.next[board[newx][newy] - 'a'], newx, newy);
                visited.remove(newx+""+newy);
            }
        }
    }
}
```

所以，明显是构造trie的方法不对。。没必要使用固定26大小的Node数组存储。 也不用纠结查询效率的问题，直接使用HashMap即可（居然忘了）。

好吧，还是超了。

```java
public class Solution {
    /**
     * @param board: A list of lists of character
     * @param words: A list of string
     * @return: A list of string
     */
    
    int[][] direction = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}};
    
    private class TrieNode{
        char val;
        Map<Character, TrieNode> next;
        TrieNode(char val) {
            this.val = val;
            next = new HashMap<>();
        }
    }
     
    public List<String> wordSearchII(char[][] board, List<String> words) {
        // write your code here
        Map<Character, TrieNode> trie = constructTrie(board);
        List<String> result = new ArrayList<>();
        for (String str : words) {
            if (!check(trie, str, 0)) continue;
            result.add(str);
        }
        return result;
    }
    
    private boolean check(Map<Character, TrieNode> trie, String str, int index) {
        if (index == str.length()) return true;
        if (!trie.containsKey(str.charAt(index))) return false;
        return check(trie.get(str.charAt(index)).next, str, index + 1);
    }
    
    private Map<Character, TrieNode> constructTrie(char[][] board) {
        Map<Character, TrieNode> trie = new HashMap<>();
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                char cur = board[i][j];
                Set<String> visited = new HashSet<>();
                visited.add(i + "" + j);
                //we still have a lot to do
                if (trie.containsKey(cur)) {
                    traversal(board, visited, trie.get(cur), i, j);
                } else {
                    TrieNode newNode = new TrieNode(cur);
                    trie.put(cur, newNode);
                    traversal(board, visited, newNode, i, j);
                }
                
            }
        }
        return trie;
    }
    
    private void traversal(char[][] board, Set<String> visited, TrieNode cur,int x, int y) {
        
        for (int i = 0; i < 4; i++) {
            int newx = x + direction[i][0];
            int newy = y + direction[i][1];
            if (newx < 0 || newx >= board.length || newy < 0 || newy >= board[0].length) continue;
            if (visited.contains(newx+""+newy)) continue;
            //we still have 
            if (cur.next.containsKey(board[newx][newy])) {
                visited.add(newx+""+newy);
                traversal(board, visited, cur.next.get(board[newx][newy]),newx, newy);
                visited.remove(newx+""+newy);
            } else {
                cur.next.put(board[newx][newy], new TrieNode(board[newx][newy]));
                visited.add(newx+""+newy);
                traversal(board, visited, cur.next.get(board[newx][newy]), newx, newy);
                visited.remove(newx+""+newy);
            }
        }
    }
}
```



