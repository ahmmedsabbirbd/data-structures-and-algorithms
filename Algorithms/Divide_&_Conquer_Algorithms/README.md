# Divide and Conquer Algorithms

## Basic Concept

**Conceptual Explanation:**
Divide and conquer is an algorithmic paradigm that breaks a problem into smaller subproblems, solves each subproblem independently, and then combines these solutions to create a solution to the original problem. This approach has three main steps:

1. **Divide**: Break the problem into smaller subproblems
2. **Conquer**: Solve each subproblem recursively
3. **Combine**: Merge the solutions of subproblems into a solution for the original problem

**Visual Example:**
```
Problem: Sort the array [38, 27, 43, 3, 9, 82, 10]

Divide:
[38, 27, 43, 3, 9, 82, 10]
↙                      ↘
[38, 27, 43, 3]    [9, 82, 10]
↙         ↘        ↙      ↘
[38, 27]  [43, 3]  [9]  [82, 10]
↙    ↘     ↙   ↘       ↙     ↘
[38] [27] [43] [3]    [82]   [10]

Conquer: Each single-element array is already sorted

Combine (merge):
[38] [27] → [27, 38]     [43] [3] → [3, 43]     [82] [10] → [10, 82]
       ↘       ↙                ↘       ↙
      [3, 27, 38, 43]           [9, 10, 82]
               ↘                  ↙
             [3, 9, 10, 27, 38, 43, 82]
```

**JavaScript Implementation (General Template):**

```javascript
function divideAndConquer(problem) {
  // Base case: If problem is small enough, solve directly
  if (isSmallEnough(problem)) {
    return solveDirect(problem);
  }
  
  // Divide: Break down the problem
  const subproblems = divide(problem);
  
  // Conquer: Solve each subproblem recursively
  const subSolutions = subproblems.map(subproblem => divideAndConquer(subproblem));
  
  // Combine: Merge the solutions
  return combine(subSolutions);
}
```

**Pseudocode:**
```
ALGORITHM DivideAndConquer(problem):
    IF IsSmallEnough(problem) THEN
        RETURN SolveDirect(problem)
    END IF
    
    subproblems = Divide(problem)
    subSolutions = []
    
    FOR EACH subproblem IN subproblems:
        subSolution = DivideAndConquer(subproblem)
        APPEND subSolution TO subSolutions
    END FOR
    
    RETURN Combine(subSolutions)
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: Typically analyzed using the Master Theorem
- Common time complexities: O(n log n) for many efficient divide and conquer algorithms
- Space Complexity: Often O(log n) due to recursion stack, though can be higher based on the specific algorithm

**Real-world Applications:**
- Efficient sorting algorithms (merge sort, quicksort)
- Fast multiplication of large integers or matrices
- Closest pair of points problem
- Convex hull algorithms
- Fast Fourier Transform (FFT)
- Search algorithms (binary search)
- Parallel programming and distributed computing

## 1. Binary Search

**Conceptual Explanation:**
Binary search is a classic divide and conquer algorithm that finds the position of a target value within a sorted array. It works by repeatedly dividing the search interval in half.

**Visual Example:**
```
Array: [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
Target: 23

Divide & Conquer Steps:
1. Middle element is 16 (index 4)
   23 > 16, so search the right half: [23, 38, 56, 72, 91]

2. Middle element is 56 (index 7)
   23 < 56, so search the left half: [23, 38]

3. Middle element is 23 (index 5)
   23 == 23, target found at index 5!
```

**JavaScript Implementation:**

```javascript
function binarySearch(arr, target, left = 0, right = arr.length - 1) {
  // Base case: element not found
  if (left > right) {
    return -1;
  }
  
  // Find the middle element
  const mid = Math.floor((left + right) / 2);
  
  // Found the target
  if (arr[mid] === target) {
    return mid;
  }
  
  // Divide & Conquer
  if (arr[mid] > target) {
    // Search in the left half
    return binarySearch(arr, target, left, mid - 1);
  } else {
    // Search in the right half
    return binarySearch(arr, target, mid + 1, right);
  }
}

// Iterative implementation
function binarySearchIterative(arr, target) {
  let left = 0;
  let right = arr.length - 1;
  
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    
    if (arr[mid] === target) {
      return mid; // Target found
    }
    
    if (arr[mid] > target) {
      right = mid - 1; // Search left half
    } else {
      left = mid + 1; // Search right half
    }
  }
  
  return -1; // Target not found
}
```

**Pseudocode:**
```
ALGORITHM BinarySearch(arr, target, left, right):
    IF left > right THEN
        RETURN -1 // Element not found
    END IF
    
    mid = floor((left + right) / 2)
    
    IF arr[mid] = target THEN
        RETURN mid // Target found
    ELSE IF arr[mid] > target THEN
        RETURN BinarySearch(arr, target, left, mid - 1) // Search left half
    ELSE
        RETURN BinarySearch(arr, target, mid + 1, right) // Search right half
    END IF
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(log n) - each step eliminates half of the remaining elements
- Space Complexity: O(log n) for recursive implementation due to call stack, O(1) for iterative version

