# Merge Sorted Array
LeetCode Problem 88: https://leetcode.com/problems/merge-sorted-array/

## Problem Statement
Given two sorted arrays `nums1` and `nums2`, merge them in a sorted manner into `nums1`. `nums1` has enough space (size m + n) to hold additional elements from `nums2`.

## Approaches

### 1. Brute Force Approach
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // Copy elements from nums2 to nums1
        for(int i = 0; i < n; i++) {
            nums1[m + i] = nums2[i];
        }
        // Sort the entire array
        Arrays.sort(nums1);
    }
}
```
**Explanation:**
- Simply copy all elements from nums2 to the end of nums1
- Use built-in sort function to sort the entire array
- Time Complexity: O((m+n)log(m+n)) due to sorting
- Space Complexity: O(1) as sorting is done in-place

### 2. Two Pointer with Extra Space (Your Approach)
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int aux[] = new int[m+n];
        int left = 0, right = 0;
        
        int k = -1;
        while(left < m && right < n) {
            if(nums1[left] <= nums2[right]) {
                aux[++k] = nums1[left++];
            } else {
                aux[++k] = nums2[right++];
            }
        }

        while(left < m) {
            aux[++k] = nums1[left++];
        }
        
        while(right < n) {
            aux[++k] = nums2[right++];
        }

        for(int i = 0; i < m+n; i++) {
            nums1[i] = aux[i];
        }
    }
}
```
**Explanation:**
- Create auxiliary array to store merged result
- Use two pointers to compare elements from both arrays
- Copy remaining elements from either array
- Copy back to nums1
- Time Complexity: O(m+n)
- Space Complexity: O(m+n) for auxiliary array

### 3. Optimal Approach (Three Pointers from End)
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int p1 = m - 1;    // Last element of nums1
        int p2 = n - 1;    // Last element of nums2
        int p = m + n - 1; // Last position in nums1
        
        while (p2 >= 0) {
            if (p1 >= 0 && nums1[p1] > nums2[p2]) {
                nums1[p] = nums1[p1];
                p1--;
            } else {
                nums1[p] = nums2[p2];
                p2--;
            }
            p--;
        }
    }
}
```
**Explanation:**
- Start from end of both arrays
- Compare elements and place larger one at the end of nums1
- No need for extra space as we're filling from end
- Time Complexity: O(m+n)
- Space Complexity: O(1)

## Time & Space Complexity Analysis
1. Brute Force:
   - Time: O((m+n)log(m+n))
   - Space: O(1)
2. Two Pointer with Extra Space:
   - Time: O(m+n)
   - Space: O(m+n)
3. Optimal Three Pointer:
   - Time: O(m+n)
   - Space: O(1)

## Companies That Asked This Question
1. Microsoft
2. Amazon
3. Facebook
4. Google
5. Adobe
6. Apple
7. LinkedIn

## Similar Problems
1. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
2. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
3. [Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)
4. [Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii/)

## Important Notes
1. Always consider the space optimization possibility in array problems
2. Three pointer technique from end is very useful when one array has extra space at end
3. This is a fundamental problem for understanding merge logic in merge sort
4. The problem tests understanding of:
   - Array manipulation
   - Two pointer technique
   - In-place operations
   - Space optimization
5. Common edge cases to consider:
   - When one array is empty
   - When arrays have same elements
   - When arrays have different lengths
6. Interview tips:
   - Start with the simple solution
   - Explain trade-offs between approaches
   - Mention space optimization possibility
   - Discuss how this relates to merge sort
