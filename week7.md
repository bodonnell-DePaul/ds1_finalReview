# csc402_week7

# Heaps
1. Definition and Properties
 - Heap Structure: A heap is a specialized tree-based data structure that satisfies the heap property. In a max-heap, the parent node is always greater than or equal to its children, while in a min-heap, the parent node is always less than or equal to its children.
 - Complete Binary Tree: Heaps are complete binary trees, meaning all levels are fully filled except possibly the last level, which is filled from left to right.
2. Types of Heaps
 - Max-Heap: Used when the highest priority element is needed first. The root node contains the maximum value.
 - Min-Heap: Used when the lowest priority element is needed first. The root node contains the minimum value.
3. Use Cases of Heaps
 - Priority Queues: Heaps are commonly used to implement priority queues, where elements are processed based on their priority.
 - Graph Algorithms: Heaps are used in algorithms like Dijkstra's shortest path and Prim's minimum spanning tree to efficiently select the next node or edge.
 - Heapsort: A sorting algorithm that uses a heap to sort elements in \(O(n \log n)\) time.
 - Median Maintenance: Heaps can be used to dynamically maintain the median of a stream of numbers by using two heaps (a max-heap for the lower half and a min-heap for the upper half).
 - Memory Management: Heaps are used in memory management systems for efficient allocation and deallocation of memory blocks.
4. Importance of Heaps
 - Efficiency: Heaps provide efficient insertion and extraction of the minimum or maximum element, with both operations having a time complexity of \(O(\log n)\).
 - Versatility: Heaps can be used in a variety of applications, from implementing priority queues to sorting algorithms and graph traversal.
 - Dynamic Data: Heaps are well-suited for applications where data is dynamically changing, such as real-time systems and streaming data.
5. When to Use Heaps Over Other Data Structures
 - Priority Management: When you need to manage elements with priorities, heaps are more efficient than other data structures like arrays or linked lists.
 - Dynamic Sorting: When you need to maintain a dynamically changing set of elements in sorted order, heaps provide better performance than other sorting methods.
 - Graph Algorithms: For algorithms that require efficient extraction of the minimum or maximum element, heaps are the preferred choice.
 - Real-Time Systems: In systems where tasks need to be processed based on priority in real-time, heaps provide the necessary efficiency and performance.
## Example Scenarios
 - Priority Queue Implementation
 - Task Scheduling in Operating Systems: Heaps are used to manage tasks based on their priority, ensuring that high-priority tasks are executed before lower-priority ones.
 - Graph Algorithms
 - Dijkstra's Algorithm: Heaps are used to efficiently find the shortest path in a graph by always selecting the node with the smallest tentative distance.
## Sorting
 - Heapsort: Heaps are used to sort elements efficiently, with a time complexity of \(O(n \log n)\).
 - Real-Time Data Processing
 - Median Maintenance: Heaps are used to dynamically maintain the median of a stream of numbers, which is useful in real-time data processing applications.

## Java Code Examples
Priority Queue Example Using Heaps
```java
import java.util.PriorityQueue;

public class PriorityQueueExample {
    public static void main(String[] args) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();

        pq.add(10);
        pq.add(20);
        pq.add(15);

        System.out.println("Priority Queue top element: " + pq.peek());
        System.out.println("Removed element: " + pq.poll());
        System.out.println("Priority Queue top element after removal: " + pq.peek());
    }
}

```
#### Heapsort Example
```java
public class HeapSort {
    public void sort(int arr[]) {
        int n = arr.length;

        for (int i = n / 2 - 1; i >= 0; i--)
            heapify(arr, n, i);

        for (int i = n - 1; i > 0; i--) {
            int temp = arr[0];
            arr[0] = arr[i];
            arr[i] = temp;

            heapify(arr, i, 0);
        }
    }

    void heapify(int arr[], int n, int i) {
        int largest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;

        if (left < n && arr[left] > arr[largest])
            largest = left;

        if (right < n && arr[right] > arr[largest])
            largest = right;

        if (largest != i) {
            int swap = arr[i];
            arr[i] = arr[largest];
            arr[largest] = swap;

            heapify(arr, n, largest);
        }
    }

    static void printArray(int arr[]) {
        int n = arr.length;
        for (int i = 0; i < n; ++i)
            System.out.print(arr[i] + " ");
        System.out.println();
    }

    public static void main(String args[]) {
        int arr[] = {12, 11, 13, 5, 6, 7};
        int n = arr.length;

        HeapSort ob = new HeapSort();
        ob.sort(arr);

        System.out.println("Sorted array is");
        printArray(arr);
    }
}

```