**Real-world Applications:**
- Dictionary and encyclopedia lookups
- Database indexing and searching
- Finding files in directories
- Root-finding algorithms
- Game AI for guessing numbers
- Network routing algorithms
- Binary search trees

## 2. Merge Sort

**Conceptual Explanation:**
Merge sort is a stable, comparison-based sorting algorithm that follows the divide and conquer approach. It divides the input array into two halves, recursively sorts them, and then merges the sorted halves.

**Visual Example:**
```
Sort the array: [38, 27, 43, 3, 9, 82, 10]

Divide:
                  [38, 27, 43, 3, 9, 82, 10]
                /                          \
       [38, 27, 43, 3]                [9, 82, 10]
       /           \                  /        \
  [38, 27]      [43, 3]            [9]     [82, 10]
  /     \       /    \                     /     \
[38]    [27]  [43]   [3]                [82]    [10]

Conquer & Combine (Merge):
[38]    [27]  [43]   [3]                [82]    [10]
  \     /       \    /                     \     /
  [27, 38]      [3, 43]                   [10, 82]
       \           /                        /
       [3, 27, 38, 43]                [9, 10, 82]
                \                      /
                  [3, 9, 10, 27, 38, 43, 82]
```

**JavaScript Implementation:**

```javascript
function mergeSort(arr) {
  // Base case: array with 0 or 1 element is already sorted
  if (arr.length <= 1) {
    return arr;
  }
  
  // Divide: Find the middle point and split the array
  const middle = Math.floor(arr.length / 2);
  const left = arr.slice(0, middle);
  const right = arr.slice(middle);
  
  // Conquer: Recursively sort both halves
  const sortedLeft = mergeSort(left);
  const sortedRight = mergeSort(right);
  
  // Combine: Merge the sorted halves
  return merge(sortedLeft, sortedRight);
}

function merge(left, right) {
  let result = [];
  let leftIndex = 0;
  let rightIndex = 0;
  
  // Compare elements from both arrays and add the smaller one to the result
  while (leftIndex < left.length && rightIndex < right.length) {
    if (left[leftIndex] < right[rightIndex]) {
      result.push(left[leftIndex]);
      leftIndex++;
    } else {
      result.push(right[rightIndex]);
      rightIndex++;
    }
  }
  
  // Add remaining elements from left array (if any)
  while (leftIndex < left.length) {
    result.push(left[leftIndex]);
    leftIndex++;
  }
  
  // Add remaining elements from right array (if any)
  while (rightIndex < right.length) {
    result.push(right[rightIndex]);
    rightIndex++;
  }
  
  return result;
}
```

**Pseudocode:**
```
ALGORITHM MergeSort(arr):
    IF length(arr) <= 1 THEN
        RETURN arr
    END IF
    
    middle = floor(length(arr) / 2)
    left = arr[0...middle-1]
    right = arr[middle...length(arr)-1]
    
    left = MergeSort(left)
    right = MergeSort(right)
    
    RETURN Merge(left, right)
END ALGORITHM

ALGORITHM Merge(left, right):
    result = []
    leftIndex = 0
    rightIndex = 0
    
    WHILE leftIndex < length(left) AND rightIndex < length(right):
        IF left[leftIndex] < right[rightIndex] THEN
            APPEND left[leftIndex] to result
            leftIndex = leftIndex + 1
        ELSE
            APPEND right[rightIndex] to result
            rightIndex = rightIndex + 1
        END IF
    END WHILE
    
    APPEND remaining elements from left to result
    APPEND remaining elements from right to result
    
    RETURN result
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(n log n) in all cases
- Space Complexity: O(n) - requires additional space for the merged arrays

**Real-world Applications:**
- Sorting large datasets
- External sorting (when data doesn't fit in memory)
- Used in many programming language standard libraries
- Database systems for efficient sorting
- Parallel algorithms (merge sort can be easily parallelized)
- Inversion count problem
- E-commerce sorting of products by various criteria

## 3. Quick Sort

**Conceptual Explanation:**
Quick sort is another divide and conquer sorting algorithm that works by selecting a 'pivot' element and partitioning the array around it. Elements smaller than the pivot go to the left, larger elements go to the right, and the pivot is placed in its final sorted position.

**Visual Example:**
```
Sort the array: [38, 27, 43, 3, 9, 82, 10]

