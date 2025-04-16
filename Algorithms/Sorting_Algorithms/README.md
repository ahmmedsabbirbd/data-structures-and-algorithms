# Sorting Algorithms

## 1. Bubble Sort

**Conceptual Explanation:**
Bubble sort repeatedly steps through the list, compares adjacent elements, and swaps them if they're in the wrong order. The process repeats until no swaps are needed, indicating the list is sorted.

**Visual Example:**
```
Initial array: [5, 3, 8, 4, 2]

Pass 1:
[3, 5, 8, 4, 2] (Swap 5 and 3)
[3, 5, 8, 4, 2] (No swap for 5 and 8)
[3, 5, 4, 8, 2] (Swap 8 and 4)
[3, 5, 4, 2, 8] (Swap 8 and 2)

Pass 2:
[3, 5, 4, 2, 8] (No swap for 3 and 5)
[3, 4, 5, 2, 8] (Swap 5 and 4)
[3, 4, 2, 5, 8] (Swap 5 and 2)
[3, 4, 2, 5, 8] (No swap for 5 and 8)

And so on until sorted: [2, 3, 4, 5, 8]
```

**JavaScript Implementation:**
```javascript
function bubbleSort(arr) {
  const n = arr.length;
  let swapped;
  
  do {
    swapped = false;
    for (let i = 0; i < n - 1; i++) {
      if (arr[i] > arr[i + 1]) {
        // Swap elements
        [arr[i], arr[i + 1]] = [arr[i + 1], arr[i]];
        swapped = true;
      }
    }
  } while (swapped);
  
  return arr;
}
```

**Pseudocode:**
```
ALGORITHM BubbleSort(arr):
    n = length(arr)
    
    REPEAT:
        swapped = false
        FOR i = 0 TO n-2:
            IF arr[i] > arr[i+1] THEN
                SWAP arr[i] AND arr[i+1]
                swapped = true
            END IF
        END FOR
    UNTIL swapped = false
    
    RETURN arr
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(n²) in worst and average cases, O(n) in best case (already sorted)
- Space Complexity: O(1) - in-place sorting

**Real-world Applications:**
- Educational purposes to teach sorting concepts
- Used in embedded systems with limited memory
- Simple applications with small datasets
- Detecting nearly sorted arrays (with early termination)

## 2. Insertion Sort

**Conceptual Explanation:**
Insertion sort builds the final sorted array one item at a time. It takes one element from the input data, finds its correct position within the sorted part, and inserts it there.

**Visual Example:**
```
Initial array: [5, 3, 8, 4, 2]

[5] | [3, 8, 4, 2] (Start with first element as sorted)
[3, 5] | [8, 4, 2] (Insert 3 before 5)
[3, 5, 8] | [4, 2] (Insert 8 after 5)
[3, 4, 5, 8] | [2] (Insert 4 after 3)
[2, 3, 4, 5, 8] (Insert 2 before 3)
```

**JavaScript Implementation:**
```javascript
function insertionSort(arr) {
  const n = arr.length;
  
  for (let i = 1; i < n; i++) {
    // Current element to be inserted
    let current = arr[i];
    let j = i - 1;
    
    // Find the position for insertion
    while (j >= 0 && arr[j] > current) {
      arr[j + 1] = arr[j]; // Shift elements to the right
      j--;
    }
    
    // Insert the element at the correct position
    arr[j + 1] = current;
  }
  
  return arr;
}
```

**Pseudocode:**
```
ALGORITHM InsertionSort(arr):
    n = length(arr)
    
    FOR i = 1 TO n-1:
        current = arr[i]
        j = i - 1
        
        WHILE j >= 0 AND arr[j] > current:
            arr[j + 1] = arr[j]
            j = j - 1
        END WHILE
        
        arr[j + 1] = current
    END FOR
    
    RETURN arr
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(n²) in worst and average cases, O(n) in best case (already sorted)
- Space Complexity: O(1) - in-place sorting

**Real-world Applications:**
- Efficient for small datasets or nearly sorted data
- Online algorithms where data arrives sequentially
- Used in some database implementations (PostgreSQL for small tables)
- Embedded systems with limited memory
- Can be used as part of more complex algorithms like Timsort

## 3. Selection Sort

**Conceptual Explanation:**
Selection sort divides the input list into two parts: a sorted sublist and an unsorted sublist. It repeatedly finds the minimum element from the unsorted sublist and moves it to the end of the sorted sublist.

**Visual Example:**
```
Initial array: [5, 3, 8, 4, 2]

Find minimum in [5, 3, 8, 4, 2] -> 2
Swap with first element: [2, 3, 8, 4, 5]

Find minimum in [3, 8, 4, 5] -> 3
Already in position: [2, 3, 8, 4, 5]

Find minimum in [8, 4, 5] -> 4
Swap with first unsorted element: [2, 3, 4, 8, 5]

Find minimum in [8, 5] -> 5
Swap with first unsorted element: [2, 3, 4, 5, 8]

Find minimum in [8] -> 8
Already in position: [2, 3, 4, 5, 8]
```

