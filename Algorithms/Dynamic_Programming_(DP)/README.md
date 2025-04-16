# Dynamic Programming (DP)

## Basic Concept

**Conceptual Explanation:**
Dynamic Programming is an algorithmic technique for solving complex problems by breaking them down into simpler overlapping subproblems and solving each subproblem only once, storing the results to avoid redundant computations. DP is typically applied to optimization problems where we seek the best solution among many possible solutions.

DP is based on two key concepts:
1. **Optimal Substructure**: If the optimal solution of a problem can be constructed from optimal solutions of its subproblems.
2. **Overlapping Subproblems**: When the same subproblems are solved multiple times.

There are two main approaches to implement DP:
1. **Top-down (Memoization)**: Start with the original problem, break it down recursively, and use a cache to store results of subproblems.
2. **Bottom-up (Tabulation)**: Start with the smallest subproblems, solve them first, and use their results to solve larger problems.

**Visual Example:**
```
Fibonacci Sequence Example:
To compute fib(5):

Recursive approach without DP (inefficient):
                       fib(5)
                    /        \
                fib(4)        fib(3)
               /     \       /     \
           fib(3)   fib(2)  fib(2)  fib(1)
          /    \    /    \  /    \
      fib(2) fib(1) fib(1) fib(0) fib(1) fib(0)
     /    \
 fib(1) fib(0)

Notice how fib(3), fib(2), fib(1), fib(0) are calculated multiple times.

With DP (memoization):
- Calculate fib(0) = 0, fib(1) = 1 (base cases)
- Calculate fib(2) = fib(1) + fib(0) = 1 (store it)
- Calculate fib(3) = fib(2) + fib(1) = 2 (store it)
- Calculate fib(4) = fib(3) + fib(2) = 3 (store it)
- Calculate fib(5) = fib(4) + fib(3) = 5 (store it)

Each subproblem is solved only once.
```

**JavaScript Implementation (General Templates):**

```javascript
// Top-down approach (Memoization)
function dpTopDown(problemParams) {
  // Create cache/memo table
  const memo = new Map();
  
  // Define the recursive function with memoization
  function solve(params) {
    // Convert params to a key for the memo table
    const key = JSON.stringify(params);
    
    // Check if this subproblem has already been solved
    if (memo.has(key)) {
      return memo.get(key);
    }
    
    // Base case(s)
    if (isBaseCase(params)) {
      return baseCaseSolution(params);
    }
    
    // Recursive case: break into subproblems
    const result = combineSubproblems(params, solve);
    
    // Store the result in memo table
    memo.set(key, result);
    return result;
  }
  
  return solve(problemParams);
}

// Bottom-up approach (Tabulation)
function dpBottomUp(problemParams) {
  // Initialize DP table
  const dp = initializeTable(problemParams);
  
  // Fill the base cases
  fillBaseCases(dp);
  
  // Fill the table iteratively
  for (let i = /* start after base cases */; i < /* end at original problem */; i++) {
    dp[i] = calculateFromPreviousEntries(dp, i);
  }
  
  // Return the result for the original problem
  return dp[originalProblemIndex];
}
```

**Pseudocode:**
```
ALGORITHM DynamicProgramming:
    // Top-down approach (Memoization)
    FUNCTION SolveTopDown(problem):
        memo = new Map()
        
        FUNCTION Solve(params):
            key = stringify(params)
            
            IF key in memo THEN
                RETURN memo[key]
            END IF
            
            IF IsBaseCase(params) THEN
                RETURN BaseCaseSolution(params)
            END IF
            
            result = CombineSubproblems(params, Solve)
            memo[key] = result
            RETURN result
        END FUNCTION
        
        RETURN Solve(problem)
    END FUNCTION
    
    // Bottom-up approach (Tabulation)
    FUNCTION SolveBottomUp(problem):
        dp = InitializeTable(problem)
        FillBaseCases(dp)
        
        FOR i = start after base cases TO original problem:
            dp[i] = CalculateFromPreviousEntries(dp, i)
        END FOR
        
        RETURN dp[originalProblemIndex]
    END FUNCTION
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: Varies by problem but typically O(n) or O(n²) where n is the input size
- Space Complexity: O(n) or O(n²) for the memoization table or DP array

**Real-world Applications:**
- Resource allocation and optimization
- Sequence alignment in bioinformatics
- Shortest path algorithms in networks
- Text similarity and spell checking
- Financial modeling and investment strategies
- Game theory and strategy optimization
- Computer graphics (e.g., seam carving)

## 1. Fibonacci Sequence

**Conceptual Explanation:**
The Fibonacci sequence is a classic example of a problem with overlapping subproblems. Each Fibonacci number is the sum of the two preceding ones, starting from 0 and 1.

**Visual Example:**
```
Fibonacci Sequence: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ...

