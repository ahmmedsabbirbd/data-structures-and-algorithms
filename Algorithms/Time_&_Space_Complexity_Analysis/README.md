# Time and Space Complexity Analysis

## Basic Concepts

**Conceptual Explanation:**
Algorithmic complexity refers to how the running time (time complexity) or memory usage (space complexity) of an algorithm scales with input size. Big O notation is used to express the upper bound of an algorithm's resource requirements in the worst-case scenario.

**Visual Example:**
```
O(1) - Constant Time
[1, 2, 3, 4, 5] -> Access arr[3] = 4 (single operation regardless of array size)

O(log n) - Logarithmic Time
[1, 2, 3, 4, 5, 6, 7, 8] -> Binary search reduces checks to 3 steps:
Step 1: Check middle (4), too small
Step 2: Check middle of right half (6), too small
Step 3: Check middle of remaining slice (7), found!

O(n) - Linear Time
[1, 2, 3, 4, 5] -> Linear search checks each element once

O(n²) - Quadratic Time 
[1, 2, 3] -> Nested loops checking all pairs: (1,1), (1,2), (1,3), (2,1), (2,2), (2,3), (3,1), (3,2), (3,3)
```

**JavaScript Implementation:**

```javascript
// O(1) - Constant time example
function getFirst(arr) {
  return arr[0]; // Always takes the same amount of time regardless of array size
}

// O(log n) - Logarithmic time example
function binarySearch(arr, target) {
  let left = 0;
  let right = arr.length - 1;
  
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }
  
  return -1; // Target not found
}

// O(n) - Linear time example
function linearSearch(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === target) return i;
  }
  return -1;
}

// O(n²) - Quadratic time example
function bubbleSort(arr) {
  const n = arr.length;
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n - i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]]; // Swap
      }
    }
  }
  return arr;
}

// O(n log n) - Linearithmic time example (merge sort)
function mergeSort(arr) {
  if (arr.length <= 1) return arr;
  
  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));
  
  return merge(left, right);
}

function merge(left, right) {
  let result = [];
  let leftIndex = 0;
  let rightIndex = 0;
  
  while (leftIndex < left.length && rightIndex < right.length) {
    if (left[leftIndex] < right[rightIndex]) {
      result.push(left[leftIndex]);
      leftIndex++;
    } else {
      result.push(right[rightIndex]);
      rightIndex++;
    }
  }
  
  return result.concat(left.slice(leftIndex)).concat(right.slice(rightIndex));
}
```

**Pseudocode:**
```
ALGORITHM TimeComplexity:
    // O(1) - Constant Time
    FUNCTION GetFirst(arr):
        RETURN arr[0]
    END FUNCTION
    
    // O(log n) - Logarithmic Time
    FUNCTION BinarySearch(arr, target):
        left = 0
        right = length(arr) - 1
        
        WHILE left <= right:
            mid = floor((left + right) / 2)
            
            IF arr[mid] = target THEN
                RETURN mid
            ELSE IF arr[mid] < target THEN
                left = mid + 1
            ELSE
                right = mid - 1
            END IF
        END WHILE
        
        RETURN -1
    END FUNCTION
    
    // O(n) - Linear Time
    FUNCTION LinearSearch(arr, target):
        FOR i = 0 to length(arr) - 1:
            IF arr[i] = target THEN
                RETURN i
            END IF
        END FOR
        
        RETURN -1
    END FUNCTION
    
    // O(n²) - Quadratic Time
    FUNCTION BubbleSort(arr):
        n = length(arr)
        
        FOR i = 0 to n - 1:
            FOR j = 0 to n - i - 2:
                IF arr[j] > arr[j + 1] THEN
                    SWAP arr[j] and arr[j + 1]
                END IF
            END FOR
        END FOR
        
        RETURN arr
    END FUNCTION
END ALGORITHM
```

**Time and Space Complexity:**
Different complexity classes:

