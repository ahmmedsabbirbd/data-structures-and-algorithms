# Graph-Based Algorithms

## Basic Concept

**Conceptual Explanation:**
A graph is a non-linear data structure consisting of nodes (vertices) and edges that connect these nodes. Graphs are used to represent networks of relationships between objects. Graph algorithms are techniques used to solve problems on these graph structures, such as finding paths, determining connectivity, or analyzing network flow.

Types of graphs:
1. **Directed vs. Undirected**: In directed graphs, edges have direction, while in undirected graphs, they don't.
2. **Weighted vs. Unweighted**: Weighted graphs have values assigned to edges, while unweighted graphs don't.
3. **Cyclic vs. Acyclic**: Cyclic graphs contain at least one cycle, while acyclic graphs don't.
4. **Connected vs. Disconnected**: Connected graphs have a path between every pair of vertices, while disconnected graphs don't.

**Visual Example:**
```
Undirected Graph:
    A --- B
    |     |
    |     |
    C --- D

Directed Graph:
    A --→ B
    ↑     ↓
    |     |
    D ←-- C

Weighted Graph:
    A ---5--- B
    |         |
    2         3
    |         |
    C ---1--- D
```

**JavaScript Implementation (Graph Representation):**

```javascript
// Adjacency List representation
class Graph {
  constructor(isDirected = false) {
    this.adjacencyList = {};
    this.isDirected = isDirected;
  }
  
  // Add a vertex to the graph
  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) {
      this.adjacencyList[vertex] = [];
    }
    return this;
  }
  
  // Add an edge between two vertices
  addEdge(v1, v2, weight = 1) {
    if (!this.adjacencyList[v1]) this.addVertex(v1);
    if (!this.adjacencyList[v2]) this.addVertex(v2);
    
    this.adjacencyList[v1].push({ node: v2, weight });
    
    if (!this.isDirected) {
      this.adjacencyList[v2].push({ node: v1, weight });
    }
    
    return this;
  }
  
  // Remove an edge between two vertices
  removeEdge(v1, v2) {
    this.adjacencyList[v1] = this.adjacencyList[v1].filter(
      edge => edge.node !== v2
    );
    
    if (!this.isDirected) {
      this.adjacencyList[v2] = this.adjacencyList[v2].filter(
        edge => edge.node !== v1
      );
    }
    
    return this;
  }
  
  // Remove a vertex and all its connections
  removeVertex(vertex) {
    while (this.adjacencyList[vertex] && this.adjacencyList[vertex].length) {
      const adjacentVertex = this.adjacencyList[vertex].pop().node;
      this.removeEdge(vertex, adjacentVertex);
    }
    
    delete this.adjacencyList[vertex];
    return this;
  }
  
  // Get all vertices
  getVertices() {
    return Object.keys(this.adjacencyList);
  }
  
  // Get neighbors of a vertex
  getNeighbors(vertex) {
    return this.adjacencyList[vertex] || [];
  }
}

// Adjacency Matrix representation
class GraphMatrix {
  constructor(size, isDirected = false) {
    this.matrix = Array(size).fill().map(() => Array(size).fill(0));
    this.vertices = {};
    this.vertexCount = 0;
    this.isDirected = isDirected;
  }
  
  // Add a vertex
  addVertex(vertex) {
    if (!(vertex in this.vertices)) {
      this.vertices[vertex] = this.vertexCount++;
    }
    return this;
  }
  
  // Add an edge with optional weight
  addEdge(v1, v2, weight = 1) {
    this.addVertex(v1);
    this.addVertex(v2);
    
    const i = this.vertices[v1];
    const j = this.vertices[v2];
    
    this.matrix[i][j] = weight;
    
    if (!this.isDirected) {
      this.matrix[j][i] = weight;
    }
    
    return this;
  }
  
  // Remove an edge
  removeEdge(v1, v2) {
    const i = this.vertices[v1];
    const j = this.vertices[v2];
    
    if (i !== undefined && j !== undefined) {
      this.matrix[i][j] = 0;
      
      if (!this.isDirected) {
        this.matrix[j][i] = 0;
      }
    }
    
    return this;
  }
  
  // Check if an edge exists
  hasEdge(v1, v2) {
    const i = this.vertices[v1];
    const j = this.vertices[v2];
    
    if (i !== undefined && j !== undefined) {
      return this.matrix[i][j] !== 0;
    }
    
    return false;
  }
  
  // Get edge weight
  getEdgeWeight(v1, v2) {
    const i = this.vertices[v1];
    const j = this.vertices[v2];
    
    if (i !== undefined && j !== undefined) {
      return this.matrix[i][j];
    }
    
    return 0;
  }
}
```

**Pseudocode:**
```
CLASS Graph:
    adjacencyList = empty map
    isDirected = false
    
    FUNCTION AddVertex(vertex):
        IF vertex not in adjacencyList THEN
            adjacencyList[vertex] = empty array
        END IF
        RETURN this
    END FUNCTION
    
    FUNCTION AddEdge(v1, v2, weight = 1):
        IF v1 not in adjacencyList THEN AddVertex(v1)
        IF v2 not in adjacencyList THEN AddVertex(v2)
        
        APPEND {node: v2, weight} to adjacencyList[v1]
        
        IF not isDirected THEN
            APPEND {node: v1, weight} to adjacencyList[v2]
        END IF
        
        RETURN this
    END FUNCTION
    
    FUNCTION RemoveEdge(v1, v2):
        adjacencyList[v1] = FILTER adjacencyList[v1] WHERE node != v2
        
        IF not isDirected THEN
            adjacencyList[v2] = FILTER adjacencyList[v2] WHERE node != v1
        END IF
        
        RETURN this
    END FUNCTION
    
    FUNCTION RemoveVertex(vertex):
        WHILE adjacencyList[vertex] is not empty:
            adjacentVertex = REMOVE last element from adjacencyList[vertex]
            RemoveEdge(vertex, adjacentVertex)
        END WHILE
        
        DELETE adjacencyList[vertex]
        RETURN this
    END FUNCTION
END CLASS
```

**Time and Space Complexity:**
- Adjacency List:
  - Space Complexity: O(V + E) where V is the number of vertices and E is the number of edges
  - Time Complexity:
    - Add Vertex: O(1)
    - Add Edge: O(1)
    - Remove Edge: O(E) in worst case
    - Remove Vertex: O(V + E) in worst case
    - Query (checking if two vertices are connected): O(V) in worst case

