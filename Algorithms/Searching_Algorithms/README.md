# Searching Algorithms

## 1. Linear Search

**Conceptual Explanation:**
Linear search is the simplest searching algorithm that works by sequentially checking each element in a collection until it finds the target value or reaches the end of the collection. It doesn't require the data to be sorted.

**Visual Example:**
```
Array: [5, 3, 8, 4, 2]
Searching for 8:

Check 5 - Not a match
Check 3 - Not a match
Check 8 - Match found at index 2
```

**JavaScript Implementation:**
```javascript
function linearSearch(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === target) {
      return i; // Return the index where target is found
    }
  }
  return -1; // Target not found
}

// Example usage
const array = [5, 3, 8, 4, 2];
const targetValue = 8;
const result = linearSearch(array, targetValue);

console.log(`Searching for ${targetValue} in [${array}]`);
if (result !== -1) {
  console.log(`Found at index ${result}`);
} else {
  console.log("Not found");
}
```

**Pseudocode:**
```
ALGORITHM LinearSearch(arr, target):
    FOR i = 0 TO length(arr) - 1:
        IF arr[i] = target THEN
            RETURN i
        END IF
    END FOR
    
    RETURN -1  // Target not found
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(n) - must check each element in worst case
- Space Complexity: O(1) - uses constant space regardless of input size

**Real-world Applications:**
- Searching small datasets
- Unsorted datasets where sorting would be inefficient
- Single occurrence checks
- Searching linked lists or other non-indexed data structures
- Finding all occurrences of an element

## 2. Binary Search

**Conceptual Explanation:**
Binary search is an efficient algorithm that works on sorted arrays by repeatedly dividing the search interval in half. It compares the target value to the middle element and eliminates half of the remaining elements in each step.

**Visual Example:**
```
Sorted array: [2, 3, 4, 5, 8]
Searching for 5:

Step 1: Middle element is 4
5 > 4, so search right half: [5, 8]

Step 2: Middle element is 5
5 = 5, element found at index 3
```

**JavaScript Implementation:**
```javascript
function binarySearch(arr, target) {
  let left = 0;
  let right = arr.length - 1;
  
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    
    // Check if target is present at mid
    if (arr[mid] === target) {
      return mid;
    }
    
    // If target is greater, ignore left half
    if (arr[mid] < target) {
      left = mid + 1;
    } 
    // If target is smaller, ignore right half
    else {
      right = mid - 1;
    }
  }
  
  return -1; // Target not found
}

// Example usage
const sortedArray = [2, 3, 4, 5, 8];
const targetValue = 5;
const result = binarySearch(sortedArray, targetValue);

console.log(`Searching for ${targetValue} in [${sortedArray}]`);
if (result !== -1) {
  console.log(`Found at index ${result}`);
} else {
  console.log("Not found");
}
```

**Recursive Implementation:**
```javascript
function binarySearchRecursive(arr, target, left = 0, right = arr.length - 1) {
  // Base case - element not found
  if (left > right) {
    return -1;
  }
  
  const mid = Math.floor((left + right) / 2);
  
  // Found the element
  if (arr[mid] === target) {
    return mid;
  }
  
  // Search in left or right half
  if (arr[mid] > target) {
    return binarySearchRecursive(arr, target, left, mid - 1);
  } else {
    return binarySearchRecursive(arr, target, mid + 1, right);
  }
}
```

**Pseudocode:**
```
ALGORITHM BinarySearch(arr, target):
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
    
    RETURN -1  // Target not found
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(log n) - each step eliminates half of the remaining elements
- Space Complexity: O(1) for iterative implementation, O(log n) for recursive due to the call stack

**Real-world Applications:**
- Dictionary and encyclopedia lookups
- Database indexing and searching
- Finding files in directories
- Machine learning (e.g., in decision trees)
- Network routing algorithms
- Computer graphics (binary space partitioning)
- Efficient array traversal in sorted datasets

## 3. Binary Search on Answers

**Conceptual Explanation:**
Binary search on answers (also known as binary search on result) is a technique used to solve optimization problems by transforming them into decision problems. Instead of searching for a specific element, we binary search for the optimal answer value within a range.

