# 📦 Stack Data Structure

A **Stack** is a **linear data structure** that follows the **LIFO** principle: **Last In, First Out**. Think of it like a stack of plates 🍽️—the last one you put on top is the first one you take off.

## Visual Representation

```
TOP → [40] 
      [30] 
      [20] 
BOTTOM→[10]
```

## 🧠 Real-Life Examples

| Scenario | Explanation |
|----------|-------------|
| **Undo/Redo** | The last action is the first to be undone |
| **Browser History** | Back button pops the last visited page |
| **Call Stack** | Functions return in reverse order of calls |
| **Pancake Stack** 🥞 | You eat the top pancake first |

## ⚙️ Core Stack Operations

| Operation | Action | Big O |
|-----------|--------|-------|
| `push(x)` | Add `x` on top | O(1) |
| `pop()` | Remove & return top element | O(1) |
| `peek()` / `top()` | View top element without removing | O(1) |
| `isEmpty()` | Check if stack is empty | O(1) |

## 🧪 Stack vs Queue vs Array

| Feature | Stack | Queue | Array |
|---------|-------|-------|-------|
| Order | LIFO | FIFO | Random access |
| Add/Remove | Top only | Front/Rear | Anywhere |
| Use Case | Undo, Recursion | Tasks, Print jobs | Storage |

## 🔧 JavaScript Stack Example

You can use an array as a stack in JavaScript:

```javascript
const stack = []; 

// Push
stack.push(10);
stack.push(20);
stack.push(30);

// Peek
console.log(stack[stack.length - 1]); // 30

// Pop
console.log(stack.pop()); // 30
console.log(stack); // [10, 20]
```

Or build your own class:

```javascript
class Stack {
  constructor() {
    this.items = [];
  }
  
  push(item) {
    this.items.push(item);
  }
  
  pop() {
    return this.items.pop();
  }
  
  peek() {
    return this.items[this.items.length - 1];
  }
  
  isEmpty() {
    return this.items.length === 0;
  }
}
```

## 🧠 Common Use Cases

| Use Case | Real-World |
|----------|------------|
| 🔄 Undo feature | Google Docs, Photoshop |
| 📞 Function Call Stack | Recursion, JavaScript Call Stack |
| 👨‍💻 Expression Parsing | `((2+3)*4)` – evaluate math expressions |
| 📍 Balanced Parentheses | `((){})` = valid or not? |
| 🧠 Backtracking | Puzzle solving (Maze, Sudoku) |

## 💡 Pros & Cons

### ✅ Pros
* Simple and fast (O(1) push/pop)
* Great for nested/recursive logic
* Predictable behavior (LIFO)

### ❌ Cons
* Only works from one end (no random access)
* Can overflow (in limited memory environments)

## Advanced Stack Applications

1. **Browser Navigation**: Forward and back buttons use stacks to track history
2. **Text Editor Undo/Redo**: Changes are pushed to a stack for undo operations
3. **Memory Management**: Function calls and returns are managed using a stack
4. **Syntax Parsing**: Compilers use stacks to validate syntax and parse expressions
5. **Depth-First Search**: Graph traversal algorithms often utilize stacks
