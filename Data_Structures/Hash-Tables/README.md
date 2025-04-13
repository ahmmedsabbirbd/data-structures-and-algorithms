# 🧠 Hash Tables / Hash Maps / Dictionaries

## 🧾 Basic Definition (Same thing, different names)

| Term | Language / Context |
|------|-------------------|
| **Hash Table** | General CS term |
| **Hash Map** | Java, Go, PHP |
| **Dictionary** | Python, C# |
| **Object** | JavaScript (kind of) |
| **Associative Array** | PHP, Perl |

All of these:
* Use a **key-value** structure
* Use a **hashing function** to quickly find, add, delete values

## 🧠 Concept (How It Works)

* Key is passed through a **hash function**
* Hash function gives a **unique index**
* Value is **stored at that index**

Example:

```javascript
{ "name": "Sabbir", "age": 22, "skill": "Frontend Dev" }
```

Here, `"name"`, `"age"`, and `"skill"` are keys. They are hashed internally and point to memory locations.

## 📚 Real-Life Analogy

| Real Life | Hash Table Concept |
|-----------|-------------------|
| Dictionary book | Word = Key, Meaning = Value |
| Student Roll Book | Roll = Key, Name = Value |
| Phone Contacts | Name = Key, Number = Value |

## ⚙️ Core Operations (Very Fast!)

| Operation | Time Complexity |
|-----------|----------------|
| Insert | O(1) average |
| Delete | O(1) average |
| Lookup | O(1) average |

## 🧪 Differences Between Hash Table / Map / Dictionary

| Feature | Hash Table | Hash Map | Dictionary |
|---------|------------|----------|------------|
| Thread Safe | Yes (Java) | No (Java) | No (Python) |
| Allows `null` keys? | No | Yes (1 key) | Yes (Python) |
| Synchronized? | Yes | No | No |
| Used In | Java | Java, Go | Python, C#, Swift |

## 🧰 Use Cases

| Use Case | How It Helps |
|----------|--------------|
| Fast Lookups | Like cache or key-value storage |
| Counting Frequency | Word counter, tag counter |
| Avoid Duplicates | Check if item already exists |
| Graphs | Adjacency lists |
| Caching | Store recent results (LRU Cache) |

## 🧑‍💻 Language Examples

### 🔹 JavaScript

```javascript
// Using Map
const map = new Map();
map.set("name", "Sabbir");
map.get("name"); // Sabbir

// Using Object
const obj = {
  name: "Sabbir",
  age: 22
};
console.log(obj.name); // Sabbir
```

### 🔹 Python

```python
user = {"name": "Sabbir", "age": 22}
print(user["name"])  # Sabbir

# Using dict methods
user.get("name")  # Sabbir
user.keys()       # dict_keys(['name', 'age'])
user.values()     # dict_values(['Sabbir', 22])
```

### 🔹 PHP

```php
$user = ["name" => "Sabbir", "age" => 22];
echo $user["name"];  // Sabbir
```

### 🔹 Java

```java
// HashMap
HashMap<String, String> user = new HashMap<>();
user.put("name", "Sabbir");
user.put("age", "22");
System.out.println(user.get("name"));  // Sabbir

// Hashtable (thread-safe)
Hashtable<String, String> userTable = new Hashtable<>();
userTable.put("name", "Sabbir");
System.out.println(userTable.get("name"));  // Sabbir
```

### 🔹 C#

```csharp
// Dictionary
Dictionary<string, string> user = new Dictionary<string, string>();
user.Add("name", "Sabbir");
user.Add("age", "22");
Console.WriteLine(user["name"]);  // Sabbir
```

## 🔐 Hash Collision & Handling

Sometimes two keys can hash to same index. We solve it using:

* **Chaining** (store values in a linked list at that index)
* **Open Addressing** (find another free slot)
  * Linear Probing: Check next available slot
  * Quadratic Probing: Check slots at quadratic distances
  * Double Hashing: Use second hash function to determine the step size

## 🛠️ Custom Hash Table Implementation

Here's a simple hash table implementation in JavaScript:

```javascript
class HashTable {
  constructor(size = 53) {
    this.keyMap = new Array(size);
  }
  
  _hash(key) {
    let total = 0;
    const PRIME = 31;
    
    // Loop through first 100 chars of key to create hash
    for (let i = 0; i < Math.min(key.length, 100); i++) {
      const char = key[i];
      const value = char.charCodeAt(0) - 96;
      total = (total * PRIME + value) % this.keyMap.length;
    }
    
    return total;
  }
  
  set(key, value) {
    const index = this._hash(key);
    
    if (!this.keyMap[index]) {
      this.keyMap[index] = [];
    }
    
    // Check if key already exists
    for (let i = 0; i < this.keyMap[index].length; i++) {
      if (this.keyMap[index][i][0] === key) {
        this.keyMap[index][i][1] = value;
        return;
      }
    }
    
    // Add new key-value pair
    this.keyMap[index].push([key, value]);
  }
  
  get(key) {
    const index = this._hash(key);
    
    if (!this.keyMap[index]) return undefined;
    
    for (let i = 0; i < this.keyMap[index].length; i++) {
      if (this.keyMap[index][i][0] === key) {
        return this.keyMap[index][i][1];
      }
    }
    
    return undefined;
  }
  
  keys() {
    const keysArr = [];
    
    for (let i = 0; i < this.keyMap.length; i++) {
      if (this.keyMap[i]) {
        for (let j = 0; j < this.keyMap[i].length; j++) {
          keysArr.push(this.keyMap[i][j][0]);
        }
      }
    }
    
    return keysArr;
  }
  
  values() {
    const valuesArr = [];
    const uniqueValues = {};
    
    for (let i = 0; i < this.keyMap.length; i++) {
      if (this.keyMap[i]) {
        for (let j = 0; j < this.keyMap[i].length; j++) {
          const value = this.keyMap[i][j][1];
          
          if (!uniqueValues[value]) {
            uniqueValues[value] = true;
            valuesArr.push(value);
          }
        }
      }
    }
    
    return valuesArr;
  }
}

// Usage
const ht = new HashTable();
ht.set("name", "Sabbir");
ht.set("age", 22);
console.log(ht.get("name"));  // Sabbir
console.log(ht.keys());       // ["name", "age"]
```

