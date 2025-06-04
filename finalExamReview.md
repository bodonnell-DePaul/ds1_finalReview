Here are 30 multiple choice exam questions with 50% code-based questions, testing conceptual understanding without referencing notes:

## **WEEKS 1-4 + MIDTERM REVIEW (Questions 1-11, 35%)**

### Question 1: Code Analysis - Array Operations
What is the output of this code?
```java
int[] arr = {5, 2, 8, 1, 9};
for (int i = 0; i < arr.length - 1; i++) {
    if (arr[i] > arr[i + 1]) {
        int temp = arr[i];
        arr[i] = arr[i + 1];
        arr[i + 1] = temp;
    }
}
System.out.println(Arrays.toString(arr));
```

A) `[2, 5, 1, 8, 9]`  
B) `[5, 2, 1, 8, 9]`  
C) `[2, 5, 8, 1, 9]`  
D) `[1, 2, 5, 8, 9]`  
E) `[5, 2, 8, 1, 9]`

---

### Question 2: Interface Implementation
Which statement about Java interfaces is TRUE?

A) A class can only implement one interface  
B) Interfaces can have constructors  
C) All methods in interfaces must be abstract  
D) A class implementing an interface must provide implementations for all abstract methods  
E) Interfaces can contain instance variables

---

### Question 3: Code Analysis - Recursion
What does this recursive method calculate?
```java
public static int mystery(int n) {
    if (n <= 1) return 1;
    return n * mystery(n - 1);
}
```

A) Sum of numbers from 1 to n  
B) n factorial (n!)  
C) Powers of 2 up to n  
D) Fibonacci sequence  
E) Prime numbers up to n

---

### Question 4: Code Debugging - Custom ArrayList
What is wrong with this ArrayList add method?
```java
public void add(T item) {
    if (size >= array.length) {
        resize();
    }
    array[size] = item;
    // Missing: size++;
}
```

A) Array bounds checking is incorrect  
B) Resize method is called too early  
C) Missing size increment  
D) Item assignment is wrong  
E) Generic type is incorrect

---

### Question 5: Code Analysis - Stack Operations
What is the final state of the stack after these operations?
```java
Stack<Integer> stack = new Stack<>();
stack.push(10);
stack.push(20);
stack.push(30);
stack.pop();
stack.push(40);
stack.pop();
```

A) Stack contains: [10, 20]  
B) Stack contains: [10, 20, 30]  
C) Stack contains: [20, 40]  
D) Stack contains: [10, 40]  
E) Stack is empty

---

### Question 6: Data Structure Selection
For implementing browser history with back/forward functionality, which combination of data structures is most appropriate?

A) One ArrayList  
B) Two Stacks  
C) One Queue  
D) One LinkedList  
E) One HashMap

---

### Question 7: Code Analysis - Hash Table
What is the time complexity of this hash table lookup?
```java
public V get(K key) {
    int index = hash(key);
    Node current = buckets[index];
    while (current != null) {
        if (current.key.equals(key)) {
            return current.value;
        }
        current = current.next;
    }
    return null;
}
```

A) Always O(1)  
B) Always O(n)  
C) O(1) average, O(n) worst case  
D) O(log n)  
E) O(n log n)

---

### Question 8: Code Debugging - Collision Resolution
This hash table uses which collision resolution strategy?
```java
private Node[] buckets;
public void put(K key, V value) {
    int index = hash(key);
    Node newNode = new Node(key, value);
    newNode.next = buckets[index];
    buckets[index] = newNode;
}
```

A) Open addressing  
B) Linear probing  
C) Quadratic probing  
D) Separate chaining  
E) Double hashing

---

### Question 9: Code Analysis - Path Finding
What does this recursive method determine?
```java
public boolean findPath(int[][] maze, int row, int col) {
    if (row < 0 || col < 0 || row >= maze.length || col >= maze[0].length) 
        return false;
    if (maze[row][col] == 1) return false;
    if (row == maze.length-1 && col == maze[0].length-1) return true;
    
    return findPath(maze, row+1, col) || findPath(maze, row, col+1);
}
```

A) If a path exists from top-left to bottom-right  
B) The shortest path length  
C) All possible paths  
D) If the maze is valid  
E) The number of obstacles

