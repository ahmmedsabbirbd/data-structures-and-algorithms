# Tree-Based Algorithms

## Basic Concept

**Conceptual Explanation:**
Tree-based algorithms operate on hierarchical tree data structures where nodes are connected without cycles. These algorithms are essential for efficiently solving a wide range of problems from search operations to hierarchical data processing. Trees represent data in a parent-child relationship, and algorithms exploit this structure for efficient traversal, search, insertion, and deletion.

A tree consists of:
- Nodes: Elements containing data
- Edges: Connections between nodes
- Root: The topmost node
- Parent/Child relationships: Hierarchical connections
- Leaf nodes: Nodes without children

**Visual Example:**
```
       A         ← Root node
     /   \
    B     C      ← Internal nodes
   / \   / \
  D   E F   G    ← Leaf nodes

This is a binary tree where each node has at most 2 children.
```

**JavaScript Implementation (Basic Tree Node):**
```javascript
// Basic tree node
class TreeNode {
  constructor(val) {
    this.val = val;
    this.children = []; // For n-ary tree
  }
}

// Binary tree node
class BinaryTreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;  // Left child
    this.right = null; // Right child
  }
}
```

**Pseudocode:**
```
CLASS TreeNode:
    value
    children (array or list)
    
    CONSTRUCTOR(value):
        this.value = value
        this.children = empty array or list
    
    FUNCTION addChild(child):
        append child to this.children
END CLASS
```

**Time and Space Complexity:**
- Time and space complexity varies by specific tree algorithm
- Many tree operations run in O(h) time, where h is the height of the tree
- For balanced trees, h is approximately log(n), making these operations O(log n)
- For degenerate/skewed trees, h can be n, making operations O(n) in the worst case

**Real-world Applications:**
- File systems organization (directories and files)
- HTML/XML/JSON document object models
- Network routing algorithms
- Decision-making systems
- Database indexing
- Organization hierarchies
- Game AI (minimax, decision trees)
- Compression algorithms (Huffman coding)

## 1. Binary Tree Traversal Algorithms

**Conceptual Explanation:**
Tree traversal is the process of visiting every node in a tree exactly once. There are several common traversal methods, each with different orders of visiting nodes:

1. **Depth-First Search (DFS)**
   - Preorder (Root → Left → Right)
   - Inorder (Left → Root → Right)
   - Postorder (Left → Right → Root)

2. **Breadth-First Search (BFS)**
   - Level Order (visit nodes level by level)

**Visual Example:**
```
     1
   /   \
  2     3
 / \   / \
4   5 6   7

Preorder: 1, 2, 4, 5, 3, 6, 7
Inorder: 4, 2, 5, 1, 6, 3, 7
Postorder: 4, 5, 2, 6, 7, 3, 1
Level Order: 1, 2, 3, 4, 5, 6, 7
```

**JavaScript Implementation:**

```javascript
// Binary tree node
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

// DFS - Preorder Traversal (Root → Left → Right)
function preorderTraversal(root) {
  const result = [];
  
  function dfs(node) {
    if (!node) return;
    
    // Visit root
    result.push(node.val);
    
    // Visit left subtree
    dfs(node.left);
    
    // Visit right subtree
    dfs(node.right);
  }
  
  dfs(root);
  return result;
}

// DFS - Inorder Traversal (Left → Root → Right)
function inorderTraversal(root) {
  const result = [];
  
  function dfs(node) {
    if (!node) return;
    
    // Visit left subtree
    dfs(node.left);
    
    // Visit root
    result.push(node.val);
    
    // Visit right subtree
    dfs(node.right);
  }
  
  dfs(root);
  return result;
}

// DFS - Postorder Traversal (Left → Right → Root)
function postorderTraversal(root) {
  const result = [];
  
  function dfs(node) {
    if (!node) return;
    
    // Visit left subtree
    dfs(node.left);
    
    // Visit right subtree
    dfs(node.right);
    
    // Visit root
    result.push(node.val);
  }
  
  dfs(root);
  return result;
}

// BFS - Level Order Traversal
function levelOrderTraversal(root) {
  if (!root) return [];
  
  const result = [];
  const queue = [root];
  
  while (queue.length > 0) {
    const levelSize = queue.length;
    const currentLevel = [];
    
    for (let i = 0; i < levelSize; i++) {
      const node = queue.shift();
      
      // Visit the current node
      currentLevel.push(node.val);
      
      // Add children to the queue
      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
    
    result.push(currentLevel);
  }
  
  return result;
}

// Iterative versions of DFS traversals
function preorderIterative(root) {
  if (!root) return [];
  
  const result = [];
  const stack = [root];
  
  while (stack.length > 0) {
    const node = stack.pop();
    
    result.push(node.val);
    
    // Push right first so that left is processed first (LIFO)
    if (node.right) stack.push(node.right);
    if (node.left) stack.push(node.left);
  }
  
  return result;
}

function inorderIterative(root) {
  if (!root) return [];
  
  const result = [];
  const stack = [];
  let current = root;
  
  while (current || stack.length > 0) {
    // Reach the leftmost node
    while (current) {
      stack.push(current);
      current = current.left;
    }
    
    // Process the current node
    current = stack.pop();
    result.push(current.val);
    
    // Move to the right subtree
    current = current.right;
  }
  
  return result;
}

function postorderIterative(root) {
  if (!root) return [];
  
  const result = [];
  const stack = [root];
  
  // Using a modified preorder traversal (Root → Right → Left)
  // and then reversing the result to get (Left → Right → Root)
  while (stack.length > 0) {
    const node = stack.pop();
    
    result.unshift(node.val); // Add to front
    
    if (node.left) stack.push(node.left);
    if (node.right) stack.push(node.right);
  }
  
  return result;
}
```

**Pseudocode:**
```
// Recursive DFS traversals
FUNCTION PreorderTraversal(root):
    result = empty array
    
    FUNCTION DFS(node):
        IF node is null THEN RETURN
        
        APPEND node.value to result  // Visit root
        DFS(node.left)               // Visit left subtree
        DFS(node.right)              // Visit right subtree
    END FUNCTION
    
    DFS(root)
    RETURN result
END FUNCTION

FUNCTION InorderTraversal(root):
    result = empty array
    
    FUNCTION DFS(node):
        IF node is null THEN RETURN
        
        DFS(node.left)               // Visit left subtree
        APPEND node.value to result  // Visit root
        DFS(node.right)              // Visit right subtree
    END FUNCTION
    
    DFS(root)
    RETURN result
END FUNCTION

FUNCTION PostorderTraversal(root):
    result = empty array
    
    FUNCTION DFS(node):
        IF node is null THEN RETURN
        
        DFS(node.left)               // Visit left subtree
        DFS(node.right)              // Visit right subtree
        APPEND node.value to result  // Visit root
    END FUNCTION
    
    DFS(root)
    RETURN result
END FUNCTION

// BFS traversal
FUNCTION LevelOrderTraversal(root):
    IF root is null THEN RETURN empty array
    
    result = empty array
    queue = [root]
    
    WHILE queue is not empty:
        levelSize = size of queue
        currentLevel = empty array
        
        FOR i = 0 TO levelSize - 1:
            node = queue.dequeue()
            APPEND node.value to currentLevel
            
            IF node.left is not null THEN queue.enqueue(node.left)
            IF node.right is not null THEN queue.enqueue(node.right)
        END FOR
        
        APPEND currentLevel to result
    END WHILE
    
    RETURN result
END FUNCTION
```

**Time and Space Complexity:**
- Time Complexity: O(n) where n is the number of nodes (each node is visited exactly once)
- Space Complexity: 
  - O(h) for DFS recursive traversals where h is the height of the tree (recursion stack)
  - O(n) for BFS traversal in the worst case (queue size)
  - For balanced trees, space complexity is O(log n) for DFS

**Real-world Applications:**
- Expression evaluation (inorder for infix, postorder for postfix)
- Directory/file traversal
- Syntax tree processing in compilers
- HTML/XML DOM traversal
- Serialization/deserialization of tree structures
- Hierarchical data processing
- Path finding in game AI
- Evaluating arithmetic expressions

## 2. Binary Search Tree (BST) Operations

**Conceptual Explanation:**
A Binary Search Tree (BST) is a binary tree with the property that for each node:
- All nodes in the left subtree have values less than the node's value
- All nodes in the right subtree have values greater than the node's value

This property enables efficient search, insertion, and deletion operations.

**Visual Example:**
```
       8         ← Root node
     /   \
    3    10      ← Internal nodes
   / \     \
  1   6     14   ← Some leaf nodes
     / \    /
    4   7  13

This is a binary search tree where for each node:
- Left child < Parent
- Right child > Parent
```

