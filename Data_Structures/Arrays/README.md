# Arrays: A Comprehensive Guide

## What is an Array?
An **Array** is a **linear data structure** that stores elements in a **contiguous block of memory**, where each element is accessible via an **index**.

📌 In JavaScript:
```javascript
let fruits = ["Apple", "Banana", "Mango"];
console.log(fruits[1]); // Output: Banana
```

## 🧠 Why Arrays Are Important?
Arrays provide **fast access** (O(1) time) to elements using an index and are often used when you want to store **multiple values** in a single variable.

## ✅ Real-Life Examples & Use Cases

| Scenario | Description |
|----------|-------------|
| 📦 Shopping Cart | An array holds all the items in a user's cart: `["bread", "milk", "eggs"]` |
| 🕒 Daily Schedule | Each hour's task can be stored in an array of size 24 |
| 📊 Chart Data | Used to store values in charts like line/bar charts: `[100, 200, 300]` |
| 📍 Maps or Grid | 2D arrays represent maps, grids, or matrices (e.g., games, paths) |

## 🔁 Common Array Operations

| Operation | Description | Time Complexity |
|-----------|-------------|----------------|
| Access | `arr[i]` retrieves element at index `i` | O(1) |
| Update | `arr[i] = x` updates value at index | O(1) |
| Insert at end | `arr.push(x)` | O(1) (amortized) |
| Remove at end | `arr.pop()` | O(1) |
| Insert at beginning | `arr.unshift(x)` | O(n) |
| Delete at beginning | `arr.shift()` | O(n) |
| Loop through | `for` loop or `forEach()` | O(n) |

## 📚 Example Use Cases

### 1. **Store Scores of a Game**
```javascript
let scores = [98, 85, 92, 75];
let highest = Math.max(...scores);
console.log("Highest Score:", highest); // 98
```

### 2. **Reverse a Sentence**
```javascript
let sentence = "hello world".split(" ").reverse().join(" ");
console.log(sentence); // Output: world hello
```

### 3. **Find Average Temperature**
```javascript
let temps = [28, 30, 29, 31];
let sum = temps.reduce((acc, curr) => acc + curr, 0);
let avg = sum / temps.length;
console.log("Average:", avg); // 29.5
```

## 🔄 Advanced Operations with Arrays

### 🔹 Sliding Window (Optimize performance)
Find the max sum of any 3 consecutive elements:
```javascript
let arr = [1, 2, 3, 4, 5];
let k = 3;
let maxSum = 0;

for (let i = 0; i <= arr.length - k; i++) {
  let windowSum = 0;
  for (let j = i; j < i + k; j++) {
    windowSum += arr[j];
  }
  maxSum = Math.max(maxSum, windowSum);
}

console.log(maxSum); // Output: 12
```

### 🔹 Two Pointers (Finding pair with sum)
```javascript
let nums = [1, 2, 4, 7, 11, 15];
let target = 15;
let i = 0, j = nums.length - 1;

while (i < j) {
  let sum = nums[i] + nums[j];
  if (sum === target) {
    console.log(`Found: ${nums[i]}, ${nums[j]}`);
    break;
  } else if (sum < target) i++;
  else j--;
}
// Output: Found: 4, 11
```

## ✅ Benefits of Using Arrays

| Benefit | Why it Matters |
|---------|----------------|
| 🔍 Fast Access | Access any element instantly using index |
| 🧩 Easy Iteration | Can be looped through easily using `for` or `forEach` |
| 📈 Compatible | Used in most algorithms (sorting, searching, etc.) |
| ⚡ Memory Efficient | Stores data in contiguous memory locations |

## Common Array Methods in JavaScript

### Basic Methods
- `push()`: Add elements to the end
- `pop()`: Remove element from the end
- `shift()`: Remove element from the beginning
- `unshift()`: Add elements to the beginning
- `splice()`: Add/remove elements at a specific position
- `slice()`: Return a portion of an array

### Transformation Methods
- `map()`: Create a new array by transforming each element
- `filter()`: Create a new array with elements that pass a test
- `reduce()`: Reduce array to a single value by applying a function
- `sort()`: Sort the elements of an array
- `reverse()`: Reverse the order of elements

### Search Methods
- `indexOf()`: Find the first index of an element
- `lastIndexOf()`: Find the last index of an element
- `find()`: Return the first element that passes a test
- `findIndex()`: Return the index of the first element that passes a test
- `includes()`: Check if an array contains a specified element

## Implementation in Different Languages

### JavaScript
```javascript
// Declaration
let numbers = [1, 2, 3, 4, 5];

// Access
console.log(numbers[2]); // 3

// Iterate
numbers.forEach(num => console.log(num));

// Functional methods
let doubled = numbers.map(num => num * 2);
let sum = numbers.reduce((total, num) => total + num, 0);
```

### Python
```python
# Declaration
numbers = [1, 2, 3, 4, 5]

# Access
print(numbers[2])  # 3

# Iterate
for num in numbers:
    print(num)

# List comprehension
doubled = [num * 2 for num in numbers]
```

### Java
```java
// Declaration
int[] numbers = {1, 2, 3, 4, 5};

// Access
System.out.println(numbers[2]); // 3

// Iterate
for (int num : numbers) {
    System.out.println(num);
}
```

## Multi-dimensional Arrays

Arrays can have multiple dimensions, with the most common being two-dimensional arrays (matrices):

```javascript
// 2D array (matrix)
let matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

// Access element at row 1, column 2
console.log(matrix[1][2]); // 6

// Iterate through a 2D array
for (let i = 0; i < matrix.length; i++) {
  for (let j = 0; j < matrix[i].length; j++) {
    console.log(matrix[i][j]);
  }
}
```

## Common Array Problems and Solutions

### 1. Find the Maximum Element
```javascript
function findMax(arr) {
  return Math.max(...arr);
}
```

### 2. Check if an Array is Sorted
```javascript
function isSorted(arr) {
  for (let i = 0; i < arr.length - 1; i++) {
    if (arr[i] > arr[i + 1]) return false;
  }
  return true;
}
```

### 3. Remove Duplicates
```javascript
function removeDuplicates(arr) {
  return [...new Set(arr)];
}
```

### 4. Rotate an Array
```javascript
function rotateArray(arr, k) {
  k = k % arr.length;
  return [...arr.slice(arr.length - k), ...arr.slice(0, arr.length - k)];
}
```

## Memory Representation of Arrays

Arrays are stored in contiguous memory locations. If the starting address of an array is `baseAddress` and the size of each element is `size`, then the address of the `i`th element can be calculated as:

```
address of arr[i] = baseAddress + (i * size)
```

This is why array access is O(1) - the memory address can be calculated directly without traversing through previous elements.

---

Arrays are fundamental data structures used across all programming domains. Understanding how to effectively use arrays is essential for efficient algorithm design and problem-solving.
