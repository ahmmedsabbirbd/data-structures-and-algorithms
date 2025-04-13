# 🌳 Trees – Data Structure Guide

## 🧾 What Is a Tree?

A **tree** is a special type of data structure that:
* Looks like a real-life **upside-down tree** (root on top 🌱, leaves at bottom 🍃)
* Is **non-linear**
* Consists of **nodes** and **edges**

## 🧠 Basic Structure of a Tree

```
      🌱 Root
      /    \
     /      \
   Node     Node
   /  \      \
Leaf  Leaf   Leaf
```

| Term | Meaning |
|------|---------|
| **Root** | The top node |
| **Node** | Each data point |
| **Edge** | The link/connection between nodes |
| **Child** | A node directly connected below another |
| **Parent** | A node directly connected above |
| **Leaf** | A node with no children |
| **Height** | Longest path from root to a leaf |
| **Depth** | Number of edges from root to the node |
| **Subtree** | Tree formed by a node and its descendants |

## 🧩 Types of Trees (Most Important)

| Tree Type | Description | Example Use |
|-----------|-------------|-------------|
| **Binary Tree** | Max 2 children per node | Basic tree structure |
| **Binary Search Tree (BST)** | Left < root < Right | Fast searching/sorting |
| **AVL Tree** | Self-balancing BST | Database indexing |
| **Red-Black Tree** | Self-balancing BST | System implementations (Java TreeMap, C++ map) |
| **B-Tree** | Generalized balanced tree (more than 2 children) | Databases, File systems |
| **Heap** | Complete binary tree used for priority | Priority queue, heap sort |
| **Trie (Prefix Tree)** | Special tree for storing strings by prefix | Autocomplete, Spell checker |
| **N-ary Tree** | Node can have *n* number of children | Folder structure, XML parsing |

## 🔥 Differences Between Tree Types (Diffly Share)

| Feature | Binary Tree | BST | Heap | Trie |
|---------|------------|-----|------|------|
| Children | Max 2 | Max 2 | Max 2 | Many |
| Sorting Order | No rule | Left < Root < Right | Based on min/max | Based on prefix |
| Main Use | General | Search | Priority | Word lookup |
| Self-Balancing | No | No | Yes (Heapify) | No |
| Lookup | O(n) | O(log n) | O(1) for root | O(m) where m is key length |
| Insert | O(1) | O(log n) | O(log n) | O(m) |
| Delete | O(1) with reference | O(log n) | O(log n) | O(m) |

## 🧠 Why Use Trees?

| Purpose | How Trees Help |
|---------|---------------|
| Fast search | BST, AVL Trees for O(log n) time |
| Organized data | Parent-child relationship |
| Autocomplete / Spell | Tries store dictionary words efficiently |
| Priority tasks | Heaps for scheduling |
| File systems / Menus | Folders and subfolders = tree structure |
| Hierarchical data | Represent hierarchical relationships naturally |

## 🔗 Real-Life Analogy

| Real-Life Thing | Tree Structure Equivalent |
|-----------------|---------------------------|
| Folder & files | N-ary Tree |
| Company org chart | Tree |
| DOM in HTML | Tree (Document Object Model) |
| Tournament brackets | Binary Tree |
| Family tree | N-ary Tree |
| Decision making | Decision Tree |

## ⚙️ Core Operations

| Operation | Time Complexity (BST) | Time Complexity (Balanced BST) |
|-----------|----------------------|-------------------------------|
| Search | O(log n) to O(n) | O(log n) |
| Insert | O(log n) to O(n) | O(log n) |
| Delete | O(log n) to O(n) | O(log n) |
| Traverse | O(n) | O(n) |

## 🔍 Tree Traversal (How to Visit Nodes)

| Type | Order of Visit | Example Output |
|------|---------------|---------------|
| In-Order | Left → Root → Right | Sorted elements in BST |
| Pre-Order | Root → Left → Right | Used to create a copy of tree |
| Post-Order | Left → Right → Root | Used to delete a tree |
| Level-Order | BFS (top to bottom) | Process level by level |