**JavaScript Implementation:**
```javascript
function selectionSort(arr) {
  const n = arr.length;
  
  for (let i = 0; i < n - 1; i++) {
    // Find the minimum element in the unsorted part
    let minIndex = i;
    
    for (let j = i + 1; j < n; j++) {
      if (arr[j] < arr[minIndex]) {
        minIndex = j;
      }
    }
    
    // Swap the found minimum element with the first element of the unsorted part
    if (minIndex !== i) {
      [arr[i], arr[minIndex]] = [arr[minIndex], arr[i]];
    }
  }
  
  return arr;
}
```

**Pseudocode:**
```
ALGORITHM SelectionSort(arr):
    n = length(arr)
    
    FOR i = 0 TO n-2:
        minIndex = i
        
        FOR j = i+1 TO n-1:
            IF arr[j] < arr[minIndex] THEN
                minIndex = j
            END IF
        END FOR
        
        IF minIndex != i THEN
            SWAP arr[i] AND arr[minIndex]
        END IF
    END FOR
    
    RETURN arr
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(n²) in all cases
- Space Complexity: O(1) - in-place sorting

**Real-world Applications:**
- Simple implementation for small datasets
- When memory writes are expensive (performs fewer swaps than bubble sort)
- When simplicity is preferred over efficiency
- Educational purposes

## 4. Merge Sort

**Conceptual Explanation:**
Merge sort uses the divide-and-conquer paradigm. It divides the array into halves, recursively sorts each half, then merges the sorted halves back together.

**Visual Example:**
```
Initial array: [5, 3, 8, 4, 2]

Divide phase:
[5, 3, 8, 4, 2]
[5, 3] [8, 4, 2]
[5] [3] [8] [4, 2]
[5] [3] [8] [4] [2]