Divide (using last element as pivot):
Pivot = 10
Partition: [3, 9] [10] [38, 27, 43, 82]
                    ↑ (pivot in final position)

Recursively sort left part:
Pivot = 9
Partition: [3] [9] []

Recursively sort right part:
Pivot = 82
Partition: [38, 27, 43] [82] []

Continue with [38, 27, 43]:
Pivot = 43
Partition: [38, 27] [43] []

Continue with [38, 27]:
Pivot = 27
Partition: [] [27] [38]

Combine (implicit in quick sort):
The array is sorted in place: [3, 9, 10, 27, 38, 43, 82]
```

**JavaScript Implementation:**

```javascript
function quickSort(arr, left = 0, right = arr.length - 1) {
  if (left < right) {
    // Divide: Partition the array and get the pivot index
    const pivotIndex = partition(arr, left, right);
    
    // Conquer: Recursively sort elements before and after pivot
    quickSort(arr, left, pivotIndex - 1);
    quickSort(arr, pivotIndex + 1, right);
  }
  
  return arr;
}

function partition(arr, left, right) {
  // Choose rightmost element as pivot
  const pivot = arr[right];
  
  // Index of smaller element
  let i = left - 1;
  
  for (let j = left; j < right; j++) {
    // If current element is smaller than or equal to pivot
    if (arr[j] <= pivot) {
      i++;
      // Swap elements
      [arr[i], arr[j]] = [arr[j], arr[i]];
    }
  }
  
  // Swap pivot with element at i+1
  [arr[i + 1], arr[right]] = [arr[right], arr[i + 1]];
  
  return i + 1; // Return the partitioning index
}

// Example of randomized pivot selection for better performance
function randomizedQuickSort(arr, left = 0, right = arr.length - 1) {
  if (left < right) {
    // Choose a random pivot
    const randomPivotIndex = left + Math.floor(Math.random() * (right - left + 1));
    
    // Swap it with the rightmost element
    [arr[randomPivotIndex], arr[right]] = [arr[right], arr[randomPivotIndex]];
    
    // Partition and sort
    const pivotIndex = partition(arr, left, right);
    randomizedQuickSort(arr, left, pivotIndex - 1);
    randomizedQuickSort(arr, pivotIndex + 1, right);
  }
  
  return arr;
}
```

**Pseudocode:**
```
ALGORITHM QuickSort(arr, left, right):
    IF left < right THEN
        pivotIndex = Partition(arr, left, right)
        QuickSort(arr, left, pivotIndex - 1)
        QuickSort(arr, pivotIndex + 1, right)
    END IF
    
    RETURN arr
END ALGORITHM

ALGORITHM Partition(arr, left, right):
    pivot = arr[right]
    i = left - 1
    
    FOR j = left TO right - 1:
        IF arr[j] <= pivot THEN
            i = i + 1
            SWAP arr[i] AND arr[j]
        END IF
    END FOR
    
    SWAP arr[i + 1] AND arr[right]
    
    RETURN i + 1
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: 
  - Best and Average case: O(n log n)
  - Worst case: O(n²) (occurs with already sorted arrays and poor pivot selection)
- Space Complexity: O(log n) on average for the recursive call stack

**Real-world Applications:**
- Default sorting algorithm in many programming language libraries
- In-memory sorting
- Virtual memory management in operating systems
- Numerical computations
- 3D rendering engines (sorting polygons by depth)
- Database query optimization
- Load balancing in distributed systems

## 4. Finding Maximum Subarray Sum

**Conceptual Explanation:**
The maximum subarray problem aims to find the contiguous subarray within a one-dimensional array of numbers that has the largest sum. This can be solved efficiently using the divide and conquer approach.