- Adjacency Matrix:
  - Space Complexity: O(V²) regardless of the number of edges
  - Time Complexity:
    - Add Vertex: O(V²) (may need to resize matrix)
    - Add Edge: O(1)
    - Remove Edge: O(1)
    - Remove Vertex: O(V²) (need to reshape matrix)
    - Query (checking if two vertices are connected): O(1)

**Real-world Applications:**
- Social networks (friend relationships)
- Road networks and navigation systems
- Internet routing protocols
- Telecommunications networks
- Recommendation systems
- Dependency resolution in package managers
- Analyzing biological networks
- Scheduling problems

## 1. Depth-First Search (DFS)

**Conceptual Explanation:**
Depth-First Search is a graph traversal algorithm that explores as far as possible along each branch before backtracking. It uses a stack (either explicitly or implicitly through recursion) to keep track of vertices to be explored. DFS can be used to:
- Detect cycles in a graph
- Find paths between two vertices
- Topologically sort vertices
- Solve maze puzzles
- Find connected components

**Visual Example:**
```
Graph:
    A --- B --- E
    |     |
    |     |
    C --- D

DFS starting from A:
1. Visit A (mark A as visited)
2. Explore neighbor B (mark B as visited)
3. Explore neighbor E (mark E as visited)
   - E has no unvisited neighbors, backtrack to B
4. Explore neighbor D (mark D as visited)
   - D has neighbor C, which is unvisited
5. Explore neighbor C (mark C as visited)
   - C has no unvisited neighbors, backtrack to A
6. A has no more unvisited neighbors, DFS complete

Traversal order: A, B, E, D, C
```

**JavaScript Implementation:**

```javascript
// Recursive DFS
function dfsRecursive(graph, startVertex) {
  const result = [];
  const visited = {};
  
  function dfs(vertex) {
    if (!vertex) return;
    
    // Mark vertex as visited
    visited[vertex] = true;
    result.push(vertex);
    
    // Visit all neighbors
    for (const neighborObj of graph.getNeighbors(vertex)) {
      const neighbor = neighborObj.node;
      
      if (!visited[neighbor]) {
        dfs(neighbor);
      }
    }
  }
  
  dfs(startVertex);
  return result;
}

// Iterative DFS
function dfsIterative(graph, startVertex) {
  const result = [];
  const visited = {};
  const stack = [startVertex];
  visited[startVertex] = true;
  
  while (stack.length > 0) {
    const currentVertex = stack.pop();
    result.push(currentVertex);
    
    // Visit all neighbors (in reverse to match recursive order)
    const neighbors = graph.getNeighbors(currentVertex);
    for (let i = neighbors.length - 1; i >= 0; i--) {
      const neighbor = neighbors[i].node;
      
      if (!visited[neighbor]) {
        visited[neighbor] = true;
        stack.push(neighbor);
      }
    }
  }
  
  return result;
}

// DFS to detect cycles in a directed graph
function hasCycle(graph) {
  const vertices = graph.getVertices();
  const visited = {};
  const recStack = {}; // Recursion stack to track current path
  
  function dfsCheckCycle(vertex) {
    // Mark current vertex as visited and add to recursion stack
    visited[vertex] = true;
    recStack[vertex] = true;
    
    // Visit all neighbors
    for (const neighborObj of graph.getNeighbors(vertex)) {
      const neighbor = neighborObj.node;
      
      // If neighbor is not visited, recursively visit it
      if (!visited[neighbor]) {
        if (dfsCheckCycle(neighbor)) {
          return true; // Cycle found in deeper recursion
        }
      } 
      // If neighbor is in recursion stack, we found a cycle
      else if (recStack[neighbor]) {
        return true;
      }
    }
    
    // Remove vertex from recursion stack
    recStack[vertex] = false;
    return false;
  }
  
  // Check all vertices (for disconnected graph)
  for (const vertex of vertices) {
    if (!visited[vertex]) {
      if (dfsCheckCycle(vertex)) {
        return true;
      }
    }
  }
  
  return false;
}

// DFS for topological sort
function topologicalSort(graph) {
  const vertices = graph.getVertices();
  const visited = {};
  const result = [];
  
  function dfs(vertex) {
    visited[vertex] = true;
    
    for (const neighborObj of graph.getNeighbors(vertex)) {
      const neighbor = neighborObj.node;
      
      if (!visited[neighbor]) {
        dfs(neighbor);
      }
    }
    
    // Add current vertex to the beginning of result
    // (vertices are added in reverse order of finishing time)
    result.unshift(vertex);
  }
  
  for (const vertex of vertices) {
    if (!visited[vertex]) {
      dfs(vertex);
    }
  }
  
  return result;
}
```

**Pseudocode:**
```
FUNCTION DFSRecursive(graph, startVertex):
    result = empty array
    visited = empty map
    
    FUNCTION DFS(vertex):
        IF vertex is null THEN RETURN
        
        visited[vertex] = true
        APPEND vertex to result
        
        FOR EACH neighbor IN graph.getNeighbors(vertex):
            IF NOT visited[neighbor] THEN
                DFS(neighbor)
            END IF
        END FOR
    END FUNCTION
    
    DFS(startVertex)
    RETURN result
END FUNCTION

FUNCTION DFSIterative(graph, startVertex):
    result = empty array
    visited = empty map
    stack = [startVertex]
    visited[startVertex] = true
    
    WHILE stack is not empty:
        currentVertex = POP from stack
        APPEND currentVertex to result
        
        FOR EACH neighbor IN graph.getNeighbors(currentVertex) in reverse order:
            IF NOT visited[neighbor] THEN
                visited[neighbor] = true
                PUSH neighbor to stack
            END IF
        END FOR
    END WHILE
    
    RETURN result
END FUNCTION
```

**Time and Space Complexity:**
- Time Complexity: O(V + E) where V is the number of vertices and E is the number of edges
  - Each vertex and edge is processed once
- Space Complexity: 
  - O(V) for the visited set and recursion stack/explicit stack
  - In worst case (very unbalanced graph), the recursion stack can grow to O(V)

**Real-world Applications:**
- Maze solving algorithms
- Finding connected components
- Topological sorting for task scheduling
- Cycle detection in graphs
- Pathfinding in game AI
- Searching web pages (web crawlers)
- Garbage collection (marking phase)
- Solving puzzles like Sudoku

## 2. Breadth-First Search (BFS)

**Conceptual Explanation:**
Breadth-First Search is a graph traversal algorithm that explores all vertices at the present depth before moving on to vertices at the next depth level. It uses a queue to keep track of vertices to visit next. BFS is particularly useful for:
- Finding shortest paths in unweighted graphs
- Testing if a graph is bipartite
- Finding all nodes within a connected component
- Finding the shortest path between two nodes

