# Maximum Subarray - Complete Analysis

## Problem Link
[LeetCode 53 - Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

## Problem Statement
Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

Example:
```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: The subarray [4,-1,2,1] has the largest sum = 6
```

## 1. Brute Force Approach
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSum = Integer.MIN_VALUE;
        
        for(int i = 0; i < nums.length; i++) {
            int currentSum = 0;
            for(int j = i; j < nums.length; j++) {
                currentSum += nums[j];
                maxSum = Math.max(maxSum, currentSum);
            }
        }
        return maxSum;
    }
}
```

### How Brute Force Works
1. Consider every possible subarray
2. Calculate sum of each subarray
3. Keep track of maximum sum found

Example walkthrough:
```
nums = [-2,1,-3,4]

Starting at i=0 (-2):
Sum: -2, 1-2=-1, -1-3=-4, -4+4=0
MaxSum = 0

Starting at i=1 (1):
Sum: 1, 1-3=-2, -2+4=2
MaxSum = 2

Starting at i=2 (-3):
Sum: -3, -3+4=1
MaxSum = 2

Starting at i=3 (4):
Sum: 4
MaxSum = 4
```

## 2. Kadane's Algorithm (Given Solution)
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int currSum = 0;
        int maxSum = Integer.MIN_VALUE;
        
        for(int i = 0; i < nums.length; i++) {
            currSum = Math.max(nums[i], currSum + nums[i]);
            maxSum = Math.max(currSum, maxSum);
        }
        return maxSum;
    }
}
```

### How Kadane's Algorithm Works
1. Keep track of current sum (currSum) and maximum sum (maxSum)
2. For each number, decide whether to:
   - Start new subarray (take current number)
   - Extend existing subarray (add to current sum)
3. Update maximum sum seen so far

Example walkthrough:
```
nums = [-2,1,-3,4,-1,2,1,-5,4]

i=0: num = -2
currSum = max(-2, 0 + -2) = -2
maxSum = max(-2, MIN_VALUE) = -2

i=1: num = 1
currSum = max(1, -2 + 1) = 1
maxSum = max(1, -2) = 1

i=2: num = -3
currSum = max(-3, 1 + -3) = -2
maxSum = max(-2, 1) = 1

i=3: num = 4
currSum = max(4, -2 + 4) = 4
maxSum = max(4, 1) = 4

...and so on
```

## 3. Divide and Conquer Approach
```java
class Solution {
    public int maxSubArray(int[] nums) {
        return maxSubArrayHelper(nums, 0, nums.length - 1);
    }
    
    private int maxSubArrayHelper(int[] nums, int left, int right) {
        if(left == right) return nums[left];
        
        int mid = left + (right - left) / 2;
        
        int leftMax = maxSubArrayHelper(nums, left, mid);
        int rightMax = maxSubArrayHelper(nums, mid + 1, right);
        int crossMax = maxCrossingSum(nums, left, mid, right);
        
        return Math.max(Math.max(leftMax, rightMax), crossMax);
    }
    
    private int maxCrossingSum(int[] nums, int left, int mid, int right) {
        int leftSum = Integer.MIN_VALUE;
        int sum = 0;
        for(int i = mid; i >= left; i--) {
            sum += nums[i];
            leftSum = Math.max(leftSum, sum);
        }
        
        int rightSum = Integer.MIN_VALUE;
        sum = 0;
        for(int i = mid + 1; i <= right; i++) {
            sum += nums[i];
            rightSum = Math.max(rightSum, sum);
        }
        
        return leftSum + rightSum;
    }
}
```

### How Divide and Conquer Works
1. Divide array into two halves
2. Recursively find maximum subarray sum in:
   - Left half
   - Right half
   - Crossing middle
3. Return maximum of these three values

## Complexity Analysis

| Approach           | Time       | Space     | When to Use |
|-------------------|------------|-----------|-------------|
| Brute Force       | O(n²)      | O(1)      | Never (inefficient) |
| Kadane's          | O(n)       | O(1)      | Most cases (optimal) |
| Divide & Conquer  | O(n log n) | O(log n)  | When parallelization needed |