**JavaScript Implementation:**

```javascript
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }
  
  // Search for a value
  search(val) {
    function searchNode(node, val) {
      // Base cases: node is null or node value matches
      if (!node || node.val === val) {
        return node;
      }
      
      // If value is less than node's value, search left subtree
      if (val < node.val) {
        return searchNode(node.left, val);
      }
      
      // If value is greater than node's value, search right subtree
      return searchNode(node.right, val);
    }
    
    return searchNode(this.root, val);
  }
  
  // Insert a value
  insert(val) {
    const newNode = new TreeNode(val);
    
    // If tree is empty, set new node as root
    if (!this.root) {
      this.root = newNode;
      return this;
    }
    
    function insertNode(node, newNode) {
      // If new value is less than node's value
      if (newNode.val < node.val) {
        // If no left child, add new node as left child
        if (!node.left) {
          node.left = newNode;
        } else {
          // Otherwise, recursively insert in left subtree
          insertNode(node.left, newNode);
        }
      } else {
        // If no right child, add new node as right child
        if (!node.right) {
          node.right = newNode;
        } else {
          // Otherwise, recursively insert in right subtree
          insertNode(node.right, newNode);
        }
      }
    }
    
    insertNode(this.root, newNode);
    return this;
  }
  
  // Delete a value
  delete(val) {
    // Helper function to find minimum value node
    function findMin(node) {
      while (node.left) {
        node = node.left;
      }
      return node;
    }
    
    function deleteNode(node, val) {
      // Base case: if node is null
      if (!node) return null;
      
      // If value is less than node's value, delete from left subtree
      if (val < node.val) {
        node.left = deleteNode(node.left, val);
      } 
      // If value is greater than node's value, delete from right subtree
      else if (val > node.val) {
        node.right = deleteNode(node.right, val);
      } 
      // If value equals node's value, this is the node to delete
      else {
        // Case 1: Node has no children
        if (!node.left && !node.right) {
          return null;
        }
        // Case 2: Node has only one child
        else if (!node.left) {
          return node.right;
        } 
        else if (!node.right) {
          return node.left;
        }
        // Case 3: Node has two children
        else {
          // Find the inorder successor (smallest node in right subtree)
          const successor = findMin(node.right);
          // Replace current node's value with successor's value
          node.val = successor.val;
          // Delete the successor
          node.right = deleteNode(node.right, successor.val);
        }
      }
      
      return node;
    }
    
    this.root = deleteNode(this.root, val);
    return this;
  }
  
  // Find minimum value
  findMin() {
    if (!this.root) return null;
    
    let current = this.root;
    while (current.left) {
      current = current.left;
    }
    
    return current.val;
  }
  
  // Find maximum value
  findMax() {
    if (!this.root) return null;
    
    let current = this.root;
    while (current.right) {
      current = current.right;
    }
    
    return current.val;
  }
  
  // Check if BST is valid
  isValid() {
    function validate(node, min = -Infinity, max = Infinity) {
      // Empty tree is valid
      if (!node) return true;
      
      // Check if current node's value is in valid range
      if (node.val <= min || node.val >= max) {
        return false;
      }
      
      // Recursively check left and right subtrees
      return validate(node.left, min, node.val) && 
             validate(node.right, node.val, max);
    }
    
    return validate(this.root);
  }
}
```

**Pseudocode:**
```
CLASS BinarySearchTree:
    root = null
    
    // Search for a value
    FUNCTION Search(val):
        FUNCTION SearchNode(node, val):
            IF node is null OR node.value = val THEN
                RETURN node
            END IF
            
            IF val < node.value THEN
                RETURN SearchNode(node.left, val)
            ELSE
                RETURN SearchNode(node.right, val)
            END IF
        END FUNCTION
        
        RETURN SearchNode(root, val)
    END FUNCTION
    
    // Insert a value
    FUNCTION Insert(val):
        newNode = TreeNode(val)
        
        IF root is null THEN
            root = newNode
            RETURN this
        END IF
        
        FUNCTION InsertNode(node, newNode):
            IF newNode.value < node.value THEN
                IF node.left is null THEN
                    node.left = newNode
                ELSE
                    InsertNode(node.left, newNode)
                END IF
            ELSE
                IF node.right is null THEN
                    node.right = newNode
                ELSE
                    InsertNode(node.right, newNode)
                END IF
            END IF
        END FUNCTION
        
        InsertNode(root, newNode)
        RETURN this
    END FUNCTION
    
    // Delete a value
    FUNCTION Delete(val):
        FUNCTION FindMin(node):
            WHILE node.left is not null:
                node = node.left
            END WHILE
            RETURN node
        END FUNCTION
        
        FUNCTION DeleteNode(node, val):
            IF node is null THEN RETURN null
            
            IF val < node.value THEN
                node.left = DeleteNode(node.left, val)
            ELSE IF val > node.value THEN
                node.right = DeleteNode(node.right, val)
            ELSE:
                // Case 1: No children
                IF node.left is null AND node.right is null THEN
                    RETURN null
                // Case 2: One child
                ELSE IF node.left is null THEN
                    RETURN node.right
                ELSE IF node.right is null THEN
                    RETURN node.left
                // Case 3: Two children
                ELSE
                    successor = FindMin(node.right)
                    node.value = successor.value
                    node.right = DeleteNode(node.right, successor.value)
                END IF
            END IF
            
            RETURN node
        END FUNCTION
        
        root = DeleteNode(root, val)
        RETURN this
    END FUNCTION
END CLASS
```

**Time and Space Complexity:**
- Time Complexity: 
  - Average case (balanced BST): O(log n) for search, insert, delete
  - Worst case (skewed BST): O(n) for search, insert, delete
- Space Complexity: 
  - O(h) where h is the height of the tree (recursion stack)
  - O(log n) for balanced trees, O(n) for skewed trees

**Real-world Applications:**
- Symbol tables in compilers
- Database indexing
- Priority queues
- Sorting algorithms (tree sort)
- File system directories
- Network routing tables
- Expression evaluation
- Set and map implementations

## 3. AVL Trees (Self-balancing BST)

**Conceptual Explanation:**
An AVL tree is a self-balancing binary search tree where the difference between heights of left and right subtrees (balance factor) for any node cannot be more than 1. After each insertion or deletion, the tree is rebalanced if needed through rotations.

Balance Factor = Height(Left Subtree) - Height(Right Subtree)

**Visual Example:**
```
Balanced AVL Tree:
      8
     / \
    4   12
   / \  / \
  2  6 10  14

Unbalanced after inserting 1:
      8
     / \
    4   12
   / \  / \
  2  6 10  14
 /
1

After right rotation:
      4
     / \
    2   8
   /   / \
  1   6  12
         / \
        10  14
```

**JavaScript Implementation:**

