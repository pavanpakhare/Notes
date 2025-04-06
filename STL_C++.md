# Notes
# C++ Standard Template Library (STL) Tutorial

## Introduction to STL

The Standard Template Library (STL) is a powerful set of C++ template classes that provide general-purpose classes and functions with templates that implement many popular and commonly used algorithms and data structures.

The STL has four main components:
1. Containers
2. Iterators
3. Algorithms
4. Functors (Function Objects)

Let's explore each of these components in detail.

## 1. Containers

Containers are objects that store data. STL provides several container types, which can be divided into three categories:

### Sequence Containers

```cpp
#include <vector>
#include <list>
#include <deque>
#include <array>
#include <forward_list>

void sequenceContainers() {
    // Vector - dynamic array
    std::vector<int> vec = {1, 2, 3};
    vec.push_back(4);  // Add element at end
    
    // List - doubly linked list
    std::list<int> lst = {5, 6, 7};
    lst.push_front(4); // Add element at front
    
    // Deque - double-ended queue
    std::deque<int> dq = {8, 9, 10};
    dq.push_back(11);  // Add at end
    dq.push_front(7);  // Add at front
    
    // Array - fixed size array
    std::array<int, 3> arr = {1, 2, 3};
    
    // Forward list - singly linked list
    std::forward_list<int> flist = {10, 20, 30};
    flist.push_front(5);
}
```

### Associative Containers

```cpp
#include <set>
#include <map>
#include <unordered_set>
#include <unordered_map>

void associativeContainers() {
    // Set - unique elements, sorted
    std::set<int> s = {3, 1, 2, 1};
    // s contains {1, 2, 3}
    
    // Multiset - allows duplicates, sorted
    std::multiset<int> ms = {3, 1, 2, 1};
    // ms contains {1, 1, 2, 3}
    
    // Map - key-value pairs, sorted by key
    std::map<std::string, int> m = {{"Alice", 25}, {"Bob", 30}};
    m["Charlie"] = 35;
    
    // Multimap - allows duplicate keys, sorted
    std::multimap<std::string, int> mm = {{"Alice", 25}, {"Alice", 26}};
    
    // Unordered versions use hash tables
    std::unordered_set<int> us = {3, 1, 2};
    std::unordered_map<std::string, int> um = {{"Alice", 25}, {"Bob", 30}};
}
```

### Container Adapters

```cpp
#include <stack>
#include <queue>

void containerAdapters() {
    // Stack - LIFO
    std::stack<int> stk;
    stk.push(1);
    stk.push(2);
    stk.pop(); // removes 2
    
    // Queue - FIFO
    std::queue<int> q;
    q.push(1);
    q.push(2);
    q.pop(); // removes 1
    
    // Priority queue - highest priority first
    std::priority_queue<int> pq;
    pq.push(3);
    pq.push(1);
    pq.push(4);
    pq.top(); // returns 4
}
```

## 2. Iterators

Iterators are used to traverse through elements of containers.

```cpp
#include <vector>
#include <iostream>

void iteratorExample() {
    std::vector<int> vec = {1, 2, 3, 4, 5};
    
    // Using iterators
    for (auto it = vec.begin(); it != vec.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;
    
    // Reverse iterator
    for (auto it = vec.rbegin(); it != vec.rend(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;
    
    // Const iterator (read-only)
    for (auto it = vec.cbegin(); it != vec.cend(); ++it) {
        std::cout << *it << " ";
        // *it = 10; // Error - can't modify
    }
    std::cout << std::endl;
    
    // Range-based for loop (uses iterators internally)
    for (int num : vec) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
}
```

## 3. Algorithms

STL provides many algorithms that work on containers via iterators.