## Problem Variations and Links

1. [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)
   ```java
   public int maxProduct(int[] nums) {
       int max = nums[0], min = nums[0], result = nums[0];
       for(int i = 1; i < nums.length; i++) {
           int tempMax = Math.max(nums[i], Math.max(max * nums[i], min * nums[i]));
           min = Math.min(nums[i], Math.min(max * nums[i], min * nums[i]));
           max = tempMax;
           result = Math.max(result, max);
       }
       return result;
   }
   ```

2. [Maximum Subarray Sum After One Operation](https://leetcode.com/problems/maximum-subarray-sum-after-one-operation/)
   ```java
   public int maxSumAfterOperation(int[] nums) {
       int maxNormal = 0, maxOneSquare = 0, result = Integer.MIN_VALUE;
       for(int num : nums) {
           maxOneSquare = Math.max(Math.max(maxNormal + num * num, maxOneSquare + num), num * num);
           maxNormal = Math.max(maxNormal + num, num);
           result = Math.max(result, Math.max(maxNormal, maxOneSquare));
       }
       return result;
   }
   ```

## Companies That Asked This Question

1. **FAANG Companies**
   - Facebook (Meta)
   - Amazon
   - Apple
   - Netflix
   - Google

2. **Other Tech Giants**
   - Microsoft
   - LinkedIn
   - Adobe
   - Oracle
   - Twitter

3. **Financial Companies**
   - Bloomberg
   - Goldman Sachs
   - Morgan Stanley

Interview Frequency: Extremely High (Asked in 50+ top companies)

## Implementation Tips

1. **Handle Edge Cases**
   ```java
   if(nums == null || nums.length == 0) return 0;
   if(nums.length == 1) return nums[0];
   ```

2. **Kadane's Algorithm Optimization**
   ```java
   // Use first element as initial values
   int currSum = nums[0];
   int maxSum = nums[0];
   // Start loop from index 1
   for(int i = 1; i < nums.length; i++)
   ```

3. **Follow-up Questions**
   - What if array is circular?
   - What if we need to print the subarray?
   - What if negative numbers are not allowed?

## Additional Approaches

1. **Dynamic Programming**
   ```java
   public int maxSubArray(int[] nums) {
       int[] dp = new int[nums.length];
       dp[0] = nums[0];
       int maxSum = dp[0];
       
       for(int i = 1; i < nums.length; i++) {
           dp[i] = Math.max(nums[i], dp[i-1] + nums[i]);
           maxSum = Math.max(maxSum, dp[i]);
       }
       return maxSum;
   }
   ```

2. **Print Maximum Subarray**
   ```java
   public int[] maxSubArrayWithIndices(int[] nums) {
       int currSum = nums[0], maxSum = nums[0];
       int start = 0, end = 0, tempStart = 0;
       
       for(int i = 1; i < nums.length; i++) {
           if(nums[i] > currSum + nums[i]) {
               currSum = nums[i];
               tempStart = i;
           } else {
               currSum = currSum + nums[i];
           }
           
           if(currSum > maxSum) {
               maxSum = currSum;
               start = tempStart;
               end = i;
           }
       }
       return Arrays.copyOfRange(nums, start, end + 1);
   }
   ```

## Similar Problems
1. [Maximum Circular Subarray Sum](https://leetcode.com/problems/maximum-sum-circular-subarray/)
2. [Maximum Absolute Sum of Any Subarray](https://leetcode.com/problems/maximum-absolute-sum-of-any-subarray/)
3. [Maximum Sum of Two Non-Overlapping Subarrays](https://leetcode.com/problems/maximum-sum-of-two-non-overlapping-subarrays/)

## Interview Success Tips
1. Start with Kadane's Algorithm solution
2. Be ready to explain why it works
3. Know how to handle negative numbers
4. Be prepared for follow-up questions
5. Consider edge cases
