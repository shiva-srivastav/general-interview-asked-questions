# Print All Subsequences: Comprehensive Notes

## Concept Overview

A **subsequence** is a sequence derived from another sequence by deleting some or no elements without changing the order of the remaining elements. For example, given a sequence [1, 2, 3], its subsequences include:
- []
- [1]
- [2]
- [3]
- [1, 2]
- [1, 3]
- [2, 3]
- [1, 2, 3]

## The "Take or Not Take" Approach

The core concept behind generating all subsequences is the "**Take or Not Take**" decision at each element:
1. For each element in the array, we have two choices:
   - **Take** the element (include it in the current subsequence)
   - **Not Take** the element (exclude it from the current subsequence)
2. We make this decision recursively for each element in the original sequence
3. When we reach the end of the sequence, we've built one complete subsequence

## Recursive Tree Visualization

For an array like [1, 2, 3], the recursive tree can be visualized as:

```mermaid
graph TD
    A[Start: []] --> B["Take 1: [1]"]
    A --> C["Not Take 1: []"]
    
    B --> D["Take 2: [1,2]"]
    B --> E["Not Take 2: [1]"]
    
    C --> F["Take 2: [2]"]
    C --> G["Not Take 2: []"]
    
    D --> H["Take 3: [1,2,3]"]
    D --> I["Not Take 3: [1,2]"]
    
    E --> J["Take 3: [1,3]"]
    E --> K["Not Take 3: [1]"]
    
    F --> L["Take 3: [2,3]"]
    F --> M["Not Take 3: [2]"]
    
    G --> N["Take 3: [3]"]
    G --> O["Not Take 3: []"]
    
    H --> P["Print: [1,2,3]"]
    I --> Q["Print: [1,2]"]
    J --> R["Print: [1,3]"]
    K --> S["Print: [1]"]
    L --> T["Print: [2,3]"]
    M --> U["Print: [2]"]
    N --> V["Print: [3]"]
    O --> W["Print: []"]
    
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style P,Q,R,S,T,U,V,W fill:#bfb,stroke:#333,stroke-width:2px
```

This diagram shows how we build each subsequence by making a binary choice (take/not take) for each element, resulting in 2^n possible subsequences.

## Time and Space Complexity

- **Time Complexity**: O(2^n) where n is the length of the array
  - For each element, we make 2 recursive calls
- **Space Complexity**: O(n) for the recursion stack

## Implementation Strategy

1. Create a recursive function that takes:
   - The original array
   - The current index
   - A data structure to store the current subsequence
2. Make two recursive calls at each step:
   - One that includes the current element (Take)
   - One that excludes the current element (Not Take)
3. Base case: When the index reaches the end of the array, print or store the subsequence

## Java Implementation

