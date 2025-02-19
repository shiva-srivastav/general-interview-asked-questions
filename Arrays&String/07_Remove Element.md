# Remove Element
LeetCode Problem 27: https://leetcode.com/problems/remove-element/

## Problem Statement
Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` in-place. Return the number of elements in `nums` which are not equal to `val`.

## Approaches

### 1. Brute Force Approach
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int n = nums.length;
        for(int i = 0; i < n; i++) {
            if(nums[i] == val) {
                // Shift all elements to left
                for(int j = i; j < n-1; j++) {
                    nums[j] = nums[j+1];
                }
                n--;
                i--;  // Check the same position again
            }
        }
        return n;
    }
}
```
**Explanation:**
- For each element equal to val, shift all subsequent elements one position to the left
- After shifting, decrease array length and recheck current position
- Time Complexity: O(n²) due to nested loops
- Space Complexity: O(1) as it's in-place

### 2. Single Pointer Approach (Your Solution)
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int index = 0;
        for(int i = 0; i < nums.length; i++) {
            if(nums[i] != val) {
                nums[index] = nums[i];
                index++;
            }
        }
        return index;
    }
}
```
**Explanation:**
- Uses a single pointer (index) to keep track of the position where non-val elements should be placed
- Iterates through array once, copying non-val elements to the front
- Effectively creates a new array prefix containing only non-val elements

**Strengths:**
- Simple and intuitive implementation
- Single pass through the array
- In-place modification
- O(n) time complexity

**Weaknesses:**
- Performs unnecessary copies when elements are already in correct position
- Doesn't handle cases where most elements are not val optimally

### 3. Two Pointer Approach (Optimal)
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int left = 0;
        int right = nums.length - 1;
        
        while(left <= right) {
            if(nums[left] == val) {
                nums[left] = nums[right];
                right--;
            } else {
                left++;
            }
        }
        return left;
    }
}
```
**Explanation:**
- Uses two pointers: left from start and right from end
- When finding val at left, replace it with element from right
- Minimizes number of moves needed
- Particularly efficient when val appears less frequently

## Time & Space Complexity Analysis
1. Brute Force:
   - Time: O(n²) - nested loops for shifting elements
   - Space: O(1) - in-place modification
2. Single Pointer (Your Approach):
   - Time: O(n) - single pass through array
   - Space: O(1) - in-place modification
3. Two Pointer:
   - Time: O(n) - single pass through array
   - Space: O(1) - in-place modification

## Companies That Asked This Question
1. Amazon
2. Microsoft
3. Apple
4. Facebook
5. Bloomberg
6. Adobe

## Similar Problems
1. [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
2. [Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)
3. [Move Zeroes](https://leetcode.com/problems/move-zeroes/)
4. [Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Important Notes
1. Array Manipulation Techniques:
   - In-place modification is often preferred
   - Two-pointer technique is powerful for array problems
   - Consider direction of array traversal

2. Interview Tips:
   - Start with the simple solution
   - Discuss trade-offs between approaches
   - Consider edge cases:
     - Empty array
     - All elements are val
     - No elements are val
     - Single element array

3. Common Patterns:
   - Two-pointer technique
   - Partition array
   - In-place modification
   - Order preservation requirement

4. Optimization Considerations:
   - Minimize number of array writes
   - Handle frequent/infrequent val occurrences
   - Balance between code simplicity and performance

5. Follow-up Questions:
   - What if order doesn't matter?
   - What if we need to preserve relative order?
   - What if we have multiple values to remove?
   - What if we need to count removed elements?