# Priority Queues
1. Definition and Properties
 - Priority Queue Structure: A priority queue is an abstract data type where each element has a priority associated with it Elements are dequeued in order of their priority, not just their insertion order.
 - Heap Implementation: Priority queues are often implemented using heaps (binary heaps, Fibonacci heaps, etc.) because heaps provide efficient insertion and extraction of the minimum or maximum element.
2. Key Operations
 - Insert: Add an element with a priority.
 - Extract-Min/Max: Remove and return the element with the highest/lowest priority.
 - Peek: Return the element with the highest/lowest priority without removing it.
3. Use Cases of Priority Queues
 - Task Scheduling: Operating systems use priority queues to manage the execution of processes, ensuring that high-priority processes are executed before lower-priority ones.
 - Event Management: In simulations and event-driven systems, priority queues manage events based on their scheduled times.
 - Graph Algorithms: Algorithms like Dijkstra's shortest path and Prim's minimum spanning tree use priority queues to efficiently select the next node or edge.
 - Network Traffic Management: Routers and switches use priority queues to manage network traffic, giving higher priority to critical data packets.
 - Job Scheduling in Print Queues: Print servers use priority queues to manage print jobs, ensuring that high-priority documents are printed first.
 - Real-Time Systems: In real-time systems, priority queues ensure that critical tasks are processed within their time constraints.
4. Importance of Priority Queues
 - Efficiency: Priority queues provide efficient insertion and extraction of elements based on priority, with both operations typically having a time complexity of \(O(n log n)\).
 - Fairness: They ensure that high-priority tasks or elements are processed before lower-priority ones, which is crucial in many applications like task scheduling and network traffic management.
 - Versatility: Priority queues can be used in a variety of applications, from operating systems to network management and real-time systems.
5. When to Use Priority Queues Over Other Data Structures
 - Priority Management: When you need to manage elements with priorities, priority queues are more efficient than other data structures like arrays or linked lists.
 - Dynamic Sorting: When you need to maintain a dynamically changing set of elements in sorted order based on priority, priority queues provide better performance than other sorting methods.
 - Graph Algorithms: For algorithms that require efficient extraction of the minimum or maximum element, priority queues are the preferred choice.
 - Real-Time Systems: In systems where tasks need to be processed based on priority in real-time, priority queues provide the necessary efficiency and performance.

## Example Scenarios
 - Task Scheduling in Operating Systems
 - - Example: An operating system uses a priority queue to manage the execution of processes. High-priority processes, such as system-critical tasks, are executed before lower-priority user applications.
 - Network Traffic Management
 - - Example: A router uses a priority queue to manage network packets. Critical data packets, such as those for VoIP or video streaming, are given higher priority over less critical data, like emails or file downloads.
 - Graph Algorithms
 - - Example: Dijkstra's algorithm uses a priority queue to efficiently find the shortest path in a graph by always selecting the node with the smallest tentative distance.

## Java Code Examples
Priority Queue Example Using Heaps
```java
import java.util.PriorityQueue;

public class PriorityQueueExample {
    public static void main(String[] args) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();

        pq.add(10);
        pq.add(20);
        pq.add(15);

        System.out.println("Priority Queue top element: " + pq.peek());
        System.out.println("Removed element: " + pq.poll());
        System.out.println("Priority Queue top element after removal: " + pq.peek());
    }
}
```
```java
import java.util.PriorityQueue;

class Task implements Comparable<Task> {
    String name;
    int priority;

    public Task(String name, int priority) {
        this.name = name;
        this.priority = priority;
    }

    @Override
    public int compareTo(Task other) {
        return Integer.compare(other.priority, this.priority); // Higher priority first
    }

    @Override
    public String toString() {
        return "Task{name='" + name + "', priority=" + priority + '}';
    }
}

public class TaskScheduling {
    public static void main(String[] args) {
        PriorityQueue<Task> taskQueue = new PriorityQueue<>();

        taskQueue.add(new Task("Task1", 3));
        taskQueue.add(new Task("Task2", 1));
        taskQueue.add(new Task("Task3", 2));

        while (!taskQueue.isEmpty()) {
            System.out.println("Processing: " + taskQueue.poll());
        }
    }
}
```

