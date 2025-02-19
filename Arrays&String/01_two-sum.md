# Two Sum Problem - Comprehensive Notes

## Problem Statement
Given an array of integers `nums` and an integer `target`, return indices of the two numbers in the array such that they add up to the `target`. You may assume that each input would have exactly one solution, and you may not use the same element twice.

## Example
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1]
```

## Solution Approaches

### 1. Brute Force Approach
#### Implementation
```java
public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] == target) {
                return new int[]{i, j};
            }
        }
    }
    return new int[]{-1, -1}; // No solution found
}
```

#### Approach Explanation
- Use two nested loops to check every possible pair of numbers
- For each number at index i, check all other numbers at index j
- If sum equals target, return the indices
- Time Complexity: O(n²)
- Space Complexity: O(1)

### 2. Two-Pass Hash Table Approach
#### Implementation
```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    
    // First pass: Store all numbers with their indices
    for (int i = 0; i < nums.length; i++) {
        map.put(nums[i], i);
    }
    
    // Second pass: Check for complement
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement) && map.get(complement) != i) {
            return new int[]{i, map.get(complement)};
        }
    }
    
    return new int[]{-1, -1};
}
```

#### Approach Explanation
- Uses a HashMap to store numbers and their indices
- Makes two passes through the array
- First pass: Build the hash table
- Second pass: Look for complement
- Time Complexity: O(n)
- Space Complexity: O(n)

### 3. One-Pass Hash Table Approach (Most Optimized)
#### Implementation
```java
public int[] twoSum(int[] nums, int target) {
    HashMap<Integer, Integer> map = new HashMap<>();
    
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        map.put(nums[i], i);
    }
    
    return new int[]{-1, -1};
}
```

#### Approach Explanation
- Most optimized solution using a single pass
- Uses a HashMap to store numbers and their indices
- For each number:
  1. Calculate its complement (target - current number)
  2. Check if complement exists in HashMap
  3. If found, return current index and complement's index
  4. If not found, add current number and index to HashMap
- Time Complexity: O(n)
- Space Complexity: O(n)

## Comparison of Approaches

### Time Complexity
1. Brute Force: O(n²)
2. Two-Pass Hash Table: O(n)
3. One-Pass Hash Table: O(n)

### Space Complexity
1. Brute Force: O(1)
2. Two-Pass Hash Table: O(n)
3. One-Pass Hash Table: O(n)

### When to Use Each Approach

1. **Brute Force**
   - When array size is very small
   - When space complexity is a critical constraint
   - During interviews to demonstrate understanding of problem

2. **Two-Pass Hash Table**
   - When code readability is prioritized
   - When you need to separate the logic of storing and searching
   - When debugging is important

3. **One-Pass Hash Table**
   - In production code where performance is critical
   - When you need the most optimized solution
   - When you're comfortable with more compact code

## Important Considerations
1. Handle edge cases (empty array, no solution)
2. Consider input constraints (array size, value ranges)
3. Handle duplicate values correctly
4. Consider space-time tradeoffs based on requirements

### 4. Two Pointer Approach (For Sorted Arrays)
#### Implementation
```java
public int[] twoSum(int[] nums, int target) {
    // First make a copy of original array with indices
    int[][] numsWithIndex = new int[nums.length][2];
    for (int i = 0; i < nums.length; i++) {
        numsWithIndex[i] = new int[]{nums[i], i};
    }
    
    // Sort the array
    Arrays.sort(numsWithIndex, (a, b) -> a[0] - b[0]);
    
    int left = 0;
    int right = nums.length - 1;
    
    while (left < right) {
        int sum = numsWithIndex[left][0] + numsWithIndex[right][0];
        if (sum == target) {
            return new int[]{numsWithIndex[left][1], numsWithIndex[right][1]};
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    
    return new int[]{-1, -1};
}
```

#### Approach Explanation
- First create a copy of array with original indices
- Sort the array while maintaining original indices
- Use two pointers (left and right) to find the target sum
- Move pointers based on sum comparison with target
- Time Complexity: O(n log n) due to sorting
- Space Complexity: O(n) for storing array copy
- Best suited for already sorted arrays

### 5. Binary Search Approach
#### Implementation
```java
public int[] twoSum(int[] nums, int target) {
    int[][] numsWithIndex = new int[nums.length][2];
    for (int i = 0; i < nums.length; i++) {
        numsWithIndex[i] = new int[]{nums[i], i};
    }
    
    Arrays.sort(numsWithIndex, (a, b) -> a[0] - b[0]);
    
    for (int i = 0; i < nums.length; i++) {
        int complement = target - numsWithIndex[i][0];
        int index = binarySearch(numsWithIndex, complement, i + 1);
        if (index != -1) {
            return new int[]{numsWithIndex[i][1], numsWithIndex[index][1]};
        }
    }
    
    return new int[]{-1, -1};
}

private int binarySearch(int[][] nums, int target, int start) {
    int left = start;
    int right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid][0] == target) {
            return mid;
        } else if (nums[mid][0] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;
}
```

#### Approach Explanation
- Sort array while maintaining original indices
- For each number, binary search for its complement
- Time Complexity: O(n log n) for sorting + O(n log n) for n binary searches = O(n log n)
- Space Complexity: O(n) for storing array copy
- Good when array is already sorted and space isn't a constraint

## Comparison of All Approaches

### Time Complexity
1. Brute Force: O(n²)
2. Two-Pass Hash Table: O(n)
3. One-Pass Hash Table: O(n)
4. Two Pointer: O(n log n)
5. Binary Search: O(n log n)

### Space Complexity
1. Brute Force: O(1)
2. Two-Pass Hash Table: O(n)
3. One-Pass Hash Table: O(n)
4. Two Pointer: O(n)
5. Binary Search: O(n)

### Best Use Cases
1. **One-Pass Hash Table**: Best overall approach for unsorted arrays
2. **Two Pointer**: Best for sorted arrays
3. **Binary Search**: Alternative for sorted arrays with clear implementation
4. **Brute Force**: For very small arrays or when space is critical
5. **Two-Pass Hash Table**: When code clarity is priority

## Common Interview Questions
1. How would you handle duplicates?
2. Can you solve it without extra space?
3. What if the array is sorted?
4. How would you modify the solution for three sum?
