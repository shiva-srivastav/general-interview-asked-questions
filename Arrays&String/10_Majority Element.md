# Majority Element

## Problem Link
[LeetCode 169 - Majority Element](https://leetcode.com/problems/majority-element/)

## Problem Statement
Given an array `nums` of size n, return the majority element (the element that appears more than ⌊n/2⌋ times).

## Moore's Voting Algorithm Statement
Moore's Voting Algorithm is a linear-time, constant-space algorithm for finding the majority element in an array, where a majority element is defined as an element that appears more than n/2 times.

**Theorem:** 
If a majority element exists in an array, it will always be left as the candidate after applying the voting algorithm.

**Proof Intuition:**
1. The majority element appears more than n/2 times
2. All other elements combined appear less than n/2 times
3. When we pair different elements:
   - Each pair eliminates two elements (count decrement)
   - The majority element can at most be paired n/2 times
   - Since it appears more than n/2 times, it will always have some unpaired occurrences
   - These unpaired occurrences ensure it survives as the final candidate

**Mathematical Foundation:**
- Let m be the majority element frequency: m > n/2
- Let x be other elements' total frequency: x < n/2
- Maximum possible pairs of majority with other elements: min(m, x)
- Remaining unpaired majority elements: m - min(m, x) > 0

## Approaches

### 1. Brute Force Approach
```java
class Solution {
    public int majorityElement(int[] nums) {
        int n = nums.length;
        for (int num : nums) {
            int count = 0;
            for (int element : nums) {
                if (element == num) {
                    count++;
                }
            }
            if (count > n/2) {
                return num;
            }
        }
        return -1;
    }
}
```

**Concept:**
- Check each element's frequency by counting its occurrences
- Return if count exceeds n/2
- Time Complexity: O(n²)
- Space Complexity: O(1)

### 2. Moore's Voting Algorithm
```java
class Solution {
    public int majorityElement(int[] nums) {
        // Initialize candidate and count
        int candidate = nums[0];
        int count = 1;
        
        // Moore's Voting Algorithm
        for(int i = 1; i < nums.length; i++) {
            if(count == 0) {
                candidate = nums[i];
                count = 1;
            }
            else if(nums[i] == candidate) {
                count++;
            }
            else {
                count--;
            }
        }
        
        return candidate;
    }
}
```

**Analysis of Moore's Voting Algorithm:**
- Core Concept:
  - Maintains a candidate and its count
  - When count reaches 0, picks new candidate
  - Majority element will survive all pairings
- Strengths:
  - Optimal space complexity (O(1))
  - Single pass through array
  - Elegant and simple implementation
- Weaknesses:
  - Assumes majority element always exists
  - May need validation pass if majority element isn't guaranteed
- Time Complexity: O(n)
- Space Complexity: O(1)

**How it Works:**
1. Initialize first element as candidate
2. For each element:
   - If count is 0, pick current element as new candidate
   - If element matches candidate, increment count
   - If element differs from candidate, decrement count
3. Final candidate will be majority element

**Optional Validation Pass:**
```java
// Add this after algorithm if majority element isn't guaranteed
int validationCount = 0;
for(int num : nums) {
    if(num == candidate) {
        validationCount++;
    }
}
if(validationCount <= nums.length/2) {
    return -1; // or throw exception
}
```

### 3. HashMap Approach
```java
class Solution {
    public int majorityElement(int[] nums) {
        Map<Integer, Integer> counts = new HashMap<>();
        int n = nums.length;
        
        for (int num : nums) {
            counts.put(num, counts.getOrDefault(num, 0) + 1);
            if (counts.get(num) > n/2) {
                return num;
            }
        }
        return -1;
    }
}
```

**Concept:**
- Use HashMap to track frequency
- Return element when count exceeds n/2
- Time Complexity: O(n)
- Space Complexity: O(n)

### 4. Sorting Approach
```java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length/2];
    }
}
```

**Concept:**
- Sort array
- Middle element will be majority element
- Time Complexity: O(n log n)
- Space Complexity: O(1) or O(n) depending on sorting algorithm

## Similar Problems
1. [Majority Element II (LeetCode 229)](https://leetcode.com/problems/majority-element-ii/)
2. [Check If a Number Is Majority Element in a Sorted Array (LeetCode 1150)](https://leetcode.com/problems/check-if-a-number-is-majority-element-in-a-sorted-array/)
3. [Find All Numbers Disappeared in an Array (LeetCode 448)](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)

## Companies That Asked This Question
- Amazon
- Microsoft
- Google
- Apple
- Adobe
- Facebook
- Bloomberg

## Follow-up Questions
1. Could you solve it in O(1) space complexity and O(n) time complexity?
2. What if there's no guarantee of a majority element?
3. How would you handle negative numbers?
4. Can you solve it using bit manipulation?

## Interview Tips
1. Clarify assumptions:
   - Is a majority element guaranteed to exist?
   - Can numbers be negative?
   - Are there memory constraints?

2. Different approaches trade-offs:
   - HashMap: Simple but uses extra space
   - Sorting: Simple but slower
   - Moore's Voting: Optimal but needs validation if majority not guaranteed

3. Edge cases to consider:
   - Array with single element
   - Array with all same elements
   - Array with exactly n/2 occurrences of an element

## Key Techniques Used
1. Moore's Voting Algorithm
2. Hash Table
3. Sorting
4. Counting
5. Divide and Conquer (alternative approach)

## Common Mistakes to Avoid
1. Not validating majority element when using Moore's Voting Algorithm
2. Incorrectly handling edge cases
3. Using unnecessary extra space
4. Not considering the possibility of negative numbers
