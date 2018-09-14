# Flatten Nested List Iterator

## Description

Given a nested list of integers, implement an iterator to flatten it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

## Example

**Example 1:**

```text
Input: [[1,1],2,[1,1]]
Output: [1,1,2,1,1]
Explanation: By calling next repeatedly until hasNext returns false, 
             the order of elements returned by next should be: [1,1,2,1,1].
```

**Example 2:**

```text
Input: [1,[4,[6]]]
Output: [1,4,6]
Explanation: By calling next repeatedly until hasNext returns false, 
             the order of elements returned by next should be: [1,4,6].
```

## Solution

感觉应该挺简单的吧。。

没有一遍过。。

偶然间看到了这个，以后**尽量别用Stack，而应该用Deque**:

Here are a few reasons why Deque is better than Stack:

1. Object oriented design - Inheritance, abstraction, classes and interfaces: Stack is a class, Deque is an interface. Only one class can be extended, whereas any number of interfaces can be implemented by a single class in Java \(multiple inheritance of type\). Using the Deque interface removes the dependency on the concrete Stack class and its ancestors and gives you more flexibility, e.g. the freedom to extend a different class or swap out different implementations of Deque \(like LinkedList, ArrayDeque\).
2. Inconsistency: Stack extends the Vector class, which allows you to access element by index. This is inconsistent with what a Stack should actually do, which is why the Deque interface is preferred \(it does not allow such operations\)--its allowed operations are consistent with what a FIFO or LIFO data structure should allow.
3. Performance: The Vector class that Stack extends is basically the "thread-safe" version of an ArrayList. The synchronizations can potentially cause a significant performance hit to your application. Also, extending other classes with unneeded functionality \(as mentioned in \#2\) bloat your objects, potentially costing a lot of extra memory and performance overhead.

```java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class NestedIterator implements Iterator<Integer> {

    List<Integer> list;
    int index;
    public NestedIterator(List<NestedInteger> nestedList) {
        list = new ArrayList<Integer>();
        helper(list, nestedList);
        index = 0;
    }
    
    private void helper(List<Integer> list, List<NestedInteger> nestedList) {
        for (NestedInteger nest : nestedList) {
            if (nest.isInteger()) {
                list.add(nest.getInteger());
            } else {
                helper(list, nest.getList());
            }
        }
    }

    @Override
    public Integer next() {
        return list.get(index++);
    }

    @Override
    public boolean hasNext() {
        if (index >= list.size()) return false;
        return true;
    }
}

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i = new NestedIterator(nestedList);
 * while (i.hasNext()) v[f()] = i.next();
 */
```

