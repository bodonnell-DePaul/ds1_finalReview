# csc402_week5

![alt text](image.png)

### O(1) - Constant Time Complexity
- **Accessing an element in an array**: Retrieving a specific element from an array by its index.
- **Hash table operations**: Inserting, deleting, or searching for an element in a hash table.

### O(log n) - Logarithmic Time Complexity
- **Binary search**: Searching for an element in a sorted array.
- **Balanced search trees**: Operations like insertion, deletion, and search in balanced trees (e.g., AVL trees, Red-Black trees).

### O(n) - Linear Time Complexity
- **Linear search**: Searching for an element in an unsorted array.
- **Traversal of a list**: Iterating through all elements of a list or array.

### O(n log n) - Linearithmic Time Complexity
- **Efficient sorting algorithms**: Algorithms like Merge Sort, Quick Sort, and Heap Sort.
- **Fast Fourier Transform (FFT)**: Used in signal processing and image analysis.

### O(n^2) - Quadratic Time Complexity
- **Bubble Sort, Insertion Sort, Selection Sort**: Simple sorting algorithms.
- **Matrix multiplication**: Multiplying two n x n matrices using the naive approach.

### O(2^n) - Exponential Time Complexity
- **Solving the Traveling Salesman Problem (TSP)**: Using a brute-force approach to find the shortest possible route.
- **Solving the Knapsack Problem**: Using a brute-force approach to find the optimal subset of items.

### O(n!) - Factorial Time Complexity
- **Generating permutations**: Finding all possible permutations of a set of elements.
- **Solving the Traveling Salesman Problem (TSP)**: Using a brute-force approach to find the shortest possible route (similar to O(2^n)).

These examples illustrate how different algorithmic complexities are applied in various practical scenarios.

### O(1) - Constant Time Complexity
An algorithm with constant time complexity will always execute in the same time regardless of the size of the input data set.

```java
public class ConstantTime {
    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 5};
        System.out.println(array[0]); // Accessing an element in an array
    }
}
```

### O(log n) - Logarithmic Time Complexity
An algorithm with logarithmic time complexity reduces the problem size by half each time.

```java
import java.util.Arrays;

public class LogarithmicTime {
    public static void main(String[] args) {
        int[] array = new int[100];
        for (int i = 0; i < array.length; i++) {
            array[i] = i + 1;
        }
        int target = 44;
        int index = binarySearch(array, target);
        if (index != -1) {
            System.out.println("Element found at index: " + index);
        } else {
            System.out.println("Element not found");
        }
    }

    public static int binarySearch(int[] array, int target) {
        int left = 0;
        int right = array.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (array[mid] == target) {
                return mid;
            }
            if (array[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1; // Element not found
    }
}
```

### O(n) - Linear Time Complexity
An algorithm with linear time complexity will grow linearly with the size of the input data set.

```java
public class LinearTime {
    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 5};
        for (int i : array) {
            System.out.println(i); // Iterating through an array
        }
    }
}
```

### O(n log n) - Linearithmic Time Complexity
An algorithm with linearithmic time complexity is typically seen in efficient sorting algorithms.

```java
import java.util.Arrays;

public class LinearithmicTime {
    public static void main(String[] args) {
        int[] array = {5, 3, 1, 4, 2};
        mergeSort(array, 0, array.length - 1);
        System.out.println(Arrays.toString(array));
    }

    public static void mergeSort(int[] array, int left, int right) {
        if (left < right) {
            int mid = (left + right) / 2;

            // Recursively sort the left and right halves
            mergeSort(array, left, mid);
            mergeSort(array, mid + 1, right);

            // Merge the sorted halves
            merge(array, left, mid, right);
        }
    }

    public static void merge(int[] array, int left, int mid, int right) {
        // Find sizes of two subarrays to be merged
        int n1 = mid - left + 1;
        int n2 = right - mid;

        // Create temporary arrays
        int[] leftArray = new int[n1];
        int[] rightArray = new int[n2];

        // Copy data to temporary arrays
        for (int i = 0; i < n1; ++i) {
            leftArray[i] = array[left + i];
        }
        for (int j = 0; j < n2; ++j) {
            rightArray[j] = array[mid + 1 + j];
        }

        // Merge the temporary arrays

        // Initial indexes of first and second subarrays
        int i = 0, j = 0;

        // Initial index of merged subarray array
        int k = left;
        while (i < n1 && j < n2) {
            if (leftArray[i] <= rightArray[j]) {
                array[k] = leftArray[i];
                i++;
            } else {
                array[k] = rightArray[j];
                j++;
            }
            k++;
        }

        // Copy remaining elements of leftArray[] if any
        while (i < n1) {
            array[k] = leftArray[i];
            i++;
            k++;
        }

        // Copy remaining elements of rightArray[] if any
        while (j < n2) {
            array[k] = rightArray[j];
            j++;
            k++;
        }
    }
}
```