**Visual Example:**
```
Graph:
    A --- B --- E
    |     |
    |     |
    C --- D

BFS starting from A:
1. Visit A (mark A as visited, enqueue A's neighbors: B, C)
2. Visit B (mark B as visited, enqueue B's neighbors: D, E)
3. Visit C (mark C as visited, enqueue C's neighbors: D)
4. Visit D (mark D as visited, no new unvisited neighbors)
5. Visit E (mark E as visited, no new unvisited neighbors)

Traversal order: A, B, C, D, E
```

**JavaScript Implementation:**

```javascript
// Standard BFS
function bfs(graph, startVertex) {
  const result = [];
  const visited = {};
  const queue = [startVertex];
  visited[startVertex] = true;
  
  while (queue.length > 0) {
    const currentVertex = queue.shift();
    result.push(currentVertex);
    
    for (const neighborObj of graph.getNeighbors(currentVertex)) {
      const neighbor = neighborObj.node;
      
      if (!visited[neighbor]) {
        visited[neighbor] = true;
        queue.push(neighbor);
      }
    }
  }
  
  return result;
}

// BFS to find shortest path (unweighted graph)
function shortestPath(graph, startVertex, endVertex) {
  if (startVertex === endVertex) {
    return [startVertex];
  }
  
  const visited = {};
  const queue = [startVertex];
  const previousVertex = {};
  visited[startVertex] = true;
  
  while (queue.length > 0) {
    const currentVertex = queue.shift();
    
    for (const neighborObj of graph.getNeighbors(currentVertex)) {
      const neighbor = neighborObj.node;
      
      if (!visited[neighbor]) {
        visited[neighbor] = true;
        previousVertex[neighbor] = currentVertex;
        queue.push(neighbor);
        
        // If we found the end vertex, reconstruct the path
        if (neighbor === endVertex) {
          return reconstructPath(previousVertex, startVertex, endVertex);
        }
      }
    }
  }
  
  // No path found
  return [];
}

// Helper function to reconstruct path from previousVertex map
function reconstructPath(previousVertex, startVertex, endVertex) {
  const path = [];
  let currentVertex = endVertex;
  
  while (currentVertex !== startVertex) {
    path.unshift(currentVertex);
    currentVertex = previousVertex[currentVertex];
  }
  
  path.unshift(startVertex);
  return path;
}

// BFS to check if a graph is bipartite (2-colorable)
function isBipartite(graph) {
  const vertices = graph.getVertices();
  if (vertices.length === 0) return true;
  
  const colors = {}; // 0: not colored, 1: color A, -1: color B
  
  function bfsColorCheck(startVertex) {
    const queue = [startVertex];
    colors[startVertex] = 1; // Color the start vertex
    
    while (queue.length > 0) {
      const currentVertex = queue.shift();
      const currentColor = colors[currentVertex];
      
      for (const neighborObj of graph.getNeighbors(currentVertex)) {
        const neighbor = neighborObj.node;
        
        // If not colored, color it with the opposite color
        if (!colors[neighbor]) {
          colors[neighbor] = -currentColor;
          queue.push(neighbor);
        } 
        // If already colored with the same color as current vertex, not bipartite
        else if (colors[neighbor] === currentColor) {
          return false;
        }
      }
    }
    
    return true;
  }
  
  // Check all vertices (for disconnected graph)
  for (const vertex of vertices) {
    if (!colors[vertex]) {
      if (!bfsColorCheck(vertex)) {
        return false;
      }
    }
  }
  
  return true;
}
```

**Pseudocode:**
```
FUNCTION BFS(graph, startVertex):
    result = empty array
    visited = empty map
    queue = [startVertex]
    visited[startVertex] = true
    
    WHILE queue is not empty:
        currentVertex = DEQUEUE from queue
        APPEND currentVertex to result
        
        FOR EACH neighbor IN graph.getNeighbors(currentVertex):
            IF NOT visited[neighbor] THEN
                visited[neighbor] = true
                ENQUEUE neighbor to queue
            END IF
        END FOR
    END WHILE
    
    RETURN result
END FUNCTION

FUNCTION ShortestPath(graph, startVertex, endVertex):
    IF startVertex = endVertex THEN
        RETURN [startVertex]
    END IF
    
    visited = empty map
    queue = [startVertex]
    previousVertex = empty map
    visited[startVertex] = true
    
    WHILE queue is not empty:
        currentVertex = DEQUEUE from queue
        
        FOR EACH neighbor IN graph.getNeighbors(currentVertex):
            IF NOT visited[neighbor] THEN
                visited[neighbor] = true
                previousVertex[neighbor] = currentVertex
                ENQUEUE neighbor to queue
                
                IF neighbor = endVertex THEN
                    RETURN ReconstructPath(previousVertex, startVertex, endVertex)
                END IF
            END IF
        END FOR
    END WHILE
    
    RETURN empty array  // No path found
END FUNCTION
```

**Time and Space Complexity:**
- Time Complexity: O(V + E) where V is the number of vertices and E is the number of edges
  - Each vertex is processed once when it's dequeued
  - Each edge is considered once when exploring neighbors
- Space Complexity: O(V) for the queue, visited set, and previous vertex map

**Real-world Applications:**
- Shortest path finding in unweighted graphs
- GPS navigation systems (when all edges have equal weights)
- Social network friend suggestions (people who are "2 degrees away")
- Web crawlers
- Finding the minimum number of moves in puzzles
- Network broadcasting
- Garbage collection algorithms
- Finding all nodes within a certain distance

## 3. Dijkstra's Algorithm

**Conceptual Explanation:**
Dijkstra's algorithm finds the shortest path from a source vertex to all other vertices in a weighted graph with non-negative edge weights. It uses a priority queue to greedily select the vertex with the smallest distance from the source and relaxes its outgoing edges.

The algorithm maintains two sets:
- Visited set: Vertices whose shortest distance from the source is determined
- Unvisited set: Vertices whose shortest distance might still be improved

**Visual Example:**
```
Weighted Graph:
    A ---3--- B ---2--- E
    |         |
    2         1
    |         |
    C ---4--- D

Dijkstra's Algorithm starting from A:

Initialize:
- Distance: A=0, B=∞, C=∞, D=∞, E=∞
- Previous: A=null, B=null, C=null, D=null, E=null
- Priority Queue: [A(0)]

Step 1: Process A (distance = 0)
- Update B: 0+3=3 (A→B), Previous[B] = A
- Update C: 0+2=2 (A→C), Previous[C] = A
- Priority Queue: [C(2), B(3)]

Step 2: Process C (distance = 2)
- Update D: 2+4=6 (A→C→D), Previous[D] = C
- Priority Queue: [B(3), D(6)]

Step 3: Process B (distance = 3)
- Update D: 3+1=4 (A→B→D), Previous[D] = B (better than 6)
- Update E: 3+2=5 (A→B→E), Previous[E] = B
- Priority Queue: [D(4), E(5)]

Step 4: Process D (distance = 4)
- No improvements
- Priority Queue: [E(5)]

Step 5: Process E (distance = 5)
- No improvements
- Priority Queue: empty, algorithm terminates

Result:
- Shortest distances from A: B=3, C=2, D=4, E=5
- Shortest paths: A→B, A→C, A→B→D, A→B→E
```