To calculate fib(6) = 8:
- fib(6) = fib(5) + fib(4)
- fib(5) = fib(4) + fib(3) = 5
- fib(4) = fib(3) + fib(2) = 3
- fib(3) = fib(2) + fib(1) = 2
- fib(2) = fib(1) + fib(0) = 1
- fib(1) = 1 (base case)
- fib(0) = 0 (base case)
```

**JavaScript Implementation:**

```javascript
// Top-down approach (Memoization)
function fibonacciMemo(n) {
  // Create a memoization map
  const memo = new Map();
  
  function fib(n) {
    // Base cases
    if (n === 0) return 0;
    if (n === 1) return 1;
    
    // Check if already computed
    if (memo.has(n)) {
      return memo.get(n);
    }
    
    // Recursive case with memoization
    const result = fib(n - 1) + fib(n - 2);
    memo.set(n, result);
    return result;
  }
  
  return fib(n);
}

// Bottom-up approach (Tabulation)
function fibonacciTab(n) {
  // Handle base cases
  if (n === 0) return 0;
  if (n === 1) return 1;
  
  // Initialize dp array with base cases
  const dp = [0, 1];
  
  // Fill the array iteratively
  for (let i = 2; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
  }
  
  return dp[n];
}

// Bottom-up with O(1) space
function fibonacciOptimized(n) {
  if (n === 0) return 0;
  if (n === 1) return 1;
  
  let prev = 0;
  let curr = 1;
  
  for (let i = 2; i <= n; i++) {
    const next = prev + curr;
    prev = curr;
    curr = next;
  }
  
  return curr;
}
```

**Pseudocode:**
```
ALGORITHM FibonacciMemoization(n):
    memo = new Map()
    
    FUNCTION Fib(n):
        IF n = 0 THEN RETURN 0
        IF n = 1 THEN RETURN 1
        
        IF n in memo THEN RETURN memo[n]
        
        result = Fib(n - 1) + Fib(n - 2)
        memo[n] = result
        RETURN result
    END FUNCTION
    
    RETURN Fib(n)
END ALGORITHM

ALGORITHM FibonacciTabulation(n):
    IF n = 0 THEN RETURN 0
    IF n = 1 THEN RETURN 1
    
    dp = array of size n+1 initialized with zeros
    dp[0] = 0
    dp[1] = 1
    
    FOR i = 2 TO n:
        dp[i] = dp[i - 1] + dp[i - 2]
    END FOR
    
    RETURN dp[n]
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: 
  - Without DP: O(2^n) - exponential
  - With DP (both memoization and tabulation): O(n) - linear
- Space Complexity: 
  - Memoization: O(n) for the memo table and recursion stack
  - Tabulation: O(n) for the DP array
  - Optimized: O(1) - constant space

**Real-world Applications:**
- Mathematical modeling in nature (e.g., growth patterns in plants)
- Financial market analysis
- Computer algorithms for search trees
- Modeling population growth
- Music composition structures
- Architecture and design

## 2. Knapsack Problem

**Conceptual Explanation:**
The 0/1 Knapsack problem is a classic optimization problem: given a set of items, each with a weight and a value, determine which items to include in a collection so that the total weight is less than or equal to a given limit and the total value is as large as possible.

**Visual Example:**
```
Items: [
  {weight: 1, value: 6},
  {weight: 2, value: 10},
  {weight: 3, value: 12}
]
Maximum Weight Capacity: 5

DP Table (Rows: Items, Columns: Weight Capacity):
   | 0 | 1 | 2 | 3 | 4 | 5 |
---+---+---+---+---+---+---+
 0 | 0 | 0 | 0 | 0 | 0 | 0 |
 1 | 0 | 6 | 6 | 6 | 6 | 6 |
 2 | 0 | 6 | 10| 16| 16| 16|
 3 | 0 | 6 | 10| 16| 18| 22|

Cell [i][w] represents the maximum value we can get with first i items and weight capacity w.
Answer: 22 (take items 1 and 3)
```