**Visual Example:**
```
Array: [-2, 1, -3, 4, -1, 2, 1, -5, 4]

Divide:
           [-2, 1, -3, 4, -1, 2, 1, -5, 4]
          /                             \
[-2, 1, -3, 4]                  [-1, 2, 1, -5, 4]
   /       \                       /         \
[-2, 1]  [-3, 4]              [-1, 2]    [1, -5, 4]
 /  \     /  \                 /  \       /     \
[-2] [1] [-3] [4]            [-1] [2]   [1]  [-5, 4]
                                               /  \
                                            [-5]  [4]

Conquer & Combine:
For each subproblem, we compute:
1. Maximum subarray sum entirely in the left half
2. Maximum subarray sum entirely in the right half
3. Maximum subarray sum that crosses the middle
4. Take the maximum of these three values

For example, at the top level:
- Left half max: 4 (the subarray [4])
- Right half max: 6 (the subarray [4, -1, 2, 1])
- Crossing max: 3 (the subarray [4, -1, 2, 1, -5])
- Overall max: 6 (the subarray [4, -1, 2, 1])
```

**JavaScript Implementation:**

```javascript
function findMaxSubarraySum(arr, low = 0, high = arr.length - 1) {
  // Base case: only one element
  if (low === high) {
    return arr[low];
  }
  
  // Divide the array into two halves
  const mid = Math.floor((low + high) / 2);
  
  // Find maximum subarray sum in left half, right half, and crossing the middle
  const leftMax = findMaxSubarraySum(arr, low, mid);
  const rightMax = findMaxSubarraySum(arr, mid + 1, high);
  const crossMax = findMaxCrossingSubarraySum(arr, low, mid, high);
  
  // Return the maximum of the three
  return Math.max(leftMax, rightMax, crossMax);
}

function findMaxCrossingSubarraySum(arr, low, mid, high) {
  // Find maximum sum starting from mid and going left
  let leftSum = -Infinity;
  let sum = 0;
  
  for (let i = mid; i >= low; i--) {
    sum += arr[i];
    if (sum > leftSum) {
      leftSum = sum;
    }
  }
  
  // Find maximum sum starting from mid+1 and going right
  let rightSum = -Infinity;
  sum = 0;
  
  for (let i = mid + 1; i <= high; i++) {
    sum += arr[i];
    if (sum > rightSum) {
      rightSum = sum;
    }
  }
  
  // Return the sum of the two halves
  return leftSum + rightSum;
}

// Alternative: Kadane's algorithm (more efficient, but not divide and conquer)
function kadaneMaxSubarraySum(arr) {
  let maxSoFar = arr[0];
  let maxEndingHere = arr[0];
  
  for (let i = 1; i < arr.length; i++) {
    maxEndingHere = Math.max(arr[i], maxEndingHere + arr[i]);
    maxSoFar = Math.max(maxSoFar, maxEndingHere);
  }
  
  return maxSoFar;
}
```

**Pseudocode:**
```
ALGORITHM FindMaxSubarraySum(arr, low, high):
    IF low = high THEN
        RETURN arr[low]
    END IF
    
    mid = floor((low + high) / 2)
    
    leftMax = FindMaxSubarraySum(arr, low, mid)
    rightMax = FindMaxSubarraySum(arr, mid + 1, high)
    crossMax = FindMaxCrossingSubarraySum(arr, low, mid, high)
    
    RETURN max(leftMax, rightMax, crossMax)
END ALGORITHM

ALGORITHM FindMaxCrossingSubarraySum(arr, low, mid, high):
    leftSum = -INFINITY
    sum = 0
    
    FOR i = mid DOWNTO low:
        sum = sum + arr[i]
        IF sum > leftSum THEN
            leftSum = sum
        END IF
    END FOR
    
    rightSum = -INFINITY
    sum = 0
    
    FOR i = mid + 1 TO high:
        sum = sum + arr[i]
        IF sum > rightSum THEN
            rightSum = sum
        END IF
    END FOR
    
    RETURN leftSum + rightSum
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(n log n) for the divide and conquer approach
- Space Complexity: O(log n) due to recursion stack
- Note: Kadane's algorithm solves this in O(n) time and O(1) space, but doesn't use divide and conquer

**Real-world Applications:**
- Stock market analysis (maximum profit in a sequence of stock prices)
- Image processing (finding brightest regions)
- Data analysis (finding periods of highest activity)
- Signal processing (finding strongest signal components)
- Gene sequence analysis
- Pattern recognition
- Optimization problems in various domains

## 5. Strassen's Matrix Multiplication

**Conceptual Explanation:**
Strassen's algorithm is a divide and conquer approach for multiplying two matrices. It reduces the number of recursive multiplications from 8 (in the naive recursive approach) to 7, resulting in a more efficient algorithm for large matrices.

**Visual Example:**
```
Multiplying two 2×2 matrices:
A = | a b |    B = | e f |
    | c d |        | g h |