**JavaScript Implementation:**

```javascript
// Dijkstra's Algorithm with Priority Queue
function dijkstra(graph, startVertex, endVertex = null) {
  const vertices = graph.getVertices();
  
  // Initialize distances with Infinity for all vertices except the start vertex
  const distances = {};
  const previous = {};
  const priorityQueue = new MinPriorityQueue();
  
  for (const vertex of vertices) {
    distances[vertex] = vertex === startVertex ? 0 : Infinity;
    previous[vertex] = null;
  }
  
  // Add start vertex to priority queue
  priorityQueue.enqueue(startVertex, 0);
  
  while (!priorityQueue.isEmpty()) {
    const { element: currentVertex, priority: currentDistance } = priorityQueue.dequeue();
    
    // If we found the target vertex, we can stop
    if (endVertex && currentVertex === endVertex) {
      break;
    }
    
    // If we've already found a shorter path to the current vertex, skip it
    if (currentDistance > distances[currentVertex]) {
      continue;
    }
    
    // Visit all neighbors of the current vertex
    for (const neighborObj of graph.getNeighbors(currentVertex)) {
      const { node: neighbor, weight } = neighborObj;
      
      // Calculate new distance to neighbor
      const distance = distances[currentVertex] + weight;
      
      // If we found a shorter path to the neighbor
      if (distance < distances[neighbor]) {
        // Update distance and previous vertex
        distances[neighbor] = distance;
        previous[neighbor] = currentVertex;
        
        // Add neighbor to priority queue with updated distance
        priorityQueue.enqueue(neighbor, distance);
      }
    }
  }
  
  // If we're looking for the shortest path to a specific vertex
  if (endVertex) {
    // If no path exists
    if (distances[endVertex] === Infinity) {
      return { distance: Infinity, path: [] };
    }
    
    // Reconstruct the path
    const path = [];
    let current = endVertex;
    
    while (current) {
      path.unshift(current);
      current = previous[current];
    }
    
    return { distance: distances[endVertex], path };
  }
  
  // Return distances to all vertices
  return { distances, previous };
}

// Simple Min Priority Queue implementation 
// (in practice, use a more efficient implementation)
class MinPriorityQueue {
  constructor() {
    this.values = [];
  }
  
  enqueue(element, priority) {
    this.values.push({ element, priority });
    this.values.sort((a, b) => a.priority - b.priority);
  }
  
  dequeue() {
    return this.values.shift();
  }
  
  isEmpty() {
    return this.values.length === 0;
  }
}

// Dijkstra with Min Heap (more efficient implementation)
function dijkstraWithMinHeap(graph, startVertex, endVertex = null) {
  const vertices = graph.getVertices();
  
  // Initialize distances with Infinity for all vertices except the start vertex
  const distances = {};
  const previous = {};
  const visited = {};
  const minHeap = new MinHeap();
  
  for (const vertex of vertices) {
    distances[vertex] = vertex === startVertex ? 0 : Infinity;
    previous[vertex] = null;
  }
  
  // Add start vertex to min heap
  minHeap.insert(startVertex, 0);
  
  while (!minHeap.isEmpty()) {
    const { node: currentVertex, priority: currentDistance } = minHeap.extractMin();
    
    // If we've already processed this vertex, skip it
    if (visited[currentVertex]) continue;
    
    // Mark vertex as visited
    visited[currentVertex] = true;
    
    // If we found the target vertex, we can stop
    if (endVertex && currentVertex === endVertex) {
      break;
    }
    
    // Visit all neighbors of the current vertex
    for (const neighborObj of graph.getNeighbors(currentVertex)) {
      const { node: neighbor, weight } = neighborObj;
      
      // Skip visited neighbors
      if (visited[neighbor]) continue;
      
      // Calculate new distance to neighbor
      const distance = distances[currentVertex] + weight;
      
      // If we found a shorter path to the neighbor
      if (distance < distances[neighbor]) {
        // Update distance and previous vertex
        distances[neighbor] = distance;
        previous[neighbor] = currentVertex;
        
        // Add neighbor to min heap with updated distance
        minHeap.insert(neighbor, distance);
      }
    }
  }
  
  // Same return logic as before
  if (endVertex) {
    if (distances[endVertex] === Infinity) {
      return { distance: Infinity, path: [] };
    }
    
    const path = [];
    let current = endVertex;
    
    while (current) {
      path.unshift(current);
      current = previous[current];
    }
    
    return { distance: distances[endVertex], path };
  }
  
  return { distances, previous };
}

// Min Heap implementation for Dijkstra's algorithm
class MinHeap {
  constructor() {
    this.heap = [];
  }
  
  insert(node, priority) {
    this.heap.push({ node, priority });
    this.siftUp(this.heap.length - 1);
  }
  
  extractMin() {
    if (this.isEmpty()) {
      return null;
    }
    
    const min = this.heap[0];
    const last = this.heap.pop();
    
    if (this.heap.length > 0) {
      this.heap[0] = last;
      this.siftDown(0);
    }
    
    return min;
  }
  
  isEmpty() {
    return this.heap.length === 0;
  }
  
  siftUp(index) {
    let parentIndex = Math.floor((index - 1) / 2);
    
    while (index > 0 && this.heap[parentIndex].priority > this.heap[index].priority) {
      // Swap with parent
      [this.heap[parentIndex], this.heap[index]] = [this.heap[index], this.heap[parentIndex]];
      
      // Move up
      index = parentIndex;
      parentIndex = Math.floor((index - 1) / 2);
    }
  }
  
  siftDown(index) {
    const leftChild = 2 * index + 1;
    const rightChild = 2 * index + 2;
    let smallest = index;
    
    // Find the smallest of the three (current, left child, right child)
    if (leftChild < this.heap.length && this.heap[leftChild].priority < this.heap[smallest].priority) {
      smallest = leftChild;
    }
    
    if (rightChild < this.heap.length && this.heap[rightChild].priority < this.heap[smallest].priority) {
      smallest = rightChild;
    }
    
    // If smallest is not the current index, swap and sift down again
    if (smallest !== index) {
      [this.heap[index], this.heap[smallest]] = [this.heap[smallest], this.heap[index]];
      this.siftDown(smallest);
    }
  }
}
```