```javascript
class AVLNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
    this.height = 1; // Height of node (leaf nodes have height 1)
  }
}

class AVLTree {
  constructor() {
    this.root = null;
  }
  
  // Get height of a node
  getHeight(node) {
    return node ? node.height : 0;
  }
  
  // Get balance factor of a node
  getBalanceFactor(node) {
    if (!node) return 0;
    return this.getHeight(node.left) - this.getHeight(node.right);
  }
  
  // Update height of a node
  updateHeight(node) {
    if (!node) return;
    node.height = Math.max(this.getHeight(node.left), this.getHeight(node.right)) + 1;
  }
  
  // Right rotation
  rightRotate(y) {
    const x = y.left;
    const T3 = x.right;
    
    // Perform rotation
    x.right = y;
    y.left = T3;
    
    // Update heights
    this.updateHeight(y);
    this.updateHeight(x);
    
    // Return new root
    return x;
  }
  
  // Left rotation
  leftRotate(x) {
    const y = x.right;
    const T2 = y.left;
    
    // Perform rotation
    y.left = x;
    x.right = T2;
    
    // Update heights
    this.updateHeight(x);
    this.updateHeight(y);
    
    // Return new root
    return y;
  }
  
  // Insert a value
  insert(val) {
    this.root = this._insert(this.root, val);
    return this;
  }
  
  _insert(node, val) {
    // Standard BST insertion
    if (!node) return new AVLNode(val);
    
    if (val < node.val) {
      node.left = this._insert(node.left, val);
    } else if (val > node.val) {
      node.right = this._insert(node.right, val);
    } else {
      // Duplicate value, ignore (or handle as needed)
      return node;
    }
    
    // Update height of current node
    this.updateHeight(node);
    
    // Get balance factor and rebalance if needed
    const balance = this.getBalanceFactor(node);
    
    // Left-Left Case (Right Rotation)
    if (balance > 1 && val < node.left.val) {
      return this.rightRotate(node);
    }
    
    // Right-Right Case (Left Rotation)
    if (balance < -1 && val > node.right.val) {
      return this.leftRotate(node);
    }
    
    // Left-Right Case (Left-Right Rotation)
    if (balance > 1 && val > node.left.val) {
      node.left = this.leftRotate(node.left);
      return this.rightRotate(node);
    }
    
    // Right-Left Case (Right-Left Rotation)
    if (balance < -1 && val < node.right.val) {
      node.right = this.rightRotate(node.right);
      return this.leftRotate(node);
    }
    
    // No rotation needed
    return node;
  }
  
  // Delete a value
  delete(val) {
    this.root = this._delete(this.root, val);
    return this;
  }
  
  _delete(node, val) {
    // Standard BST deletion
    if (!node) return null;
    
    if (val < node.val) {
      node.left = this._delete(node.left, val);
    } else if (val > node.val) {
      node.right = this._delete(node.right, val);
    } else {
      // Node with only one child or no child
      if (!node.left || !node.right) {
        const temp = node.left ? node.left : node.right;
        
        // No child case
        if (!temp) {
          node = null;
        } else { // One child case
          node = temp;
        }
      } else {
        // Node with two children
        // Find inorder successor (smallest in right subtree)
        const temp = this._findMin(node.right);
        
        // Copy successor value to this node
        node.val = temp.val;
        
        // Delete the successor
        node.right = this._delete(node.right, temp.val);
      }
    }
    
    // If the tree had only one node, return
    if (!node) return null;
    
    // Update height of current node
    this.updateHeight(node);
    
    // Get balance factor and rebalance if needed
    const balance = this.getBalanceFactor(node);
    
    // Left-Left Case (Right Rotation)
    if (balance > 1 && this.getBalanceFactor(node.left) >= 0) {
      return this.rightRotate(node);
    }
    
    // Left-Right Case
    if (balance > 1 && this.getBalanceFactor(node.left) < 0) {
      node.left = this.leftRotate(node.left);
      return this.rightRotate(node);
    }
    
    // Right-Right Case
    if (balance < -1 && this.getBalanceFactor(node.right) <= 0) {
      return this.leftRotate(node);
    }
    
    // Right-Left Case
    if (balance < -1 && this.getBalanceFactor(node.right) > 0) {
      node.right = this.rightRotate(node.right);
      return this.leftRotate(node);
    }
    
    return node;
  }
  
  _findMin(node) {
    let current = node;
    while (current && current.left) {
      current = current.left;
    }
    return current;
  }
  
  // Search for a value
  search(val) {
    return this._search(this.root, val);
  }
  
  _search(node, val) {
    if (!node) return null;
    
    if (val === node.val) {
      return node;
    } else if (val < node.val) {
      return this._search(node.left, val);
    } else {
      return this._search(node.right, val);
    }
  }
}
```

**Pseudocode:**
```
CLASS AVLTree:
    root = null
    
    FUNCTION GetHeight(node):
        IF node is null THEN RETURN 0
        RETURN node.height
    END FUNCTION
    
    FUNCTION GetBalanceFactor(node):
        IF node is null THEN RETURN 0
        RETURN GetHeight(node.left) - GetHeight(node.right)
    END FUNCTION
    
    FUNCTION UpdateHeight(node):
        IF node is null THEN RETURN
        node.height = MAX(GetHeight(node.left), GetHeight(node.right)) + 1
    END FUNCTION
    
    FUNCTION RightRotate(y):
        x = y.left
        T3 = x.right
        
        // Perform rotation
        x.right = y
        y.left = T3
        
        // Update heights
        UpdateHeight(y)
        UpdateHeight(x)
        
        RETURN x  // New root
    END FUNCTION
    
    FUNCTION LeftRotate(x):
        y = x.right
        T2 = y.left
        
        // Perform rotation
        y.left = x
        x.right = T2
        
        // Update heights
        UpdateHeight(x)
        UpdateHeight(y)
        
        RETURN y  // New root
    END FUNCTION
    
    FUNCTION Insert(val):
        root = _Insert(root, val)
        RETURN this
    END FUNCTION
    
    FUNCTION _Insert(node, val):
        // Standard BST insertion
        IF node is null THEN RETURN new AVLNode(val)
        
        IF val < node.value THEN
            node.left = _Insert(node.left, val)
        ELSE IF val > node.value THEN
            node.right = _Insert(node.right, val)
        ELSE
            RETURN node  // Duplicate value
        END IF
        
        // Update height
        UpdateHeight(node)
        
        // Get balance factor
        balance = GetBalanceFactor(node)
        
        // Rebalance if needed (4 cases)
        IF balance > 1 AND val < node.left.value THEN
            RETURN RightRotate(node)  // Left-Left Case
        END IF
        
        IF balance < -1 AND val > node.right.value THEN
            RETURN LeftRotate(node)  // Right-Right Case
        END IF
        
        IF balance > 1 AND val > node.left.value THEN
            node.left = LeftRotate(node.left)  // Left-Right Case
            RETURN RightRotate(node)
        END IF
        
        IF balance < -1 AND val < node.right.value THEN
            node.right = RightRotate(node.right)  // Right-Left Case
            RETURN LeftRotate(node)
        END IF
        
        RETURN node  // No rotation needed
    END FUNCTION
END CLASS
```

**Time and Space Complexity:**
- Time Complexity: 
  - O(log n) for search, insert, delete (guaranteed due to balancing)
- Space Complexity: 
  - O(log n) for the recursion stack

**Real-world Applications:**
- Database indexing
- File systems
- Geographic information systems
- Network routing
- Online dictionaries
- Memory allocation
- Efficiently implementable priority queues
- Applications requiring guaranteed O(log n) operations

## 4. Binary Heap and Priority Queue

**Conceptual Explanation:**
A binary heap is a complete binary tree where all levels are completely filled except possibly the last level, which is filled from left to right. There are two types:
- Max Heap: Parent node values are greater than or equal to child node values
- Min Heap: Parent node values are less than or equal to child node values

A priority queue is an abstract data type where each element has a "priority" and elements with higher priority are served before elements with lower priority. Binary heaps are commonly used to implement priority queues.

**Visual Example:**
```
Max Heap:
      9
     / \
    7   8
   / \ / \
  6  5 3  2

Min Heap:
      1
     / \
    2   3
   / \ / \
  7  4 5  6
```

**JavaScript Implementation:**

```javascript
// Min Heap Implementation
class MinHeap {
  constructor() {
    this.heap = [];
  }
  
  // Helper methods to get parent and child indices
  getParentIndex(i) {
    return Math.floor((i - 1) / 2);
  }
  
  getLeftChildIndex(i) {
    return 2 * i + 1;
  }
  
  getRightChildIndex(i) {
    return 2 * i + 2;
  }
  
  // Check if node has parent or children
  hasParent(i) {
    return this.getParentIndex(i) >= 0;
  }
  
  hasLeftChild(i) {
    return this.getLeftChildIndex(i) < this.heap.length;
  }
  
  hasRightChild(i) {
    return this.getRightChildIndex(i) < this.heap.length;
  }
  
  // Get values of parent and children
  parent(i) {
    return this.heap[this.getParentIndex(i)];
  }
  
  leftChild(i) {
    return this.heap[this


**Time and Space Complexity:**
- Time Complexity: 
  - Insert (add): O(log n)
  - Delete (poll): O(log n)
  - Access min/max (peek): O(1)
- Space Complexity: O(n) for storing n elements

**Real-world Applications:**
- Task scheduling by priority
- Pathfinding algorithms (Dijkstra's, A*)
- Event-driven simulation
- Huffman coding for data compression
- Network traffic management
- Operating system process scheduling
- Database query optimization
- Job scheduling in distributed systems

## 5. Trie (Prefix Tree)

**Conceptual Explanation:**
A Trie (pronounced "try") is a tree-like data structure that efficiently stores a dynamic set of strings, with key lookup time independent of the number of keys stored. Each node in the trie represents a character, and paths from the root to a marked node represent complete words.

Tries excel at:
- Prefix matching
- Auto-completion
- Spell checking
- Dictionary implementations

**Visual Example:**
```
Trie containing "cat", "can", "car", "dog", "do":

        (root)
       /      \
      c        d
     /         |
    a          o
   / | \       |
  t  n  r      g
```

**JavaScript Implementation:**

```javascript
class TrieNode {
  constructor() {
    this.children = {}; // Maps characters to TrieNodes
    this.isEndOfWord = false; // Marks end of a word
  }
}

class Trie {
  constructor() {
    this.root = new TrieNode();
  }
  