| Notation      | Name           | Description                            | Example Algorithms                          |
|---------------|----------------|----------------------------------------|--------------------------------------------|
| O(1)          | Constant       | Doesn't change with input size         | Array access, stack push/pop              |
| O(log n)      | Logarithmic    | Divides problem in half at each step   | Binary search, balanced BST operations    |
| O(n)          | Linear         | Scales linearly with input size        | Linear search, array traversal            |
| O(n log n)    | Linearithmic   | n multiplied by log n                  | Merge sort, heap sort, quick sort (avg)   |
| O(n²)         | Quadratic      | Scales with square of input size       | Bubble sort, insertion sort, selection sort |
| O(n³)         | Cubic          | Scales with cube of input size         | Some matrix operations, naive 3D matching |
| O(2ⁿ)         | Exponential    | Doubles with each additional element   | Recursive Fibonacci, power set generation |
| O(n!)         | Factorial      | Grows factorially with input size      | Permutations, brute force traveling salesman |

**Real-world Applications:**
- Algorithm selection based on data size and constraints
- Optimizing critical code paths in software
- Predicting performance characteristics of systems
- Interview preparation for technical roles
- Database query optimization
- Resource allocation in distributed systems

## Common Time Complexities

### 1. Constant Time - O(1)

**Conceptual Explanation:**
Operations that execute in the same amount of time regardless of the input size. The running time is "constant" and doesn't depend on the number of elements.

**Visual Example:**
```
Array: [5, 10, 15, 20, 25]

Accessing arr[2] → 15        // Always one operation
Getting array.length → 5     // Always one operation
```

**JavaScript Example:**
```javascript
function isEven(num) {
  return num % 2 === 0; // Constant time regardless of the number's size
}

function getFirstElement(arr) {
  return arr[0]; // Always takes the same time regardless of array size
}

function pushToStack(stack, value) {
  stack.push(value); // Push operation is O(1)
  return stack;
}
```

**Time and Space Complexity:**
- Time Complexity: O(1) - constant regardless of input size
- Space Complexity: O(1) - uses fixed extra space

**Real-world Applications:**
- Hash table lookups
- Array index access
- Stack operations (push/pop)
- Mathematical operations on fixed-size numbers
- Accessing object properties

### 2. Logarithmic Time - O(log n)

**Conceptual Explanation:**
Algorithms with logarithmic time complexity reduce the problem size by a factor (usually 2) with each step. These algorithms typically don't need to process all elements in the input.

**Visual Example:**
```
Binary search on sorted array: [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
Looking for 23:

1. Check middle: 16 (too small)
2. Check middle of right half: 38 (too large)
3. Check middle of left part of right half: 23 (found!)

Only 3 comparisons for an array of 10 elements
```

**JavaScript Example:**
```javascript
function countBits(num) {
  let count = 0;
  while (num > 0) {
    count += num & 1; // Check last bit
    num >>= 1;        // Shift right (divide by 2)
  }
  return count;
}

// Binary search on sorted array
function binarySearch(arr, target) {
  let left = 0;
  let right = arr.length - 1;
  
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }
  
  return -1;
}
```

**Time and Space Complexity:**
- Time Complexity: O(log n) - time grows logarithmically with input size
- Space Complexity: O(1) for iterative implementation, O(log n) for recursive due to call stack

**Real-world Applications:**
- Binary search
- Binary search trees operations
- Certain divide and conquer algorithms
- Finding number of digits in a number
- Binary representation operations
- Heap operations

### 3. Linear Time - O(n)

**Conceptual Explanation:**
Algorithms with linear time complexity process each input element a constant number of times. The running time grows linearly with the input size.

**Visual Example:**
```
Linear search in array: [7, 2, 9, 4, 1]
Looking for 9:

1. Check 7 (not match)
2. Check 2 (not match)
3. Check 9 (match found!)

Average case would check n/2 elements, worst case checks all n elements
```