**Pseudocode:**
```
FUNCTION Dijkstra(graph, startVertex, endVertex = null):
    distances = map of all vertices initialized to Infinity
    previous = map of all vertices initialized to null
    priorityQueue = new MinPriorityQueue()
    visited = empty set
    
    // Initialize start vertex with distance 0
    distances[startVertex] = 0
    priorityQueue.enqueue(startVertex, 0)
    
    WHILE priorityQueue is not empty:
        {currentVertex, currentDistance} = priorityQueue.dequeue()
        
        // Skip if we've already found a better path
        IF currentDistance > distances[currentVertex] THEN
            CONTINUE
        END IF
        
        // Mark as visited
        ADD currentVertex to visited
        
        // If we found the target, we can stop
        IF endVertex is not null AND currentVertex = endVertex THEN
            BREAK
        END IF
        
        // Process all neighbors
        FOR EACH {neighbor, weight} IN graph.getNeighbors(currentVertex):
            // Skip visited neighbors
            IF neighbor IN visited THEN
                CONTINUE
            END IF
            
            // Calculate new distance
            distance =



**Time and Space Complexity:**
- Time Complexity:
    - With simple priority queue: O(V² + E) = O(V²) in worst case
    - With binary heap priority queue: O((V + E) log V) = O(E log V) for sparse graphs
    - With Fibonacci heap: O(V log V + E)
- Space Complexity: O(V) for the distance, previous, and visited arrays

**Real-world Applications:**
- GPS navigation systems
- Network routing protocols
- Flight scheduling
- Robotics (path planning)
- Telecommunications networks
- Public transportation systems
- Game AI pathfinding
- Logistics and supply chain optimization

## 4. Bellman-Ford Algorithm

**Conceptual Explanation:**
Bellman-Ford algorithm finds the shortest paths from a source vertex to all other vertices in a weighted graph. Unlike Dijkstra's algorithm, it can handle graphs with negative edge weights and can detect negative cycles. It works by relaxing all edges V-1 times, where V is the number of vertices in the graph.

A negative cycle is a cycle whose edges sum to a negative value, which would allow indefinitely decreasing path lengths by repeatedly traversing the cycle.

**Visual Example:**
```
Weighted Graph with negative edge:
    A ---3--- B ---2--- E
    |         |
    2        -1
    |         |
    C ---4--- D

Bellman-Ford Algorithm starting from A:

Initialize:
- Distance: A=0, B=∞, C=∞, D=∞, E=∞

Iteration 1: Relax all edges
- A→B: Update B to 3
- A→C: Update C to 2
- B→D: Update D to 2 (3-1)
- B→E: Update E to 5
- C→D: Update D to 6
- No change for D→B (since Distance[D] + (-1) = 2 + (-1) = 1 < Distance[B] = 3)

Iteration 2: Relax all edges
- No changes this iteration

Iteration 3: Relax all edges
- No changes this iteration

Iteration 4: Relax all edges
- No changes this iteration

No changes after |V|-1 iterations and no negative cycles detected.

Result:
- Shortest distances from A: B=3, C=2, D=2, E=5
```

**JavaScript Implementation:**

```javascript
// Bellman-Ford Algorithm
function bellmanFord(graph, startVertex) {
  const vertices = graph.getVertices();
  const edges = getAllEdges(graph);
  
  // Initialize distances with Infinity for all vertices except the start vertex
  const distances = {};
  const previous = {};
  
  for (const vertex of vertices) {
    distances[vertex] = vertex === startVertex ? 0 : Infinity;
    previous[vertex] = null;
  }
  
  // Relax all edges |V| - 1 times
  const numVertices = vertices.length;
  
  for (let i = 0; i < numVertices - 1; i++) {
    for (const { from, to, weight } of edges) {
      // If the "from" vertex is not reachable, skip this edge
      if (distances[from] === Infinity) continue;
      
      // Relax the edge
      if (distances[from] + weight < distances[to]) {
        distances[to] = distances[from] + weight;
        previous[to] = from;
      }
    }
  }
  
  // Check for negative cycles
  for (const { from, to, weight } of edges) {
    if (distances[from] === Infinity) continue;
    
    // If we can still relax an edge, there is a negative cycle
    if (distances[from] + weight < distances[to]) {
      return { hasNegativeCycle: true, distances, previous };
    }
  }
  
  return { hasNegativeCycle: false, distances, previous };
}

// Helper function to extract all edges from the graph
function getAllEdges(graph) {
  const edges = [];
  const vertices = graph.getVertices();
  
  for (const vertex of vertices) {
    for (const { node: neighbor, weight } of graph.getNeighbors(vertex)) {
      edges.push({ from: vertex, to: neighbor, weight });
    }
  }
  
  return edges;
}

// Reconstruct the shortest path from start to end
function reconstructPath(previous, startVertex, endVertex) {
  if (!previous[endVertex]) return null; // No path exists
  
  const path = [];
  let current = endVertex;
  
  while (current !== null) {
    path.unshift(current);
    current = previous[current];
  }
  
  // Ensure the path starts with the start vertex
  if (path[0] !== startVertex) return null;
  
  return path;
}
```

**Pseudocode:**
```
FUNCTION BellmanFord(graph, startVertex):
    vertices = graph.getVertices()
    edges = getAllEdges(graph)
    
    // Initialize distances
    distances = map of all vertices initialized to Infinity
    previous = map of all vertices initialized to null
    distances[startVertex] = 0
    
    // Relax edges |V| - 1 times
    FOR i = 0 TO vertices.length - 2:
        FOR EACH {from, to, weight} IN edges:
            // Skip edges from unreachable vertices
            IF distances[from] = Infinity THEN
                CONTINUE
            END IF
            
            // Relax edge
            IF distances[from] + weight < distances[to] THEN
                distances[to] = distances[from] + weight
                previous[to] = from
            END IF
        END FOR
    END FOR
    
    // Check for negative cycles
    FOR EACH {from, to, weight} IN edges:
        IF distances[from] = Infinity THEN
            CONTINUE
        END IF
        
        IF distances[from] + weight < distances[to] THEN
            RETURN {hasNegativeCycle: true, distances, previous}
        END IF
    END FOR
    
    RETURN {hasNegativeCycle: false, distances, previous}