Traditional approach requires 8 multiplications:
C[0][0] = a*e + b*g
C[0][1] = a*f + b*h
C[1][0] = c*e + d*g
C[1][1] = c*f + d*h

Strassen's approach uses 7 multiplications:
M1 = (a + d) * (e + h)
M2 = (c + d) * e
M3 = a * (f - h)
M4 = d * (g - e)
M5 = (a + b) * h
M6 = (c - a) * (e + f)
M7 = (b - d) * (g + h)

Then:
C[0][0] = M1 + M4 - M5 + M7
C[0][1] = M3 + M5
C[1][0] = M2 + M4
C[1][1] = M1 - M2 + M3 + M6
```

**JavaScript Implementation:**

```javascript
function strassenMultiply(A, B) {
  const n = A.length;
  
  // Base case: 1x1 matrices
  if (n === 1) {
    return [[A[0][0] * B[0][0]]];
  }
  
  // Divide matrices into four n/2 x n/2 submatrices
  const mid = n / 2;
  
  const A11 = createSubmatrix(A, 0, 0, mid);
  const A12 = createSubmatrix(A, 0, mid, mid);
  const A21 = createSubmatrix(A, mid, 0, mid);
  const A22 = createSubmatrix(A, mid, mid, mid);
  
  const B11 = createSubmatrix(B, 0, 0, mid);
  const B12 = createSubmatrix(B, 0, mid, mid);
  const B21 = createSubmatrix(B, mid, 0, mid);
  const B22 = createSubmatrix(B, mid, mid, mid);
  
  // Calculate the 7 products
  const M1 = strassenMultiply(addMatrices(A11, A22), addMatrices(B11, B22));
  const M2 = strassenMultiply(addMatrices(A21, A22), B11);
  const M3 = strassenMultiply(A11, subtractMatrices(B12, B22));
  const M4 = strassenMultiply(A22, subtractMatrices(B21, B11));
  const M5 = strassenMultiply(addMatrices(A11, A12), B22);
  const M6 = strassenMultiply(subtractMatrices(A21, A11), addMatrices(B11, B12));
  const M7 = strassenMultiply(subtractMatrices(A12, A22), addMatrices(B21, B22));
  
  // Combine results
  const C11 = addMatrices(subtractMatrices(addMatrices(M1, M4), M5), M7);
  const C12 = addMatrices(M3, M5);
  const C21 = addMatrices(M2, M4);
  const C22 = addMatrices(subtractMatrices(addMatrices(M1, M3), M2), M6);
  
  // Combine the four submatrices into the result
  return combineMatrices(C11, C12, C21, C22);
}

// Helper functions for matrix operations
function createSubmatrix(matrix, startRow, startCol, size) {
  const submatrix = Array(size).fill().map(() => Array(size).fill(0));
  
  for (let i = 0; i < size; i++) {
    for (let j = 0; j < size; j++) {
      submatrix[i][j] = matrix[i + startRow][j + startCol];
    }
  }
  
  return submatrix;
}

function addMatrices(A, B) {
  const n = A.length;
  const result = Array(n).fill().map(() => Array(n).fill(0));
  
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      result[i][j] = A[i][j] + B[i][j];
    }
  }
  
  return result;
}

function subtractMatrices(A, B) {
  const n = A.length;
  const result = Array(n).fill().map(() => Array(n).fill(0));
  
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      result[i][j] = A[i][j] - B[i][j];
    }
  }
  
  return result;
}