### O(n^2) - Quadratic Time Complexity
An algorithm with quadratic time complexity is often seen in algorithms with nested loops.

```java
public class QuadraticTime {
    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 5};
        for (int i : array) {
            for (int j : array) {
                System.out.println(i + " " + j); // Nested loops
            }
        }
    }
}
```

### O(2^n) - Exponential Time Complexity
An algorithm with exponential time complexity grows very quickly with the size of the input data set.

```java
public class ExponentialTime {
    public static void main(String[] args) {
        int n = 5;
        System.out.println(fibonacci(n)); // Recursive Fibonacci
    }

    public static int fibonacci(int n) {
        if (n <= 1) return n;
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
}
```
```shell
fibonacci(5)
├── fibonacci(4)
│   ├── fibonacci(3)
│   │   ├── fibonacci(2)
│   │   │   ├── fibonacci(1)
│   │   │   └── fibonacci(0)
│   │   └── fibonacci(1)
│   └── fibonacci(2)
│       ├── fibonacci(1)
│       └── fibonacci(0)
└── fibonacci(3)
    ├── fibonacci(2)
    │   ├── fibonacci(1)
    │   └── fibonacci(0)
    └── fibonacci(1)
```    

### OR
```java
public class ExponentialTimeSubsetSum {
    public static void main(String[] args) {
        int[] set = {3, 34, 4, 12, 5, 2};
        int targetSum = 9;
        if (isSubsetSum(set, set.length, targetSum)) {
            System.out.println("Found a subset with given sum");
        } else {
            System.out.println("No subset with given sum");
        }
    }

    public static boolean isSubsetSum(int[] set, int n, int sum) {
        // Base Cases
        if (sum == 0) return true;
        if (n == 0 && sum != 0) return false;

        // If last element is greater than sum, then ignore it
        if (set[n - 1] > sum) return isSubsetSum(set, n - 1, sum);

        // Check if sum can be obtained by any of the following:
        // (a) including the last element
        // (b) excluding the last element
        return isSubsetSum(set, n - 1, sum) || isSubsetSum(set, n - 1, sum - set[n - 1]);
    }
}
```
```shell
isSubsetSum({3, 34, 4, 12, 5, 2}, 6, 9)
├── isSubsetSum({3, 34, 4, 12, 5}, 5, 9)
│   ├── isSubsetSum({3, 34, 4, 12}, 4, 9)
│   │   ├── isSubsetSum({3, 34, 4}, 3, 9)
│   │   │   ├── isSubsetSum({3, 34}, 2, 9)
│   │   │   │   ├── isSubsetSum({3}, 1, 9)
│   │   │   │   └── isSubsetSum({3}, 1, 5)
│   │   │   └── isSubsetSum({3, 34}, 2, 5)
│   │   └── isSubsetSum({3, 34, 4}, 3, 5)
│   └── isSubsetSum({3, 34, 4, 12}, 4, 4)
└── isSubsetSum({3, 34, 4, 12, 5}, 5, 4)
```

### O(n!) - Factorial Time Complexity
An algorithm with factorial time complexity is often seen in problems involving permutations.

```java
public class FactorialTime {
    public static void main(String[] args) {
        String str = "ABC";
        permute("", str); // Generate all permutations of the string
    }

    public static void permute(String prefix, String str) {
        int n = str.length();
        if (n == 0) {
            System.out.println(prefix); // Print the permutation
        } else {
            for (int i = 0; i < n; i++) {
                permute(prefix + str.charAt(i), str.substring(0, i) + str.substring(i + 1, n));
            }
        }
    }
}
```
```shell
permute("", "ABC")
├── permute("A", "BC")
│   ├── permute("AB", "C")
│   │   └── permute("ABC", "")
│   └── permute("AC", "B")
│       └── permute("ACB", "")
├── permute("B", "AC")
│   ├── permute("BA", "C")
│   │   └── permute("BAC", "")
│   └── permute("BC", "A")
│       └── permute("BCA", "")
└── permute("C", "AB")
    ├── permute("CA", "B")
    │   └── permute("CAB", "")
    └── permute("CB", "A")
        └── permute("CBA", "")
```