  // Insert a word into the trie
  insert(word) {
    let current = this.root;
    
    for (const char of word) {
      // If current character doesn't exist in current node's children, add it
      if (!current.children[char]) {
        current.children[char] = new TrieNode();
      }
      // Move to the child node
      current = current.children[char];
    }
    
    // Mark the end of the word
    current.isEndOfWord = true;
  }
  
  // Search for a word in the trie
  search(word) {
    let current = this.root;
    
    for (const char of word) {
      // If character not found, word doesn't exist
      if (!current.children[char]) {
        return false;
      }
      // Move to the next node
      current = current.children[char];
    }
    
    // Word exists only if we've reached an end-of-word marker
    return current.isEndOfWord;
  }
  
  // Check if any word in the trie starts with the given prefix
  startsWith(prefix) {
    let current = this.root;
    
    for (const char of prefix) {
      // If character not found, no words with this prefix
      if (!current.children[char]) {
        return false;
      }
      // Move to the next node
      current = current.children[char];
    }
    
    // We've traversed the entire prefix, so it exists
    return true;
  }
  
  // Delete a word from the trie
  delete(word) {
    // Helper function for recursive deletion
    const deleteHelper = (current, word, depth = 0) => {
      // Base case: If we've processed all characters
      if (depth === word.length) {
        // Word exists, unmark as end of word
        if (current.isEndOfWord) {
          current.isEndOfWord = false;
        }
        
        // If this node has no children, it can be deleted
        return Object.keys(current.children).length === 0;
      }
      
      const char = word[depth];
      
      // If character doesn't exist, word not in trie
      if (!current.children[char]) {
        return false;
      }
      
      // Recursively delete from children
      const shouldDeleteCurrentNode = deleteHelper(current.children[char], word, depth + 1);
      
      // If child should be deleted and is not an end of another word
      if (shouldDeleteCurrentNode) {
        delete current.children[char];
        
        // Return true if current node can be deleted too
        return Object.keys(current.children).length === 0 && !current.isEndOfWord;
      }
      
      return false;
    };
    
    deleteHelper(this.root, word);
  }
  
  // Find all words with given prefix
  findWordsWithPrefix(prefix) {
    const result = [];
    let current = this.root;
    
    // Traverse to the node representing the prefix
    for (const char of prefix) {
      if (!current.children[char]) {
        return result; // Prefix not found
      }
      current = current.children[char];
    }
    
    // Helper function to find all words from a given node
    const findAllWords = (node, currentWord) => {
      // If we've reached the end of a word, add it to results
      if (node.isEndOfWord) {
        result.push(currentWord);
      }
      
      // Recursively explore all children
      for (const [char, childNode] of Object.entries(node.children)) {
        findAllWords(childNode, currentWord + char);
      }
    };
    
    // Find all words starting from the prefix node
    findAllWords(current, prefix);
    return result;
  }
}
```

**Pseudocode:**
```
CLASS TrieNode:
    children = empty map // Maps characters to TrieNodes
    isEndOfWord = false  // Marks end of a word
END CLASS

CLASS Trie:
    root = new TrieNode()
    
    FUNCTION Insert(word):
        current = root
        
        FOR EACH char IN word:
            IF char NOT IN current.children THEN
                current.children[char] = new TrieNode()
            END IF
            current = current.children[char]
        END FOR
        
        current.isEndOfWord = true
    END FUNCTION
    
    FUNCTION Search(word):
        current = root
        
        FOR EACH char IN word:
            IF char NOT IN current.children THEN
                RETURN false
            END IF
            current = current.children[char]
        END FOR
        
        RETURN current.isEndOfWord
    END FUNCTION
    
    FUNCTION StartsWith(prefix):
        current = root
        
        FOR EACH char IN prefix:
            IF char NOT IN current.children THEN
                RETURN false
            END IF
            current = current.children[char]
        END FOR
        
        RETURN true
    END FUNCTION
    
    FUNCTION FindWordsWithPrefix(prefix):
        result = empty array
        current = root
        
        // Navigate to prefix node
        FOR EACH char IN prefix:
            IF char NOT IN current.children THEN
                RETURN result
            END IF
            current = current.children[char]
        END FOR
        
        // DFS to find all words with prefix
        FindAllWords(current, prefix, result)
        
        RETURN result
    END FUNCTION
    
    FUNCTION FindAllWords(node, currentWord, result):
        IF node.isEndOfWord THEN
            APPEND currentWord to result
        END IF
        
        FOR EACH char, childNode IN node.children:
            FindAllWords(childNode, currentWord + char, result)
        END FOR
    END FUNCTION
END CLASS
```

**Time and Space Complexity:**
- Time Complexity:
  - Insert: O(m) where m is the length of the word
  - Search: O(m)
  - Prefix lookup: O(p) where p is the length of the prefix
  - Delete: O(m)
- Space Complexity: O(n × m) where n is the number of words and m is the average word length

**Real-world Applications:**
- Autocomplete in search engines and text editors
- Spell checkers
- IP routing (longest prefix matching)
- T9 predictive text
- Implementing dictionaries
- Word games
- Contact lists and phone directories
- DNA sequence matching

## 6. Segment Tree

**Conceptual Explanation:**
A Segment Tree is a tree data structure used for storing information about intervals or segments. It allows querying which of the stored segments contain a given point or overlap with a given interval, and efficient updates of segment values.

It's particularly useful for range queries (like finding the sum, minimum, or maximum of a range) where the data is mutable.

**Visual Example:**
```
Array: [1, 3, 5, 7, 9, 11]
Segment Tree for sum queries:

                36 (sum of entire array)
               /  \
           16      20
          /  \    /  \
         4   12  16   4
        / \  / \  / \
       1  3  5  7 9  11

Each node contains the sum of the elements in its range.
```

**JavaScript Implementation:**

```javascript
class SegmentTree {
  constructor(array, operation = (a, b) => a + b, defaultValue = 0) {
    this.array = array;
    this.operation = operation;  // Function to combine values (e.g., sum, min, max)
    this.defaultValue = defaultValue;  // Identity element for the operation
    
    // Build the segment tree
    this.tree = new Array(4 * array.length).fill(defaultValue);
    if (array.length > 0) {
      this.buildTree(0, 0, array.length - 1);
    }
  }
  
  // Build the segment tree
  buildTree(treeIndex, left, right) {
    // Leaf node (single element)
    if (left === right) {
      this.tree[treeIndex] = this.array[left];
      return this.tree[treeIndex];
    }
    
    // Internal node
    const mid = Math.floor((left + right) / 2);
    
    // Build left and right children
    const leftVal = this.buildTree(2 * treeIndex + 1, left, mid);
    const rightVal = this.buildTree(2 * treeIndex + 2, mid + 1, right);
    
    // Combine values from children
    this.tree[treeIndex] = this.operation(leftVal, rightVal);
    
    return this.tree[treeIndex];
  }
  
  // Query the segment tree for the value in range [queryLeft, queryRight]
  rangeQuery(queryLeft, queryRight) {
    // Check bounds
    if (queryLeft < 0 || queryRight >= this.array.length || queryLeft > queryRight) {
      throw new Error("Invalid range");
    }
    
    return this.query(0, 0, this.array.length - 1, queryLeft, queryRight);
  }
  
  // Helper for range query
  query(treeIndex, left, right, queryLeft, queryRight) {
    // Complete overlap
    if (queryLeft <= left && queryRight >= right) {
      return this.tree[treeIndex];
    }
    
    // No overlap
    if (queryRight < left || queryLeft > right) {
      return this.defaultValue;
    }
    
    // Partial overlap - query both children
    const mid = Math.floor((left + right) / 2);
    const leftVal = this.query(2 * treeIndex + 1, left, mid, queryLeft, queryRight);
    const rightVal = this.query(2 * treeIndex + 2, mid + 1, right, queryLeft, queryRight);
    
    return this.operation(leftVal, rightVal);
  }
  
  // Update a value in the array and the segment tree
  update(index, newValue) {
    // Check bounds
    if (index < 0 || index >= this.array.length) {
      throw new Error("Invalid index");
    }
    
    // Update array
    this.array[index] = newValue;
    
    // Update tree
    this.updateTree(0, 0, this.array.length - 1, index, newValue);
  }
  
  // Helper for update
  updateTree(treeIndex, left, right, arrayIndex, newValue) {
    // Found the leaf node for the array index
    if (left === right && left === arrayIndex) {
      this.tree[treeIndex] = newValue;
      return this.tree[treeIndex];
    }
    
    // If the array index is not in this range
    if (arrayIndex < left || arrayIndex > right) {
      return this.tree[treeIndex];
    }
    
    // Update relevant child
    const mid = Math.floor((left + right) / 2);
    
    if (arrayIndex <= mid) {
      // Update left child
      const leftVal = this.updateTree(2 * treeIndex + 1, left, mid, arrayIndex, newValue);
      const rightVal = this.tree[2 * treeIndex + 2];
      this.tree[treeIndex] = this.operation(leftVal, rightVal);
    } else {
      // Update right child
      const leftVal = this.tree[2 * treeIndex + 1];
      const rightVal = this.updateTree(2 * treeIndex + 2, mid + 1, right, arrayIndex, newValue);
      this.tree[treeIndex] = this.operation(leftVal, rightVal);
    }
    
    return this.tree[treeIndex];
  }
}

