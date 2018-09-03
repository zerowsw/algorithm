# Account Merge

## Description

Given a list `accounts`, each element `accounts[i]` is a list of strings, where the first element `accounts[i][0]` is a name, and the rest of the elements are emails representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some email that is common to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails **in sorted order**. The accounts themselves can be returned in any order.

## Example

```text
Input: 
accounts = [["John", "johnsmith@mail.com", "john00@mail.com"], ["John", "johnnybravo@mail.com"], ["John", "johnsmith@mail.com", "john_newyork@mail.com"], ["Mary", "mary@mail.com"]]
Output: [["John", 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com'],  ["John", "johnnybravo@mail.com"], ["Mary", "mary@mail.com"]]
Explanation: 
The first and third John's are the same person as they have the common email "johnsmith@mail.com".
The second John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], 
['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.
```

## Solution

题目也看过了，同一个人的不同账户名字一定是一样的，名字一样并且有共同email地址的账户属于同一个人。

这不会要用Union Find吧。。我擦，真的

直接看答案吧

首先可以用DFS的方法来处理，将Email构建成Graph， 同时，记录email到account name的映射，然后DFS遍历，找到所有connected component。

此外的话，可以用Union Find方法

Union Find还可以参考这道题：[https://leetcode.com/articles/redundant-connection/](https://leetcode.com/articles/redundant-connection/)

还需要多加练习

```java
class Solution {
    private class DSU{
        int[] parent;
        public DSU(){
            parent = new int[10001];
            for (int i = 0; i < parent.length; i++) {
                parent[i] = i;
            }
        }
        
        public int find(int x) {
            if (parent[x] != x) parent[x] = find(parent[x]);
            return parent[x];
        }
        
        public void union(int x, int y) {
            parent[find(x)] = find(y);
        }
    }
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        Map<String, String> emailToName = new HashMap<>();
        Map<String, Integer> emailToID = new HashMap<>();
        
        int id = 0;
        DSU dsu = new DSU();
        for (List<String> account : accounts) {
            String name = "";
            for (String email : account) {
                if (name == "") {
                    name = email;
                    continue;
                }
                emailToName.put(email, name);
                if (!emailToID.containsKey(email)) {
                    emailToID.put(email, id++);
                }
                dsu.union(emailToID.get(email), emailToID.get(account.get(1)));
            }
        }
        
        Map<Integer, List<String>> group = new HashMap<>();
        for (String email : emailToName.keySet()) {
            group.computeIfAbsent(dsu.find(emailToID.get(email)), x -> new ArrayList<>()).add(email);
        }
        
        List<List<String>> res = new ArrayList<>();
        for (Integer key : group.keySet()) {
            List<String> emails = group.get(key);
            Collections.sort(emails);
            emails.add(0, emailToName.get(emails.get(0)));
            res.add(emails);
        }
        return res;
    }
}
```