**JavaScript Implementation:**

```javascript
// 0/1 Knapsack using tabulation
function knapsack(weights, values, capacity) {
  const n = weights.length;
  
  // Create a 2D DP table
  const dp = Array(n + 1).fill().map(() => Array(capacity + 1).fill(0));
  
  // Fill the dp table
  for (let i = 1; i <= n; i++) {
    for (let w = 0; w <= capacity; w++) {
      // Current item's weight and value (0-indexed)
      const currentWeight = weights[i - 1];
      const currentValue = values[i - 1];
      
      // If current item can't be included in the knapsack
      if (currentWeight > w) {
        dp[i][w] = dp[i - 1][w];
      } else {
        // Max of including or excluding the current item
        dp[i][w] = Math.max(
          currentValue + dp[i - 1][w - currentWeight], // Include item
          dp[i - 1][w] // Exclude item
        );
      }
    }
  }
  
  return dp[n][capacity];
}

// Space-optimized version (uses 1D array)
function knapsackOptimized(weights, values, capacity) {
  const n = weights.length;
  
  // Create a 1D array
  const dp = Array(capacity + 1).fill(0);
  
  for (let i = 0; i < n; i++) {
    // Process from right to left to avoid overwriting needed values
    for (let w = capacity; w >= weights[i]; w--) {
      dp[w] = Math.max(dp[w], values[i] + dp[w - weights[i]]);
    }
  }
  
  return dp[capacity];
}

// Memoization approach
function knapsackMemo(weights, values, capacity) {
  const n = weights.length;
  // Create memoization table (initialize with -1)
  const memo = Array(n).fill().map(() => Array(capacity + 1).fill(-1));
  
  function solve(index, remainingCapacity) {
    // Base case
    if (index === n || remainingCapacity === 0) {
      return 0;
    }
    
    // Check if already computed
    if (memo[index][remainingCapacity] !== -1) {
      return memo[index][remainingCapacity];
    }
    
    // If current item is too heavy
    if (weights[index] > remainingCapacity) {
      return memo[index][remainingCapacity] = solve(index + 1, remainingCapacity);
    }
    
    // Maximum of including and excluding the current item
    const includeItem = values[index] + solve(index + 1, remainingCapacity - weights[index]);
    const excludeItem = solve(index + 1, remainingCapacity);
    
    return memo[index][remainingCapacity] = Math.max(includeItem, excludeItem);
  }
  
  return solve(0, capacity);
}
```

**Pseudocode:**
```
ALGORITHM Knapsack(weights, values, capacity):
    n = length(weights)
    dp = 2D array of size (n+1) × (capacity+1) filled with 0
    
    FOR i = 1 TO n:
        FOR w = 0 TO capacity:
            currentWeight = weights[i-1]
            currentValue = values[i-1]
            
            IF currentWeight > w THEN
                dp[i][w] = dp[i-1][w]  // Can't include this item
            ELSE
                // Max of including or excluding the current item
                dp[i][w] = MAX(
                    currentValue + dp[i-1][w-currentWeight],  // Include item
                    dp[i-1][w]  // Exclude item
                )
            END IF
        END FOR
    END FOR
    
    RETURN dp[n][capacity]
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(n × W) where n is the number of items and W is the capacity
- Space Complexity: 
  - 2D approach: O(n × W)
  - 1D optimized: O(W)

**Real-world Applications:**
- Resource allocation in economics
- Cutting stock problems in manufacturing
- Budget management and financial planning
- Cargo loading optimization
- Portfolio optimization
- Task scheduling
- Data storage optimization

## 3. Longest Common Subsequence (LCS)

**Conceptual Explanation:**
The Longest Common Subsequence problem finds the longest subsequence common to two sequences. A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

**Visual Example:**
```
Strings: s1 = "ABCBDAB", s2 = "BDCABA"
LCS = "BCBA" (length 4)

DP Table (Rows: s1, Columns: s2):
    | ""| B | D | C | A | B | A |
