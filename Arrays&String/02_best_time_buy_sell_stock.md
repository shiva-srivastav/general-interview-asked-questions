# Best Time to Buy and Sell Stock - Complete Guide

## Problem Statement
Given an array `prices` where `prices[i]` is the price of a given stock on the ith day, find the maximum profit you can get by buying and selling once. You can only sell after buying.

### Examples
```
Example 1:
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5

Example 2:
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: No profit possible, return 0
```

## 1. Brute Force Solution

### Kadane's State Transition Rules
1. Every element has two possibilities:
   - Be part of current subproblem (buying price)
   - Start a new subproblem (reset minimum price)

2. At each price, we:
   - Update minimum price if current is lower
   - Calculate potential profit with current price
   - Update maximum profit if current profit is higher

### Implementation
```java
class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        
        for(int i = 0; i < prices.length; i++) {
            for(int j = i + 1; j < prices.length; j++) {
                int profit = prices[j] - prices[i];
                maxProfit = Math.max(maxProfit, profit);
            }
        }
        
        return maxProfit;
    }
}
```

### Technique Explanation
- Uses two nested loops
- Outer loop: buying day
- Inner loop: selling day
- Calculate profit for each combination
- Keep track of maximum profit

### Visual Example
```
Array: [7,1,5,3,6,4]

Buy at 7:
7 → 1 = -6
7 → 5 = -2
7 → 3 = -4
7 → 6 = -1
7 → 4 = -3

Buy at 1:
1 → 5 = 4
1 → 3 = 2
1 → 6 = 5 ← Maximum profit
1 → 4 = 3
...
```

### Complexity
- Time: O(n²)
- Space: O(1)

## 2. Maximum Array Solution

### Implementation
```java
class Solution {
    public int maxProfit(int[] prices) {
        int maxBlock[] = new int[prices.length];
        
        // Initialize last element
        maxBlock[prices.length-1] = prices[prices.length-1];
        int max = 0;
        
        // Process from right to left
        for(int i = prices.length-2; i >= 0; i--) {
            if(prices[i] > maxBlock[i+1]) {
                maxBlock[i] = prices[i];
                max = Math.max(max, 0);
            } else {
                maxBlock[i] = maxBlock[i+1];
                max = Math.max(max, maxBlock[i] - prices[i]);
            }
        }
        
        return max;
    }
}
```

### Technique Explanation
1. Create array to store maximum future prices
2. Process right to left
3. For each position:
   - If current price > next maximum:
     * Update maximum
     * No profit possible from buying here
   - Else:
     * Keep previous maximum
     * Calculate potential profit

### Visual Step-by-Step
```
Array: [7,1,5,3,6,4]

Initial:
prices:   [7,1,5,3,6,4]
maxBlock: [?,?,?,?,?,4]

Step 1 (i=4):
prices:   [7,1,5,3,6,4]
maxBlock: [?,?,?,?,6,4]
max = 0

Step 2 (i=3):
prices:   [7,1,5,3,6,4]
maxBlock: [?,?,?,6,6,4]
max = max(0, 6-3) = 3

Step 3 (i=2):
prices:   [7,1,5,3,6,4]
maxBlock: [?,?,6,6,6,4]
max = max(3, 6-5) = 3

Step 4 (i=1):
prices:   [7,1,5,3,6,4]
maxBlock: [?,6,6,6,6,4]
max = max(3, 6-1) = 5

Step 5 (i=0):
prices:   [7,1,5,3,6,4]
maxBlock: [7,6,6,6,6,4]
max = 5
```

### Complexity
- Time: O(n)
- Space: O(n)

## 3. Optimized Solution (Kadane's Variant)

### Understanding Kadane's Algorithm
Kadane's algorithm was originally designed to find the maximum subarray sum. The stock problem is a variant of this, where instead of finding maximum sum, we're finding maximum difference.

#### Original Kadane's Algorithm
```java
// For maximum subarray sum
public int maxSubArray(int[] nums) {
    int currentSum = 0;
    int maxSum = Integer.MIN_VALUE;
    
    for(int num : nums) {
        currentSum = Math.max(num, currentSum + num);
        maxSum = Math.max(maxSum, currentSum);
    }
    return maxSum;
}
```

#### State Transition in Stock Problem
In the stock problem, we modify Kadane's concept to track:
1. Minimum price seen so far (analogous to currentSum)
2. Maximum profit possible (analogous to maxSum)

The state transition follows these rules:
```
For each price:
1. minPrice = min(minPrice, currentPrice)
   - This tracks best buying opportunity
   
2. maxProfit = max(maxProfit, currentPrice - minPrice)
   - This tracks best selling opportunity
```