👉 Used in printing, evaluating expressions, rendering DOM, etc.

## 🛠️ Example (Binary Search Tree)

```
      10
     /  \
    5    15
   / \    \
  2   7   20
```

* In-Order Traversal = 2, 5, 7, 10, 15, 20 ✅ (Sorted)
* Pre-Order Traversal = 10, 5, 2, 7, 15, 20
* Post-Order Traversal = 2, 7, 5, 20, 15, 10
* Level-Order Traversal = 10, 5, 15, 2, 7, 20

## 📝 Tree Implementations

### Binary Search Tree Implementation (JavaScript)

```javascript
class Node {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }
  
  insert(value) {
    const newNode = new Node(value);
    
    if (!this.root) {
      this.root = newNode;
      return this;
    }
    
    let current = this.root;
    
    while (true) {
      // Handle duplicates (can be modified based on requirements)
      if (value === current.value) return undefined;
      
      if (value < current.value) {
        if (!current.left) {
          current.left = newNode;
          return this;
        }
        current = current.left;
      } else {
        if (!current.right) {
          current.right = newNode;
          return this;
        }
        current = current.right;
      }
    }
  }
  
  search(value) {
    if (!this.root) return false;
    
    let current = this.root;
    
    while (current) {
      if (value === current.value) return true;
      if (value < current.value) {
        current = current.left;
      } else {
        current = current.right;
      }
    }
    
    return false;
  }
  
  // Breadth-First Search / Level Order Traversal
  bfs() {
    const result = [];
    const queue = [];
    
    if (this.root) {
      queue.push(this.root);
      
      while (queue.length) {
        const current = queue.shift();
        result.push(current.value);
        
        if (current.left) queue.push(current.left);
        if (current.right) queue.push(current.right);
      }
    }
    
    return result;
  }
  
  // In-Order Traversal: Left, Root, Right
  dfsInOrder() {
    const result = [];
    
    function traverse(node) {
      if (node.left) traverse(node.left);
      result.push(node.value);
      if (node.right) traverse(node.right);
    }
    
    if (this.root) traverse(this.root);
    return result;
  }
  
  // Pre-Order Traversal: Root, Left, Right
  dfsPreOrder() {
    const result = [];
    
    function traverse(node) {
      result.push(node.value);
      if (node.left) traverse(node.left);
      if (node.right) traverse(node.right);
    }
    
    if (this.root) traverse(this.root);
    return result;
  }
  
  // Post-Order Traversal: Left, Right, Root
  dfsPostOrder() {
    const result = [];
    
    function traverse(node) {
      if (node.left) traverse(node.left);
      if (node.right) traverse(node.right);
      result.push(node.value);
    }
    
    if (this.root) traverse(this.root);
    return result;
  }
}

// Usage
const bst = new BinarySearchTree();
bst.insert(10);
bst.insert(5);
bst.insert(15);
bst.insert(2);
bst.insert(7);
bst.insert(20);

console.log(bst.dfsInOrder());   // [2, 5, 7, 10, 15, 20]
console.log(bst.dfsPreOrder());  // [10, 5, 2, 7, 15, 20]
console.log(bst.dfsPostOrder()); // [2, 7, 5, 20, 15, 10]
console.log(bst.bfs());          // [10, 5, 15, 2, 7, 20]
```

### Trie Implementation (JavaScript)