Heapsort
1. Definition and Properties
- Heapsort Algorithm: Heapsort is a comparison-based sorting algorithm that uses a binary heap data structure to sort elements. It is an in-place algorithm, meaning it requires only a constant amount of additional space.
- Heap Property: Heapsort leverages the heap property, where in a max-heap, the parent node is always greater than or equal to its children, and in a min-heap, the parent node is always less than or equal to its children.
2. Steps of Heapsort
- Build a Max-Heap: Convert the input array into a max-heap.
- Extract the Maximum Element: Swap the root of the heap (the maximum element) with the last element of the heap and reduce the heap size by one.
- Heapify the Root: Restore the heap property by heapifying the root.
- Repeat: Continue extracting the maximum element and heapifying until the heap size is reduced to one.
3. Use Cases of Heapsort
- Sorting Large Datasets: Heapsort is efficient for sorting large datasets due to its 
```
O(n log n)
```
O(nlogn) time complexity.
- Real-Time Systems: Heapsort's worst-case time complexity is 
```
O(n log n)
```
O(nlogn), making it predictable and suitable for real-time systems where consistent performance is crucial.
- Embedded Systems: Heapsort's in-place sorting capability makes it suitable for systems with limited memory, such as embedded systems.
4. Importance of Heapsort
- Efficiency: Heapsort has a time complexity of 
```
O(n log n)
```
O(nlogn) for all cases (best, average, and worst), making it efficient for large datasets.
- In-Place Sorting: Heapsort requires only a constant amount of additional space, making it memory efficient.
- Stability: While Heapsort is not a stable sorting algorithm (it does not preserve the relative order of equal elements), its efficiency and in-place nature make it valuable in many applications.
5. When to Use Heapsort Over Other Sorting Algorithms
- Consistent Performance: When you need a sorting algorithm with consistent 
```
O(n log n)
```
O(nlogn) performance, regardless of the input data distribution.
- Memory Constraints: When you need an in-place sorting algorithm that does not require additional memory beyond the input array.
- Large Datasets: When sorting large datasets where other algorithms like quicksort may degrade to 
```
O(n2)
O(n 2)
```
in the worst case.
## Example Scenarios
 - Sorting Large Datasets
 - - Example: Heapsort is used to sort large log files or datasets in data processing systems where consistent performance and memory efficiency are required.
 - Real-Time Systems
 - - Example: In real-time systems, Heapsort is used to ensure that sorting operations complete within predictable time bounds, which is crucial for system reliability.
 - Embedded Systems
 - - Example: Heapsort is used in embedded systems with limited memory resources, such as sorting sensor data in IoT devices.
## Java Code Example for Heapsort
```java
public class HeapSort {
    public void sort(int arr[]) {
        int n = arr.length;

        // Build heap (rearrange array)
        for (int i = n / 2 - 1; i >= 0; i--)
            heapify(arr, n, i);

        // One by one extract an element from heap
        for (int i = n - 1; i > 0; i--) {
            // Move current root to end
            int temp = arr[0];
            arr[0] = arr[i];
            arr[i] = temp;

            // call max heapify on the reduced heap
            heapify(arr, i, 0);
        }
    }

    // To heapify a subtree rooted with node i which is an index in arr[]
    void heapify(int arr[], int n, int i) {
        int largest = i; // Initialize largest as root
        int left = 2 * i + 1; // left = 2*i + 1
        int right = 2 * i + 2; // right = 2*i + 2

       
        if (left < n && arr[left] > arr[largest])
            largest = left;

        // If right child is larger than largest so far
        if (right < n && arr[right] > arr[largest])
            largest = right;

        // If largest is not root
        if (largest != i) {
            int swap = arr[i];
            arr[i] = arr[largest];
            arr[largest] = swap;

            // Recursively heapify the affected sub-tree
            heapify(arr, n, largest);
        }
    }

    // A utility function to print array of size n
    static void printArray(int arr[]) {
        int n = arr.length;
        for (int i = 0; i < n; ++i)
            System.out.print(arr[i] + " ");
        System.out.println();
    }

    // Driver program
    public static void main(String args[]) {
        int arr[] = {12, 11, 13, 5, 6, 7};
        int n = arr.length;

        HeapSort ob = new HeapSort();
        ob.sort(arr);

        System.out.println("Sorted array is");
        printArray(arr);
   }

```