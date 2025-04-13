# 🌐 Graphs – Data Structure Guide

## 📘 What Is a Graph?

A **Graph** is a **non-linear** data structure consisting of:
* **Nodes** (also called **vertices**)
* **Edges** (connections between nodes)

🧠 Unlike Trees (which are a special type of graph), **Graphs** can:
* Be **cyclic** or **acyclic**
* Have **multiple connections**
* Allow **loops** or **self-connections**
* Be **directed** or **undirected**

## 🧩 Graph Structure

```
   A ---- B
   |     /|
   |    / |
   |   /  |
   |  /   |
   | /    |
   C ---- D
```

| Element | Description |
|---------|-------------|
| **Vertices (Nodes)** | A, B, C, D |
| **Edges (Links)** | A-B, B-C, C-D, etc. |
| **Directed Edge** | One-way arrow (A → B) |
| **Undirected Edge** | Two-way (A — B) |
| **Weighted Edge** | Edge with a value (e.g., distance) |

## 💡 Graph vs Tree (Diffly)

| Feature | Graph | Tree |
|---------|-------|------|
| Structure | Non-linear, interconnected | Hierarchical, acyclic |
| Cycles | Can have cycles (loops) | No cycles allowed |
| Connections | Many-to-many | One-to-many (parent → child) |
| Root Node | No root (unless tree-like) | Has a root |
| Use Cases | Networks, Maps, Paths | Hierarchy, Menus |

## 🔄 Types of Graphs

| Type | Description | Example Use Case |
|------|-------------|-----------------|
| **Directed Graph (Digraph)** | Edges have direction | Twitter followers |
| **Undirected Graph** | No direction | Facebook friends |
| **Weighted Graph** | Edges have weight/cost | GPS, shortest path |
| **Unweighted Graph** | Just connections | Social networks |
| **Cyclic Graph** | Contains cycles | Process dependency graph |
| **Acyclic Graph (DAG)** | No cycles | Task scheduling, Git commits |
| **Connected Graph** | Every node is reachable | Internet, road maps |
| **Sparse vs Dense** | Few edges vs many edges | Depends on context |
| **Bipartite Graph** | Nodes divisible into two sets | Matching problems, scheduling |
| **Complete Graph** | Every node connects to every other | Network topology analyses |

## 🧠 Why Use Graphs?

| Problem Type | Graph Solution |
|--------------|---------------|
| Network modeling | Internet, social media, telecom |
| Shortest path | Dijkstra's, A* algorithm |
| Web crawlers | Page links (Graph of the web) |
| Maps/navigation | GPS route finding |
| Recommendations | Graph-based (friends of friends) |
| Task dependencies | Topological sorting (DAGs) |
| Circuit design | Electric circuits as graphs |
| Machine learning | Graph neural networks |

## ⚙️ Graph Representations in Code

### 1. **Adjacency Matrix**

```
  A B C
A 0 1 1
B 1 0 1
C 1 1 0
```

* Good for small, dense graphs
* Faster edge lookup (O(1))
* Space complexity: O(V²)
* Memory inefficient for sparse graphs

### 2. **Adjacency List**

```javascript
{
  A: [B, C],
  B: [A, C],
  C: [A, B]
}
```

* Good for large, sparse graphs
* Uses less space
* Space complexity: O(V + E)
* Slightly slower edge lookup (O(degree))

### 3. **Edge List**

```javascript
[
  [A, B],
  [A, C],
  [B, C]
]
```

* Simple representation
* Good for algorithms that process all edges
* Less memory overhead for very sparse graphs

## 🔍 Graph Algorithms (Essential Ones)

| Algorithm | Use Case | Time Complexity |
|-----------|----------|----------------|
| **DFS (Depth-First Search)** | Explore as deep as possible | O(V + E) |
| **BFS (Breadth-First Search)** | Explore level-by-level | O(V + E) |
| **Dijkstra's Algorithm** | Shortest path in weighted graph | O(E + V log V) |
| **A* Search** | Optimized pathfinding (uses heuristic) | O(E) but depends on heuristic |
| **Kruskal's Algorithm** | Minimum spanning tree | O(E log E) |
| **Prim's Algorithm** | Minimum spanning tree | O(E log V) |
| **Topological Sort** | Task scheduling (DAG only) | O(V + E) |
| **Bellman-Ford** | Shortest path (handles negative weights) | O(V·E) |
| **Floyd-Warshall** | All pairs shortest paths | O(V³) |

## 📱 Real-Life Examples