----+---+---+---+---+---+---+---+
 "" | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
 A  | 0 | 0 | 0 | 0 | 1 | 1 | 1 |
 B  | 0 | 1 | 1 | 1 | 1 | 2 | 2 |
 C  | 0 | 1 | 1 | 2 | 2 | 2 | 2 |
 B  | 0 | 1 | 1 | 2 | 2 | 3 | 3 |
 D  | 0 | 1 | 2 | 2 | 2 | 3 | 3 |
 A  | 0 | 1 | 2 | 2 | 3 | 3 | 4 |
 B  | 0 | 1 | 2 | 2 | 3 | 4 | 4 |

Cell [i][j] contains length of LCS of s1[0...i-1] and s2[0...j-1]
```

**JavaScript Implementation:**

```javascript
// LCS using tabulation
function longestCommonSubsequence(s1, s2) {
  const m = s1.length;
  const n = s2.length;
  
  // Create a 2D DP table
  const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));
  
  // Fill the dp table
  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      if (s1[i - 1] === s2[j - 1]) {
        // If characters match, add 1 to the diagonal value
        dp[i][j] = dp[i - 1][j - 1] + 1;
      } else {
        // Take the max from the left or top
        dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
      }
    }
  }
  
  // Length of LCS is in the bottom-right cell
  return dp[m][n];
}

// Function to print the LCS
function printLCS(s1, s2) {
  const m = s1.length;
  const n = s2.length;
  
  // Create a 2D DP table
  const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));
  
  // Fill the dp table
  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      if (s1[i - 1] === s2[j - 1]) {
        dp[i][j] = dp[i - 1][j - 1] + 1;
      } else {
        dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
      }
    }
  }
  
  // Backtrack to find the actual LCS
  let lcs = "";
  let i = m, j = n;
  
  while (i > 0 && j > 0) {
    if (s1[i - 1] === s2[j - 1]) {
      // Characters match, include in LCS
      lcs = s1[i - 1] + lcs;
      i--;
      j--;
    } else if (dp[i - 1][j] > dp[i][j - 1]) {
      // Move to the direction with larger value
      i--;
    } else {
      j--;
    }
  }
  
  return lcs;
}

// Memoization approach
function lcsMemo(s1, s2) {
  const m = s1.length;
  const n = s2.length;
  // Create memoization table (initialize with -1)
  const memo = Array(m + 1).fill().map(() => Array(n + 1).fill(-1));
  
  function solve(i, j) {
    // Base case
    if (i === 0 || j === 0) {
      return 0;
    }
    
    // Check if already computed
    if (memo[i][j] !== -1) {
      return memo[i][j];
    }
    
    // If characters match
    if (s1[i - 1] === s2[j - 1]) {
      return memo[i][j] = 1 + solve(i - 1, j - 1);
    }
    
    // Characters don't match
    return memo[i][j] = Math.max(solve(i - 1, j), solve(i, j - 1));
  }
  
  return solve(m, n);
}
```

**Pseudocode:**
```
ALGORITHM LongestCommonSubsequence(s1, s2):
    m = length(s1)
    n = length(s2)
    dp = 2D array of size (m+1) × (n+1) filled with 0
    
    FOR i = 1 TO m:
        FOR j = 1 TO n:
            IF s1[i-1] = s2[j-1] THEN
                dp[i][j] = dp[i-1][j-1] + 1  // Characters match
            ELSE
                dp[i][j] = MAX(dp[i-1][j], dp[i][j-1])  // Characters don't match
            END IF
        END FOR
    END FOR
    
    RETURN dp[m][n]  // Length of LCS
END ALGORITHM

ALGORITHM PrintLCS(s1, s2):
    // First compute the dp table as in LongestCommonSubsequence
    // Then backtrack to find the actual LCS
    
    lcs = ""
    i = m, j = n
    
    WHILE i > 0 AND j > 0:
        IF s1[i-1] = s2[j-1] THEN
            lcs = s1[i-1] + lcs  // Add character to LCS
            i = i - 1
            j = j - 1
        ELSE IF dp[i-1][j] > dp[i][j-1] THEN
            i = i - 1  // Move up
        ELSE
            j = j - 1  // Move left
        END IF
    END WHILE
    
    RETURN lcs
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(m × n) where m and n are the lengths of the strings
- Space Complexity: O(m × n) for the DP table