// Example usage:
// Sum segment tree
function createSumSegmentTree(array) {
  return new SegmentTree(array, (a, b) => a + b, 0);
}

// Min segment tree
function createMinSegmentTree(array) {
  return new SegmentTree(array, (a, b) => Math.min(a, b), Infinity);
}

// Max segment tree
function createMaxSegmentTree(array) {
  return new SegmentTree(array, (a, b) => Math.max(a, b), -Infinity);
}
```

**Pseudocode:**
```
CLASS SegmentTree:
    array = input array
    tree = array of size 4 * length(array)
    operation = function to combine values (e.g., sum, min, max)
    defaultValue = identity element for the operation
    
    FUNCTION BuildTree(treeIndex, left, right):
        IF left = right THEN  // Leaf node
            tree[treeIndex] = array[left]
            RETURN tree[treeIndex]
        END IF
        
        mid = floor((left + right) / 2)
        
        leftValue = BuildTree(2 * treeIndex + 1, left, mid)
        rightValue = BuildTree(2 * treeIndex + 2, mid + 1, right)
        
        tree[treeIndex] = operation(leftValue, rightValue)
        
        RETURN tree[treeIndex]
    END FUNCTION
    
    FUNCTION RangeQuery(queryLeft, queryRight):
        // Validate query range
        IF queryLeft < 0 OR queryRight >= length(array) OR queryLeft > queryRight THEN
            THROW error
        END IF
        
        RETURN Query(0, 0, length(array) - 1, queryLeft, queryRight)
    END FUNCTION
    
    FUNCTION Query(treeIndex, left, right, queryLeft, queryRight):
        // Complete overlap
        IF queryLeft <= left AND queryRight >= right THEN
            RETURN tree[treeIndex]
        END IF
        
        // No overlap
        IF queryRight < left OR queryLeft > right THEN
            RETURN defaultValue
        END IF
        
        // Partial overlap
        mid = floor((left + right) / 2)
        leftValue = Query(2 * treeIndex + 1, left, mid, queryLeft, queryRight)
        rightValue = Query(2 * treeIndex + 2, mid + 1, right, queryLeft, queryRight)
        
        RETURN operation(leftValue, rightValue)
    END FUNCTION
    
    FUNCTION Update(index, newValue):
        // Validate index
        IF index < 0 OR index >= length(array) THEN
            THROW error
        END IF
        
        array[index] = newValue
        UpdateTree(0, 0, length(array) - 1, index, newValue)
    END FUNCTION
    
    FUNCTION UpdateTree(treeIndex, left, right, arrayIndex, newValue):
        IF left = right AND left = arrayIndex THEN
            tree[treeIndex] = newValue
            RETURN tree[treeIndex]
        END IF
        
        IF arrayIndex < left OR arrayIndex > right THEN
            RETURN tree[treeIndex]
        END IF
        
        mid = floor((left + right) / 2)
        
        IF arrayIndex <= mid THEN
            leftValue = UpdateTree(2 * treeIndex + 1, left, mid, arrayIndex, newValue)
            rightValue = tree[2 * treeIndex + 2]
        ELSE
            leftValue = tree[2 * treeIndex + 1]
            rightValue = UpdateTree(2 * treeIndex + 2, mid + 1, right, arrayIndex, newValue)
        END IF
        
        tree[treeIndex] = operation(leftValue, rightValue)
        RETURN tree[treeIndex]
    END FUNCTION
END CLASS
```

**Time and Space Complexity:**
- Time Complexity:
  - Build: O(n)
  - Range Query: O(log n)
  - Update: O(log n)
- Space Complexity: O(n) for storing the segment tree

**Real-world Applications:**
- Range sum, minimum, maximum queries
- Computational geometry
- Geographic information systems
- Database query optimization
- Event scheduling systems
- Finding nearest restaurants or points of interest
- Stock market applications for range analysis
- Network traffic monitoring and analysis

## 7. Fenwick Tree (Binary Indexed Tree)

**Conceptual Explanation:**
A Fenwick Tree (or Binary Indexed Tree) is a data structure that efficiently supports prefix sum queries and point updates on an array. It uses a clever representation based on the binary representation of indices that allows operations to be performed with fewer memory accesses than a flat array.

Fenwick Trees are more space-efficient than Segment Trees for certain types of queries (especially prefix sums) but are less versatile for general range queries.

**Visual Example:**
```
Array: [3, 2, -1, 6, 5, 4, -3, 3]
Fenwick Tree for prefix sums:

Index:  0  1   2   3   4   5   6   7
Array:  3  2  -1   6   5   4  -3   3
BIT:   --  3   5   4  10   5   9   6

Each node in the BIT stores the sum of certain elements based on index bits.
For example, BIT[4] stores the sum of array[1] through array[4].
```

**JavaScript Implementation:**

```javascript
class FenwickTree {
  constructor(size) {
    // Initialize with zeros (1-indexed)
    this.tree = new Array(size + 1).fill(0);
  }
  
  // Build the Fenwick tree from an array
  buildFromArray(array) {
    // First, copy the array to the tree (1-indexed)
    for (let i = 0; i < array.length; i++) {
      this.tree[i + 1] = array[i];
    }
    
    // Then, update the parent values
    for (let i = 1; i < this.tree.length; i++) {
      const parent = i + (i & -i); // Next node that is responsible for i
      if (parent < this.tree.length) {
        this.tree[parent] += this.tree[i];
      }
    }
  }
  
  // Update the value at index by delta
  update(index, delta) {
    // Convert to 1-indexed
    index += 1;
    
    // Update all responsible nodes
    while (index < this.tree.length) {
      this.tree[index] += delta;
      index += index & -index; // Next node responsible for current node
    }
  }
  
  // Get prefix sum up to index (inclusive)
  prefixSum(index) {
    // Convert to 1-indexed and check bounds
    index = Math.min(index + 1, this.tree.length - 1);
    
    let sum = 0;
    
    // Traverse all responsible nodes
    while (index > 0) {
      sum += this.tree[index];
      index -= index & -index; // Previous node the current node is responsible for
    }
    
    return sum;
  }
  
  // Get the sum for range [left, right] (inclusive)
  rangeSum(left, right) {
    if (left > right || left < 0 || right >= this.tree.length - 1) {
      throw new Error("Invalid range");
    }
    
    // Range sum is prefixSum(right) - prefixSum(left-1)
    return this.prefixSum(right) - (left > 0 ? this.prefixSum(left - 1) : 0);
  }
  
  // Get the original array value at index
  get(index) {
    return this.rangeSum(index, index);
  }
  
  // Set the value at index
  set(index, value) {
    const delta = value - this.get(index);
    this.update(index, delta);
  }
}
```

**Pseudocode:**
```
CLASS FenwickTree:
    tree = array of size n+1 (1-indexed)
    
    FUNCTION BuildFromArray(array):
        // Copy array to tree (1-indexed)
        FOR i = 0 TO length(array) - 1:
            tree[i + 1] = array[i]
        END FOR
        
        // Update parent values
        FOR i = 1 TO length(tree) - 1:
            parent = i + (i & -i)  // Next node responsible for i
            IF parent < length# Tree-Based Algorithms

## Basic Concept

**Conceptual Explanation:**
Tree-based algorithms operate on hierarchical tree data structures where nodes are connected without cycles. These algorithms are essential for efficiently solving a wide range of problems from search operations to hierarchical data processing. Trees represent data in a parent-child relationship, and algorithms exploit this structure for efficient traversal, search, insertion, and deletion.

A tree consists of:
- Nodes: Elements containing data
- Edges: Connections between nodes
- Root: The topmost node
- Parent/Child relationships: Hierarchical connections
- Leaf nodes: Nodes without children

**Visual Example:**
```
       A         ← Root node
     /   \
    B     C      ← Internal nodes
   / \   / \
  D   E F   G    ← Leaf nodes

This is a binary tree where each node has at most 2 children.
```

**JavaScript Implementation (Basic Tree Node):**
```javascript
// Basic tree node
class TreeNode {
  constructor(val) {
    this.val = val;
    this.children = []; // For n-ary tree
  }
}

// Binary tree node
class BinaryTreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;  // Left child
    this.right = null; // Right child
  }
}
```

