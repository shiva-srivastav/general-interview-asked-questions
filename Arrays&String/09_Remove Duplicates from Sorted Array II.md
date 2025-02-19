# Remove Duplicates from Sorted Array II

## Problem Link
[LeetCode 80 - Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)

## Problem Statement
Given a sorted array `nums`, remove duplicates in-place such that duplicates appear at most twice and return the new length. The relative order of the elements should be kept the same.

## Approaches

### 1. Brute Force Approach
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int n = nums.length;
        int count = 1;
        int current = 1;
        
        for(int i = 1; i < n; i++) {
            if(nums[i] == nums[i-1]) {
                count++;
            } else {
                count = 1;
            }
            
            if(count <= 2) {
                nums[current] = nums[i];
                current++;
            }
        }
        return current;
    }
}
```

**Concept:**
- Iterate through the array and keep track of consecutive occurrences
- Copy elements to the front when count ≤ 2
- Time Complexity: O(n)
- Space Complexity: O(1)

### 2. HashMap Approach (Your Solution)
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        LinkedHashMap<Integer,Integer> hm = new LinkedHashMap<>();
        
        for(int i = 0; i < nums.length; i++) {
            if(!hm.containsKey(nums[i]) || hm.get(nums[i]) < 2) {
                hm.put(nums[i], hm.getOrDefault(nums[i],0) + 1);
            }
        }
        
        int k = 0;
        for(Map.Entry<Integer,Integer> entry: hm.entrySet()) {
            int key = entry.getKey();
            int value = entry.getValue();
            for(int i = 1; i <= value; i++) {
                nums[k++] = key;
            }
        }
        return k;
    }
}
```

**Analysis:**
- Uses LinkedHashMap to maintain order and count occurrences
- Strengths:
  - Clean and readable code
  - Maintains element order using LinkedHashMap
- Weaknesses:
  - Extra space complexity due to HashMap
  - Two passes through the array
- Time Complexity: O(n)
- Space Complexity: O(n)

### 3. Optimized Two-Pointer Approach
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums.length <= 2) return nums.length;
        
        int slow = 2;
        for(int fast = 2; fast < nums.length; fast++) {
            if(nums[fast] != nums[slow-2]) {
                nums[slow] = nums[fast];
                slow++;
            }
        }
        return slow;
    }
}
```

**Concept:**
- Use two pointers: slow (insertion position) and fast (scanning position)
- Compare with element two positions behind
- Most efficient approach
- Time Complexity: O(n)
- Space Complexity: O(1)

## Similar Problems
1. [Remove Duplicates from Sorted Array (LeetCode 26)](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
2. [Remove Duplicates from Sorted List II (LeetCode 82)](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)
3. [Remove Element (LeetCode 27)](https://leetcode.com/problems/remove-element/)

## Companies That Asked This Question
- Amazon
- Microsoft
- Facebook
- Google
- Adobe
- Apple

## Follow-up Questions
1. How would you modify the solution if at most k duplicates were allowed?
2. What if the array wasn't sorted?
3. How would you handle negative numbers?

## Key Techniques Used
1. Two Pointer Technique
2. In-place Array Modification
3. Hash Table
4. Array Manipulation

## Interview Tips
1. Always clarify the input constraints
2. Discuss the space-time tradeoffs
3. Consider edge cases:
   - Empty array
   - Array with all duplicates
   - Array with no duplicates
   - Array with exactly two duplicates
4. Mention that in-place modification saves space but might not be suitable for all use cases