---

### Question 10: Code Analysis - Doubly Linked List
What operation does this method perform?
```java
public void operation(Node node) {
    if (node.prev != null) {
        node.prev.next = node.next;
    } else {
        head = node.next;
    }
    if (node.next != null) {
        node.next.prev = node.prev;
    } else {
        tail = node.prev;
    }
    size--;
}
```

A) Insert a node  
B) Remove a node  
C) Find a node  
D) Update a node's value  
E) Swap two nodes

---

### Question 11: Queue Implementation Strategy
For a customer service call center queue, which principle should be followed?

A) LIFO (Last In, First Out)  
B) FIFO (First In, First Out)  
C) Priority-based ordering  
D) Random selection  
E) Alphabetical ordering

---

## **WEEKS 5-8 (Questions 12-30, 65%)**

### Question 12: Code Analysis - Binary Search
What is the output when searching for 7?
```java
public static int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    while (left <= right) {
        int mid = (left + right) / 2;
        if (arr[mid] == target) return mid;
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
// Called with: binarySearch(new int[]{1, 3, 5, 7, 9, 11}, 7)
```

A) -1  
B) 2  
C) 3  
D) 7  
E) 4

---

### Question 13: Time Complexity Analysis
What is the time complexity of this subset generation algorithm?
```java
public void generateSubsets(int[] arr, int index, List<Integer> current) {
    if (index == arr.length) {
        System.out.println(current);
        return;
    }
    generateSubsets(arr, index + 1, current);
    current.add(arr[index]);
    generateSubsets(arr, index + 1, current);
    current.remove(current.size() - 1);
}
```

A) O(n)  
B) O(n²)  
C) O(n log n)  
D) O(2^n)  
E) O(n!)

---

### Question 14: Algorithm Classification
Dynamic programming is most effective for problems that exhibit:

A) Overlapping subproblems and optimal substructure  
B) Random data access patterns  
C) Linear time complexity requirements  
D) Constant space complexity needs  
E) Parallel processing capabilities

---

### Question 15: Code Analysis - Merge Sort
What does this merge function accomplish?
```java
public void merge(int[] arr, int left, int mid, int right) {
    int[] temp = new int[right - left + 1];
    int i = left, j = mid + 1, k = 0;
    
    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) temp[k++] = arr[i++];
        else temp[k++] = arr[j++];
    }
    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];
    
    System.arraycopy(temp, 0, arr, left, temp.length);
}
```

A) Splits array into two parts  
B) Finds the minimum element  
C) Merges two sorted subarrays into one sorted array  
D) Reverses the array order  
E) Removes duplicates

---

### Question 16: Code Debugging - BST Insertion
What is wrong with this BST insert method?
```java
public Node insert(Node root, int value) {
    if (root == null) return new Node(value);
    
    if (value < root.value) {
        root.left = insert(root.left, value);
    } else {
        root.right = insert(root.right, value);
    }
}
```

A) Base case is incorrect  
B) Comparison logic is wrong  
C) Missing return statement  
D) Recursive calls are incorrect  
E) Node creation is wrong

---

### Question 17: BST Performance Analysis
In the worst case, a Binary Search Tree can degrade to what time complexity for search operations?

A) O(1)  
B) O(log n)  
C) O(n)  
D) O(n log n)  
E) O(n²)

---

### Question 18: Code Analysis - Heap Operations
What operation does this method perform on a max-heap?
```java
public int operation() {
    if (size == 0) throw new RuntimeException("Empty heap");
    int max = heap[0];
    heap[0] = heap[--size];
    percolateDown(0);
    return max;
}
```

A) Insert an element  
B) Find maximum without removing  
C) Extract maximum element  
D) Increase key value  
E) Build heap from array

---

### Question 19: Code Analysis - Priority Queue Usage
What does this code simulate?
```java
PriorityQueue<Task> pq = new PriorityQueue<>((a, b) -> a.priority - b.priority);
pq.offer(new Task("Email", 3));
pq.offer(new Task("Meeting", 1));
pq.offer(new Task("Report", 2));

while (!pq.isEmpty()) {
    System.out.println(pq.poll().name);
}
```

