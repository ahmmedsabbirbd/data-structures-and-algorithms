# 🚦 Queue Data Structure

A **Queue** is a **linear data structure** that follows the **FIFO** principle: **First In, First Out**. Just like people standing in line - the first person who arrives is the first to be served.

## Visual Representation

```
FRONT → [10] [20] [30] [40] ← REAR
```

## 🧠 Real-Life Examples

| Real World | Queue Representation |
|------------|----------------------|
| 🚌 Bus Stop | First person gets on first |
| 📞 Call Center | First caller gets answered first |
| 🖨️ Printer Queue | First file prints first |
| 🧾 Token System | Bank or hospital queue |

## ⚙️ Core Queue Operations

| Operation | Description | Big O |
|-----------|-------------|-------|
| `enqueue(x)` | Add `x` at the end (rear) | O(1) |
| `dequeue()` | Remove & return front item | O(1)* |
| `peek()` / `front()` | View front item without removing | O(1) |
| `isEmpty()` | Check if queue is empty | O(1) |

*Note: Dequeue is O(1) with linked list implementation but O(n) with array implementation if using shift()

## 🔁 Queue vs Stack vs Array

| Feature | Queue (FIFO) | Stack (LIFO) | Array |
|---------|--------------|--------------|-------|
| Access Order | First → Last | Last → First | Random |
| Insert/Remove | Rear/Front | Top only | Any index |
| Use Case | Tasks, Print Queue | Undo, Recursion | Generic data storage |

## 🛠️ JavaScript Queue Example (Array-based)

```javascript
const queue = []; 

// Enqueue
queue.push(10);
queue.push(20);
queue.push(30);

// Peek front
console.log(queue[0]); // 10

// Dequeue
console.log(queue.shift()); // 10
console.log(queue); // [20, 30]
```

## 🔧 Custom Queue Class in JS

```javascript
class Queue {
  constructor() {
    this.items = [];
  }
  
  enqueue(item) {
    this.items.push(item);
  }
  
  dequeue() {
    return this.items.shift();
  }
  
  peek() {
    return this.items[0];
  }
  
  isEmpty() {
    return this.items.length === 0;
  }
}
```

## 🔄 Types of Queues

| Type | Description |
|------|-------------|
| 🚶‍♂️ **Simple Queue** | FIFO |
| 🔄 **Circular Queue** | Wraps around (useful in memory management) |
| 🐍 **Deque (Double Ended Queue)** | Insert/remove from both ends |
| 🧠 **Priority Queue** | Elements served based on priority, not position |

## 💼 Use Cases of Queues

| Use Case | Description |
|----------|-------------|
| 🧠 **CPU Scheduling** | Jobs handled in order |
| 📤 **Asynchronous Tasks** | Email sending, job queues |
| 🎮 **Game AI** | Pathfinding (BFS) |
| 🧮 **Data Buffers** | Streaming data or pipelines |

## ✅ Pros & ❌ Cons

### ✅ Pros:
* Simple and fast for order-based tasks
* Prevents starvation (everyone gets served)
* Clean separation of tasks (FIFO)

### ❌ Cons:
* Slow dequeue if implemented with arrays (JS `.shift()` is O(n))
* Doesn't allow access to middle items

## Queue Implementations in Different Languages

### Python

```python
# Using collections.deque (efficient)
from collections import deque

queue = deque()
queue.append(10)       # enqueue
queue.append(20)
front_item = queue.popleft()  # dequeue

# Custom implementation
class Queue:
    def __init__(self):
        self.items = []
    
    def enqueue(self, item):
        self.items.append(item)
    
    def dequeue(self):
        if not self.is_empty():
            return self.items.pop(0)  # O(n) operation
    
    def peek(self):
        if not self.is_empty():
            return self.items[0]
    
    def is_empty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)
```

### Java