**JavaScript Example:**
```javascript
function findMax(arr) {
  let max = arr[0];
  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
      max = arr[i];
    }
  }
  return max;
}

function countOccurrences(arr, target) {
  let count = 0;
  for (const element of arr) {
    if (element === target) {
      count++;
    }
  }
  return count;
}
```

**Time and Space Complexity:**
- Time Complexity: O(n) - time grows linearly with input size
- Space Complexity: O(1) - uses constant extra space regardless of input size

**Real-world Applications:**
- Array traversal
- Linear search
- Finding maximum/minimum value
- Counting occurrences
- String operations
- Simple data processing
- Hash table initialization

### 4. Linearithmic Time - O(n log n)

**Conceptual Explanation:**
Algorithms with linearithmic time complexity often involve dividing the data into smaller chunks (log n factor) and then processing all elements (n factor). These are typically efficient sorting algorithms.

**Visual Example:**
```
Merge sort on array: [38, 27, 43, 3, 9, 82, 10]

1. Divide into smaller arrays recursively until single elements
2. Merge sorted subarrays back together in log n levels:

[38, 27, 43, 3, 9, 82, 10]
[38, 27, 43, 3] and [9, 82, 10]
[38, 27] and [43, 3] and [9, 82] and [10]
[38] and [27] and [43] and [3] and [9] and [82] and [10]
[27, 38] and [3, 43] and [9, 82] and [10]
[3, 27, 38, 43] and [9, 10, 82]
[3, 9, 10, 27, 38, 43, 82]
```

**JavaScript Example:**
```javascript
function mergeSort(arr) {
  if (arr.length <= 1) return arr;
  
  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));
  
  return merge(left, right);
}

function merge(left, right) {
  let result = [];
  let leftIndex = 0;
  let rightIndex = 0;
  
  while (leftIndex < left.length && rightIndex < right.length) {
    if (left[leftIndex] < right[rightIndex]) {
      result.push(left[leftIndex]);
      leftIndex++;
    } else {
      result.push(right[rightIndex]);
      rightIndex++;
    }
  }
  
  return result.concat(left.slice(leftIndex)).concat(right.slice(rightIndex));
}
```

**Time and Space Complexity:**
- Time Complexity: O(n log n) - more efficient than quadratic but slower than linear
- Space Complexity: O(n) for merge sort, O(log n) for quicksort (average case)

**Real-world Applications:**
- Efficient sorting algorithms (merge sort, quicksort, heapsort)
- Certain divide and conquer algorithms
- Best comparison-based sorting
- External sorting for databases
- Various computational geometry algorithms
- Fast Fourier Transform algorithms

### 5. Quadratic Time - O(n²)

**Conceptual Explanation:**
Algorithms with quadratic time complexity have nested loops or iterations where each element in the input needs to be compared or processed with every other element.

**Visual Example:**
```
Bubble sort on array: [5, 3, 8, 4, 2]

Outer pass 1:
  Compare and possibly swap: 5,3 → 3,5
  Compare and possibly swap: 5,8 → 5,8
  Compare and possibly swap: 8,4 → 4,8
  Compare and possibly swap: 8,2 → 2,8
Outer pass 2:
  Compare and possibly swap: 3,5 → 3,5
  Compare and possibly swap: 5,4 → 4,5
  Compare and possibly swap: 5,2 → 2,5
...and so on

Roughly n² comparisons
```

**JavaScript Example:**
```javascript
function bubbleSort(arr) {
  const n = arr.length;
  
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n - i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]]; // Swap
      }
    }
  }
  
  return arr;
}

function allPairs(arr) {
  const pairs = [];
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length; j++) {
      pairs.push([arr[i], arr[j]]);
    }
  }
  return pairs;
}
```

**Time and Space Complexity:**
- Time Complexity: O(n²) - grows quadratically with input size
- Space Complexity: O(1) for bubble sort (in-place), O(n²) for all pairs example

