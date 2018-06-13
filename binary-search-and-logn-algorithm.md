# Binary Search & LogN algorithm

基本上问到比O\(n\)更优的算法，面试当中几乎只能是nlog\(n\)的二分法

1. 二分法模版

   start + 1 &lt; end \(这种写法有一定的优势， 比如从题目last-position-of-target就可以看出\)

   mid = start + \(end - start\) / 2 \(防止溢出\)

2. 二分的位置 （first min  rotated sorted array）
3. 二分位置保留一半 half half \(find peak element\)
4. 二分答案 （copy book）

Time complexity: T\(n\) = T\(n/2\) + O\(1\) = O\(logn\)

面试中能不Recursion就不Recursion, 是否使用取决于：（记得跟面试官讨论，不要自下判断）

1. 面试官是否要求不使用Recursion
2. 不用Recursion是否造成变得复杂
3. Recursion的深度是否太深



