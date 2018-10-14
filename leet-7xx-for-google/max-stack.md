# Max Stack

## Description

Design a max stack that supports push, pop, top, peekMax and popMax.

1. push\(x\) -- Push element x onto stack.
2. pop\(\) -- Remove the element on top of the stack and return it.
3. top\(\) -- Get the element on the top.
4. peekMax\(\) -- Retrieve the maximum element in the stack.
5. popMax\(\) -- Retrieve the maximum element in the stack, and remove it. If you find more than one maximum elements, only remove the top-most one.

**Example 1:**

```text
MaxStack stack = new MaxStack();
stack.push(5); 
stack.push(1);
stack.push(5);
stack.top(); -> 5
stack.popMax(); -> 5
stack.top(); -> 1
stack.peekMax(); -> 5
stack.pop(); -> 1
stack.top(); -> 5
```

**Note:**

1. -1e7 &lt;= x &lt;= 1e7
2. Number of operations won't exceed 10000.
3. The last four operations won't be called when stack is empty.

## Solution

最优解：double linkedlist + TreeMap&lt;Integer, List&lt;Node&gt;&gt;

次优解：类似MinStack，用一个普通栈和一个单调栈来处理，关键是popMax的时候比较耗时

这里写一下两个栈的实现吧：

```java
class MaxStack {
    
    Deque<Integer> stack;
    Deque<Integer> maxstack;

    /** initialize your data structure here. */
    public MaxStack() {
        stack = new LinkedList<>();
        maxstack = new LinkedList<>();
    }
    
    public void push(int x) {
        int max = maxstack.isEmpty() ? x : maxstack.peek();
        max = Math.max(max, x);
        stack.push(x);
        maxstack.push(max);
    }
    
    public int pop() {
        maxstack.pop();
        return stack.pop();
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int peekMax() {
        return maxstack.peek();
    }
    
    public int popMax() {
        Deque<Integer> buffer = new LinkedList<>();
        int max = maxstack.peek();
        while (stack.peek() != max) {
            buffer.push(pop());
        }
        pop();
        while (!buffer.isEmpty()) {
            push(buffer.pop());
        }
        return max;
    }
}

/**
 * Your MaxStack object will be instantiated and called as such:
 * MaxStack obj = new MaxStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.peekMax();
 * int param_5 = obj.popMax();
 */
```