**Visual Example:**
Problem: Find the minimum capacity needed to ship all packages within D days.
```
Packages weights: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
Days allowed: 5

Possible answer range: [10, 55] (max weight to sum of all weights)
Binary search process:

Try capacity = 32:
Can we ship with capacity 32? Yes (easily within 5 days)
Now search lower half: [10, 31]

Try capacity = 20:
Can we ship with capacity 20? Yes
Now search lower half: [10, 19]

Try capacity = 14:
Can we ship with capacity 14? Yes
Now search lower half: [10, 13]

Try capacity = 11:
Can we ship with capacity 11? Yes
Now search lower half: [10, 10]

Try capacity = 10:
Can we ship with capacity 10? No (would need more than 5 days)

Final answer: 11
```

**JavaScript Implementation:**
```javascript
// Example: Find minimum capacity needed to ship all packages within D days
function shipWithinDays(weights, days) {
  // Helper function to check if we can ship with given capacity
  function canShipWithCapacity(capacity) {
    let dayCount = 1;
    let currentWeight = 0;
    
    for (const weight of weights) {
      // If a single package is heavier than capacity, we can't ship
      if (weight > capacity) return false;
      
      // If adding current package exceeds capacity, put it in next day
      if (currentWeight + weight > capacity) {
        dayCount++;
        currentWeight = weight;
      } else {
        currentWeight += weight;
      }
      
      // If we need more days than allowed, return false
      if (dayCount > days) return false;
    }
    
    return true;
  }
  
  // Binary search on the capacity
  let left = Math.max(...weights); // Minimum capacity is the heaviest package
  let right = weights.reduce((a, b) => a + b, 0); // Maximum is sum of all weights
  
  while (left < right) {
    const mid = Math.floor((left + right) / 2);
    
    if (canShipWithCapacity(mid)) {
      right = mid; // Try a smaller capacity
    } else {
      left = mid + 1; // Need a larger capacity
    }
  }
  
  return left; // Minimum capacity needed
}
```

**Another Example: Finding Square Root Using Binary Search:**
```javascript
function mySqrt(x) {
  if (x <= 1) return x;
  
  let left = 1;
  let right = Math.floor(x / 2);
  
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    
    if (mid * mid === x) {
      return mid;
    } else if (mid * mid < x) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }
  
  return right; // Return the floor value
}
```

**Pseudocode:**
```
ALGORITHM ShipWithinDays(weights, days):
    // Helper function to check if we can ship with given capacity
    FUNCTION CanShipWithCapacity(capacity):
        dayCount = 1
        currentWeight = 0
        
        FOR each weight in weights:
            IF weight > capacity THEN
                RETURN false
            END IF
            
            IF currentWeight + weight > capacity THEN
                dayCount = dayCount + 1
                currentWeight = weight
            ELSE
                currentWeight = currentWeight + weight
            END IF
            
            IF dayCount > days THEN
                RETURN false
            END IF
        END FOR
        
        RETURN true
    END FUNCTION
    
    // Binary search on the capacity
    left = maximum element in weights
    right = sum of all weights
    
    WHILE left < right:
        mid = floor((left + right) / 2)
        
        IF CanShipWithCapacity(mid) THEN
            right = mid
        ELSE
            left = mid + 1
        END IF
    END WHILE
    
    RETURN left
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(n log range) where n is the size of the input array and range is the difference between possible minimum and maximum answer values
- Space Complexity: O(1) - uses constant extra space

**Real-world Applications:**
- Resource allocation problems
- Load balancing in distributed systems
- Finding optimum in monotonic functions
- Minimizing maximum or maximizing minimum problems
- Finding the rate of continuous processes (e.g., manufacturing)
- Optimization problems in logistics and scheduling
- Approximating real number solutions

## 4. Jump Search

**Conceptual Explanation:**
Jump search is an algorithm for sorted arrays that works by jumping ahead by fixed steps and then performing a linear search backward when needed. It's a middle ground between linear search and binary search.

**Visual Example:**
```
Sorted array: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377]
Jump size: √15 ≈ 4
Searching for 55:

Check arr[0] = 0 (< 55)
Jump to arr[4] = 3 (< 55)
Jump to arr[8] = 21 (< 55)
Jump to arr[12] = 144 (> 55)