```javascript
class TrieNode {
  constructor() {
    this.children = {};
    this.isEndOfWord = false;
  }
}

class Trie {
  constructor() {
    this.root = new TrieNode();
  }
  
  // Insert a word into the trie
  insert(word) {
    let current = this.root;
    
    for (let char of word) {
      if (!current.children[char]) {
        current.children[char] = new TrieNode();
      }
      current = current.children[char];
    }
    
    current.isEndOfWord = true;
  }
  
  // Search for a word in the trie
  search(word) {
    let current = this.root;
    
    for (let char of word) {
      if (!current.children[char]) {
        return false;
      }
      current = current.children[char];
    }
    
    return current.isEndOfWord;
  }
  
  // Check if any word starts with the given prefix
  startsWith(prefix) {
    let current = this.root;
    
    for (let char of prefix) {
      if (!current.children[char]) {
        return false;
      }
      current = current.children[char];
    }
    
    return true;
  }
  
  // Get all words with the given prefix
  getWordsWithPrefix(prefix) {
    const words = [];
    let current = this.root;
    
    // Navigate to the end of the prefix
    for (let char of prefix) {
      if (!current.children[char]) {
        return words;
      }
      current = current.children[char];
    }
    
    // Helper function to find all words
    function findWords(node, word) {
      if (node.isEndOfWord) {
        words.push(word);
      }
      
      for (let char in node.children) {
        findWords(node.children[char], word + char);
      }
    }
    
    findWords(current, prefix);
    return words;
  }
}

// Usage
const trie = new Trie();
trie.insert("apple");
trie.insert("application");
trie.insert("app");
trie.insert("banana");

console.log(trie.search("apple"));       // true
console.log(trie.search("apples"));      // false
console.log(trie.startsWith("app"));     // true
console.log(trie.getWordsWithPrefix("app")); // ["app", "apple", "application"]
```

### Min-Heap Implementation (JavaScript)

```javascript
class MinHeap {
  constructor() {
    this.values = [];
  }
  
  // Helper methods for index calculation
  getParentIndex(index) {
    return Math.floor((index - 1) / 2);
  }
  
  getLeftChildIndex(index) {
    return 2 * index + 1;
  }
  
  getRightChildIndex(index) {
    return 2 * index + 2;
  }
  
  // Swap elements at two indices
  swap(index1, index2) {
    [this.values[index1], this.values[index2]] = [this.values[index2], this.values[index1]];
  }
  
  // Insert a value and maintain heap property
  insert(value) {
    this.values.push(value);
    this.bubbleUp();
    return this;
  }
  
  // Move the last element up to its correct position
  bubbleUp() {
    let index = this.values.length - 1;
    const element = this.values[index];
    
    while (index > 0) {
      const parentIndex = this.getParentIndex(index);
      const parent = this.values[parentIndex];
      
      if (element >= parent) break;
      
      this.swap(index, parentIndex);
      index = parentIndex;
    }
  }
  
  // Extract the minimum (root) element
  extractMin() {
    if (this.values.length === 0) return null;
    
    const min = this.values[0];
    const end = this.values.pop();
    
    if (this.values.length > 0) {
      this.values[0] = end;
      this.sinkDown();
    }
    
    return min;
  }
  
  // Move the root element down to its correct position
  sinkDown() {
    let index = 0;
    const length = this.values.length;
    const element = this.values[0];
    
    while (true) {
      const leftChildIndex = this.getLeftChildIndex(index);
      const rightChildIndex = this.getRightChildIndex(index);
      let leftChild, rightChild;
      let swapIndex = null;
      
      if (leftChildIndex < length) {
        leftChild = this.values[leftChildIndex];
        if (leftChild < element) {
          swapIndex = leftChildIndex;
        }
      }
      
      if (rightChildIndex < length) {
        rightChild = this.values[rightChildIndex];
        if (
          (swapIndex === null && rightChild < element) || 
          (swapIndex !== null && rightChild < leftChild)
        ) {
          swapIndex = rightChildIndex;
        }
      }
      
      if (swapIndex === null) break;
      
      this.swap(index, swapIndex);
      index = swapIndex;
    }
  }
  
  // Get the minimum element without removing it
  peek() {
    return this.values.length > 0 ? this.values[0] : null;
  }
}

// Usage
const minHeap = new MinHeap();
minHeap.insert(10);
minHeap.insert(5);
minHeap.insert(15);
minHeap.insert(1);

console.log(minHeap.peek());        // 1
console.log(minHeap.extractMin());  // 1
console.log(minHeap.peek());        // 5
```