**Real-world Applications:**
- Simple sorting algorithms (bubble, insertion, selection sort)
- Comparing all pairs of elements
- Small datasets where implementation simplicity is more important than efficiency
- Simple graph algorithms with adjacency matrices
- Elementary matrix operations

### 6. Exponential Time - O(2ⁿ)

**Conceptual Explanation:**
Algorithms with exponential time complexity have their runtime double with each additional element in the input. These algorithms quickly become impractical for even moderately sized inputs.

**Visual Example:**
```
Generating all subsets of set {1, 2, 3}:

For each element, we have two choices: include it or not
This gives us 2³ = 8 subsets:
[], [1], [2], [3], [1,2], [1,3], [2,3], [1,2,3]
```

**JavaScript Example:**
```javascript
function generateAllSubsets(arr) {
  const result = [];
  
  function backtrack(index, currentSubset) {
    if (index === arr.length) {
      result.push([...currentSubset]);
      return;
    }
    
    // Don't include current element
    backtrack(index + 1, currentSubset);
    
    // Include current element
    currentSubset.push(arr[index]);
    backtrack(index + 1, currentSubset);
    currentSubset.pop();
  }
  
  backtrack(0, []);
  return result;
}

// Naive recursive Fibonacci
function fibonacciNaive(n) {
  if (n <= 1) return n;
  return fibonacciNaive(n - 1) + fibonacciNaive(n - 2);
}
```

**Time and Space Complexity:**
- Time Complexity: O(2ⁿ) - grows exponentially with input size
- Space Complexity: O(n) for recursive call stack, O(2ⁿ) to store all subsets

**Real-world Applications:**
- Power set generation
- Combinatorial problems
- Some backtracking algorithms
- Naive solutions to NP-complete problems
- Brute force password cracking
- State space exploration in some algorithms

## Space Complexity Analysis

**Conceptual Explanation:**
Space complexity measures the amount of memory an algorithm uses in relation to the input size. It includes both auxiliary space (extra memory used) and input space.

**Visual Example:**
```
O(1) Space:
Iterative sum of array elements - only uses a few variables

O(n) Space:
Creating a copy of an array - space grows linearly with input

O(n²) Space:
Creating an adjacency matrix for a graph with n nodes
```

**JavaScript Examples:**

```javascript
// O(1) Space Complexity
function findMax(arr) {
  let max = arr[0];
  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
      max = arr[i];
    }
  }
  return max;
}

// O(n) Space Complexity
function doubleArray(arr) {
  const result = []; // New array created
  for (let i = 0; i < arr.length; i++) {
    result.push(arr[i] * 2);
  }
  return result;
}

// O(n²) Space Complexity
function createAdjacencyMatrix(edges, n) {
  // Initialize n×n matrix with zeros
  const matrix = Array(n).fill().map(() => Array(n).fill(0));
  
  // Fill the matrix based on edges
  for (const [from, to] of edges) {
    matrix[from][to] = 1;
    matrix[to][from] = 1; // For undirected graph
  }
  
  return matrix;
}
```

**Time and Space Complexity:**
- Space Complexity Range: O(1), O(log n), O(n), O(n log n), O(n²), O(2ⁿ), etc.
- Auxiliary Space: Extra space used by the algorithm (excluding input)
- Total Space: Auxiliary space + space used by input

**Real-world Applications:**
- Optimizing memory usage in constrained environments
- Algorithms for embedded systems or mobile devices
- Cache-friendly algorithm design
- Database query optimization
- Big data processing frameworks
- In-place algorithms for limited memory situations

## Amortized Analysis

**Conceptual Explanation:**
Amortized analysis considers the average performance of operations over a sequence of operations, even if some individual operations might be expensive.

**Visual Example:**
```
Dynamic Array Resizing:
- Adding elements is usually O(1)
- But occasional resize operations cost O(n)
- Amortized time for n insertions: O(n) overall, or O(1) per insertion
```