```java
import java.util.LinkedList;
import java.util.Queue;

// Using built-in Queue interface
Queue<Integer> queue = new LinkedList<>();
queue.add(10);    // enqueue
queue.add(20);
int frontItem = queue.poll();  // dequeue
int peekItem = queue.peek();   // peek
boolean isEmpty = queue.isEmpty();

// Custom implementation
public class CustomQueue<T> {
    private LinkedList<T> items = new LinkedList<>();
    
    public void enqueue(T item) {
        items.addLast(item);
    }
    
    public T dequeue() {
        if (!isEmpty()) {
            return items.removeFirst();
        }
        return null;
    }
    
    public T peek() {
        if (!isEmpty()) {
            return items.getFirst();
        }
        return null;
    }
    
    public boolean isEmpty() {
        return items.isEmpty();
    }
    
    public int size() {
        return items.size();
    }
}
```

### C++

```cpp
#include <queue>
#include <iostream>

int main() {
    // Using STL queue
    std::queue<int> q;
    q.push(10);  // enqueue
    q.push(20);
    
    int front_item = q.front();  // peek
    q.pop();  // dequeue
    bool is_empty = q.empty();
    
    return 0;
}

// Custom implementation
template <typename T>
class CustomQueue {
private:
    std::list<T> items;
    
public:
    void enqueue(T item) {
        items.push_back(item);
    }
    
    T dequeue() {
        if (!isEmpty()) {
            T front = items.front();
            items.pop_front();
            return front;
        }
        throw std::runtime_error("Queue is empty");
    }
    
    T peek() {
        if (!isEmpty()) {
            return items.front();
        }
        throw std::runtime_error("Queue is empty");
    }
    
    bool isEmpty() {
        return items.empty();
    }
    
    size_t size() {
        return items.size();
    }
};
```

## Advanced Queue Implementations

### Circular Queue

A circular queue optimizes memory usage by wrapping around to the beginning when it reaches the end of its allocated memory.

```javascript
class CircularQueue {
  constructor(capacity) {
    this.items = new Array(capacity);
    this.capacity = capacity;
    this.front = -1;
    this.rear = -1;
    this.size = 0;
  }
  
  enqueue(item) {
    if (this.isFull()) {
      return false;
    }
    
    if (this.isEmpty()) {
      this.front = 0;
    }
    
    this.rear = (this.rear + 1) % this.capacity;
    this.items[this.rear] = item;
    this.size++;
    return true;
  }
  
  dequeue() {
    if (this.isEmpty()) {
      return null;
    }
    
    const item = this.items[this.front];
    
    if (this.front === this.rear) {
      // Last element
      this.front = -1;
      this.rear = -1;
    } else {
      this.front = (this.front + 1) % this.capacity;
    }
    
    this.size--;
    return item;
  }
  
  peek() {
    if (this.isEmpty()) {
      return null;
    }
    return this.items[this.front];
  }
  
  isEmpty() {
    return this.size === 0;
  }
  
  isFull() {
    return this.size === this.capacity;
  }
}
```

### Priority Queue

A priority queue serves elements based on their priority rather than insertion order.

```javascript
class PriorityQueue {
  constructor() {
    this.items = [];
  }
  
  // Higher priority number = higher priority
  enqueue(item, priority) {
    const queueElement = { item, priority };
    let added = false;
    
    for (let i = 0; i < this.items.length; i++) {
      if (queueElement.priority > this.items[i].priority) {
        this.items.splice(i, 0, queueElement);
        added = true;
        break;
      }
    }
    
    if (!added) {
      this.items.push(queueElement);
    }
  }
  
  dequeue() {
    if (this.isEmpty()) {
      return null;
    }
    return this.items.shift().item;
  }
  
  peek() {
    if (this.isEmpty()) {
      return null;
    }
    return this.items[0].item;
  }
  
  isEmpty() {
    return this.items.length === 0;
  }
}
```

## Queue vs BFS vs Breadth-First Traversal

Queues are fundamental to breadth-first search (BFS) algorithms:

1. **BFS Algorithm**: Uses a queue to explore all neighbors at the current depth before moving to nodes at the next depth
2. **Level Order Traversal**: Tree traversal that uses a queue to visit nodes level by level
3. **Shortest Path Finding**: In unweighted graphs, BFS with a queue guarantees the shortest path

## Time and Space Complexity Analysis

| Operation | Array-based Queue | Linked List Queue |
|-----------|------------------|-------------------|
| Enqueue | O(1) | O(1) |
| Dequeue | O(n) | O(1) |
| Peek | O(1) | O(1) |
| Space | O(n) | O(n) |
