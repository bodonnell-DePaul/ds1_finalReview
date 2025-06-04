# README.md

# Java Sorting Algorithms Lecture

This project provides a comprehensive overview of various sorting algorithms implemented in Java. It covers the following algorithms:

- **Bubble Sort**
- **Heap Sort**
- **Quick Sort**

## Overview of Sorting Algorithms

### Bubble Sort
Bubble Sort is a simple sorting algorithm that repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. The pass through the list is repeated until the list is sorted. 

**When to Use:** 
- Best for small datasets or when the data is nearly sorted.
- Not suitable for large datasets due to its O(n^2) time complexity.

### Heap Sort
Heap Sort is a comparison-based sorting algorithm that uses a binary heap data structure. It first builds a max heap from the input data, then repeatedly extracts the maximum element from the heap and rebuilds the heap until the data is sorted.

**When to Use:** 
- Suitable for large datasets.
- Has a time complexity of O(n log n) and is more efficient than Bubble Sort.

### Quick Sort
Quick Sort is a highly efficient sorting algorithm that uses a divide-and-conquer approach. It selects a 'pivot' element from the array and partitions the other elements into two sub-arrays according to whether they are less than or greater than the pivot. 

**When to Use:** 
- Generally faster in practice than other O(n log n) algorithms like Heap Sort.
- Best for large datasets, but performance can degrade with certain input patterns.

## Practical Applications
Sorting algorithms are fundamental in computer science and are used in various applications, including:
- Organizing data for efficient searching.
- Preparing data for visualization.
- Implementing algorithms that require sorted data, such as binary search.

## Project Structure
The project is organized as follows:

- **src/main/java/com/lecture/sorting/algorithms/**: Contains the implementations of the sorting algorithms.
- **src/main/java/com/lecture/sorting/utils/**: Contains utility classes for sorting and performance analysis.
- **src/main/java/com/lecture/sorting/Main.java**: Entry point for demonstrating the sorting algorithms.
- **src/test/java/com/lecture/sorting/algorithms/**: Contains unit tests for each sorting algorithm.
- **docs/**: Contains lecture notes and performance comparison documents.
- **examples/**: Provides example usage of the sorting algorithms.
- **pom.xml**: Maven configuration file for project dependencies and build settings.

## Conclusion
This project serves as a valuable resource for understanding and implementing sorting algorithms in Java. It provides both theoretical insights and practical implementations, making it suitable for educational purposes and real-world applications.