```cpp
#include <algorithm>
#include <vector>
#include <iostream>

void algorithmExamples() {
    std::vector<int> vec = {5, 3, 1, 4, 2};
    
    // Sorting
    std::sort(vec.begin(), vec.end());
    // vec is now {1, 2, 3, 4, 5}
    
    // Reverse
    std::reverse(vec.begin(), vec.end());
    // vec is now {5, 4, 3, 2, 1}
    
    // Find
    auto it = std::find(vec.begin(), vec.end(), 3);
    if (it != vec.end()) {
        std::cout << "Found 3 at position " << (it - vec.begin()) << std::endl;
    }
    
    // Count
    int count = std::count(vec.begin(), vec.end(), 3);
    std::cout << "3 appears " << count << " times" << std::endl;
    
    // Accumulate (sum)
    int sum = std::accumulate(vec.begin(), vec.end(), 0);
    std::cout << "Sum is " << sum << std::endl;
    
    // Transform
    std::vector<int> squared(vec.size());
    std::transform(vec.begin(), vec.end(), squared.begin(), 
                   [](int x) { return x * x; });
    
    // Remove duplicates (needs sorted container)
    std::sort(vec.begin(), vec.end());
    auto last = std::unique(vec.begin(), vec.end());
    vec.erase(last, vec.end());
}
```

## 4. Functors (Function Objects)

Functors are objects that can be called like functions.

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

// Functor class
struct Square {
    int operator()(int x) const {
        return x * x;
    }
};

void functorExamples() {
    Square square;
    std::cout << "Square of 5: " << square(5) << std::endl;
    
    std::vector<int> vec = {1, 2, 3, 4, 5};
    
    // Using functor with algorithm
    std::transform(vec.begin(), vec.end(), vec.begin(), Square());
    
    // Using lambda expressions (C++11)
    std::transform(vec.begin(), vec.end(), vec.begin(), 
                  [](int x) { return x * x * x; });
    
    // Predicate example
    auto is_even = [](int x) { return x % 2 == 0; };
    int even_count = std::count_if(vec.begin(), vec.end(), is_even);
    std::cout << "Even numbers: " << even_count << std::endl;
}
```

## Practical Examples

### Example 1: Using vector and sorting

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

void vectorSortExample() {
    std::vector<std::string> names = {"Alice", "Bob", "Charlie", "Dave"};
    
    // Sort alphabetically
    std::sort(names.begin(), names.end());
    
    // Sort by length
    std::sort(names.begin(), names.end(), 
             [](const std::string& a, const std::string& b) {
                 return a.length() < b.length();
             });
    
    for (const auto& name : names) {
        std::cout << name << " ";
    }
    std::cout << std::endl;
}
```

### Example 2: Using map to count word frequencies

```cpp
#include <iostream>
#include <map>
#include <string>
#include <vector>

void wordFrequencyCounter() {
    std::vector<std::string> words = {"apple", "banana", "apple", "orange", "banana", "apple"};
    std::map<std::string, int> wordCount;
    
    for (const auto& word : words) {
        wordCount[word]++;
    }
    
    for (const auto& pair : wordCount) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }
}
```

### Example 3: Using priority queue for a to-do list

```cpp
#include <iostream>
#include <queue>
#include <string>

struct Task {
    std::string description;
    int priority;
    
    // Overload < operator for priority queue
    bool operator<(const Task& other) const {
        return priority < other.priority; // Higher priority comes first
    }
};

void todoListExample() {
    std::priority_queue<Task> tasks;
    
    tasks.push({"Fix bug", 3});
    tasks.push({"Implement feature", 1});
    tasks.push({"Write documentation", 2});
    
    while (!tasks.empty()) {
        Task current = tasks.top();
        std::cout << "Priority " << current.priority << ": " 
                  << current.description << std::endl;
        tasks.pop();
    }
}
```

## Best Practices

1. **Choose the right container**: Select containers based on your needs for insertion, deletion, access patterns, and ordering requirements.
2. **Use range-based for loops**: When possible, prefer range-based for loops over explicit iterator loops.
3. **Prefer algorithms over raw loops**: STL algorithms are optimized and make your code more expressive.
4. **Be careful with iterator invalidation**: Some operations (like inserting into a vector) can invalidate iterators.
5. **Use emplace operations**: For containers, prefer `emplace_back()` over `push_back()` when constructing objects in place.
6. **Reserve space for vectors**: If you know the approximate size, use `reserve()` to avoid multiple reallocations.

## Conclusion

The C++ STL is a powerful toolkit that can significantly boost your productivity as a C++ programmer. By understanding and effectively using containers, iterators, algorithms, and functors, you can write more efficient, maintainable, and expressive code.