| Scenario | Graph Model |
|----------|-------------|
| Facebook / LinkedIn | Users as nodes, connections as edges |
| Google Maps | Locations as nodes, roads as edges |
| Git Version Control | Commits as nodes, changes as edges |
| YouTube Recommendations | Videos as nodes, suggestions as edges |
| Flight Routes | Airports (nodes), flights (edges) |
| Internet | Routers as nodes, connections as edges |
| Blockchain | Transactions as nodes, links as edges |
| Knowledge Graphs | Concepts as nodes, relationships as edges |

## 🧩 Graph Implementation Examples

### JavaScript Graph Implementation (Adjacency List)

```javascript
class Graph {
  constructor(isDirected = false) {
    this.adjacencyList = {};
    this.isDirected = isDirected;
  }
  
  // Add a vertex
  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) {
      this.adjacencyList[vertex] = [];
    }
    return this;
  }
  
  // Add an edge between vertices
  addEdge(vertex1, vertex2, weight = 1) {
    if (!this.adjacencyList[vertex1]) {
      this.addVertex(vertex1);
    }
    if (!this.adjacencyList[vertex2]) {
      this.addVertex(vertex2);
    }
    
    this.adjacencyList[vertex1].push({ node: vertex2, weight });
    
    // If undirected, add the reverse edge
    if (!this.isDirected) {
      this.adjacencyList[vertex2].push({ node: vertex1, weight });
    }
    
    return this;
  }
  
  // Remove an edge
  removeEdge(vertex1, vertex2) {
    this.adjacencyList[vertex1] = this.adjacencyList[vertex1].filter(
      edge => edge.node !== vertex2
    );
    
    if (!this.isDirected) {
      this.adjacencyList[vertex2] = this.adjacencyList[vertex2].filter(
        edge => edge.node !== vertex1
      );
    }
    
    return this;
  }
  
  // Remove a vertex and all its edges
  removeVertex(vertex) {
    if (!this.adjacencyList[vertex]) return;
    
    // Remove all edges connected to this vertex
    while (this.adjacencyList[vertex].length) {
      const adjacentVertex = this.adjacencyList[vertex].pop().node;
      this.removeEdge(vertex, adjacentVertex);
    }
    
    // Delete the vertex key
    delete this.adjacencyList[vertex];
    
    return this;
  }
  
  // Depth-First Search (recursive)
  dfsRecursive(start) {
    const result = [];
    const visited = {};
    const adjacencyList = this.adjacencyList;
    
    (function dfs(vertex) {
      if (!vertex) return null;
      visited[vertex] = true;
      result.push(vertex);
      
      adjacencyList[vertex].forEach(neighbor => {
        if (!visited[neighbor.node]) {
          return dfs(neighbor.node);
        }
      });
    })(start);
    
    return result;
  }
  
  // Depth-First Search (iterative)
  dfsIterative(start) {
    const stack = [start];
    const result = [];
    const visited = {};
    visited[start] = true;
    
    while (stack.length) {
      const currentVertex = stack.pop();
      result.push(currentVertex);
      
      this.adjacencyList[currentVertex].forEach(neighbor => {
        if (!visited[neighbor.node]) {
          visited[neighbor.node] = true;
          stack.push(neighbor.node);
        }
      });
    }
    
    return result;
  }
  
  // Breadth-First Search
  bfs(start) {
    const queue = [start];
    const result = [];
    const visited = {};
    visited[start] = true;
    
    while (queue.length) {
      const currentVertex = queue.shift();
      result.push(currentVertex);
      
      this.adjacencyList[currentVertex].forEach(neighbor => {
        if (!visited[neighbor.node]) {
          visited[neighbor.node] = true;
          queue.push(neighbor.node);
        }
      });
    }
    
    return result;
  }
  
  // Dijkstra's Algorithm for shortest path
  dijkstra(start, finish) {
    const nodes = new PriorityQueue();
    const distances = {};
    const previous = {};
    const path = []; // to return at end
    let smallest;
    
    // Build up initial state
    for (let vertex in this.adjacencyList) {
      if (vertex === start) {
        distances[vertex] = 0;
        nodes.enqueue(vertex, 0);
      } else {
        distances[vertex] = Infinity;
        nodes.enqueue(vertex, Infinity);
      }
      previous[vertex] = null;
    }
    
    // As long as there is something to visit
    while (nodes.values.length) {
      smallest = nodes.dequeue().val;
      
      if (smallest === finish) {
        // We are done, build up path to return
        while (previous[smallest]) {
          path.push(smallest);
          smallest = previous[smallest];
        }
        break;
      }
      
      if (smallest || distances[smallest] !== Infinity) {
        for (let neighbor of this.adjacencyList[smallest]) {
          // Calculate new distance to neighboring node
          let candidate = distances[smallest] + neighbor.weight;
          if (candidate < distances[neighbor.node]) {
            // Updating new smallest distance to neighbor
            distances[neighbor.node] = candidate;
            // Updating previous - How we got to neighbor
            previous[neighbor.node] = smallest;
            // Enqueue in priority queue with new priority
            nodes.enqueue(neighbor.node, candidate);
          }
        }
      }
    }
    
    return path.concat(smallest).reverse();
  }
}

// Simple priority queue for Dijkstra's
class PriorityQueue {
  constructor() {
    this.values = [];
  }
  
  enqueue(val, priority) {
    this.values.push({ val, priority });
    this.sort();
  }
  
  dequeue() {
    return this.values.shift();
  }
  
  sort() {
    this.values.sort((a, b) => a.priority - b.priority);
  }
}

// Usage
const g = new Graph();
g.addVertex("A");
g.addVertex("B");
g.addVertex("C");
g.addVertex("D");
g.addEdge("A", "B", 3);
g.addEdge("A", "C", 2);
g.addEdge("B", "D", 1);
g.addEdge("C", "D", 4);

console.log(g.dfsRecursive("A")); // ["A", "B", "D", "C"]
console.log(g.bfs("A"));          // ["A", "B", "C", "D"]
console.log(g.dijkstra("A", "D")); // ["A", "B", "D"]
```

