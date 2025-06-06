# Lecture Notes on Sorting Algorithms and Union-Find Data Structure

## Overview
This document covers fundamental algorithms in computer science: three popular sorting algorithms (Bubble Sort, Heap Sort, and Quick Sort) and the Union-Find data structure. Each algorithm has its own strengths and weaknesses, making them suitable for different scenarios.

## Part I: Sorting Algorithms

### 1. Bubble Sort
#### Implementation
Bubble Sort is a simple comparison-based sorting algorithm. It repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. The process is repeated until the list is sorted.

#### Time & Space Complexity
- **Time Complexity**: O(n²) average and worst case, O(n) best case with optimization
- **Space Complexity**: O(1) - in-place sorting
- **Stability**: Stable (maintains relative order of equal elements)

#### When to Use
- Best for small datasets or when the data is nearly sorted.
- Easy to implement and understand.
- Educational purposes where simplicity matters more than efficiency.

#### Practical Applications
- Educational purposes to teach sorting concepts.
- Embedded systems with severe memory constraints.
- Simple data validation where performance isn't critical.

### 2. Heap Sort
#### Implementation
Heap Sort is a comparison-based sorting algorithm that uses a binary heap data structure. It first builds a max heap from the input data, then repeatedly extracts the maximum element from the heap and rebuilds the heap until the array is sorted.

#### Time & Space Complexity
- **Time Complexity**: O(n log n) in all cases - guaranteed performance
- **Space Complexity**: O(1) - in-place sorting
- **Stability**: Not stable

#### When to Use
- Suitable for large datasets.
- When guaranteed O(n log n) performance is required.
- Memory-constrained environments.
- Real-time systems requiring predictable performance.

#### Practical Applications
- Used in applications where memory usage is a concern, as it sorts in-place.
- Operating system job scheduling algorithms.
- Priority queue implementations.
- Safety-critical systems requiring predictable timing.

### 3. Quick Sort
#### Implementation
Quick Sort is a highly efficient sorting algorithm that uses a divide-and-conquer approach. It selects a 'pivot' element from the array and partitions the other elements into two sub-arrays according to whether they are less than or greater than the pivot. The sub-arrays are then sorted recursively.

#### Time & Space Complexity
- **Time Complexity**: O(n log n) average case, O(n²) worst case
- **Space Complexity**: O(log n) due to recursion stack
- **Stability**: Not stable

#### When to Use
- Generally faster than other O(n log n) algorithms like Heap Sort and Merge Sort.
- Effective for large datasets with random distribution.
- When average-case performance is critical.

#### Practical Applications
- Widely used in various applications, including database sorting and in-memory sorting.
- Java's `Arrays.sort()` for primitive types.
- C++ standard library implementations.
- Graphics programming for z-buffer sorting.


| Algorithm | Time Complexity | Space | Stability | Best Use Case |
|-----------|----------------|-------|-----------|---------------|
| Bubble Sort | O(n²) avg/worst | O(1) | Stable | Education, tiny datasets |
| Quick Sort | O(n log n) avg | O(log n) | Unstable | General purpose, large random data |
| Heap Sort | O(n log n) all | O(1) | Unstable | Guaranteed performance |

## Conclusion
Understanding these fundamental algorithms is crucial for software development and problem-solving:

- **Sorting algorithms** each have specific strengths: Bubble Sort for education, Quick Sort for general use, and Heap Sort for guaranteed performance.