function combineMatrices(C11, C12, C21, C22) {
  const n = C11.length;
  const size = n * 2;
  const result = Array(size).fill().map(() => Array(size).fill(0));
  
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      result[i][j] = C11[i][j];
      result[i][j + n] = C12[i][j];
      result[i + n][j] = C21[i][j];
      result[i + n][j + n] = C22[i][j];
    }
  }
  
  return result;
}
```

**Pseudocode:**
```
ALGORITHM StrassenMultiply(A, B):
    n = length(A)
    
    IF n = 1 THEN
        RETURN [[A[0][0] * B[0][0]]]
    END IF
    
    mid = n / 2
    
    A11 = submatrix of A from (0,0) to (mid-1,mid-1)
    A12 = submatrix of A from (0,mid) to (mid-1,n-1)
    A21 = submatrix of A from (mid,0) to (n-1,mid-1)
    A22 = submatrix of A from (mid,mid) to (n-1,n-1)
    
    B11 = submatrix of B from (0,0) to (mid-1,mid-1)
    B12 = submatrix of B from (0,mid) to (mid-1,n-1)
    B21 = submatrix of B from (mid,0) to (n-1,mid-1)
    B22 = submatrix of B from (mid,mid) to (n-1,n-1)
    
    M1 = StrassenMultiply(A11 + A22, B11 + B22)
    M2 = StrassenMultiply(A21 + A22, B11)
    M3 = StrassenMultiply(A11, B12 - B22)
    M4 = StrassenMultiply(A22, B21 - B11)
    M5 = StrassenMultiply(A11 + A12, B22)
    M6 = StrassenMultiply(A21 - A11, B11 + B12)
    M7 = StrassenMultiply(A12 - A22, B21 + B22)
    
    C11 = M1 + M4 - M5 + M7
    C12 = M3 + M5
    C21 = M2 + M4
    C22 = M1 + M3 - M2 + M6
    
    RETURN combined matrix of C11, C12, C21, C22
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(n^log₂7) ≈ O(n^2.81), better than the naive O(n^3)
- Space Complexity: O(n^2) for storing the matrices

**Real-world Applications:**
- Large matrix multiplications in scientific computing
- Computer graphics and 3D transformations
- Machine learning (neural networks)
- Linear algebra systems
- Signal processing
- Physics simulations
- Cryptography algorithms

## 6. Closest Pair of Points

**Conceptual Explanation:**
The closest pair of points problem aims to find the two points with the smallest distance between them in a set of points. A divide and conquer approach can solve this in O(n log n) time.

**Visual Example:**
```
Points: [(1,2), (3,4), (5,6), (2,3), (8,9), (4,5)]

Divide:
1. Sort points by x-coordinate
2. Divide into two halves
   Left half: [(1,2), (2,3), (3,4)]
   Right half: [(4,5), (5,6), (8,9)]
3. Find closest pair in each half recursively
4. Find closest pair that spans the dividing line
5. Return the closest of all three pairs
```

**JavaScript Implementation:**

```javascript
function closestPair(points) {
  // Sort points by x-coordinate
  points.sort((a, b) => a.x - b.x);
  
  // Find closest pair recursively
  return closestPairRec(points, 0, points.length - 1);
}

function closestPairRec(points, start, end) {
  // Base case: if there are 2 or 3 points, use brute force
  if (end - start <= 2) {
    return bruteForceClosestPair(points, start, end);
  }
  
  // Divide the points into two halves
  const mid = Math.floor((start + end) / 2);
  const midPoint = points[mid];
  
  // Recursively find closest pairs in left and right halves
  const leftClosest = closestPairRec(points, start, mid);
  const rightClosest = closestPairRec(points, mid + 1, end);
  
  // Find the smaller of the two distances
  let minDist = Math.min(leftClosest.distance, rightClosest.distance);
  let closestPair = leftClosest.distance < rightClosest.distance ? leftClosest : rightClosest;
  
  // Find points that could be closer than minDist across the dividing line
  const strip = [];
  for (let i = start; i <= end; i++) {
    if (Math.abs(points[i].x - midPoint.x) < minDist) {
      strip.push(points[i]);
    }
  }
  
  // Sort the strip by y-coordinate
  strip.sort((a, b) => a.y - b.y);
  
  // Find the closest pair in the strip
  for (let i = 0; i < strip.length; i++) {
    for (let j = i + 1; j < strip.length && (strip[j].y - strip[i].y) < minDist; j++) {
      const dist = distance(strip[i], strip[j]);
      if (dist < minDist) {
        minDist = dist;
        closestPair = { point1: strip[i], point2: strip[j], distance: dist };
      }
    }
  }
  
  return closestPair;
}

function bruteForceClosestPair(points, start, end) {
  let minDist = Infinity;
  let pair = { point1: null, point2: null, distance: minDist };
  
  for (let i = start; i < end; i++) {
    for (let j = i + 1; j <= end; j++) {
      const dist = distance(points[i], points[j]);
      if (dist < minDist) {
        minDist = dist;
        pair = { point1: points[i], point2: points[j], distance: dist };
      }
    }
  }
  
  return pair;
}

function distance(p1, p2) {
  return Math.sqrt(Math.pow(p1.x - p2.x, 2) + Math.pow(p1.y - p2.y, 2));
}