# Search in Rotated Sorted Array - Comprehensive Analysis

## Problem Link
[LeetCode 33 - Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

## Problem Description
Given an integer array `nums` that is sorted in ascending order and rotated at some pivot point, and a target value, return the index of target if it exists in the array, or -1 if it does not exist.

Example:
```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

## Approach 1: Brute Force (Linear Search)

### Code
```java
class Solution {
    public int search(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) {
                return i;
            }
        }
        return -1;
    }
}
```

### Concept
- Simply iterate through the array from start to end
- Return the index when target is found
- Return -1 if target is not found

### Analysis
#### Strengths
- Simple to implement
- Easy to understand
- Works regardless of rotation
- No additional space needed
- Good for small arrays

#### Weaknesses
- Ignores the sorted property of the array
- Linear time complexity
- Inefficient for large arrays
- Doesn't take advantage of the array's special properties

#### Complexity
- Time Complexity: O(n)
- Space Complexity: O(1)

## Approach 2: Optimized Binary Search

### Step-by-Step Solution Procedure

1. **Initial Setup**
   - Initialize two pointers: `left = 0` and `right = nums.length - 1`
   - This represents our current search space

2. **Binary Search Loop**
   - While `left <= right`:
     - Calculate middle point: `mid = left + (right - left) / 2`
     - If `nums[mid] == target`, we found our answer, return `mid`

3. **Identify Sorted Portion**
   - At any given point, at least one half of the array must be sorted
   - Check if left half is sorted by comparing `nums[left] <= nums[mid]`
   
4. **Search in Left Half (if sorted)**
   - If `nums[left] <= nums[mid]`, left half is sorted
   - Check if target lies in this sorted range:
     - If `target >= nums[left] && target < nums[mid]`
       - Search left half: `right = mid - 1`
     - Else
       - Search right half: `left = mid + 1`

5. **Search in Right Half (if sorted)**
   - If left half is not sorted, right half must be sorted
   - Check if target lies in this sorted range:
     - If `target > nums[mid] && target <= nums[right]`
       - Search right half: `left = mid + 1`
     - Else
       - Search left half: `right = mid - 1`

6. **Return Result**
   - If we exit the while loop without finding target, return -1

### Visual Example
Consider array: [4, 5, 6, 7, 0, 1, 2], target = 0

```
Step 1: Initial array
[4, 5, 6, 7, 0, 1, 2]
 L     M        R     mid = 3, nums[mid] = 7

Step 2: Left half is sorted (4 ≤ 7), but target = 0 is not in range [4,7]
[4, 5, 6, 7, 0, 1, 2]
         L  M  R      mid = 5, nums[mid] = 1

Step 3: Right half is sorted (1 ≤ 2), but target = 0 is not in range [1,2]
[4, 5, 6, 7, 0, 1, 2]
         L     M  R   mid = 4, nums[mid] = 0

Step 4: Found target at index 4
Return 4
```

### Common Pitfalls to Avoid
1. **Wrong Mid Calculation**
   - Use `mid = left + (right - left) / 2`
   - Not `(left + right) / 2` (can cause overflow)

2. **Incorrect Comparisons**
   - Always compare with `nums[mid]`, not just `mid`
   - Use `<=` for sorted portion check

3. **Wrong Range Checks**
   - For left portion: `target >= nums[left] && target < nums[mid]`
   - For right portion: `target > nums[mid] && target <= nums[right]`

4. **Edge Cases**
   - Single element array
   - Two element array
   - Target at rotation point

### Debugging Tips
1. Print the values of `left`, `mid`, `right` at each step
2. Track which portion is identified as sorted
3. Verify the range conditions
4. Test with small arrays first

### Code
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] == target) {
                return mid;
            }
            
            // Left half is sorted
            if (nums[left] <= nums[mid]) {
                if (target >= nums[left] && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
            // Right half is sorted
            else {
                if (target > nums[mid] && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }
        return -1;
    }
}
```

### Concept
- Modified binary search that handles rotation
- At each step, identify which half is sorted
- Use the sorted half to determine where target might be
- Navigate the search space accordingly

### Analysis
#### Strengths
- Optimal time complexity
- Utilizes the sorted nature of the array
- Handles rotation efficiently
- No additional space needed

#### Weaknesses
- More complex implementation
- Requires careful handling of conditions
- More prone to bugs if not implemented correctly

#### Complexity
- Time Complexity: O(log n)
- Space Complexity: O(1)

## Approach 3: Two-Step Binary Search

### Code
```java
class Solution {
    public int search(int[] nums, int target) {
        // Step 1: Find pivot
        int pivot = findPivot(nums);
        
        // Step 2: Determine which half to search and perform binary search
        if (pivot == 0 || target < nums[0]) {
            return binarySearch(nums, pivot, nums.length - 1, target);
        }
        return binarySearch(nums, 0, pivot - 1, target);
    }
    
    private int findPivot(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left;
    }
    
    private int binarySearch(int[] nums, int left, int right, int target) {
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
}
```

### Concept
- Split the problem into two steps:
  1. Find the pivot point (smallest element)
  2. Perform regular binary search on appropriate half
- More structured approach but requires two passes

## Related Problems
1. [Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/) (with duplicates)
2. [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
3. [Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)

## Companies That Ask This Question
- Google
- Amazon
- Microsoft
- Facebook
- Apple
- Adobe
- Oracle
- Uber
- LinkedIn

## Important Edge Cases
1. Empty array
2. Single element array
3. Array with two elements
4. No rotation (pivot at start)
5. Full rotation (pivot at end)
6. Target not in array
7. Target at rotation point

## Interview Tips
1. Start with the brute force approach to demonstrate problem-solving process
2. Mention that the array is sorted but rotated
3. Explain why binary search can be modified to work here
4. Discuss the trade-offs between different approaches
5. Always handle edge cases
6. Be prepared for follow-up questions about handling duplicates

## Follow-up Questions
1. How would you handle duplicates in the array?
2. Can you implement it recursively?
3. What if you need to find all occurrences of the target?
4. How would you handle multiple rotations?
5. What if the rotation point is given?