**Pseudocode:**
```
CLASS TreeNode:
    value
    children (array or list)
    
    CONSTRUCTOR(value):
        this.value = value
        this.children = empty array or list
    
    FUNCTION addChild(child):
        append child to this.children
END CLASS
```

**Time and Space Complexity:**
- Time and space complexity varies by specific tree algorithm
- Many tree operations run in O(h) time, where h is the height of the tree
- For balanced trees, h is approximately log(n), making these operations O(log n)
- For degenerate/skewed trees, h can be n, making operations O(n) in the worst case

**Real-world Applications:**
- File systems organization (directories and files)
- HTML/XML/JSON document object models
- Network routing algorithms
- Decision-making systems
- Database indexing
- Organization hierarchies
- Game AI (minimax, decision trees)
- Compression algorithms (Huffman coding)

## 1. Binary Tree Traversal Algorithms

**Conceptual Explanation:**
Tree traversal is the process of visiting every node in a tree exactly once. There are several common traversal methods, each with different orders of visiting nodes:

1. **Depth-First Search (DFS)**
   - Preorder (Root → Left → Right)
   - Inorder (Left → Root → Right)
   - Postorder (Left → Right → Root)

2. **Breadth-First Search (BFS)**
   - Level Order (visit nodes level by level)

**Visual Example:**
```
     1
   /   \
  2     3
 / \   / \
4   5 6   7

Preorder: 1, 2, 4, 5, 3, 6, 7
Inorder: 4, 2, 5, 1, 6, 3, 7
Postorder: 4, 5, 2, 6, 7, 3, 1
Level Order: 1, 2, 3, 4, 5, 6, 7
```

**JavaScript Implementation:**

```javascript
// Binary tree node
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

// DFS - Preorder Traversal (Root → Left → Right)
function preorderTraversal(root) {
  const result = [];
  
  function dfs(node) {
    if (!node) return;
    
    // Visit root
    result.push(node.val);
    
    // Visit left subtree
    dfs(node.left);
    
    // Visit right subtree
    dfs(node.right);
  }
  
  dfs(root);
  return result;
}

// DFS - Inorder Traversal (Left → Root → Right)
function inorderTraversal(root) {
  const result = [];
  
  function dfs(node) {
    if (!node) return;
    
    // Visit left subtree
    dfs(node.left);
    
    // Visit root
    result.push(node.val);
    
    // Visit right subtree
    dfs(node.right);
  }
  
  dfs(root);
  return result;
}

// DFS - Postorder Traversal (Left → Right → Root)
function postorderTraversal(root) {
  const result = [];
  
  function dfs(node) {
    if (!node) return;
    
    // Visit left subtree
    dfs(node.left);
    
    // Visit right subtree
    dfs(node.right);
    
    // Visit root
    result.push(node.val);
  }
  
  dfs(root);
  return result;
}

// BFS - Level Order Traversal
function levelOrderTraversal(root) {
  if (!root) return [];
  
  const result = [];
  const queue = [root];
  
  while (queue.length > 0) {
    const levelSize = queue.length;
    const currentLevel = [];
    
    for (let i = 0; i < levelSize; i++) {
      const node = queue.shift();
      
      // Visit the current node
      currentLevel.push(node.val);
      
      // Add children to the queue
      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
    
    result.push(currentLevel);
  }
  
  return result;
}

// Iterative versions of DFS traversals
function preorderIterative(root) {
  if (!root) return [];
  
  const result = [];
  const stack = [root];
  
  while (stack.length > 0) {
    const node = stack.pop();
    
    result.push(node.val);
    
    // Push right first so that left is processed first (LIFO)
    if (node.right) stack.push(node.right);
    if (node.left) stack.push(node.left);
  }
  
  return result;
}

function inorderIterative(root) {
  if (!root) return [];
  
  const result = [];
  const stack = [];
  let current = root;
  
  while (current || stack.length > 0) {
    // Reach the leftmost node
    while (current) {
      stack.push(current);
      current = current.left;
    }
    
    // Process the current node
    current = stack.pop();
    result.push(current.val);
    
    // Move to the right subtree
    current = current.right;
  }
  
  return result;
}

function postorderIterative(root) {
  if (!root) return [];
  
  const result = [];
  const stack = [root];
  
  // Using a modified preorder traversal (Root → Right → Left)
  // and then reversing the result to get (Left → Right → Root)
  while (stack.length > 0) {
    const node = stack.pop();
    
    result.unshift(node.val); // Add to front
    
    if (node.left) stack.push(node.left);
    if (node.right) stack.push(node.right);
  }
  
  return result;
}
```

**Pseudocode:**
```
// Recursive DFS traversals
FUNCTION PreorderTraversal(root):
    result = empty array
    
    FUNCTION DFS(node):
        IF node is null THEN RETURN
        
        APPEND node.value to result  // Visit root
        DFS(node.left)               // Visit left subtree
        DFS(node.right)              // Visit right subtree
    END FUNCTION
    
    DFS(root)
    RETURN result
END FUNCTION

FUNCTION InorderTraversal(root):
    result = empty array
    
    FUNCTION DFS(node):
        IF node is null THEN RETURN
        
        DFS(node.left)               // Visit left subtree
        APPEND node.value to result  // Visit root
        DFS(node.right)              // Visit right subtree
    END FUNCTION
    
    DFS(root)
    RETURN result
END FUNCTION

FUNCTION PostorderTraversal(root):
    result = empty array
    
    FUNCTION DFS(node):
        IF node is null THEN RETURN
        
        DFS(node.left)               // Visit left subtree
        DFS(node.right)              // Visit right subtree
        APPEND node.value to result  // Visit root
    END FUNCTION
    
    DFS(root)
    RETURN result
END FUNCTION

// BFS traversal
FUNCTION LevelOrderTraversal(root):
    IF root is null THEN RETURN empty array
    
    result = empty array
    queue = [root]
    
    WHILE queue is not empty:
        levelSize = size of queue
        currentLevel = empty array
        
        FOR i = 0 TO levelSize - 1:
            node = queue.dequeue()
            APPEND node.value to currentLevel
            
            IF node.left is not null THEN queue.enqueue(node.left)
            IF node.right is not null THEN queue.enqueue(node.right)
        END FOR
        
        APPEND currentLevel to result
    END WHILE
    
    RETURN result
END FUNCTION
```

**Time and Space Complexity:**
- Time Complexity: O(n) where n is the number of nodes (each node is visited exactly once)
- Space Complexity: 
  - O(h) for DFS recursive traversals where h is the height of the tree (recursion stack)
  - O(n) for BFS traversal in the worst case (queue size)
  - For balanced trees, space complexity is O(log n) for DFS

**Real-world Applications:**
- Expression evaluation (inorder for infix, postorder for postfix)
- Directory/file traversal
- Syntax tree processing in compilers
- HTML/XML DOM traversal
- Serialization/deserialization of tree structures
- Hierarchical data processing
- Path finding in game AI
- Evaluating arithmetic expressions

## 2. Binary Search Tree (BST) Operations

**Conceptual Explanation:**
A Binary Search Tree (BST) is a binary tree with the property that for each node:
- All nodes in the left subtree have values less than the node's value
- All nodes in the right subtree have values greater than the node's value

This property enables efficient search, insertion, and deletion operations.

**Visual Example:**
```
       8         ← Root node
     /   \
    3    10      ← Internal nodes
   / \     \
  1   6     14   ← Some leaf nodes
     / \    /
    4   7  13

This is a binary search tree where for each node:
- Left child < Parent
- Right child > Parent
```

**JavaScript Implementation:**