**JavaScript Example:**
```javascript
class DynamicArray {
  constructor() {
    this.data = new Array(1); // Start with size 1
    this.length = 0;
    this.capacity = 1;
  }
  
  push(element) {
    // If we've reached capacity, resize
    if (this.length === this.capacity) {
      this.resize();
    }
    
    // Add the element
    this.data[this.length] = element;
    this.length++;
  }
  
  resize() {
    // Double the capacity
    this.capacity *= 2;
    
    // Create new array with double capacity
    const newData = new Array(this.capacity);
    
    // Copy all elements (this is the O(n) operation)
    for (let i = 0; i < this.length; i++) {
      newData[i] = this.data[i];
    }
    
    // Replace old array with new one
    this.data = newData;
  }
  
  get(index) {
    if (index < 0 || index >= this.length) {
      throw new Error("Index out of bounds");
    }
    return this.data[index];
  }
}
```

**Time and Space Complexity:**
- Amortized Time for push: O(1) - even though some operations take O(n)
- Total Time for n pushes: O(n) - not O(n²)
- Space Complexity: O(n) - proportional to number of elements

**Real-world Applications:**
- Dynamic arrays (like JavaScript arrays, ArrayList in Java)
- Hash tables with resizing
- Self-balancing binary search trees
- Splay trees
- Fibonacci heaps
- Advanced data structures with occasional costly operations

## Analyzing Recursive Algorithms

**Conceptual Explanation:**
Recursive algorithms call themselves with smaller inputs. Their time complexity can be analyzed using recurrence relations, and their space complexity must account for the recursion stack.

**Visual Example:**
```
Recursive Factorial:
factorial(4) calls factorial(3)
factorial(3) calls factorial(2)
factorial(2) calls factorial(1)
factorial(1) returns 1
factorial(2) returns 2
factorial(3) returns 6
factorial(4) returns 24

Call stack depth: 4 (O(n))
```

**JavaScript Example:**
```javascript
// Recursive binary search
function binarySearchRecursive(arr, target, left = 0, right = arr.length - 1) {
  // Base case: element not found
  if (left > right) return -1;
  
  const mid = Math.floor((left + right) / 2);
  
  // Found target
  if (arr[mid] === target) return mid;
  
  // Recursive cases
  if (arr[mid] > target) {
    // Search left half
    return binarySearchRecursive(arr, target, left, mid - 1);
  } else {
    // Search right half
    return binarySearchRecursive(arr, target, mid + 1, right);
  }
}

// Merge sort recurrence relation: T(n) = 2T(n/2) + O(n)
function mergeSort(arr) {
  if (arr.length <= 1) return arr;
  
  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));
  
  return merge(left, right);
}

function merge(left, right) {
  // Merge implementation (O(n) time)
  // ...
}
```

**Recurrence Relations:**
```
Binary Search: T(n) = T(n/2) + O(1) => O(log n)
Merge Sort: T(n) = 2T(n/2) + O(n) => O(n log n)
Quicksort (average): T(n) = 2T(n/2) + O(n) => O(n log n)
Quicksort (worst): T(n) = T(n-1) + O(n) => O(n²)
```

**Time and Space Complexity:**
- Time Complexity: Depends on the recurrence relation
- Space Complexity: Must include recursion stack depth

**Real-world Applications:**
- Master theorem application for divide-and-conquer algorithms
- Performance analysis of recursive algorithms
- Identifying potential stack overflow situations
- Tail call optimization opportunities
- Converting between recursive and iterative implementations

## Practical Considerations for Algorithm Selection

**Conceptual Explanation:**
When choosing algorithms, consider both asymptotic complexity and practical factors like constant factors, memory access patterns, and problem size.

**Rules of Thumb for Algorithm Selection Based on Input Size:**