END FUNCTION
```

**Time and Space Complexity:**
- Time Complexity: O(V × E) where V is the number of vertices and E is the number of edges
    - Each edge is relaxed V-1 times, and then checked one more time for negative cycles
- Space Complexity: O(V + E) for storing distances, previous pointers, and the edge list

**Real-world Applications:**
- Distributed routing protocols (like RIP - Routing Information Protocol)
- Currency exchange and arbitrage detection
- Network traffic engineering
- Detecting negative cycles in various systems
- Traffic flow analysis with constraints
- Resource allocation in systems with constraints
- Optimal timing in electronic circuits
- Solving linear programming problems

## 5. Floyd-Warshall Algorithm

**Conceptual Explanation:**
Floyd-Warshall algorithm finds the shortest paths between all pairs of vertices in a weighted graph with positive or negative edge weights (but no negative cycles). It's a dynamic programming approach that incrementally improves an estimate on the shortest path between two vertices by considering whether including a third vertex in the path would create a shorter path.

**Visual Example:**
```
Weighted Graph:
    A ---3--- B
    |       / |
    2   -1    2
    |   /     |
    C ---4--- D

Floyd-Warshall matrices:
Initial (direct edges only):
    A   B   C   D
A   0   3   2   ∞
B   ∞   0   -1  2
C   ∞   ∞   0   4
D   ∞   ∞   ∞   0

After considering A as intermediate:
    A   B   C   D
A   0   3   2   ∞
B   ∞   0   -1  2
C   ∞   ∞   0   4
D   ∞   ∞   ∞   0

After considering B as intermediate:
    A   B   C   D
A   0   3   2   5
B   ∞   0   -1  2
C   ∞   ∞   0   4
D   ∞   ∞   ∞   0

After considering C as intermediate:
    A   B   C   D
A   0   3   2   5
B   ∞   0   -1  2
C   ∞   ∞   0   4
D   ∞   ∞   ∞   0

After considering D as intermediate:
    A   B   C   D
A   0   3   2   5
B   ∞   0   -1  2
C   ∞   ∞   0   4
D   ∞   ∞   ∞   0

Final distance matrix (shortest path lengths between all vertex pairs)
```

**JavaScript Implementation:**

```javascript
// Floyd-Warshall Algorithm
function floydWarshall(graph) {
  const vertices = graph.getVertices();
  const n = vertices.length;
  
  // Create a mapping from vertex names to indices
  const vertexToIndex = {};
  const indexToVertex = {};
  
  for (let i = 0; i < n; i++) {
    vertexToIndex[vertices[i]] = i;
    indexToVertex[i] = vertices[i];
  }
  
  // Initialize distance and next matrices
  const distance = Array(n).fill().map(() => Array(n).fill(Infinity));
  const next = Array(n).fill().map(() => Array(n).fill(null));
  
  // Set diagonal to 0 (distance from vertex to itself)
  for (let i = 0; i < n; i++) {
    distance[i][i] = 0;
  }
  
  // Initialize with direct edges
  for (const vertex of vertices) {
    const i = vertexToIndex[vertex];
    
    for (const { node: neighbor, weight } of graph.getNeighbors(vertex)) {
      const j = vertexToIndex[neighbor];
      distance[i][j] = weight;
      next[i][j] = j;
    }
  }
  
  // Floyd-Warshall algorithm
  for (let k = 0; k < n; k++) {
    for (let i = 0; i < n; i++) {
      for (let j = 0; j < n; j++) {
        if (distance[i][k] !== Infinity && distance[k][j] !== Infinity) {
          const throughK = distance[i][k] + distance[k][j];
          
          if (throughK < distance[i][j]) {
            distance[i][j] = throughK;
            next[i][j] = next[i][k];
          }
        }
      }
    }
  }
  
  // Check for negative cycles
  for (let i = 0; i < n; i++) {
    if (distance[i][i] < 0) {
      return { hasNegativeCycle: true };
    }
  }
  
  // Function to reconstruct path between two vertices
  function getPath(startVertex, endVertex) {
    const i = vertexToIndex[startVertex];
    const j = vertexToIndex[endVertex];
    
    if (distance[i][j] === Infinity) return null; // No path exists
    
    const path = [startVertex];
    let currentIndex = i;
    
    while (currentIndex !== j) {
      currentIndex = next[currentIndex][j];
      
      // If we encounter null, there's no path
      if (currentIndex === null) return null;
      
      path.push(indexToVertex[currentIndex]);
    }
    
    return path;
  }
  
  // Function to get distance between two vertices
  function getDistance(startVertex, endVertex) {
    const i = vertexToIndex[startVertex];
    const j = vertexToIndex[endVertex];
    return distance[i][j];
  }
  
  return {
    hasNegativeCycle: false,
    getPath,
    getDistance,
    distance,
    next,
    vertexToIndex,
    indexToVertex
  };
}
```

**Pseudocode:**
```
FUNCTION FloydWarshall(graph):
    vertices = graph.getVertices()
    n = vertices.length
    
    // Create index mappings
    vertexToIndex = empty map
    indexToVertex = empty map
    
    FOR i = 0 TO n-1:
        vertexToIndex[vertices[i]] = i
        indexToVertex[i] = vertices[i]
    END FOR
    
    // Initialize distance and next matrices
    distance = n×n matrix filled with Infinity
    next = n×n matrix filled with null
    
    // Set diagonal to 0
    FOR i = 0 TO n-1:
        distance[i][i] = 0
    END FOR
    
    // Initialize with direct edges
    FOR EACH vertex IN vertices:
        i = vertexToIndex[vertex]
        
        FOR EACH {neighbor, weight} IN graph.getNeighbors(vertex):
            j = vertexToIndex[neighbor]
            distance[i][j] = weight
            next[i][j] = j
        END FOR
    END FOR
    
    // Main algorithm
    FOR k = 0 TO n-1:
        FOR i = 0 TO n-1:
            FOR j = 0 TO n-1:
                IF distance[i][k] != Infinity AND distance[k][j] != Infinity THEN
                    throughK = distance[i][k] + distance[k][j]
                    
                    IF throughK < distance[i][j] THEN
                        distance[i][j] = throughK
                        next[i][j] = next[i][k]
                    END IF
                END IF
            END FOR
        END FOR
    END FOR
    
    // Check for negative cycles
    FOR i = 0 TO n-1:
        IF distance[i][i] < 0 THEN
            RETURN {hasNegativeCycle: true}
        END IF
    END FOR
    
    // Return result
    RETURN {
        hasNegativeCycle: false,
        distance,
        next,
        vertexToIndex,
        indexToVertex
    }