### Python Graph Implementation

```python
class Graph:
    def __init__(self, is_directed=False):
        self.adjacency_list = {}
        self.is_directed = is_directed
        
    def add_vertex(self, vertex):
        if vertex not in self.adjacency_list:
            self.adjacency_list[vertex] = []
        return self
        
    def add_edge(self, vertex1, vertex2, weight=1):
        if vertex1 not in self.adjacency_list:
            self.add_vertex(vertex1)
        if vertex2 not in self.adjacency_list:
            self.add_vertex(vertex2)
            
        self.adjacency_list[vertex1].append({"node": vertex2, "weight": weight})
        
        # If undirected, add reverse edge
        if not self.is_directed:
            self.adjacency_list[vertex2].append({"node": vertex1, "weight": weight})
            
        return self
        
    def remove_edge(self, vertex1, vertex2):
        if vertex1 in self.adjacency_list and vertex2 in self.adjacency_list:
            self.adjacency_list[vertex1] = [
                edge for edge in self.adjacency_list[vertex1] 
                if edge["node"] != vertex2
            ]
            
            if not self.is_directed:
                self.adjacency_list[vertex2] = [
                    edge for edge in self.adjacency_list[vertex2] 
                    if edge["node"] != vertex1
                ]
                
        return self
        
    def remove_vertex(self, vertex):
        if vertex not in self.adjacency_list:
            return self
            
        # Remove all edges connected to this vertex
        for v in self.adjacency_list:
            self.remove_edge(v, vertex)
            
        # Remove the vertex
        del self.adjacency_list[vertex]
        return self
        
    def dfs_recursive(self, start):
        result = []
        visited = {}
        
        def dfs(vertex):
            if not vertex:
                return None
                
            visited[vertex] = True
            result.append(vertex)
            
            for neighbor in self.adjacency_list[vertex]:
                if neighbor["node"] not in visited:
                    dfs(neighbor["node"])
                    
        dfs(start)
        return result
        
    def dfs_iterative(self, start):
        stack = [start]
        result = []
        visited = {start: True}
        
        while stack:
            current_vertex = stack.pop()
            result.append(current_vertex)
            
            for neighbor in self.adjacency_list[current_vertex]:
                if neighbor["node"] not in visited:
                    visited[neighbor["node"]] = True
                    stack.append(neighbor["node"])
                    
        return result
        
    def bfs(self, start):
        queue = [start]
        result = []
        visited = {start: True}
        
        while queue:
            current_vertex = queue.pop(0)  # Dequeue
            result.append(current_vertex)
            
            for neighbor in self.adjacency_list[current_vertex]:
                if neighbor["node"] not in visited:
                    visited[neighbor["node"]] = True
                    queue.append(neighbor["node"])
                    
        return result

# Usage
g = Graph()
g.add_vertex("A")
g.add_vertex("B")
g.add_vertex("C")
g.add_vertex("D")
g.add_edge("A", "B")
g.add_edge("A", "C")
g.add_edge("B", "D")
g.add_edge("C", "D")

print(g.dfs_recursive("A"))  # ['A', 'B', 'D', 'C']
print(g.bfs("A"))           # ['A', 'B', 'C', 'D']
```

## 🔍 Advanced Graph Algorithms

### Topological Sort

Useful for scheduling tasks with dependencies. Only works on Directed Acyclic Graphs (DAGs).