| Input Size (n) | Acceptable Time Complexity | Examples |
|----------------|----------------------------|----------|
| n ≤ 10         | O(n!), O(n⁶)              | Permutations, brute force solutions |
| n ≤ 20         | O(2ⁿ)                     | Combinatorial problems, subset generation |
| n ≤ 500        | O(n³)                     | Cubic time algorithms, small matrix operations |
| n ≤ 5,000      | O(n²)                     | Simple sorting, all-pairs problems |
| n ≤ 10⁶        | O(n log n)                | Efficient sorting, divide and conquer |
| n ≤ 10⁸        | O(n)                      | Linear algorithms, single-pass solutions |
| n > 10⁸        | O(log n), O(1)            | Binary search, constant-time lookups |

**JavaScript Example - Algorithm Selection:**
```javascript
function selectSortingAlgorithm(arr) {
  if (arr.length <= 10) {
    // For very small arrays, use insertion sort
    return insertionSort(arr);
  } else if (arr.length <= 1000) {
    // For small-medium arrays, use quicksort
    return quickSort(arr);
  } else {
    // For large arrays, use mergeSort (stable, predictable performance)
    return mergeSort(arr);
  }
}

// Check if array is already mostly sorted
function isNearlySorted(arr) {
  let outOfPlaceCount = 0;
  for (let i = 0; i < arr.length - 1; i++) {
    if (arr[i] > arr[i + 1]) {
      outOfPlaceCount++;
      if (outOfPlaceCount > arr.length * 0.1) { // >10% out of place
        return false;
      }
    }
  }
  return true;
}
```

**Real-world Applications:**
- System design decisions based on expected data sizes
- Competitive programming strategy
- Database query optimization
- API design for scalability
- Embedded systems with resource constraints
- High-frequency trading systems
- Real-time applications

## Advanced Topics in Complexity Analysis

### 1. NP-Completeness and Computational Complexity Classes

**Conceptual Explanation:**
Complexity theory classifies problems based on their inherent difficulty. NP-complete problems are believed to have no efficient (polynomial-time) solutions.

**Common Complexity Classes:**
- P: Problems solvable in polynomial time
- NP: Problems verifiable in polynomial time
- NP-Complete: Hardest problems in NP
- NP-Hard: At least as hard as the hardest problems in NP

**Examples of NP-Complete Problems:**
- Traveling Salesman Problem
- Knapsack Problem
- Boolean Satisfiability Problem (SAT)
- Graph Coloring
- Subset Sum

### 2. Average Case vs. Worst Case Analysis

**Conceptual Explanation:**
Algorithms can have different performance characteristics depending on the input. Analysis often focuses on:
- Worst-case: Upper bound on running time (Big O)
- Average-case: Expected performance on random input
- Best-case: Lower bound on running time (Omega notation)

**Example - Quicksort:**
- Worst-case: O(n²) when the pivot is always the smallest/largest element
- Average-case: O(n log n) for random inputs
- Best-case: O(n log n) when the pivot always divides the array evenly

### 3. Omega and Theta Notation

**Omega Notation (Ω):**
- Lower bound on growth rate
- f(n) = Ω(g(n)) means f(n) grows at least as fast as g(n)

**Theta Notation (Θ):**
- Tight bound on growth rate
- f(n) = Θ(g(n)) means f(n) grows exactly as fast as g(n)
- Equivalent to both O(g(n)) and Ω(g(n))

### 4. Space-Time Tradeoffs

**Conceptual Explanation:**
Many algorithms allow trading memory usage for speed or vice versa.

**Example - Memoization:**
```javascript
// Without memoization - O(2^n) time, O(n) space
function fibNaive(n) {
  if (n <= 1) return n;
  return fibNaive(n - 1) + fibNaive(n - 2);
}

// With memoization - O(n) time, O(n) space
function fibMemo(n, memo = {}) {
  if (n in memo) return memo[n];
  if (n <= 1) return n;
  
  memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
  return memo[n];
}
```