END FUNCTION
```

**Time and Space Complexity:**
- Time Complexity: O(V³) where V is the number of vertices
- Space Complexity: O(V²) for the distance and next matrices

**Real-world Applications:**
- Finding all-pairs shortest paths in transportation networks
- Network routing tables
- Traffic flow analysis
- Determining transitive closure of a graph
- Impact analysis in complex systems
- Finding maximum bandwidth paths in computer networks
- Path optimization for multiple routes in logistics
- Comparing multiple possible routes in navigation systems

## 6. Minimum Spanning Tree Algorithms

**Conceptual Explanation:**
A Minimum Spanning Tree (MST) is a subset of edges from a connected, undirected graph that connects all vertices with the minimum possible total edge weight. Two popular algorithms for finding MSTs are:

1. **Kruskal's Algorithm**: Sorts all edges by weight and greedily adds the lightest edge that doesn't create a cycle.
2. **Prim's Algorithm**: Starts from an arbitrary vertex and greedily adds the lightest edge that connects a vertex in the MST to a vertex outside it.

**Visual Example:**
```
Weighted Undirected Graph:
   B
  /|\
 3 | 4
/  |  \
A--2--C
 \    /
  5  1
   \ /
    D

Kruskal's Algorithm:
1. Sort edges: D-C (1), A-C (2), A-B (3), B-C (4), A-D (5)
2. Add D-C: MST = {D-C}
3. Add A-C: MST = {D-C, A-C}
4. Add A-B: MST = {D-C, A-C, A-B}
   (All vertices connected, MST complete)

Prim's Algorithm (starting from A):
1. Edges from A: A-B (3), A-C (2), A-D (5)
2. Add A-C (lightest): MST = {A-C}
3. Edges from A, C: A-B (3), A-D (5), C-B (4), C-D (1)
4. Add C-D (lightest): MST = {A-C, C-D}
5. Edges from A, C, D: A-B (3), A-D (5), C-B (4)
6. Add A-B (lightest): MST = {A-C, C-D, A-B}
   (All vertices connected, MST complete)

Result: MST = {A-B (3), A-C (2), C-D (1)} with total weight 6
```

**JavaScript Implementation:**

```javascript
// Kruskal's Algorithm with Union-Find
function kruskalMST(graph) {
  const edges = getAllEdges(graph).sort((a, b) => a.weight - b.weight);
  const vertices = graph.getVertices();
  const n = vertices.length;
  
  // Initialize disjoint-set (Union-Find)
  const parent = {};
  const rank = {};
  
  for (const vertex of vertices) {
    parent[vertex] = vertex; // Each vertex is its own parent initially
    rank[vertex] = 0;
  }
  
  // Find root of a vertex
  function find(vertex) {
    // Path compression
    if (parent[vertex] !== vertex) {
      parent[vertex] = find(parent[vertex]);
    }
    return parent[vertex];
  }
  
  // Union two sets
  function union(vertex1, vertex2) {
    const root1 = find(vertex1);
    const root2 = find(vertex2);
    
    if (root1 === root2) return false; // Already in the same set
    
    // Union by rank
    if (rank[root1] < rank[root2]) {
      parent[root1] = root2;
    } else if (rank[root1] > rank[root2]) {
      parent[root2] = root1;
    } else {
      parent[root2] = root1;
      rank[root1]++;
    }
    
    return true;
  }
  
  const mstEdges = [];
  let mstWeight = 0;
  
  // Process edges in ascending order of weight
  for (const { from, to, weight } of edges) {
    // If including this edge doesn't create a cycle
    if (union(from, to)) {
      mstEdges.push({ from, to, weight });
      mstWeight += weight;
      
      // If we've added n-1 edges, MST is complete
      if (mstEdges.length === n - 1) break;
    }
  }
  
  return { mstEdges, mstWeight };
}

// Prim's Algorithm
function primMST(graph, startVertex) {
  const vertices = graph.getVertices();
  if (!startVertex) startVertex = vertices[0];
  
  // Set to track vertices included in MST
  const inMST = new Set();
  inMST.add(startVertex);
  
  // Priority queue for edge selection (use a more efficient implementation in practice)
  const priorityQueue = [];
  
  // Add all edges from the start vertex to the priority queue
  for (const { node: neighbor, weight } of graph.getNeighbors(startVertex)) {
    priorityQueue.push({ from: startVertex, to: neighbor, weight });
  }
  
  // Sort the priority queue by edge weight
  priorityQueue.sort((a, b) => a.weight - b.weight);
  
  const mstEdges = [];
  let mstWeight = 0;
  
  // Continue until all vertices are included or no more edges
  while (priorityQueue.length > 0 && inMST.size < vertices.length) {
    // Get the lightest edge
    const { from, to, weight } = priorityQueue.shift();
    
    // If the destination vertex is already in MST, skip this edge
    if (inMST.has(to)) continue;
    
    // Add the edge to MST
    mstEdges.push({ from, to, weight });
    mstWeight += weight;
    inMST.add(to);
    
    // Add all edges from the newly added vertex
    for (const { node: neighbor, weight } of graph.getNeighbors(to)) {
      // Only consider edges to vertices not yet in MST
      if (!inMST.has(neighbor)) {
        priorityQueue.push({ from: to, to: neighbor, weight });
        // Re-sort the priority queue
        priorityQueue.sort((a, b) => a.weight - b.weight);
      }
    }
  }
  
  // Check if MST includes all vertices
  if (inMST.size !== vertices.length) {
    return { isConnected: false }; // Graph is not connected
  }
  
  return { isConnected: true, mstEdges, mstWeight };
}