## 🔄 Tree Balancing Techniques

### AVL Trees

AVL trees maintain balance through rotation operations:
- **Left Rotation**: When right subtree is higher
- **Right Rotation**: When left subtree is higher
- **Left-Right Rotation**: Combined rotation for specific imbalances
- **Right-Left Rotation**: Combined rotation for specific imbalances

Balance factor = height(left subtree) - height(right subtree)

A valid AVL tree maintains a balance factor of -1, 0, or 1 for every node.

### Red-Black Trees

Red-Black trees maintain balance with these properties:
1. Every node is either red or black
2. The root is black
3. All leaf nodes (NIL/null) are black
4. If a node is red, both its children are black
5. Every path from a node to its descendant leaves contains the same number of black nodes

## 🔍 Tree Applications in Detail

### 1. File Systems
Directories and files form a hierarchical tree structure:
- Root directory at the top
- Subdirectories as child nodes
- Files as leaf nodes

### 2. DOM (Document Object Model)
HTML pages are represented as a tree:
- HTML tag as root
- Nested tags as children
- Text content as leaf nodes

### 3. Database Indexing
B-Trees and B+ Trees optimize database operations:
- Fast search, insert, delete
- Balancing for consistent performance
- Efficient range queries

### 4. Decision Trees
Used in machine learning and decision-making:
- Internal nodes: questions/features
- Branches: possible answers
- Leaf nodes: final decisions/classifications

### 5. Syntax Trees
Compilers use Abstract Syntax Trees (AST):
- Parse expressions and statements
- Check syntax and semantics
- Generate code

## 🎯 Summary Table

| Feature | Description |
|---------|-------------|
| Structure | Hierarchical (root, children, leaf) |
| Best Use Cases | Search, hierarchy, autocomplete, folders |
| Common Types | BST, AVL, Heap, Trie, N-ary |
| Performance | O(log n) in balanced trees |
| Space Complexity | O(n) |
| Balancing Techniques | AVL, Red-Black, B-Tree |
| Traversal Methods | In-order, Pre-order, Post-order, Level-order |

## 🧪 Tree Operations Comparison

| Operation | BST (Average) | BST (Worst) | AVL | Red-Black | B-Tree | Heap | Trie |
|-----------|---------------|-------------|-----|-----------|--------|------|------|
| Search | O(log n) | O(n) | O(log n) | O(log n) | O(log n) | O(n) | O(m)* |
| Insert | O(log n) | O(n) | O(log n) | O(log n) | O(log n) | O(log n) | O(m)* |
| Delete | O(log n) | O(n) | O(log n) | O(log n) | O(log n) | O(log n) | O(m)* |
| Min/Max | O(log n)† | O(n)† | O(log n)† | O(log n)† | O(log n)† | O(1) | N/A |

*Where m is the length of the key
†O(h) where h is the height of the tree (log n in balanced trees, potentially n in unbalanced)

## 🌲 Advanced Tree Concepts

### 1. Self-Balancing Trees
Trees that automatically maintain balance:
- AVL Trees: Strict balance factor (-1, 0, 1)
- Red-Black Trees: Less strict, fewer rotations
- B-Trees: Used in databases and file systems
- Splay Trees: Recently accessed nodes moved to root

### 2. Specialized Trees
- **Segment Trees**: Range queries and updates
- **Fenwick Trees (BIT)**: Efficient prefix sums
- **Suffix Trees**: String pattern matching
- **Merkle Trees**: Used in cryptography/blockchain
- **Octrees/Quadtrees**: Spatial partitioning

### 3. Tree Algorithms
- **Lowest Common Ancestor (LCA)**: Find common ancestor of two nodes
- **Diameter**: Longest path between any two nodes
- **Path Sum**: Find if there's a path with a specific sum
- **Tree Isomorphism**: Check if two trees have same structure