Linear search from arr[8] to arr[11]:
arr[8] = 21 (< 55)
arr[9] = 34 (< 55)
arr[10] = 55 (= 55) - Found!
```

**JavaScript Implementation:**
```javascript
function jumpSearch(arr, target) {
  const n = arr.length;
  if (n === 0) return -1;
  
  // Finding the jump step size
  const step = Math.floor(Math.sqrt(n));
  
  // Finding the block where element is present (if exists)
  let prev = 0;
  while (arr[Math.min(step, n) - 1] < target) {
    prev = step;
    step += Math.floor(Math.sqrt(n));
    if (prev >= n) return -1; // Element not present
  }
  
  // Linear search in the identified block
  while (arr[prev] < target) {
    prev++;
    if (prev === Math.min(step, n)) return -1; // Element not present
  }
  
  // If element is found
  if (arr[prev] === target) return prev;
  
  return -1; // Element not present
}
```

**Pseudocode:**
```
ALGORITHM JumpSearch(arr, target):
    n = length(arr)
    IF n = 0 THEN
        RETURN -1
    END IF
    
    step = floor(sqrt(n))
    prev = 0
    
    // Finding the block where element is present
    WHILE arr[min(step, n) - 1] < target:
        prev = step
        step = step + floor(sqrt(n))
        IF prev >= n THEN
            RETURN -1
        END IF
    END WHILE
    
    // Linear search in the identified block
    WHILE arr[prev] < target:
        prev = prev + 1
        IF prev = min(step, n) THEN
            RETURN -1
        END IF
    END WHILE
    
    // If element is found
    IF arr[prev] = target THEN
        RETURN prev
    END IF
    
    RETURN -1
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(√n) - Optimal when jump size is set to √n
- Space Complexity: O(1) - uses constant extra space

**Real-world Applications:**
- Medium-sized sorted datasets
- When binary search is too complex to implement
- Memory-constrained environments
- Systems with slower backward traversal

## 5. Interpolation Search

**Conceptual Explanation:**
Interpolation search is an improved variant of binary search that works best for uniformly distributed data. Instead of always checking the middle element, it uses a formula to estimate the most likely position of the target value.

**Visual Example:**
```
Sorted array: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
Searching for 7:

Step 1: Using the interpolation formula:
pos = low + ((target - arr[low]) * (high - low)) / (arr[high] - arr[low])
pos = 0 + ((7 - 0) * (9 - 0)) / (9 - 0) = 7
Check arr[7] = 7 - Found!
```

**JavaScript Implementation:**
```javascript
function interpolationSearch(arr, target) {
  let low = 0;
  let high = arr.length - 1;
  
  while (low <= high && target >= arr[low] && target <= arr[high]) {
    // Formula for position estimation
    const pos = low + Math.floor(
      ((target - arr[low]) * (high - low)) / (arr[high] - arr[low])
    );
    
    // Target found
    if (arr[pos] === target) {
      return pos;
    }
    
    // Target is larger, search in right part
    if (arr[pos] < target) {
      low = pos + 1;
    } 
    // Target is smaller, search in left part
    else {
      high = pos - 1;
    }
  }
  
  return -1; // Element not found
}
```

**Pseudocode:**
```
ALGORITHM InterpolationSearch(arr, target):
    low = 0
    high = length(arr) - 1
    
    WHILE low <= high AND target >= arr[low] AND target <= arr[high]:
        // Formula for position estimation
        pos = low + floor(((target - arr[low]) * (high - low)) / (arr[high] - arr[low]))
        
        IF arr[pos] = target THEN
            RETURN pos
        ELSE IF arr[pos] < target THEN
            low = pos + 1
        ELSE
            high = pos - 1
        END IF
    END WHILE
    
    RETURN -1  // Element not found
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(log log n) average case for uniformly distributed data, O(n) worst case
- Space Complexity: O(1) - uses constant extra space

**Real-world Applications:**
- Searching in phone books or dictionaries
- Database systems with uniformly distributed keys
- Search in systems where elements are organized by statistical distribution
- Optimized version in some database indexing structures

## 6. Exponential Search

**Conceptual Explanation:**
Exponential search involves two steps: finding a range where the target might exist by exponentially increasing the index, and then applying binary search within that range. It's particularly useful for unbounded or infinite arrays.

**Visual Example:**
```
Sorted array: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]
Searching for 11:

Step 1: Check powers of 2
Check arr[1] = 2 (< 11)
Check arr[2] = 3 (< 11)
Check arr[4] = 5 (< 11)
Check arr[8] = 9 (< 11)
Check arr[16] - out of bounds, range found: [8, 15]

