# Remove Duplicates from Sorted Array
LeetCode Problem 26: https://leetcode.com/problems/remove-duplicates-from-sorted-array/

## Problem Statement
Given a sorted array `nums`, remove the duplicates in-place such that each element appears only once and return the new length.

## Approaches

### 1. Brute Force Approach
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int n = nums.length;
        for(int i = 0; i < n-1; i++) {
            for(int j = i+1; j < n; j++) {
                if(nums[i] == nums[j]) {
                    // Shift elements to left
                    for(int k = j; k < n-1; k++) {
                        nums[k] = nums[k+1];
                    }
                    n--;
                    j--;
                }
            }
        }
        return n;
    }
}
```
**Explanation:**
- Uses nested loops to find and remove duplicates
- Shifts elements left when duplicate found
- Time Complexity: O(n³)
- Space Complexity: O(1)

### 2. LinkedHashSet Approach (Your First Solution)
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        LinkedHashSet<Integer> lsh = new LinkedHashSet<>();
        for(int i = 0; i < nums.length; i++) {
            if(!lsh.contains(nums[i])) {
                lsh.add(nums[i]);
            }
        }
        // Using stream to copy elements back
        final int[] index = {0}; 
        lsh.stream().forEach(a -> {nums[index[0]++] = a;});
        return lsh.size();
    }
}
```

### 3. LinkedHashSet with Iterator Approach (Your Second Solution)
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        LinkedHashSet<Integer> lsh = new LinkedHashSet<>();
        for(int i = 0; i < nums.length; i++) {
            if(!lsh.contains(nums[i])) {
                lsh.add(nums[i]);
            }
        }
        Iterator<Integer> itr = lsh.iterator();
        int i = 0;
        while(itr.hasNext()) {
            nums[i++] = itr.next();
        }
        return lsh.size();
    }
}
```

**Analysis of Your Approaches:**
*Strengths:*
- Maintains sorted order using LinkedHashSet
- Clean and readable code
- Handles all cases correctly

*Weaknesses:*
- Uses extra space O(n) for LinkedHashSet
- Performs unnecessary containment checks
- Doesn't utilize the fact that array is already sorted

### 4. Optimal Two-Pointer Approach
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums.length == 0) return 0;
        
        int uniquePointer = 0;
        
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] != nums[uniquePointer]) {
                uniquePointer++;
                nums[uniquePointer] = nums[i];
            }
        }
        
        return uniquePointer + 1;
    }
}
```
**Explanation:**
- Uses two pointers: uniquePointer for last unique element position
- Leverages the fact that array is sorted
- Only copies when finding new unique element
- In-place modification with O(1) space

## Time & Space Complexity Analysis
1. Brute Force:
   - Time: O(n³)
   - Space: O(1)

2. LinkedHashSet Approaches (Both versions):
   - Time: O(n)
   - Space: O(n) for LinkedHashSet

3. Optimal Two-Pointer:
   - Time: O(n)
   - Space: O(1)

## Companies That Asked This Question
1. Facebook
2. Amazon
3. Microsoft
4. Google
5. Apple
6. Bloomberg
7. Adobe
8. Oracle

## Similar Problems
1. [Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)
2. [Remove Element](https://leetcode.com/problems/remove-element/)
3. [Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)
4. [Move Zeroes](https://leetcode.com/problems/move-zeroes/)

## Important Notes

1. Algorithm Selection Considerations:
   - Is extra space allowed?
   - Is input array sorted?
   - Are duplicates consecutive?
   - Frequency of duplicates

2. Interview Tips:
   - Start by mentioning the sorted nature of array
   - Discuss space-time tradeoffs
   - Consider edge cases:
     - Empty array
     - No duplicates
     - All duplicates
     - Single element

3. Common Patterns:
   - Two-pointer technique
   - In-place modification
   - Utilizing sorted property
   - Order preservation

4. Optimization Strategies:
   - Leverage sorted property
   - Minimize array writes
   - Use constant extra space
   - Single pass solution

5. Follow-up Questions:
   - What if array is not sorted?
   - What if we need to keep duplicates up to K times?
   - What if we need to count removed duplicates?
   - What if we need to handle negative numbers?

6. Common Mistakes to Avoid:
   - Not utilizing sorted property
   - Unnecessary space usage
   - Incorrect handling of edge cases
   - Over-complicated solutions