```javascript
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }
  
  // Search for a value
  search(val) {
    function searchNode(node, val) {
      // Base cases: node is null or node value matches
      if (!node || node.val === val) {
        return node;
      }
      
      // If value is less than node's value, search left subtree
      if (val < node.val) {
        return searchNode(node.left, val);
      }
      
      // If value is greater than node's value, search right subtree
      return searchNode(node.right, val);
    }
    
    return searchNode(this.root, val);
  }
  
  // Insert a value
  insert(val) {
    const newNode = new TreeNode(val);
    
    // If tree is empty, set new node as root
    if (!this.root) {
      this.root = newNode;
      return this;
    }
    
    function insertNode(node, newNode) {
      // If new value is less than node's value
      if (newNode.val < node.val) {
        // If no left child, add new node as left child
        if (!node.left) {
          node.left = newNode;
        } else {
          // Otherwise, recursively insert in left subtree
          insertNode(node.left, newNode);
        }
      } else {
        // If no right child, add new node as right child
        if (!node.right) {
          node.right = newNode;
        } else {
          // Otherwise, recursively insert in right subtree
          insertNode(node.right, newNode);
        }
      }
    }
    
    insertNode(this.root, newNode);
    return this;
  }
  
  // Delete a value
  delete(val) {
    // Helper function to find minimum value node
    function findMin(node) {
      while (node.left) {
        node = node.left;
      }
      return node;
    }
    
    function deleteNode(node, val) {
      // Base case: if node is null
      if (!node) return null;
      
      // If value is less than node's value, delete from left subtree
      if (val < node.val) {
        node.left = deleteNode(node.left, val);
      } 
      // If value is greater than node's value, delete from right subtree
      else if (val > node.val) {
        node.right = deleteNode(node.right, val);
      } 
      // If value equals node's value, this is the node to delete
      else {
        // Case 1: Node has no children
        if (!node.left && !node.right) {
          return null;
        }
        // Case 2: Node has only one child
        else if (!node.left) {
          return node.right;
        } 
        else if (!node.right) {
          return node.left;
        }
        // Case 3: Node has two children
        else {
          // Find the inorder successor (smallest node in right subtree)
          const successor = findMin(node.right);
          // Replace current node's value with successor's value
          node.val = successor.val;
          // Delete the successor
          node.right = deleteNode(node.right, successor.val);
        }
      }
      
      return node;
    }
    
    this.root = deleteNode(this.root, val);
    return this;
  }
  
  // Find minimum value
  findMin() {
    if (!this.root) return null;
    
    let current = this.root;
    while (current.left) {
      current = current.left;
    }
    
    return current.val;
  }
  
  // Find maximum value
  findMax() {
    if (!this.root) return null;
    
    let current = this.root;
    while (current.right) {
      current = current.right;
    }
    
    return current.val;
  }
  
  // Check if BST is valid
  isValid() {
    function validate(node, min = -Infinity, max = Infinity) {
      // Empty tree is valid
      if (!node) return true;
      
      // Check if current node's value is in valid range
      if (node.val <= min || node.val >= max) {
        return false;
      }
      
      // Recursively check left and right subtrees
      return validate(node.left, min, node.val) && 
             validate(node.right, node.val, max);
    }
    
    return validate(this.root);
  }
}
```

**Pseudocode:**
```
CLASS BinarySearchTree:
    root = null
    
    // Search for a value
    FUNCTION Search(val):
        FUNCTION SearchNode(node, val):
            IF node is null OR node.value = val THEN
                RETURN node
            END IF
            
            IF val < node.value THEN
                RETURN SearchNode(node.left, val)
            ELSE
                RETURN SearchNode(node.right, val)
            END IF
        END FUNCTION
        
        RETURN SearchNode(root, val)
    END FUNCTION
    
    // Insert a value
    FUNCTION Insert(val):
        newNode = TreeNode(val)
        
        IF root is null THEN
            root = newNode
            RETURN this
        END IF
        
        FUNCTION InsertNode(node, newNode):
            IF newNode.value < node.value THEN
                IF node.left is null THEN
                    node.left = newNode
                ELSE
                    InsertNode(node.left, newNode)
                END IF
            ELSE
                IF node.right is null THEN
                    node.right = newNode
                ELSE
                    InsertNode(node.right, newNode)
                END IF
            END IF
        END FUNCTION
        
        InsertNode(root, newNode)
        RETURN this
    END FUNCTION
    
    // Delete a value
    FUNCTION Delete(val):
        FUNCTION FindMin(node):
            WHILE node.left is not null:
                node = node.left
            END WHILE
            RETURN node
        END FUNCTION
        
        FUNCTION DeleteNode(node, val):
            IF node is null THEN RETURN null
            
            IF val < node.value THEN
                node.left = DeleteNode(node.left, val)
            ELSE IF val > node.value THEN
                node.right = DeleteNode(node.right, val)
            ELSE:
                // Case 1: No children
                IF node.left is null AND node.right is null THEN
                    RETURN null
                // Case 2: One child
                ELSE IF node.left is null THEN
                    RETURN node.right
                ELSE IF node.right is null THEN
                    RETURN node.left
                // Case 3: Two children
                ELSE
                    successor = FindMin(node.right)
                    node.value = successor.value
                    node.right = DeleteNode(node.right, successor.value)
                END IF
            END IF
            
            RETURN node
        END FUNCTION
        
        root = DeleteNode(root, val)
        RETURN this
    END FUNCTION
END CLASS
```

**Time and Space Complexity:**
- Time Complexity: 
  - Average case (balanced BST): O(log n) for search, insert, delete
  - Worst case (skewed BST): O(n) for search, insert, delete
- Space Complexity: 
  - O(h) where h is the height of the tree (recursion stack)
  - O(log n) for balanced trees, O(n) for skewed trees

**Real-world Applications:**
- Symbol tables in compilers
- Database indexing
- Priority queues
- Sorting algorithms (tree sort)
- File system directories
- Network routing tables
- Expression evaluation
- Set and map implementations

## 3. AVL Trees (Self-balancing BST)

**Conceptual Explanation:**
An AVL tree is a self-balancing binary search tree where the difference between heights of left and right subtrees (balance factor) for any node cannot be more than 1. After each insertion or deletion, the tree is rebalanced if needed through rotations.

Balance Factor = Height(Left Subtree) - Height(Right Subtree)

**Visual Example:**
```
Balanced AVL Tree:
      8
     / \
    4   12
   / \  / \
  2  6 10  14

Unbalanced after inserting 1:
      8
     / \
    4   12
   / \  / \
  2  6 10  14
 /
1

After right rotation:
      4
     / \
    2   8
   /   / \
  1   6  12
         / \
        10  14
```

**JavaScript Implementation:**

```javascript
class AVLNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
    this.height = 1; // Height of node (leaf nodes have height 1)
  }
}

class AVLTree {
  constructor() {
    this.root = null;
  }
  
  // Get height of a node
  getHeight(node) {
    return node ? node.height : 0;
  }
  
  // Get balance factor of a node
  getBalanceFactor(node) {
    if (!node) return 0;
    return this.getHeight(node.left) - this.getHeight(node.right);
  }
  
  // Update height of a node
  updateHeight(node) {
    if (!node) return;
    node.height = Math.max(this.getHeight(node.left), this.getHeight(node.right)) + 1;
  }
  
  // Right rotation
  rightRotate(y) {
    const x = y.left;
    const T3 = x.right;
    
    // Perform rotation
    x.right = y;
    y.left = T3;
    
    // Update heights
    this.updateHeight(y);
    this.updateHeight(x);
    
    // Return new root
    return x;
  }
  
  // Left rotation
  leftRotate(x) {
    const y = x.right;
    const T2 = y.left;
    
    // Perform rotation
    y.left = x;
    x.right = T2;
    
    // Update heights
    this.updateHeight(x);
    this.updateHeight(y);
    
    // Return new root
    return y;
  }
  
  // Insert a value
  insert(val) {
    this.root = this._insert(this.root, val);
    return this;
  }
  
  _insert(node, val) {
    // Standard BST insertion
    if (!node) return new AVLNode(val);
    
    if (val < node.val) {
      node.left = this._insert(node.left, val);
    } else if (val > node.val) {
      node.right = this._insert(node.right, val);
    } else {
      // Duplicate value, ignore (or handle as needed)
      return node;
    }
    
    // Update height of current node
    this.updateHeight(node);
    
    // Get balance factor and rebalance if needed
    const balance = this.getBalanceFactor(node);
    
    // Left-Left Case (Right Rotation)
    if (balance > 1 && val < node.left.val) {
      return this.rightRotate(node);
    }
    
    // Right-Right Case (Left Rotation)
    if (balance < -1 && val > node.right.val) {
      return this.leftRotate(node);
    }
    
    // Left-Right Case (Left-Right Rotation)
    if (balance > 1 && val > node.left.val) {
      node.left = this.leftRotate(node.left);
      return this.rightRotate(node);
    }
    
    // Right-Left Case (Right-Left Rotation)
    if (balance < -1 && val < node.right.val) {
      node.right = this.rightRotate(node.right);
      return this.leftRotate(node);
    }
    
    // No rotation needed
    return node;
  }
  
  // Delete a value
  delete(val) {
    this.root = this._delete(this.root, val);
    return this;
  }
  
  _delete(node, val) {
    // Standard BST deletion
    if (!node) return null;
    
    if (val < node.val) {
      node.left = this._delete(node.left, val);
    } else if (val > node.val) {
      node.right = this._delete(node.right, val);
    } else {
      // Node with only one child or no child
      if (!node.left || !node.right) {
        const temp = node.left ? node.left : node.right;
        
        // No child case
        if (!temp) {
          node = null;
        } else { // One child case
          node = temp;
        }
      } else {
        // Node with two children
        // Find inorder successor (smallest in right subtree)
        const temp = this._findMin(node.right);
        
        // Copy successor value to this node
        node.val = temp.val;
        
        // Delete the successor
        node.right = this._delete(node.right, temp.val);
      }
    }
    
    // If the tree had only one node, return
    if (!node) return null;
    
    // Update height of current node
    this.updateHeight(node);
    
    // Get balance factor and rebalance if needed
    const balance = this.getBalanceFactor(node);
    
    // Left-Left Case (Right Rotation)
    if (balance > 1 && this.getBalanceFactor(node.left) >= 0) {
      return this.rightRotate(node);
    }
    
    // Left-Right Case
    if (balance > 1 && this.getBalanceFactor(node.left) < 0) {
      node.left = this.leftRotate(node.left);
      return this.rightRotate(node);
    }
    
    // Right-Right Case
    if (balance < -1 && this.getBalanceFactor(node.right) <= 0) {
      return this.leftRotate(node);
    }
    
    // Right-Left Case
    if (balance < -1 && this.getBalanceFactor(node.right) > 0) {
      node.right = this.rightRotate(node.right);
      return this.leftRotate(node);
    }
    
    return node;
  }
  
  _findMin(node) {
    let current = node;
    while (current && current.left) {
      current = current.left;
    }
    return current;
  }
  
  // Search for a value
  search(val) {
    return this._search(this.root, val);
  }
  
  _search(node, val) {
    if (!node) return null;
    
    if (val === node.val) {
      return node;
    } else if (val < node.val) {
      return this._search(node.left, val);
    } else {
      return this._search(node.right, val);
    }
  }
}
```