Step 2: Binary search in range [8, 15]
Middle: arr[11] = 12 (> 11)
New range: [8, 10]
Middle: arr[9] = 10 (< 11)
New range: [10, 10]
Middle: arr[10] = 11 (= 11) - Found!
```

**JavaScript Implementation:**
```javascript
function exponentialSearch(arr, target) {
  const n = arr.length;
  
  // If target is the first element
  if (arr[0] === target) return 0;
  
  // Find range for binary search by exponentially increasing index
  let i = 1;
  while (i < n && arr[i] <= target) {
    i *= 2;
  }
  
  // Apply binary search in the found range
  return binarySearch(arr, target, i/2, Math.min(i, n-1));
}

function binarySearch(arr, target, left, right) {
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    
    if (arr[mid] === target) {
      return mid;
    }
    
    if (arr[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }
  
  return -1;
}
```

**Pseudocode:**
```
ALGORITHM ExponentialSearch(arr, target):
    n = length(arr)
    
    IF arr[0] = target THEN
        RETURN 0
    END IF
    
    i = 1
    WHILE i < n AND arr[i] <= target:
        i = i * 2
    END WHILE
    
    RETURN BinarySearch(arr, target, i/2, min(i, n-1))
END ALGORITHM

ALGORITHM BinarySearch(arr, target, left, right):
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
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(log i) where i is the position of the target element
- Space Complexity: O(1) for iterative implementation

**Real-world Applications:**
- Searching in unbounded or infinite arrays
- When the element position is closer to the beginning of the array
- Online algorithms where data continues to arrive
- File systems with unknown size limits
- Search in expanding datasets

## 7. Ternary Search

**Conceptual Explanation:**
Ternary search is a divide-and-conquer algorithm that divides the array into three parts rather than two (as in binary search). It's particularly useful for finding the maximum or minimum in a unimodal function.

**Visual Example:**
```
Sorted array: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
Searching for 7:

Step 1: Divide into three parts:
mid1 = low + (high - low) / 3 = 3
mid2 = high - (high - low) / 3 = 6
Check arr[3] = 4 (< 7)
Check arr[6] = 7 (= 7) - Found!
```

**JavaScript Implementation:**
```javascript
function ternarySearch(arr, target) {
  return ternarySearchRecursive(arr, target, 0, arr.length - 1);
}

function ternarySearchRecursive(arr, target, left, right) {
  if (right >= left) {
    // Find the mid1 and mid2
    const mid1 = left + Math.floor((right - left) / 3);
    const mid2 = right - Math.floor((right - left) / 3);
    
    // Check if target is at mid1
    if (arr[mid1] === target) {
      return mid1;
    }
    
    // Check if target is at mid2
    if (arr[mid2] === target) {
      return mid2;
    }
    
    // Check which part should be searched
    if (target < arr[mid1]) {
      // Target lies between left and mid1
      return ternarySearchRecursive(arr, target, left, mid1 - 1);
    } else if (target > arr[mid2]) {
      // Target lies between mid2 and right
      return ternarySearchRecursive(arr, target, mid2 + 1, right);
    } else {
      // Target lies between mid1 and mid2
      return ternarySearchRecursive(arr, target, mid1 + 1, mid2 - 1);
    }
  }
  
  return -1; // Element not found
}
```

**Pseudocode:**
```
ALGORITHM TernarySearch(arr, target, left, right):
    IF right >= left THEN
        mid1 = left + floor((right - left) / 3)
        mid2 = right - floor((right - left) / 3)
        
        IF arr[mid1] = target THEN
            RETURN mid1
        END IF
        
        IF arr[mid2] = target THEN
            RETURN mid2
        END IF
        
        IF target < arr[mid1] THEN
            RETURN TernarySearch(arr, target, left, mid1 - 1)
        ELSE IF target > arr[mid2] THEN
            RETURN TernarySearch(arr, target, mid2 + 1, right)
        ELSE
            RETURN TernarySearch(arr, target, mid1 + 1, mid2 - 1)
        END IF
    END IF
    
    RETURN -1
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(log3 n) - divides search space by 3 in each step
- Space Complexity: O(log3 n) for recursive implementation due to call stack

**Real-world Applications:**
- Finding maximum or minimum in unimodal functions
- Optimization problems
- Scientific computing
- Game theory for finding optimal strategies
- Real-time signal processing
