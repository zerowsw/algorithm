# Moving Average from Data Stream

## Description

Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

## Example

```text
MovingAverage m = new MovingAverage(3);
m.next(1) = 1
m.next(10) = (1 + 10) / 2
m.next(3) = (1 + 10 + 3) / 3
m.next(5) = (10 + 3 + 5) / 3
```

## Solution

没啥好说的

```java
class MovingAverage {

    int size;
    double sum;
    int count;
    Queue<Integer> queue;
    /** Initialize your data structure here. */
    public MovingAverage(int size) {
        this.size = size;
        queue = new LinkedList<>();
        sum = 0.0;
        count = 0;
    }
    
    public double next(int val) {
        queue.offer(val);
        sum += val;
        count++;
        if (count > size) {
            sum -= queue.poll();
            count--;
        }
        return sum / count;
    }
}

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param_1 = obj.next(val);
 */
```