## 🔍 Building a Simple Cache System

Using hash tables to build a basic LRU (Least Recently Used) cache:

```javascript
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.cache = new Map();  // Using Map for O(1) operations and tracking insertion order
  }
  
  get(key) {
    if (!this.cache.has(key)) return -1;
    
    // Update access by deleting and re-adding
    const value = this.cache.get(key);
    this.cache.delete(key);
    this.cache.set(key, value);
    
    return value;
  }
  
  put(key, value) {
    // If key exists, remove it first
    if (this.cache.has(key)) {
      this.cache.delete(key);
    }
    
    // If cache is full, remove least recently used item (first item)
    if (this.cache.size >= this.capacity) {
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }
    
    // Add new key
    this.cache.set(key, value);
  }
}

// Usage
const cache = new LRUCache(2);
cache.put(1, 1);      // cache = {1=1}
cache.put(2, 2);      // cache = {1=1, 2=2}
console.log(cache.get(1));  // returns 1
cache.put(3, 3);      // evicts key 2, cache = {1=1, 3=3}
console.log(cache.get(2));  // returns -1 (not found)
```

## 🚀 Fast Search Implementation

Simple autocomplete system using a hash table:

```javascript
class SearchEngine {
  constructor() {
    this.wordMap = new Map();
  }
  
  // Index a document with words
  indexDocument(docId, text) {
    const words = text.toLowerCase().split(/\W+/);
    
    words.forEach(word => {
      if (word.length < 2) return;  // Skip short words
      
      if (!this.wordMap.has(word)) {
        this.wordMap.set(word, new Set());
      }
      
      this.wordMap.get(word).add(docId);
    });
  }
  
  // Search for documents containing the word
  search(query) {
    const searchWord = query.toLowerCase();
    
    if (!this.wordMap.has(searchWord)) {
      return [];
    }
    
    return Array.from(this.wordMap.get(searchWord));
  }
  
  // Get prefix-based suggestions (autocomplete)
  getSuggestions(prefix, limit = 5) {
    prefix = prefix.toLowerCase();
    const suggestions = [];
    
    for (const [word] of this.wordMap) {
      if (word.startsWith(prefix)) {
        suggestions.push(word);
        if (suggestions.length >= limit) break;
      }
    }
    
    return suggestions;
  }
}

// Usage
const search = new SearchEngine();
search.indexDocument(1, "Hash tables are fast data structures");
search.indexDocument(2, "Hash functions map keys to array indices");
search.indexDocument(3, "Tables help organize data in rows and columns");

console.log(search.search("hash"));        // [1, 2]
console.log(search.search("tables"));      // [1, 3]
console.log(search.getSuggestions("ha"));  // ["hash"]
```

## 🧠 Summary Table

| Feature | Description |
|---------|-------------|
| Structure | Key → Value |
| Speed | Super fast (O(1)) |
| Real Use | Cache, Lookup, Storage, Mapping |
| Example Use | `{"email": "user@example.com"}` |
| Nicknames | Map, Dict, Hash, Assoc. Array, Object |

## 📈 Hash Tables in Time & Space Complexity

| Operation | Average | Worst |
|-----------|---------|-------|
| Space | O(n) | O(n) |
| Search | O(1) | O(n) |
| Insert | O(1) | O(n) |
| Delete | O(1) | O(n) |

The worst-case O(n) time complexity occurs when there are many collisions, which is rare with good hash functions.

## 🧩 Advanced Hash Table Concepts

### Load Factor
The load factor is the ratio of elements to bucket count. When the load factor exceeds a certain threshold (typically 0.7), many implementations automatically resize the hash table to maintain performance.

```
Load Factor = Number of Elements / Number of Buckets
```

### Cryptographic Hash Functions
While regular hash tables use simple hashing functions optimized for speed, cryptographic applications use stronger hash functions like SHA-256 that:
- Are one-way (impossible to reverse)
- Have avalanche effect (small changes cause large differences in output)
- Are collision-resistant

### Perfect Hashing
A perfect hash function maps each key to a unique integer without collisions, which is useful for static sets of keys (like keywords in programming languages).

## 🌐 Hash Tables in Modern Applications

1. **Database Indexing**: Hash indexes in databases
2. **Caching Systems**: Redis, Memcached use hash tables
3. **Language Runtimes**: JavaScript object implementation
4. **Blockchain**: Merkle trees and transaction verification
5. **Network Routing**: IP address lookup tables

## 🔗 Hash Tables vs. Other Data Structures

| Data Structure | Advantages Over Hash Tables | Disadvantages |
|----------------|----------------------------|---------------|
| Arrays | Ordered, direct access by index | Slow inserts/deletes |
| Linked Lists | Dynamic size, fast inserts | Slow lookup O(n) |
| Trees (BST) | Ordered data, sorted ops | Slower than hash tables O(log n) |
| Tries | Prefix searching | Uses more memory |