```javascript
// Add to Graph class
topologicalSort() {
  const visited = {};
  const result = [];
  const adjacencyList = this.adjacencyList;
  
  function dfs(vertex) {
    if (!adjacencyList[vertex]) return;
    
    visited[vertex] = true;
    
    for (let neighbor of adjacencyList[vertex]) {
      if (!visited[neighbor.node]) {
        dfs(neighbor.node);
      }
    }
    
    // Add to result AFTER all dependents are processed
    result.unshift(vertex);
  }
  
  // Start DFS from each unvisited vertex
  for (let vertex in adjacencyList) {
    if (!visited[vertex]) {
      dfs(vertex);
    }
  }
  
  return result;
}
```

### Minimum Spanning Tree (Kruskal's Algorithm)

Finds the minimum weight set of edges that connects all vertices.

```javascript
// Requires a disjoint-set data structure
function kruskalMST(graph) {
  // Create result array to store edges of MST
  const result = [];
  
  // Create disjoint sets for each vertex
  const disjointSet = new DisjointSet(Object.keys(graph.adjacencyList));
  
  // Create an array of all edges
  const edges = [];
  for (let vertex in graph.adjacencyList) {
    for (let edge of graph.adjacencyList[vertex]) {
      edges.push({
        source: vertex,
        destination: edge.node,
        weight: edge.weight
      });
    }
  }
  
  // Sort edges by weight
  edges.sort((a, b) => a.weight - b.weight);
  
  // Process edges in order of increasing weight
  for (let edge of edges) {
    const { source, destination, weight } = edge;
    
    // If including this edge doesn't cause a cycle
    if (disjointSet.find(source) !== disjointSet.find(destination)) {
      // Include it in the result
      result.push(edge);
      
      // Union the two sets
      disjointSet.union(source, destination);
    }
  }
  
  return result;
}
```

## 🧠 Graph Problem Patterns

| Pattern | Description | Example Problems |
|---------|-------------|------------------|
| **Traversal** | Visit all nodes/edges | Connected components, Maze solving |
| **Shortest Path** | Find min cost path | Network routing, Maze shortest path |
| **Connectivity** | Check if nodes connect | Friend circles, Network reliability |
| **Cycle Detection** | Find/detect cycles | Deadlock detection, Scheduling |
| **Topological Order** | Sequence with dependencies | Course scheduling, Build systems |
| **Matching** | Pair nodes optimally | Job assignment, Stable marriage |
| **Flow Networks** | Max flow through edges | Traffic analysis, Resource allocation |

## 🧩 Specialized Graph Types

### 1. Bipartite Graphs
Nodes can be divided into two sets where no edge connects nodes within the same set. Used for:
- Matching problems
- Resource allocation
- Scheduling

### 2. Directed Acyclic Graphs (DAGs)
Directed graphs with no cycles. Used for:
- Task scheduling
- Dependency resolution
- Version control systems

### 3. Flow Networks
Directed graphs where edges have capacities. Used for:
- Network traffic modeling
- Airline scheduling
- Resource distribution

## 🧠 Summary Table – Graphs

| Concept | Description |
|---------|-------------|
| Type | Directed, Undirected, Weighted, etc. |
| Real-life Uses | Maps, Networks, Social Graphs |
| Representations | Adjacency List / Matrix / Edge List |
| Key Algorithms | DFS, BFS, Dijkstra's, A* |
| Strength | Powerful for **interconnected data** |
| Weakness | Can be complex to implement/optimize |
| Space Complexity | O(V + E) for adjacency list, O(V²) for matrix |
| Time Complexity | Varies by algorithm and representation |

## 🔗 Relationship to Other Data Structures

| Data Structure | Relationship to Graphs |
|----------------|------------------------|
| **Trees** | Special type of graph (acyclic, connected) |
| **Heaps** | Used in graph algorithms (e.g., Dijkstra's) |
| **Hash Tables** | Used to implement adjacency lists efficiently |
| **Queues/Stacks** | Used in BFS/DFS traversals |
| **Disjoint Sets** | Used in minimum spanning tree algorithms |

## 📊 Graph Visualization Techniques

- **Force-directed layout**: Positions nodes based on physical simulation
- **Circular layout**: Arranges nodes in a circle
- **Hierarchical layout**: Shows parent-child relationships (good for DAGs)
- **Matrix visualization**: Shows connections in a grid
- **Sankey diagrams**: Shows flow between nodes

## 🧪 Real-World Graph Algorithms in Action

### 1. PageRank
Used by Google to rank web pages based on link importance.

### 2. Facebook's Graph Search
Uses graph structure to find connections between people and interests.

### 3. Netflix Recommendation Engine
Uses graph-based collaborative filtering to suggest content.

### 4. Uber's Route Optimization
Uses graph algorithms to find optimal routes for drivers.

### 5. LinkedIn's "People You May Know"
Uses graph traversal to suggest new connections.
