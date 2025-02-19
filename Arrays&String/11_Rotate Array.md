# Rotate Array

## Problem Link
[LeetCode 189 - Rotate Array](https://leetcode.com/problems/rotate-array/)

## Problem Statement
Given an array, rotate the array to the right by k steps, where k is non-negative.

## Approaches

### 1. Brute Force Approach
```java
class Solution {
    public void rotate(int[] nums, int k) {
        k = k % nums.length;
        for(int i = 0; i < k; i++) {
            int last = nums[nums.length - 1];
            for(int j = nums.length - 1; j > 0; j--) {
                nums[j] = nums[j-1];
            }
            nums[0] = last;
        }
    }
}
```

**Concept:**
- Rotate array one position at a time, k times
- Store last element and shift all elements right
- Time Complexity: O(n×k)
- Space Complexity: O(1)

### 2. Extra Array Approach (Your First Solution)
```java
class Solution {
    public void rotate(int[] nums, int k) {
        int aux[] = new int[nums.length];
        for(int i = 0; i < nums.length; i++) {
            aux[(i+k)%nums.length] = nums[i];
        }
        
        for(int i = 0; i < nums.length; i++) {
            nums[i] = aux[i];
        }
    }
}
```

**Analysis:**
- Uses auxiliary array to store rotated elements
- Strengths:
  - Simple and straightforward
  - Easy to understand and implement
- Weaknesses:
  - Uses extra space
  - Two passes through array
- Time Complexity: O(n)
- Space Complexity: O(n)

### 3. Reverse Array Approach (Your Second Solution)
```java
class Solution {
    public void rotate(int[] nums, int k) {
        k = k % nums.length;
        reverse(nums, 0, nums.length-1);
        reverse(nums, 0, k-1);
        reverse(nums, k, nums.length-1);
    }
    
    private void reverse(int[] nums, int left, int right) {
        while(left < right) {
            int tmp = nums[left];
            nums[left] = nums[right];
            nums[right] = tmp;
            left++;
            right--;
        }
    }
}
```

**Analysis:**
- Uses three reverse operations
- Most efficient approach
- Strengths:
  - In-place rotation
  - Optimal space complexity
  - Clear and elegant solution
- Time Complexity: O(n)
- Space Complexity: O(1)

### 4. Cyclic Replacements Approach
```java
class Solution {
    public void rotate(int[] nums, int k) {
        k = k % nums.length;
        int count = 0;
        
        for(int start = 0; count < nums.length; start++) {
            int current = start;
            int prev = nums[start];
            
            do {
                int next = (current + k) % nums.length;
                int temp = nums[next];
                nums[next] = prev;
                prev = temp;
                current = next;
                count++;
            } while(start != current);
        }
    }
}
```

**Concept:**
- Move elements in cyclic manner
- Each element goes to its final position in one move
- Time Complexity: O(n)
- Space Complexity: O(1)

## Similar Problems
1. [Rotate List (LeetCode 61)](https://leetcode.com/problems/rotate-list/)
2. [Rotate Image (LeetCode 48)](https://leetcode.com/problems/rotate-image/)
3. [Reverse Words in a String (LeetCode 151)](https://leetcode.com/problems/reverse-words-in-a-string/)

## Companies That Asked This Question
- Microsoft
- Amazon
- Apple
- Google
- Facebook
- Adobe
- Uber

## Follow-up Questions
1. Can you do it with O(1) extra space?
2. What if k is negative?
3. How would you handle very large values of k?
4. Can you optimize for when k is close to array length?

## Interview Tips
1. Always handle the case where k > array length
2. Consider edge cases:
   - Empty array
   - Single element array
   - k = 0
   - k = array length
3. Discuss trade-offs between approaches:
   - Space vs Time complexity
   - Code simplicity vs Performance
   - Memory usage vs CPU usage

## Key Techniques Used
1. Array Manipulation
2. Modular Arithmetic
3. In-place Reversal
4. Cyclic Replacements
5. Two-pointer Technique

## Common Mistakes to Avoid
1. Forgetting to handle k > array length
2. Not considering memory constraints
3. Incorrect handling of edge cases
4. Inefficient implementation of array rotation
5. Not taking advantage of problem properties

## Mathematical Insight
The reverse approach works because:
1. Original array: [1,2,3,4,5,6,7], k = 3
2. Full reverse: [7,6,5,4,3,2,1]
3. Reverse first k: [5,6,7,4,3,2,1]
4. Reverse rest: [5,6,7,1,2,3,4]

This pattern works because it effectively:
- Brings last k elements to front
- Maintains relative order within each segment
- Does it all in-place
