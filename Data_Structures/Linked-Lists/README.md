## What is a Linked List?
A **Linked List** is a **linear data structure** where each element (called a **node**) points to the **next** one. It's like a train: each coach is connected to the next.

```
[10 | next] → [20 | next] → [30 | next] → null
```

Each node contains:
1. **Data** (the value)
2. **Pointer/Reference** to the next node

## 📦 Real-Life Analogy
Imagine a **chain of people** holding hands:
* Each person has a **name** (data)
* Each one **knows** who's next (reference)
* You can **walk through them one by one**, but you **can't jump directly** to the 4th person like in arrays.

## 🧠 Why Use Linked Lists?

| When to Use | Why |
|-------------|-----|
| You need **dynamic memory allocation** | No need to define size like arrays |
| Frequent **insert/delete** operations | Fast insert/delete at beginning/middle |
| Memory reallocation is expensive | It stores elements in non-contiguous memory |

## 🧪 Linked List vs Array

| Feature | Array | Linked List |
|---------|-------|-------------|
| Access by index | ✅ Fast (O(1)) | ❌ Slow (O(n)) |
| Insert/Delete at start | ❌ Slow (O(n)) | ✅ Fast (O(1)) |
| Memory | Contiguous | Non-contiguous |
| Fixed size | Yes | No |
| Overhead | Low | High (extra pointer memory) |

## ⚙️ How a Linked List Looks in JavaScript (Custom Class)

```javascript
class Node {
    constructor(data) {
        this.data = data;
        this.next = null;
    }
}

class LinkedList {
    constructor() {
        this.head = null;
    }
    
    append(data) {
        const newNode = new Node(data);
        
        if (!this.head) {
            this.head = newNode;
            return;
        }
        
        let current = this.head;
        while (current.next) {
            current = current.next;
        }
        current.next = newNode;
    }
    
    printList() {
        let current = this.head;
        while (current) {
            console.log(current.data);
            current = current.next;
        }
    }
}

const list = new LinkedList();
list.append(10);
list.append(20);
list.append(30);
list.printList(); // Output: 10 20 30
```

## 🔄 Types of Linked Lists

| Type | Description |
|------|-------------|
| **Singly Linked List** | Each node points to next |
| **Doubly Linked List** | Each node has `next` and `prev` pointers |
| **Circular Linked List** | Last node points back to head |

## 🧩 Use Cases in Real Life / Coding

| Use Case | Example |
|----------|---------|
| 🎵 Media Playlist | Songs linked in order; skip next/prev |
| 📜 Undo/Redo in Editors | Doubly linked list of actions |
| 🕹️ Game Mechanics | Levels connected in sequence |
| 🧠 Memory Management | OS uses linked lists for free memory blocks |

## 💡 Pros & Cons Summary

### ✅ Pros:
* Dynamic size
* Efficient insert/delete (especially at start)
* Ideal for queues, stacks, etc.

### ❌ Cons:
* Slow access (no random access like arrays)
* More memory (extra pointers)
* Debugging can be trickier

## 📝 Common Operations with Time Complexity

| Operation | Time Complexity | Description |
|-----------|-----------------|-------------|
| Access | O(n) | Need to traverse from head |
| Search | O(n) | Linear search through nodes |
| Insert at beginning | O(1) | Change head pointer |
| Insert at end | O(n) | Traverse to end first |
| Delete at beginning | O(1) | Change head pointer |
| Delete at end | O(n) | Traverse to second-last node |

## 🔍 Common Interview Questions

1. **How to detect a cycle in a linked list?**
   - Use Floyd's Cycle-Finding Algorithm (fast/slow pointers)

2. **How to find the middle element?**
   - Use two pointers: one moves twice as fast as the other

3. **How to reverse a linked list?**
   - Track previous, current, and next pointers while iterating

4. **How to merge two sorted linked lists?**
   - Compare heads of both lists and build result list