**Real-world Applications:**
- DNA sequence alignment in bioinformatics
- File comparison tools (like diff)
- Spell checking and correction
- Plagiarism detection
- Version control systems
- Speech recognition
- Pattern matching in machine learning

## 4. Longest Increasing Subsequence (LIS)

**Conceptual Explanation:**
The Longest Increasing Subsequence problem aims to find the length of the longest subsequence of a given sequence such that all elements of the subsequence are sorted in increasing order.

**Visual Example:**
```
Array: [10, 22, 9, 33, 21, 50, 41, 60, 80]
LIS: [10, 22, 33, 50, 60, 80] (length 6)

DP Table (each cell contains the length of LIS ending at that index):
[1, 2, 1, 3, 2, 4, 4, 5, 6]

For index 3 (value 33):
- Check all previous elements:
  - 10 < 33, so LIS can be extended: 1+1 = 2
  - 22 < 33, so LIS can be extended: 2+1 = 3
  - 9 < 33, so LIS can be extended: 1+1 = 2
- Take maximum: 3
```

**JavaScript Implementation:**

```javascript
// LIS using tabulation - O(n²) approach
function longestIncreasingSubsequence(nums) {
  const n = nums.length;
  if (n === 0) return 0;
  
  // Initialize dp array (LIS ending at index i)
  const dp = new Array(n).fill(1);
  
  // Fill dp array
  for (let i = 1; i < n; i++) {
    for (let j = 0; j < i; j++) {
      if (nums[i] > nums[j]) {
        dp[i] = Math.max(dp[i], dp[j] + 1);
      }
    }
  }
  
  // Return the maximum value in dp array
  return Math.max(...dp);
}

// Improved LIS using binary search - O(n log n) approach
function lisOptimized(nums) {
  const n = nums.length;
  if (n === 0) return 0;
  
  // tails[i] = smallest end value of all LIS of length i+1
  const tails = [nums[0]];
  
  for (let i = 1; i < n; i++) {
    // If nums[i] is greater than the largest tail, append it
    if (nums[i] > tails[tails.length - 1]) {
      tails.push(nums[i]);
    } else {
      // Binary search to find the position to replace
      let left = 0;
      let right = tails.length - 1;
      
      while (left < right) {
        const mid = Math.floor((left + right) / 2);
        if (tails[mid] < nums[i]) {
          left = mid + 1;
        } else {
          right = mid;
        }
      }
      
      tails[left] = nums[i];
    }
  }
  
  return tails.length;
}

// To also reconstruct the actual LIS
function reconstructLIS(nums) {
  const n = nums.length;
  if (n === 0) return [];
  
  // dp[i] = length of LIS ending at index i
  const dp = new Array(n).fill(1);
  // prev[i] = previous index in the LIS ending at index i
  const prev = new Array(n).fill(-1);
  
  for (let i = 1; i < n; i++) {
    for (let j = 0; j < i; j++) {
      if (nums[i] > nums[j] && dp[j] + 1 > dp[i]) {
        dp[i] = dp[j] + 1;
        prev[i] = j;
      }
    }
  }
  
  // Find the index with the maximum LIS length
  let maxLength = dp[0];
  let maxIndex = 0;
  
  for (let i = 1; i < n; i++) {
    if (dp[i] > maxLength) {
      maxLength = dp[i];
      maxIndex = i;
    }
  }
  
  // Reconstruct the LIS
  const lis = [];
  while (maxIndex !== -1) {
    lis.unshift(nums[maxIndex]);
    maxIndex = prev[maxIndex];
  }
  
  return lis;
}
```