```java
import java.util.ArrayList;
import java.util.List;

public class PrintAllSubsequences {
    
    public static void main(String[] args) {
        int[] arr = {1, 2, 3};
        System.out.println("Array: [1, 2, 3]");
        System.out.println("All subsequences:");
        
        List<Integer> ds = new ArrayList<>();
        printSubsequences(0, ds, arr);
        
        // Variation: Print subsequences with sum equal to target
        int targetSum = 3;
        System.out.println("\nSubsequences with sum equal to " + targetSum + ":");
        printSubsequencesWithSum(0, ds, arr, 0, targetSum);
    }
    
    /**
     * Recursive function to print all subsequences using take/not-take approach
     * 
     * @param index Current index in the array
     * @param ds Data structure to store current subsequence
     * @param arr Original array
     */
    private static void printSubsequences(int index, List<Integer> ds, int[] arr) {
        // Base case: We've processed all elements
        if (index == arr.length) {
            // Print the subsequence
            if (ds.isEmpty()) {
                System.out.println("[]");
            } else {
                System.out.println(ds);
            }
            return;
        }
        
        // Decision 1: Take the current element
        ds.add(arr[index]);
        printSubsequences(index + 1, ds, arr);
        
        // Decision 2: Not take the current element (backtrack)
        ds.remove(ds.size() - 1);
        printSubsequences(index + 1, ds, arr);
    }
    
    /**
     * Variation: Print subsequences with sum equal to target
     * 
     * @param index Current index in the array
     * @param ds Data structure to store current subsequence
     * @param arr Original array
     * @param currentSum Current sum of elements in the subsequence
     * @param targetSum Target sum to achieve
     */
    private static void printSubsequencesWithSum(int index, List<Integer> ds, int[] arr, 
                                               int currentSum, int targetSum) {
        // Base case: We've processed all elements
        if (index == arr.length) {
            // Check if current subsequence has the target sum
            if (currentSum == targetSum) {
                if (ds.isEmpty()) {
                    System.out.println("[]");
                } else {
                    System.out.println(ds);
                }
            }
            return;
        }
        
        // Decision 1: Take the current element
        ds.add(arr[index]);
        printSubsequencesWithSum(index + 1, ds, arr, currentSum + arr[index], targetSum);
        
        // Decision 2: Not take the current element (backtrack)
        ds.remove(ds.size() - 1);
        printSubsequencesWithSum(index + 1, ds, arr, currentSum, targetSum);
    }
    
    /**
     * Variation: Count subsequences with sum equal to target
     * Returns the count instead of printing
     */
    private static int countSubsequencesWithSum(int index, int[] arr, int currentSum, int targetSum) {
        // Base case: We've processed all elements
        if (index == arr.length) {
            // Check if current subsequence has the target sum
            if (currentSum == targetSum) {
                return 1; // Found one valid subsequence
            }
            return 0; // No valid subsequence found
        }
        
        // Decision 1: Take the current element
        int take = countSubsequencesWithSum(index + 1, arr, currentSum + arr[index], targetSum);
        
        // Decision 2: Not take the current element
        int notTake = countSubsequencesWithSum(index + 1, arr, currentSum, targetSum);
        
        // Return the total count
        return take + notTake;
    }
    
    /**
     * Variation: Check if any subsequence exists with the target sum
     * Returns true/false instead of printing
     */
    private static boolean hasSubsequenceWithSum(int index, int[] arr, int currentSum, int targetSum) {
        // Base case: We've processed all elements
        if (index == arr.length) {
            // Check if current subsequence has the target sum
            return currentSum == targetSum;
        }
        
        // Decision 1: Take the current element
        boolean take = hasSubsequenceWithSum(index + 1, arr, currentSum + arr[index], targetSum);
        
        // If already found, no need to explore further
        if (take) return true;
        
        // Decision 2: Not take the current element
        boolean notTake = hasSubsequenceWithSum(index + 1, arr, currentSum, targetSum);
        
        // Return true if either choice leads to a valid subsequence
        return take || notTake;
    }
}
```

## Execution Trace Example (for array [1, 2, 3])

Let's trace through the execution of the algorithm for the array [1, 2, 3]:

1. Start with empty subsequence []
2. For index 0 (element 1):
   - **Take**: Add 1 to subsequence → [1], recurse to index 1
   - **Not Take**: Keep subsequence as [], recurse to index 1
3. For index 1 (element 2) with [1]:
   - **Take**: Add 2 to subsequence → [1, 2], recurse to index 2
   - **Not Take**: Keep subsequence as [1], recurse to index 2
4. For index 1 (element 2) with []:
   - **Take**: Add 2 to subsequence → [2], recurse to index 2
   - **Not Take**: Keep subsequence as [], recurse to index 2
5. Continue until all paths reach index 3 (base case)
6. At each base case, print the current subsequence

## Common Subsequence Problems and Variations

1. **Sum of Subsequence Equals K**:
   - Modify the base case to check if sum equals K
   - Only print/count subsequences that satisfy the condition

2. **Count Subsequences with Sum K**:
   - Return 1 when sum equals K
   - Return 0 otherwise
   - Sum the results from both recursive calls

3. **Check if Any Subsequence Exists with Sum K**:
   - Return true if sum equals K
   - Return result1 OR result2 from recursive calls

## Key Insights

1. The "Take or Not Take" approach creates a binary decision tree
2. Each leaf node of the tree represents one possible subsequence
3. Total number of subsequences is 2^n (including empty subsequence)
4. This approach can be adapted to solve various subsequence problems
5. Backtracking is essential to maintain correct state during recursion
