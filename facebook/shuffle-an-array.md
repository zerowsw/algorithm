# Shuffle an Array

## Description

Shuffle a set of numbers without duplicates.

**Example:**

```text
// Init an array with set 1, 2, and 3.
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// Shuffle the array [1,2,3] and return its result. Any permutation of [1,2,3] must equally likely to be returned.
solution.shuffle();

// Resets the array back to its original configuration [1,2,3].
solution.reset();

// Returns the random shuffling of array [1,2,3].
solution.shuffle();
```

## Solution

Fisher-Yates Algorithm

```java
class Solution {

    int[] original;
    int[] array;
    Random rand;
    public Solution(int[] nums) {
        array = nums;
        original = nums.clone();
        rand = new Random();
    }
    
    /** Resets the array to its original configuration and return it. */
    public int[] reset() {
        array = original;
        original = original.clone();
        return original;
    }
    
    /** Returns a random shuffling of the array. */
    public int[] shuffle() {
        for (int i = 0; i < array.length; i++) {
            randomSwap(array, i);
        }
        return array;
    }
    
    private void randomSwap(int[] array, int start) {
        int target = rand.nextInt(array.length - start) + start;
        int temp = array[target];
        array[target] = array[start];
        array[start] = temp;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int[] param_1 = obj.reset();
 * int[] param_2 = obj.shuffle();
 */
```

