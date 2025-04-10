# Data Structures and Algorithms

## Minimap Path Setup

### 7 Fundamental Data Structures You Must Learn First:

1. **Arrays**
2. **Linked Lists**
3. **Stacks**
4. **Queues**
5. **Hash Tables / Hash Maps / Dictionaries**
6. **Trees**
7. **Graphs**

---

### 7 Essential Algorithms You Should Master:

1. **Searching Algorithms**
2. **Sorting Algorithms**
3. **Time and Space Complexity Analysis**
4. **Divide and Conquer Algorithms**  
   - Examples: Binary Search, Merge Sort
5. **Dynamic Programming (DP)**
6. **Tree-Based Algorithms**
7. **Graph-Based Algorithms**

---

## 1 Prerequisites for DSA

Before diving into **Data Structures and Algorithms (DSA)**, it's crucial to have a solid understanding of the basics of programming, particularly JavaScript. These foundational concepts will make learning DSA much easier.

### 1.1 **Basic Programming Concepts**  
   Familiarize yourself with the basics in any programming language (C/C++/Java/Python/JavaScript). In JavaScript, the key concepts include:

   - **Variable Declaration**  
     JavaScript provides three ways to declare variables: `var`, `let`, and `const`.  
     Example:
     ```js
        let num = 5; // Mutable variable
        const name = 'Sabbir'; // Constant, cannot be reassigned
     ```

   - **Control Flow**  
     Control flow is used to make decisions based on certain conditions. JavaScript uses `if`, `else`, and `switch` statements for control flow..
     ```js
        let num = 10;

        if (num > 5) {
            console.log("Number is greater than 5");
        } else {
            console.log("Number is less than or equal to 5");
        }

     ```

   - **Functions**  
     Functions are reusable blocks of code that perform specific tasks.
     ```js
        function add(a, b) {
            return a + b;
        }

        let result = add(3, 4); 
        console.log(result); // Output: 7

     ```

   - **Loops**  
     Be comfortable with loops like `for`, `while` for repetitive tasks.

     - For Loop:
        ```js
            for (let i = 0; i < 5; i++) {
                console.log(i); // Output: 0 1 2 3 4
            }
        ```
     - For Loop:
        ```js
            let i = 0;
            while (i < 5) {
                console.log(i); // Output: 0 1 2 3 4
                i++;
            }
        ```
  - **Recursion**  
     Recursion occurs when a function calls itself to solve smaller instances of a problem.
       ```js
            function factorial(n) {
                if (n === 0) return 1;
                return n * factorial(n - 1);
            }
            console.log(factorial(5)); // Output: 120
       ``` 

### 1.2 **Arrays and Strings**  
   Understand the fundamental operations and manipulations for arrays and strings, as they form the backbone of many algorithms.
 
   - **Arrays**  
     In JavaScript, an array is an ordered collection of values.  
     Example:
     - Creating and Accessing Arrays:
        ```js
            let arr = [1, 2, 3, 4, 5];
            console.log(arr[2]); // Output: 3
            arr[2] = 10;
            console.log(arr); // Output: [1, 2, 10, 4, 5]
        ```
     - Iterating Over Arrays::
        ```js
            for (let i = 0; i < arr.length; i++) {
                console.log(arr[i]); // Output: 1 2 10 4 5
            }
        ```
 

### 1.3 **Input/Output (I/O) in JavaScript**  
   JavaScript handles input/output in various ways, but for now, we’ll focus on browser-based I/O using `prompt()` and `console.log()`.

   - **Input/Output (I/O)**  
     In JavaScript, an array is an ordered collection of values.  
     - Getting User Input:
        ```js
            let userInput = prompt("Enter a number:"); 
            console.log("You entered: " + userInput);
        ```
     - Console Output:
        ```js
            console.log("Hello, World!"); // Output: Hello, World!
        ```
### 1.4 **Arrays and Strings Operations**  
   JavaScript handles input/output in various ways, but for now, we’ll focus on browser-based I/O using `prompt()` and `console.log()`.

   - **Array Traversa**  
     - Sum of Array Elements:
        ```js
            let arr = [1, 2, 3, 4, 5];
            let sum = 0;
            for (let i = 0; i < arr.length; i++) {
                sum += arr[i];
            }
            console.log(sum); // Output: 15
        ``` 
   - **Array Traversa**  
     - Sum of Array Elements:
        ```js
            function isPalindrome(str) {
                let reversed = str.split('').reverse().join('');
                return str === reversed;
            }

            console.log(isPalindrome("madam")); // Output: true
            console.log(isPalindrome("hello")); // Output: false
        ``` 

---

By mastering the above concepts, you'll be better prepared to tackle the complexities of data structures and algorithms effectively.