**Pseudocode:**
```
ALGORITHM LongestIncreasingSubsequence(nums):
    n = length(nums)
    IF n = 0 THEN RETURN 0
    
    dp = array of size n filled with 1
    
    FOR i = 1 TO n - 1:
        FOR j = 0 TO i - 1:
            IF nums[i] > nums[j] THEN
                dp[i] = MAX(dp[i], dp[j] + 1)
            END IF
        END FOR
    END FOR
    
    RETURN maximum value in dp array
END ALGORITHM

ALGORITHM LISOptimized(nums):
    n = length(nums)
    IF n = 0 THEN RETURN 0
    
    tails = [nums[0]]
    
    FOR i = 1 TO n - 1:
        IF nums[i] > tails[length(tails) - 1] THEN
            APPEND nums[i] to tails
        ELSE
            // Binary search to find position to replace
            left = 0
            right = length(tails) - 1
            
            WHILE left < right:
                mid = floor((left + right) / 2)
                IF tails[mid] < nums[i] THEN
                    left = mid + 1
                ELSE
                    right = mid
                END IF
            END WHILE
            
            tails[left] = nums[i]
        END IF
    END FOR
    
    RETURN length(tails)
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: 
  - Standard approach: O(n²)
  - Optimized approach with binary search: O(n log n)
- Space Complexity: O(n) for both approaches

**Real-world Applications:**
- Box stacking problems in logistics
- Sequence prediction in time series analysis
- Network packet ordering
- Optimal chain of matrix multiplications
- Poker hand analysis
- Route planning with constraints
- Stock price trend analysis

## 5. Matrix Chain Multiplication

**Conceptual Explanation:**
The Matrix Chain Multiplication problem determines the most efficient way to multiply a sequence of matrices. Given a sequence of matrices, the goal is to find the order of multiplications that minimizes the number of operations.

**Visual Example:**
```
Matrices: A₁ (dimensions 30×35), A₂ (35×15), A₃ (15×5), A₄ (5×10), A₅ (10×20)

Different orders of multiplication have different computational costs:
- ((A₁A₂)A₃)A₄A₅ vs A₁(A₂(A₃(A₄A₅)))

DP Table for optimal costs (entries show minimum number of operations):
    | 1 | 2  | 3  | 4   | 5    |
----+---+----+----+-----+------+
 1  | 0 | 15750 | 7875 | 9375 | 11875 |
 2  |   | 0  | 2625 | 4375 | 7125  |
 3  |   |    | 0  | 750  | 2500  |
 4  |   |    |    | 0   | 1000  |
 5  |   |    |    |     | 0    |

dp[i][j] = minimum cost of multiplying matrices i through j
```

**JavaScript Implementation:**

```javascript
// Matrix Chain Multiplication using tabulation
function matrixChainMultiplication(dimensions) {
  const n = dimensions.length - 1; // Number of matrices
  
  // dp[i][j] = Minimum cost of multiplying matrices i through j
  const dp = Array(n).fill().map(() => Array(n).fill(0));
  
  // l is the chain length
  for (let l = 2; l <= n; l++) {
    for (let i = 0; i <= n - l; i++) {
      const j = i + l - 1;
      dp[i][j] = Infinity;
      
      // Try different split positions k
      for (let k = i; k < j; k++) {
        // Cost = Cost of multiplying matrices i to k + 
        //        Cost of multiplying matrices k+1 to j +
        //        Cost of multiplying the two resulting matrices
        const cost = dp[i][k] + dp[k + 1][j] + 
                    dimensions[i] * dimensions[k + 1] * dimensions[j + 1];
        
        if (cost < dp[i][j]) {
          dp[i][j] = cost;
        }
      }
    }
  }
  
  return dp[0][n - 1]; // Minimum cost of multiplying all matrices
}

// With parenthesization (to get the optimal order)
function matrixChainOrder(dimensions) {
  const n = dimensions.length - 1;
  const dp = Array(n).fill().map(() => Array(n).fill(0));
  const split = Array(n).fill().map(() => Array(n).fill(0));
  
  for (let l = 2; l <= n; l++) {
    for (let i = 0; i <= n - l; i++) {
      const j = i + l - 1;
      dp[i][j] = Infinity;
      
      for (let k = i; k < j; k++) {
        const cost = dp[i][k] + dp[k + 1][j] + 
                    dimensions[i] * dimensions[k + 1] * dimensions[j + 1];
        
        if (cost < dp[i][j]) {
          dp[i][j] = cost;
          split[i][j] = k; // Save the optimal split point
        }
      }
    }
  }
  
  // Function to print the optimal parenthesization
  function printOptimalParenthesis(i, j) {
    if (i === j) {
      return `A${i+1}`;
    }
    
    return `(${printOptimalParenthesis(i, split[i][j])} × ${printOptimalParenthesis(split[i][j] + 1, j)})`;
  }
  
  return {
    minOperations: dp[0][n - 1],
    optimalParenthesization: printOptimalParenthesis(0, n - 1)
  };
}