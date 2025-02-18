# Product of Array Except Self - Complete Analysis

## Problem Link
[LeetCode 238 - Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

## Problem Statement
Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`. The algorithm should run in O(n) time and without using the division operation.

Example:
```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
Explanation: 
answer[0] = 2*3*4 = 24
answer[1] = 1*3*4 = 12
answer[2] = 1*2*4 = 8
answer[3] = 1*2*3 = 6
```

## 1. Brute Force Approach
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] result = new int[nums.length];
        
        for(int i = 0; i < nums.length; i++) {
            int product = 1;
            for(int j = 0; j < nums.length; j++) {
                if(i != j) {
                    product *= nums[j];
                }
            }
            result[i] = product;
        }
        return result;
    }
}
```

### How Brute Force Works
1. For each index i:
   - Calculate product of all elements except nums[i]
   - Store in result array

Example walkthrough:
```
nums = [1,2,3,4]

i=0: product = 2*3*4 = 24
i=1: product = 1*3*4 = 12
i=2: product = 1*2*4 = 8
i=3: product = 1*2*3 = 6

result = [24,12,8,6]
```

## 2. Left and Right Product Arrays Approach
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] leftProducts = new int[n];
        int[] rightProducts = new int[n];
        int[] result = new int[n];
        
        // Calculate left products
        leftProducts[0] = 1;
        for(int i = 1; i < n; i++) {
            leftProducts[i] = leftProducts[i-1] * nums[i-1];
        }
        
        // Calculate right products
        rightProducts[n-1] = 1;
        for(int i = n-2; i >= 0; i--) {
            rightProducts[i] = rightProducts[i+1] * nums[i+1];
        }
        
        // Combine left and right products
        for(int i = 0; i < n; i++) {
            result[i] = leftProducts[i] * rightProducts[i];
        }
        
        return result;
    }
}
```

### How Left-Right Products Works
1. Create two arrays for left and right products
2. Fill left products array from left to right
3. Fill right products array from right to left
4. Multiply corresponding elements

Example walkthrough:
```
nums = [1,2,3,4]

Left products:
[1, 1, 2, 6]
    ↑  ↑  ↑
    1  2  3

Right products:
[24,12,4,1]
     ↑  ↑  ↑
     3  4  1

Result = left * right:
[24,12,8,6]
```

## 3. Space Optimized Approach (Detailed)

### Implementation
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        
        // Calculate left products first
        result[0] = 1;  // nothing to left of first element
        for(int i = 1; i < n; i++) {
            result[i] = result[i-1] * nums[i-1];
        }
        
        // Calculate right products and combine
        int rightProduct = 1;
        for(int i = n-1; i >= 0; i--) {
            result[i] = result[i] * rightProduct;
            rightProduct *= nums[i];
        }
        
        return result;
    }
}
```

### Detailed Step-by-Step Explanation
Let's use the example: nums = [1,2,3,4]

1. **First Pass (Left Products)**
   ```
   Initialize result[0] = 1 (no elements to the left)
   
   result = [1,_,_,_]
   
   i=1: result[1] = result[0] * nums[0] = 1 * 1 = 1
   result = [1,1,_,_]
   
   i=2: result[2] = result[1] * nums[1] = 1 * 2 = 2
   result = [1,1,2,_]
   
   i=3: result[3] = result[2] * nums[2] = 2 * 3 = 6
   result = [1,1,2,6]
   ```
   After first pass, each position contains product of all numbers to its left.

2. **Second Pass (Right Products and Combine)**
   ```
   Initialize rightProduct = 1 (no elements to the right)
   
   i=3: result[3] = result[3] * rightProduct = 6 * 1 = 6
        rightProduct = rightProduct * nums[3] = 1 * 4 = 4
   result = [1,1,2,6]
   
   i=2: result[2] = result[2] * rightProduct = 2 * 4 = 8
        rightProduct = rightProduct * nums[2] = 4 * 3 = 12
   result = [1,1,8,6]
   
   i=1: result[1] = result[1] * rightProduct = 1 * 12 = 12
        rightProduct = rightProduct * nums[1] = 12 * 2 = 24
   result = [1,12,8,6]
   
   i=0: result[0] = result[0] * rightProduct = 1 * 24 = 24
        rightProduct = rightProduct * nums[0] = 24 * 1 = 24
   result = [24,12,8,6]
   ```