**Pseudocode:**
```
CLASS AVLTree:
    root = null
    
    FUNCTION GetHeight(node):
        IF node is null THEN RETURN 0
        RETURN node.height
    END FUNCTION
    
    FUNCTION GetBalanceFactor(node):
        IF node is null THEN RETURN 0
        RETURN GetHeight(node.left) - GetHeight(node.right)
    END FUNCTION
    
    FUNCTION UpdateHeight(node):
        IF node is null THEN RETURN
        node.height = MAX(GetHeight(node.left), GetHeight(node.right)) + 1
    END FUNCTION
    
    FUNCTION RightRotate(y):
        x = y.left
        T3 = x.right
        
        // Perform rotation
        x.right = y
        y.left = T3
        
        // Update heights
        UpdateHeight(y)
        UpdateHeight(x)
        
        RETURN x  // New root
    END FUNCTION
    
    FUNCTION LeftRotate(x):
        y = x.right
        T2 = y.left
        
        // Perform rotation
        y.left = x
        x.right = T2
        
        // Update heights
        UpdateHeight(x)
        UpdateHeight(y)
        
        RETURN y  // New root
    END FUNCTION
    
    FUNCTION Insert(val):
        root = _Insert(root, val)
        RETURN this
    END FUNCTION
    
    FUNCTION _Insert(node, val):
        // Standard BST insertion
        IF node is null THEN RETURN new AVLNode(val)
        
        IF val < node.value THEN
            node.left = _Insert(node.left, val)
        ELSE IF val > node.value THEN
            node.right = _Insert(node.right, val)
        ELSE
            RETURN node  // Duplicate value
        END IF
        
        // Update height
        UpdateHeight(node)
        
        // Get balance factor
        balance = GetBalanceFactor(node)
        
        // Rebalance if needed (4 cases)
        IF balance > 1 AND val < node.left.value THEN
            RETURN RightRotate(node)  // Left-Left Case
        END IF
        
        IF balance < -1 AND val > node.right.value THEN
            RETURN LeftRotate(node)  // Right-Right Case
        END IF
        
        IF balance > 1 AND val > node.left.value THEN
            node.left = LeftRotate(node.left)  // Left-Right Case
            RETURN RightRotate(node)
        END IF
        
        IF balance < -1 AND val < node.right.value THEN
            node.right = RightRotate(node.right)  // Right-Left Case
            RETURN LeftRotate(node)
        END IF
        
        RETURN node  // No rotation needed
    END FUNCTION
END CLASS
```

**Time and Space Complexity:**
- Time Complexity: 
  - O(log n) for search, insert, delete (guaranteed due to balancing)
- Space Complexity: 
  - O(log n) for the recursion stack

**Real-world Applications:**
- Database indexing
- File systems
- Geographic information systems
- Network routing
- Online dictionaries
- Memory allocation
- Efficiently implementable priority queues
- Applications requiring guaranteed O(log n) operations

## 4. Binary Heap and Priority Queue

**Conceptual Explanation:**
A binary heap is a complete binary tree where all levels are completely filled except possibly the last level, which is filled from left to right. There are two types:
- Max Heap: Parent node values are greater than or equal to child node values
- Min Heap: Parent node values are less than or equal to child node values

A priority queue is an abstract data type where each element has a "priority" and elements with higher priority are served before elements with lower priority. Binary heaps are commonly used to implement priority queues.

**Visual Example:**
```
Max Heap:
      9
     / \
    7   8
   / \ / \
  6  5 3  2

Min Heap:
      1
     / \
    2   3
   / \ / \
  7  4 5  6
```

**JavaScript Implementation:**

```javascript
// Min Heap Implementation
class MinHeap {
  constructor() {
    this.heap = [];
  }
  
  // Helper methods to get parent and child indices
  getParentIndex(i) {
    return Math.floor((i - 1) / 2);
  }
  
  getLeftChildIndex(i) {
    return 2 * i + 1;
  }
  
  getRightChildIndex(i) {
    return 2 * i + 2;
  }
  
  // Check if node has parent or children
  hasParent(i) {
    return this.getParentIndex(i) >= 0;
  }
  
  hasLeftChild(i) {
    return this.getLeftChildIndex(i) < this.heap.length;
  }
  
  hasRightChild(i) {
    return this.getRightChildIndex(i) < this.heap.length;
  }
  
  // Get values of parent and children
  parent(i) {
    return this.heap[this.getParentIndex(i)];
  }
  
  leftChild(i) {
    return this.heap[this.getLeftChildIndex(i)];
  }
  
  rightChild(i) {
    return this.heap[this.getRightChildIndex(i)];
  }
  
  // Swap two elements in the heap
  swap(i, j) {
    [this.heap[i], this.heap[j]] = [this.heap[j], this.heap[i]];
  }
  
  // Return the minimum element without removing it
  peek() {
    if (this.heap.length === 0) return null;
    return this.heap[0];
  }
  
  // Remove and return the minimum element
  poll() {
    if (this.heap.length === 0) return null;
    
    const item = this.heap[0];
    const lastItem = this.heap.pop();
    
    if (this.heap.length > 0) {
      this.heap[0] = lastItem;
      this.heapifyDown();
    }
    
    return item;
  }
  
  // Add an element to the heap
  add(item) {
    this.heap.push(item);
    this.heapifyUp();
    return this;
  }
  
  // Restore heap property going up
  heapifyUp() {
    let index = this.heap.length - 1;
    
    while (this.hasParent(index) && this.parent(index) > this.heap[index]) {
      const parentIndex = this.getParentIndex(index);
      this.swap(parentIndex, index);
      index = parentIndex;
    }
  }
  
  // Restore heap property going down
  heapifyDown() {
    let index = 0;
    
    while (this.hasLeftChild(index)) {
      let smallerChildIndex = this.getLeftChildIndex(index);
      
      if (this.hasRightChild(index) && this.rightChild(index) < this.leftChild(index)) {
        smallerChildIndex = this.getRightChildIndex(index);
      }
      
      if (this.heap[index] < this.heap[smallerChildIndex]) {
        break;
      } else {
        this.swap(index, smallerChildIndex);
      }
      
      index = smallerChildIndex;
    }
  }
  
  // Check if heap is empty
  isEmpty() {
    return this.heap.length === 0;
  }
  
  // Get size of heap
  size() {
    return this.heap.length;
  }
}

// Priority Queue implementation using Min Heap
class PriorityQueue {
  constructor() {
    this.minHeap = new MinHeap();
  }
  
  // Add element with priority
  enqueue(item, priority) {
    this.minHeap.add({ item, priority });
    return this;
  }
  
  // Remove and return element with highest priority (lowest value)
  dequeue() {
    if (this.isEmpty()) return null;
    
    const heapNode = this.minHeap.poll();
    return heapNode.item;
  }
  
  // Return highest priority element without removing
  front() {
    if (this.isEmpty()) return null;
    
    return this.minHeap.peek().item;
  }
  
  // Check if priority queue is empty
  isEmpty() {
    return this.minHeap.isEmpty();
  }
  
  // Get size of priority queue
  size() {
    return this.minHeap.size();
  }
}