Conquer phase (merge):
[3, 5] [4, 8] [2]
[3, 5] [2, 4, 8]
[2, 3, 4, 5, 8]
```

**JavaScript Implementation:**
```javascript
function mergeSort(arr) {
  // Base case: array with 0 or 1 element is already sorted
  if (arr.length <= 1) {
    return arr;
  }
  
  // Divide the array into two halves
  const middle = Math.floor(arr.length / 2);
  const left = arr.slice(0, middle);
  const right = arr.slice(middle);
  
  // Recursively sort both halves
  return merge(mergeSort(left), mergeSort(right));
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
  
  // Add remaining elements
  return result.concat(left.slice(leftIndex)).concat(right.slice(rightIndex));
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
- Parallel processing applications
- Used in counting inversions problem

## 5. Quick Sort

**Conceptual Explanation:**
Quick sort also uses the divide-and-conquer approach. It selects a 'pivot' element and partitions the array around it, placing smaller elements before the pivot and larger elements after it, then recursively sorts the sub-arrays.

**Visual Example:**
```
Initial array: [5, 3, 8, 4, 2]

Choose pivot (e.g., 5):
Partition: [3, 2, 4] [5] [8]

Recursively sort left part:
Choose pivot (e.g., 3):
Partition: [2] [3] [4]

Both subarrays are sorted, combine:
[2, 3, 4, 5, 8]
```

**JavaScript Implementation:**
```javascript
function quickSort(arr, left = 0, right = arr.length - 1) {
  if (left < right) {
    const pivotIndex = partition(arr, left, right);
    
    // Recursively sort elements before and after partition
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
- Time Complexity: O(n²) worst case (when already sorted), O(n log n) average and best case
- Space Complexity: O(log n) due to recursion stack

**Real-world Applications:**
- Default sorting algorithm in many programming language libraries
- Database systems
- Virtual memory management in operating systems
- Efficient for in-memory sorting
- Used in numerical computations and simulations

## 6. Counting Sort

**Conceptual Explanation:**
Counting sort is a non-comparative integer sorting algorithm that works by counting the number of occurrences of each element in the input array. It then uses this count to determine the position of each element in the output array.

**Visual Example:**
```
Input array: [4, 2, 2, 8, 3, 3, 1]

Count occurrences:
count[1] = 1
count[2] = 2
count[3] = 2
count[4] = 1
count[8] = 1

Position each number:
Output: [1, 2, 2, 3, 3, 4, 8]
```

**JavaScript Implementation:**
```javascript
function countingSort(arr) {
  if (arr.length === 0) return arr;
  
  // Find the maximum element to determine the count array size
  const max = Math.max(...arr);
  
  // Create a count array and initialize with zeros
  const count = new Array(max + 1).fill(0);
  
  // Count occurrences of each element
  for (let i = 0; i < arr.length; i++) {
    count[arr[i]]++;
  }
  
  // Modify count array to store the position of each element
  for (let i = 1; i <= max; i++) {
    count[i] += count[i - 1];
  }
  
  // Create output array
  const output = new Array(arr.length);
  
  // Build the output array
  for (let i = arr.length - 1; i >= 0; i--) {
    output[count[arr[i]] - 1] = arr[i];
    count[arr[i]]--;
  }
  
  return output;
}
```

**Pseudocode:**
```
ALGORITHM CountingSort(arr):
    IF length(arr) = 0 THEN
        RETURN arr
    END IF
    
    max = maximum value in arr
    
    // Create count array and initialize with zeros
    count = new array of size (max + 1) filled with 0
    
    // Count occurrences of each element
    FOR i = 0 TO length(arr) - 1:
        count[arr[i]] = count[arr[i]] + 1
    END FOR
    
    // Modify count to store position
    FOR i = 1 TO max:
        count[i] = count[i] + count[i - 1]
    END FOR
    
    // Create output array
    output = new array of size length(arr)
    
    // Build output array
    FOR i = length(arr) - 1 TO 0:
        output[count[arr[i]] - 1] = arr[i]
        count[arr[i]] = count[arr[i]] - 1
    END FOR
    
    RETURN output
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(n + k) where n is the number of elements and k is the range of input values
- Space Complexity: O(n + k) for the count and output arrays

**Real-world Applications:**
- Sorting integers with small range
- Sorting strings (as part of radix sort)
- Data preprocessing in statistical applications
- Used in radix sort algorithm
- Histogram construction

## 7. Radix Sort

**Conceptual Explanation:**
Radix sort is a non-comparative sorting algorithm that sorts integers by processing individual digits. It sorts numbers by their place values, starting from the least significant digit (LSD) to the most significant digit (MSD), using a stable sort for each digit.

**Visual Example:**
```
Input array: [170, 45, 75, 90, 802, 24, 2, 66]

Sort by 1s place:
[170, 90, 802, 2, 24, 45, 75, 66]

Sort by 10s place:
[802, 2, 24, 45, 66, 170, 75, 90]

Sort by 100s place:
[2, 24, 45, 66, 75, 90, 170, 802]
```

**JavaScript Implementation:**
```javascript
function radixSort(arr) {
  // Find the maximum number to know the number of digits
  const max = Math.max(...arr);
  
  // Do counting sort for every digit
  for (let exp = 1; Math.floor(max / exp) > 0; exp *= 10) {
    countingSortByDigit(arr, exp);
  }
  
  return arr;
}

function countingSortByDigit(arr, exp) {
  const n = arr.length;
  const output = new Array(n).fill(0);
  const count = new Array(10).fill(0);
  
  // Count occurrences of each digit
  for (let i = 0; i < n; i++) {
    const digit = Math.floor(arr[i] / exp) % 10;
    count[digit]++;
  }
  
  // Change count[i] so that it contains the position of this digit in output
  for (let i = 1; i < 10; i++) {
    count[i] += count[i - 1];
  }
  
  // Build the output array
  for (let i = n - 1; i >= 0; i--) {
    const digit = Math.floor(arr[i] / exp) % 10;
    output[count[digit] - 1] = arr[i];
    count[digit]--;
  }
  
  // Copy the output array to arr, so that arr contains sorted numbers
  for (let i = 0; i < n; i++) {
    arr[i] = output[i];
  }
}
```

**Pseudocode:**
```
ALGORITHM RadixSort(arr):
    // Find the maximum number to know the number of digits
    max = maximum value in arr
    
    // Do counting sort for every digit
    FOR exp = 1 TO floor(max / exp) > 0 STEP exp *= 10:
        CountingSortByDigit(arr, exp)
    END FOR
    
    RETURN arr
END ALGORITHM

ALGORITHM CountingSortByDigit(arr, exp):
    n = length(arr)
    output = new array of size n filled with 0
    count = new array of size 10 filled with 0
    
    // Count occurrences of each digit
    FOR i = 0 TO n - 1:
        digit = floor(arr[i] / exp) % 10
        count[digit] = count[digit] + 1
    END FOR
    
    // Change count[i] so that it contains the position of this digit in output
    FOR i = 1 TO 9:
        count[i] = count[i] + count[i - 1]
    END FOR
    
    // Build the output array
    FOR i = n - 1 TO 0:
        digit = floor(arr[i] / exp) % 10
        output[count[digit] - 1] = arr[i]
        count[digit] = count[digit] - 1
    END FOR
    
    // Copy the output array to arr
    FOR i = 0 TO n - 1:
        arr[i] = output[i]
    END FOR
END ALGORITHM
```

**Time and Space Complexity:**
- Time Complexity: O(d * (n + k)) where d is the number of digits, n is the number of elements, and k is the range of digits (usually 10)
- Space Complexity: O(n + k)

**Real-world Applications:**
- Sorting large integers or long strings
- Database indexing systems
- Lexicographical sorting of strings
- IP address sorting
- Used in computer graphics for sorting polygons by depth