### State Management Visualization
```
Array: [7,1,5,3,6,4]

Initial State:
minPrice = Integer.MAX_VALUE
maxProfit = 0

States for each price:

price = 7
┌─────────────────┐
│ minPrice = 7    │ ← First potential buying price
│ maxProfit = 0   │ ← No profit yet
└─────────────────┘

price = 1
┌─────────────────┐
│ minPrice = 1    │ ← Better buying price found
│ maxProfit = 0   │ ← Still no profit
└─────────────────┘

price = 5
┌─────────────────┐
│ minPrice = 1    │ ← Keep best buying price
│ maxProfit = 4   │ ← Found potential profit (5-1)
└─────────────────┘

price = 3
┌─────────────────┐
│ minPrice = 1    │ ← Keep best buying price
│ maxProfit = 4   │ ← Keep better profit (3-1 < 4)
└─────────────────┘

price = 6
┌─────────────────┐
│ minPrice = 1    │ ← Keep best buying price
│ maxProfit = 5   │ ← Better profit found (6-1)
└─────────────────┘

price = 4
┌─────────────────┐
│ minPrice = 1    │ ← Keep best buying price
│ maxProfit = 5   │ ← Keep better profit (4-1 < 5)
└─────────────────┘
```

### Why This State Management Works
1. **Minimum Price State**
   - Always keeps track of best buying opportunity
   - Only updates when we find a better (lower) price
   - Represents the "valley" in our price chart

2. **Maximum Profit State**
   - Calculates potential profit at each step
   - Only updates when we find a better profit
   - Represents the best "peak-valley" difference

3. **State Invariants**
   - minPrice is always the minimum price seen so far
   - maxProfit is always the best profit possible using prices seen so far
   - Current price - minPrice represents potential profit if we sell now

### Implementation
```java
class Solution {
    public int maxProfit(int[] prices) {
        int minPrice = Integer.MAX_VALUE;
        int maxProfit = 0;
        
        for(int price : prices) {
            minPrice = Math.min(minPrice, price);
            maxProfit = Math.max(maxProfit, price - minPrice);
        }
        
        return maxProfit;
    }
}
```

### Technique Explanation
1. Track minimum price seen so far
2. For each price:
   - Update minimum price if current is lower
   - Calculate potential profit using current price
   - Update maximum profit if better

### Visual Step-by-Step
```
Array: [7,1,5,3,6,4]

Step 1: price = 7
minPrice = 7
maxProfit = 0
State: [7],1,5,3,6,4

Step 2: price = 1
minPrice = 1
maxProfit = 0
State: 7,[1],5,3,6,4

Step 3: price = 5
minPrice = 1
maxProfit = 4
State: 7,1,[5],3,6,4

Step 4: price = 3
minPrice = 1
maxProfit = 4
State: 7,1,5,[3],6,4

Step 5: price = 6
minPrice = 1
maxProfit = 5
State: 7,1,5,3,[6],4

Step 6: price = 4
minPrice = 1
maxProfit = 5
State: 7,1,5,3,6,[4]
```

### Complexity
- Time: O(n)
- Space: O(1)

## Comparison of Approaches

### 1. Brute Force
Pros:
- Simple to understand
- No extra space
- Good for small arrays

Cons:
- Very slow for large arrays
- Quadratic time complexity

### 2. Maximum Array
Pros:
- Linear time complexity
- Tracks future maximums
- Easy to modify for variants

Cons:
- Uses extra space
- More complex implementation

### 3. Kadane's Variant
Pros:
- Linear time complexity
- Constant space
- Simple implementation

Cons:
- Less intuitive
- Harder to modify for variants

## Common Patterns & Techniques

### 1. Valley-Peak Pattern
```
        Peak
         ↑
Price    6
         ↑
    5 →  ↑
    ↑    ↑
    ↑  3 ↑
    ↑    ↑ 4
7   ↑    ↑
    1    ↑
Valley
```
- Find lowest valley followed by highest peak
- Used in optimized solution

### 2. Future Information Pattern
```
Current day: Need to know best future price
[7,1,5,3,6,4]
   ↑     ↑
   Buy   Sell
```
- Used in maximum array solution
- Helps make optimal decisions

### 3. State Tracking Pattern
```
Track state:
- Minimum price so far
- Maximum profit so far
```
- Used in optimized solution
- Minimizes space usage

## Edge Cases to Handle
1. Array is empty or has one element
2. Prices only decrease
3. Same price throughout
4. Very large price differences
5. Negative prices (if allowed)

## Code Templates

### Basic Template
```java
public int maxProfit(int[] prices) {
    // Handle edge cases
    if(prices == null || prices.length < 2) return 0;
    
    // Main logic
    
    // Return result
}
```

### State Tracking Template
```java
public int maxProfit(int[] prices) {
    // Initialize states
    int state1 = initialization;
    int state2 = initialization;
    
    // Process array
    for(int price : prices) {
        // Update states
        state1 = someCalculation;
        state2 = someCalculation;
    }
    
    // Return result
    return finalCalculation;
}
```

## Problem Variations
1. Multiple transactions allowed
2. K transactions allowed
3. With cooldown period
4. With transaction fee
5. With holding period

Each variation might require different techniques, but understanding these base approaches helps in solving them.