### Visual Representation of Process
```
Original array: [1,2,3,4]

Left products accumulation:
Step 1: [1,_,_,_]    // Initialize first element as 1
Step 2: [1,1,_,_]    // 1 (no left elements)
Step 3: [1,1,2,_]    // 1*2
Step 4: [1,1,2,6]    // 1*2*3

Right products and combination:
Step 1: [1,1,2,6]    // rightProduct = 1
Step 2: [1,1,8,6]    // rightProduct = 4
Step 3: [1,12,8,6]   // rightProduct = 12
Step 4: [24,12,8,6]  // rightProduct = 24
```

### Key Insights
1. **Space Optimization**
   - Uses result array to store left products
   - Uses single variable (rightProduct) for right products
   - Combines results in-place during second pass

2. **Two-Pass Strategy**
   - First pass: Build left products
   - Second pass: Multiply by right products
   - Each element gets products from both sides

3. **Why It Works**
   - For each position i:
     * After first pass: Contains product of all numbers to the left
     * During second pass: Multiplied by product of all numbers to the right
     * Final result: Product of all numbers except self
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        
        // Calculate left products
        result[0] = 1;
        for(int i = 1; i < n; i++) {
            result[i] = result[i-1] * nums[i-1];
        }
        
        // Calculate right products and combine
        int rightProduct = 1;
        for(int i = n-1; i >= 0; i--) {
            result[i] = result[i] * rightProduct;
            rightProduct *= nums[i];
        }
        
        return result;
    }
}
```

### How Space Optimized Approach Works
1. Use result array to store left products
2. Use single variable for right product
3. Combine in second pass

Example walkthrough:
```
nums = [1,2,3,4]

After left pass:
result = [1,1,2,6]

During right pass:
rightProduct = 1
i=3: result[3] = 6*1 = 6,  rightProduct = 4
i=2: result[2] = 2*4 = 8,  rightProduct = 12
i=1: result[1] = 1*12 = 12, rightProduct = 24
i=0: result[0] = 1*24 = 24, rightProduct = 24

Final result = [24,12,8,6]
```

## Complexity Analysis

| Approach              | Time | Space  | When to Use |
|----------------------|------|---------|-------------|
| Brute Force          | O(n²)| O(1)    | Never (inefficient) |
| Left-Right Products  | O(n) | O(n)    | When space is not a constraint |
| Space Optimized      | O(n) | O(1)    | Most cases (optimal) |

## Problem Variations and Links

1. [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)
   ```java
   public int maxProduct(int[] nums) {
       int max = nums[0], min = nums[0], result = nums[0];
       for(int i = 1; i < nums.length; i++) {
           int temp = max;
           max = Math.max(Math.max(nums[i], max * nums[i]), min * nums[i]);
           min = Math.min(Math.min(nums[i], temp * nums[i]), min * nums[i]);
           result = Math.max(result, max);
       }
       return result;
   }
   ```

2. [Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/)
   ```java
   public int numSubarrayProductLessThanK(int[] nums, int k) {
       if(k <= 1) return 0;
       int product = 1, count = 0, left = 0;
       for(int right = 0; right < nums.length; right++) {
           product *= nums[right];
           while(product >= k) product /= nums[left++];
           count += right - left + 1;
       }
       return count;
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
   - Uber
   - Lyft

3. **Financial Companies**
   - Bloomberg
   - Goldman Sachs
   - JPMorgan

Interview Frequency: Very High (Asked in 40+ top companies)

## Implementation Tips

1. **Handle Edge Cases**
   ```java
   if(nums == null || nums.length == 0) return new int[0];
   if(nums.length == 1) return new int[]{0};
   ```

2. **Avoid Division**
   - Problem specifically asks not to use division
   - Handle zero values carefully

3. **Space Optimization**
   ```java
   // Use input array as one of the product arrays if allowed
   // Use single variable for running product
   ```

## Additional Optimization Techniques

1. **Handle Zeros**
   ```java
   // Count zeros
   // If more than one zero, result is all zeros
   // If exactly one zero, only that position has non-zero value
   ```

2. **Early Termination**
   ```java
   // If array contains more than one zero
   if(zeroCount > 1) return new int[n]; // all zeros
   ```

## Similar Problems
1. [Maximum Product of Three Numbers](https://leetcode.com/problems/maximum-product-of-three-numbers/)
2. [Product of Array Except Self Without Space](https://leetcode.com/problems/product-of-array-except-self-follow-up/)
3. [Array of Doubled Pairs](https://leetcode.com/problems/array-of-doubled-pairs/)

## Interview Success Tips
1. Start with clarifying questions about constraints
2. Mention the O(n) time requirement
3. Discuss the no-division constraint
4. Be ready to optimize space usage
5. Handle edge cases (zeros, negative numbers)