// Efficient Prim's Algorithm with Min Heap
function primMSTWithHeap(graph, startVertex) {
  const vertices = graph.getVertices();
  if (!startVertex) startVertex = vertices[0];
  
  // Set to track vertices included in MST
  const inMST = new Set();
  
  // Map to track minimum edge weight to each vertex
  const key = {};
  // Map to track parent of each vertex in MST
  const parent = {};
  
  // Initialize all vertices with infinite key
  for (const vertex of vertices) {
    key[vertex] = Infinity;
    parent[vertex] = null;
  }
  
  // Min heap to get the vertex with minimum key
  const minHeap = new MinHeap();
  
  // Start with the given vertex
  key[startVertex] = 0;
  minHeap.insert(startVertex, 0);
  
  const mstEdges = [];
  let mstWeight = 0;
  
  while (!minHeap.isEmpty() && inMST.size < vertices.length) {
    const { node: u } = minHeap.extractMin();
    
    // Skip if already in MST
    if (inMST.has(u)) continue;
    
    // Add to MST
    inMST.add(u);
    
    // Add the edge to MST (except for the starting vertex)
    if (parent[u] !== null) {
      const edge = { from: parent[u], to: u, weight: key[u] };
      mstEdges.push(edge);
      mstWeight += key[u];
    }
    
    // Update keys of adjacent vertices
    for (const { node: v, weight } of graph.getNeighbors(u)) {
      if (!inMST.has(v) && weight < key[v]) {
        parent[v] = u;
        key[v] = weight;
        minHeap.insert(v, weight);
      }
    }
  }
  
  // Check if MST includes all vertices
  if (inMST.size !== vertices.length) {
    return { isConnected: false }; // Graph is not connected
  }
  
  return { isConnected: true, mstEdges, mstWeight };
}
```

**Pseudocode:**
```
FUNCTION KruskalMST(graph):
    edges = getAllEdges(graph) sorted by weight
    vertices = graph.getVertices()
    
    // Initialize Union-Find data structure
    parent = map from each vertex to itself
    rank = map from each vertex to 0
    
    FUNCTION Find(vertex):
        IF parent[vertex] != vertex THEN
            parent[vertex] = Find(parent[vertex])
        END IF
        RETURN parent[vertex]
    END FUNCTION
    
    FUNCTION Union(vertex1, vertex2):
        root1 = Find(vertex1)
        root2 = Find(vertex2)
        
        IF root1 = root2 THEN RETURN false
        
        IF rank[root1] < rank[root2] THEN
            parent[root1] = root2
        ELSE IF rank[root1] > rank[root2] THEN
            parent[root2] = root1
        ELSE
            parent[root2] = root1
            rank[root1] = rank[root1] + 1
        END IF
        
        RETURN true
    END FUNCTION
    
    mstEdges = empty array
    mstWeight = 0
    
    FOR EACH {from, to, weight} IN edges:
        IF Union(from, to) THEN
            APPEND {from, to, weight} to mstEdges
            mstWeight = mstWeight + weight
            
            IF length(mstEdges) = length(vertices) - 1 THEN
                BREAK
            END IF
        END IF
    END FOR
    
    RETURN {mstEdges, mstWeight}
END FUNCTION

FUNCTION PrimMST(graph, startVertex):
    vertices = graph.getVertices()
    IF startVertex is null THEN startVertex = vertices[0]
    
    inMST = empty set
    key = map from each vertex to Infinity
    parent = map from each vertex to null
    
    key[startVertex] = 0
    minHeap = new MinHeap()
    minHeap.insert(startVertex, 0)
    
    mstEdges = empty array
    mstWeight = 0
    
    WHILE minHeap is not empty AND size(inMST) < length(vertices):
        u = minHeap.extractMin()
        
        IF u in inMST THEN CONTINUE
        
        ADD u to inMST
        
        IF parent[u] is not null THEN
            edge = {from: parent[u], to: u, weight: key[u]}
            APPEND edge to mstEdges
            mstWeight = mstWeight + key[u]
        END IF
        
        FOR EACH {neighbor, weight} IN graph.getNeighbors(u):
            IF neighbor not in inMST AND weight < key[neighbor] THEN
                parent[neighbor] = u
                key[neighbor] = weight
                minHeap.insert(neighbor, weight)
            END IF
        END FOR
    END WHILE
    
    IF size(inMST) != length(vertices) THEN
        RETURN {isConnected: false}
    END IF
    
    RETURN {isConnected: true, mstEdges, mstWeight}
END FUNCTION
```

**Time and Space Complexity:**
Kruskal's Algorithm:
- Time Complexity: O(E log E) or O(E log V) where E is the number of edges and V is the number of vertices
  - Sorting edges: O(E log E)
  - Union-Find operations: O(E log* V) which is nearly constant
- Space Complexity: O(V + E) for the edge list and union-find structures

Prim's Algorithm:
- Time Complexity:
  - With simple priority queue: O(V² + E) = O(V²)
  - With binary heap: O((V + E) log V) = O(E log V) for connected graphs
  - With Fibonacci heap: O(E + V log V)
- Space Complexity: O(V) for the priority queue and tracking structures

**Real-world Applications:**
- Network design (minimizing total cable length)
- Cluster analysis
- Circuit design
- Approximation algorithms for NP-hard problems like Traveling Salesman
- Water supply networks
- Power grid optimization
- Telecommunications network design
- Transportation systems planning

## 7. Topological Sort

**Conceptual Explanation:**
Topological sorting is an algorithm for ordering the vertices of a directed acyclic graph (DAG) such that for every directed edge u → v, vertex u comes before vertex v in the ordering. It's only possible if the graph has no directed cycles, i.e., it's a DAG.

Two main algorithms for topological sorting:
1. **DFS-based**: Use depth-first search and add vertices to the result in post-order
2. **Kahn's Algorithm**: Repeatedly remove vertices with no incoming edges

**Visual Example:**
```
Directed Acyclic Graph:
    A → B → C
    ↓   ↓
    D → E

DFS-based Topological Sort:
1. Start DFS from vertex A:
   - Visit B, then C (mark all as visited)
   - Visit E (mark as visited)
   - Visit D (mark as visited)
   - After completing DFS for A and its descendants, add A to result
2. Result (in reverse order): C, E, B, D, A

Kahn's Algorithm:
1. Identify vertices with no incoming edges: A
2. Remove A, add to result: Result = [A]
3. Update graph, new vertices with no incoming edges: B, D
4. Remove B, add to result: Result = [A, B]
5. Update graph, new vertices with no incoming edges: D, C
6. Remove D, add to result: Result = [A, B, D]
7. Update graph, new vertices with no incoming edges: C, E
8. Remove C, add to result: Result = [A, B, D, C]
9. Update graph, new vertices with no incoming edges: E
10. Remove E, add to result: Result = [A, B, D, C, E]

Valid Topological Orderings: 
- A, B, C, D, E
- A, B, D, C, E
- A, D, B, C, E
```

**JavaScript Implementation:**

```javascript
// DFS-based Topological Sort
function topologicalSortDFS(graph) {
  const vertices = graph.getVertices();
  const visited = {};
  const result = [];
  
  function dfs(vertex) {
    // Mark as visited
    visited[vertex] = true;
    
    // Visit all neighbors
    for (const { node: neighbor } of graph.getNeighbors(vertex)) {
      if (!visited[neighbor]) {
        dfs(neighbor);
      }
    }
    
    // Add current vertex to the beginning of result
    result.unshift(vertex);
  }
  
  // Process all vertices (for disconnected graph)
  for (const vertex of vertices) {
    if (!visited[vertex]) {
      dfs(vertex);
    }
  }
  
  return result;
}