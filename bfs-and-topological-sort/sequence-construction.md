# Sequence Construction

## Description

Check whether the original sequence `org` can be uniquely reconstructed from the sequences in `seqs`. The org sequence is a permutation of the integers from 1 to n, with 1 ≤ n ≤ 10^4. Reconstruction means building a shortest common supersequence of the sequences in `seqs` \(i.e., a shortest sequence so that all sequences in `seqs` are subsequences of it\). Determine whether there is only one sequence that can be reconstructed from `seqs` and it is the `org` sequence.

## Example

```text
Given org = [1,2,3], seqs = [[1,2],[1,3]]
Return false
Explanation:
[1,2,3] is not the only one sequence that can be reconstructed, because [1,3,2] is also a valid sequence that can be reconstructed.

Given org = [1,2,3], seqs = [[1,2]]
Return false
Explanation:
The reconstructed sequence can only be [1,2].

Given org = [1,2,3], seqs = [[1,2],[1,3],[2,3]]
Return true
Explanation:
The sequences [1,2], [1,3], and [2,3] can uniquely reconstruct the original sequence [1,2,3].

Given org = [4,1,5,2,6,3], seqs = [[5,2,6,3],[4,1,5,2]]
Return true
```

## Solution

在试图从seqs构建supersequence的过程中，就会发现这又是一个拓扑排序，或者说是course schedule II， 相当于再问有没有唯一的课程安排计划。 不过，处理seqs的时候又有些不一样，这里的关系不再是一对一的pair，需要稍微进行一下转化。