A) Task scheduling by arrival time  
B) Task scheduling by priority (lowest first)  
C) Random task execution  
D) Alphabetical task sorting  
E) Task scheduling by duration

---

### Question 20: Heap Property Understanding
In a min-heap implementation, which property must be maintained?

A) Parent nodes are greater than children  
B) Parent nodes are less than or equal to children  
C) Left children are always smaller than right children  
D) All leaves are at the same level  
E) The tree must be a complete binary tree

---

### Question 21: Code Debugging - Heap Insert
What is missing from this heap insertion?
```java
public void insert(int value) {
    if (size == capacity) resize();
    heap[size] = value;
    percolateUp(size);
}
```

A) Capacity check is wrong  
B) Array assignment is incorrect  
C) Missing size increment  
D) PercolateUp call is wrong  
E) Value parameter is wrong

---

### Question 22: Algorithm Application
Which data structure is most suitable for implementing a hospital emergency room triage system?

A) Stack  
B) Queue  
C) Priority Queue  
D) Hash Table  
E) Binary Search Tree

---

### Question 23: Code Analysis - Quick Sort Partition
What does this partition function accomplish?
```java
public int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    
    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr, i, j);
        }
    }
    swap(arr, i + 1, high);
    return i + 1;
}
```

A) Finds the minimum element  
B) Sorts the entire array  
C) Partitions array around pivot element  
D) Merges two sorted arrays  
E) Reverses array elements

---

### Question 24: Code Analysis - Bubble Sort Optimization
What optimization does this bubble sort implementation include?
```java
public void bubbleSort(int[] arr) {
    boolean swapped;
    for (int i = 0; i < arr.length - 1; i++) {
        swapped = false;
        for (int j = 0; j < arr.length - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr, j, j + 1);
                swapped = true;
            }
        }
        if (!swapped) break;
    }
}
```

A) Early termination when no swaps occur  
B) Reduces number of comparisons by half  
C) Uses binary search for positioning  
D) Implements merge sort logic  
E) No optimization present

---

### Question 25: Sorting Algorithm Characteristics
Which sorting algorithm maintains the relative order of equal elements (stable)?

A) Quick Sort  
B) Heap Sort  
C) Selection Sort  
D) Merge Sort  
E) All of the above

---

### Question 26: Code Analysis - Space Complexity
What is the space complexity of this merge sort implementation?
```java
public void mergeSort(int[] arr, int left, int right) {
    if (left < right) {
        int mid = (left + right) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right); 
    }
}
```

A) O(1)  
B) O(log n)  
C) O(n)  
D) O(n log n)  
E) O(n²)

---

### Question 27: Algorithm Selection
For sorting a dataset where worst-case performance guarantees are critical, which algorithm is best?

A) Quick Sort  
B) Bubble Sort  
C) Insertion Sort  
D) Heap Sort  
E) Selection Sort

---

### Question 28: Code Debugging - Quick Sort
What could cause this Quick Sort to have O(n²) performance?
```java
public void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int pivotIndex = partition(arr, low, high);
        quickSort(arr, low, pivotIndex - 1);
        quickSort(arr, pivotIndex + 1, high);
    }
}

```

A) Array contains duplicates  
B) Array is already sorted  
C) Array size is too small  
D) Recursive calls are incorrect  
E) Base case is wrong

---

### Question 29: Code Analysis - Tree Traversal
What type of traversal does this method perform?
```java
public void traverse(TreeNode root) {
    if (root != null) {
        System.out.print(root.value + " ");
        traverse(root.left);
        traverse(root.right);
    }
}
```

A) Inorder traversal  
B) Preorder traversal  
C) Postorder traversal  
D) Level-order traversal  
E) Depth-first search

---

### Question 30: System Design Problem
You need to implement a real-time leaderboard system that can quickly insert new scores and retrieve the top 10 players. Which data structure combination is most appropriate?

A) Array with linear search  
B) LinkedList with sorting  
C) HashMap with values sorting  
D) Priority Queue (max-heap)  
E) Binary Search Tree with in-order traversal

---

**Content Distribution:**
- **Weeks 1-4 + Midterm Review:** Questions 1-11 (36.7%)
- **Weeks 5-8:** Questions 12-30 (63.3%)
- **Code-based questions:** 15 out of 30 (50%)