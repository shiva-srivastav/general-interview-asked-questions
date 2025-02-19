# Contains Duplicate - Complete Analysis

## Problem Link
[LeetCode 217 - Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

## Problem Statement
Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

## 1. Brute Force Approach
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        for(int i = 0; i < nums.length; i++) {
            for(int j = i + 1; j < nums.length; j++) {
                if(nums[i] == nums[j]) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

### How Brute Force Works
1. Compare each element with all other elements
2. Two nested loops:
   - Outer loop: selects current element
   - Inner loop: compares with all remaining elements
3. Return true if any match found

Example walkthrough:
```
nums = [1,2,3,1]

i=0: num=1
  j=1: 1≠2
  j=2: 1≠3
  j=3: 1=1 → return true

If no duplicates found, continue until end
```

## 2. HashSet Approach (Given Solution)
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> hs = new HashSet<>();
        for(int i = 0; i < nums.length; i++) {
            if(hs.contains(nums[i])) {
                return true;
            } else {
                hs.add(nums[i]);
            }
        }
        return false;
    }
}
```

### How HashSet Approach Works
1. Create empty HashSet
2. For each number:
   - Check if number exists in set
   - If exists → found duplicate
   - If not → add to set

Example walkthrough:
```
nums = [1,2,3,1]

Step 1: hs = {}
        check 1 → not found → add → hs = {1}

Step 2: hs = {1}
        check 2 → not found → add → hs = {1,2}

Step 3: hs = {1,2}
        check 3 → not found → add → hs = {1,2,3}

Step 4: hs = {1,2,3}
        check 1 → found! → return true
```

## 3. Sorting Approach
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums);
        for(int i = 1; i < nums.length; i++) {
            if(nums[i] == nums[i-1]) {
                return true;
            }
        }
        return false;
    }
}
```

### How Sorting Approach Works
1. Sort array in ascending order
2. Compare adjacent elements
3. If any adjacent elements are equal → duplicate found

Example walkthrough:
```
nums = [1,3,2,1]

Step 1: Sort array
        [1,1,2,3]

Step 2: Compare adjacent elements
        1=1 → return true
```

## Complexity Analysis

| Approach    | Time       | Space     | When to Use |
|-------------|------------|-----------|-------------|
| Brute Force | O(n²)      | O(1)      | Small arrays |
| HashSet     | O(n)       | O(n)      | Most cases |
| Sorting     | O(n log n) | O(1)      | Space constrained |

## Problem Variations and Links

1. [Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)
   - Find duplicates within k positions
   ```java
   public boolean containsNearbyDuplicate(int[] nums, int k) {
       Map<Integer, Integer> map = new HashMap<>();
       for(int i = 0; i < nums.length; i++) {
           if(map.containsKey(nums[i]) && i - map.get(nums[i]) <= k) {
               return true;
           }
           map.put(nums[i], i);
       }
       return false;
   }
   ```

2. [Contains Duplicate III](https://leetcode.com/problems/contains-duplicate-iii/)
   - Find duplicates within k distance and value difference t
   ```java
   public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
       TreeSet<Long> set = new TreeSet<>();
       for(int i = 0; i < nums.length; i++) {
           Long floor = set.floor((long)nums[i] + t);
           Long ceil = set.ceiling((long)nums[i] - t);
           if((floor != null && floor >= nums[i]) || 
              (ceil != null && ceil <= nums[i])) {
               return true;
           }
           set.add((long)nums[i]);
           if(i >= k) {
               set.remove((long)nums[i-k]);
           }
       }
       return false;
   }
   ```

## Companies That Asked This Question

1. **FAANG Companies**
   - Amazon
   - Facebook
   - Google
   - Apple
   - Microsoft

2. **Other Tech Companies**
   - Adobe
   - Oracle
   - Uber
   - LinkedIn
   - Twitter

3. **Financial Companies**
   - Goldman Sachs
   - Morgan Stanley
   - Bloomberg

Interview Frequency: Very High (Asked in 45+ top companies)

## Implementation Tips

1. **HashSet Approach**
   - Most efficient for general cases
   - Use when space is not constrained
   - Consider using `set.add()` return value
   ```java
   if(!set.add(num)) return true;  // More concise
   ```

2. **Sorting Approach**
   - Good when space is limited
   - Consider parallel sorting for large arrays
   - Watch for array modification requirements

3. **Edge Cases**
   ```java
   // Handle edge cases first
   if(nums == null || nums.length <= 1) return false;
   ```

## Additional Optimization Techniques

1. **Early Termination**
   ```java
   // If array length > possible unique elements
   if(nums.length > Integer.MAX_VALUE) return true;
   ```

2. **BitSet for Limited Range**
   ```java
   // If numbers are in small range
   BitSet bs = new BitSet(range);
   for(int num : nums) {
       if(bs.get(num)) return true;
       bs.set(num);
   }
   ```

3. **Stream API (Though less efficient)**
   ```java
   return nums.length != Arrays.stream(nums).distinct().count();
   ```

## Similar Problems
1. [Find All Duplicates in Array](https://leetcode.com/problems/find-all-duplicates-in-an-array/)
2. [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)
3. [Missing Number](https://leetcode.com/problems/missing-number/)

## Interview Success Tips
1. Always clarify input constraints
2. Discuss time-space tradeoffs
3. Start with HashSet solution
4. Be ready to optimize for space
5. Know how to handle edge cases
